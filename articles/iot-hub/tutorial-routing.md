---
title: Настройка маршрутизации сообщений с помощью Центра Интернета вещей Azure | Документация Майкрософт
description: Настройка маршрутизации сообщений с помощью Центра Интернета вещей Azure
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 6e421aa630dc121589dece789e2e0d7f9a56bbe6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434845"
---
# <a name="tutorial-configure-message-routing-with-iot-hub"></a>Руководство. Настройка маршрутизации сообщений с Центром Интернета вещей

Маршрутизация сообщений позволяет отправлять данные телеметрии с устройств Центра Интернета вещей на встроенные конечные точки, совместимые с центрами событий, или на настраиваемые конечные точки, такие как хранилище больших двоичных объектов, очередь служебной шины, раздел служебной шины и концентраторы событий. При настройке маршрутизации сообщений вы можете создать правила маршрутизации для собственного маршрута, соответствующего определенному правилу. После выполнения настройки входящие данные автоматически направляются на конечные точки Центра Интернета вещей. 

Из этого руководства вы узнаете, как настроить и использовать правила маршрутизации с Центром Интернета вещей. Сообщения будут направляться от устройств IoT к одной из нескольких служб, включая хранилище больших двоичных объектов и очередь служебной шины. Сообщения в очередь служебной шины будут подхвачены приложением логики и отправлены по электронной почте. Сообщения, для которых нет специально настроенной маршрутизации, отправляются на конечную точку по умолчанию и отображаются в визуализации Power BI.

Вот какие шаги выполняются в этом руководстве:

> [!div class="checklist"]
> * Настройка базовых ресурсов — Центра Интернета вещей, учетной записи хранения, очереди служебной шины и имитированного устройства, используя Azure CLI или PowerShell.
> * Настройка конечных точек и маршрутов в центр IoT для учетной записи хранения и очереди служебной шины.
> * Создание приложения логики, которое запущено и отправляет сообщение по электронной почте, когда сообщение добавляется в очередь служебной шины.
> * Загрузка и запуск приложения, которое имитирует устройство IoT отправки сообщений в концентратор для различных параметров маршрутизации.
> * Создайте визуализацию Power BI для данных, отправляемых в конечную точку по умолчанию.
> * Просмотр результатов ...
> * ... в очереди служебной шины и в сообщениях электронной почты.
> * ... в учетной записи хранения.
> * ... в визуализации Power BI.

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

- Установите [Visual Studio для Windows](https://www.visualstudio.com/). 

- Учетная запись Power BI для анализа потока аналитики конечных точек по умолчанию. (Доступна [бесплатная пробная версия Power BI](https://app.powerbi.com/signupredirect?pbi_source=web).)

- Учетная запись Office 365 для отправки уведомлений по электронной почте. 

В этом руководстве для выполнения действий по настройке потребуется Azure CLI или Azure PowerShell. 

Чтобы использовать интерфейс Azure CLI, его можно установить локально, но мы рекомендуем использовать Azure Cloud Shell. Azure Cloud Shell — это бесплатная интерактивная оболочка, которую можно использовать для запуска сценариев CLI Azure. Общие инструменты Azure предварительно установлены и настроены в Cloud Shell для использования с вашей учетной записью, поэтому нет необходимости устанавливать их локально. 

Для использования PowerShell установите его локально с помощью приведенных ниже инструкций. 

### <a name="azure-cloud-shell"></a>Azure Cloud Shell

Cloud Shell можно открыть разными способами:

|  |   |
|-----------------------------------------------|---|
| Нажмите кнопку **Попробовать** в правом верхнем углу блока с кодом. | ![Cloud Shell в этой статье](./media/tutorial-routing/cli-try-it.png) |
| Откройте Cloud Shell в браузере. | [![https://shell.azure.com/bash](./media/tutorial-routing/launchcloudshell.png)](https://shell.azure.com) |
| Нажмите кнопку меню **Cloud Shell** в правом верхнем углу окна [портала Azure](https://portal.azure.com). |    ![Cloud Shell на портале](./media/tutorial-routing/cloud-shell-menu.png) |
|  |  |

### <a name="using-azure-cli-locally"></a>Использование локального интерфейса Azure

Если вы предпочитаете использовать локальный интерфейс CLI, а не Cloud Shell, необходимо иметь модуль Azure CLI версии 2.0.30.0 или новее. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli). 

### <a name="using-powershell-locally"></a>Использование PowerShell в локальной среде

Для работы с этим руководством требуется модуль Azure PowerShell версии не ниже 5.7. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable AzureRM`. Если вам необходимо выполнить установку или обновление, см. статью [об установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="set-up-resources"></a>Настройка ресурсов

Для этого руководства требуется Центр IoT, учетная запись хранения и очередь служебной шины. Все эти ресурсы создаются с помощью Azure CLI или Azure PowerShell. Используйте те же группу ресурсов и расположение для всех ресурсов. Затем в конце, удалив группу ресурсов, можно удалить все данные за один шаг.

В следующих разделах описывается, как выполнить эти необходимые шаги. Следуйте инструкциям CLI *или* PowerShell.

1. Создайте [группу ресурсов](../azure-resource-manager/resource-group-overview.md). 

    <!-- When they add the Basic tier, change this to use Basic instead of Standard. -->

1. Создаете Центр Интернета вещей на уровне S1. Добавьте группы потребителей в Центр Интернета вещей. Azure Stream Analytics использует группу потребителей при получении данных.

1. Создайте стандартную учетная запись хранения V1 с репликацией Standard_LRS.

1. Создайте пространство имен и очередь служебной шины. 

1. Создайте идентификатор устройства для имитированного устройства, которое отправляет сообщения концентратору. Сохраните ключ для этапа тестирования.

### <a name="azure-cli-instructions"></a>Инструкции по работе в Azure CLI

Самый простой способ использовать этот сценарий - скопировать его и вставить в Cloud Shell. Предполагая, что вы уже вошли в систему, Cloud Shell будет запускать сценарий по одной строке за раз. 

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device 

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$RANDOM
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, so add a random number to the end.
storageAccountName=contosostorage$RANDOM
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key. 
#    You need this to create the container.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"') 

# See the value of the storage account key.
echo "$storageAccountKey"

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off 

# The Service Bus namespace must be globally unique, so add a random number to the end.
sbNameSpace=ContosoSBNamespace$RANDOM
echo "Service Bus namespace = " $sbNameSpace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNameSpace \
    --location $location
    
# The Service Bus queue name must be globally unique, so add a random number to the end.
sbQueueName=ContosoSBQueue$RANDOM
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNameSpace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

```

### <a name="powershell-instructions"></a>Инструкции по работе в среде PowerShell

Самый простой способ использовать этот сценарий — открыть [PowerShell ISE](/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise.md), скопировать сценарий в буфер обмена и вставить весь скрипт в окно сценария. Затем можно изменить значения имен ресурсов (если хотите) и запустить весь сценарий. 

```azurepowershell-interactive
# Log into Azure account.
Login-AzureRMAccount

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"
$iotDeviceName = "Contoso-Test-Device"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, so add a random number to the end.
$iotHubName = "ContosoTestHub$(Get-Random)"
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzureRmIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzureRmIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random number to the end.
$storageAccountName = "contosostorage$(Get-Random)"
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
$storageContext = $storageAccount.Context 

# Create the container in the storage account.
New-AzureStorageContainer -Name $containerName `
    -Context $storageContext

# The Service Bus namespace must be globally unique,
#   so add a random number to the end.
$serviceBusNamespace = "ContosoSBNamespace$(Get-Random)"
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzureRmServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random number to the end.
$serviceBusQueueName  = "ContosoSBQueue$(Get-Random)"
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzureRmServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName 

```

Затем создайте идентификатор устройства и сохраните его ключ для последующего использования. Это удостоверение устройства используется приложением моделирования для отправки сообщений в Центр IoT. Эта возможность недоступна в PowerShell, но устройство можно создать в [портале Azure](https://portal.azure.com).

1. Откройте [портал Azure](https://portal.azure.com) и войти в учетную запись Azure.

1. Нажмите **Группы ресурсов** и выберите группу ресурсов. В этом руководстве используется **ContosoResources**.

1. В списке ресурсов выберите Центр IoT. В этом руководстве используется **ContosoTestHub**. Из области концентратора выберите **Устройства IoT**.

1. Щелкните **+ Добавить**. В панели «Добавить устройство» введите идентификатор устройства. В этом руководстве используется **Contoso-Test-Device**. Оставьте пустым ключи и поставьте флажок **Автоматически создавать ключи**. Включите кнопку **Подключить устройство к центру IoT**. Выберите команду **Сохранить**.

   ![Снимок экрана добавленных устройств.](./media/tutorial-routing/add-device.png)

1. Теперь, когда оно было создано, щелкните на устройстве, чтобы увидеть созданный ключи. Нажмите значок копирования для первичного ключа и сохраните его где-нибудь, например в блокноте, для этапа тестирования в этом руководстве.

   ![Снимок экрана сведений об устройстве, включая ключи.](./media/tutorial-routing/device-details.png)

## <a name="set-up-message-routing"></a>Настройка маршрутизации сообщений

Вы собираетесь направлять сообщения на разные ресурсы на основе свойств, прикрепленных к сообщению имитируемым устройством. Сообщения, которые не являются настраиваемыми, отправляются к конечной точке по умолчанию (сообщения и события). 

|value |Результат|
|------|------|
|level="storage" |Запись в хранилище Azure.|
|level="critical" |Запись в очередь служебной шины. Приложение логики извлекает сообщение из очереди и использует Office 365 для отправки сообщения по электронной почте.|
|по умолчанию |Отобразите эти данные с помощью Power BI.|

### <a name="routing-to-a-storage-account"></a>Маршрутизация к учетной записи хранения 

Теперь настройте маршрутизацию для учетной записи хранения. Определите конечную точку, а затем настройте маршрут для этой конечной точки. Сообщения, в которых свойство **level** установлено как **storage**, автоматически записываются в учетная запись хранения.

1. В [портале Azure](https://portal.azure.com) нажмите кнопку **Группы ресурсов**, затем выберите группу ресурсов. В этом руководстве используется **ContosoResources**. Выберите Центр IoT в списке ресурсов. В этом руководстве используется **ContosoTestHub**. Нажмите кнопку **Конечные точки**. В панели **Конечные точки** нажмите кнопку **+Добавить**. Введите следующие сведения:

   **Имя**. Введите имя для конечной точки. В этом руководстве используется **StorageContainer**.
   
   **Тип конечной точки**. Из раскрывающегося списка выберите **Контейнер хранилища Azure**.

   Для просмотра списка учетных записей хранения нажмите кнопку **Выберите контейнер**. Затем выберите учетную запись хранения. В этом руководстве используется **contosostorage**. Затем выберите контейнер. В этом руководстве используется **contosoresults**. Щелкните **Выбрать**, чтобы вернуться на панель **Добавление конечной точки**. 
   
   ![Снимок экрана добавления конечной точки.](./media/tutorial-routing/add-endpoint-storage-account.png)
   
   Нажмите кнопку **OK** для завершения добавления конечной точки.
   
1. Щелкните **Маршруты** в Центре Интернета вещей. Вы собираетесь создать правило маршрутизации, которое направляет сообщения в контейнер хранения, который был только-что добавлен в качестве конечной точки. Нажмите кнопку **+Добавить** в верхней части панели маршрутов. Заполните поля на экране. 

   **Имя**. Введите имя для правила маршрутизации. В этом руководстве используется **StorageRule**.

   **Источник данных**. Из раскрывающегося списка выберите **Сообщения устройства**.

   **Конечная точка**. Выберите только-что настроенную конечную точку. В этом руководстве используется **StorageContainer**. 
   
   **Строка запроса**. Введите `level="storage"` как строку запроса. 

   ![Снимок экрана создания правила маршрутизации для учетной записи хранения.](./media/tutorial-routing/create-a-new-routing-rule-storage.png)
   
   Выберите команду **Сохранить**. После завершения в панели маршрутов можно просмотреть новое правило маршрутизации для хранения. Закрыв панель маршрутов, возвратитесь на страницу группы ресурсов.

### <a name="routing-to-a-service-bus-queue"></a>Маршрутизация к очереди служебной шины 

Теперь настройте маршрутизацию для очереди служебной шины. Определите конечную точку, а затем настройте маршрут для этой конечной точки. Сообщения, в которых свойство **level** установлено как **critical**, записываются в очередь служебной шины, которая запускает приложение логики, которое затем отправляет сообщение электронной почты с информацией. 

1. На странице группы ресурсов выберите Центр IoT, затем нажмите **Конечные точки**. В панели **Конечные точки** нажмите кнопку **+Добавить**. Введите следующие сведения.

   **Имя**. Введите имя для конечной точки. В этом руководстве используется **CriticalQueue**. 

   **Тип конечной точки**. Из раскрывающегося списка выберите **Очередь служебной шины**.

   **Пространство имен служебной шины**. Из раскрывающегося списка для этого руководства выберите пространство имен служебной шины. В этом руководстве используется **ContosoSBNamespace**.

   **Очередь Service Bus**. Из раскрывающегося списка выберите очереди служебной шины. В этом руководстве используется **contososbqueue**.

   ![Снимок экрана добавления конечной точки для очереди Service Bus.](./media/tutorial-routing/add-endpoint-sb-queue.png)

   Чтобы сохранить конечную точку, нажмите кнопку **OK**. После завершения закройте панель конечных точек. 
    
1. Щелкните **Маршруты** в Центре Интернета вещей. Вы собираетесь создать правило маршрутизации, которое направляет сообщения в очередь служебной шины, которая была только-что добавлена в качестве конечной точки. Нажмите кнопку **+Добавить** в верхней части панели маршрутов. Заполните поля на экране. 

   **Имя**. Введите имя для правила маршрутизации. В этом руководстве используется **SBQueueRule**. 

   **Источник данных**. Из раскрывающегося списка выберите **Сообщения устройства**.

   **Конечная точка**. Выберите только-что созданную конечную точку **CriticalQueue**.

   **Строка запроса**. Введите `level="critical"` как строку запроса. 

   ![Снимок экрана, показывающий создание правила маршрутизации для очереди служебной шины.](./media/tutorial-routing/create-a-new-routing-rule-sbqueue.png)
   
   Выберите команду **Сохранить**. Как показано здесь, при возвращении в область маршрутов видно оба новых правила маршрутизации.

   ![Снимок экрана, показывающий только-что созданные маршруты.](./media/tutorial-routing/show-routing-rules-for-hub.png)

   Закрыв панель маршрутов, возвратитесь на страницу группы ресурсов.

## <a name="create-a-logic-app"></a>Создание приложения логики  

Очередь служебной шины должна использоваться для приема сообщений, обозначенных как критические. Настройте приложение логики для мониторинга очереди служебной шины и отправки электронного письма, когда сообщение добавлено в очередь. 

1. В [портале Azure](https://portal.azure.com) нажмите **+Создать ресурс**. Поместите **Приложение логики** в поле поиска и нажмите клавишу ВВОД. Из отображаемых результатов поиска выберите приложение логики, затем нажмите **Создать**, чтобы перейти к панели **Создание приложения логики**. Заполните поля. 

   **Имя**. Это поле является именем приложения логики. В этом руководстве используется **ContosoLogicApp**. 

   **Подписка**. Выберите подписку Azure.

   **Группа ресурсов**. Щелкните **Использовать существующие** и выберите группу ресурсов. В этом руководстве используется **ContosoResources**. 

   **Расположение**. Используйте свое расположение. В этом руководстве используется **Западная часть США**. 

   **Log Analytics**. Этот переключатель должен быть отключен. 

   ![Снимок экрана создания приложения логики.](./media/tutorial-routing/create-logic-app.png)

   Нажмите кнопку **Создать**.

1. Теперь перейдите в приложение логики. Самый простой способ получить приложение логики — щелкнуть **Группы ресурсов**, выбрать группу ресурсов (в этом руководстве используется **ContosoResources**), затем выберите из списка ресурсов приложения логики. Появится страница конструктора Logic Apps (может потребоваться прокрутить вправо для просмотра всей страницы). На странице конструктора Logic Apps прокрутите вниз, пока не увидите плитку с надписью **Пустое приложение логики +** и щелкните его. 

1. Отобразится список соединителей. Выберите **служебную шину**. 

   ![Снимок экрана, показывающий список соединителей.](./media/tutorial-routing/logic-app-connectors.png)

1. Отобразится список триггеров. Выберите **Служебная шина — При получении сообщения в очереди (автозавершение)**. 

   ![Снимок экрана, показывающий список триггеров для служебной шины.](./media/tutorial-routing/logic-app-triggers.png)

1. На следующем экране введите имя подключения. В этом руководстве используется **ContosoConnection**. 

   ![Снимок экрана настройки подключения для очереди служебной шины.](./media/tutorial-routing/logic-app-define-connection.png)

   Выберите пространство имен служебной шины. В этом руководстве используется **ContosoSBNamespace**. При выборе пространства имен портал запрашивает пространство имен служебной шины для извлечения ключей. Выберите **RootManageSharedAccessKey** и нажмите кнопку **Создать**. 
   
   ![Снимок экрана, показывающий завершение настройки подключения.](./media/tutorial-routing/logic-app-finish-connection.png)

1. На следующем экране из раскрывающегося списка выберите имя очереди (в этом руководстве используется **contososbqueue**). Можно использовать значения по умолчанию для остальных полей. 

   ![Снимок экрана параметров очереди.](./media/tutorial-routing/logic-app-queue-options.png)

1. Теперь настройте действие для отправки сообщений электронной почты, когда очередь получила сообщение. Чтобы добавить шаг, в конструкторе Logic Apps нажмите **+ Новый шаг**, затем нажмите кнопку **Добавить действие**. В панели **выбора действия** найдите и щелкните **Office 365 Outlook**. На экране триггеров выберите **Outlook Office 365 - Отправить электронное письмо**.  

   ![Снимок экрана, показывающий параметры Office 365.](./media/tutorial-routing/logic-app-select-outlook.png)

1. Затем войдите в свою учетную запись Office 365, чтобы установить соединение. Укажите адреса электронной почты получателей сообщений электронной почты. Также укажите тему и введите текст сообщения, который увидит получатель. Для тестирования заполните свой собственный адрес электронной почты в качестве получателя.

   Нажмите кнопку **Добавить динамическое содержимое** для отображения содержимого сообщения, которое можно включить. Выберите **Содержимое** — оно будет содержать сообщение по электронной почте. 

   ![Снимок экрана, показывающий параметры электронной почты для приложения логики.](./media/tutorial-routing/logic-app-send-email.png)

1. Выберите команду **Сохранить**. Затем закройте конструктор Logic App.

## <a name="set-up-azure-stream-analytics"></a>Настройка Azure Stream Analytics

Чтобы просмотреть данные в визуализации Power BI, прежде всего настройте задание Stream Analytics для извлечения данных. Помните, что в конечную точку по умолчанию отправляются только те сообщения, у которых параметр **level** имеет значение **normal**, и именно они будут извлекаться заданием Stream Analytics для визуализации Power BI.

### <a name="create-the-stream-analytics-job"></a>Создание задания Stream Analytics

1. На [портале Azure](https://portal.azure.com) последовательно выберите **Создать ресурс** > **"Интернет вещей"** > **Stream Analytics job** (Задание Stream Analytics).

1. Введите представленные ниже сведения для задания.

   **Имя задания**. Имя задания. Оно должно быть глобально уникальным. В этом руководстве используется **contosoJob**.

   **Группа ресурсов**. Выберите ту же группу ресурсов, которую использует Центр IoT. В этом руководстве используется **ContosoResources**. 

   **Расположение**. Используйте то же расположение, которое используется в сценарии установки. В этом руководстве используется **Западная часть США**. 

   ![Снимок экрана, показывающий, как создать задание Stream Analytics.](./media/tutorial-routing/stream-analytics-create-job.png)

1. Щелкните **Создать**, чтобы создать задание. Чтобы вернуться к заданию, щелкните **Группа ресурсов**. В этом руководстве используется **ContosoResources**. Выберите группу ресурсов, а затем выберите задание Stream Analytics в списке ресурсов. 

### <a name="add-an-input-to-the-stream-analytics-job"></a>Добавление входных данных в задание Stream Analytics

1. В разделе **Топология задания** щелкните **Входные данные**.

1. В панели **входов**, нажмите кнопку **Добавить потоковый вход** и выберите Центр IoT. На появившемся экране заполните следующие поля:

   **Входной псевдоним**. В этом руководстве используется **contosoinputs**.

   **Подписка**: выберите свою подписку.

   **Центр IoT**. Выберите центр IoT. В этом руководстве используется **ContosoTestHub**.

   **Конечная точка**. Выберите **Обмен сообщениями**. (При выборе "мониторинг операций", вы получите данные телеметрии Центра IoT, а не данные, которые вы отправляете через него.) 

   **Имя политики общего доступа**. Выберите **iothubowner**. Портал заполняет ключ политики совместного доступа для вас.

   **Группа потребителей**. Выберите ранее созданную группу потребителей. В этом руководстве используется **contosoconsumers**.
   
   Для остальных полей примите параметры по умолчанию. 

   ![Снимок экрана, показывающий, как настроить входы для задания Stream Analytics.](./media/tutorial-routing/stream-analytics-job-inputs.png)

1. Выберите команду **Сохранить**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Добавление выходных данных в задание Stream Analytics

1. В разделе **Топология задания** щелкните **Выходные данные**.

1. На панели **выходных данных** щелкните **Добавить** и выберите **Power BI**. На появившемся экране заполните следующие поля:

   **Выходной псевдоним**. Уникальный псевдоним для выходных данных. В этом руководстве используется **contosooutputs**. 

   **Имя набора данных**. Имя набора данных для использования в Power BI. В этом руководстве используется **contosodataset**. 

   **Имя таблицы**. Имя таблицы для использования в Power BI. В этом руководстве используется **contosotable**.

   Для остальных полей оставьте значения по умолчанию.

1. Щелкните **Авторизация** и войдите в учетную запись Power BI.

   ![Снимок экрана, показывающий, как настроить выходы для задания Stream Analytics.](./media/tutorial-routing/stream-analytics-job-outputs.png)

1. Выберите команду **Сохранить**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Настройка запроса задания Stream Analytics

1. В разделе **Топология задания** щелкните **Запрос**.

1. Замените значение `[YourInputAlias]` значением псевдонима входных данных задания. В этом руководстве используется **contosoinputs**.

1. Замените значение `[YourOutputAlias]` значением псевдонима выходных данных задания. В этом руководстве используется **contosooutputs**.

   ![Снимок экрана, показывающий, как настроить запрос для задания Stream Analytics.](./media/tutorial-routing/stream-analytics-job-query.png)

1. Выберите команду **Сохранить**.

1. Закройте панель запроса. Это действие вернет вас в представление ресурсов в группе ресурсов. Щелкните задание Stream Analytics. В этом руководстве оно имеет имя **contosoJob**.

### <a name="run-the-stream-analytics-job"></a>Выполнение задания Stream Analytics

В задании Stream Analytics щелкните **Запуск** > **Сейчас** > **Запустить**. После успешного запуска состояние задания **Остановлено** изменится на **Выполняется**.

Чтобы настроить отчет Power BI, нужны данные. Поэтому настройка Power BI будет происходить после создания устройства и запуска приложения имитации устройства.

## <a name="run-simulated-device-app"></a>Запуск приложения имитации устройства

Ранее в разделе настройки скрипта было настроено устройство для имитации с использованием устройства IoT. В этом разделе происходит загрузка консольного приложения .NET, которое имитирует устройство, отправляющее сообщения с устройства в облако в Центре Интернета вещей. Это приложение отправляет сообщения для каждого из различных методов маршрутизации. 

Загрузите решение для [имитации устройств IoT](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip). Загрузится репозиторий с несколькими приложениями. Нужное решение находится в папке iot-hub/Tutorials/Routing/SimulatedDevice/.

Дважды щелкните файл решения (SimulatedDevice.sln), чтобы открыть код в Visual Studio, а затем откройте файл Program.cs. Замените `{iot hub hostname}` на имя узла Центра IoT. Формат имени узла Центра IoT **{iot-hub-name}.azure-devices.net**. В этом руководстве имя узла концентратора **ContosoTestHub.azure-devices.net**. Затем замените `{device key}` ранее сохраненным ключом устройства при настройке имитированного устройства. 

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Запуск и тестирование 

Запустите консольное приложение. Подождите несколько минут. Вы увидите сообщения, отправленные на экран консоли приложения.

Приложение отправляет новое сообщение с устройства в облако к Центру IoT каждую секунду. Сообщение содержит JSON-сериализованный объект с идентификатором устройства, температурой, влажностью и уровнем сообщений, значение которого по умолчанию `normal`. Случайным образом назначается уровень `critical` или `storage`, вызывая сообщение, которое направляется в учетную запись хранения или в очередь служебной шины (которая запускает приложение логики для отправки сообщения электронной почты). Показания по умолчанию (`normal`) будут отображаться в отчете бизнес-аналитики, которая настроится позже.

Если все настроено правильно, на этом этапе можно увидеть следующие результаты:

1. Вы начинаете получать сообщения о критических сообщениях. 

   ![Снимок экрана, показывающий итоговое сообщение электронной почты.](./media/tutorial-routing/results-in-email.png)

   Это означает следующее:

   * Маршрутизация в очередь служебной шины работает правильно.
   * Приложение логики, получающее сообщение из очереди служебной шины, работает правильно.
   * Соединитель приложения логики для Outlook работает правильно. 

1. В [портале Azure](https://portal.azure.com) нажмите кнопку **Группы ресурсов**, затем выберите группу ресурсов. В этом руководстве используется **ContosoResources**. Выберите учетную запись хранения, нажмите кнопку **Большие двоичные объекты**, затем выберите контейнер. В этом руководстве используется **contosoresults**. Должна появиться папка, и можно выполнить детализацию по каталогах пока не найдется один или несколько файлов. Откройте один из этих файлов. Они содержат записи, направляемые в учетную запись хранилища. 

   ![Снимок экрана, показывающий результирующие файлы в хранилище.](./media/tutorial-routing/results-in-storage.png)

Это означает следующее:

   * Маршрутизация к учетной записи хранения работает правильно.

Теперь, не останавливая приложение, настройте визуализацию Power BI, чтобы увидеть сообщения, поступающие через маршруты по умолчанию. 

## <a name="set-up-the-power-bi-visualizations"></a>Настройка визуализаций Power BI

1. Выполните вход в учетную запись [Power BI](https://powerbi.microsoft.com/).

1. Перейдите в **Рабочие области** и выберите рабочую область, которая устанавливается при создании вывода для задания Stream Analytics. В этом руководстве используется **Моя рабочая область**. 

1. Нажмите кнопку **Наборы данных**.

   Вы должны увидеть набор данных, указанный при создании выходных данных для задания Stream Analytics. В этом руководстве используется **contosodataset**. (Это может занять 5–10 минут для отображения набора данных в первый раз.)

1. В разделе **Действия** щелкните первый значок для создания отчета.

   ![Снимок экрана с рабочей областью Power BI, на которой выделены меню "Действия" и значок отчета.](./media/tutorial-routing/power-bi-actions.png)

1. Создайте график для отображения данных температуры в реальном времени за определенный период времени.

   a. На странице создания отчетов добавьте график, щелкнув значок графика.

   ![Снимок экрана, показывающий визуализации и поля.](./media/tutorial-routing/power-bi-visualizations-and-fields.png)

   b. В области **Поля** разверните таблицу, указанную при создании выходных данных для задания Stream Analytics. В этом руководстве используется **contosotable**.

   c. Перетащите элемент **EventEnqueuedUtcTime** на **ось** в области **Визуализации**.

   d. Перетащите элемент **температура** в раздел **Значения** той же области.

   График создан. Ось Х отображает дату и время в часовом поясе UTC. Ось Y отображает данные температуры, полученные от датчика.

1. Создайте другой график для отображения влажности в реальном времени за определенный период времени. Чтобы настроить второй график, выполните действия, аналогичные действиям выше, и перетащите **EventEnqueuedUtcTime** на ось Х, а **влажность** на ось Y.

   ![Снимок экрана, демонстрирующий итоговый отчет Power BI с двумя графиками.](./media/tutorial-routing/power-bi-report.png)

1. Нажмите кнопку **Сохранить**, чтобы сохранить отчет.

Вы сможете просматривать данные на обоих графиках. Это означает следующее:

   * Маршрутизация в конечной точке по умолчанию работает правильно.
   * Задание Azure Stream Analytics выполняется правильно.
   * Визуализация Power BI настроена правильно.

Чтобы увидеть последние данные, можно обновить графики, нажав кнопку "Обновить" в верхней части окна PowerBI. 

## <a name="clean-up-resources"></a>Очистка ресурсов 

Если требуется удалить все ресурсы, которые были созданы, удалите группу ресурсов. При этом будут также удалены все ресурсы, содержащиеся в группе. В этом случае действие удаляет Центр IoT, пространство имен служебной шины и очереди, приложение логики, учетную запись хранения и саму группу ресурсов. 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Очистка ресурсов в визуализации Power BI

Войдите в учетную запись [Power BI](https://powerbi.microsoft.com/). Перейдите в рабочую область. В этом руководстве используется **Моя рабочая область**. Чтобы удалить визуализацию Power BI, перейдите к наборам данных и щелкните значок корзины для удаления набора данных. В этом руководстве используется **contosodataset**. При удалении набора данных отчет будет также удален.

### <a name="clean-up-resources-using-azure-cli"></a>Очистка ресурсов с помощью Azure CLI

Чтобы удалить группу ресурсов, используйте команду [az group delete](https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az-group-delete).

```azurecli-interactive
az group delete --name $resourceGroup
```
### <a name="clean-up-resources-using-powershell"></a>Очистка ресурсов с помощью PowerShell

Чтобы удалить группу ресурсов, используйте команду [Remove-AzureRmResourceGroup](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup). Значение $resourceGroup было присвоено **ContosoIoTRG1** в начале этого руководства.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $resourceGroup
```


## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как использовать маршрутизацию сообщений для маршрутизации сообщений Центра IoT в разные места назначения, выполняя следующие задачи.  

> [!div class="checklist"]
> * Настройка базовых ресурсов — Центра Интернета вещей, учетной записи хранения, очереди служебной шины и имитированного устройства, используя Azure CLI или PowerShell.
> * Настройка конечных точек и маршрутов в центр IoT для учетной записи хранения и очереди служебной шины.
> * Создание приложения логики, которое запущено и отправляет сообщение по электронной почте, когда сообщение добавляется в очередь служебной шины.
> * Загрузка и запуск приложения, которое имитирует устройство IoT отправки сообщений в концентратор для различных параметров маршрутизации.
> * Создайте визуализацию Power BI для данных, отправляемых в конечную точку по умолчанию.
> * Просмотр результатов ...
> * ... в очереди служебной шины и в сообщениях электронной почты.
> * ... в учетной записи хранения.
> * ... в визуализации Power BI.

Перейдите к следующему руководству, чтобы узнать, как управлять состоянием устройств IoT. 

> [!div class="nextstepaction"]
[Настройка устройств из внутренней службы](tutorial-device-twins.md)

 <!--  [Manage the state of a device](./tutorial-manage-state.md) -->