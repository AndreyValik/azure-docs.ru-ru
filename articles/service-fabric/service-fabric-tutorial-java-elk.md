---
title: Мониторинг приложений в Service Fabric с использованием ELK в Azure | Документы Майкрософт
description: Из этого руководства вы узнаете, как настроить ELK и отслеживать работу приложений Service Fabric.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/26/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 938d8efeaa88cc5bebbf33e525132a030f1b3c7c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112509"
---
# <a name="tutorial-monitor-your-service-fabric-applications-using-elk"></a>Руководство. Мониторинг приложений Service Fabric с помощью ELK

Это руководство представляет собой четвертую часть цикла. В нем показано, как использовать стек ELK (Elasticsearch, Logstash и Kibana) для мониторинга приложений Service Fabric, работающих в Azure.

В четвертой части цикла вы узнаете, как выполнять такие задачи:
> [!div class="checklist"]
> * настройка сервера ELK в Azure;
> * настройка Logstash для получения журналов из концентраторов событий;
> * визуализация журналов платформы и приложений в Kibana.

Из этого цикла руководств вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * [Развертывание приложения Java служб Service Fabric Reliable Services в Azure.](service-fabric-tutorial-create-java-app.md)
> * [Развертывание и отладка приложения в локальном кластере.](service-fabric-tutorial-debug-log-local-cluster.md)
> * [Развертывание приложения в кластере Azure.](service-fabric-tutorial-java-deploy-azure.md)
> * Настройка мониторинга и диагностики приложения
> * [Настройка процесса непрерывной интеграции и доставки](service-fabric-tutorial-java-jenkins.md)

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы с этим руководством выполните следующие действия:

* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Настройте приложение на передачу журналов в расположение, указанное во [второй части цикла](service-fabric-tutorial-debug-log-local-cluster.md).
* Выполните инструкции в [третьей части цикла](service-fabric-tutorial-java-deploy-azure.md), чтобы настроить в запущенном кластере Service Fabric отправку журналов в концентраторы событий.
* В концентраторах событий требуется использовать политику с разрешениями на ожидание передачи данных и связанным первичным ключом из третьей части цикла.

## <a name="download-the-voting-sample-application"></a>Скачивание примера приложения для голосования

Если вы не создавали пример приложения для голосования [в первой части этой серии руководств](service-fabric-tutorial-create-java-app.md), вы можете скачать его. В окне терминала выполните следующую команду, чтобы клонировать репозиторий с примером приложения на локальный компьютер.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart
```

## <a name="create-an-elk-server-in-azure"></a>Создание сервера ELK в Azure

Для этого руководства можно использовать предварительно настроенную среду ELK. При наличии такой среды перейдите к разделу о **настройке Logstash**. Если у вас нет такой среды, выполните следующие действия, чтобы создать ее в Azure.

1. Создайте ELK с сертификацией от [Bitnami](https://ms.portal.azure.com/#create/bitnami.elk4-6) в Azure. Для этого руководства нет конкретных требований к созданию этого сервера.

2. Перейдите к ресурсу на портале Azure и выберите вкладку **Диагностика загрузки** в разделе **Поддержка и устранение неполадок**. Затем щелкните вкладку **Журнал последовательного вывода**.

    ![Диагностика загрузки](./media/service-fabric-tutorial-java-elk/bootdiagnostics.png)
3. В журналах выполните поиск пароля, который требуется для получения доступа к экземпляру Kibana. Результат будет выглядеть, как следующий фрагмент кода:

    ```bash
    [   25.932766] bitnami[1496]: #########################################################################
    [   25.948656] bitnami[1496]: #                                                                       #
    [   25.962448] bitnami[1496]: #        Setting Bitnami application password to '[PASSWORD]'           #
    [   25.978137] bitnami[1496]: #        (the default application username is 'user')                   #
    [   26.004770] bitnami[1496]: #                                                                       #
    [   26.029413] bitnami[1496]: #########################################################################
    ```

4. На странице обзора сервера на портале Azure нажмите кнопку "Подключиться", чтобы получить данные для входа.

    ![Подключение виртуальной машины](./media/service-fabric-tutorial-java-elk/vmconnection.png)

5. Подключитесь к серверу по протоколу SSH, на котором хранится образ ELK, с помощью следующей команды.

    ```bash
    ssh [USERNAME]@[CONNECTION-IP-OF-SERVER]

    Example: ssh testaccount@104.40.63.157
    ```

## <a name="set-up-elk"></a>Настройка ELK

1. Сначала загрузите среду ELK.

    ```bash
    sudo /opt/bitnami/use_elk
    ```

2. Если используется существующая среда, приостановите работу службы Logstash, выполнив следующую команду.

    ```bash
    sudo /opt/bitnami/ctlscript.sh stop logstash
    ```

3. Чтобы установить подключаемый модуль Logstash для концентраторов событий, выполните следующую команду.

    ```bash
    logstash-plugin install logstash-input-azureeventhub
    ```

4. Создайте файл конфигурации Logstash с приведенным ниже содержимым или измените содержимое существующего файла. Если вы создаете файл, сохраните его в каталоге ```/opt/bitnami/logstash/conf/access-log.conf``` при использовании образа ELK от Bitnami в Azure.

    ```json
    input
    {
        azureeventhub
        {
            key => "[RECEIVER-POLICY-KEY-FOR-EVENT-HUB]"
            username => "[RECEIVER-POLICY-NAME]"
            namespace => "[EVENTHUB-NAMESPACENAME]"
            eventhub => "[EVENTHUB-NAME]"
            partitions => 4
        }
    }

    output {
         elasticsearch {
             hosts => [ "127.0.0.1:9200" ]
         }
     }
    ```

5. Чтобы проверить конфигурацию, выполните следующую команду:

    ```bash
    /opt/bitnami/logstash/bin/logstash -f /opt/bitnami/logstash/conf/ --config.test_and_exit
    ```

6. Запустите службу Logstash.

    ```bash
    sudo /opt/bitnami/ctlscript.sh start logstash
    ```

7. Проверьте работу Elasticsearch, чтобы убедиться в том, что вы получаете данные.

    ```bash
    curl 'localhost:9200/_cat/indices?v'
    ```

8. Перейдите на панель мониторинга Kibana по адресу **http://SERVER-IP** и введите имя пользователя и пароль для Kibana. Если вы использовали образ ELK в Azure, имя пользователя по умолчанию — user, а пароль такой же, как полученный на вкладке **Диагностика загрузки**.

    ![Kibana](./media/service-fabric-tutorial-java-elk/kibana.png)

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * запуск сервера ELK в Azure;
> * настройка сервера на получение диагностических данных из кластера Service Fabric.

Перейдите к следующему руководству:
> [!div class="nextstepaction"]
> [Настройка непрерывной интеграции и непрерывной поставки с помощью Jenkins](service-fabric-tutorial-java-jenkins.md)
