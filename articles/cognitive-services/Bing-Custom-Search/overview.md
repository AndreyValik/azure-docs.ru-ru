---
title: Что такое "Пользовательский поиск Bing"? | Документация Майкрософт
description: В этой статье представлен общий обзор службы "Пользовательский поиск Bing".
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: 7cd61fc63d0d7734b842ed222c67c6753da9a418
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381012"
---
# <a name="what-is-bing-custom-search"></a>Что такое "Пользовательский поиск Bing"?

Служба "Пользовательский поиск Bing" позволяет создавать специально адаптированные интерфейсы для поиска по темам, которые действительно важны для вас. Например, если ваш веб-сайт предоставляет функцию поиска, то вы можете указать домены, веб-сайты и веб-страницы, по которым Bing будет выполнять поиск. Ваши пользователи увидят результаты поиска, адаптированные к интересующей их информации, вместо того, чтобы пролистывать результаты поиска с несоответствующим содержимым.

Чтобы создать пользовательское представление веб-страницы, перейдите на [портал](https://customsearch.ai) службы "Пользовательский поиск Bing". Этот портал позволяет создать экземпляр пользовательского поиска с указанием доменов, веб-сайтов и веб-страниц, по которым будет выполняться поиск Bing, а также веб-сайтов, по которым поиск выполнять не требуется. Помимо указания известных вам URL-адресов вы также можете воспользоваться порталом для поиска соответствующего содержимого, которое также требуется добавить.

С помощью портала также можно закрепить определенную веб-страницу в верхней части результатов поиска, если пользователь введет какое-то конкретное условие поиска. 

После определения параметров вы можете интегрировать полученный экземпляр пользовательского поиска в свой веб-сайт, в классическое или мобильное приложении, вызывая API пользовательского поиска. При использовании веб-сайта или веб-приложения вы можете разрешить размещенному пользовательскому интерфейсу выполнять функции интерфейса поиска.

На следующем рисунке показана простота интеграции пользовательского поиска.

![Рисунок](./media/bcs-overview.png "Как работает \"Пользовательский поиск Bing\".")

## <a name="customize-search-suggestions"></a>Настройка предложений поиска

Если вы имеете соответствующий уровень подписки на службу пользовательского поиска (см. [страницу расценок](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), то вы можете настроить предложения поиска, которые будут отображаться поисковой системой. API пользовательского автозаполнения возвращает список предлагаемых запросов на основе частично введенной пользователем строки запроса. Функция настройки пользовательского автозаполнения позволяет предоставлять предложения поиска, которые больше соответствуют вашим потребностям. Вы можете указать, возвращать ли только пользовательские предложения или также включать предложения Bing. Если включить предложения Bing, то пользовательские предложения будут предшествовать предложениям Bing. Предложения Bing ограничены контекстом экземпляра службы пользовательского поиска.

## <a name="next-steps"></a>Дополнительная информация

Для быстрого начала работы см. статью [Create your first Bing Custom Search instance](quick-start.md) (Создание первого экземпляра службы "Пользовательский поиск Bing").

Дополнительные сведения о доступных параметрах настройки экземпляра поиска см. в статье [Define a custom search instance](define-your-custom-view.md) (Определение экземпляра пользовательского поиска).

Ознакомьтесь со справочными материалами по [API пользовательского поиска](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference). Руководство содержит список конечных точек, заголовков и параметров запроса, которые будут использоваться для запроса результатов поиска. Оно также содержит определения объектов ответа.

Сведения о настройке предложений см. в статье [Настройка пользовательского автозаполнения](define-custom-suggestions.md).

Обязательно прочтите [эту статью](./use-and-display-requirements.md), чтобы не нарушать какие-либо правила использования результатов поиска.