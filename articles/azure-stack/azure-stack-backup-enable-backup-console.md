---
title: Включение резервного копирования для Azure Stack на портале администрирования | Документация Майкрософт
description: Включение службы резервного копирования инфраструктуры с помощью портала администрирования для восстановления Azure Stack в случае сбоя.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffgilb
ms.openlocfilehash: 08bce6284b672ae092e2cee3c26140e8c6049a34
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242858"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Включение резервного копирования для Azure Stack на портале администрирования
Включите службу резервного копирования инфраструктуры с помощью портала администрирования, чтобы разрешить Azure Stack создавать резервные копии. Вы сможете использовать эти резервные копии для восстановления среды с помощью облачного восстановления в случае [неустранимого сбоя](.\azure-stack-backup-recover-data.md). Облачное восстановление предназначено для того, чтобы операторы и пользователи смогли входить на портал по завершении восстановления. При этом восстанавливаются подписки пользователей, в том числе права доступа на основе ролей и роли, исходные планы, предложения и ранее определенные квоты вычислительных ресурсов, ресурсов хранилища и сетевых ресурсов.

Тем не менее служба "Резервное копирование инфраструктуры" не выполняет резервное копирование виртуальных машин IaaS, конфигураций сети и ресурсов хранилища, таких как учетные записи хранения, большие двоичные объекты, таблицы и т. д., поэтому пользователи, выполнившие вход после облачного восстановления, не увидят свои ранее существовавшие ресурсы. Кроме того, эта служба не выполняет резервное копирование ресурсов и данных PaaS (платформа как услуга). 

Администраторы и пользователи ответственны за резервное копирование и восстановление ресурсов IaaS и PaaS отдельно от процессов резервного копирования инфраструктуры. Дополнительные сведения о резервном копировании ресурсов IaaS и PaaS доступны по следующим ссылкам:

- [Виртуальные машины](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [Служба приложений](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Прежде чем включить резервное копирование в консоли, необходимо настроить службу резервного копирования. Это можно сделать с помощью PowerShell. Дополнительные сведения см. в статье [Включение резервного копирования для Azure Stack с помощью PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Включение резервного копирования

1. Откройте портал администрирования Azure Stack по адресу: [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Выберите **Дополнительные службы** > **Резервное копирование инфраструктуры**. Выберите **Конфигурация** в колонке **Резервное копирование инфраструктуры**.

    ![Azure Stack — параметры контроллера резервного копирования](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Введите путь к **расположению хранилища резервных копий**. В качестве пути к общей папке на отдельном устройстве нужно использовать UNC-строку. UNC-строка указывает расположение ресурсов, например общих файлов или устройств. Для службы можно использовать IP-адрес. Чтобы обеспечить доступность резервных копий данных после аварии, устройство должно находиться в отдельном расположении.
    > [!Note]  
    > Если в корпоративной среде поддерживается разрешение имен из сети инфраструктуры Azure Stack, вы можете использовать полное доменное имя, а не IP-адрес.
4. Введите **имя пользователя**, указав домен и имя пользователя. У этого пользователя должны быть достаточные права на чтение и запись файлов. Например, `Contoso\backupshareuser`.
5. Введите **пароль** пользователя.
5. Введите пароль еще раз для его **подтверждения**.
6. Укажите общий ключ в поле **Ключ шифрования**. С помощью этого ключа шифруются резервные копии. Обязательно храните его в безопасном расположении. После первой установки этого ключа или его смены в будущем он не будет отображаться в этом интерфейсе. Инструкции по созданию общего ключа см. в скриптах в руководстве по [включению резервного копирования для Azure Stack с помощью PowerShell](azure-stack-backup-enable-backup-powershell.md).
7. Нажмите кнопку **ОК**, чтобы сохранить параметры контроллера резервного копирования.

Чтобы выполнить резервное копирование, нужно скачать средства Azure Stack и выполнить командлет PowerShell **Start-AzSBackup** на узле администрирования Azure Stack. Дополнительные сведения см. в статье [Резервное копирование для Azure Stack](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Дополнительная информация

- Узнайте, как выполнить резервное копирование. См. статью [Резервное копирование для Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Узнайте, как проверить, выполнено ли резервное копирование. См. раздел [Подтверждение завершения резервного копирования на портале администрирования](azure-stack-backup-back-up-azure-stack.md).