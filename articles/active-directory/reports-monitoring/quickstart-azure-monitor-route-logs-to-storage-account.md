---
title: Руководство. Архивация журналов Azure Active Directory в учетной записи хранения Azure (предварительная версия) | Документация Майкрософт
description: Узнайте, как настроить систему диагностики Azure для отправки журналов Azure Active Directory в учетную запись хранения (предварительная версия)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 061a4f2d1b6a1661e341166ec0a1541af073c1f5
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "41918100"
---
# <a name="tutorial-archive-azure-ad-logs-to-an-azure-storage-account-preview"></a>Руководство: архивация журналов Azure AD в учетную запись хранения Azure (предварительная версия)

В этом руководстве вы узнаете, как настроить параметры диагностики Azure Monitor для передачи журналов Azure Active Directory (Azure AD) в учетную запись хранения Azure.

## <a name="prerequisites"></a>Предварительные требования 

Для использования этой функции необходимо иметь следующее.

* Подписка Azure с учетной записью хранения Azure. Если у вас нет подписки Azure, можно [зарегистрироваться и получить бесплатную пробную версию](https://azure.microsoft.com/free/).
* Клиент Azure AD.
* Пользователь, который является *глобальным администратором* или *администратором безопасности* для этого клиента Azure AD.

## <a name="archive-logs-to-an-azure-storage-account"></a>Архивация журналов в учетную запись хранения Azure

1. Войдите на [портале Azure](https://portal.azure.com). 

2. Щелкните **Azure Active Directory** > **Действие** > **Журналы аудита**. 

3. Выберите **Параметры экспорта**. 

4. В области **Параметры диагностики** выполните одно из действий ниже:
    * Чтобы изменить имеющиеся параметры, выберите **Настройка параметра**.
    * Чтобы добавить новые параметры, выберите **Add diagnostics setting** (Добавить параметр диагностики).  
      Можно задать не более трех параметров. 

    ![Параметры экспорта](./media/quickstart-azure-monitor-route-logs-to-storage-account/ExportSettings.png)

5. Введите понятное имя параметра, чтобы не забыть, для чего он нужен (например, *Отправить в учетную запись хранения Azure*). 

6. Установите флажок **Архивировать в учетной записи хранения**, а затем выберите **Учетная запись хранения**. 

7. Выберите подписку Azure и учетную запись хранения, в которые нужно отправить журналы.
 
8. Нажмите кнопку **ОК**, чтобы закрыть конфигурацию.

9. Выполните одно из приведенных ниже действий или оба:
    * Установите флажок **AuditLogs**, чтобы отправить журналы аудита в учетную запись хранения. 
    * Установите флажок **SignInLogs**, чтобы отправить журналы входа в учетную запись хранения.

10. С помощью ползунка установите период хранения данных журнала. По умолчанию это значение равно *0*. Это означает, что журналы будут храниться в учетной записи хранения бессрочно. Если вы зададите другое значение, события, старше указанного количества дней, будут автоматически очищены.

11. Нажмите кнопку **Сохранить**, чтобы сохранить параметры.

    ![Параметры диагностики](./media/quickstart-azure-monitor-route-logs-to-storage-account/DiagnosticSettings.png)

12. Примерно через 15 минут проверьте наличие журналов в учетной записи хранения. Перейдите на [портал Azure](https://portal.azure.com), щелкните **Учетные записи хранения**, выберите учетную запись хранения, которая использовалась ранее, и щелкните **Большие двоичные объекты**. 

13. Для **журналов аудита** выберите **insights-log-audit**. Для **журналов входа** выберите **insights-logs-signin**.
    ![Учетная запись хранения](./media/quickstart-azure-monitor-route-logs-to-storage-account/StorageAccount.png)

## <a name="next-steps"></a>Дополнительная информация

* [Интерпретация схемы журналов аудита в Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Interpret the Azure AD sign-in logs schema in Azure Monitor (preview)](reference-azure-monitor-sign-ins-log-schema.md) (Интерпретация схемы журналов входа Azure Active Directory в Azure Monitor (предварительная версия))
* [Часто задаваемые вопросы и известные проблемы](overview-activity-logs-in-azure-monitor.md#frequently-asked-questions)
