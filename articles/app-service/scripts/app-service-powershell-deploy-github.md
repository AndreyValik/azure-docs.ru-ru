---
title: Пример сценария Azure PowerShell. Создание веб-приложения и развертывание кода из GitHub | Документация Майкрософт
description: Пример сценария Azure PowerShell для создания веб-приложения и развертывания кода из GitHub.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: d60503e195c1a8f32992cfb1978ad7a181c069b0
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39323948"
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a>Создание веб-приложения и развертывание кода из GitHub

Этот пример скрипта создает веб-приложение в службе приложений со связанными ресурсами, а затем развертывает код веб-приложения из открытого репозитория GitHub (без непрерывного развертывания). Дополнительные сведения о непрерывном развертывании на GitHub см. в статье [Создание веб-приложения с непрерывным развертыванием из GitHub](app-service-powershell-continuous-deployment-github.md).

При необходимости установите Azure PowerShell с помощью инструкции, приведенной в [руководстве Azure PowerShell](/powershell/azure/overview), а затем выполните команду `Connect-AzureRmAccount`, чтобы создать подключение к Azure. Кроме того, необходима ссылка на репозиторий GitHub, который содержит код веб-приложения.

## <a name="sample-script"></a>Пример скрипта

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполнив пример сценария, вы можете удалить группу ресурсов, веб-приложение и все связанные ресурсы, используя следующую команду.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Создает план службы приложений. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Создает веб-приложение. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Изменяет ресурс в группе ресурсов. |

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о модуле Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

Дополнительные примеры сценариев Azure PowerShell для веб-приложений службы приложений Azure доступны [здесь](../app-service-powershell-samples.md).
