---
title: Развертывание приложения ASP.NET на виртуальных машинах Azure с помощью проекта Azure DevOps | Руководство по VSTS
description: Проект DevOps упрощает начало работы с Azure. Проект Azure DevOps упрощает развертывание вашего приложения ASP.NET на виртуальных машинах Azure за несколько простых шагов.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b5a9ce204f68d812da4dfcfebb18b79b9c70c6c9
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967442"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Руководство. Развертывание приложения ASP.NET на виртуальных машинах Azure с помощью проекта Azure DevOps

Проект Azure DevOps представляет собой упрощенный интерфейс, к которому вы подключаете существующий код и репозиторий Git. Или можно выбрать один из примеров приложений, чтобы создать конвейер непрерывной интеграции (CI) и непрерывной поставки (CD) в Azure.  Проект DevOps автоматически создает такие ресурсы Azure, как новая виртуальная машина Azure, создает и настраивает конвейер выпуска в VSTS, содержащий определение сборки для CI, который настраивает определения выпуска для компакт-диска и создает ресурс Azure Application Insights для мониторинга.

Вы сможете выполнять следующие задачи:

> [!div class="checklist"]
> * Создание проекта Azure DevOps для приложения ASP.NET.
> * Настройка VSTS и подписки Azure. 
> * Изучение определения сборки VSTS CI.
> * Изучение определения Release Management VSTS CD.
> * Фиксация изменений в VSTS и автоматическое развертывание в Azure.
> * Настройка мониторинга Azure Application Insights.
> * Очистка ресурсов

## <a name="prerequisites"></a>предварительным требованиям

* Подписка Azure. Вы можете получить бесплатную подписку с помощью [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>Создание проекта Azure DevOps для приложения ASP.NET

Проект Azure DevOps создает конвейер CI/CD в VSTS.  Вы можете создать учетную запись **VSTS** или использовать **имеющуюся**.  Проект Azure DevOps также создает **ресурсы Azure**, такие как виртуальные машины, в **подписке Azure** по вашему выбору.

1. Войдите на портал [Microsoft Azure](https://portal.azure.com).

1. Щелкните значок **+ Создать** на левой панели навигации, затем найдите **проект DevOps**.  Выберите **Создать**.

    ![Начало непрерывной поставки](_img/azure-devops-project-github/fullbrowser.png)

1. Щелкните **.NET**, а затем нажмите кнопку **Далее**.

1. Для параметра **Выберите платформу приложений** выберите значение **ASP.NET**, а затем нажмите кнопку **Далее**. 

1. Исполняющая среда, которую вы выбрали на предыдущих шагах, определяет тип цели развертывания службы Azure, доступной здесь.  Щелкните **Виртуальная машина**, а затем нажмите кнопку **Далее**.

## <a name="configure-vsts-and-an-azure-subscription"></a>Настройка VSTS и подписки Azure

1. **Создайте** учетную запись VSTS или выберите **имеющуюся**.  Выберите **имя** для проекта VSTS.  

1. Выберите свою **подписку Azure**.  Вы можете по желанию щелкнуть ссылку **​​Изменить**, а затем ввести дополнительные сведения о конфигурации, такие как изменение местоположения ресурсов Azure.
 
1. Введите **имя виртуальной машины**, **имя пользователя** и **пароль** для нового ресурса виртуальной машины Azure, а затем нажмите кнопку **Готово**.

1. Подготовка виртуальной машины Azure займет несколько минут.  Пример приложения ASP.NET настраивается в репозитории в вашей учетной записи VSTS, выполняется сборка и выпуск, а ваше приложение развертывается на созданной виртуальной машине Azure.  

    После этого на портале Azure загружается **панель мониторинга** проекта Azure DevOps.  Вы также можете перейти к **панели мониторинга проекта Azure DevOps** непосредственно на вкладке **Все ресурсы** на **портале Azure**.  

    Эта панель мониторинга обеспечивает видимость вашего **репозитория кода**, **VSTS CI/CD** и вашего **приложения в Azure**.    

    ![Представление панели мониторинга](_img/azure-devops-project-vms/dashboardnopreview.png)

1. Проект Azure DevOps автоматически настраивает триггер сборки и выпуска CI, который автоматически развертывает любые изменения кода в вашем репозитории.  Вы можете настроить дополнительные параметры в VSTS.  В правой части панели мониторинга выберите **Обзор**, чтобы просмотреть запущенное приложение.
    
## <a name="examine-the-vsts-ci-build-definition"></a>Изучение определения сборки VSTS CI

Проект Azure DevOps автоматически настраивает полный конвейер VSTS CI/CD в вашей учетной записи VSTS.  Вы можете изучить и настроить конвейер.  Выполните следующие шаги, чтобы ознакомиться с определением сборки VSTS.

1. Щелкните **Конвейеры сборки** в **верхней части** **панели мониторинга проекта Azure DevOps**.  Эта ссылка открывает вкладку браузера и определение сборки VSTS нового проекта.

1. Переместите указатель мыши справа от определения сборки рядом с полем **Состояние**. Выберите отобразившееся **многоточие**.  После этого откроется меню, в котором можно начать выполнение нескольких действий, таких как **постановка новой сборки в очередь**, **приостановка создания** и **изменение определения сборки**.

1. Выберите **Изменить**

1. В этом представлении **рассмотрите различные задачи** для определения сборки.  Сборка выполняет различные задачи, такие как выборка источников из репозитория VSTS Git, восстановление зависимостей и публикация результатов, используемых для развертывания.

1. В верхней части определения сборки выберите **имя определения сборки**.

1. Замените **имя** определения сборки более описательным именем.  Щелкните **Save & queue** (Сохранить и поместить в очередь), а затем выберите **Сохранить**.

1. Под именем определения сборки щелкните **Журнал**.  Вы увидите журнал аудита последних изменений сборки.  VSTS отслеживает любые изменения, внесенные в определение сборки, и позволяет сравнивать версии.

1. Выберите **Триггеры**.  Проект Azure DevOps автоматически создал триггер CI, а каждая фиксация в репозитории запускает новую сборку.  При желании выберите включение или исключение ветвей из процесса CI.

1. Щелкните **Период удержания**.  На основе вашего сценария можно указать политики для хранения или удаления определенного количества сборок.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Изучение определения Release Management VSTS CD

Проект Azure DevOps автоматически создает и настраивает необходимые шаги для развертывания из вашей учетной записи VSTS в вашу подписку Azure.  Эти шаги включают в себя настройку подключения Azure для проверки подлинности VSTS в вашей подписке Azure.  Автоматизация также создает определение выпуска VSTS, что обеспечивает непрерывное развертывание виртуальной машины Azure.  Выполните следующие шаги, чтобы узнать больше об определении выпуска VSTS.

1. Щелкните **Сборка и выпуск**, а затем выберите **Выпуски**.  Проект Azure DevOps создал определение выпуска VSTS для управления развертываниями в Azure.

1. В левой части браузера щелкните **многоточие** рядом с определением выпуска, а затем выберите **Изменить**.

1. Определение выпуска содержит **конвейер**, определяющий процесс выпуска.  В разделе **Артефакты** выберите **Удалить**.  Определение сборки, рассмотренное на предыдущих этапах, создает выходные данные, используемые для артефакта. 

1. Справа от значка **Удалить** выберите **значок** **Триггер непрерывного развертывания** (в виде молнии).  Это определение выпуска имеет активированный триггер непрерывного развертывания.  Триггер запускает развертывание каждый раз, когда имеется новый артефакт сборки.  При желании вы можете отключить триггер, чтобы развертывание требовалось выполнять вручную. 

1. В левой части браузера выберите **Задачи**, а затем выберите **среду**.  

1. Задачами являются действия, выполняемые вашим процессом развертывания, которые сгруппированы по **фазам**.  Для этого определения выпуска есть две **фазы**.  Первая фаза содержит **задачу развертывания группы ресурсов Azure**, которая настраивает виртуальную машину для развертывания и добавляет новую виртуальную машину в **группу развертывания VSTS**.  Группа развертывания виртуальной машины в VSTS управляет логическими группами **целевых компьютеров** развертывания.

1. На этом втором этапе была создана задача **Управление веб-приложением IIS** для создания веб-сайта IIS на виртуальной машине.  Для развертывания сайта была создана вторая задача **Развертывание веб-приложения IIS**.

1. В правой части окна браузера выберите **Просмотреть выпуски**.  В этом представлении можно просмотреть журнал выпусков.

1. Щелкните **многоточие** рядом с одним из выпусков и выберите **Открыть**.  В этом представлении есть несколько меню, которые можно изучить, например меню **сводки**, **связанных рабочих элементов** и **тестов**.

1. Щелкните **Фиксации**.  В этом представлении отображаются фиксации кода, связанные с конкретным развертыванием. Сравните выпуски, чтобы просмотреть различия между развертываниями.

1. Выберите **Журналы**.  Журналы содержат полезную информацию о процессе развертывания.  Их можно просматривать как во время, так и после развертывания.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Фиксация изменений в VSTS и автоматическое развертывание в Azure 

Теперь вы готовы сотрудничать с командой в своем приложении с процессом CI/CD, который автоматически развертывает данные последней работы на вашем веб-сайте.  Каждое изменение в репозитории Git VSTS запускает сборку в VSTS, а определение управления выпуском VSTS выполняет развертывание на вашей виртуальной машине Azure.  Выполните следующие действия или используйте другие методы для фиксации изменений в вашем репозитории.  Изменения кода инициируют процесс CI/CD и автоматически развертывают ваши новые изменения на веб-сайте IIS на виртуальной машине Azure.

1. Щелкните **Код** в меню VSTS и перейдите в свой репозиторий.

1. Перейдите в каталог **Views\Home**, затем щелкните **многоточие** рядом с **Index.cshtml**, а затем щелкните **Изменить**.

1. Внесите изменения в файл, например, измените некоторый текст внутри одного из тегов **div**.  В правом верхнем углу щелкните **Зафиксировать**.  Щелкните **Зафиксировать** еще раз, чтобы внести изменения. 

1. Через несколько минут **начинается сборка в VSTS**, а затем выполняется выпуск для развертывания изменений.  Отслеживайте **состояние сборки** с помощью панели мониторинга проекта DevOps или в браузере с учетной записью VSTS.

1. По завершении выпуска **обновите приложение** в браузере, чтобы убедиться, что вы видите изменения.

## <a name="configure-azure-application-insights-monitoring"></a>Настройка мониторинга Azure Application Insights

С помощью Azure Application Insights можно легко отслеживать производительность и использование своего приложения.  Проект Azure DevOps автоматически настроил ресурс Application Insights для вашего приложения.  Вы можете дополнительно настроить различные предупреждения и возможности мониторинга по мере необходимости.

1. Перейдите на панель мониторинга **проекта Azure DevOps** на **портале Azure**.  В правом нижнем углу панели мониторинга выберите ссылку **​​Application Insights** для своего приложения.

1. На портале Azure откроется колонка **Application Insights**.  Это представление содержит информацию об использовании, производительности и доступности приложения.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Щелкните **Диапазон времени**, а затем выберите **За последний час**.  Щелкните **Обновить**, чтобы отфильтровать результаты.  Отобразится вся активность за последние 60 минут.  Щелкните **x**, чтобы закрыть диапазон времени.

1. Вы можете найти **Оповещения** и несколько других полезных ссылок в верхней части панели.  Выберите **Оповещения**, затем щелкните **+ Добавить оповещение метрики**.

1. Введите **имя** оповещения.

1. По умолчанию оповещение отображается, если **время отклика сервера превышает 1 секунду**.  Выберите раскрывающийся список **Метрика**, чтобы просмотреть различные оповещения метрики.  Например, вы можете настроить **время выполнения запроса ASP.NET** для приложения ASP.NET.  Различные оповещения можно легко настроить, чтобы улучшить возможности мониторинга приложения.

1. Установите флажок **Notify via Email owners, contributors, and readers** (Уведомлять по электронной почте владельцев, участников и читателей).  При желании вы можете выполнять дополнительные действия, когда срабатывает оповещение во время выполнения приложения логики Azure.

1. Нажмите кнопку **ОК**, чтобы создать оповещение.  Через несколько секунд предупреждение появится на панели мониторинга как активное.  **Выйдите из** области оповещений и вернитесь в колонку **Application Insights**.

1. Выберите **Доступность**, затем щелкните **+ Add test** (+ Добавить тест). 

1. Введите **имя теста**, а затем выберите **Создать**.  Будет создан простой тест проверки связи для проверки доступности вашего приложения.  Через несколько минут результаты тестирования будут доступны, а на панели мониторинга Application Insights отобразится состояние доступности.

## <a name="clean-up-resources"></a>Очистка ресурсов

 > [!NOTE]
 > При выполнении шагов ниже ресурсы удаляются навсегда.  Используйте эту функцию только после того, как досконально ознакомитесь с подсказками.

Если вы выполняете тестирование, вы можете очистить ресурсы, чтобы избежать непредвиденных расходов.  Вы можете удалить ненужную виртуальную машину Azure и связанные с ней ресурсы, созданные в этом руководстве, с помощью функции **Удалить** на панели мониторинга Azure DevOps.  **Будьте внимательны**, так как функция удаления уничтожает данные, созданные проектом Azure DevOps как в Azure, так и в VSTS, и вы не сможете получить их после этого.

1. На **портале Azure** перейдите к **проекту Azure DevOps**.
2. В **верхней правой** стороне панели мониторинга нажмите кнопку **Удалить**.  Прочитав подсказку, щелкните **Да**, чтобы**навсегда удалить** ресурсы.

## <a name="next-steps"></a>Дополнительная информация

Вы можете по желанию изменить эти определения сборки и выпуска в соответствии с потребностями вашей команды. Вы также можете использовать этот шаблон CI/CD в качестве шаблона для других своих проектов.  Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> * Создание проекта Azure DevOps для приложения ASP.NET.
> * Настройка VSTS и подписки Azure. 
> * Изучение определения сборки VSTS CI.
> * Изучение определения Release Management VSTS CD.
> * Фиксация изменений в VSTS и автоматическое развертывание в Azure.
> * Настройка мониторинга Azure Application Insights.
> * Очистка ресурсов

Чтобы получить дополнительные сведения о конвейере VSTS, см. это руководство:

> [!div class="nextstepaction"]
> [Define your multi-stage continuous deployment (CD) process](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts) (Определение многоэтапного процесса непрерывного развертывания)
