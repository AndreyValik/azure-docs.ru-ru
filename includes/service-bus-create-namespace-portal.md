---
title: включение файла
description: включение файла
services: service-bus-messaging
author: sethmanheim
ms.service: service-bus-messaging
ms.topic: include
ms.date: 07/03/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: a110998505ed49c36e1ec722b1dfbf0969def060
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449564"
---
Чтобы приступить к использованию сущностей обмена сообщениями в служебной шине в Azure, сначала необходимо создать пространство имен с уникальным для Azure именем. Пространство имен предоставляет контейнер для адресации ресурсов служебной шины в вашем приложении.

Создание пространства имен службы:

1. Войдите на [портал Azure][Azure portal].
2. На портале в панели навигации слева щелкните **+ Создать ресурс**, **Интеграция** и **Служебная шина**.
3. В диалоговом окне **Создание пространства имен** укажите имя пространства имен. Система немедленно проверяет, доступно ли оно.
4. Убедившись, что пространство имен доступно, выберите ценовую категорию: "Базовый", "Стандартный" или "Премиум". Чтобы использовать [разделы и подписки](../articles/service-bus-messaging/service-bus-queues-topics-subscriptions.md#topics-and-subscriptions), выберите "Стандартный" или "Премиум". Разделы и подписки не поддерживаются в ценовой категории "Базовый".
5. Выберите **подписку** Azure, в рамках которой будет создано пространство имен.
6. Выберите существующую **группу ресурсов** , в которую будет включено это пространство имен, или создайте новую.      
7. Укажите **расположение**— страну или регион для размещения пространства имен.
   
    ![Создание пространства имен][create-namespace]
8. Нажмите кнопку **Создать**. Теперь система создает пространство имен и включает его. Возможно, вам придется подождать несколько минут, пока система выделит ресурсы для вашей учетной записи.

### <a name="obtain-the-management-credentials"></a>Получение учетных данных управления

При создании нового пространства имен автоматически создается начальное правило подписанного URL-адреса (SAS) и связанная с ним пара первичного и вторичного ключей, каждый из которых предоставляет полный контроль над всеми аспектами пространства имен. Сведения о том, как в дальнейшем создавать правила с ограниченными правами для постоянных отправителей и получателей, см. в статье [Аутентификация и авторизация в служебной шине](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md). Чтобы скопировать начальное правило, выполните следующие действия. 

1. Щелкните **Все ресурсы**, а затем щелкните созданное имя пространства имен.
2. В окне пространства имен щелкните **Политики общего доступа**.
3. В окне **Политики общего доступа** щелкните **RootManageSharedAccessKey**.
   
    ![Сведения о подключении][connection-info]
4. В окне **Policy: RootManageSharedAccessKey** (Политика: RootManageSharedAccessKey) нажмите кнопку копирования рядом с полем **Строка подключения — первичный ключ**, чтобы скопировать строку подключения в буфер обмена для последующего использования. Вставьте на время эти значения в Блокноте или любом другом месте.
   
    ![Строка подключения][connection-string]

5. Повторите предыдущий шаг, скопировав и вставив значение **первичного ключа** во временное расположение для последующего использования.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
