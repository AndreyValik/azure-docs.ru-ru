---
title: Добавление и удаление образа виртуальной машины в Azure Stack | Документация Майкрософт
description: Добавление корпоративного образа виртуальной машины Windows или Linux для использования клиентами и удаление его.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 06/27/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 5c2088ab39e32c049ce867698e84efba759c9a87
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447342"
---
# <a name="make-a-virtual-machine-image-available-in-azure-stack"></a>Размещение образа виртуальной машины через Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

В Azure Stack вы можете разместить образы виртуальных машин, сделав их доступными для пользователей. На эти образы можно ссылаться с помощью шаблонов Azure Resource Manager или добавлять к пользовательскому интерфейсу Azure Marketplace в качестве элемента Marketplace. Вы можете использовать образ из глобального магазина Azure Marketplace или добавить собственный образ. Виртуальную машину можно добавить с помощью портала или Windows PowerShell.

## <a name="add-a-vm-image-through-the-portal"></a>Добавление образа виртуальной машины через портал

> [!NOTE]
> С помощью этого метода необходимо создать элемент Marketplace отдельно.

Нужно предоставить возможность ссылаться на образы через URI хранилища BLOB-объектов. Подготовьте образ операционной системы Windows или Linux в формате VHD (не VHDX), а затем отправьте образ в учетную запись хранения в Azure или Azure Stack. Если образ уже отправлен в хранилище BLOB-объектов в Azure или Azure Stack, шаг 1 можно пропустить.

1. [Отправьте образ виртуальной машины Windows в Azure для развертываний Resource Manager](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) или следуйте инструкциям в статье [Добавление образов Linux в Azure Stack](azure-stack-linux.md) для образов Linux. Перед отправкой образа необходимо принять во внимание следующие факторы:

   - Azure Stack поддерживает диски фиксированного размера в формате VHD. Фиксированный формат структурирует логический диск в файле линейно, то есть смещение диска X хранится в смещении BLOB-объекта X. Небольшая сноска в конце BLOB-объекта описывает свойства VHD-файла. Чтобы проверить, фиксированного ли размера ваш диск, используйте команду [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) в PowerShell.  

    > [!IMPORTANT]
    >  Azure Stack не поддерживает динамические диски VHD. Изменение размера динамического диска, подключенного к виртуальной машине, приведет к неисправному состоянию виртуальной машины. Чтобы устранить эту проблему, удалите виртуальную машину, не удаляя ее диск — большой двоичный объект VHD в учетной записи хранения. Затем преобразуйте VHD из динамического диска в диск с фиксированным размером и повторно создайте виртуальную машину.

   * Образ эффективнее отправлять в хранилище BLOB-объектов Azure Stack, чем в хранилище BLOB-объектов Azure, так как для размещения образа в репозитории образов Azure Stack требуется меньше времени.

   * При отправке [образов виртуальной машины Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) не забудьте заменить шаг **Вход в Azure** шагом [Настройка среды PowerShell для оператора Azure Stack](azure-stack-powershell-configure-admin.md).  

   * При передаче образа в хранилище BLOB-объектов запишите созданный для него URI. URI в хранилище BLOB-объектов имеет следующий формат: *&lt;учетная_запись_хранения&gt;/&lt;контейнер_BLOB-объектов&gt;/&lt;имя_целевого_виртуального_диска&gt;*.vhd.

   * Чтобы настроить для большого двоичного объекта анонимный доступ, перейдите в контейнер больших двоичных объектов учетной записи хранения, в который был отправлен VHD образа виртуальной машины. Выберите **Большой двоичный объект**, а затем — **Политика доступа**. Если требуется, можно создать подписанный URL-адрес для контейнера и включить его как часть URI большого двоичного объекта.

   ![Переход к большим двоичным объектам учетной записи хранения](./media/azure-stack-add-vm-image/image1.png)

   ![Установка общего доступа к большим двоичным объектам](./media/azure-stack-add-vm-image/image2.png)

2. Войдите в Azure Stack как оператор. В меню выберите **Больше служб**. Выберите **Среда выполнения приложений** > **Образы виртуальных машин** > **Добавить**.

3. В колонке **Add a VM Image** (Добавление образа виртуальной машины) введите издателя, предложение, номер SKU и версию образа виртуальной машины. Эти сегменты имен ссылаются на образ виртуальной машины в шаблонах Resource Manager. Выберите правильное значение для параметра **osType**. В поле **OS Disk Blob URI** (URI большого двоичного объекта диска ОС) введите URI большого двоичного объекта, куда был отправлен образ, и нажмите кнопку **Создать**, чтобы создать образ виртуальной машины.

   ![Начало создания образа](./media/azure-stack-add-vm-image/image4.png)

   При успешном создании образа состояние образа виртуальной машины изменяется на **Успешно**.

4. Чтобы сделать образ виртуальной машины более доступным для потребления пользователем в пользовательском интерфейсе, лучше всего [создать элемент Marketplace](azure-stack-create-and-publish-marketplace-item.md).

## <a name="remove-a-vm-image-through-the-portal"></a>Удаление образа виртуальной машины через портал

1. Откройте портал администрирования, размещенный по адресу [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).

2. Выберите **Marketplace management** (Управление Marketplace) и виртуальную машину, которую вы хотите удалить.

3. Нажмите кнопку **Delete**(Удалить).

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>Добавление образа виртуальной машины в Marketplace с помощью PowerShell

> [!Note]  
> Добавляемый образ будет доступен только для шаблонов на базе Azure Resource Manager и развертываний PowerShell. Чтобы обеспечить пользователям доступ к образу в Marketplace, опубликуйте его в качестве элемента Marketplace, как описывается в статье [Создание и публикация элемента Marketplace](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-and-publish-marketplace-item)

1. [Установите PowerShell для Azure Stack](azure-stack-powershell-install.md).  

2. Войдите в Azure Stack с правами оператора. Этот процесс описан в статье [Configure the Azure Stack PowerShell environment](azure-stack-powershell-configure-admin.md) (Настройка окружения Azure Stack PowerShell).

3. Откройте PowerShell с помощью командной строки с повышенными привилегиями и выполните следующую команду:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>” `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
  ````

  Командлет **Add-AzsPlatformimage** позволяет задать значения, которые используются в шаблонах Azure Resource Manager для ссылки на образ виртуальной машины. Допустимые значения:
  - **publisher**  
    Например: `Canonical`  
    Сегмент имени издателя образа виртуальной машины, которое применяется пользователями при развертывании образа. Например, **Microsoft**. Не используйте пробел или другие специальные символы в этом поле.  
  - **offer**  
    Например: `UbuntuServer`  
    Сегмент имени предложения образа виртуальной машины, которое применяется пользователями при развертывании образа виртуальной машины. Например, **WindowsServer**. Не используйте пробел или другие специальные символы в этом поле.  
  - **sku**  
    Например: `14.04.3-LTS`  
    Сегмент имени SKU образа виртуальной машины, которое применяется пользователями при развертывании образа виртуальной машины. Например, **Datacenter2016**. Не используйте пробел или другие специальные символы в этом поле.  
  - **version**  
    Например: `1.0.0`  
    Версия виртуальной машины, которая применяется пользователями при развертывании образа виртуальной машины. Эта версия имеет формат *\#.\#.\#*. Например, **1.0.0**. Не используйте пробел или другие специальные символы в этом поле.  
  - **osType**  
    Например: `Linux`  
    Тип ОС образа должен быть **Windows** или **Linux**.  
  - **OSUri**  
    Например: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    В качестве значения `osDisk` вы можете указать URI хранилища BLOB-объектов.  

    Дополнительные сведения см. в справочнике PowerShell в разделах о командлетах [Add-AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) и [New-DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject).

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>Добавление пользовательского образа виртуальной машины в Marketplace с помощью PowerShell

1. [Установите PowerShell для Azure Stack](azure-stack-powershell-install.md).

  ```PowerShell  
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ```

2. Если вы используете **службы федерации Active Directory (AD FS)**, примените следующий командлет:

  ```PowerShell
  # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAuidence endpoint for your environment>"

  # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    ```

3. Войдите в Azure Stack с правами оператора. Этот процесс описан в статье [Configure the Azure Stack PowerShell environment](azure-stack-powershell-configure-admin.md) (Настройка окружения Azure Stack PowerShell).

4. Создайте на глобальной платформе Azure или в Azure Stack учетную запись хранения, где будет храниться пользовательский образ виртуальной машины. Дополнительные сведения см. в [кратком руководстве по передаче, скачиванию и составлению списка больших двоичных объектов с помощью портала Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

5. Подготовьте образ операционной системы Windows или Linux в формате VHD (не VHDX), отправьте этот образ в учетную запись хранения и запишите URI, по которому можно получить этот образ виртуальной машины из PowerShell.  

  ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

6. (Необязательно.) В образе виртуальной машины можно передать массив дисков с данными. Создайте диски данных с помощью командлета New-DataDiskObject. Откройте PowerShell в командной строке с повышенными привилегиями и выполните следующую команду:

  ````PowerShell  
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
  ````

7. Откройте PowerShell с помощью командной строки с повышенными привилегиями и выполните следующую команду:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>” -OSType "<ostype>" -OSUri "<osuri>"
  ````

    Дополнительные сведения о командлетах Add-AzsPlatformimage и New-DataDiskObject см. в [документации по модулю оператора Azure Stack](https://docs.microsoft.com/powershell/module/) для Microsoft PowerShell.

## <a name="remove-a-vm-image-by-using-powershell"></a>Удаление образа виртуальной машины с помощью PowerShell

Если отправленный образ виртуальной машины больше не требуется, его можно удалить из Marketplace с помощью следующего командлета:

1. [Установите PowerShell для Azure Stack](azure-stack-powershell-install.md).

2. Войдите в Azure Stack с правами оператора.

3. Откройте PowerShell с помощью командной строки с повышенными привилегиями и выполните следующую команду:

  ````PowerShell  
  Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
  ````
  Командлет **Remove-AzsPlatformimage** позволяет задать значения, которые используются в шаблонах Azure Resource Manager для ссылки на образ виртуальной машины. Допустимые значения:
  - **publisher**  
    Например: `Canonical`  
    Сегмент имени издателя образа виртуальной машины, которое применяется пользователями при развертывании образа. Например, **Microsoft**. Не используйте пробел или другие специальные символы в этом поле.  
  - **offer**  
    Например: `UbuntuServer`  
    Сегмент имени предложения образа виртуальной машины, которое применяется пользователями при развертывании образа виртуальной машины. Например, **WindowsServer**. Не используйте пробел или другие специальные символы в этом поле.  
  - **sku**  
    Например: `14.04.3-LTS`  
    Сегмент имени SKU образа виртуальной машины, которое применяется пользователями при развертывании образа виртуальной машины. Например, **Datacenter2016**. Не используйте пробел или другие специальные символы в этом поле.  
  - **version**  
    Например: `1.0.0`  
    Версия виртуальной машины, которая применяется пользователями при развертывании образа виртуальной машины. Эта версия имеет формат *\#.\#.\#*. Например, **1.0.0**. Не используйте пробел или другие специальные символы в этом поле.  
    
    Дополнительные сведения о командлете Remove-AzsPlatformimage см. в [документации по модулю оператора Azure Stack](https://docs.microsoft.com/powershell/module/) для Microsoft PowerShell.

## <a name="next-steps"></a>Дополнительная информация

[Подготовка виртуальной машины](azure-stack-provision-vm.md)
