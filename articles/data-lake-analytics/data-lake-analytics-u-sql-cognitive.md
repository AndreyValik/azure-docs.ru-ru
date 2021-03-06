---
title: Использование когнитивных возможностей U-SQL в Azure Data Lake Analytics
description: Использование аналитики когнитивных возможностей в U-SQL
services: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: 019c1d53-4e61-4cad-9b2c-7a60307cbe19
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/05/2018
ms.openlocfilehash: ab40d466d7b60dd09b8953012c80d0e84f4ac471
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34802074"
---
# <a name="get-started-with-the-cognitive-capabilities-of-u-sql"></a>Начало работы с когнитивными возможностями U-SQL

## <a name="overview"></a>Обзор
Когнитивные возможности U-SQL позволяют разработчикам использовать аналитику в программах, которые работают с большими данными. 

Доступны следующие когнитивные возможности:
* обработка изображений: распознавание лиц ([пример](https://github.com/Azure-Samples/usql-cognitive-imaging-ocr-hello-world));
* обработка изображений: распознавание эмоций ([пример](https://github.com/Azure-Samples/usql-cognitive-imaging-emotion-detection-hello-world));
* обработка изображений: распознавание объектов, т. е. добавление тегов ([пример](https://github.com/Azure-Samples/usql-cognitive-imaging-object-tagging-hello-world));
* обработка изображений: распознавание текста ([пример](https://github.com/Azure-Samples/usql-cognitive-imaging-ocr-hello-world));
* текст: извлечение ключевых фраз и анализ тональности ([пример](https://github.com/Azure-Samples/usql-cognitive-text-hello-world)).

## <a name="registering-cognitive-extensions-in-u-sql"></a>Регистрация когнитивных возможностей в U-SQL
Прежде чем начать, следуйте шагам по регистрации когнитивных возможностей в U-SQL, описанным в этом [разделе](https://msdn.microsoft.com/azure/data-lake-analytics/u-sql/cognitive-capabilities-in-u-sql#registeringExtensions).

## <a name="next-steps"></a>Дополнительная информация
* [Примеры использования когнитивных возможностей в U-SQL](https://github.com/Azure-Samples?utf8=✓&q=usql%20cognitive)
* [Разработка сценариев U-SQL с помощью средств озера данных для Visual Studio.](data-lake-analytics-data-lake-tools-get-started.md)
* [Использование оконных функций U-SQL для заданий в службе аналитики озера данных Azure](data-lake-analytics-use-window-functions.md)
