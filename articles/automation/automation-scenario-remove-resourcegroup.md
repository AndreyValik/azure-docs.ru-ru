---
title: Автоматизация удаления групп ресурсов с помощью службы автоматизации Azure
description: Сценарий службы автоматизации Azure с использованием рабочего процесса PowerShell предусматривает удаление всех групп ресурсов в подписке с помощью модулей Runbook.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f0ec42a1e62c4aa35bb6bdffce0acf00a971cb7d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/16/2018
ms.locfileid: "34194526"
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Сценарий службы автоматизации Azure: автоматизация удаления групп ресурсов
Многие клиенты создают несколько групп ресурсов. Одни можно использовать для управления рабочими приложениями, а другие — в качестве промежуточных сред или сред разработки и тестирования. Но автоматизация развертывания этих ресурсов и возможность удалить группу ресурсов с помощью нажатия кнопки — это две разных задачи. С помощью службы автоматизации Azure можно упростить эту распространенную задачу управления. Это будет полезно, если вы работаете с подпиской Azure, для которой настроена предельная сумма расходов, например в рамках предложения для участников MSDN или программы Microsoft Partner Network Cloud Essentials.

В этом сценарии используется модуль Runbook PowerShell, который предназначен для удаления из подписки одной или нескольких выбранных групп ресурсов. Для модуля Runbook по умолчанию настроено тестирование перед выполнением дальнейших действий. Это позволяет предотвратить случайное удаление группы ресурсов.   

## <a name="getting-the-scenario"></a>Получение сценария
В этом сценарии используется модуль Runbook PowerShell, который можно скачать из [коллекции PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Его также можно импортировать непосредственно из [коллекции Runbook](automation-runbook-gallery.md) на портале Azure.<br><br>

| Модуль Runbook | ОПИСАНИЕ |
| --- | --- |
| Remove-ResourceGroup |Удаляет из подписки одну или несколько групп ресурсов Azure вместе со связанными ресурсами. |

<br>
Для этого модуля Runbook определены следующие входные параметры.

| Параметр | ОПИСАНИЕ |
| --- | --- |
| NameFilter (обязательный) |Определяет имя фильтра для ограничения групп ресурсов, которые будут удалены. Можно передать несколько значений, указав их через запятую.<br>Фильтр не учитывает регистр, возвращая все группы ресурсов, которые содержат строку. |
| PreviewMode (необязательный) |Выполняет модуль Runbook, чтобы узнать, какие группы ресурсов будут удалены.<br>Значение по умолчанию **true** используется, чтобы избежать случайного удаления одной или нескольких групп ресурсов, переданных в модуль Runbook. |

## <a name="install-and-configure-this-scenario"></a>Установка и настройка сценария
### <a name="prerequisites"></a>предварительным требованиям
Этот модуль Runbook выполняет проверку подлинности с помощью [учетной записи запуска от имени Azure](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-the-runbooks"></a>Установка и публикация модулей Runbook
Скачав модули Runbook, вы можете импортировать их с помощью процедуры, описанной в статье [Создание или импорт модуля Runbook в службе автоматизации Azure](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation). Опубликуйте модуль Runbook после импорта в учетную запись службы автоматизации.

## <a name="using-the-runbook"></a>Использование модуля Runbook
Ниже объясняется, как выполнить модуль Runbook и как он работает. Мы протестируем модуль Runbook без удаления группы ресурсов.  

1. На портале Azure откройте свою учетную запись службы автоматизации и щелкните **Модули Runbook**.
2. Выберите модуль Runbook **Remove-ResourceGroup** и нажмите кнопку **Запустить**.
3. При запуске модуля Runbook открывается страница **Запуск Runbook**, где можно настроить параметры. Укажите имена групп ресурсов в подписке, которые можно протестировать и без последствий удалить.

   > [!NOTE]
   > Чтобы избежать случайного удаления выбранных групп ресурсов, необходимо присвоить для параметра **Previewmode** значение **true**. Этот модуль Runbook не удаляет группу ресурсов, содержащую учетную запись службы автоматизации, которая отвечает за его выполнение.  
   >
   >
1. Настроив значения всех параметров, нажмите кнопку **ОК**. Модуль Runbook будет помещен в очередь для выполнения.  

Сведения о задании Runbook **Remove-ResourceGroup** можно просмотреть на портале Azure в разделе **Ресурс**. Для этого выберите плитку **Задания** модуля Runbook. Выберите задание, данные которого необходимо просмотреть. Помимо общей информации о задании, в сводке будут перечислены входные параметры, поток вывода, а также все возникшие исключения.<br> ![Состояние задания модуля Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

**Сводка по заданию** содержит сообщения из потоков вывода, предупреждений и ошибок. Щелкните **Вывод**, чтобы просмотреть подробные сведения о результатах выполнения модуля Runbook.<br> ![Выходные данные модуля Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Дополнительная информация
* Инструкции по созданию модулей Runbook см. в статье [Создание или импорт модуля Runbook в службе автоматизации Azure](automation-creating-importing-runbook.md).
* Сведения о том, как начать работу с модулями Runbook рабочих процессов PowerShell, см. в статье [Первый Runbook рабочего процесса PowerShell](automation-first-runbook-textual.md).
