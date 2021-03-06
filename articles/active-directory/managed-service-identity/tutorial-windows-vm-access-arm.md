---
title: Доступ к Azure Resource Manager с помощью MSI виртуальной машины Windows
description: Из этого руководства вы узнаете, как получить доступ к Azure Resource Manager с помощью Управляемого удостоверения службы виртуальной машины Windows.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: bd314dd1543280cf2533e45f156ca634d15d1d2a
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247250"
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-resource-manager"></a>Доступ к Resource Manager с помощью Управляемого удостоверения службы виртуальной машины Windows

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

В этом руководстве показано, как активировать Управляемое удостоверение службы для виртуальной машины Windows. и его использование для получения доступа к API Azure Resource Manager. Управляемыми удостоверениями службы автоматически управляет Azure. Они позволяют проходить аутентификацию в службах, поддерживающих аутентификацию Azure AD, без указания учетных данных в коде. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Включение Управляемого удостоверения службы на виртуальной машине Windows 
> * Предоставление виртуальной машине доступа к группе ресурсов в Azure Resource Manager 
> * Получение маркера доступа с помощью удостоверения виртуальной машины и вызов Azure Resource Manager с его помощью.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Вход в Azure
Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Создание виртуальной машины Windows в новой группе ресурсов

В рамках этого руководства мы создадим виртуальную машину Windows.  Управляемое удостоверение службы можно также включить на существующей виртуальной машине.

1.  Нажмите кнопку **Создать ресурс** в верхнем левом углу окна портала Azure.
2.  Выберите **Вычисления**, а затем — **Windows Server 2016 Datacenter**. 
3.  Введите сведения о виртуальной машине. **Имя пользователя** и **пароль**, созданные здесь, используются для входа на виртуальную машину.
4.  В раскрывающемся списке выберите нужную **подписку** для виртуальной машины.
5.  Чтобы выбрать новую **группу ресурсов**, в которой вы хотите создать виртуальную машину, щелкните **Создать**. По завершении нажмите кнопку **ОК**.
6.  Выберите размер виртуальной машины. Чтобы просмотреть дополнительные размеры, выберите **Просмотреть все** или измените фильтр **Supported disk type** (Поддерживаемые типы диска). На странице параметров оставьте значения по умолчанию и нажмите кнопку **OK**.

    ![Замещающий текст](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Активация Управляемого удостоверения службы на виртуальной машине 

Управляемое удостоверение службы на виртуальной машине позволяет получить маркеры доступа из Azure AD без необходимости указывать в коде учетные данные. Включение MSI предназначено для двух целей: выполняется регистрация виртуальной машины в Azure Active Directory для создания управляемого удостоверения и его настройка на этой виртуальной машине.

1.  Выберите **виртуальную машину**, на которой необходимо активировать Управляемое удостоверение службы.  
2.  В левой области навигации щелкните **Конфигурация**. 
3.  Появится страница **Managed Service Identity** (Управляемое удостоверение службы). Чтобы зарегистрировать и включить Управляемое удостоверение службы, нажмите кнопку **Да**. Чтобы отключить его, нажмите кнопку "Нет". 
4.  Нажмите кнопку **Сохранить**, чтобы сохранить конфигурацию.  
    ![Замещающий текст](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-resource-group-in-resource-manager"></a>Предоставление виртуальной машине доступа к группе ресурсов в Resource Manager
С помощью Управляемого удостоверения службы код может получить маркеры доступа, чтобы пройти проверку подлинности и получить доступ к ресурсам, поддерживающим проверку подлинности Azure AD.  Azure Resource Manager поддерживает аутентификацию Azure AD.  Сначала нам необходимо предоставить этому удостоверению виртуальной машины доступ к ресурсу в Resource Manager, в этом случае — к группе ресурсов, в которой находится виртуальная машина.  

1.  Перейдите на вкладку **Группы ресурсов**. 
2.  Выберите определенную **группу ресурсов**, созданную ранее для **виртуальной машины Windows**. 
3.  Перейдите к элементу **Управление доступом (IAM)** на панели слева. 
4.  Щелкните **Добавить**, чтобы добавить новое назначение роли **виртуальной машине Windows**.  В поле **Роль** выберите **Читатель**. 
5.  В раскрывающемся списке далее в поле **Назначение доступа к** выберите ресурс **Виртуальная машина**. 
6.  Проверьте, чтобы в раскрывающемся списке **Подписка** была выбрана нужная подписка. В поле **Группа ресурсов** выберите **Все группы ресурсов**. 
7.  И наконец, в поле **Выбрать** выберите свою виртуальную машину Windows в раскрывающемся списке, а затем нажмите кнопку **Сохранить**.

    ![Замещающий текст](media/msi-tutorial-windows-vm-access-arm/msi-windows-permissions.png)

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-resource-manager"></a>Получение маркера доступа с помощью удостоверения виртуальной машины и вызов Azure Resource Manager с его помощью. 

На этом этапе понадобится использовать **PowerShell**.  Если эта среда не установлена, загрузите ее [отсюда](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). 

1.  На портале перейдите к разделу **Виртуальные машины**, выберите свою виртуальную машину Windows и в разделе **Обзор** щелкните **Подключить**. 
2.  Введите **имя пользователя** и **пароль**, добавленные при создании виртуальной машины Windows. 
3.  Теперь, когда создано **подключение к удаленному рабочему столу** с виртуальной машиной, откройте **PowerShell** в удаленном сеансе. 
4.  С помощью командлета PowerShell Invoke-WebRequest выполните запрос к локальной конечной точке Управляемого удостоверения службы, чтобы получить маркер доступа к Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > Значение параметра resource должно точно совпадать со значением, которое будет использоваться в Azure AD. Если используется идентификатор ресурса Resource Manager, добавьте косую черту после универсального кода ресурса (URI).
    
    Затем извлеките полный ответ, который хранится в виде форматированной строки JSON в объекте $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Затем извлеките маркер доступа из ответа.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
    
    Теперь вызовите Azure Resource Manager с использованием маркера доступа. В этом примере мы также используем команду Invoke-WebRequest PowerShell для вызова Azure Resource Manager и включаем маркер доступа в заголовок авторизации.
    
    ```powershell
    (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $ArmToken"}).content
    ```
    > [!NOTE] 
    > URL-адрес чувствителен к регистру, поэтому должен использоваться тот же регистр, который использовался, когда вы присваивали имя группе ресурсов. Проверьте, чтобы в resourceGroups обязательно использовался символ "G" (прописная буква).
        
    Следующая команда возвращает сведения о группе ресурсов:

    ```powershell
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}}
    ```

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как создать назначенное пользователем удостоверение и подключить его к виртуальной машине Azure, а затем получить доступ к API Azure Resource Manager.  Сведения об Azure Resource Manager см. здесь:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

