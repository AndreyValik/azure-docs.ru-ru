---
title: Использование проб работоспособности Load Balancer | Документация Майкрософт
description: Сведения об использовании проб работоспособности для мониторинга экземпляров, находящихся за Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2018
ms.author: kumud
ms.openlocfilehash: 7366273e30132daf7dc5ea15072c574180d1bc8b
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397296"
---
# <a name="load-balancer-health-probes"></a>Пробы работоспособности Load Balancer

Azure Load Balancer использует пробы работоспособности, чтобы определить, какие экземпляры внутреннего пула должны получить новые потоки. Пробы работоспособности можно использовать для обнаружения сбоя приложения в экземпляре серверной части. Вы также можете создать настраиваемый ответ пробы работоспособности и использовать ее для управления потоком и подачи сигнала Load Balancer на отправку новых потоков или остановку передачи этих потоков в экземпляр серверной части, что можно использовать для управления нагрузкой или запланированным простоем.

Если проба работоспособности выдает сбой, Load Balancer прекращает отправлять новые потоки в соответствующий неработоспособный экземпляр. Поведение новых и имеющихся потоков зависит от того, передается ли поток по TCP- или UDP-протоколу, а также от используемого номера SKU Load Balancer.  Дополнительные сведения см. в разделе [Реакция на сбой проверки работоспособности](#probedown).

> [!IMPORTANT]
> Пробы работоспособности Load Balancer поступают с IP-адреса 168.63.129.16 и не должны блокироваться, чтобы они отметили экземпляр как работоспособный.  Дополнительные сведения см. в разделе [IP-адрес источника пробы](#probesource).

## <a name="health-probe-types"></a>Типы проб работоспособности

Пробы работоспособности могут наблюдать за любым портом во внутреннем экземпляре, включая порт, на котором предоставляется фактическое обслуживание. Пробы работоспособности поддерживают прослушиватели TCP или конечные точки HTTP. 

Для балансировки нагрузки UDP нужно создать настраиваемый сигнал пробы работоспособности для экземпляра серверной части, используя пробу работоспособности TCP или HTTP.

При использовании [правил балансировки нагрузки портов HA](load-balancer-ha-ports-overview.md) с [Load Balancer (цен. категория "Стандартный")](load-balancer-standard-overview.md) на всех портах выполняется балансировка нагрузки, а в одном ответе пробы работоспособности должно отражаться состояние всего экземпляра.  

Вы не должны использовать для пробы работоспособности преобразование сетевых адресов (NAT) или прокси-сервер через экземпляр, который получает пробу работоспособности к другому экземпляру в виртуальной сети, так как это может привести к каскадным сбоям в сценарии.

Если вы хотите протестировать сбой пробы работоспособности или отметить отдельный экземпляр, вы можете использовать группу безопасности для явного блокирования пробы работоспособности (назначение или [источник](#probesource)).

### <a name="tcp-probe"></a>проба TCP.

Проверки TCP инициализируют подключение, выполняя трехстороннее подтверждение по открытому TCP-протоколу на определенном порту.  Затем следует четырехстороннее подтверждение закрытого TCP-протокола.

Минимальный интервал проверки составляет 5 секунд, а минимальное количество ответов о неработоспособности — 2.  Общая продолжительность не может превышать 120 секунд.

Проба TCP завершается сбоем в следующих случаях:
* Прослушиватель TCP в экземпляре не отвечает в течение периода времени ожидания.  Проба отмечается как неработоспособная в зависимости от количества неудачных запросов на пробу, достаточного для того, чтобы проба была помечена как неработоспособная.
* Проба получает сброс TCP-соединения от экземпляра.

### <a name="http-probe"></a>проба HTTP

Пробы HTTP устанавливают подключение TCP и выдают запрос HTTP GET с указанным расположением. Эти пробы поддерживают относительные пути для HTTP GET. Проба работоспособности отмечается как работоспособная, когда экземпляр отвечает с HTTP-состоянием 200 в течение периода ожидания.  По умолчанию пробы работоспособности HTTP пытаются проверить настроенный порт пробы работоспособности каждые 15 секунд. Минимальный интервал проверки составляет 5 секунд. Общая продолжительность не может превышать 120 секунд. 


Пробы HTTP могут быть полезны, если нужно реализовать собственную логику для удаления экземпляров из смены подсистемы балансировки нагрузки. Например, можно удалить экземпляр, если он использует более 90 % ресурсов ЦП и возвращает состояние HTTP, отличное от кода 200. 

Если вы используете облачные службы и у вас есть веб-роли, использующие w3wp.exe, вам доступен также автоматический мониторинг веб-сайта. Ошибки в коде веб-сайта возвращают проверке подсистемы балансировки нагрузки состояние с кодом, отличным от 200.  Проба HTTP переопределяет проверку гостевого агента по умолчанию. 

Проба HTTP завершается сбоем в следующих случаях:
* Конечная точка пробы HTTP возвращает код ответа HTTP, отличный от 200 (например, 403, 404 или 500). В этом случае проба будет сразу же отмечена как неработоспособная. 
* Конечная точка пробы HTTP не отвечает в течение 31-секундного периода ожидания. В зависимости от установленного времени ожидания множество запросов на проверку могут остаться без ответа, перед тем как проверка отмечает экземпляр как неработоспособный (то есть перед отправкой количества проверок, равного SuccessFailCount).
* Конечная точка пробы HTTP закрывает подключение путем сброса TCP-соединения.

### <a name="guest-agent-probe-classic-only"></a>Проба гостевого агента (только для классической версии)

Роли облачной службы (рабочие роли и веб-роли) используют гостевой агент для мониторинга проб по умолчанию.   Эту функцию стоит использовать в последнюю очередь.  Вы должны всегда явно определять пробу работоспособности как пробу TCP или HTTP. Проба гостевого агента не так эффективна, как явно определенные пробы для большинства сценариев приложений.  

Проба гостевого агента представляет собой проверку гостевого агента внутри виртуальной машины. Затем она ожидает передачу данных и передает ответ HTTP с кодом 200 (ОК) только в том случае, если экземпляр находится в состоянии готовности. (Другие возможные состояния: занято, перезапуск или остановлено.)

Дополнительные сведения см. в статьях [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx) (Схема конфигурации службы Azure (CSCFG-файл)) и [Приступая к работе по созданию общедоступного балансировщика нагрузки для облачных служб](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Если гостевой агент не отправляет ответ HTTP с кодом 200 (ОК), подсистема балансировки нагрузки отмечает экземпляр как неработоспособный. Затем он прекращает отправлять потоки к этому экземпляру. Подсистема балансировки нагрузки продолжит проверять связь с этим экземпляром. 

Если гостевой агент отправляет ответ HTTP с кодом 200, подсистема балансировки нагрузки снова начинает отправлять потоки к этому экземпляру.

При использовании веб-роли код веб-сайта обычно выполняется в процессе w3wp.exe, который не отслеживается структурой Azure или гостевым агентом. Ошибки в w3wp.exe (например, ответы HTTP с кодом 500) не передаются в гостевой агент. Следовательно, подсистема балансировки нагрузки не убирает этот экземпляр из ротации.

## <a name="probe-health"></a>Работоспособность пробы

Пробы TCP и HTTP считаются допустимыми и отмечают экземпляр роли как работоспособный в следующих случаях:

* проба работоспособности успешна при первой загрузке виртуальной машины.
* Количество успешных проверок, необходимых для отметки экземпляра роли как работоспособного, определяется параметром SuccessFailCount (см. выше). Если экземпляр роли был удален, то для пометки экземпляра роли как неработоспособного количество последующих успешных проверок должно быть не меньше значения SuccessFailCount.

> [!NOTE]
> Если состояние работоспособности экземпляра роли постоянно меняется, подсистема балансировки нагрузки ожидает более длительное время перед переводом экземпляра роли обратно в работоспособное состояние. Это дополнительное время ожидания защищает пользователя и инфраструктуру и преднамеренно заложено в политику.

## <a name="probe-count-and-timeout"></a>Количество проб и время ожидания

Поведение проверки зависит от следующих параметров:

* количества успешных проб, достаточного для того, чтобы пометить экземпляр как работоспособный;
* количества неудачных проверок, достаточного для того, чтобы пометить экземпляр как неработоспособный.

Время ожидания и частота, заданные в SuccessFailCount, определяют, считается ли экземпляр запущенным или незапущенным. На портале Azure в качестве значения времени ожидания задается удвоенное значение частоты.

Правило балансировки нагрузки содержит одну пробу работоспособности, определяемую соответствующим серверным пулом.

## <a name="probedown"></a>Реакция на сбой проверки работоспособности

### <a name="tcp-connections"></a>TCP-подключения

Новые TCP-подключения будут успешными при установке к экземпляру серверной части, который является работоспособным и содержит гостевую ОС и приложение, способное принять новый поток.

При сбое пробы работоспособности для экземпляра серверной части установленные к нему TCP-подключения прерваны не будут.

При сбое в серверном пуле всех проб для всех экземпляров новые потоки в этот пул передаваться не будут. Load Balancer (цен. категория "Стандартный") позволит поддерживать установленные потоки TCP.  Load Balancer (цен. категория "Базовый") завершит передачу всех имеющихся потоков TCP в серверный пул.
 
Так как поток всегда находится между клиентом и гостевой ОС виртуальной машины, пул со всеми пробами, завершившимися сбоем, приведет к отсутствию ответа внешнего интерфейса на попытки открыть TCP-подключение из-за отсутствия в серверной части работоспособного экземпляра, необходимого для получения потока.

### <a name="udp-datagrams"></a>Датаграммы UDP

Датаграммы UDP будут доставлены работоспособным экземплярам серверной части.

UDP не требует соединения, поэтому состояние этого потока не отслеживается. При сбое пробы работоспособности экземпляра серверной части имеющиеся UDP-потоки могут переместиться в другой работоспособный экземпляр в серверном пуле.

Если в серверном пуле все пробы всех экземпляров завершатся сбоем, будут прекращены все имеющиеся потоки UDP для служб Load Balancer (цен. категория "Базовый" и "Стандартный").


## <a name="probesource"></a>IP-адрес источника пробы

Все пробы работоспособности Load Balancer поступают с IP-адреса 168.63.129.16. Он является их источником.  При добавлении собственного IP-адреса в виртуальную сеть Azure этот IP-адрес источника пробы работоспособности обязательно будет уникальным, так как он глобально зарезервирован для корпорации Майкрософт.  Этот адрес является одинаковым во всех регионах и не изменяется. Он не должен считаться угрозой для безопасности, так как источником пакета с него может быть только внутренняя платформа Azure. 

Чтобы проба Load Balancer пометила ваш экземпляр как работоспособный, **разрешите** этот IP-адрес во всех [группах безопасности](../virtual-network/security-overview.md) Azure и политиках локального брандмауэра.

Если в политиках брандмауэра этого не сделать, проба работоспособности завершится сбоем, так как будет неспособна получить доступ к экземпляру.  В свою очередь, из-за сбоя пробы Load Balancer пометит экземпляр как неработоспособный.  Это может привести к сбою службы с балансировкой нагрузки. 

Вы также не должны настраивать свою виртуальную сеть с диапазоном IP-адресов, принадлежащим корпорации Майкрософт, который содержит 168.63.129.16,  так как произойдет конфликт с IP-адресом пробы работоспособности.

Если на вашей виртуальной машине имеется несколько интерфейсов, нужно ответить на пробу в интерфейсе, куда поступил запрос.  Для этого может потребоваться уникальный источник, преобразующий этот адрес на виртуальной машине для каждого интерфейса.

## <a name="monitoring"></a>Мониторинг

[Load Balancer (цен. категория "Стандартный")](load-balancer-standard-overview.md) предоставляет состояние пробы работоспособности как многомерные показатели для каждого экземпляра через Azure Monitor.

Load Balancer (цен. категория "Базовый") предоставляет состояние пробы работоспособности для каждого серверного пула через Log Analytics.  Это доступно только для общедоступных Load Balancer (цен. категория "Базовый") и недоступно для таких же внутренних экземпляров Load Balancer.  Вы можете использовать [Log Analytics](load-balancer-monitor-log.md) для проверки состояния работоспособности проб общедоступных Load Balancer, а также проверки количества этих проб. Функция ведения журналов в Power BI или Azure Operational Insights позволяет получать статистические данные о работоспособности подсистемы балансировки нагрузки.


## <a name="limitations"></a>Ограничения

-  Проба работоспособности HTTP не поддерживает TLS (HTTPS).  Вместо этого для порта 443 используйте пробу TCP.

## <a name="next-steps"></a>Дополнительная информация

- [Краткое руководство. Создание общедоступной подсистемы балансировки нагрузки с помощью Azure PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [Load Balancer Probes - Get](https://docs.microsoft.com/en-us/rest/api/load-balancer/loadbalancerprobes/get) (Получение проб работоспособности Load Balancer)

