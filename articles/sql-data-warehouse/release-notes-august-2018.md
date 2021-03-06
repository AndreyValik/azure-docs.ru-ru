---
title: Хранилище данных SQL Azure, заметки о выпуске за август 2018 г. | Документация Майкрософт
description: Заметки о выпуске для хранилища данных SQL Azure.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/06/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 1f3b17f3163c29f9b9e1e47e14ccdbc1e37e1010
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39633928"
---
# <a name="whats-new-in-azure-sql-data-warehouse-august-2018"></a>Что нового в Хранилище данных SQL Azure? Август 2018 г.
Хранилище данных SQL Azure постоянно совершенствуется. В этой статье описаны новые возможности и изменения, вступившие в силу с августа 2018 года.

## <a name="automatic-intelligent-insights"></a>Автоматический Intelligent Insights
Корпорация Майкрософт представила [автоматический Intelligent Insights](https://azure.microsoft.com/blog/automatic-intelligent-insights-to-optimize-performance-with-sql-data-warehouse/), чтобы выполнить обязательство по внедрению автоматизации в хранилище данных в облаке. Больше не нужно будет отслеживать хранилище данных на неравномерное распределение данных и неоптимальную статистику таблиц. Без дополнительной платы хранилище данных SQL предоставляет Intelligent Insights для всех экземпляров Gen2. Благодаря интеграции с [Помощником по Azure](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) можно автоматически получать лучшие практические рекомендации по повышению производительности активных рабочих нагрузок. Хранилище данных SQL анализирует рабочие нагрузки и выводит рекомендации на основе вашего использования. Этот анализ проводится ежедневно и дает возможность отслеживать отчеты об использовании и рекомендации по улучшению рабочей нагрузки.

Рекомендации можно просмотреть на портале Помощника по Azure: ![рекомендации на портале Помощника по Azure для Хранилища данных SQL Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/4e205b6d-df04-48db-8eec-d591f2592cf4.png)

Вы также можете детальнее рассмотреть каждую категорию, чтобы просмотреть рекомендации по определенному оповещению: ![сведения о рекомендации на портале Помощника по Azure для Хранилища данных SQL Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/3c42426e-6969-46e3-9025-c34c0755a302.png)

## <a name="next-steps"></a>Дополнительная информация
Теперь, когда вам уже известны некоторые сведения о хранилище данных SQL, узнайте о том, как его [создать][create a SQL Data Warehouse]. Если вы раньше не работали с Azure, используйте [глоссарий Azure][Azure glossary], чтобы узнать значение новых терминов. Или ознакомьтесь со следующими ресурсами, посвященными хранилищу данных SQL.  

* [Истории успеха клиентов]
* [Блоги]
* [Запросы функций]
* [Видеоролики]
* [Блоги группы консультирования клиентов]
* [Форум Stack Overflow]
* [Twitter]


[Блоги]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Блоги группы консультирования клиентов]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Истории успеха клиентов]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Запросы функций]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Форум Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Видеоролики]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md