---
title: Состояние пользователей в службе Многофакторной идентификации Azure Microsoft Azure
description: Узнайте подробнее о состояниях пользователей в службе Многофакторной идентификации Azure.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: b9f0571c88b6ec4aa9e3851d5bf618e5104b0652
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716252"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Включение двухфакторной проверки подлинности пользователя

Двухфакторную проверку подлинности можно требовать, используя один из двух подходов. Первый способ — включить многофакторную идентификацию Azure (MFA) для каждого отдельного пользователя. В этом случае пользователи выполняют двухфакторную проверку подлинности при каждом входе (за некоторыми исключениями, к примеру, если они входят из доверенных IP-адресов или включена функция _запоминания устройств_). Второй способ — настроить политику условного доступа, которая требует прохождения двухфакторной проверки подлинности при определенных условиях.

> [!TIP]
> Выберите один из этих методов, чтобы требовать прохождения двухфакторной проверки подлинности, но не оба. Включение Azure MFA для пользователя переопределяет все политики условного доступа.

## <a name="choose-how-to-enable"></a>Выбор способа включения

**Включить, изменив состояние пользователя**. Это традиционный метод применения двухфакторной проверки подлинности, который рассматривается в этой статье. Он подходит и для Azure MFA в облаке, и для сервера Azure MFA. Использование этого метода требует, чтобы пользователи выполняли двухфакторную проверку подлинности при **каждом** входе в учетную запись, и переопределяет политики условного доступа.

Включить с помощью политики условного доступа. Это наиболее гибкий способ включить двухфакторную проверку подлинности для пользователей. Включение с помощью политики условного доступа работает только для Azure MFA в облаке и является компонентом уровня "Премиум" в Azure AD. Дополнительные сведения об этом методе можно найти в статье [Приступая к работе с многофакторной идентификацией Azure в облаке](howto-mfa-getstarted.md).

Включить с помощью защиты идентификации Azure AD. Этот метод использует политику рисков защиты идентификации Azure AD, чтобы требовать двухфакторную проверку подлинности только на основе риска входа для всех облачных приложений. Этот метод требует лицензирования Azure Active Directory P2. Дополнительные сведения об этом методе см. в статье [Защита идентификации Azure Active Directory](../identity-protection/overview.md#risky-sign-ins).

> [!Note]
> Дополнительные сведения о лицензиях и ценах можно найти на страницах цен для [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) и [Многофакторной идентификации](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Включение Azure MFA путем изменения состояния пользователя

Учетные записи пользователей в службе Многофакторной идентификации Azure имеют три различных состояния:

| Status | ОПИСАНИЕ | Затронутые приложения, не использующие браузер | Затронутые приложения, использующие браузер | Затронутая современная аутентификация |
|:---:|:---:|:---:|:--:|:--:|
| Отключено |Состояние по умолчанию для нового пользователя, не зарегистрированного в Azure MFA. |Нет  |Нет  |Нет  |
| Включено |Пользователь указан в Azure MFA, но не зарегистрирован. Ему будет предложено зарегистрироваться при следующем входе в систему. |Нет.  Они будут продолжать работать, пока не завершится регистрация. | Да. После окончания сеанса требуется регистрация в Azure MFA.| Да. После окончания срока действия маркера доступа требуется регистрация в Azure MFA. |
| Принудительно |Пользователь указан и зарегистрирован для использования Azure MFA. |Да. Для приложений нужны пароли приложений. |Да. Аутентификация в Azure MFA требуется при входе в систему. | Да. Аутентификация в Azure MFA требуется при входе в систему. |

Состояние пользователя отражает, указал ли администратор его в Azure MFA и завершил ли пользователь процесс регистрации.

Все пользователи начинают с состояния *Отключено*. При указании пользователей в Azure MFA их состояние меняется на *Включено*. После включения пользователи входят в систему и завершают регистрацию, затем их состояние меняется на *Enforced* (Принудительно).  

### <a name="view-the-status-for-a-user"></a>Просмотр состояния пользователя

Чтобы открыть страницу, на которой можно просмотреть состояние пользователей и управлять им, выполните следующее.

1. Войдите на [портал Azure](https://portal.azure.com) с использованием учетной записи администратора.
2. Последовательно выберите **Azure Active Directory** > **Пользователи и группы** > **Все пользователи**.
3. Выберите **Многофакторная идентификация**.
   ![Выберите Многофакторную идентификации](./media/howto-mfa-userstates/selectmfa.png)
4. Откроется новая страница, где будет показано состояние пользователя.
   ![Состояние пользователя Многофакторной идентификации (снимок экрана)](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Изменение состояния пользователя

1. Выполните шаги выше, чтобы перейти на страницу **пользователей** MFA.
2. Найдите пользователя, для которого требуется включить Azure MFA. Возможно, потребуется изменить представление в верхней части страницы.
   ![Поиск пользователя (снимок экрана)](./media/howto-mfa-userstates/enable1.png)
3. Установите флажок рядом с именем пользователя.
4. Справа, в разделе **быстрых действий**, щелкните **Включить** или **Отключить**.
   ![Включение MFA для выбранного пользователя (снимок экрана)](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > Пользователи с состоянием *Включено* автоматически перейдут в состояние *Enforced* (Принудительно) при регистрации в службе Azure MFA. Не изменяйте состояние пользователя вручную на *Enforced* (Принудительно).

5. Подтвердите свой выбор во всплывающем окне, которое откроется.

После включения уведомите пользователей по электронной почте. Сообщите, что им будет предложено зарегистрироваться при следующем входе. Кроме того, если ваша организация использует небраузерные приложения, не поддерживающие современную аутентификацию, потребуется создать пароли приложений. Можно также добавить ссылку на [руководство для пользователей Azure MFA](../user-help/multi-factor-authentication-end-user.md), чтобы им было проще приступить к работе.

### <a name="use-powershell"></a>Использование PowerShell

Чтобы изменить состояние пользователя с помощью [Azure AD PowerShell](/powershell/azure/overview), измените `$st.State`. Существуют три возможных состояния:

* Включено
* Принудительно
* Отключено  

Не переводите пользователей непосредственно в состояние *Применено*. Если сделать это, приложения, не использующие браузер, перестанут работать, так как пользователь не прошел регистрацию в Azure MFA и не получил [пароль приложения](howto-mfa-mfasettings.md#app-passwords).

PowerShell — удобный инструмент для массового включения пользователей. Создайте сценарий PowerShell, который обходит список пользователей и включает их.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Следующий сценарий является примером.

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="next-steps"></a>Дополнительная информация

Чтобы настроить дополнительные параметры, например надежные IP-адреса, пользовательские голосовые сообщения и предупреждения о мошенничестве, см. статью [Настройка параметров Многофакторной идентификации Azure](howto-mfa-mfasettings.md).

Сведения об управлении пользовательскими параметрами Многофакторной идентификации Azure можно найти в статье [Управление параметрами пользователей с помощью Многофакторной идентификации Azure в облаке](howto-mfa-userdevicesettings.md).
