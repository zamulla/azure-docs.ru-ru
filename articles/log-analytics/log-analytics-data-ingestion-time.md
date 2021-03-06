---
title: Время приема данных в Azure Log Analytics | Документы Майкрософт
description: Описание различных факторов, которые влияют на задержку при сборе данных в Azure Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/10/2018
ms.author: bwren
ms.openlocfilehash: 0e513cc4f6a7d5d030ded807870de9eb0fdc0ed8
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38973186"
---
# <a name="data-ingestion-time-in-log-analytics"></a>Время приема данных в Log Analytics
Azure Log Analytics — это высокомасштабируемая служба, которая обслуживает тысячи клиентов, ежемесячно стремительными темпами отправляющих терабайты данных. Часто возникают вопросы о времени, в течение которого собранные данные становятся доступными в Log Analytics. В этой статье объясняются факторы, влияющие на эту задержку.

## <a name="typical-latency"></a>Обычная задержка
Задержка — это время, в течение которого данные создаются в отслеживаемой системе, и время, в течение которого они становятся доступными для анализа в Log Analytics. Обычная задержка для приема данных в Log Analytics составляет от 3 до 10 минут, с приемом 95 % данных менее чем за 7 минут. Конкретная задержка для определенных данных зависит от различных факторов, описанных ниже.

## <a name="sla-for-log-analytics"></a>Соглашение об уровне обслуживания для Log Analytics
[Соглашение об уровне обслуживания (SLA) для Log Analytics](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) представляет собой юридически обязательное соглашение, определяющее ситуации, в которых корпорация Майкрософт выплачивает клиентам компенсацию, если служба не соответствует целям. Оно не зависит от стандартной производительности системы, но, в худшем случае, учитывает потенциальные критические ситуации.

## <a name="factors-affecting-latency"></a>Факторы, влияющие на задержку
Общее время приема для конкретного набора данных можно разделить на следующие высокоуровневые составляющие. 

- Время агента — время на обнаружение события, его сбора и отправки в точку приема Log Analytics в виде записи журнала. В большинстве случаев этот процесс обрабатывается агентом.
- Время конвейера — время, необходимое конвейеру приема для обработки записи журнала. Сюда входит анализ свойств события и возможное добавление вычисляемых данных.
- Время индексирования — время, затраченное на прием записи журнала в хранилище больших данных Log Analytics.

Далее приводятся сведения о различных видах задержки, представленных в этом процессе.

### <a name="agent-collection-latency"></a>Задержка агента сбора данных
Агенты и решения по управлению используют разные стратегии для сбора данных с виртуальной машины, что может повлиять на задержку. Вот несколько конкретных примеров.

- Сбор событий Windows, событий системного журнала и метрик производительности выполняется немедленно. Счетчики производительности Linux опрашиваются с 30-секундным интервалом.
- Сбор данных журналов служб IIS и настраиваемых журналов происходит при изменении метки времени. Сбор данных журналов служб IIS зависит от [расписания переключения, настроенного в службах IIS](log-analytics-data-sources-iis-logs.md). 
- Решение репликации Active Directory выполняет свою оценку каждые пять дней, а решение оценки Active Directory оценивает инфраструктуру Active Directory еженедельно. Агент собирает данные этих журналов только после завершения оценки.

### <a name="agent-upload-frequency"></a>Частота отправки данных агентом
Подтверждая простоту своего функционала, агент Log Analytics помещает журналы в буфер и периодически отправляет их в Log Analytics. В зависимости от типа данных частота отправки меняется от 30 секунд до 2 минут. Большая часть данных отправляется меньше чем через минуту. Состояние сети может отрицательно повлиять на задержку при достижении этими данными точки приема Log Analytics.

### <a name="azure-logs-and-metrics"></a>Журналы и метрики Azure 
На обеспечение доступности данных журнала действий в Log Analytics требуется около 5 минут. В зависимости от службы Azure данные из журналов диагностики и метрики могут стать доступными через 1–5 минут. Отправка журналов в точку приема Log Analytics займет дополнительно 30–60 секунд, а отправка метрик — 3 минуты.

### <a name="management-solutions-collection"></a>Сбор данных решениями по управлению
Некоторые решения не собирают свои данные с агента и могут использовать метод сбора, приводящий к дополнительной задержке. Некоторые решения собирают данные через регулярные интервалы, не предпринимая попыток сбора данных в режиме, близком к реальному времени. Вот несколько конкретных примеров.

- Решение Office 365 опрашивает журналы действий с помощью API действий управления Office 365, который в настоящее время не дает гарантий задержки в режиме, близком к реальному времени.
- Решение Windows Analytics (например, для поддержки обновлений) собирает данные ежедневно.

Частоту сбора данных решением см. в документации к каждому решению.

### <a name="pipeline-process-time"></a>Время обработки конвейером
После добавления в конвейер Log Analytics записи журнала заносятся во временное хранилище, что позволяет обеспечить изоляцию клиента и сохранность данных. Как правило, этот процесс добавляет еще около 5–15 секунд задержки. В некоторых решениях по управлению реализованы более сложные алгоритмы по объединению данных и получению аналитических сведений по мере потокового поступления данных. Например, решение по мониторингу производительности сети собирает входящие данные через 3-минутные интервалы, добавляя тем самым 3-минутную задержку.

### <a name="new-custom-data-types-provisioning"></a>Подготовка новых типов пользовательских данных
При создании нового типа пользовательских данных из [настраиваемого журнала](../log-analytics/log-analytics-data-sources-custom-logs.md) или [API сборщика данных](../log-analytics/log-analytics-data-collector-api.md) система формирует выделенный контейнер хранилища. Это однократное увеличение нагрузки, которое возникает только при первом входе этого типа данных.

### <a name="surge-protection"></a>Защита от пиковых нагрузок
Наивысшим приоритетом Log Analytics является сохранность данных клиента, поэтому система оснащена встроенной защитой от резкого увеличения объемов данных. Сюда входят буферы, гарантирующие, что даже при огромный нагрузке система продолжит работать. При обычной нагрузке эти средства управления добавляют задержку меньше минуты, но в экстремальных условиях и при критических сбоях они могут добавлять значительное время, обеспечивая при этом безопасность данных.

### <a name="indexing-time"></a>Время индексации
На каждой платформе больших данных существует встроенный баланс между предоставлением возможностей анализа и расширенных функций поиска и немедленным предоставлением доступа к данным. Azure Log Analytics позволяет выполнять мощные запросы к миллионам записей и получить результаты через несколько секунд. Это стало возможным благодаря существенному преобразованию данных во время их приема и их хранению в уникальных компактных структурах. Система помещает данные в буфер до тех пор, пока их не будет достаточно для создания этих структур. Это должно быть сделано до появления записей журнала в результатах поиска.

Сейчас этот процесс занимает около 5 минут при небольшом объеме данных. При более высокой скорости передачи данных задача выполняется быстрее. Это кажется нелогичным, но данный процесс позволяет оптимизировать задержку для производственных рабочих нагрузок большого объема.



## <a name="checking-ingestion-time"></a>Проверка времени приема
Чтобы узнать приблизительное значение задержки данных с агентов, воспользуйтесь таблицей **пакета пульса**. Так как пакет пульса отправляется один раз в минуту, разница между текущим временем и последней записью пакета пульса в идеальном случае будет максимально близка к минуте.

Используйте следующий запрос, чтобы получить список компьютеров с наибольшим показателем задержки.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | top 50 by IngestionTime asc

 
Используйте следующий запрос в крупных средах, чтобы суммировать задержку для различных процентных долей от общего числа компьютеров.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | summarize percentiles(IngestionTime, 50,95,99)



## <a name="next-steps"></a>Дополнительная информация
* Ознакомьтесь с [соглашением об уровне обслуживания (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) для Log Analytics.

