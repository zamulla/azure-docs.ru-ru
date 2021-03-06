---
title: Добавление предварительно созданных сущностей в приложение для обучения ведению диалога. Microsoft Cognitive Services | Документация Майкрософт
titleSuffix: Azure
description: Сведения о том, как добавить предварительно созданные сущности в приложение для обучения ведению диалога.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f014464419bfac39a9e57e679fcd28a737e9ebdb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382484"
---
# <a name="how-to-add-pre-built-entities"></a>Добавление предварительно созданных сущностей
В этом руководстве показано, как добавить предварительно созданные сущности в приложение для обучения ведению диалога.

## <a name="requirements"></a>Требования
Для работы с этим руководством необходим запущенный бот обучения общего назначения

    npm run tutorial-general

## <a name="details"></a>Сведения

Предварительно созданные сущности распознают общие типы сущностей, например числа, даты, денежные суммы и другие.  В отличие от настраиваемых сущностей, они готовы к работе и не требуют обучения.  Также, в отличие от настраиваемых сущностей, их поведение нельзя изменить.  По умолчанию предварительно созданные сущности поддерживают несколько значений — то есть в памяти бота будет накапливаться каждый идентифицированный экземпляр сущности.

## <a name="steps"></a>Действия

### <a name="create-the-application"></a>Создание приложения

1. В пользовательском веб-интерфейсе щелкните "Новое приложение".
2. В поле "Имя" введите BuiltInEntities. Затем нажмите кнопку "Создать".

### <a name="create-an-entity"></a>Создание сущности

1. Щелкните "Сущности", затем "Создать сущность".
2. Щелкните раскрывающийся список EntityType и выберите datetimev2.
    - Программируемые и отрицаемые параметры недоступны, так как они не применяются к предварительно созданным сущностям.
3. Щелкните Создать.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>Создание двух действий

1. Последовательно щелкните "Действия" и "Создать действие".
2. В поле "Ответ" введите "Дата $luis-datetimev2".
3. Щелкните Создать.

![](../media/tutorial7_actions.PNG)

Теперь создайте второе действие:

1. Выберите "Действия", затем "Создать действие", чтобы создать второе действие.
3. В поле "Ответ" введите "What's the date?" (Какая дата?).
4. В поле Disqualifying Entities (Блокирующие сущности) введите "luis-datetimev2".
4. Нажмите кнопку "Создать"

![](../media/tutorial7_actions2.PNG)

Теперь у вас есть два действия.

### <a name="train-the-bot"></a>Обучение бота

1. Щелкните Train Dialogs (Обучение диалогам), затем New Train Dialog (Обучение новому диалогу).
2. Введите "hello" (привет).
3. Щелкните Score Actions (Оценка действий) и выберите "What's the date?" (Какая дата?).
2. Введите "today" (сегодня). 
    - Обратите внимание, что ответ "today" (сегодня) снабжен тегом и отображается во второй строке, так как это предварительно созданная сущность и она не подлежит редактированию.
5. Щелкните Score Actions (Оценка действий).
    - Обратите внимание, что дата теперь отображается в разделе Entity Memory (Память сущности). 
    - Если навести указатель мыши на дату, отобразятся дополнительные данные, предоставленные службой LUIS, которые могут быть полезны и в дальнейшем обработаны в коде. 
6. Выберите "The date is $luis-datetimev2" (Дата $luis-datetimev2).
7. Щелкните Done Teaching (Завершить обучение).

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Альтернативные входные данные](./8-alternative-inputs.md)
