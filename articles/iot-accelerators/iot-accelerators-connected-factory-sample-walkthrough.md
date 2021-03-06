---
title: Пошаговое руководство. Работа с решением подключенной фабрики Azure | Документация Майкрософт
description: Описание акселератора решений подключенной фабрики Интернета вещей Azure и его архитектуры.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 214573374498955c4abf018e8c7854c6d9c2c164
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627148"
---
# <a name="connected-factory-solution-accelerator-walkthrough"></a>Инструкция по работе с акселератором решений подключенной фабрики

[Акселератор решений][lnk-preconfigured-solutions] подключенной фабрики — это полнофункциональная реализация отраслевого решения, предназначенного для следующих задач:

* Подключается к виртуальным отраслевым устройствам под управлением серверов OPC UA на производственных линиях виртуальной фабрики и к физическим устройствам сервера OPC UA. Дополнительные сведения об OPC UA см. в статье [Часто задаваемые вопросы о предварительно настроенном решении для подключенной фабрики IoT Suite](iot-accelerators-faq-cf.md).
* Показывает оперативные ключевые показатели эффективности и общую эффективность оборудования этих устройств и производственных линий.
* Демонстрирует, как можно использовать облачное приложение для взаимодействия с серверными системами OPC UA.
* Позволяет подключать собственные устройства под управлением сервера OPC UA.
* Позволяет просматривать и изменять данные сервера OPC UA.
* Интегрируется со службой Azure Time Series Insights (TSI) для предоставления пользовательских представлений данных с серверов OPC UA.

Его можно использовать в качестве отправной точки для собственной реализации и [настроить][lnk-customize] в соответствии с потребностями конкретной организации.

В этой статье рассматриваются некоторые основные компоненты решения подключенной фабрики. В статье также описывается порядок обмена данными в решении. Эти знания помогут вам:

* устранить проблемы, возникшие с решением;
* спланировать настройку решения в соответствии с определенными требованиями;
* спроектировать собственное решение IoT, использующее службы Azure.

См. дополнительные сведения о [подключенной фабрике](iot-accelerators-faq-cf.md).

## <a name="logical-architecture"></a>Логическая архитектура

На следующей схеме показаны логические компоненты акселератора решений:

![Логическая архитектура подключенной фабрики][connected-factory-logical]

## <a name="communication-patterns"></a>Модели связи

Решение использует [спецификацию публикации и подписки OPC UA](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) для отправки данных телеметрии OPC UA в Центр Интернета вещей в формате JSON. В решении для этого используется модуль IoT Edge [издателя OPC](https://github.com/Azure/iot-edge-opc-publisher).

Решение также содержит клиент OPC UA, интегрированный в веб-приложение, которое может устанавливать соединение с локальными серверами OPC UA. Клиент использует [обратный прокси-сервер](https://wikipedia.org/wiki/Reverse_proxy) и с помощью Центра Интернета вещей создает подключение без необходимости открывать порты в локальном брандмауэре. Эта модель связи называется [взаимодействием с поддержкой службы](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/). В решении для этого используется модуль IoT Edge [прокси OPC](https://github.com/Azure/iot-edge-opc-proxy/).


## <a name="simulation"></a>Моделирование

Виртуальные станции и системы управления производственными процессами (MES) образуют производственную линию фабрики. Виртуальные устройства и модуль издателя OPC основаны на [стандарте OPC UA .NET][lnk-OPC-UA-NET-Standard], опубликованном OPC Foundation.

Прокси-сервер и издатель OPC реализованы как модули на основе [Edge Интернета вещей Azure][lnk-Azure-IoT-Gateway]. К каждой виртуальной производственной линии подключен отдельный шлюз.

Все виртуальные компоненты работают в контейнерах Docker, размещенных на виртуальной машине Azure под управлением Linux. По умолчанию моделирование настроено для работы восьми виртуальных производственных линий.

## <a name="simulated-production-line"></a>Виртуальная производственная линия

Производственная линия изготавливает детали. Она состоит из различных станций: станция сборки, станция тестирования и станция упаковки.

Моделирование работает и обновляет данные, которые отображаются на узлах OPC UA. Работа всех станций виртуальной производственной линии координируется с помощью MES через OPC UA.

## <a name="simulated-manufacturing-execution-system"></a>Виртуальная система управления производственными процессами

Система MES отслеживает каждую станцию на производственной линии через OPC UA для обнаружения изменений в состоянии станции. Она вызывает методы OPC UA для управления станциями и передает изделие с одной рабочей станции на другую, пока оно не будет готово.

## <a name="gateway-opc-publisher-module"></a>Модуль издателя OPC шлюза

Модуль издателя OPC подключается к серверам OPC UA на станции и подписывается на узлы OPC, которые должны быть опубликованы. Модуль преобразует данные узла в формат JSON, шифрует их и отправляет Центр Интернета вещей в виде сообщений публикации и подписки OPC UA.

Модулю издателя OPC требуется только исходящий порт HTTPS (443), и он может работать с существующей инфраструктурой предприятия.

## <a name="gateway-opc-proxy-module"></a>Модуль прокси OPC шлюза

Модуль прокси OPC UA шлюза предоставляет доступ через туннели двоичным командам и управляющим сообщениям OPC UA. Для его работы требуется только исходящий порт HTTPS (443). Он может работать с существующей инфраструктурой предприятия, в том числе с веб-прокси.

Этот модуль использует методы устройства Центра Интернета вещей для передачи пакетированных данных TCP/IP на уровне приложения. Таким образом с помощью SSL/TLS он обеспечивает доверие конечной точки, шифрование данных и их целостность.

Двоичный протокол OPC UA, который ретранслируется через сам прокси, использует проверку подлинности и шифрование UA.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Модуль издателя OPC шлюза подписывается на узлы сервера OPC UA для обнаружения изменений в значениях данных. Если обнаруживается изменение данных на одном из узлов, этот модуль отправляет сообщения в Центр Интернета вещей Azure.

Центр Интернета вещей передает источник события в Azure TSI. TSI хранит данные в течение 30 дней в зависимости от меток времени, вложенных в сообщения. Эти данные включают:

* OPC UA ApplicationUri;
* OPC UA NodeId;
* значение узла;
* метка времени источника;
* OPC UA DisplayName.

Сейчас TSI не позволяет клиентам настраивать продолжительность хранения данных.

TSI отправляет запрос к данным узла с помощью **SearchSpan** (**Time.From**, **Time.To**) и выполняет вычисление на основе значений **OPC UA ApplicationUri**, **OPC UA NodeId** или **OPC UA DisplayName**.

Чтобы получить данные для датчиков общей эффективности оборудования и ключевых показателей эффективности, а также диаграммы временных рядов, данные группируются по количеству событий (Sum — всего событий, Avg — среднее количество, Min — минимальное количество и Max — максимальное количество).

Временные ряды создаются с помощью другого процесса. Показатели общей эффективности оборудования и ключевые показатели эффективности вычисляются на основе базовых данных станции и отображаются для топологии (производственные линии, фабрики, предприятие) в приложении.

Кроме того, временные ряды для топологии общей эффективности оборудования и ключевых показателей эффективности вычисляются в приложении, когда готов отображаемый временной диапазон. Например, представление за день обновляется каждый час.

Представление временного ряда данных узла извлекается напрямую из TSI с помощью вычисления временного диапазона.

## <a name="iot-hub"></a>Центр Интернета вещей
[Центр Интернета вещей][lnk-IoT Hub] принимает данные, отправленные из модуля издателя OPC в облако, и предоставляет к ним доступ службе Azure TSI. 

Центр Интернета вещей также выполняет в решении следующие функции.
- Поддерживает реестр удостоверений, в котором хранятся идентификаторы всех модулей издателя OPC и модулей прокси OPC.
- Используется в качестве канала транспортировки для двустороннего обмена данными модуля прокси OPC.

## <a name="azure-storage"></a>Хранилище Azure
Решение использует хранилище BLOB-объектов Azure в качестве дискового хранилища виртуальной машины и в качестве хранилища данных развертывания.

## <a name="web-app"></a>Веб-приложение
Веб-приложение, развернутое как часть акселератора решений, состоит из встроенного клиента OPC UA, механизма обработки оповещений и визуализации данных телеметрии.

## <a name="telemetry-data-flow"></a>Передача данных телеметрии

![Передача данных телеметрии](./media/iot-accelerators-connected-factory-sample-walkthrough/telemetry_dataflow.png)

### <a name="flow-steps"></a>Этапы передачи

1. Издатель OPC считывает необходимые сертификаты X509 OPC UA и учетные данные безопасности Центра Интернета вещей из локального хранилища сертификатов.
    - При необходимости издатель OPC создает и сохраняет отсутствующие сертификаты и учетные данные в хранилище сертификатов.

2. Издатель OPC регистрируется в Центре Интернета вещей.
    - Используется настроенный протокол. Вы можете использовать поддерживаемый протокол пакета SDK клиента Центра Интернета вещей. По умолчанию используется MQTT.
    - Обмен данными по протоколу защищен протоколом TLS.

3. Издатель OPC считывает файл конфигурации.

4. Издатель OPC создает сеанс OPC с каждым настроенным сервером OPC UA.
    - Используется TCP-соединение.
    - Происходит проверка подлинности между издателем OPC и сервером OPC UA с использованием сертификата X509.
    - Дальнейший трафик OPC UA шифруется с использованием настроенного механизма шифрования OPC UA.
    - Издатель OPC создает подписки OPC в сеансе OPC для каждого настроенного интервала публикации.
    - Создаются отслеживаемые элементы OPC для узлов OPC для публикации в подписке OPC.

5. При изменении значения отслеживаемого узла OPC сервер OPC UA отправляет обновления издателю OPC.

6. Издатель OPC перекодирует новое значение.
    - Несколько изменений помещаются в пакеты, если включена пакетная обработка.
    - Создается сообщение Центра Интернета вещей.

7. Издатель OPC отправляет сообщение в Центр Интернета вещей.
    - Используется настроенный протокол.
    - Обмен данными защищен протоколом TLS.

8. Time Series Insights (TSI) считывает сообщения из Центра Интернета вещей.
    - Используется AMQP через TCP/TLS.
    - Этот шаг является внутренним для центра обработки данных.

9. Данные сохраняются в TSI.

10. Подключенная фабрика веб-приложения в Службе приложений Azure запрашивает требуемые данные в TSI.
    - Используется безопасный обмен данными через TCP/TLS.
    - Этот шаг является внутренним для центра обработки данных.

11. Веб-браузер подключается к веб-приложению подключенной фабрики.
    - Отображается панель мониторинга подключенной фабрики.
    - Выполняется подключение по протоколу HTTPS.
    - Для доступа к приложению подключенной фабрики нужно выполнить аутентификацию пользователя через Azure Active Directory.
    - Все вызовы приложения подключенной фабрики данных в веб-API защищены с помощью токенов для защиты от подделки.

12. При обновлении веб-приложение подключенной фабрики данных отправляет обновленные данные в веб-браузер.
    - Используется протокол SignalR.
    - Обеспечивается защита через TCP/TLS.

## <a name="browsing-data-flow"></a>Передача данных просмотра

![Передача данных просмотра](./media/iot-accelerators-connected-factory-sample-walkthrough/browsing_dataflow.png)

### <a name="flow-steps"></a>Этапы передачи

1. Запускается прокси-сервер OPC (компонент сервера).
    - Ключи общего доступа считываются из локального хранилища.
    - При необходимости отсутствующие ключи доступа сохраняются в хранилище.

2. Прокси-сервер OPC (серверный компонент) регистрируется в Центре Интернета вещей.
    - Все известные устройства считываются из Центра Интернета вещей.
    - Используется MQTT через TLS через Socket или безопасный Websocket.

3. Веб-браузер подключается к веб-приложению подключенной фабрики и отображает ее панель мониторинга.
    - Используется протокол HTTPS.
    - Пользователь выбирает сервер OPC UA для подключения.

4. Веб-приложение подключенной фабрики устанавливает сеанс OPC UA на выбранном сервере OPC UA.
    - Используется стек OPC UA.

5. Транспорт прокси-сервера OPC получает из стека OPC UA запрос на подключение сокета TCP к серверу OPC UA.
    - Он просто получает полезные данные TCP и использует их без изменений.
    - Этот шаг выполняется в веб-приложении подключенной фабрики.

6. Прокси-сервер OPC (клиентский компонент) находит устройство прокси-сервера OPC (серверный компонент) в реестре устройств Центра Интернета вещей. Затем в Центре Интернета вещей вызывается метод устройства прокси-сервера OPC (серверный компонент).
    - Для поиска прокси-сервера OPC используется протокол HTTPS через TCP/TLS.
    - HTTPS через TCP/TLS используется также для подключения сокета TCP к серверу OPC UA.
    - Этот шаг является внутренним для центра обработки данных.

7. Затем в Центре Интернета вещей вызывается метод устройства прокси-сервера OPC (серверный компонент).
    - Для подключения сокета TCP к серверу OPC UA используется подключение MQTT через TLS через Socket или Secure Websocket.

8. Прокси-сервер OPC (серверный компонент) отправляет полезные данные TCP в производственную сеть.

9. Сервер OPC UA обрабатывает полезные данные и отправляет ответ.

10. Сокет получает ответ прокси-сервера OPC (серверный компонент).
    - Прокси-сервер OPC отправляет данные как возвращаемое значение метода устройства в Центр Интернета вещей и на прокси-сервер OPC (клиентский компонент).
    - Эти данные доставляются в стек OPC UA в приложении подключенной фабрики.

11. Веб-приложение подключенной фабрики возвращает интерфейс браузера OPC со сведениями OPC UA, полученными с сервера OPC UA, в веб-браузер для отображения.
    - При просмотре с использованием пространства адресов OPC и применении функций к узлам в нем клиентская часть интерфейса браузера OPC использует вызовы AJAX через HTTPS, защищенные с помощью токенов защиты от подделки, для получения данных из веб-приложения подключенной фабрики.
    - При необходимости клиент использует метод обмена, описанный в шагах с 4 по 10, для обмена данными с сервером OPC UA.

> [!NOTE]
> Серверный и клиентский компонент прокси-сервера OPC завершают действия 4–10 для всего трафика TCP, связанного с обменом данными OPC UA.

> [!NOTE]
> Для сервера OPC UA и стека OPC UA в веб-приложении подключенной фабрики реализован прозрачный обмен данными прокси-сервера OPC. Применяются все возможности обеспечения безопасности для аутентификации и шифрования в OPC UA.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об акселераторах решений Интернета вещей см. в следующих статьях:

* [Разрешения на сайте azureiotsuite.com][lnk-permissions]
* [Развертывание шлюза Edge для предварительно настроенного решения подключенной фабрики в ОС Windows или Linux](iot-accelerators-connected-factory-gateway-deployment.md)
* [OPC Publisher for Azure IoT Edge](https://github.com/Azure/iot-edge-opc-publisher/blob/master/README.md) (Издатель OPC для Azure IoT Edge)

[connected-factory-logical]:media/iot-accelerators-connected-factory-sample-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]:iot-accelerators-what-are-solution-accelerators.md
[lnk-customize]: iot-accelerators-connected-factory-customize.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-accelerators-faq.md
