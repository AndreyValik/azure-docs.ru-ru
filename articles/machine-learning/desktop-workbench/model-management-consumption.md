---
title: Использование веб-службы Управления моделями Машинного обучения Azure | Документация Майкрософт
description: В этом документе описываются действия и основные понятия, связанные с использованием веб-служб, развернутых в службе управления моделями в Машинном обучении Azure.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: 4a49ccff68003cf7b81a7d945176992a2893d1ac
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38973176"
---
# <a name="consuming-web-services"></a>Использование веб-служб
После развертывания модели в качестве веб-службы в реальном времени вы можете отправлять ее данные и получать прогнозы из различных платформ и приложений. Веб-служба в реальном времени предоставляет REST API для получения прогнозов. Можно отправлять данные в веб-службу в однострочном или многострочном формате, чтобы получить один или несколько прогнозов за один раз.

С помощью [веб-службы Машинного обучения Azure](model-management-service-deploy.md) внешнее приложение синхронно обменивается данными с прогнозной моделью с помощью вызова HTTP POST к URL-адресу службы. Чтобы вызвать веб-службу, клиентскому приложению необходимо указать ключ API, который создается при развертывании прогноза, и поместить данные запроса в тело запроса POST.

> [!NOTE]
> Обратите внимание, что ключи API доступны только в режиме развертывания кластера. Локальные веб-службы не имеют ключей.

## <a name="service-deployment-options"></a>Варианты развертывания службы
Веб-службы машинного обучения Azure можно развернуть в облачных кластерах как для рабочих, так и для тестовых сценариев, а также для локальных рабочих станций, используя модуль Docker. Функциональные возможности прогнозной модели в обоих случаях остаются неизменными. Развертывание в кластере обеспечивает масштабируемое и производительное решение на основе Службы контейнеров Azure, в то время как локальное развертывание можно использовать для отладки. 

Интерфейс командной строки и API Машинного обучения Azure предоставляют удобные команды для создания вычислительных сред и управления ими для развертывания служб с использованием параметра ```az ml env```. 

## <a name="list-deployed-services-and-images"></a>Получение списка развернутых служб и образов
Вы можете получить список текущих развернутых служб и образов Docker с помощью команды интерфейса командной строки ```az ml service list realtime -o table```. Обратите внимание, что эта команда всегда работает в контексте текущей вычислительной среды. При этом службы, развернутые в среде, которая не настроена как текущая, не отображаются. Чтобы настроить среду, используйте ```az ml env set```. 

## <a name="get-service-information"></a>Получение сведений о службе
После успешного развертывания веб-службы используйте следующую команду, чтобы получить URL-адрес службы и другие данные для вызова конечной точки службы. 

```
az ml service usage realtime -i <web service id>
```

Эта команда выводит URL-адрес службы, требуемые заголовки запросов, URL-адрес Swagger и примеры данных для вызова службы, если схема API службы предоставлена ​​во время развертывания.

Вы можете протестировать службу непосредственно из интерфейса командной строки без составления HTTP-запроса. Для этого введите команду интерфейса командной строки с входными данными:

```
az ml service run realtime -i <web service id> -d "Your input data"
```

## <a name="get-the-service-api-key"></a>Получение ключа API службы
Чтобы получить ключ веб-службы, используйте следующую команду:

```
az ml service keys realtime -i <web service id>
```
При создании HTTP-запроса используйте ключ в заголовке авторизации: "Authorization": "Bearer <key>"

## <a name="get-the-service-swagger-description"></a>Получение описания службы Swagger
Если предоставлена ​​схема API службы, конечная точка службы предоставит документ Swagger в ```http://<ip>/api/v1/service/<service name>/swagger.json```. Документ Swagger можно использовать для автоматического создания клиента службы и анализа ожидаемых входных данных и других сведений о службе.

## <a name="get-service-logs"></a>Получение журналов службы
Чтобы понять поведение службы и диагностировать проблемы, есть несколько способов получить журналы службы:
- Команда интерфейса командной строки ```az ml service logs realtime -i <service id>```. Эта команда работает как в кластерном, так и в локальном режимах.
- Если во время развертывания было включено ведение журнала службы, журналы службы также будут отправлены в AppInsight. Команда интерфейса командной строки ```az ml service usage realtime -i <service id>``` отображает URL-адрес AppInsight. Обратите внимание, что журналы AppInsight могут задерживаться на 2–5 минут.
- Журналы кластеров можно просмотреть в консоли Kubernetes, которая подключается, когда вы устанавливаете текущую среду кластера с помощью ```az ml env set```.
- Локальные журналы Docker доступны в журналах модуля Docker, когда служба работает локально.

## <a name="call-the-service-using-c"></a>Вызов службы с помощью C#
Используйте URL-адрес службы для отправки запроса из консольного приложения C#. 

1. Создайте консольное приложение в Visual Studio. 
    * В меню щелкните "Файл -> Создать -> Проект".
    * В Visual Studio C# щелкните Windows Class Desktop (Рабочий стол класса Windows), а затем — Console App (Консольное приложение).
2. Введите `MyFirstService` как имя проекта, а затем нажмите кнопку "ОК".
3. В ссылках проекта установите для ссылок значения `System.Net` и `System.Net.Http`.
4. Щелкните "Инструменты -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов", а затем установите пакет **Microsoft.AspNet.WebApi.Client**.
5. Откройте файл **Program.cs** и замените существующий код представленным ниже:
6. Дополните параметры `SERVICE_URL` и `API_KEY` сведениями из веб-службы.
7. Запустите проект.

```csharp
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MyFirstService
{
    class Program
    {
        // Web Service URL (update it with your service url)
        private const string SERVICE_URL = "http://<service ip address>:80/api/v1/service/<service name>/score";
        private const string API_KEY = "your service key";

        static void Main(string[] args)
        {
            Program.PostRequest();
            Console.ReadLine();
        }

        private static void PostRequest()
        {
            HttpClient client = new HttpClient();
            client.BaseAddress = new Uri(SERVICE_URL);
            //For local web service, comment out this line.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", API_KEY);

            var inputJson = new List<RequestPayload>();
            RequestPayload payload = new RequestPayload();
            List<InputDf> inputDf = new List<InputDf>();
            inputDf.Add(new InputDf()
            {
                feature1 = value1,
                feature2 = value2,
            });
            payload.Input_df_list = inputDf;
            inputJson.Add(payload);

            try
            {
                var request = new HttpRequestMessage(HttpMethod.Post, string.Empty);
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;

                Console.Write(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
        public class InputDf
        {
            public double feature1 { get; set; }
            public double feature2 { get; set; }
        }
        public class RequestPayload
        {
            [JsonProperty("input_df")]
            public List<InputDf> Input_df_list { get; set; }
        }
    }
}
```

## <a name="call-the-web-service-using-python"></a>Вызов веб-службы с помощью Python
Используйте Python для отправки запроса в веб-службу в режиме реального времени. 

1. Скопируйте следующий код в новый файл Python.
2. Обновите параметры data, url и api_key. Для локальных веб-служб удалите заголовок Authorization.
3. Выполните код. 

```python
import requests
import json

data = "{\"input_df\": [{\"feature1\": value1, \"feature2\": value2}]}"
body = str.encode(json.dumps(data))

url = 'http://<service ip address>:80/api/v1/service/<service name>/score'
api_key = 'your service key' 
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

resp = requests.post(url, body, headers=headers)
resp.text
```
