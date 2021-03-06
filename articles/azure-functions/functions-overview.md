---
title: Обзор Функций Azure | Документация Майкрософт
description: Узнайте, как использовать Функции Azure для оптимизации асинхронных рабочих нагрузок за считанные минуты.
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: ''
tags: ''
keywords: функции azure, функции, обработка событий, веб-перехватчики, динамические вычисления, независимая архитектура
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/03/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: d60c898225b944801504f38d536262134a31e021
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2017
ms.locfileid: "24877907"
---
# <a name="an-introduction-to-azure-functions"></a>Общие сведения о Функциях Azure  
Функции Azure — это решение для быстрого запуска фрагментов кода (функций) в облаке. Вы можете быстро написать код для решения своих задач, не беспокоясь о наличии приложения или инфраструктуры для его выполнения. Функции делают разработку более продуктивной. Кроме того, вы можете использовать любой язык программирования, например C#, F#, Node.js, Java или PHP. Оплата взимается только один раз при выполнении кода, а масштабирование в Azure выполняется при необходимости. Функции Azure предоставляют возможность разрабатывать в Microsoft Azure [независимые от сервера](https://azure.microsoft.com/overview/serverless-computing/) приложения.

В этой статье представлен общий обзор функций Azure. Чтобы приступить к работе с Функциями Azure сразу, [создайте первую функцию Azure](functions-create-first-azure-function.md). Дополнительные технические сведения о Функциях см. в статье [Справочник разработчика по функциям Azure](functions-reference.md).

## <a name="features"></a>Функции
Вот несколько ключевых возможностей Функций Azure:

* **Выбор языка** — возможность написания функций на удобном для вас языке, например C#, F# или Javascript. Сведения о поддержке других языков см. [здесь](supported-languages.md).
* **Модель ценообразования с оплатой за использование** — оплата только за время, в течение которого выполняется код. Дополнительные сведения см. в [разделе о расценках](#pricing) (план потребления).  
* **Использование собственных зависимостей** — Функции поддерживают NuGet и NPM, поэтому можно использовать предпочитаемые библиотеки.  
* **Встроенная система безопасности** — вы можете защитить функции с HTTP-активацией, которые используют поставщики OAuth (Azure Active Directory, Facebook, Google, Twitter и учетная запись Майкрософт).  
* **Упрощенная интеграция** — вы можете легко пользоваться службами Azure и предложениями SaaS (программное обеспечение как услуга). Примеры см. в разделе [Интеграция](#integrations).  
* **Гибкая разработка** — вы можете программировать функции на портале, а также настраивать непрерывную интеграцию и развертывание кода с помощью [GitHub](../app-service/scripts/app-service-cli-continuous-deployment-github.md), [Visual Studio Team Services](../app-service/scripts/app-service-cli-continuous-deployment-vsts.md) и других [поддерживаемых средств разработки](../app-service/app-service-deploy-local-git.md).  
* **Открытый код** — среда выполнения Функций имеет открытый исходный код, [доступный на GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Что можно сделать с помощью функций
Функции Azure — это идеальное решение для обработки данных, интеграции систем, работы с Интернетом вещей (IoT) и создания простых API-интерфейсов и микрослужб. Используйте Функции для обработки изображений или заказов, обслуживания файлов или для любых задач, выполняемых по расписанию. 

Функции содержат шаблоны, которые помогут приступить к работе, и ключевые сценарии, включая следующие:

* **HTTPTrigger** — активация выполнения кода с помощью HTTP-запроса. Пример см. в статье [Создание первой функции на портале Azure](functions-create-first-azure-function.md).
* **TimerTrigger** — выполнение очистки или других пакетных задач на основе предопределенного расписания. Пример см. в статье [Создание в Azure функции, активируемой по таймеру](functions-create-scheduled-function.md).
* **Объект webhook GitHub** — реагирование на события, происходящие в репозитории GitHub. Дополнительные сведения см. в статье [Создание функции, активируемой объектом webhook GitHub](functions-create-github-webhook-triggered-function.md).
* **Универсальный веб-перехватчик** — обработка HTTP-запросов веб-перехватчика от любой службы, поддерживающей веб-перехватчики. Пример см. в статье [Создание функции, активируемой универсальным веб-перехватчиком](functions-create-generic-webhook-triggered-function.md).
* **CosmosDBTrigger** — обработка документов Azure Cosmos DB при их добавлении или обновлении в коллекциях в базе данных, отличной от SQL. Пример см. в статье [Создание функции, активируемой с помощью Azure Cosmos DB](functions-create-cosmos-db-triggered-function.md).
* **BlobTrigger** — обработка больших двоичных объектов службы хранилища Azure при добавлении в контейнер. Эту функцию можно использовать для изменения размеров изображения. Дополнительные сведения см. в статье [Привязки хранилища BLOB-объектов для Функций Azure](functions-bindings-storage-blob.md).
* **QueueTrigger** — обработка сообщений, помещаемых в очереди службы хранилища Azure. Пример см. в статье [Создание функции, активируемой хранилищем очередей Azure](functions-create-storage-queue-triggered-function.md).
* **EventHubTrigger** — реагирование на события, доставляемые в концентратор событий Azure. Особенно полезно в сценариях инструментирования приложений, обеспечения удобства работы пользователей или функционирования рабочих процессов, а также Интернета вещей (IoT). Дополнительные сведения см. в статье [Привязки концентраторов событий функций Azure](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger** — подключение кода к другим службам Azure или локальным службам посредством ожидания передачи данных из очередей сообщений. Дополнительные сведения см. в статье [Привязки служебной шины в Функциях Azure](functions-bindings-service-bus.md).
* **ServiceBusTopicTrigger** — подключение кода к другим службам Azure или локальным службам посредством подписки на разделы. Дополнительные сведения см. в статье [Привязки служебной шины в Функциях Azure](functions-bindings-service-bus.md).

Функции Azure поддерживают *триггеры*, позволяющие запускать выполнение кода, и *привязки*, позволяющие упростить программирование входных и выходных данных. Подробное описание триггеров и привязок в Функциях Azure см. в статье [Справочник разработчика по триггерам и привязкам в Функциях Azure](functions-triggers-bindings.md).

## <a name="integrations"></a>Интеграция
Функции Azure интегрируются с различными службами Azure, а также службами сторонних поставщиков. Эти службы могут активировать функцию и запустить выполнение. Кроме того, их можно использовать в качестве входных и выходных данных кода. Функции Azure поддерживают интеграцию со следующими службами:

* Azure Cosmos DB
* Концентраторы событий Azure 
* Сетка событий Azure
* служба мобильных приложений Azure (таблицы);
* Концентраторы уведомлений Azure
* служебная шина Azure (очереди и разделы);
* служба хранилища Azure (большие двоичные объекты, очереди и таблицы); 
* GitHub (веб-перехватчики);
* локальные службы (с использованием служебной шины).
* Twilio (SMS-сообщения).

## <a name="pricing"></a>Расценки на использование Функций
В службе "Функции Azure" существует два вида тарифных планов. Выберите тот, который лучше всего соответствует вашим потребностям: 

* **План потребления**. Когда функция выполняется, Azure предоставляет для нее все необходимые вычислительные ресурсы. Вам не нужно беспокоиться об управлении ресурсами. Вы платите только за то время, в течение которого выполняется код. 
* **План службы приложений**. Позволяет запускать функции как веб-приложения, мобильные приложения и приложения API. Если служба приложений уже используется для других приложений, функции можно запускать в том же плане без дополнительной оплаты. 

Дополнительные сведения о планах размещения см. в статье [Планы потребления и службы приложений Функций Azure](functions-scale.md). Подробные сведения о ценообразовании см. на странице с информацией о [ценах на Функции Azure](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Дальнейшие действия
* [Создание первой функции Azure](functions-create-first-azure-function.md)  
  Приступите к созданию первой функции с помощью быстрой настройки Функций Azure. 
* [Справочник разработчика по функциям Azure](functions-reference.md)  
  Дополнительные технические сведения о среде выполнения Функций Azure, а также справочник по программированию функций и определению триггеров и привязок.
* [Тестирование функций Azure](functions-test-a-function.md)  
  Описание различных средств и методов тестирования функций.
* [Масштабирование функций Azure](functions-scale.md)  
  Обсуждение планов обслуживания, доступных для использования с функциями Azure (включая план потребления), а также выбор подходящего плана. 
* [Дополнительные сведения о службе приложений Azure](../app-service/app-service-web-overview.md)  
  Служба "Функции Azure" использует службу приложений Azure для таких базовых операций, как развертывание, диагностика и использование переменных среды. 

