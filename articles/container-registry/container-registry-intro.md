---
title: Использование частных реестров контейнеров Docker в Azure
description: Общие сведения о службе реестра контейнеров Azure, предоставляющей облачные, управляемые и частные реестры Docker.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview
ms.date: 05/08/2018
ms.author: stevelas
ms.custom: mvc
ms.openlocfilehash: 394297e87ef03541725aad0689f11bca17c05ed9
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576306"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Общие сведения о частных реестрах контейнеров Docker в Azure

Реестр контейнеров Azure — это управляемая служба [реестра Docker](https://docs.docker.com/registry/), основанная на реестре Docker 2.0 с открытым кодом. Создавайте и настраивайте реестры контейнеров Azure, чтобы хранить частные образы [контейнеров Docker](https://www.docker.com/what-docker) и управлять ими.

Используйте реестры контейнеров Azure с имеющимися конвейерами разработки и развертывания контейнеров. Используйте сборку "Реестр контейнеров Azure" (сборка ACR), чтобы создавать образы контейнеров в Azure. Создавайте сборки по требованию или полностью автоматизированные сборки с фиксацией исходного кода и триггерами сборки обновления базового образа.

Общие сведения о Docker и контейнерах см. в [руководстве по Docker](https://docs.docker.com/engine/docker-overview/).

## <a name="use-cases"></a>Варианты использования

Извлекайте образы из реестра контейнеров Azure и отправляйте их в разные места назначения развертывания:

* **Масштабируемые системы управления**, управляющие контейнерными приложениями в кластерах узлов, в том числе [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) и [Kubernetes](http://kubernetes.io/docs/).
* **Службы Azure**, поддерживающие создание и выполнение масштабированных приложений, в том числе [служба Azure Kubernetes (AKS)](../aks/index.yml), [служба приложений](/app-service/index.md), [пакетная служба](../batch/index.yml), [Service Fabric](/azure/service-fabric/) и т. д.

Разработчики также могут отправлять образы в реестр контейнеров в рамках рабочего процесса разработки контейнера. Например, они могут получить доступ к реестру контейнеров из средства развертывания и обеспечения непрерывной интеграции, например [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) или [Jenkins](https://jenkins.io/).

Настройте задания сборки [ACR](#azure-container-registry-build), чтобы автоматически повторно создавать образы приложений при обновлении их базовых образов сборки. Используйте сборку ACR для автоматизации сборок образа, если ваша команда фиксирует код в репозитории Git. *В настоящее время сборка ACR доступна в режиме предварительной версии.*

## <a name="key-concepts"></a>Основные понятия

* **Реестр.** Создайте один или несколько реестров контейнеров в своей подписке Azure. Реестры доступны в трех номерах SKU: ["Базовый", "Стандартный" и "Премиум"](container-registry-skus.md), каждый из которых поддерживает интеграцию веб-перехватчика, аутентификацию в реестре с помощью Azure Active Directory и функцию удаления. Создание реестра в том же расположении Azure, где находятся развертывания, позволяет воспользоваться преимуществами локального хранилища образов контейнеров с доступом по сети. Используйте возможность [георепликации](container-registry-geo-replication.md) реестров уровня "Премиум" для сценариев расширенной репликации и распределения образов контейнеров. Полное имя реестра имеет формат `myregistry.azurecr.io`.

  [Контроль доступа](container-registry-authentication.md) к реестру контейнеров осуществляется с помощью [субъекта-службы](../active-directory/develop/app-objects-and-service-principals.md) на основе Azure Active Directory или предоставленной учетной записи администратора. Чтобы пройти проверку подлинности с помощью реестра, используется стандартная команда `docker login`.

* **Репозиторий.** Каждый реестр содержит по крайней мере один репозиторий, представляющий собой группу образов контейнеров. Реестр контейнеров Azure поддерживает многоуровневые пространства имен для репозитория. Благодаря многоуровневому пространству имен можно группировать коллекции образов, связанные с определенным приложением, или коллекции приложений, связанные с определенным развертыванием или рабочими группами. Например: 

  * `myregistry.azurecr.io/aspnetcore:1.0.1` представляет образ, используемый в организации;
  * `myregistry.azurecr.io/warrantydept/dotnet-build` представляет образ, используемый для создания приложений .NET, доступ к которым предоставляется отделу гарантий;
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` представляет веб-образ, сгруппированный в приложении customer submissions, которое принадлежит отделу гарантий.

* **Образ.** Это моментальный снимок контейнера Docker с доступом только для чтения, который хранится в репозитории. Реестры контейнеров Azure могут содержать образы Windows и Linux. Имена образов нужно указывать для всех развертываний контейнеров. Используйте стандартные [команды Docker](https://docs.docker.com/engine/reference/commandline/), чтобы отправлять образы в репозиторий и извлекать их из него.

* **Контейнер.** Контейнер определяет программное приложение и его зависимости, содержащиеся в полной файловой системе, которая включает в себя код, среду выполнения, служебные программы и библиотеки. Запустите контейнеры Docker на основе образов Windows или Linux, извлеченных из реестра контейнеров. Контейнеры, запущенные на одном компьютере, используют то же самое ядро операционной системы. Контейнеры Docker можно полностью перенести во все основные дистрибутивы Linux, macOS и Windows.

## <a name="azure-container-registry-build-preview"></a>Сборка реестра контейнеров Azure (предварительная версия)

[Сборка реестра контейнеров Azure](container-registry-build-overview.md) (сборка ACR) — это набор компонентов в Реестре контейнеров Azure, предоставляющий упрощенные и эффективные сборки образа контейнера Docker в Azure. Используйте сборку ACR для расширения внутреннего цикла разработки в облако за счет разгрузки операций `docker build` в Azure. Настройте задачи сборки, чтобы автоматизировать конвейер установки исправлений ОС и платформы контейнера и автоматически создавать образы, когда ваша команда фиксирует код в системе управления версиями.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="next-steps"></a>Дополнительная информация

* [Создание реестра контейнеров на портале Azure](container-registry-get-started-portal.md)
* [Создание реестра контейнеров с помощью Azure CLI](container-registry-get-started-azure-cli.md)
* [Automate OS and framework patching with ACR Build](container-registry-build-overview.md) (Автоматизация установки исправлений ОС и платформы с помощью сборки ACR) (предварительная версия)
