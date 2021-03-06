---
title: Сквозная аутентификация Azure AD Connect — как это работает? | Документация Майкрософт
description: В этой статье описывается процедура сквозной аутентификации Azure Active Directory.
services: active-directory
keywords: сквозная проверка подлинности azure ad connect, установка active directory, необходимые компоненты для azure ad, единый вход
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 674952982ac4342caaf31c05f3d644c1e74b649d
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215902"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Сквозная аутентификация Azure Active Directory — подробное техническое руководство
Эта статья содержит обзор принципов работы сквозной аутентификации Azure Active Directory (Azure AD). Подробные технические сведения и информацию о безопасности см. в [руководстве по безопасности](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md).

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Как работает сквозная аутентификация Azure Active Directory?

>[!NOTE]
>В качестве предварительного требования для сквозной проверки подлинности пользователей необходимо подготовить для Azure AD в локальной службе Active Directory с помощью Azure AD Connect. Сквозная проверка подлинности не применяется к исключительно облачным пользователям.

Когда пользователь пытается войти в приложение, защищенное с помощью Azure AD, и на клиенте включена сквозная аутентификация, выполняются следующие действия.

1. Пользователь пытается получить доступ к приложению, например [Outlook Web App](https://outlook.office365.com/owa/).
2. Если пользователь еще не выполнил вход, он перенаправляется на страницу **Вход пользователя** Azure AD.
3. Пользователь вводит имя пользователя на странице входа Azure AD и нажимает кнопку **Далее**.
4. Пользователь вводит пароль на странице входа Azure AD и нажимает кнопку **Войти**.
5. Служба Azure AD, получив запрос на вход, помещает имя пользователя и пароль (зашифрованный с помощью открытого ключа или агентов аутентификации) в очередь.
6. Локальный агент проверки подлинности получает имя пользователя и зашифрованный пароль из очереди. Обратите внимание, что агент не часто опрашивает запросы из очереди, но извлекает запросы через готовое постоянное подключение.
7. Агент расшифровывает пароль, используя свой закрытый ключ.
8. Затем агент проверяет имя пользователя и пароль в Active Directory, используя стандартные интерфейсы API Windows (механизм, похожий на используемый службами федерации Active Directory (AD FS)). Именем пользователя может быть либо локальное имя пользователя по умолчанию (обычно это `userPrincipalName`), либо другой атрибут (известный как `Alternate ID`), настроенный в Azure AD Connect.
9. Локальный контроллер домена Active Directory анализирует запрос и возвращает агенту соответствующий ответ ("успешно", "ошибка", "срок действия пароля истек" или "пользователь заблокирован").
10. Агент проверки подлинности, в свою очередь, возвращает этот ответ в Azure AD.
11. Azure AD оценивает этот ответ и соответствующим образом отвечает пользователю. Например, Azure AD либо сразу же выполняет вход пользователя, либо запрашивает Многофакторную идентификацию Azure.
12. После успешного входа в систему пользователь может получить доступ к приложению.

На схеме ниже показаны все соответствующие компоненты и шаги.

![Сквозная проверка подлинности](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Дополнительная информация
- [Текущие ограничения](active-directory-aadconnect-pass-through-authentication-current-limitations.md). Сведения о том, какие сценарии поддерживаются, а какие нет.
- [Краткое руководство по сквозной аутентификации Azure Active Directory](active-directory-aadconnect-pass-through-authentication-quick-start.md). Настройка и подготовка к работе сквозной аутентификации Azure Active Directory.
- [Migrate from AD FS to Pass-through Authentication](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) (Переход с AD FS на сквозную проверку подлинности). Подробное руководство по переходу с AD FS (или других технологии федерации) на сквозную проверку подлинности.
- [Интеллектуальная блокировка](../authentication/howto-password-smart-lockout.md). Настройте возможность интеллектуальной блокировки на клиенте для защиты учетных записей пользователей.
- [Часто задаваемые вопросы](active-directory-aadconnect-pass-through-authentication-faq.md). Найдите ответы на часто задаваемые вопросы.
- [Устранение неполадок](active-directory-aadconnect-troubleshoot-pass-through-authentication.md). Узнайте, как устранять распространенные проблемы со сквозной аутентификации.
- [Руководство по безопасности](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md). Получите дополнительные технические сведения о сквозной аутентификации.
- [Простой единый вход Azure Active Directory](active-directory-aadconnect-sso.md). Узнайте подробнее об этой дополнительной функции.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect). Оставить запрос на новые функции можно на форуме по Azure Active Directory.

