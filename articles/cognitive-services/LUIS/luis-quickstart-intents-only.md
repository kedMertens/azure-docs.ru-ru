---
title: Создание простого приложения с двумя намерения — Azure | Документы Майкрософт
description: В этом кратком руководстве вы узнаете, как создать простое приложение LUIS, использующее два намерения и не имеющее сущностей, для определения фраз пользователя.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 3f23ade2b0256c72c344e2a619227a79e3c79a47
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160121"
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

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="purpose-of-the-app"></a>Назначение приложения
Это приложение имеет несколько намерений. Первое намерение, **`GetJobInformation`**, определяет, когда пользователь хочет получить сведения о вакансиях в компании. Второе намерение, **`None`**, определяет все остальные типы фраз. Далее в кратком руководстве будет добавлено третье намерение — `ApplyForJob`. 

## <a name="create-a-new-app"></a>Создание нового приложения
1. Выполните вход на веб-сайте [LUIS](luis-reference-regions.md#luis-website). Войдите в [регион](luis-reference-regions.md#publishing-regions), где нужно опубликовать конечные точки LUIS.

2. На веб-сайте [LUIS](luis-reference-regions.md#luis-website) выберите **Создать приложение**.  

    [![](media/luis-quickstart-intents-only/app-list.png "Снимок экрана страницы \"Мои приложения\"")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Во всплывающем диалоговом окне введите имя `HumanResources`. В этом приложении рассматриваются вопросы об отделе кадров вашей компании. Этот отдел обрабатывает вопросы, связанные с трудоустройством, например, на какие должности требуются сотрудники.

    ![Новое приложение LUIS](./media/luis-quickstart-intents-only/create-app.png)

4. После завершения процесса появится страница **намерений** с намерением **None**. 

## <a name="create-getjobinformation-intention"></a>Создание намерения GetJobInformation
1. Выберите **Create new intent**. (Создать намерение). Введите имя нового намерения `GetJobInformation`. Это намерение предсказывается каждый раз, когда пользователь хочет получить сведения об открытых вакансиях в вашей компании.

    ![](media/luis-quickstart-intents-only/create-intent.png "Снимок экрана диалогового окна создания намерения")

    Создавая намерение, вы создаете категорию информации, которую хотите идентифицировать. Поскольку мы присваиваем категории имя, любое другое приложение, использующее результаты запроса LUIS, по этому имени может найти подходящий ответ. LUIS не отвечает на эти вопросы, а только определяет, какой тип информации запрашивается на естественном языке. 

2. Добавьте в намерение несколько фраз, которые вы ожидаете от пользователя, например:

    | Примеры высказываний|
    |--|
    |Сегодня опубликованы новые вакансии?|
    |Какие должности доступны для старших инженеров?|
    |Есть ли должность для работы с базами данных?|
    |Ищу новую работу с обязанностями в бухгалтерском учете|
    |Где находится список вакансий|
    |Новые вакансии?|
    |Есть вакансии в офисе в Сиэтле?|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Снимок экрана с вводом новых фраз для намерения MyStore")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

3. В настоящее время приложение LUIS не содержит фраз для намерения **None**. Ему нужны фразы, на которые приложение не отвечает. Не оставляйте это намерение пустым. Выберите **намерения** на панели слева. 

4. Выберите намерение **None**. Добавьте три фразы, которые может ввести пользователь, но которые не имеют отношения к приложению. Если приложение связано с объявлениями о вакансиями, вот несколько подходящих фраз **None**:

    | Примеры фраз|
    |--|
    |Громкий лай раздражает|
    |Закажи пиццу|
    |Пингвины в океане|

    В приложении, вызывающем LUIS (таком как чат-бот), если LUIS возвращает намерение **None** для фразы, бот может спросить, хочет ли пользователь завершить разговор. Чат-бот может также дать больше указаний для продолжения разговора, если пользователь хочет продолжить. 

## <a name="train-and-publish-the-app"></a>Обучение и публикация приложения

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Публикация приложения в конечной точке

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="query-endpoint-for-getjobinformation-intent"></a>Запрос к конечной точке для намерения GetJobInformation

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

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

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. В новом окне браузера введите `Can I submit my resume for job 235986` в конце URL-адреса. 

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

Результат JSON определяет намерение с высшим показателем. Все оценки находятся в диапазоне 0–1, наилучшие оценки — те, которые ближе к 1. Оценка намерений `GetJobInformation` и `None` гораздо ближе к нулю. 

## <a name="where-is-this-luis-data-used"></a>Место использования этих данных приложения LUIS 
Приложение LUIS уже выполнило этот запрос. Вызывающее приложение, например чат-бот, может взять результат topScoringIntent и найти сведения (не хранятся в LUIS) для ответа на вопрос или завершить диалог. Это программные варианты для бота или вызывающего приложения. LUIS не выполняет эту работу. LUIS только определяет намерение пользователя. 

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Добавление предварительно созданных намерений и сущностей к этому приложению](luis-tutorial-prebuilt-intents-entities.md)
