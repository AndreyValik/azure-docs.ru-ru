---
title: Основные понятия, связанные с Индексатором видео Azure | Документация Майкрософт
description: В этом разделе описываются некоторые основные понятия, связанные со службой "Индексатор видео".
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 224c8b05027f51fb99c8d58be34c3604032c0f77
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399501"
---
# <a name="video-indexer-concepts"></a>Основные понятия, связанные с Индексатором видео
 
В этом разделе описываются некоторые основные понятия, связанные со службой "Индексатор видео".
    
## <a name="summarized-insights"></a>Сводные аналитические сведения

Сводные аналитические сведения отображают агрегированное представление данных: лица, ключевые слова, тональности. Например, вместо того, чтобы обрабатывать каждый из нескольких тысяч диапазонов времени и лица в нем, можно использовать сводные аналитические сведения. Они содержат данные обо всех лицах с указанием диапазонов времени, в течение которых отображается каждое лицо, и значений времени отображения в процентах.

## <a name="topicskeywords"></a>Разделы и ключевые слова

Разделы и ключевые слова входят в список ключевых фраз, которые Индексатор видео извлекает из текста. Например, видео Скотта Гатри (Scott Guthrie) может содержать следующие разделы и ключевые слова: безопасность, Azure, Microsoft Cloud, доход.

## <a name="sentiments"></a>Тональности

Когда Индексатор видео анализирует расшифровку, он обнаруживает также и тональности. Например, фраза "это очень интересное событие" указывает на позитивную тональность.

## <a name="time-range-vs-adjusted-time-range"></a>Диапазон времени по сравнению со скорректированным диапазоном времени

TimeRange — диапазон времени в исходном видео. AdjustedTimeRange — диапазон времени, который относится к текущему списку воспроизведения. Можно создать список воспроизведения с помощью разных строк из разных видео, поэтому вы можете взять видео продолжительностью один час и использовать только одну строку из него, например в диапазоне времени 10:00–10:15. В этом случае вы получите список воспроизведения, содержащий одну строку, где диапазон времени составляет 10:00–10:15, но скорректированный диапазон времени будет 00:00–00:15.
 
## <a name="blocks"></a>Блоки

Блоки предназначены для упрощения восприятия данных. Например, разделение на блоки происходит, когда меняется докладчик или возникает длинная пауза.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в руководстве по [регистрации и отправке видео](video-indexer-get-started.md).

## <a name="see-also"></a>См. также

[Общие сведения об Индексаторе видео](video-indexer-overview.md)
