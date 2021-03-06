---
title: Доступ к SQL Azure с помощью виртуальной машины Windows
description: В этом руководстве описано, как получить доступ к службе SQL Azure с помощью Управляемого удостоверения службы виртуальной машины Windows.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: ca920a93d754254390a5c5c5a066be3144b47fc7
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41920747"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-sql"></a>Руководство. Доступ к SQL Azure с помощью Управляемого удостоверения службы виртуальной машины Windows

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

В этом руководстве описывается доступ к серверу SQL Azure с помощью Управляемого удостоверения службы виртуальной машины Windows. Управляемыми удостоверениями службы автоматически управляет Azure. Они позволяют проходить проверку подлинности в службах, поддерживающих аутентификацию Azure AD, без указания учетных данных в коде. Вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Включение Управляемого удостоверения службы на виртуальной машине Windows 
> * Предоставление виртуальной машине доступа к серверу SQL Azure.
> * Получение маркера доступа с использованием идентификатора виртуальной машины и отправка запроса на сервер SQL Azure с его помощью.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Создание виртуальной машины Windows в новой группе ресурсов

В рамках этого руководства мы создадим виртуальную машину Windows.  Управляемое удостоверение службы можно также активировать на имеющейся виртуальной машине.

1.  Нажмите кнопку **Создать ресурс** в верхнем левом углу окна портала Azure.
2.  Выберите **Вычисления**, а затем — **Windows Server 2016 Datacenter**. 
3.  Введите сведения о виртуальной машине. **Имя пользователя** и **пароль**, созданные здесь, используются для входа на виртуальную машину.
4.  В раскрывающемся списке выберите нужную **подписку** для виртуальной машины.
5.  Чтобы выбрать новую **группу ресурсов**, в которой вы хотите создать виртуальную машину, щелкните **Создать**. По завершении нажмите кнопку **ОК**.
6.  Выберите размер виртуальной машины. Чтобы просмотреть дополнительные размеры, выберите **Просмотреть все** или измените фильтр **Supported disk type** (Поддерживаемые типы диска). На странице параметров оставьте значения по умолчанию и нажмите кнопку **OK**.

    ![Замещающий текст](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Активация управляемого удостоверения службы на виртуальной машине 

Управляемое удостоверение службы на виртуальной машине позволяет получить маркеры доступа из Azure AD без необходимости указывать в коде учетные данные. Активация Управляемого удостоверения службы сообщает Azure о создании управляемого удостоверения для виртуальной машины. Как правило, включение Управляемого удостоверения службы имеет две цели: регистрацию виртуальной машины в Azure Active Directory для создания управляемого удостоверения и его настройку на этой виртуальной машине.

1.  Выберите **виртуальную машину**, на которой необходимо активировать Управляемое удостоверение службы.  
2.  В левой области навигации щелкните **Конфигурация**. 
3.  Появится страница **Managed Service Identity** (Управляемое удостоверение службы). Чтобы зарегистрировать и включить управляемое удостоверение службы, нажмите кнопку **Да**. Чтобы отключить его, нажмите кнопку "Нет". 
4.  Нажмите кнопку **Сохранить**, чтобы сохранить конфигурацию.  
    ![Замещающий текст](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Предоставление виртуальной машине доступа к базе данных на сервере SQL Azure

Теперь вы можете предоставлять виртуальной машине доступ к базе данных на сервере SQL Azure.  Для выполнения этого шага можно использовать имеющийся сервер SQL или создать новый.  Чтобы создать сервер и базу данных с помощью портала Azure, следуйте указаниям в статье [Создание базы данных SQL Azure на портале Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). В [документации по SQL Azure](https://docs.microsoft.com/azure/sql-database/) также есть краткие руководства, описывающие использование Azure CLI и Azure PowerShell.

Для предоставления виртуальной машине доступа к базе данных нужно выполнить три шага:
1.  Создать группу в Azure AD и сделать Управляемое удостоверение службы виртуальной машины ее элементом.
2.  Включить аутентификацию Azure AD для сервера SQL.
3.  Создать в базе данных **автономного пользователя**, представляющего группу Azure AD.

> [!NOTE]
> Обычно создается автономный пользователь, который сопоставляется напрямую с Управляемым удостоверением службы виртуальной машины.  Сейчас SQL Azure не поддерживает субъект-службу Azure AD, представляющую Управляемое удостоверение службы виртуальной машины, которое нужно сопоставить с автономным пользователем.  В качестве поддерживаемого обходного решения сделайте Управляемое удостоверение службы виртуальной машины элементом группы Azure AD, а затем создайте в базе данных автономного пользователя, представляющего группу.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-managed-service-identity-a-member-of-the-group"></a>Создание группы в Azure AD и назначение Управляемого удостоверения службы виртуальной машины ее элементом

Можно использовать существующую группу Azure AD или создать новую с помощью Azure AD PowerShell.  

Сначала установите модуль [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2). Затем войдите с помощью `Connect-AzureAD` и выполните следующую команду, чтобы создать группу и сохранить ее в переменной:

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

Выходные данные выглядят следующим образом. В них содержатся подробные сведения о значении переменной.

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM Managed Service Identity access to SQL
```

Затем добавьте Управляемое удостоверение службы виртуальной машины в группу.  Требуется значение **ObjectId** Управляемого удостоверения службы, которое можно получить с помощью Azure PowerShell.  Сначала скачайте [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Затем войдите с помощью `Connect-AzureRmAccount` и выполните следующие команды, чтобы:
- Убедиться, что для контекста сеанса задана нужная подписка Azure (если имеется несколько).
- Перечислить доступные ресурсы в подписке Azure, чтобы удостовериться в наличии группы ресурсов и виртуальной машины с правильными именами.
- Получить свойства Управляемого удостоверения службы виртуальной машины, используя соответствующие значения для `<RESOURCE-GROUP>` и `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

Выходные данные выглядят, как показано ниже. В них содержатся подробные сведения об идентификаторе объекта субъект-службы Управляемого удостоверения службы виртуальной машины.
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Теперь добавьте Управляемое удостоверение службы виртуальной машины в группу.  Добавить субъект-службу в группу можно только с помощью Azure AD PowerShell.  Выполните следующую команду:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Если вы захотите проверить сведения об участии в группе, выходные данные будут выглядеть следующим образом:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Включение аутентификации Azure AD для сервера SQL

Теперь, когда создана группа и добавлено Управляемое удостоверение службы виртуальной машины как элемента группы, можно [настроить аутентификацию Azure AD для сервера SQL](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-server) следующим образом.

1.  На портале Azure выберите **SQL servers** (Серверы SQL Server) на левой панели навигации.
2.  Щелкните сервер SQL, для которого нужно включить аутентификацию Azure AD.
3.  В разделе колонки **Параметры** щелкните **Администратор Active Directory**.
4.  В командной строке щелкните **Задать администратора**.
5.  Выберите учетную запись пользователя Azure AD, которую необходимо сделать администратором сервера, а затем нажмите кнопку **Выбрать**.
6.  На панели команд нажмите кнопку **Сохранить**.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Создание в базе данных автономного пользователя, представляющего группу Azure AD

В следующем шаге потребуется [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Прежде чем начать, советуем ознакомиться со следующими статьями, содержащими общие сведения об интеграции Azure AD:

- [Универсальная проверка подлинности для Базы данных SQL и хранилища данных SQL (поддержка SSMS для MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Настройка аутентификации Azure Active Directory и управление ею с использованием базы данных SQL или хранилища данных SQL](/azure/sql-database/sql-database-aad-authentication-configure)

1.  Запустите среду SQL Server Management Studio.
2.  В диалоговом окне **Подключение к серверу** в поле **Имя сервера** введите имя сервера SQL.
3.  В поле **Authentication** (Аутентификация) выберите **Active Directory — универсальная с поддержкой MFA**.
4.  В поле **Имя пользователя** введите имя учетной записи Azure AD, установленной в качестве администратора сервера, например helen@woodgroveonline.com.
5.  Щелкните по элементу **Параметры**.
6.  В поле **Подключение к базе данных** введите имя несистемной базы данных, которую требуется настроить.
7.  Щелкните **Подключить**.  Завершите процесс входа в систему.
8.  В **обозревателе объектов** разверните папку **Базы данных**.
9.  Щелкните правой кнопкой мыши пользовательскую базу данных и выберите **Создать запрос**.
10.  В окне запроса введите следующую строку и на панели инструментов нажмите кнопку **Выполнить**.
    
     ```
     CREATE USER [VM Managed Service Identity access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     В результате выполнения команды должен быть создан автономный пользователь для группы.
11.  Очистите окно запроса, введите следующую строку и на панели инструментов нажмите кнопку **Выполнить**.
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM Managed Service Identity access to SQL]
     ```

     В результате выполнения команды автономному пользователю должна быть предоставлена возможность считывания всей базы данных.

Теперь код, запущенный на виртуальной машине, может получить маркер из Управляемого удостоверения службы и использовать его для проверки подлинности на сервере SQL.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Получение маркера доступа с использованием удостоверения виртуальной машины и вызов SQL Azure с его помощью 

В службе SQL Azure реализована поддержка аутентификации Azure AD, поэтому она может напрямую принимать маркеры доступа, полученные с использованием Управляемого удостоверения службы.  Для создания подключения к SQL используется метод на основе **маркера доступа**.  Он реализуется как часть интеграции SQL Azure с Azure AD и отличается от указания учетных данных в строке подключения.

Ниже приведен пример кода .NET для установки подключения к SQL с помощью маркера доступа.  Этот код должен выполняться на виртуальной машине, чтобы иметь доступ к конечной точке Управляемого удостоверения службы виртуальной машины.  Для использования метода с маркером доступа необходима платформа **.NET Framework 4.6** или более поздней версии.  Замените значения AZURE-SQL-SERVERNAME и DATABASE соответствующим образом.  Обратите внимание, что идентификатор ресурса для SQL Azure — https://database.windows.net/.

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call Managed Service Identity endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

Кроме того, вы можете быстро протестировать сквозную настройку с помощью PowerShell без необходимости писать и развертывать приложение на виртуальной машине.

1.  На портале перейдите к разделу **Виртуальные машины**, выберите свою виртуальную машину Windows и в разделе **Обзор** щелкните **Подключить**. 
2.  Введите **имя пользователя** и **пароль**, добавленные при создании виртуальной машины Windows. 
3.  Теперь, когда создано **подключение к удаленному рабочему столу** с виртуальной машиной, откройте **PowerShell** в удаленном сеансе. 
4.  С помощью команды PowerShell `Invoke-WebRequest` выполните запрос к локальной конечной точке Управляемого удостоверения службы, чтобы получить маркер доступа к SQL Azure.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatabase.windows.net%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    Преобразуйте ответ из объекта JSON в объект PowerShell. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Извлеките маркер доступа из ответа.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Откройте подключение к серверу SQL. Не забудьте заменить значения AZURE-SQL-SERVERNAME и DATABASE.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Затем создайте и отправьте запрос на сервер.  Не забудьте заменить значение для TABLE.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Проверьте значение `$DataSet.Tables[0]`, чтобы просмотреть результаты запроса.  Поздравляем, вы выполнили запрос базы данных с помощью Управляемого удостоверения службы виртуальной машины, не предоставляя учетные данные!

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как создать компонент "Управляемое удостоверение службы" и получить доступ к серверу Azure SQL Server.  Дополнительные сведения о сервере Azure SQL Server см. здесь:

> [!div class="nextstepaction"]
>[Функции службы базы данных SQL Azure](/azure/sql-database/sql-database-technical-overview)
