---
title: Руководство. Улучшение Интеллектуальной службы распознавания речи (LUIS) с помощью ролей шаблона Azure | Документация Майкрософт
titleSuffix: Cognitive Services
description: В этом руководстве описано использование ролей шаблона для контекстно-зависимых элементов, чтобы улучшить прогнозирование API распознавания речи.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/03/2018
ms.author: diberry
ms.openlocfilehash: 2fd473226dca2576be79b90bc05d66599f759713
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524150"
---
# <a name="tutorial-improve-app-with-pattern-roles"></a>Руководство. Улучшение приложения с помощью ролей шаблона

В этом руководстве используется простая сущность с ролями, объединенными с шаблонами, для улучшения прогнозирования намерения и сущности.  При использовании шаблонов для намерения требуется меньшее количество примеров высказываний.

> [!div class="checklist"]
* Общие сведения о ролях шаблона
* Использование простого объекта с ролями 
* Создание шаблона для высказываний с помощью простого объекта с ролями
* Проверка улучшения прогнозирования для шаблона

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Перед началом работы
Если у вас нет приложения для управления персоналом из руководства по [шаблонам](luis-tutorial-pattern.md), [импортируйте](luis-how-to-start-new-app.md#import-new-app) файл JSON в новое приложение на веб-сайте [LUIS](luis-reference-regions.md#luis-website). Приложение, которое следует импортировать, находится в репозитории GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-patterns-HumanResources-v2.json).

Чтобы сохранить исходное приложение по управлению персоналом, клонируйте версию приложения на странице [Параметры](luis-how-to-manage-versions.md#clone-a-version) и назовите ее `roles`. Клонирование — это отличный способ поэкспериментировать с различными функциями LUIS без влияния на исходную версию. 

## <a name="the-purpose-of-roles"></a>Назначение ролей
Назначение ролей — извлечь контекстно-зависимые сущности в высказывании. В высказывании `Move new employee Robert Williams from Sacramento and San Francisco` значения исходного города и города назначения связаны друг с другом и для указания каждого расположения используется общий язык. 

При использовании шаблонов любые объекты в шаблоне должны быть обнаружены _до_ того, как шаблон будет соответствовать высказыванию. 

При создании шаблона первое, что нужно сделать, это выбрать намерение для шаблона. Если выбранное намерение соответствует шаблону, правильное намерение всегда возвращается с высокой оценкой (обычно 99–100 %). 

### <a name="compare-hierarchical-entity-to-simple-entity-with-roles"></a>Сравнение иерархических сущностей для простого объекта с ролями

В [Руководстве 5. Добавление иерархической сущности](luis-quickstart-intent-and-hier-entity.md) описано, как обнаруживается намерение **MoveEmployee**, когда нужно переместить существующего сотрудника из одного здания и офиса в другой. Примеры высказываний включали исходное расположение и место назначения, но не использовали роли. Вместо этого исходным расположением и местом назначения были дочерние элементы иерархической сущности. 

В этом руководстве приложение по управлению персоналом обнаруживает высказывания о перемещении новых сотрудников из одного города в другой. Эти два типа высказываний похожи между собой, но решаются с помощью различных возможностей API распознавания речи.

|Учебник|Пример фразы|Исходное и конечное расположение|
|--|--|--|
|[Иерархическое (нет ролей)](luis-quickstart-intent-and-hier-entity.md)|mv Джил Джонс из **a-2349** в **b-1298**|a-2349, b-1298|
|Руководство (с ролями)|Переместить Билли Патерсона из **Юмы** в **Денвер**.|Юма, Денвер|

Нельзя использовать иерархическую сущность в шаблоне, поскольку в родительских элементах используются только иерархические родительские элементы. Для возвращения названных исходного расположения и расположения назначения можно использовать шаблон.

### <a name="simple-entity-for-new-employee-name"></a>Простая сущность для нового имени сотрудника
Имя нового сотрудника, Билли Патерсона, еще не является элементом сущности **Сотрудник**. Сначала извлекается имя нового сотрудника, чтобы отправить во внешнюю систему для создания учетных данных организации. После создания учетных данных организации учетные данные сотрудника добавляются в объект сущности **Сотрудник**.

Список **Сотрудник** был создан в разделе [Руководство 4. Добавление сущности списка](luis-quickstart-intent-and-list-entity.md).

Сущность **NewEmployee** является простой сущностью без ролей. 

### <a name="simple-entity-with-roles-for-relocation-cities"></a>Простая сущность с ролями для городов перемещения
Новый сотрудник с семьей должны быть перемещены из текущего города в город, где находится вымышленная компания. Новый сотрудник может прибыть из любого города, поэтому нужно учитывать расположение. Список набора, такой как сущность списка, не будет работать, так как будет извлекаться только название города.

Имена ролей, связанных с исходным городом и городом назначения, должны быть уникальными для всех сущностей. Чтобы убедиться, что роли являются уникальными, необходимо привязать их к содержащей сущности через стратегию именования. Сущность **NewEmployeeRelocation** — это простая сущность с двумя ролями: **NewEmployeeReloOrigin** и **NewEmployeeReloDestination**.

### <a name="simple-entities-need-enough-examples-to-be-detected"></a>Потребность простых сущностей в достаточном количестве примеров для обнаружения
Поскольку пример выражения `Move new employee Robert Williams from Sacramento and San Francisco` имеет только машинно-обученные сущности, важно для намерения предоставить достаточное количество примерных высказываний, чтобы сущности были обнаружены.  

**Хотя шаблоны и помогают указать меньше примеров фраз, но если сущности не обнаруживаются, шаблоны не совпадают.**

Если возникают трудности с определением простой сущности, например названием города, рассмотрите возможность добавления списка высказываний с похожим значением. Это помогает определить название города, предоставляя API распознавания речи дополнительные данные об этом типе слова или фразы. Списки фраз только помогают шаблону. Они обнаруживают сущности, которые необходимы шаблону для сравнения. 

## <a name="create-new-entities"></a>Добавление новых сущностей
1. Выберите функцию **Сборка** в верхнем меню.

2. Выберите **Сущности** в меню навигации слева. 

3. Нажмите кнопку **Создать сущность**.

4. Во всплывающем окне введите `NewEmployee`**Простая** сущность.

5. Нажмите кнопку **Создать сущность**.

6. Во всплывающем окне введите `NewEmployeeRelocation`**Простая** сущность.

7. Выберите **NewEmployeeRelocation** из списка сущностей. 

8. Введите название первой роли как `NewEmployeeReloOrigin` и нажмите клавишу "ВВОД".

9. Введите название первой роли `NewEmployeeReloDestination` и нажмите клавишу "ВВОД".

## <a name="create-new-intent"></a>Создание нового намерения
Маркировка объектов на этих шагах может быть проще, если предварительно созданная сущность keyPhrase удаляется перед началом, а затем добавляется обратно после того, как закончится работа с шагами в этом разделе. 

1. В левой области навигации выберите **Intents** (Намерения).

2. Выберите **Create new intent**. (Создать намерение). 

3. Введите `NewEmployeeRelocationProcess` как название намерения во всплывающем диалоговом окне.

4. Введите следующие примеры высказываний, помечая новые сущности. Значения сущности и роли выделены жирным шрифтом. Не забудьте переключиться в режим **Просмотра маркеров безопасности**, чтобы было проще пометить текст. 

    Роль сущности не указывается при добавлении меток в намерение. Это можно сделать позже при создании шаблона. 

    |Фраза|NewEmployee|NewEmployeeRelocation|
    |--|--|--|
    |Переместить **Боба Джонса** из **Сиэтла** в **Лас Колинас**|Боб Джонс|Сиэтл, Лас Колинас|
    |Переместить **Дейва Купера** из **Редмонда** в **Нью-Йорк**|Дейв Купер|Редмонд, Нью-Йорк|
    |Переместить **Джима Смита** из **Торонто** в **Западный Ванкувер**|Джим Смит|Торонто, Западный Ванкувер|
    |Переместить **Дж. Бенсона** из **Бостона** в **Стейнз-на-Темзе**|Дж. Бенсон|Бостон, Стейнс|
    |Переместить **Тревиса** из **Кастело Бранко** в **Орландо**|Тревис|Кастело Бранко, Орландо|
    |Переместить **Тревор Ноттингтон III** из **Аранда де Дуеро** в **Буаз**|Тревор Ноттингтон III|Аранда де Дуеро, Буаз|
    |Переместить **доктора Грега Вильямса** из **Орландо** в  **Элликотт**|Dr. Грег Вильямс|Орландо, Элликотт-Сити|
    |Переместить **Роберта "Бобби" Грэгсон** из **Канзаса** **в Сан-Хуан Капистрано**|Роберт "Бобби" Грэгсон|Канзас, Сан-Хуан Капистрано|
    |Переместить **Марту Артемьеву** из **Бельвью** в **Рокфорд**|Марта Артемьева|Бельвью, Рокфорд|
    |Переместить **Джанет Бартлет** из **Тосканы** в **Санта-Фе**|Джанет Бартлет|Тоскана, Санта-Фе|

    Имя сотрудника сочетает в себе широкий набор префиксов, разное количество слов, синтаксиса и суффиксов. Для LUIS важно понимать варианты имен нового сотрудника. Названия городов также имеют разное количество слов и различный синтаксис. Такое разнообразие важно для обучения LUIS тому, как эти сущности могут выглядеть в речи пользователя. 
    
    Если бы каждый объект имел одно и то же количество слов без вариантов, можно обучить LUIS, что у этой сущности есть только это количество слов и никаких других вариантов. LUIS не сможет правильно предугадать более широкий набор вариантов, так как она не видела ни одного. 

    Если сущность keyPhrase была удалена, верните ее в приложение.

## <a name="train-the-luis-app"></a>Обучение приложения LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Публикация приложения для получения URL-адреса конечной точки

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-without-pattern"></a>Запрос конечной точки без шаблона

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Перейдите в конец URL-адреса и введите `Move Wayne Berry from Miami to Mount Vernon`. Последний параметр строки запроса — `q`. Это **запрос** фразы. 

    ```JSON
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

Оценка прогноза намерений составляет только около 50 %. Если приложению требуется более высокое число, это необходимо исправить. Сущности не были прогнозируемыми.

Шаблоны помогут совершить оценку прогнозирования, тем не менее они должны быть правильно прогнозированными прежде, чем высказывание будет соответствовать шаблону. 

## <a name="add-a-pattern-that-uses-roles"></a>Добавление шаблона, использующего роли
1. Выберите функцию **Сборка** в верхней панели навигации.

2. Выберите **Шаблоны** в меню навигации слева.

3. Выберите **NewEmployeeRelocationProcess** из раскрывающегося списка **Select an intent** (Выбрать намерение). 

4. Введите следующий шаблон: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Если вы тренируете, публикуете и делаете запрос на конечную точку, то будете разочарованы, если не увидите сущности, потому что прогнозирование не улучшилось и шаблон не совпал. Это является следствием того, что не хватает примеров высказываний с помеченными сущностями. Вместо добавления примеров добавьте список высказываний, чтобы устранить эту проблему.

## <a name="create-a-phrase-list-for-cities"></a>Создание списка высказываний для городов
Название городов, так же как имена людей, являются непростой задачей, потому что они могут состоять из любых сочетаний слов и знаков препинания. Но название регионов, областей известны всему миру, поэтому, чтобы начать обучение, служба LUIS должна знать эти города. 

1. Выберите **Список фраз** из раздела **Improve app performance** (Повысить производительность приложения) из меню слева. 

2. Назовите список `Cities` и добавьте в него следующие `values`.

    |Значения из списка фраз|
    |--|
    |Сиэтл;|
    |San Diego|
    |Нью-Йорк|
    |Лос-Анджелес|
    |Портленд|
    |Филадельфия|
    |Майами|
    |Даллас|

    Не добавляйте каждый город в мире или даже каждый город в регионе. LUIS должен уметь распознавать, что город из этого списка. 

    Не забудьте сохранить выбранные **These values are interchangeable** (Взаимозаменяемые значения). Этот параметр означает, что слова из списка рассматриваются как синонимы. Именно так они должны обрабатываться в шаблоне.

    Помните, что в разделе [Руководство 7. Добавление простой сущности и списка фраз](luis-quickstart-primary-and-secondary-data.md) был создан список фраз для повышения обнаружения сущности простой сущности.  

3. Обучите и опубликуйте приложение.

## <a name="query-endpoint-for-pattern"></a>Конечная точка запроса для шаблона
1. В нижней части страницы **публикации** выберите ссылку на **конечную точку**. В результате откроется другое окно браузера с URL-адресом конечной точки в адресной строке. 

2. Перейдите в конец URL-адреса и введите `Move wayne berry from miami to mount vernon`. Последний параметр строки запроса — `q`. Это **запрос** фразы. 

    ```JSON
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
}
    ```

Оценка намерения теперь гораздо выше, и имена ролей являются частью ответа сущности.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Рекомендации](luis-concept-best-practices.md) по приложениям LUIS