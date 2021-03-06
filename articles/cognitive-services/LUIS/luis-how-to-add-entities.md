---
title: Добавление сущностей в приложения LUIS | Документация Майкрософт
titleSuffix: Azure
description: Добавление сущностей (ключевые данные в предметной области приложения) в приложения LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: e97f9a5391799849983bd98db5400e0a842627b7
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224132"
---
# <a name="manage-entities"></a>Управление сущностями
После определения [намерений](luis-concept-intent.md) приложения, необходимо [отметить пример выражений](luis-concept-utterance.md) с помощью [сущностей](luis-concept-entity-types.md). Сущности — важные части команды или запроса и могут быть необходимыми для выполнения задачи клиентского приложения. 

Добавлять, изменять или удалять сущности в приложении можно с помощью **списка сущностей** на странице **Сущности**. Интеллектуальная служба распознавания речи предоставляет два основных типа сущностей: [предварительно созданные сущности](luis-reference-prebuilt-entities.md) и собственные пользовательские сущности.

В разделе **Сборка** доступны следующие разделы только внутри приложения LUIS. Ссылка **Сборка** находится на верхней панели навигации. В разделе **Сборка** выберите **Сущности** в меню навигации слева. Если сущность прошла машинное обучение, после добавления ее в приложение можно [отметить ее](luis-how-to-add-example-utterances.md) внутри выражения. После обучения и публикации приложения можно получить [извлеченные](luis-concept-data-extraction.md) данные сущности из прогноза. 

## <a name="add-prebuilt-entity"></a>Добавление предварительно созданной сущности
Предварительно созданные сущности определены в проекте [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Общие предварительно созданные сущности, добавляемые в приложение: *number* и *datetimeV2*. 

1. Зайдите в раздел приложения **Сборка**, а затем щелкните в левой панели **Сущности**.
 
2. На странице **Сущности** выберите **Управление предварительно созданными сущностями**.

    ![Снимок добавления предварительно созданных сущностей на странице "Сущности"](./media/add-entities/manage-prebuilt-entities-button.png)

3. В диалоговом окне **Добавление или удаление предварительно созданных сущностей** выберите предварительно созданные сущности **number** и **datetimeV2**. Затем выберите **Готово**.

    ![Снимок экрана диалогового окна "Добавление предварительно созданной сущности"](./media/add-entities/list-of-prebuilt-entities.png)

    Дополнительные сведения об извлечении предварительно созданной сущности из ответа на запрос JSON конечной точки см. в разделе [Извлечение данных](luis-concept-data-extraction.md#prebuilt-entity-data).

## <a name="add-simple-entities"></a>Добавление простой сущности
Простая сущность — это универсальная сущность, описывающая одно понятие. 

1. Войдите в раздел приложения **Сборка**, щелкните в левой панели **Сущности**, а затем выберите **Создать сущность**.

    ![Снимок страницы сущностей с выделенной кнопкой "Создать сущность"](./media/add-entities/create-new-entity-button.png)

2. Во всплывающем диалоговом окне введите `Airline` в поле **имя сущности**, выберите **Простая** в списке **Тип сущности**, а затем выберите **Готово**.

    ![Снимок диалогового окна для создания простой сущности авиалинии](./media/add-entities/create-simple-airline-entity.png)

    Дополнительные сведения об извлечении простой сущности из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#simple-entity-data). Дополнительные сведения об использовании простой сущности см. в соответствующем [кратком руководстве](luis-quickstart-primary-and-secondary-data.md).

## <a name="add-regular-expression-entities"></a>Добавление сущностей регулярного выражения
Сущность регулярного выражения используется для извлечения данных из выражения на основе указанного регулярного выражения. 

1. В левой области навигации приложения выберите **Сущности**, а затем выберите **Создать сущность**.

2. Во всплывающем диалоговом окне введите `AirFrance Flight` в поле **имя сущности**, выберите **Регулярное выражение** в списке **Тип сущности**, введите регулярное выражение `AFR[0-9]{3,4}`, а затем выберите **Готово**. 

    Это регулярное выражение рейсов компании AirFrance должно включать три символа: литерал `AFR`, затем 3 или 4 цифры. Цифрами может быть любое число от 0 до 9. Регулярное выражение соответствует цифрам рейсов AirFrance, такие как: "AFR101", "ARF1302" и "AFR5006". Дополнительные сведения об извлечении сущности из ответа на запрос JSON конечной точки см. в разделе [Извлечение данных](luis-concept-data-extraction.md).

    ![Снимок диалогового окна для создания сущности регулярного выражения](./media/add-entities/regex-entity-create-dialog.png)

    Дополнительные сведения об извлечении сущности регулярных выражений из ответа на запрос JSON конечной точки см. в разделе [Извлечение данных](luis-concept-data-extraction.md#regular-expression-entity-data). Дополнительные сведения об использовании сущности регулярного выражения см. в разделе [Руководство: Добавление сущности регулярного выражения](luis-quickstart-intents-regex-entity.md).

## <a name="add-hierarchical-entities"></a>Добавление иерархических сущностей
Иерархическая сущность — это категория контекстно-зависимых обученных и концептуально связанных сущностей. В следующем примере сущность содержит исходное и конечное расположения. 

В высказывании `Book 2 tickets from Seattle to Cairo` "Сиэтл" является исходным, а "Каир" — конечным расположением. Каждое расположение контекстуально отличается и получается на основе порядка и выбора слов в высказывании.

Чтобы добавить иерархические сущности, выполните следующие действия. 

1. В левой области навигации приложения выберите **Сущности**, а затем выберите **Создать сущность**.

2. Во всплывающем диалоговом окне введите `Location` в поле **Имя сущности**, выберите **Иерархическая** в списке **Тип сущности**.

    ![Добавление иерархической сущности](./media/add-entities/hier-location-entity-creation.png)

3. Выберите **Добавить дочерний элемент**, а затем введите "Исходный" в поле **Дочерний №1**. 

4. Выберите **Добавить дочерний элемент**, а затем введите "Назначение" в поле **Дочерний №2**. Нажмите кнопку **Готово**.
5. 
    >[!NOTE]
    >Чтобы удалить дочерний элемент, выберите значок корзины рядом с ним.

    >[!CAUTION]
    >Имена дочерних сущностей должны быть уникальными для всех сущностей в одном приложении. Две разные иерархические сущности не могут содержать дочерние элементы с тем же именем. 

    Дополнительные сведения об извлечении иерархической сущности из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#hierarchical-entity-data). Дополнительные сведения об использовании иерархической сущности см. в соответствующем [кратком руководстве](luis-quickstart-intent-and-hier-entity.md).

## <a name="add-composite-entities"></a>Добавление составных сущностей
Создавая составную сущность, можно определить связь между несколькими существующими сущностями. В следующем примере сущность содержит несколько билетов, исходное и конечное расположения. 

В высказывании `Book 2 tickets from Seattle to Cairo` число 2 соответствует предварительно созданной сущности,"Сиэтл" является исходным, а "Каир" — конечным расположением. После создания составной сущности каждая сущность является частью большей родительской сущности.

1. В приложении добавьте предварительно созданную сущность **number**. Инструкции см. в разделе [Добавление предварительно созданных сущностей](#add-prebuilt-entity). 

2. Добавление иерархической сущности `Location`, включая подтипы: `origin`, `destination`. Дополнительные инструкции см. в разделе [Добавление иерархических сущностей](#add-hierarchical-entities). 

3. В левой области навигации выберите **Сущности**, а затем выберите **Создать сущность**.

4. Во всплывающем диалоговом окне введите `TicketsOrder` в поле **Имя сущности**, выберите **Составная** в списке **Тип сущности**.

5. Выберите **Добавить дочерний элемент** для добавления нового дочернего элемента.

6. В **Дочерний №1** выберите сущность **number** из списка.

7. В **Дочерний №2** выберите сущность **Location::Origin** из списка. 

8. В **Дочерний №3** выберите сущность **Location::Destination** из списка. 

9. Нажмите кнопку **Готово**.

    ![Снимок диалогового окна для создания составной сущности](./media/add-entities/ticketsorder-composite-entity.png)

    >[!NOTE]
    >Чтобы удалить дочерний элемент, выберите корзину рядом с ним.

    Дополнительные сведения об извлечении составной сущности из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#composite-entity-data). Дополнительные сведения об использовании составной сущности см. в соответствующем [кратком руководстве](luis-tutorial-composite-entity.md).


## <a name="add-patternany-entities"></a>Добавление сущностей Pattern.any
Сущности [Pattern.any](luis-concept-entity-types.md) допустимы только в [шаблонах](luis-how-to-model-intent-pattern.md). Эта сущность помогает Интеллектуальной службе распознавания речи найти конец сущностей различной длины и выбора слов. Так как эта сущность используется в шаблоне, Интеллектуальная служба распознавания речи знает, где конец сущности в высказывании.

Если приложение имеет намерение `FindBookInfo`, название книги может конфликтовать с намерением прогнозирования. Чтобы уточнить, какие машинные слова находятся в названии книги, используйте Pattern.any в шаблоне. Прогнозирование LUIS начинается с высказывания. Сначала высказывание проверяется и сопоставляется сущностям при их обнаружении, а затем шаблон проверяется и сопоставляется. 

В высказывании `Who wrote the book Ask and when was it published?` название книги "Ask" является каверзным, потому что оно не является контекстно-зависимым, и не понятно, когда заканчивается заголовок и где начинается остальная часть высказывания. В названиях книг порядок слов может быть любым, они могут состоять из одного слова, сложных фраз со знаками препинания и иметь бессмысленный порядок слов. Шаблон позволяет создать сущность, в которой можно извлечь полную и точную сущность. После обнаружения названия книги намерение `FindBookInfo` прогнозированное, потому что это намерение для шаблона.

1. Войдите в раздел приложения **Сборка**, щелкните в левой панели **Сущности**, а затем выберите **Создать сущность**.

2. В диалоговом окне**Добавление сущности** введите `BookTitle` в поле **Имя сущности** и выберите **Pattern.any** как **Тип сущности**.
 
    ![Снимок создания сущности Pattern.any](./media/add-entities/create-pattern-any-entity.png)

    Чтобы использовать сущность Pattern.any, добавьте шаблон на странице **Шаблоны** (в разделе **Повышение производительности приложения**) с помощью правильного синтаксиса фигурных скобок, например "For **{BookTitle}** who is the author?".

    Дополнительные сведения об извлечении сущности Pattern.any из ответа на запрос JSON конечной точки см. в разделе [Извлечение данных](luis-concept-data-extraction.md#patternany-entity-data). Используйте руководство [по шаблонам](luis-tutorial-pattern.md) для получения дополнительных сведений о том, как использовать сущность Pattern.any.

Если ваш шаблон, который включает сущность Pattern.any, извлекает сущности неправильно, используйте [явный список](luis-concept-patterns.md#explicit-lists), чтобы решить эту проблему. 

## <a name="add-role-to-pattern-based-entity"></a>Добавление роли к сущности на основе шаблонов
Роль — именованный подтип сущности на основе контекста. Ее можно сравнить с [иерархической](#add-hierarchical-entities) сущностью, но роли используются только в [шаблонах](luis-how-to-model-intent-pattern.md). 

Например, авиабилет имеет *город отправления* и *город назначения*, но оба — это города. Интеллектуальная служба распознавания речи определяет эти два города отправления и назначения на основе контекста порядка и выбора машинных слов. 

Синтаксис для роли **{Имя сущности: Имя роли}**, где имя сущности находится перед двоеточием, а имя роли за ним. Например "Забронировать билет из {Расположение: Отправление} {Расположение: Назначение}".

1. Войдите в раздел приложения **Сборка**, а затем выберите на левой панели **Сущности**.

2. Нажмите кнопку **Создать сущность**. Укажите имя `Location`. Выберите тип **Простая** и выберите **Готово**

3. На левой панели выберите **Сущности**, затем выберите **Расположение** новой сущности, созданное на шаге 2.

4. В текстовом поле **Имя роли** введите имя роли `Origin` и нажмите клавишу ВВОД. Добавьте имя второй роли `Destination`. Например, авиаперелет может иметь город отправления и назначения. Есть две роли: "Отправление" и "Назначение".

    ![Снимок экрана добавления роли отправления сущности Location](./media/add-entities/roles-enter-role-name-text.png)

    Дополнительные сведения об извлечении ролей из ответа на запрос JSON конечной точки см. в разделе [Извлечение данных](luis-concept-data-extraction.md). Используйте руководство по шаблонам для получения дополнительных сведений о том, как использовать сущность Pattern.any.

## <a name="add-list-entities"></a>Добавление сущностей списка
Сущности списка представляют собой фиксированный, закрытый набор связанных слов в системе. 

Для сущности списка напитков можно установить два нормализованных значения: вода и содовая. Каждое нормализованное название имеет синонимы. Синонимы к слову "вода": H20, газировка, простая вода. Синонимы к слову "содовая": соковая вода, кола, имбирная вода. При создании сущности не нужно знать все значения. Вы можете добавить больше значений после просмотра высказываний реального пользователя с синонимами.

|Нормализованное название|синонимы;|
|--|--|
|Вода|H20, газировка, простая вода|
|Содовая|Соковая вода, кола, имбирная вода|

1. Войдите в раздел приложения **Сборка**, щелкните в левой панели **Сущности**, а затем выберите **Создать сущность**.

2. В диалоговом окне**Добавление сущности** введите `Drinks` в поле **Имя сущности** и выберите **Список** как **Тип сущности**. Нажмите кнопку **Готово**.
 
    ![Изображение диалогового окна создания сущностей списка напитков](./media/add-entities/menu-list-dialog.png)
  
3.  На странице "Сущность списка" можно добавлять нормализованные имена. В текстовом поле **Значения** введите элемент списка, например, `Water` в списке напитков, а затем нажмите клавишу ВВОД на клавиатуре. 

    ![Снимок сущности списка напитков с нормализованным значением воды в текстовом поле](./media/add-entities/entity-list-normalized-name.png)

4. Справа от нормализованного значения **Вода** введите синонимы `h20`, `flat`, и `gas`, нажав клавишу ВВОД на клавиатуре после каждого элемента.

    ![Снимок сущности списка напитков с выделенными элементами синонимов для слова "вода"](./media/add-entities/menu-list-synonyms.png)

5. Если необходимо добавить нормализованные элементы в список, выберите **Рекомендуемые**, чтобы просмотреть параметры в [семантическом словаре](luis-glossary.md#semantic-dictionary).

    ![Снимок сущности списка напитков с выделенными рекомендуемыми элементами](./media/add-entities/entity-list-recommended-list.png)

6. Выберите элемент в списке "Рекомендуемые", чтобы добавить его в качестве нормализованного значения или выберите **Добавить все**, чтобы добавить все элементы. 

    ![Снимок сущности списка напитков с рекомендуемыми элементами пива и выделенной кнопкой "Добавить все"](./media/add-entities/list-entity-add-suggestions.png)

    Дополнительные сведения об извлечении сущностей списков из ответа на запрос JSON конечной точки см. в [этом разделе](luis-concept-data-extraction.md#list-entity-data). Дополнительные сведения об использовании сущности списка см. в соответствующем [кратком руководстве](luis-quickstart-intent-and-list-entity.md).

## <a name="import-list-entity-values"></a>Импорт значений сущности списка
Можно импортировать значения в существующую сущность списка.

 1. На странице "Сущность списка" выберите **Импорт списков**.

 2. В диалоговом окне **Import New Entries** (Импорт новых записей) нажмите **Выбрать файл** и выберите JSON-файл, который содержит список.

    ![Снимок всплывающего диалогового окна "Импорт значений сущности списка"](./media/add-entities/menu-list-import-json-dialog-with-file.png)

    >[!NOTE]
    >Интеллектуальная служба распознавания речи импортирует файлы только с расширением ".json".

 3. Чтобы узнать о синтаксисе поддерживаемого списка в JSON, см. раздел **о синтаксисе поддерживаемого списка**, чтобы развернуть диалоговое окно и отобразить пример допустимого синтаксиса. Чтобы свернуть диалоговое окно и скрыть синтаксис, выберите повторно заголовок ссылки.

 4. Нажмите кнопку **Готово**.

    Пример допустимых данных JSON для сущности списка **Colors** отображается в следующем коде формата JSON.

    ```
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```

## <a name="edit-entity-name"></a>Изменение имени сущности
1. На странице списка **Сущности** выберите сущность в списке. Это действие вернет вас на страницу **Сущность**.

2. На странице **Сущность** можно изменить имя сущности, выбрав значок "Правка" рядом с именем сущности. Тип сущности не изменяется. 

## <a name="delete-entity"></a>Удаление сущности

На странице **Сущность** выберите кнопку **Удалить сущность**. Выберите **OК** в окне подтверждения, чтобы подтвердить удаление.
 
![Снимок страницы сущности Location с выделенной кнопкой "Удалить сущность"](./media/add-entities/entity-delete.png)

>[!NOTE]
>* При удалении иерархической сущности удаляются все ее дочерние элементы.
>* При удалении составной сущности удаляется только составная сущность и разрываются составные связи, но сущность, сформировавшая их, не удаляется.

## <a name="changing-entity-type"></a>Изменение типа сущности
Интеллектуальная служба распознавания речи не позволяет изменить тип сущности, поскольку неизвестно, что добавлять или удалять для создания этой сущности. Чтобы изменить тип, лучше создать новую сущность правильного типа с незначительно отличающимся именем. После создания сущности в каждом высказывании удалите старое отмеченное имя сущности и добавьте новое. После того как все высказывания будут повторно отмечены, удалите старую сущность. 

## <a name="create-a-pattern-from-an-utterance"></a>Создание шаблона на основе высказывания
Дополнительные сведения см. в разделе [Добавление шаблона на основе существующего фрагмента речи на странице сущности или намерения](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="search-utterances"></a>Поиск высказываний
Выполнять поиск и фильтрацию высказываний можно с помощью значка лупы на панели инструментов. 

## <a name="train-your-app-after-changing-model-with-entities"></a>Обучение приложения после изменения модели с сущностями
После добавления, изменения и удаления сущностей выполните [обучение](luis-how-to-train.md) и [публикацию](luis-how-to-publish-app.md) приложения, чтобы применить изменения к запросам конечной точки. 

## <a name="next-steps"></a>Дополнительная информация
Теперь, когда добавлены намерения, выражения и сущности, вы получаете базовое приложение LUIS. Узнайте, как [обучать](luis-how-to-train.md), [тестировать](luis-interactive-test.md) и [опубликовать](luis-how-to-publish-app.md) приложение.
 
