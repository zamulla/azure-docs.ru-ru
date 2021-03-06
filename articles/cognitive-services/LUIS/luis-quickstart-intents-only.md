---
title: Создание простого приложения с двумя намерения — Azure | Документы Майкрософт
description: В этом кратком руководстве вы узнаете, как создать простое приложение LUIS, использующее два намерения и не имеющее сущностей, для определения фраз пользователя.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 06/27/2018
ms.author: v-geberr
ms.openlocfilehash: fea311eb36c0654b45b2354cb283f1699be7257c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109347"
---
# <a name="tutorial-1-build-app-with-custom-domain"></a>Руководство: 1. Создание приложения с личным доменом
В этом руководстве мы создаем приложение, которое демонстрирует, как использовать **намерения** для определения _намерений_ пользователей на основе фраз (текста), которые они отправляют в приложение. В итоге вы получите конечную точку LUIS, работающую в облаке.

Это простейший тип приложения LUIS, поскольку оно не извлекает данные из фразы. Оно определяет только намерение пользователя во фразе.

<!-- green checkmark -->
> [!div class="checklist"]
> * Создание приложения для домена отдела кадров 
> * Добавление намерения GetJobInformation
> * Добавление примера фраз в намерение GetJobInformation 
> * Тестирование и публикация приложения.
> * Запрос конечной точки приложения для просмотра ответа JSON LUIS.
> * Добавление намерения ApplyForJob
> * Добавление примера фраз в намерение ApplyForJob 
> * Обучение, публикация и повторный запрос конечной точки 

Для работы с этой статьей требуется бесплатная учетная запись [LUIS](luis-reference-regions.md#luis-website), в которой вы создадите приложение LUIS.

## <a name="purpose-of-the-app"></a>Назначение приложения
Это приложение имеет несколько намерений. Первое намерение, **`GetJobInformation`**, определяет, когда пользователь хочет получить сведения о вакансиях в компании. Второе намерение, **`None`**, определяет все остальные типы фраз. Далее в кратком руководстве будет добавлено третье намерение — `ApplyForJob`. 

## <a name="create-a-new-app"></a>Создание нового приложения
1. Выполните вход на веб-сайте [LUIS](luis-reference-regions.md#luis-website). Войдите в [регион](luis-reference-regions.md#publishing-regions), где нужно опубликовать конечные точки LUIS.

2. На веб-сайте [LUIS](luis-reference-regions.md#luis-website) выберите **Создать приложение**.  

    [![](media/luis-quickstart-intents-only/app-list.png "Снимок экрана страницы \"Мои приложения\"")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Во всплывающем диалоговом окне введите имя `HumanResources`. В этом приложении рассматриваются вопросы об отделе кадров вашей компании. Этот отдел обрабатывает вопросы, связанные с трудоустройством, например, на какие должности требуются сотрудники.

    ![Новое приложение LUIS](./media/luis-quickstart-intents-only/create-app.png)

4. После завершения процесса появится страница **намерений** с намерением **None**. 

    [![](media/luis-quickstart-intents-only/intents-list.png "Снимок экрана страницы списка намерений")](media/luis-quickstart-intents-only/intents-list.png#lightbox)

## <a name="create-getjobinformation-intention"></a>Создание намерения GetJobInformation
1. Выберите **Create new intent**. (Создать намерение). Введите имя нового намерения `GetJobInformation`. Это намерение предсказывается каждый раз, когда пользователь хочет получить сведения об открытых вакансиях в вашей компании.

    ![](media/luis-quickstart-intents-only/create-intent.png "Снимок экрана диалогового окна создания намерения")

    Создавая намерение, вы создаете категорию информации, которую хотите идентифицировать. Поскольку мы присваиваем категории имя, любое другое приложение, использующее результаты запроса LUIS, по этому имени может найти подходящий ответ. LUIS не отвечает на эти вопросы, а только определяет, какой тип информации запрашивается на естественном языке. 

2. Добавьте в намерение несколько фраз, которые вы ожидаете от пользователя, например:

    | Примеры фраз|
    |--|
    |Сегодня опубликованы новые вакансии?|
    |Какие должности доступны для старших инженеров?|
    |Есть ли должность для работы с базами данных?|
    |Ищу новую работу с обязанностями в бухгалтерском учете|
    |Где находится список вакансий|
    |Новые вакансии?|
    |Есть вакансии в офисе в Сиэтле?|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Снимок экрана с вводом новых фраз для намерения MyStore")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

3. В настоящее время приложение LUIS не содержит фраз для намерения **None**. Ему нужны фразы, на которые приложение не отвечает. Заполните его. Выберите **намерения** на панели слева. 

4. Выберите намерение **None**. Добавьте три фразы, которые может ввести пользователь, но которые не имеют отношения к приложению. Если приложение связано с объявлениями о вакансиями, вот несколько подходящих фраз **None**:

    | Примеры фраз|
    |--|
    |Громкий лай раздражает|
    |Закажи пиццу|
    |Пингвины в океане|

    В приложении, вызывающем LUIS (таком как чат-бот), если LUIS возвращает намерение **None** для фразы, бот может спросить, хочет ли пользователь завершить разговор. Чат-бот может также дать больше указаний для продолжения разговора, если пользователь хочет продолжить. 

## <a name="train-and-publish-the-app"></a>Обучение и публикация приложения
1. В верхней правой части веб-сайта LUIS нажмите кнопку **Train** (Обучить). 

    ![Кнопка обучения](./media/luis-quickstart-intents-only/train-button.png)

    Когда обучение будет завершено, в верхней части веб-сайта появится зеленая панель состояния, свидетельствующая об успешном результате.

    ![Строка состояния "Обучение пройдено"](./media/luis-quickstart-intents-only/trained.png)

2. В верхней правой части веб-сайта LUIS нажмите кнопку **Publish** (Опубликовать), чтобы открыть страницу публикации. По умолчанию выбран рабочий слот. Если вам нужен этот слот, нажмите кнопку **Publish** (Опубликовать). Когда публикация будет завершена, в верхней части веб-сайта появится зеленая панель состояния, свидетельствующая об успешном результате.

    Перед публикацией или перед тестированием URL-адреса конечной точки не требуется создавать ключ конечной точки LUIS на портале Azure. Каждое приложение LUIS имеет бесплатный начальный ключ для разработки. Он предоставляет неограниченное число обращений для разработки и [несколько для конечных точек](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-for-getjobinformation-intent"></a>Запрос к конечной точке для намерения GetJobInformation
1. В нижней части страницы **публикации** выберите ссылку на **конечную точку**. В результате откроется другое окно браузера с URL-адресом конечной точки в адресной строке. 

2. Перейдите в конец URL-адреса и введите `I'm looking for a job with Natual Language Processing`. Последний параметр строки запроса — `q`. Это **запрос** фразы. Эта фраза не совпадает с фразами из примера в шаге 4, поэтому она является хорошим тестом. В результате должно быть возвращено намерение `GetJobInformation` как самое подходящее. 

    ```
    {
      "query": "I'm looking for a job with Natual Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

## <a name="create-applyforjob-intention"></a>Создание намерения ApplyForJob
Вернитесь на вкладку браузера с веб-сайтом LUIS и создайте новое намерение для подачи заявки.

1. Выберите **Создать** в меню в правом верхнем углу, чтобы вернуться к созданию приложения.

2. Выберите **намерения** в меню слева.

3. Выберите **Создать намерение** и введите имя `ApplyForJob`. 

    ![Диалоговое окно LUIS для создания намерения](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Добавьте в намерение несколько фраз, которые вы ожидаете от пользователя, например:

    | Примеры фраз|
    |--|
    |Я хочу подать заявку на вакансию в бухгалтерии|
    |Заполнить заявку для вакансии 123456|
    |Отправить резюме на должность инженера|
    |Это мое резюме должность 654234|
    |Должность 567890 и мои документы|

    [![](media/luis-quickstart-intents-only/utterance-applyforjob.png "Снимок экрана с вводом новых фраз для намерения ApplyForJob")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    Помеченное намерение выделено красным цветом, так как в настоящее время приложение LUIS не уверено в правильности намерения. При обучении приложение сообщает LUIS, что эти фразы выражают верное намерение. 

    Повторное [обучение и публикация](#train-and-publish-the-app). 

## <a name="query-endpoint-for-applyforjob-intent"></a>Запрос к конечной точке для намерения ApplyForJob
В нижней части страницы **публикации** выберите ссылку на **конечную точку**. В новом окне браузера введите `Can I submit my resume for job 235986` в конце URL-адреса. 

    ```
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

## <a name="what-has-this-luis-app-accomplished"></a>Результаты работы этого приложения LUIS
Это приложение с помощью всего нескольких намерений распознало запрос на естественном языке, который имеет то же намерение, но сформулирован иначе. 

Результат JSON определяет намерение с высшим показателем. Все оценки находятся в диапазоне 0–1, наилучшие оценки — те, которые ближе к 1. Оценка намерений `GetJobInformation` и `None` гораздо ближе к нулю. 

## <a name="where-is-this-luis-data-used"></a>Место использования этих данных приложения LUIS 
Приложение LUIS уже выполнило этот запрос. Вызывающее приложение, например чат-бот, может взять результат topScoringIntent и найти сведения (не хранятся в LUIS) для ответа на вопрос или завершить диалог. Это программные варианты для бота или вызывающего приложения. LUIS не выполняет эту работу. LUIS только определяет намерение пользователя. 

## <a name="clean-up-resources"></a>Очистка ресурсов
Удалите приложение LUIS, если оно больше не нужно. Для этого выберите **Мои приложения** в верхнем меню слева. Щелкните меню с тремя точками (…) справа от имени приложения в списке и выберите пункт **Удалить**. Во всплывающем диалоговом окне **Delete app?** (Удалить приложение?) нажмите кнопку **ОК**.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Добавление предварительно созданных намерений и сущностей к этому приложению](luis-tutorial-prebuilt-intents-entities.md)
