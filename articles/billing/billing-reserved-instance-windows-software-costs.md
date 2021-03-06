---
title: Затраты на программное обеспечение Windows при резервировании в Azure | Документация Майкрософт
description: Узнайте, какие счетчики программного обеспечения Windows не учитываются в затратах на зарезервированные экземпляры виртуальных машин Azure.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: 8ff2d126fd491ddc40f6223b6d4ec1fc4505b8fb
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630117"
---
# <a name="windows-software-costs-not-included-with-azure-reservations"></a>Затраты на программное обеспечение Windows, которые не включены в стоимость резервирования в Azure

Если не применяется преимущество гибридного использования Azure для зарезервированных экземпляров виртуальных машин, плата будет взиматься за единицы измерения программного обеспечения Windows (см. раздел ниже).

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Единицы измерения программного обеспечения Windows, не учитываемые в затратах на резервирование

| Значение MeterId | Значение MeterName в файле использования | Какой виртуальной машиной используется |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Зарезервированные экземпляры с Windows Server (1 ядро) — пакетная передача | Серия B |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Зарезервированные экземпляры с Windows Server (2 ядра) — пакетная передача | Серия B |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Зарезервированные экземпляры с Windows Server (4 ядра) — пакетная передача | Серия B |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Зарезервированные экземпляры с Windows Server (8 ядер) — пакетная передача | Серия B |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Зарезервированные экземпляры с Windows Server (1 ядро) | Все, кроме серии B |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Зарезервированные экземпляры с Windows Server (2 ядра) | Все, кроме серии B |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Зарезервированные экземпляры с Windows Server (4 ядра) | Все, кроме серии B |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Зарезервированные экземпляры с Windows Server (6 ядер) | Все, кроме серии B |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Зарезервированные экземпляры с Windows Server (8 ядер) | Все, кроме серии B |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Зарезервированные экземпляры с Windows Server (12 ядер) | Все, кроме серии B |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Зарезервированные экземпляры с Windows Server (16 ядер) | Все, кроме серии B |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Зарезервированные экземпляры с Windows Server (20 ядер) | Все, кроме серии B |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Зарезервированные экземпляры с Windows Server (24 ядра) | Все, кроме серии B |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Зарезервированные экземпляры с Windows Server (32 ядра) | Все, кроме серии B |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Зарезервированные экземпляры с Windows Server (40 ядер) | Все, кроме серии B |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Зарезервированные экземпляры с Windows Server (64 ядра) | Все, кроме серии B |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Зарезервированные экземпляры с Windows Server (72 ядра) | Все, кроме серии B |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Зарезервированные экземпляры с Windows Server (128 ядер) | Все, кроме серии B |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Зарезервированные экземпляры с Windows Server (256 ядер) | Все, кроме серии B |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Зарезервированные экземпляры с Windows Server (96 ядер) | Все, кроме серии B |

Стоимость каждой из этих единиц измерения можно узнать с помощью API Azure RateCard. Сведения о том, как узнать тарифы на единицы измерения в Azure, см. в статье [Get price and metadata information for resources used in an Azure subscription](https://msdn.microsoft.com/library/azure/mt219004) (Получение сведений о ценах и метаданных для ресурсов, используемых в подписке Azure).

## <a name="next-steps"></a>Дополнительная информация
Дополнительные сведения о резервировании в Azure см. в следующих статьях.

- [Основные сведения о резервировании в Azure](billing-save-compute-costs-reservations.md)
- [Предоплата виртуальных машин с помощью Azure Reserved Virtual Machine Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Управление зарезервированными экземплярами в Azure](billing-manage-reserved-vm-instance.md)
- [Сведения о применении скидки на зарезервированный экземпляр виртуальной машины](billing-understand-vm-reservation-charges.md)
- [Использование зарезервированного экземпляра виртуальной машины для подписки с оплатой по мере использования](billing-understand-reserved-instance-usage.md)
- [Общие сведения об использовании зарезервированных экземпляров Azure с Соглашением о регистрации Enterprise](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Требуется помощь? Обратитесь в службу поддержки.

Если у вас есть дополнительные вопросы, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.



