---
title: 'Руководство по созданию приложения LUIS для получения данных, соответствующих регулярному выражению: Azure | Документация Майкрософт'
description: В этом руководстве вы узнаете, как создать простое приложение LUIS, использующее намерения и сущность регулярного выражения для извлечения данных.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 994bd6f2a041e25d15c7e0b4a216952cec4101fa
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492829"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>Руководство: 3 Добавление сущности регулярного выражения
В этом руководстве создается приложение, которое демонстрирует, как извлечь согласованно отформатированные данные из фразы с помощью сущности **регулярного выражения**.


<!-- green checkmark -->
> [!div class="checklist"]
> * Общие сведения о сущностях регулярных выражений. 
> * Использование приложения LUIS для работы с кадровыми ресурсами с намерением "FindForm".
> * Добавление сущности регулярного выражения для извлечения номеров формы из фразы.
> * Тестирование и публикация приложения.
> * Запрос конечной точки приложения для просмотра ответа JSON LUIS.

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Перед началом работы
Если у вас нет приложения Human Resources, созданного с помощью руководства по [предварительно созданным сущностям](luis-tutorial-prebuilt-intents-entities.md), [импортируйте](luis-how-to-start-new-app.md#import-new-app) файл JSON из репозитория GitHub с [примерами LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json) в новое приложение на веб-сайте [LUIS](luis-reference-regions.md#luis-website).

Чтобы сохранить исходное приложение по управлению персоналом, клонируйте версию приложения на странице [Параметры](luis-how-to-manage-versions.md#clone-a-version) и назовите ее `regex`. Клонирование — это отличный способ поэкспериментировать с различными функциями LUIS без влияния на исходную версию. 


## <a name="purpose-of-the-regular-expression-entity"></a>Назначение сущности регулярного выражения
Назначение сущности — получение важных данных, которые содержатся во фразе. Приложение использует сущность регулярного выражения для извлечения форматированных номеров формы в системе управления персоналом из фразы. Это не задача машинного обучения. 

Ниже приведены примеры простых фраз.

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

Ниже приведены сокращенные или сленговые версии фраз.

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
Сущность регулярного выражения для поиска номера формы — `hrf-[0-9]{6}`. Это регулярное выражение сопоставляет символьные литералы `hrf -`, но не учитывает регистр и варианты языка и региональных параметров. Оно ищет цифры 0–9 (всего 6 цифр).

HRF — это форма в системе управления персоналом.

### <a name="tokenization-with-hyphens"></a>Разметки с дефисами
LUIS помечает фразы, добавляемые в намерение. При разметке этих фраз добавляются пробелы до и после дефиса (`Where is HRF - 123456?`). Регулярное выражение применяется к фразам в необработанной форме до выполнения разметки. Так как он применяется к _необработанные_ формы, регулярное выражение не имеет дело с границами слов. 


## <a name="add-findform-intent"></a>Добавление намерения FindForm

1. Убедитесь, что приложение Human Resources находится в разделе **Build** (Создание) на веб-сайте LUIS. Вы можете перейти к этому разделу, выбрав **Build** (Создание) в верхней правой строке меню. 

2. Выберите **Create new intent**. (Создать намерение). 

3. Введите `FindForm` во всплывающем диалоговом окне и нажмите кнопку **Done** (Готово). 

    ![Снимок экрана диалогового окна создания намерения со служебными программами в поле поиска](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Добавьте примеры фраз в намерение.

    |Примеры фраз|
    |--|
    |Каков URL-адрес для hrf-123456?|
    |Где находится hrf-345678?|
    |Когда hrf-456098 был обновлен?|
    |Обновлял ли Артем Кузнецов hrf-234639 на прошлой неделе?|
    |Сколько существует версий hrf-345123?|
    |Кому необходимо авторизовать форму hrf-123456?|
    |Скольким сотрудникам необходимо подтвердить hrf-345678?|
    |Дата hrf-234123?|
    |Автор hrf-546234?|
    |Заголовок hrf-456234?|

    [ ![Снимок экрана страницы намерения с выделенными фразами](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    В приложении есть предварительно созданная сущность номера, добавленная в предыдущем руководстве, поэтому каждый номер формы помечен. Этого может быть достаточно для клиентского приложения, но номер не будет помечен типом. Благодаря созданию сущности с соответствующим именем клиентское приложение может обрабатывать возвращенную из LUIS сущность соответствующим образом.

## <a name="create-an-hrf-number-regular-expression-entity"></a>Создание сущности регулярного выражения для номера HRF 
Создайте сущность регулярного выражения, чтобы сообщить LUIS формат номера HRF, выполнив следующие шаги:

1. В области слева выберите **Entities** (Сущности).

2. Нажмите кнопку **Create new entity** (Создать сущность) на странице "Сущности". 

3. Во всплывающем диалоговом окне введите имя новой сущности `HRF-number` и выберите **RegEx** как тип сущности, а затем введите `hrf-[0-9]{6}` в качестве регулярного выражения и нажмите кнопку **Done** (Готово).

    ![Снимок экрана всплывающего диалогового окна, где задаются свойства новой сущности](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Выберите **Intents** (Намерения), а затем намерение **FindForm**, чтобы увидеть регулярное выражение, помеченное во фразе. 

    [![Снимок экрана помеченной фразы с имеющейся сущностью и шаблоном регулярного выражения](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Так как сущность не обучена с помощью машинного обучения, метка применяется к фразам и отображается на веб-сайте LUIS сразу же после создания.

## <a name="train-the-luis-app"></a>Обучение приложения LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Публикация приложения для получения URL-адреса конечной точки

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Запрос конечной точки с другой фразой

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Перейдите в конец URL-адреса и введите `When were HRF-123456 and hrf-234567 published in the last year?`. Последний параметр строки запроса — `q`. Это **запрос** фразы. Эта фраза не совпадает ни с какими помеченными фразами, поэтому она является хорошим тестом. В результате должно быть возвращено намерение `FindForm` с двумя номерами форм: `HRF-123456` и `hrf-234567`.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    Номера во фразе возвращаются дважды: один раз как новая сущность `hrf-number` и один раз в качестве предварительно созданной сущности `number`. Как показывает этот пример, фраза может иметь несколько сущностей, которые могут принадлежать к одному типу. С помощью сущности регулярного выражения LUIS извлекает именованные данные, что программно более полезно для клиентского приложения, получающего ответ JSON.

## <a name="what-has-this-luis-app-accomplished"></a>Результаты работы этого приложения LUIS
Приложение определило намерение и возвратило извлеченные данные. 

Ваш чат-бот теперь имеет достаточно информации для определения основного действия, `FindForm`, и номеров форм в поиске. 

## <a name="where-is-this-luis-data-used"></a>Место использования этих данных приложения LUIS 
Приложение LUIS уже выполнило этот запрос. Вызывающее приложение, такое как чат-бот, может принимать результат с наивысшим показателем и номера форм, а также искать по стороннему API. LUIS не выполняет эту работу. LUIS только определяет намерение пользователя и извлекает данные о нем. 

## <a name="clean-up-resources"></a>Очистка ресурсов

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Дополнительные сведения о списке сущности](luis-quickstart-intent-and-list-entity.md)

