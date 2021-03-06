---
title: Публикация настраиваемого элемента Marketplace в Azure Stack (для оператора облака) | Документация Майкрософт
description: Сведения о том, как оператор Azure Stack может опубликовать настраиваемый элемент Marketplace в Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 4ea23ed01e6432f24024d7e8cc07c2dfe42ac639
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605575"
---
# <a name="the-azure-stack-marketplace-overview"></a>Общие сведения об Azure Stack Marketplace

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Marketplace — это коллекция служб, приложений и ресурсов, настроенных для Azure Stack. Ресурсы включают виртуальные сети, виртуальные машины, хранилище и др. Здесь пользователи создают новые ресурсы и развертывают новые приложения. Это место можно рассматривать как каталог товаров, который пользователи просматривают, выбирая элементы для использования. Чтобы использовать элемент Marketplace, пользователям необходимо подписаться на предложение, которое предоставит им доступ к элементу.

Как оператор Azure Stack вы определяете, какие элементы следует добавить (опубликовать) в Marketplace. Опубликовать можно, например, базы данных, службы приложений и т. д. Публикация сделает их видимыми для всех пользователей. Вы можете опубликовать настраиваемые элементы, которые вы создали. Также можно опубликовать элементы из постоянно растущего [списка элементов Azure Marketplace](azure-stack-marketplace-azure-items.md). После публикации элемента в Marketplace он станет видимым для пользователей в течение пяти минут.

> [!Caution]  
> Все артефакты элемента коллекции, известные как образы и JSON-файлы, доступны без аутентификации после внесения их в Azure Stack Marketplace. Дополнительные сведения о публикации пользовательских элементов Marketplace см. в статье [Создание и публикация элемента Marketplace](azure-stack-create-and-publish-marketplace-item.md).

Чтобы открыть marketplace, в консоли администрирования выберите **New** (Создать).

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Элементы Marketplace
Элемент Azure Stack Marketplace — это служба, приложение или ресурс, которые пользователи могут скачать и использовать. Все элементы Azure Stack Marketplace, включая такие административные элементы, как планы и предложения, видны всем вашим пользователям. Эти элементы не требует подписки для просмотра, но пользователи не смогут с ними работать.

У каждого элемента Marketplace есть:

* Шаблон диспетчера ресурсов для подготовки ресурсов.
* Метаданные, например строки, значки и другие маркетинговые материалы.
* Форматирование сведений для отображения элемента на портале

Каждый элемент, опубликованный в Marketplace, доступен в формате пакета коллекции Azure (файл AZPKG). Ресурсы развертывания или среды выполнения (например, код, ZIP-файлы с программным обеспечением или образы виртуальных машин) следует добавлять в Azure Stack отдельно, а не в составе элемента Marketplace. 

Начиная с версии 1803 и выше, Azure Stack преобразует изображения в разреженные файлы при скачивании из Azure или отправке пользовательских образов. Этот процесс увеличивает время добавления изображения, но позволяет сэкономить место и ускорить развертывание этих образов. Преобразование применяется только для новых образов.  Существующие образы не изменяются. 

## <a name="next-steps"></a>Дополнительная информация
[Скачивание элементов marketplace](azure-stack-download-azure-marketplace-item.md)  
[Создание и публикация элемента Marketplace](azure-stack-create-and-publish-marketplace-item.md)

