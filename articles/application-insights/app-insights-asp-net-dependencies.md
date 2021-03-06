---
title: Отслеживание зависимостей в Azure Application Insights | Документация Майкрософт
description: Анализ использования, доступности и производительности локального приложения или веб-приложения Microsoft Azure с помощью Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/08/2018
ms.author: mbullwin
ms.openlocfilehash: f2ebd2a021d3803b6e3f7d805b9253d181cb16c3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293645"
---
# <a name="set-up-application-insights-dependency-tracking"></a>Настройка Application Insights: отслеживание зависимостей
*Зависимость* – это внешний компонент, который вызывается приложением. Как правило, это служба, вызываемая с использованием HTTP, база данных или файловая система. [Application Insights](app-insights-overview.md) измеряет время, в течение которого приложение ожидает зависимости, и определяет, как часто происходит сбой вызова зависимости. Можно изучить определенные вызовы и установить их взаимосвязь с теми или иными запросами и исключениями.

![примеры диаграмм](./media/app-insights-asp-net-dependencies/10-intro.png)

Сейчас монитор зависимостей по умолчанию предоставляет отчеты о вызовах зависимостей таких типов:

* сервер;
  * базы данных SQL;
  * веб-службы и службы WCF ASP.NET, использующие привязки на основе HTTP;
  * локальные или удаленные HTTP-вызовы;
  * Azure Cosmos DB, таблица, хранилище BLOB-объектов и очередь.
* Веб-страницы
  * Вызовы AJAX

Мониторинг работает с помощью [инструментирования байт-кода](https://msdn.microsoft.com/library/z9z62c29.aspx) вокруг выбранных методов. Снижение производительности при этом сводится к минимуму.

Можно также написать собственные вызовы пакета SDK для мониторинга других зависимостей (в клиентском и серверном коде) с помощью [API TrackDependency](app-insights-api-custom-events-metrics.md#trackdependency).

## <a name="set-up-dependency-monitoring"></a>Настройка мониторинга зависимостей
Неполные сведения о зависимостях собираются автоматически [пакетом SDK для Application Insights](app-insights-asp-net.md). Чтобы получить полные данные, установите соответствующий агент для сервера узла.

| платформа | Install |
| --- | --- |
| Сервер IIS |[Установите монитор состояний на сервере](app-insights-monitor-performance-live-website-now.md) или [обновите свое приложение до .NET Framework 4.6 или более поздней версии](http://go.microsoft.com/fwlink/?LinkId=528259) и установите в нем [пакет SDK для Application Insights](app-insights-asp-net.md). |
| Веб-приложение Azure |На панели управления веб-приложения [откройте колонку "Application Insights"](app-insights-azure-web-apps.md) и при появлении соответствующего запроса выберите "Установить". |
| Облачная служба Azure |[Используйте задачу при запуске](app-insights-cloudservices.md) или [установите платформу .NET Framework 4.6 или более поздней версии](../cloud-services/cloud-services-dotnet-install-dotnet.md). |

## <a name="where-to-find-dependency-data"></a>Где найти данные зависимостей
* [Схема приложения](#application-map) наглядно представляет зависимости между приложением и смежными компонентами.
* [Колонки "Производительность", "Браузер" и "Сбой"](#performance-and-blades) отображают данные зависимостей сервера.
* [Колонка "Браузеры"](#ajax-calls) содержит вызовы AJAX из браузеров ваших пользователей.
* [Щелкните по очереди медленные или неудачно завершенные запросы](#diagnose-slow-requests), чтобы проверить из вызовы зависимостей.
* [Аналитику](#analytics) можно использовать для запроса данных зависимостей.

## <a name="application-map"></a>Схема сопоставления приложений
Схема сопоставления приложений — это визуальное средство, которое позволяет обнаруживать зависимости между компонентами приложения. Она создается автоматически на основе данных телеметрии из приложения. В этом примере показаны вызовы AJAX из сценариев браузера и вызовы REST из серверного приложения к двум внешним службам.

![Схема сопоставления приложений](./media/app-insights-asp-net-dependencies/08.png)

* **Из полей перейдите** к соответствующей зависимости и другим диаграммам.
* **Закрепите схему сопоставления** на [панели мониторинга](app-insights-dashboards.md), где она будет полностью функциональной.

[Узнайте больше](app-insights-app-map.md).

## <a name="performance-and-failure-blades"></a>Колонки "Производительность" и "Сбой"
В колонке "Производительность" отображается длительность вызовов зависимостей, выполненных серверным приложением. Она содержит сводную диаграмму и таблицу, разбитую по вызовам.

![Диаграммы зависимостей в колонке "Производительность"](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

По очереди щелкните сводные диаграммы или элементы таблицы, чтобы найти необработанные экземпляры этих вызовов.

![Экземпляры вызовов зависимостей](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

В колонке **Сбой** отображены **количества сбоев**. Сбоем является любой код возврата, который не попадает в диапазон 200–399 либо неизвестен.

> [!NOTE]
> **100% failures?** (100 % сбоев?) — вероятно, означает, что вы получаете только неполные данные зависимостей. Необходимо [настроить мониторинг зависимостей в соответствии со своей платформой](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>Вызовы AJAX
Колонка "Браузеры" отображает длительность и частоту сбоев вызовов AJAX из [JavaScript на веб-страницах](app-insights-javascript.md). Они отображаются как зависимости.

## <a name="diagnosis"></a> Диагностика медленных запросов
Каждое событие запроса связано с вызовами зависимостей, исключениями и другими событиями, которые отслеживаются во время обработки запроса вашим приложением. Поэтому, если какие-либо запросы завершаются неудачно, можно узнать, не является ли причиной этого медленный отклик зависимости.

Давайте подробно рассмотрим подобный пример.

### <a name="tracing-from-requests-to-dependencies"></a>Трассировка от запросов до зависимостей
Откройте колонку "Производительность" и посмотрите на сетку запросов.

![Список запросов со средними значениями и их количеством](./media/app-insights-asp-net-dependencies/02-reqs.png)

Получение ответа на верхний запрос заняло слишком много времени. Стоит попытаться выяснить, на что потрачено время.

Щелкните эту строку, чтобы просмотреть события для отдельного запроса.

![Список вхождений запросов](./media/app-insights-asp-net-dependencies/03-instances.png)

Щелкните любой длительный экземпляр, чтобы проверить его более детально, и прокрутите вниз до удаленных вызовов зависимостей, связанных с этим запросом.

![Найдите вызовы удаленных зависимостей и определите необычную продолжительность.](./media/app-insights-asp-net-dependencies/04-dependencies.png)

По-видимому, большую часть времени обработки этого запроса занял вызов локальной службы.

Выберите эту строку, чтобы получить дополнительную информацию.

![Щелкните эту удаленную зависимость, чтобы определить причину.](./media/app-insights-asp-net-dependencies/05-detail.png)

Похоже, проблема именно в этом. Мы выявили проблему, так что теперь осталось выяснить, почему этот вызов занимает так много времени.

### <a name="request-timeline"></a>Временная шкала запроса
В другом случае вызов зависимости не имеет большую длительность. Но, переключившись на представление временной шкалы, мы видим, где произошла задержка во время внутренней обработки.

![Найдите вызовы удаленных зависимостей и определите необычную продолжительность.](./media/app-insights-asp-net-dependencies/04-1.png)

Похоже, после первого вызова зависимости была большая задержка. Нам нужно изучить наш код, чтобы узнать, почему так произошло.

### <a name="profile-your-live-site"></a>Выполните профилирование активного сайта

Не имеете представления, куда девается время? [Профилировщик Application Insights](app-insights-profiler.md) отслеживает вызовы HTTP к активному веб-сайту и показывает, какие функции в коде выполнялись дольше всего.

## <a name="failed-requests"></a>Failed requests (Неудачные запросы)
Неудачно завершенные запросы также могут быть связаны с неудачными вызовами зависимостей. Опять же, мы можем определить причину проблемы.

![Щелкните диаграмму неудачных запросов.](./media/app-insights-asp-net-dependencies/06-fail.png)

Щелкните экземпляр неудачно завершенного запроса и просмотрите связанные с ним события.

![Щелкните тип запроса, а затем его экземпляр, чтобы открыть этот же экземпляр в другом представлении. Щелкните его еще раз, чтобы просмотреть подробную информацию об исключении.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analytics
Вы можете отслеживать зависимости, используя [язык запросов Log Analytics](https://docs.loganalytics.io/). Ниже приведены некоторые примеры.

* Поиск неудачно завершенных вызовов зависимостей:

```

    dependencies | where success != "True" | take 10
```

* Поиск вызовов AJAX:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Поиск вызовов зависимостей, связанных с запросами:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Поиск вызовов AJAX, связанных с представлениями страницы:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Пользовательское отслеживание зависимостей
Стандартный модуль отслеживания зависимостей выявляет внешние зависимости, например базы данных и API REST, автоматически. Однако при необходимости аналогичную обработку можно настроить и для других компонентов.

Код, который отправляет сведения о зависимостях, можно написать с использованием того же интерфейса [API TrackDependency](app-insights-api-custom-events-metrics.md#trackdependency) , что используется в стандартных модулях.

Например, формируя код на основе готовой сборки, можно назначить время для всех вызовов этого кода и таким образом выяснить, как он влияет на время отклика вашей системы. Чтобы эти данные отображались в диаграммах зависимостей в Application Insights, используйте для их отправки командлет `TrackDependency`.

```csharp

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
                // The call above has been made obsolete in the latest SDK. The updated call follows this format:
                // TrackDependency (string dependencyTypeName, string dependencyName, string data, DateTimeOffset startTime, TimeSpan duration, bool success);
            }
```

Чтобы отключить стандартный модуль отслеживания зависимостей, удалите ссылку на DependencyTrackingTelemetryModule в файле [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Устранение неполадок
*Флаг успешной зависимости всегда показывает значение true или false.*

*SQL-запрос отображается неполностью.*

Просмотрите таблицу ниже и проверьте, правильную ли конфигурацию вы выбрали для мониторинга зависимостей своего приложения.

| платформа | Install |
| --- | --- |
| Сервер IIS |Либо [установите Монитор состояния на сервер](app-insights-monitor-performance-live-website-now.md), либо [обновите в своем приложении .NET Framework до версии 4.6 (или новее)](http://go.microsoft.com/fwlink/?LinkId=528259) и установите в него [пакет SDK для Application Insights](app-insights-asp-net.md). |
| Веб-приложение Azure |На панели управления веб-приложения [откройте колонку "Application Insights"](app-insights-azure-web-apps.md) и при появлении соответствующего запроса выберите "Установить". |
| Облачная служба Azure |[Используйте задачу при запуске](app-insights-cloudservices.md) или [установите платформу .NET Framework 4.6 или более поздней версии](../cloud-services/cloud-services-dotnet-install-dotnet.md). |

## <a name="video"></a>Видео

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Дополнительная информация
* [Исключения](app-insights-asp-net-exceptions.md)
* [Данные пользователей и страниц](app-insights-javascript.md)
* [Доступность](app-insights-monitor-web-app-availability.md)
