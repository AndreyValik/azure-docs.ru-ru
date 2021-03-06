---
title: Как управлять учетной записью и ключами службы "Карты Azure" | Документация Майкрософт
description: Вы можете управлять учетной записью и ключами доступа службы "Карты Azure" с помощью портала Azure.
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 38926cc42b44e61bd83f6b1e33946a9026cda06d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989407"
---
# <a name="how-to-manage-your-azure-maps-account-and-keys"></a>Как управлять учетной записью и ключами службы "Карты Azure"

Вы можете управлять учетной записью и ключами службы "Карты Azure" через портал Azure. Если у вас есть учетная запись и ключ, вы можете реализовать API-интерфейсы на своем веб-сайте или в мобильном приложении.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="create-a-new-account"></a>Создание учетной записи

1. Войдите на [портале Azure](http://portal.azure.com).

1. Щелкните **Создать ресурс** в верхнем левом углу окна портала Azure.

2. Найдите и выберите службу **Карты**, а затем щелкните **Создать**.

3. Введите сведения о новой учетной записи. 

![Ввод данных учетной записи на портале](./media/how-to-manage-account-keys/new-account-portal.png)

## <a name="manage-keys-on-the-account-page"></a>Управление ключами на странице учетной записи

Создав учетную запись, вы получите два случайно сгенерированных ключа. Используйте ключи для аутентификации в API службы "Карты", когда нужно получить данные карты или создать экземпляр карты JavaScript. 

Ключи можно найти на портале Azure. Перейдите к своей учетной записи, а затем выберите **Ключи** в меню.

![Управление ключами учетной записи на портале](./media/how-to-manage-account-keys/account-keys-portal.png)

На этой странице вы можете скопировать ключи или создать новые. 

## <a name="delete-an-account"></a>Удаление учетной записи

Вы можете удалить учетную запись с портала Azure. Перейдите на страницу сведений об учетной записи и щелкните **Удалить**.

![Удаление учетной записи на портале](./media/how-to-manage-account-keys/account-delete-portal.png)

## <a name="next-steps"></a>Дополнительная информация

Узнайте, как с помощью [API управления службой "Карты"](https://docs.microsoft.com/rest/api/maps-management/accounts) создавать, обновлять и удалять учетные записи службы "Карты". 