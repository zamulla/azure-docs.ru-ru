---
title: Сохранение сообщений Центра Интернета вещей в хранилище данных Azure | Документация Майкрософт
description: Используйте маршрутизацию сообщений Центра Интернета вещей для их сохранения в хранилище BLOB-объектов Azure. Сообщения Центра Интернета вещей содержат такие сведения, как данные датчиков, отправленные из устройства Интернета вещей.
author: rangv
manager: ''
keywords: хранилище данных Центра Интернета вещей, хранилище для данных датчиков Центра Интернета вещей
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 84355453a5cb8d8f42abdcbde5432651c9c035b0
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856296"
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Сохранение сообщений Центра Интернета вещей, которые содержат данные датчиков, в хранилище BLOB-объектов Azure

![Комплексная схема](./media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Что вы узнаете

Вы узнаете, как создать учетную запись хранения Azure и приложение-функцию Azure для сохранения сообщений Центра Интернета вещей в хранилище BLOB-объектов Azure.

## <a name="what-you-do"></a>Что нужно сделать

- Создайте учетную запись хранения Azure.
- Настройте Центр Интернета вещей для маршрутизации сообщений в хранилище.

## <a name="what-you-need"></a>Необходимые элементы

- [Настройте свое устройство](iot-hub-raspberry-pi-kit-node-get-started.md). У вас должны быть следующие компоненты:
  - активная подписка Azure;
  - Центр Интернета вещей в подписке; 
  - работающее приложение, отправляющее сообщения в Центр Интернета вещей.

## <a name="create-an-azure-storage-account"></a>Создание учетной записи хранения Azure

1. На [портале Azure](https://portal.azure.com/) последовательно выберите **Создать ресурс** > **Хранилище** > **Учетная запись хранения**.

2. Введите необходимые сведения для учетной записи хранения:

   ![Создание учетной записи хранения на портале Azure](./media/iot-hub-store-data-in-azure-table-storage/1_azure-portal-create-storage-account.png)

   * **Имя**. Имя учетной записи хранения. Оно должно быть глобально уникальным.

   * **Тип учетной записи**. Выберите `Storage (general purpose v1)`.

   * **Расположение**. Выберите то же расположение, которое используется для Центра Интернета вещей.

   * **Репликация**. Выберите `Locally-redundant storage (LRS)`.

   * **Производительность**. Выберите `Standard`.

   * **Требуется безопасная передача данных**. Выберите `Disabled`.

   * **Подписка**. Выберите подписку Azure.

   * **Группа ресурсов**. Выберите ту же группу ресурсов, которую использует Центр Интернета вещей.

   * **Закрепить на панели мониторинга.** Установите этот флажок, чтобы быстро открывать Центр Интернета вещей с помощью панели мониторинга.

3. Нажмите кнопку **Создать**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Подготовка Центра Интернета вещей для маршрутизации сообщений в хранилище

Центр Интернета вещей изначально поддерживает маршрутизацию сообщений в хранилище Azure в качестве больших двоичных объектов. Дополнительные сведения о пользовательских конечных точках Центра Интернета вещей см. в [списке встроенных конечных точек Центра Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints).

### <a name="add-storage-as-a-custom-endpoint"></a>Добавление хранилища в качестве пользовательской конечной точки

1. Перейдите в Центр Интернета вещей на портале Azure. 

2. Щелкните **Конечные точки** > **Добавить**. 

3. Назовите конечную точку и выберите **Контейнер службы хранилища Azure** в качестве типа конечной точки. 

4. С помощью средства выбора выберите учетную запись хранения, созданную в предыдущем разделе. Создайте контейнер хранилища, а затем выберите его и щелкните **ОК**.

   ![Создание пользовательской конечной точки в Центре Интернета вещей](./media/iot-hub-store-data-in-azure-table-storage/2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Добавление маршрута для маршрутизации данных в хранилище

1. Щелкните **Маршруты** > **Добавить** и введите имя маршрута. 

2. Выберите **Device Messages** (Сообщения устройства) в качестве источника данных и созданную конечную точку хранилища в качестве конечной точки маршрута. 

3. Введите `true` в качестве строки запроса, а затем щелкните **Сохранить**.

   ![Создание маршрута в Центре Интернета вещей](./media/iot-hub-store-data-in-azure-table-storage/3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Добавление маршрута для телеметрии критического пути (необязательно)

По умолчанию Центр Интернета вещей направляет все сообщения, которые не соответствуют другим маршрутам, на встроенную конечную точку. Так как все сообщения телеметрии теперь соответствуют правилу, которое направляет сообщения в хранилище, нужно добавить еще один маршрут для сообщений, которые будут записаны во встроенную конечную точку. Дополнительная плата за маршрутизацию сообщений в несколько конечных точек не взимается.

> [!NOTE]
> Этот шаг можно пропустить, если дополнительная обработка сообщений телеметрии не выполняется.

1. В области "Маршруты" щелкните **Добавить** и введите имя маршрута. 

2. Выберите **Device Messages** (Сообщения устройства) в качестве источника данных и **события** в качестве конечной точки. 

3. Введите `true` в качестве строки запроса, а затем щелкните **Сохранить**.

  ![Создание маршрута критического пути в Центре Интернета вещей](./media/iot-hub-store-data-in-azure-table-storage/4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Проверка сообщения в контейнере хранилища

1. Выполните пример приложения на устройстве, чтобы отправить сообщения в Центр Интернета вещей.

2. [Скачайте и установите обозреватель хранилищ Azure](http://storageexplorer.com/).

3. Откройте обозреватель хранилищ, щелкните **Add an Azure Account** (Добавление учетной записи Azure)  > **Вход**, а затем выполните вход в свою учетную запись Azure.

4. Щелкните подписку Azure, выберите **Учетные записи хранения** > ваша учетная запись хранения > **Контейнеры больших двоичных объектов** и щелкните нужный контейнер.

   Вы должны увидеть сообщения, отправленные с устройства в Центр Интернета вещей, в контейнере большого двоичного объекта.

## <a name="clean-up-resources"></a>Очистка ресурсов 

В этом руководстве вы добавили учетную запись хранения, а затем добавили маршрутизацию для записи сообщений из Центра Интернета вещей в эту учетную запись хранения. Для очистки созданных ресурсов необходимо удалить сведения о маршрутизации, а затем удалить учетную запись хранения. 

1. Войдите на [портал Azure](https://portal.azure.com).

2. Нажмите **Группы ресурсов** и выберите группу ресурсов, которая использовалась. Появится список ресурсов в группе. 

   > [!NOTE]
   > Если вы хотите удалить все ресурсы в группе ресурсов, нажмите **Удалить**, чтобы удалить группу ресурсов, а затем следуйте указаниям. В результате будет удалено все содержимое группы ресурсов, то есть ресурсы будут очищены и вы сможете перейти к следующему разделу.

3. Щелкните Центр Интернета вещей, который вы использовали для этого руководства. 

4. В области Центра Интернета вещей щелкните **Маршруты**. Установите флажок рядом с добавленным правилом маршрутизации и щелкните **Удалить**. В ответ на запрос о подтверждении удаления маршрута нажмите кнопку **Да**.

   ![Удаление правила маршрутизации](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-routing.png)

   Закройте область маршрутизации. Вы вернетесь в область "Группа ресурсов".

5. Еще раз щелкните Центр Интернета вещей. 

6. В области Центра Интернета вещей щелкните **Конечные точки**. Установите флажок рядом с конечной точкой, добавленной для контейнера хранилища, и щелкните **Удалить**. В ответ на запрос о подтверждении удаления выбранной конечной точки нажмите кнопку **Да**.

    ![Удаление конечной точки](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-endpoint.png)

    Закройте область "Конечные точки". Вы вернетесь в область "Группа ресурсов". 

7.  Щелкните учетную запись хранения, настроенную для этого руководства. 

8.  В области учетной записи хранения щелкните **Удалить**, чтобы удалить учетную запись. Появится область **Удаление учетной записи хранения**.

   ![Удаление учетной записи хранения](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-storageaccount.png)

8.  Введите имя учетной записи хранения и щелкните **Удалить** в нижней части области. 

## <a name="next-steps"></a>Дополнительная информация

Вы успешно создали учетную запись хранения Azure и перенаправили сообщения из Центр Интернета вещей в контейнер больших двоичных объектов в учетной записи хранения.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
