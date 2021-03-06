---
title: Как добавить источник событий центра событий в службу "Аналитика временных рядов Azure" | Документация Майкрософт
description: В этой статье описывается, как добавить источник события, подключенный к концентратору событий и к среде "Аналитика временных рядов".
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: edett
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/21/2017
ms.openlocfilehash: 8b1fe447cb673b9bc1f4fe4e73f7412a21f701a5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "36330868"
---
# <a name="how-to-add-an-event-hub-event-source-to-time-series-insights-environment"></a>Как добавить источник событий концентратора событий в среду "Аналитика временных рядов Azure"

В этой статье описывается, как добавить источник события, считывающий данные из концентратора событий в среду "Аналитика временных рядов", с помощью портала Azure.

## <a name="prerequisites"></a>предварительным требованиям
- Создайте среду "Аналитика временных рядов". Дополнительные сведения см. в руководстве по [созданию среды "Аналитика временных рядов Azure"](time-series-insights-get-started.md). 
- Создайте концентратор событий. Дополнительные сведения о концентраторах событий см. в статье [Создание пространства имен концентраторов событий и концентратора событий с помощью портала Azure](../event-hubs/event-hubs-create.md).
- Концентратор событий должен иметь активные события сообщений, которые в него отправляются. Дополнительные сведения см. в статье [Отправка событий в концентраторы событий Azure с помощью платформы .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Создайте выделенную группу потребителей в концентраторе событий для среды "Аналитика временных рядов". Каждый источник событий "Аналитика временных рядов" должен иметь собственную выделенную группу потребителей, не используемую другими потребителями. Если несколько модулей чтения используют события из одной группы потребителей, как правило, во всех модулях могут произойти сбои. Обратите внимание, что у каждого концентратора событий должно быть не более 20 групп получателей. Дополнительные сведения см. в [руководстве по программированию концентраторов событий](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Добавление группы потребителей в концентратор событий
Группы потребителей используются приложениями для извлечения данных из концентраторов событий Azure. Укажите выделенную группу потребителей для использования только в этой среде "Аналитика временных рядов", чтобы надежно считывать данные из концентратора событий.

Чтобы добавить новую группу потребителей в концентратор событий, выполните следующие действия.
1. На портале Azure найдите и откройте концентратор событий.

2. Под заголовком **Сущности** установите флажок **Группы потребителей**.

   ![Концентратор событий — добавление группы потребителей](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

3. Выберите **+Группа потребителей**, чтобы добавить новую группу потребителей. 

4. На странице **Группы потребителей** укажите новое уникальное **имя**.  Используйте это же имя при создании нового источника событий в среде "Аналитика временных рядов Azure".

5. Выберите **Создать** для создания группы потребителей.

## <a name="add-a-new-event-source"></a>Добавление нового источника событий
1. Войдите на [портале Azure](https://portal.azure.com).

2. Найдите существующую среду "Аналитика временных рядов". На портале Azure щелкните **All resources** (Все ресурсы) в меню слева. Выберите среду Time Series Insights.

3. В разделе **Топология окружения** щелкните **Источники событий**.

   ![Добавление источников событий](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

4. Щелкните **+ Добавить**.

5. Укажите **Имя источника событий**, уникальное для этой среды "Аналитика временных рядов", например **event-stream**.

   ![Заполните первые три параметра в форме.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

6. В качестве параметра **Источник** выберите **Концентратор событий**.

7. Выберите соответствующий **Вариант импорта**. 
   - Выберите **Использовать концентратор событий из доступных подписок**, если в одной из ваших подписок уже имеется существующий концентратор событий. Это самый простой подход.
   - Выберите **Указать настройки концентратора событий вручную**, если концентратор событий находится за пределами вашей подписки или требуется выбрать дополнительные параметры. 

8. Если выбран параметр **Использовать концентратор событий из доступных подписок**, см. описание каждого из обязательных свойств в следующей таблице:

   ![Сведения о подписке и Центре Интернета вещей](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Свойство | ОПИСАНИЕ |
   | --- | --- |
   | идентификатор подписки; | Выберите подписку, в которой был создан этот концентратор событий.
   | Пространство имен служебной шины | Выберите пространство имен служебной шины, содержащее концентратор событий.
   | Имя концентратора событий | Выберите имя концентратора событий.
   | Имя политики концентратора событий | Выберите политику общего доступа, которую можно создать на вкладке с настройками концентратора событий. Каждой политике общего доступа присваивается имя, а также для нее задаются разрешения и ключи доступа. Политика общего доступа для источника событий *должна* иметь разрешения на **чтение**.
   | Ключ политики концентратора событий | Значение ключа может быть предварительно заполнено.
   | Группа потребителей концентратора событий | Группа потребителей для чтения событий из концентратора событий. Мы настоятельно рекомендуем использовать выделенную группу потребителей для источника событий. |
   | Формат сериализации событий | JSON — сейчас это единственная доступная сериализация. Сообщения событий должны быть в этом формате, иначе данные не удастся прочитать. |
   | Имя свойства метки времени | Чтобы определить это значение, необходимо понимать формат данных сообщения, отправляемого в концентратор событий. Это значение представляет собой **имя** определенного свойства события в данных сообщения, которое требуется использовать в качестве метки времени события. Для этого значения учитывается регистр. Если оставить значение пустым, в качестве метки времени события будет использоваться **время включения события в очередь** в пределах источника событий. |


9. Если выбран параметр **Указать настройки концентратора событий вручную**, см. описание каждого из обязательных свойств в следующей таблице:

   | Свойство | ОПИСАНИЕ |
   | --- | --- |
   | Идентификатор подписки | Подписка, в которой был создан этот концентратор событий.
   | Группа ресурсов | Группа ресурсов, в которой создан этот концентратор событий.
   | Пространство имен служебной шины | Пространство имен Service Bus — это контейнер для набора сущностей обмена сообщениями. При создании нового концентратора событий создается также пространство имен Service Bus.
   | Имя концентратора событий | Имя концентратора событий. При создании вы также присваиваете концентратору событий определенное имя.
   | Имя политики концентратора событий | Политика общего доступа, которую можно создать на вкладке с настройками концентратора событий. Каждой политике общего доступа присваивается имя, а также для нее задаются разрешения и ключи доступа. Политика общего доступа для источника событий *должна* иметь разрешения на **чтение**.
   | Ключ политики концентратора событий | Ключ общего доступа, используемый для проверки подлинности при получении доступа к пространству имен служебной шины. Введите первичный или вторичный ключ здесь.
   | Группа потребителей концентратора событий | Группа потребителей для чтения событий из концентратора событий. Мы настоятельно рекомендуем использовать выделенную группу потребителей для источника событий.
   | Формат сериализации событий | JSON — сейчас это единственная доступная сериализация. Сообщения событий должны быть в этом формате, иначе данные не удастся прочитать. |
   | Имя свойства метки времени | Чтобы определить это значение, необходимо понимать формат данных сообщения, отправляемого в концентратор событий. Это значение представляет собой **имя** определенного свойства события в данных сообщения, которое требуется использовать в качестве метки времени события. Для этого значения учитывается регистр. Если оставить значение пустым, в качестве метки времени события будет использоваться **время включения события в очередь** в пределах источника событий. |

10. Добавьте имя отдельной группы потребителей TSI, которое вы добавили в концентратор событий.

11. Выберите **Создать**, чтобы добавить новый источник событий.
   
   ![Нажмите кнопку "Создать"](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   После создания источника событий Time Series Insights автоматически запустит передачу данных в среду.


## <a name="next-steps"></a>Дополнительная информация
- [Определите политики доступа к данным](time-series-insights-data-access.md), чтобы защитить ваши данные.
- [Отправьте события](time-series-insights-send-events.md) в источник событий.
- Получите доступ к своей среде в [обозревателе службы "Аналитика временных рядов"](https://insights.timeseries.azure.com).
