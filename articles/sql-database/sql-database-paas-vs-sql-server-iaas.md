---
title: Сравнение базы данных SQL (PaaS) с SQL Server в облаке на виртуальных машинах (IaaS) | Документация Майкрософт
description: 'Узнайте, какой вариант SQL Server подходит для вашего приложения: база данных SQL Azure (PaaS) или SQL Server в облаке на виртуальных машинах Azure.'
services: sql-database, virtual-machines
keywords: облако SQL Server, SQL Server в облаке, база данных SaaS, облачный SQL Server, DBaaS
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: a6d6a7639d3db0cc7d194ca9fae126ad9a2cc3ba
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413660"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Вы можете выбрать компонент SQL Server в облаке: база данных SQL Azure (PaaS) или SQL Server на виртуальных машинах Azure (IaaS)

В Azure вы можете запускать рабочие нагрузки SQL Server в размещенной инфраструктуре (IaaS) или в качестве размещенных служб ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

- [База данных SQL Azure](https://azure.microsoft.com/services/sql-database/). Ядро базы данных SQL на основе SQL Server Enterprise Edition, оптимизированное для разработки современных приложений. База данных SQL Azure предлагает несколько вариантов развертывания:
  - Можно развернуть одну базу данных на [логическом сервере](sql-database-logical-servers.md).
  - Можно выполнить развертывание в [эластичный пул](sql-database-elastic-pool.md) на [логическом сервере](sql-database-logical-servers.md) для совместного использования ресурсов и снижения затрат. 

      > [!NOTE]
      > База данных SQL Azure с отдельными базами данных и базами данных в пуле обеспечивает большую часть возможностей уровня базы данных в SQL Server.

      Эти варианты развертывания показаны на рисунке ниже:

      ![Варианты развертывания](./media/sql-database-technical-overview/deployment-options.png) 
  - Вы также можете развернуть [Управляемые экземпляры Базы данных SQL (предварительная версия)](sql-database-managed-instance.md). 

      > [!NOTE]
      > В обоих версиях база данных SQL Azure предоставляет дополнительные функции, недоступные в SQL Server, например встроенная аналитика и управление. При использовании первой версии База данных SQL Azure предлагает совместные ресурсы для баз данных и дополнительные возможности уровня экземпляра. Управляемый экземпляр базы данных SQL Azure поддерживает миграцию базы данных с минимальными изменениями базы данных или вовсе без них.
- [SQL Server на виртуальных машинах Azure.](https://azure.microsoft.com/services/virtual-machines/sql-server/) Сервер SQL Server, который установлен и работает на виртуальных машинах Windows Server или Linux в облаке Azure, предоставляется по модели "инфраструктура как услуга" (IaaS). SQL Server на виртуальных машинах Azure — это хороший вариант для миграции локальных баз данных и приложений SQL Server без изменения базы данных. Все последние версии и выпуски SQL Server доступны для установки на виртуальной машине IaaS. Наиболее важное отличие от базы данных SQL заключается в том, что виртуальные машины SQL Server предоставляют полный контроль над ядром базы данных. Вы можете выбрать время запуска обслуживания и исправления, изменить модель восстановления на простую или с неполным протоколированием для ускорения загрузки, приостановить или запустить ядро при необходимости и настраивать все параметры ядра базы данных SQL Server. Дополнительные возможности контроля означают дополнительные обязанности по управлению виртуальными машинами.

Мы расскажем вам, как эти варианты развертывания сочетаются с платформой Microsoft Data, и поможем правильно выбрать решение, соответствующее потребностям вашей компании. Независимо от ваших приоритетов, будь то сокращение затрат или минимальное администрирование, эта статья поможет определить, какой подход отвечает самым важным требованиям вашей компании.

## <a name="microsofts-sql-data-platform"></a>Платформа данных Microsoft SQL

Одна из первых вещей, которые нужно определить до обсуждения, — можете ли вы использовать базы данных Azure и локальные базы данных SQL Server совместно. Платформа Microsoft Data использует технологию SQL Server, поэтому она доступна для физических локальных компьютеров, частных облачных сред (в том числе для размещенных облачных сред сторонних поставщиков) и для общедоступного облака. SQL Server на виртуальных машинах Azure позволяет использовать стандартный набор серверных продуктов, средств разработки и опыт работы в разных средах для создания комбинаций локальных и облачных развертываний, которые удовлетворяют самые разные бизнес-потребности.

   ![Варианты облачной БД SQL Server: SQL Server (IaaS) или база данных SQL в облаке (SaaS).](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Как видно на схеме, каждый предложенный вариант можно охарактеризовать по уровню администрирования его инфраструктуры (по оси X) и по степени эффективности затрат, которая достигается за счет консолидации на уровне базы данных и автоматики, (по оси Y).

При разработке приложения доступны четыре основных параметра размещения части SQL Server приложения:

* SQL Server на невиртуализированных физических компьютерах;
* SQL Server в локальных виртуальных машинах (частное облако);
* SQL Server в виртуальной машине Azure (общедоступное облако Microsoft);
* база данных SQL Azure (общедоступное облако Microsoft).

В следующих разделах мы рассмотрим SQL Server в общедоступном облаке Microsoft, т. е. базу данных SQL Azure и SQL Server на виртуальных машинах Azure. Кроме того, вы изучите основные факторы, влияющие на выбор оптимального варианта для вашего приложения.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Более детальный обзор базы данных SQL Azure и SQL Server на виртуальных машинах Azure

**База данных SQL Azure** — это реляционная база данных, которая предоставляется как услуга (DBaaS). Она размещается в облаке Azure и относится к отраслевым категориям *платформа как услуга (PaaS)*. [База данных SQL](sql-database-technical-overview.md) создана на основе стандартного аппаратного и программного обеспечения, которое принадлежит корпорации Майкрософт, а также размещается и обслуживается ею. С базой данных SQL можно использовать встроенные функции и возможности, которые требуют сложной настройки в SQL Server. При использовании базы данных SQL вы платите по мере использования и имеете возможность увеличивать масштаб базы данных или развертывать ее для повышения производительности без прерывания работы. База данных SQL Azure — это мощная и гибкая среда для разработки приложений в облаке А благодаря [управляемым экземплярам базы данных SLQ Azure](sql-database-managed-instance.md) вы можете использовать собственную лицензию. Кроме того, этот вариант предоставляет все преимущества PaaS базы данных SQL Azure, но добавляет возможности, ранее доступные только на виртуальных машинах SQL. Сюда входит собственная виртуальная сеть (VNet) и почти полная совместимость с SQL Server в локальной среде. [Управляемый экземпляр](sql-database-managed-instance.md) идеально подходит для переноса локальных баз данных в Azure с минимальными изменениями. 

**SQL Server на виртуальных машинах Azure** относится к отраслевой категории *инфраструктура как услуга (IaaS)* и позволяет запускать SQL Server на виртуальной машине в облаке. [Виртуальные машины SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) также работают на стандартном аппаратном обеспечении, которое принадлежит корпорации Майкрософт, размещается и обслуживается ею. Для SQL Server на виртуальной машине вы можете использовать включенную в образ SQL Server лицензию с оплатой по мере использования или уже имеющуюся у вас лицензию. Вы можете останавливать или возобновлять работу виртуальной машины при необходимости.

Как правило, эти два варианта использования SQL оптимизированы для разных целей:

* **База данных SQL Azure** оптимизирована таким образом, чтобы уменьшить общие затраты на подготовку и администрирование нескольких баз данных. Она снижает затраты на текущее администрирование, поскольку вам не требуется управлять виртуальными машинами, операционными системами или программным обеспечением баз данных. Вам не придется управлять обновлениями, высокой доступностью и [резервными копиями](sql-database-automated-backups.md). В целом база данных SQL Azure позволяет значительно увеличить количество баз данных, которыми может управлять один сотрудник отдела ИТ или разработки. [Эластичные пулы](sql-database-elastic-pool.md) также поддерживают мультитенантные архитектуры для приложений SaaS, включая изоляцию клиентов и возможность масштабирования для сокращения затрат благодаря совместному использованию ресурсов в базах данных. [Управляемый экземпляр базы данных SQL Azure](sql-database-managed-instance.md) поддерживает возможности на уровне экземпляров для простой миграции существующих приложений, а также для совместного использования ресурсов в базах данных.
* **Сервер SQL Server на виртуальных машинах Azure** оптимизирован для переноса существующих приложений в Azure или расширения существующих локальных приложений в облако в рамках гибридного развертывания. Кроме того, SQL Server на виртуальной машине можно применять для разработки и тестирования традиционных приложений SQL Server. Если SQL Server выполняется на виртуальных машинах Azure, вы обладаете всеми правами администратора в выделенном экземпляре SQL Server и облачной виртуальной машине. Это идеальный выбор, если в организации имеются ИТ-ресурсы для обслуживания виртуальных машин. Все это позволяет персонализировать систему с учетом требований конкретного приложения к производительности и доступности.

В следующей таблице представлены основные характеристики базы данных SQL и SQL Server на виртуальных машинах Azure.

| | Базы данных SQL Azure<br>Логические серверы, эластичные пулы и отдельные базы данных | Базы данных SQL Azure<br>Управляемый экземпляр |Виртуальная машина Azure<br>SQL Server; |
| --- | --- | --- |---|
| **Области предназначения** |Новые облачные приложения, для которых необходимо использовать последние стабильные возможности SQL Server, когда разработка и маркетинг ограничены во времени. | Новые приложения или существующие локальные приложения, которые используют последние стабильные возможности SQL Server и переносятся в облако с минимальными изменениями.  | Существующие приложения, которым требуется быстрая миграция в облако с минимальными изменениями или без них. Сценарии быстрой разработки и тестирования без покупки оборудования для дополнительного локального сервера SQL Server. |
|  | Разработчики баз данных, для которых важны высокая доступность, аварийное восстановление и обновление. | Как и SQL Database. | Команды, способные настроить и поддерживать высокую доступность, аварийное восстановление и установку исправлений для SQL Server. Предлагаемые функции автоматизации позволяют значительно упростить эти процессы. | |
|  | Разработчики, которые не хотят тратить силы на настройку конфигурации и базовой операционной системы. | Как и SQL Database. | Вам потребуется настраиваемая среда со всеми правами администратора. | |
|  | Базы данных размером до 4 ТБ или более крупные базы данных, которые можно [секционировать горизонтально или вертикально](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), используя шаблон сегментирования. | Как и SQL Database. | Экземпляры SQL Server с хранилищем объемом до 64 ТБ. Экземпляр может поддерживать любое необходимое количество баз данных. |
| **Совместимость** | Поддерживает большинство возможностей уровня базы данных в локальной среде. | Поддерживает почти все возможности уровня экземпляра и уровня базы данных в локальной среде. | Поддерживает все возможности в локальной среде. |
| **Ресурсы:** | Вы не хотите нанимать ИТ-специалистов для настройки и обслуживания базовой инфраструктуры, а предпочитаете сосредоточиться на уровне приложений. | Как и SQL Database. | У вас есть ИТ-специалисты для настройки и управления. Предлагаемые функции автоматизации позволяют значительно упростить эти процессы. |
| **Совокупная стоимость владения** | Устраняет расходы на оборудование и уменьшает затраты на администрирование. | Как и SQL Database. | Сводит затраты на оборудование к нулю. |
| **Непрерывность бизнес-процессов** |Кроме [встроенных возможностей отказоустойчивости инфраструктуры](sql-database-high-availability.md), база данных SQL Azure предоставляет такие функции для улучшения непрерывности бизнес-процессов, как [автоматическая архивация](sql-database-automated-backups.md), [восстановление до точки во времени](sql-database-recovery-using-backups.md#point-in-time-restore), [геовосстановление](sql-database-recovery-using-backups.md#geo-restore) и [группа отработки отказа и активная георепликация](sql-database-geo-replication-overview.md). Дополнительные сведения см. в [обзоре непрерывности бизнес-процессов с помощью баз данных SQL](sql-database-business-continuity.md). | Как база данных SQL, а также инициируемые пользователем резервные копии только для копирования. | SQL Server на виртуальных машинах Azure позволяет создать решение с высоким уровнем доступности и возможностью аварийного восстановления с учетом потребностей конкретной базы данных. Таким образом, у вас может быть система, сильно оптимизированная для приложения. При необходимости вы можете выполнить тестирование и отработку отказа самостоятельно. Дополнительные сведения см. в статье [Высокий уровень доступности и аварийное восстановление для SQL Server на виртуальных машинах Azure](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Гибридное облако** |Локальное приложение получает доступ к данным в базе данных SQL Azure. | [Собственная реализованная виртуальная сеть](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-vnet-configuration) и подключение к локальной среде с помощью Azure Express Route или VPN-шлюза. | SQL Server на виртуальных машинах Azure позволяет создавать приложения, которые частично работают в облаке и частично — на локальных ресурсах. Например, вы можете расширить локальную сеть и домен Active Directory в облако через [виртуальную сеть Azure](../virtual-network/virtual-networks-overview.md). Кроме того, вы можете хранить локальные файлы данных в службе хранилища Azure с помощью [файлов данных SQL Server в Azure](http://msdn.microsoft.com/library/dn385720.aspx). Дополнительные сведения см. в статье [Приступая к работе с SQL Server в виртуальных машинах Azure](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Поддерживает [репликацию транзакций SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) в качестве подписчика для репликации данных. | Репликация не поддерживается в управляемом экземпляре базы данных SQL Azure. | Полностью поддерживает [репликацию баз данных SQL](https://msdn.microsoft.com/library/mt589530.aspx), [группы доступности AlwaysOn](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), службы интеграции и доставку журналов для репликации данных. Кроме того, полностью поддерживает традиционное резервное копирование SQL Server. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Коммерческие критерии выбора между базой данных SQL Azure и SQL Server на виртуальных машинах Azure
### <a name="cost"></a>Стоимость
Определяющий фактор при выборе способа размещения базы данных — ограниченность в средствах, как для начинающих разработчиков, стесненных в деньгах, так и для групп в солидных организациях с ограниченным бюджетом. Из этого раздела вы узнаете основные сведения о выставлении счетов и лицензировании в Azure для двух вариантов размещения реляционных баз данных: базы данных SQL и SQL Server на виртуальных машинах Azure. Также вы ознакомитесь с расчетом общей стоимости приложений.

#### <a name="billing-and-licensing-basics"></a>Основы лицензирования и выставления счетов

Сейчас **База данных SQL** продается как услуга и имеет несколько уровней с разными ценами за ресурсы. Счета выставляются за каждый час использования по фиксированной ставке, которая зависит от выбранного уровня службы и уровня производительности. Благодаря управляемым экземплярам базы данных SLQ Azure вы можете использовать собственную лицензию. Дополнительные сведения об использовании собственной лицензии см. в статье [Перемещение лицензий в рамках программы Software Assurance в Azure](https://azure.microsoft.com/pricing/license-mobility/). Кроме того, счета выставляются за исходящий интернет-трафик по обычным [тарифам на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/). Вы можете менять уровни служб и уровни производительности в динамическом режиме в соответствии с различными требованиями приложения к пропускной способности. Последние сведения о текущих поддерживаемых уровнях служб см. в разделах [Модель приобретения на основе DTU](sql-database-service-tiers-dtu.md) и [Модель приобретения на основе виртуальных ядер](sql-database-service-tiers-vcore.md). Вы можете также создавать [эластичные пулы](sql-database-elastic-pool.md) для совместного использования ресурсов в базах данных, чтобы сократить расходы и справляться с пиками нагрузок.

При использовании **базы данных SQL**корпорация Майкрософт автоматически настраивает, исправляет и обновляет программное обеспечение базы данных, что позволит вам сократить затраты на администрирование. Кроме того, [встроенные функции резервного копирования](sql-database-automated-backups.md) помогают значительно экономить средства, особенно при наличии большого количества баз данных. 

При размещении **SQL Server на виртуальных машинах Azure** вы можете использовать любой образ SQL Server, предоставляемый платформой (все они содержат лицензию), или собственную лицензию SQL Server. Доступны все поддерживаемые версии (2008 R2, 2012, 2014, 2016) и выпуски (Developer, Express, Web, Standard, Enterprise) SQL Server. Кроме того, доступны версии образов с использованием собственной лицензии (BYOL). Стоимость использования образов, которые предоставляет Azure, зависит от размера виртуальной машины, а также от выбранного выпуска SQL Server. Независимо от размера виртуальной машины или выпуска SQL Server вы оплачиваете лицензии SQL Server и Windows или Linux Server, а также стоимость службы хранилища Azure для дисков виртуальной машины по поминутному тарифу. Поминутное выставление счетов позволяет использовать SQL Server ровно столько, сколько нужно, без приобретения дополнительных лицензий SQL Server. Если вы передаете в Azure собственную лицензию SQL Server, плата взимается только за сервер и хранение данных. Дополнительные сведения об использовании собственной лицензии см. в статье [Перемещение лицензий в рамках программы Software Assurance в Azure](https://azure.microsoft.com/pricing/license-mobility/). Кроме того, счета выставляются за исходящий интернет-трафик по обычным [тарифам на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/).

#### <a name="calculating-the-total-application-cost"></a>Расчет общей стоимости приложений
Когда вы начинаете использовать облачную платформу, стоимость выполнения приложения включает затраты на разработку и администрирование, а также затраты на службу платформы в общедоступном облаке.

**Для базы данных SQL Azure:**

- Сокращение административных расходов
- Низкие затраты на разработку для перенесенных приложений
- Расходы на службу базы данных SQL
- Нет расходов на покупку оборудования

**Для SQL Server на виртуальных машинах Azure:**

- Высокая стоимость администрирования
- Низкие затраты на разработку для перенесенных приложений или отсутствие затрат
- Расходы на обслуживание виртуальных машин
- Стоимость лицензии SQL Server
- Нет расходов на покупку оборудования

Дополнительную информацию см. в следующих ресурсах:

* [Цены на базы данных SQL](https://azure.microsoft.com/pricing/details/sql-database/)
* [Цены на виртуальные машины](https://azure.microsoft.com/pricing/details/virtual-machines/) для [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) и [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
* [Калькулятор стоимости — оцените свои расходы](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Администрирование
Для многих компаний решение о переходе в облачную службу позволит уменьшить не только расходы, но и сложность администрирования системы. При использовании IaaS и PaaS корпорация Майкрософт управляет инфраструктурой и автоматически реплицирует все данные для аварийного восстановления, настраивает и обновляет программное обеспечение базы данных, управляет балансировкой нагрузки и выполняет прозрачную отработку отказа при сбое сервера в центре обработки данных. 

- С **базой данных SQL Azure** вы можете продолжать администрировать базу данных, но вам больше не нужно беспокоиться об управлении ядром СУБД, операционной системой или оборудованием сервера.  Под вашим управлением остаются базы данных и учетные записи, индексы и оптимизация запросов, аудит и безопасность. Кроме того, при настройке высокой доступности для другого центра обработки данных требуется минимальная конфигурация и администрирование.
- Решение **SQL Server на виртуальных машинах Azure** позволит полностью контролировать операционную систему и конфигурацию экземпляра SQL Server. Для виртуальной машины вы сами решаете, когда обновлять операционную систему и программное обеспечение базы данных, а также выбираете дополнительные программы (например, антивирусные) и время их установки. Некоторые предлагаемые функции автоматизации позволяют значительно упростить процессы исправления, резервного копирования и обеспечения высокой доступности. Кроме того, вы можете контролировать размер виртуальной машины, количество дисков и их конфигурации хранения. Azure позволяет изменять размер виртуальной машины по мере необходимости. Дополнительные сведения см. в статье [Размеры виртуальных машин в Azure](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Соглашение об уровне обслуживания (SLA):
Для многих ИТ-отделов основным приоритетом является соблюдение обязательств по времени непрерывной работы, определенных в соглашении об уровне обслуживания (SLA). В этом разделе мы рассмотрим условия SLA для каждого варианта размещения базы данных.

Для **базы данных SQL** корпорация Майкрософт предоставляет соглашения об уровне обслуживания с доступностью 99,99 %. Последние сведения см. на странице [Соглашение об уровне обслуживания для базы данных SQL](https://azure.microsoft.com/support/legal/sla/sql-database/). 

Для **SQL Server на виртуальных машинах Azure** мы гарантируем уровень доступности 99,95 %, но только в отношении виртуальной машины. Это соглашение об уровне обслуживания не распространяется на процессы (например, SQL Server), запущенные на виртуальной машине, и предусматривает наличие по крайней мере двух экземпляров виртуальных машин в каждой группе доступности. Последние сведения см. на странице [Соглашение об уровне обслуживания для виртуальных машин](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Чтобы обеспечить высокую доступность базы данных на виртуальных машинах, следует настроить один из поддерживаемых вариантов высокого уровня доступности на SQL Server, например [группы доступности AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Использование параметра высокой доступности не дает дополнительных гарантий уровня обслуживания, но позволяет достичь доступности базы данных на уровне более 99,99 %.

### <a name="market"></a>Пора переходить на Azure
**Логические серверы, эластичные пулы и отдельные базы данных в базе данных SQL Azure** идеально подойдут для приложений, разработанных для облачной среды, поскольку для них критически важными являются эффективность разработки и сокращение времени выхода на рынок новых решений. Благодаря функциональным возможностям, схожим с возможностями администратора базы данных, она идеально подходит для разработчиков облачных служб, так как позволяет уменьшить необходимость управления базовой операционной системой и базой данных. 

**Управляемый экземпляр базы данных SQL** значительно упрощает перенос существующих приложений в базу данных SQL Azure, позволяя быстро выводить перенесенные приложения базы данных на рынок в Azure.

**SQL Server на виртуальных машинах Azure** идеально подходит для ситуаций, когда существующим или новым приложениям требуются большие базы данных или доступ ко всем функциям SQL Server или Windows либо Linux, а также когда вы хотите сэкономить время и деньги на покупку нового локального оборудования. Кроме того, это прекрасный вариант для переноса в Azure существующих локальных приложений и баз данных в неизменном виде, когда управляемый экземпляр базы данных SQL Azure не подходит. Так как изменять уровень презентации, приложения и данных не нужно, вы экономите время и средства при повторном изменении имеющегося решения. Вместо этого вы можете уделить внимание переносу всех решений в Azure и оптимизации производительности, необходимой для платформы Azure. Дополнительные сведения см. в статье [Рекомендации по оптимизации производительности SQL Server в виртуальных машинах Azure](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Сводка
В этой статье приведен обзор баз данных SQL и SQL Server на виртуальных машинах Azure, а также рассматриваются коммерческие факторы, которые могут повлиять на ваше решение. Ниже приводится краткий обзор рекомендаций.

Выберите **базу данных SQL Azure** , если:

* Вы создаете новые облачные приложения, чтобы воспользоваться возможностями облачных служб для экономии и оптимизации производительности. Такой подход предоставит преимущества полностью управляемой облачной службы, сократит время выхода на рынок и поможет оптимизировать затраты в долгосрочной перспективе.
* Вы хотите, чтобы корпорация Майкрософт выполняла стандартные операции по управлению в вашей базе данных, и вам требуется более высокий уровень доступности согласно соглашению об уровне обслуживания для баз данных.
* Вы хотите перенести существующее приложение в управляемый экземпляр базы данных SQL Azure в неизменном виде и воспользоваться преимуществами равенства функций с SQL Server или повышенной безопасности и сети. Управляемый экземпляр хорошо подходит как для новых, так и для существующих приложений.

Выберите **SQL Server на виртуальных машинах Azure** , если:

* У вас есть существующие локальные приложения, которые вы хотите перенести или расширить в облако, или вы планируете создавать корпоративные приложения объемом более 4 ТБ. Такой подход обеспечивает возможность выбора версии и выпуска SQL Server, большую емкость для хранения баз данных, полный контроль над SQL Server и Windows/Linux, а также безопасное туннелирование в локальную сеть. Это позволить минимизировать затраты на разработку и модификацию существующих приложений.
* У вас уже есть ИТ-специалисты, способные самостоятельно управлять процессами исправлений, резервного копирования и обеспечения доступности базы данных. Обратите внимание, что некоторые функции автоматизации позволяют значительно упростить эти операции. 

## <a name="next-steps"></a>Дополнительная информация

* Чтобы начать работу с Базой данных SQL, см. статью [Краткое руководство. Начало работы с базой данных SQL Azure](sql-database-get-started-portal.md).
* См. страницу с [ценами на базы данных SQL](https://azure.microsoft.com/pricing/details/sql-database/).
* Чтобы приступить к работе с SQL Server в виртуальной машине Azure, см. статью [Подготовка виртуальной машины SQL Server на портале Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).