---
title: Что такое базовая защита в условном доступе Azure Active Directory? (предварительная версия) | Документация Майкрософт
description: Сведения о том, как обеспечить наличие уровня безопасности не ниже базового с помощью базовой защиты в среде Azure Active Directory.
services: active-directory
keywords: условный доступ к приложениям, условный доступ посредством Azure Active Directory, безопасный доступ к ресурсам организации, политики условного доступа
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 856673d2a5465f9646172a1436ed75c0d73692cb
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003178"
---
# <a name="what-is-baseline-protection-preview"></a>Что такое базовая защита (предварительная версия)?  

За последний год количество атак на удостоверения увеличилось на 300 %. Чтобы защитить среду от все учащающихся атак, Azure Active Directory (Azure AD) представляет новый компонент, который называется базовой защитой. [Базовая защита — это набор предопределенных политик условного доступа](../active-directory-conditional-access-azure-portal.md). Цель этих политик — обеспечить наличие уровня безопасности не ниже базового во всех выпусках Azure AD. 

В этой статье представлены общие сведения о базовой защите в Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>Обязательная многофакторная проверка подлинности для администраторов

Пользователи, имеющие доступ к привилегированным учетным записям, получат неограниченный доступ к вашей среде. Из-за возможностей этих учетных записей при работе с ними следует соблюдать осторожность. Наиболее распространенный способ повышения защиты привилегированных учетных записей — использовать более строгую форму проверки учетной записи при входе в систему. В Azure Active Directory для более строгой проверки учетной записи можно включить обязательную многофакторную проверку подлинности.  

**Обязательная многофакторная проверка подлинности для администраторов** — это базовая политика, которая требует использования многофакторной проверки подлинности для следующих ролей каталога: 

- Глобальный администратор.  

- администратор SharePoint;  

- администратор Exchange;  

- Администратор условного доступа  

- администратор безопасности;  


![Azure Active Directory](./media/baseline-protection/01.png)

Базовая политика позволяет исключать пользователей и группы. Вы можете исключить одну *[административную учетную запись аварийного доступа](../users-groups-roles/directory-emergency-access.md)*, чтобы не заблокировать клиент для себя.


## <a name="enable-a-baseline-policy"></a>Включение базовой политики 

Так как базовые политики находятся в предварительной версии, они не включены по умолчанию. Чтобы включить политику, необходимо активировать ее вручную. Как только эта функция станет общедоступной, политики будут включены по умолчанию. Из-за изменения запланированного поведения наряду с включением и отключением политики существует третий вариант для установки состояния политики: **Автоматически включать политику в будущем**. При выборе этого параметра момент включения политики в будущем определяется корпорацией Майкрософт.      


**Чтобы включить базовую политику, выполните следующие действия:**  

1. Войдите на [портал Azure](https://portal.azure.com) в качестве глобального администратора, администратора безопасности или администратора условного доступа.

2. На **портале Azure** на панели навигации слева щелкните **Azure Active Directory**.

    ![Azure Active Directory](./media/baseline-protection/02.png)

3. На странице **Azure Active Directory** в разделе **Безопасность** щелкните **Условный доступ**.

    ![Условный доступ](./media/baseline-protection/05.png)

4. В списке политик щелкните политику, название которой начинается со слов **Базовая политика:**. 

5. Чтобы включить политику, выберите **Use policy immediately** (Использовать политику немедленно).

6. Выберите команду **Сохранить**. 
 
  
 

## <a name="what-you-should-know"></a>Необходимая информация 

Хотя для управления пользовательскими политиками условного доступа требуется лицензия Azure AD Premium, базовые политики доступны во всех выпусках Azure AD.     

Роли каталога, которые включены в базовую политику, являются наиболее привилегированными ролями Azure AD. 

Если в ваших сценариях используются привилегированные учетные записи, вместо них следует использовать [управляемые удостоверения службы (MSI)](../managed-service-identity/overview.md) или [субъекты-службы с сертификатами](../../azure-resource-manager/resource-group-authenticate-service-principal.md). Для временного решения проблемы можно исключить отдельные учетные записи пользователей из базовой политики. 

Базовые политики применяются к устаревшим механизмам проверки подлинности, таким как POP, IMAP и старый настольный клиент Office. 




## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения можно найти в разделе 

- [Пять шагов по защите инфраструктуры удостоверений](https://docs.microsoft.com/azure/security/azure-ad-secure-steps)

- [Что представляет собой условный доступ в Azure Active Directory?](overview.md) 

