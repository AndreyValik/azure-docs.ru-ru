---
title: 'Руководство по созданию приложения LUIS, которое возвращает анализ тональности: Azure | Документация Майкрософт'
description: В этом руководстве показано, как добавить анализ тональности в приложение LUIS, чтобы проанализировать фразы на наличие позитивных, негативных и нейтральных эмоций.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: baa449bb9e78a5c6437b0a9528e5d1f10dfa519f
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520458"
---
# <a name="tutorial-9--add-sentiment-analysis"></a>Руководство: 9.  Добавление анализа тональности.
В этом руководстве создается приложение, демонстрирующее, как извлечь позитивные, негативные и нейтральные тональности из фраз.

<!-- green checkmark -->
> [!div class="checklist"]
> * Понимание анализа тональности.
> * Использование приложения LUIS в домене управления персоналом. 
> * Добавление анализа тональности.
> * Тестирование и публикация приложения.
> * Запрос конечной точки приложения для просмотра ответа JSON LUIS. 

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Перед началом работы
Если у вас нет приложения для управления персоналом, созданного с помощью руководства по [предварительно созданной сущности keyPhrase](luis-quickstart-intent-and-key-phrase.md), [импортируйте](luis-how-to-start-new-app.md#import-new-app) файл JSON в новое приложение на веб-сайте [LUIS](luis-reference-regions.md#luis-website). Приложение, которое следует импортировать, находится в репозитории Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-keyphrase-HumanResources.json).

Чтобы сохранить исходное приложение по управлению персоналом, клонируйте версию приложения на странице [Параметры](luis-how-to-manage-versions.md#clone-a-version) и назовите ее `sentiment`. Клонирование — это отличный способ поэкспериментировать с различными функциями LUIS без влияния на исходную версию. 

## <a name="sentiment-analysis"></a>Анализ мнений
Анализ тональности — это возможность определить, является ли фраза пользователя позитивной, негативной или нейтральной. 

Ниже приведены примеры фраз, передающие тональность.

|Мнение|Оценка|Фраза|
|:--|:--|:--|
|Позитивная|0,91 |Тарас Хмелев проделал отличную работу на презентации в Париже.|
|Позитивная|0,84 |Сотрудник jill-jones@mycompany.com отлично поработал над презентацией Parker.|

Анализ тональности — это параметр приложения, который применяется к каждой фразе. Вам не нужно искать и выделять слова, указывающие на тональность в каждой фразе, так как анализ тональности применяется ко всей фразе целиком. 

## <a name="add-employeefeedback-intent"></a>Добавление намерения EmployeeFeedback 
Добавьте новое намерение, чтобы получить отзывы о сотрудниках от других работников компании. 

1. Убедитесь, что приложение Human Resources находится в разделе **Build** (Создание) на веб-сайте LUIS. Вы можете перейти к этому разделу, выбрав **Build** (Создание) в верхней правой строке меню. 

    [ ![Снимок экрана приложения LUIS со вкладкой "Build" (Создание), выделенной в верхней правой строке навигации](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png#lightbox)

2. Выберите **Create new intent**. (Создать намерение).

    [ ![Снимок экрана приложения LUIS со вкладкой "Build" (Создание), выделенной в верхней правой строке навигации](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png#lightbox)

3. В качестве имени нового намерения введите `EmployeeFeedback`.

    ![Диалоговое окно для создания намерения с именем EmployeeFeedback](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Добавьте несколько выражений, чтобы указать на хорошую работу сотрудника или область, в которой требуются улучшения:

    Помните, что в этом приложении для управления персоналом сотрудники определяются в сущности списка `Employee` по имени, электронной почте, добавочному номеру телефона, номеру мобильного телефона и номеру социального страхования в США. 

    |Высказывания|
    |--|
    |Сотрудник 425-555-1212 хорошо проявил себя при организации встречи для коллеги, вернувшейся из декретного отпуска.|
    |Сотрудник 234-56-7891 хорошо проявил себя, оказав поддержку коллеге во время траура.|
    |Сотрудник jill-jones@mycompany.com не располагал всеми необходимыми счетами для оформления документов.|
    |Сотрудник john.w.smith@mycompany.com вернул необходимые бланки с опозданием на месяц и без подписей.|
    |Сотрудник x23456 не явился на важную выездную встречу маркетологов.|
    |Сотрудник x12345 пропустил собрание для обсуждения показателей за июнь.|
    |Мария Шевченко подготовила прекрасное коммерческое предложение для встречи в Лондоне.|
    |Тарас Хмелев проделал отличную работу на презентации в Мюнхене.|

    [ ![Снимок экрана с приложением LUIS и примерами фраз в намерении EmployeeFeedback](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="train-the-luis-app"></a>Обучение приложения LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Настройка приложения для включения анализа тональности
Настройте анализ тональности на странице **публикации**. 

1. Выберите **Publish** (Публикация) на правой верхней панели навигации.

    ![Снимок экрана страницы намерений с выделенной кнопкой публикации ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-button-in-top-nav-highlighted.png)

2. Выберите **Enable Sentiment Analysis** (Включить анализ тональности). 

## <a name="publish-app-to-endpoint"></a>Публикация приложения в конечной точке

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-an-utterance"></a>Запрос конечной точки с фразой

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Перейдите в конец URL-адреса и введите `Jill Jones work with the media team on the public portal was amazing`. Последний параметр строки запроса — `q`. Это **запрос** фразы. Эта фраза не совпадает ни с какими помеченными фразами, поэтому она является хорошим тестом. В результате должно быть возвращено намерение `EmployeeFeedback` с извлечением анализа тональности.

```
{
  "query": "Jill Jones work with the media team on the public portal was amazing",
  "topScoringIntent": {
    "intent": "EmployeeFeedback",
    "score": 0.4983256
  },
  "intents": [
    {
      "intent": "EmployeeFeedback",
      "score": 0.4983256
    },
    {
      "intent": "MoveEmployee",
      "score": 0.06617523
    },
    {
      "intent": "GetJobInformation",
      "score": 0.04631853
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0103248553
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.007531875
    },
    {
      "intent": "FindForm",
      "score": 0.00344597152
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00337914471
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0026357458
    },
    {
      "intent": "None",
      "score": 0.00214573368
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00157622492
    },
    {
      "intent": "Utilities.Confirm",
      "score": 7.379545E-05
    }
  ],
  "entities": [
    {
      "entity": "jill jones",
      "type": "Employee",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "media team",
      "type": "builtin.keyPhrase",
      "startIndex": 25,
      "endIndex": 34
    },
    {
      "entity": "public portal",
      "type": "builtin.keyPhrase",
      "startIndex": 43,
      "endIndex": 55
    },
    {
      "entity": "jill jones",
      "type": "builtin.keyPhrase",
      "startIndex": 0,
      "endIndex": 9
    }
  ],
  "sentimentAnalysis": {
    "label": "positive",
    "score": 0.8694164
  }
}
```

Анализ тональности является положительным с оценкой 0,86. 

## <a name="what-has-this-luis-app-accomplished"></a>Результаты работы этого приложения LUIS
Это приложение с включенным анализом тональности распознало намерение запроса на естественном языке и вернуло извлеченные данные вместе с общей тональностью в виде оценки. 

Ваш чат-бот теперь имеет достаточно информации для определения следующего шага в диалоге. 

## <a name="where-is-this-luis-data-used"></a>Место использования этих данных приложения LUIS 
Приложение LUIS уже выполнило этот запрос. Вызывающее приложение, например чат-бот, может принять результат с наивысшим показателем и данные тональности из фразы, чтобы выполнить следующий шаг. LUIS не выполняет программные действия за чат-бота или вызывающее приложение. LUIS только определяет намерение пользователя. 

## <a name="clean-up-resources"></a>Очистка ресурсов

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"] 
> [Проверка фрагментов речи конечной точки в приложении для управления персоналом](luis-tutorial-review-endpoint-utterances.md) 

