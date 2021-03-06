---
title: Использование преобразования речи в текст | Документы Майкрософт
description: Сведения об использовании преобразования речи в текст в службе речи
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 878a31992415b1f8688afcfb186fcd94ce2567b4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "35383377"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Использование преобразования речи в текст в службе речи

**Преобразование речи в текст** можно использовать в приложениях двумя разными способами.

| Метод | ОПИСАНИЕ |
|-|-|
| [Пакет SDK](speech-sdk.md) | Простейший способ для разработчиков C/C++, C# и Java * |
| [REST](rest-apis.md) | Распознавание коротких фрагментов речи с помощью запроса HTTP POST | 

\* *Пакет SDK для Java входит в [пакет SDK для речевых устройств](speech-devices-sdk.md).*

## <a name="using-the-sdk"></a>Использование пакета SDK

[Пакет SDK для службы "Речь"](speech-sdk.md) обеспечивает простой способ использования **преобразования речи в текст** в приложении с полным набором функций.

1. Создайте фабрику речи, указав ключ подписки службы речи или маркер авторизации. На этом этапе также можно настроить параметры, такие как язык распознавания или настраиваемая конечная точка для собственных моделей распознавания речи.

2. Получите распознаватель из фабрики. Доступно три типа распознавателя. Каждый тип распознавателя может использовать микрофон устройства по умолчанию, аудиопоток или звук из файла.

    Распознаватель | Функция
    -|-
    Распознаватель речи|Выполняет текстовую расшифровку речи.
    Распознаватель намерений|Получает намерение говорящего через службу [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) после распознавания\*.
    Распознаватель перевода|Переводит расшифрованный текст на другой языка (см. [Перевод речи](how-to-translate-speech.md)).

    \* *Для распознавания намерения необходимо использовать отдельный ключ подписки LUIS при создании фабрики речи для распознавателя намерений.*
    
4. При необходимости свяжите события для асинхронной операции. Распознаватель затем вызывает обработчики событий при получении промежуточного и конечного результатов. В противном случае приложение получает конечный результат расшифровки.

5. Запустите распознавание.

### <a name="sdk-samples"></a>Примеры пакета SDK

Актуальный набор примеров доступен в [репозитории GitHub с примерами для пакета SDK службы "Речь" в Cognitive Services](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Использование REST API

REST API является самым простым способом распознавания речи, если вы не используете язык, поддерживаемый пакетом SDK. Вы отправляете запрос HTTP POST к конечной точке службы, передавая весь фрагмент речи в теле запроса. Полученный ответ содержит распознанный текст.

> [!NOTE]
> При использовании интерфейса REST API ограничение длительности фрагмента речи составляет 15 секунд.


Дополнительные сведения об REST API **преобразования текста в речь** см. в разделе [REST APIs](rest-apis.md#speech-to-text) (Речевые службы REST API). Чтобы увидеть это в действии, загрузите [примеры REST API](https://github.com/Azure-Samples/SpeechToText-REST) из GitHub.

## <a name="next-steps"></a>Дополнительная информация

- [Получение пробной подписки на службу "Речь"](https://azure.microsoft.com/try/cognitive-services/)
- [Распознавание речи в C#](quickstart-csharp-windows.md)
