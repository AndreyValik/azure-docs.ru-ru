---
title: Управление настройками двухфакторной проверки подлинности в Azure Active Directory | Документация Майкрософт
description: Управление использованием Многофакторной идентификации Azure, в том числе изменение контактных данных и настройка устройств.
services: active-directory
keywords: многофакторная проверка подлинности клиента, проблемы с проверкой подлинности, идентификатор корреляции
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.component: user-help
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: lizross
ms.openlocfilehash: c3fd74731dbed2c2f36d97b3cb42b383f8e4ca0f
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345095"
---
# <a name="manage-your-settings-for-two-step-verification"></a>Управление параметрами двухфакторной проверки подлинности
Эта статья содержит информацию о том, как изменить параметры двухфакторной проверки подлинности или многофакторной идентификации. Если у вас возникают проблемы со входом в учетную запись, см. сведения об устранении неполадок в статье [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) (Трудности с двухфакторной проверкой подлинности).

## <a name="where-to-find-the-settings-page"></a>Где найти страницу параметров
В зависимости от настроек многофакторной проверки подлинности в вашей организации есть несколько способов изменения таких параметров, как номер телефона.

Если служба поддержки компании предоставила вам URL-адрес или процедуру для управления параметрами двухфакторной проверки подлинности, выполните приведенные ниже инструкции. Для всех остальных случаев должны подойти следующие действия. Если при выполнении следующих действий вы увидите параметры, отличающиеся от описанных, значит ваша организация использует персонализированную версию портала. Попросите у администратора ссылку на портал Многофакторной идентификации Azure.

**Переход на страницу "Дополнительная проверка безопасности"**

- Перейдите на сайт https://aka.ms/MFASetup.

    ![Подтверждение](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

Если ссылка не срабатывает, на страницу **Дополнительная проверка безопасности** можно также перейти, выполнив следующие действия:

1. Войдите на сайт [https://myapps.microsoft.com](https://myapps.microsoft.com)  

2. В правом верхнем углу выберите имя учетной записи и **профиль**.

3. Выберите элемент **Дополнительная проверка безопасности**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage-settings/myapps1.png)

4. Загрузится страница дополнительной проверки безопасности с настроенными параметрами.

    ![Подтверждение](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Я хочу изменить свой номер телефона или добавить дополнительный номер
Важно настроить дополнительный номер телефона для проверки подлинности.  Скорее всего, ваш основной номер телефона и мобильное приложение находятся на одном телефоне. Поэтому дополнительный номер телефона является единственным способом возврата в вашу учетную запись в случае потери или кражи телефона.

> [!NOTE]
> Если у вас нет доступа к основному телефону и вам требуется помощь для входа в учетную запись, см. дополнительную справочную информацию в статье [Get help with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) (Получение справки по двухфакторной проверке подлинности).  

**Изменение основного номера телефона**  

1. На странице **Дополнительная проверка безопасности** выберите текстовое поле с текущим номером телефона и исправьте его на новый номер телефона.  
2. Щелкните **Сохранить**.  
3. Если вы используете этот номер телефона в качестве основного варианта проверки подлинности, для сохранения нового номера потребуется подтвердить его.  

**Добавление дополнительного номера телефона**  

1. На странице дополнительной проверки безопасности установите флажок **Дополнительный телефон для проверки подлинности**.  
2. В текстовом поле введите дополнительный номер телефона.  
3. Щелкните **Сохранить**. Настройка завершена.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Повторное выполнение двухфакторной проверки подлинности устройства, помеченного как доверенное

В зависимости от параметров организации при выполнении двухфакторной проверки подлинности в браузере может отображаться флажок "Больше не спрашивать **X** дн.". Если вы установили этот флажок и потеряли устройство или вам кажется, что учетная запись была скомпрометирована, двухфакторную проверку подлинности необходимо восстановить на всех своих устройствах.

1. На странице "Дополнительная проверка безопасности" выберите **Восстановление многофакторной проверки подлинности на ранее доверенных устройствах**.
2. При следующем входе в систему на любом устройстве отобразится запрос на выполнение двухфакторной проверки подлинности.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Как удалить Microsoft Authenticator со старого устройства и перенести на новое?
При удалении приложения с устройства или сбросе его параметров активация на внутреннем сервере не удаляется. Дополнительные сведения можно найти в статье [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Дополнительная информация
* См. советы и справочные сведения по устранению неполадок в статье [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) (Трудности с двухфакторной проверкой подлинности).
* Настройте [пароли приложений](multi-factor-authentication-end-user-app-passwords.md) для всех приложений, которые не поддерживают двухфакторную проверку подлинности.
