---
title: Основные сведения о службе "Речь" (предварительная версия)
description: 'Служба "Речь", входящая в набор служб Microsoft Cognitive Services, объединяет несколько служб речи Azure, которые ранее были доступны по отдельности: службу распознавания речи Bing (состоящую из распознавания речи и преобразования текста в речь), службу пользовательского распознавания речи и службу перевода речи.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: overview
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 922320bb0b880e933b27025257e6a533fe257680
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091478"
---
# <a name="what-is-the-speech-service"></a>Что собой представляет служба "Речь"

Служба "Речь" объединяет голосовые функции Azure, ранее доступные через [API распознавания речи Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home), [API перевода речи](https://docs.microsoft.com/azure/cognitive-services/translator-speech/), [Пользовательскую службу распознавания речи](https://docs.microsoft.com/azure/cognitive-services/custom-speech-service/cognitive-services-custom-speech-home) и [службу пользовательских голосовых моделей](http://customvoice.ai/). Теперь одна подписка предоставляет доступ ко всем этим возможностям.

Аналогично другим службам распознавания речи Azure служба "Речь" основана на проверенных технологиях распознавания речи, используемых в таких продуктах, как Кортана и Microsoft Office. Вы можете полагаться на качество результатов и надежность облака Azure.

> [!NOTE]
> Служба "Речь" в настоящее время доступна в виде общедоступной предварительной версии. На этой странице периодически публикуются обновления документации, новые примеры кода и многое другое.

## <a name="main-speech-service-functions"></a>Основные функции службы "Речь"

К основным функциям службы "Речь" относятся преобразование речи в текст (также называемое распознаванием или транскрибированием), преобразование текста в речь (синтез речи) и перевод речи.

|Функция|Функции|
|-|-|
|[Преобразование речи в текст](speech-to-text.md)| <ul><li>Расшифровывает непрерывную речь в режиме реального времени в текстовый формат.<li>Может выполнять пакетное транскрибирование аудиозаписей. <li>Предлагает режимы распознавания интерактивной речи, разговора или диктовки.<li>Поддерживает промежуточные результаты, обнаружение окончания речи, автоматическое форматирование текста и маскировку нецензурной лексики. <li>Можно вызвать в [Интеллектуальной службе распознавания речи](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), чтобы получить намерение пользователя из расшифрованной речи.\*|
|[Преобразование текста в речь](text-to-speech.md)| <ul><li>Преобразует текст в естественно звучащую речь. <li>Предлагает голоса обеих полов и диалекты для множества поддерживаемых языков. <li>Поддерживает ввод простого текста или на языке разметки синтеза речи (Speech Synthesis Markup Language, SSML). |
|[Перевод речи](speech-translation.md)| <ul><li>Преобразует потоковое аудио в режиме, близком к реальному времени.<li> Может также обрабатывать записанную речь.<li>Предоставляет результаты в виде текста или синтезированной речи. |

\* *Для распознавания намерений требуется подписка LUIS.*


## <a name="customizing-speech-features"></a>Настройка функций обработки речи

Служба "Речь" позволяет на базе собственных данных обучать модели, используемые такими функциями службы "Речь", как преобразование речи в текст и текста в речь. 

|Функция|Модель|Назначение|
|-|-|-|
|Преобразование речи в текст|[Акустическая модель](how-to-customize-acoustic-models.md)|Позволяет транскрибировать речь определенных говорящих и распознать речь в таких средах, как автомобили или фабрики|
||[Языковая модель](how-to-customize-language-model.md)|Позволяет транскрибировать речь с профильными терминами и грамматикой, например медицинский или ИТ-жаргон|
||[Модель произношения](how-to-customize-pronunciation.md)|Позволяет транскрибировать сокращения и акронимы, например "IOU" для "i oh you" |
|Преобразование текста в речь|[Настраиваемый голос](how-to-customize-voice-font.md)|Предоставляет приложению собственный голос, обучая модель на примерах человеческой речи.|

Созданные пользовательские модели можно использовать везде, где в функциях преобразования речи в текст и текст в речь применялись стандартные модели.


## <a name="using-the-speech-service"></a>Использование службы "Речь"

Чтобы упростить разработку приложений с поддержкой речевых функций, корпорация Майкрософт предоставляет [пакет SDK для службы "Речь"](speech-sdk.md), используемый с новой службой "Речь". Пакет SDK для службы "Речь" предоставляет согласованные собственные API для преобразования текста в речь и речи в текст для C#, C++ и Java. Если вы разрабатываете с помощью одного из этих языков, пакет SDK для службы "Речь" упрощает разработку, обрабатывая сетевые сведения.

Служба "Речь" также имеет [REST API](rest-apis.md), который работает с любым языком, поддерживающим HTTP-запросы. Тем не менее интерфейс REST не предоставляет функцию потоковой передачи в режиме реального времени для пакета SDK.

|<br>Метод|Речь<br>в текст|Текст в<br>Речь|Речь<br>Перевод|<br>ОПИСАНИЕ|
|-|-|-|-|-|
|[пакет SDK для службы "Речь"](speech-sdk.md);|Yes|Нет |Yes|Собственные API-интерфейсы для C#, C++ и Java для упрощения разработки.|
|[REST](rest-apis.md)|Yes|Да|Нет |Простой API на основе HTTP, который упрощает добавление речи в приложения.|

### <a name="websockets"></a>Подключения WebSocket

Служба "Речь" также имеет протоколы WebSocket для потоковой передачи преобразования речи в текст и перевода речи. Пакеты SDK для службы "Речь" используют эти протоколы для обмена данными со службой. Следует использовать пакет SDK для службы "Речь", а не пытаться реализовать собственный обмен данными со службой по протоколу WebSocket.

При наличии кода, в котором запрограммировано использование API распознавания речи Bing или API перевода речи через подключение WebSocket, нужно просто обновить его для использования службы "Речь". Протоколы WebSocket совместимы. Отличаются только конечные точки.

### <a name="speech-devices-sdk"></a>Пакет SDK для речевых устройств

[Пакет SDK для речевых устройств](speech-devices-sdk.md) — это интегрированная программная и аппаратная платформа для разработчиков устройств с поддержкой речи. Наш поставщик оборудования предоставляет примеры структуры и единицы разработки. Корпорация Майкрософт предоставляет оптимизированный под устройство пакет SDK, который использует все преимущества аппаратных возможностей.


## <a name="speech-scenarios"></a>Сценарии службы "Речь"

Варианты использования службы "Речь" включают:

> [!div class="checklist"]
> * Создание активируемых голосом приложений
> * Транскрибирование записей звонков в колл-центр
> * Реализация голосовых ботов

### <a name="voice-user-interface"></a>Голосовой пользовательский интерфейс

Голосовой ввод — это отличный способ сделать приложение гибким, автоматизированным и легким в использовании. В приложении с голосовым управлением пользователи могут просто запрашивать необходимую информацию без необходимости переходить к ней.

Если приложение предназначено для общего доступа, можно использовать модели распознавания голоса по умолчанию. Они хорошо распознают голоса широкого спектра говорящих в общих средах.

Если ваше приложение будет использоваться в определенной предметной области (например, медицина или ИТ), можно создать [языковую модель](how-to-customize-language-model.md), чтобы научить службу "Речь" специальной терминологии, используемой приложением.

Если приложение будет использоваться в шумной среде, например, на производстве, можно создать настраиваемую [звуковую модель](how-to-customize-acoustic-models.md), чтобы помочь службе "Речь" лучше различать голос и шум.

Чтобы приступить к работе, достаточно просто скачать [пакет SDK для службы "Речь"](speech-sdk.md) и следовать соответствующему [краткому руководству](quickstart-csharp-dotnet-windows.md).

### <a name="call-center-transcription"></a>Транскрибирование звонков в колл-центр

Зачастую к записям звонков в колл-центр обращаются только при возникновении проблемы со звонком. С помощью службы "Речь" можно легко преобразовать любую запись в текст. Как только они будут преобразованы в текст, можно легко проиндексировать их для [полнотекстового поиска](https://docs.microsoft.com/azure/search/search-what-is-azure-search) или применить [анализ текста](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/), чтобы обнаружить тональность, язык и ключевые фразы.

Если в записях звонков в колл-центр часто наблюдается специализированная терминология (такая, как имена продуктов или ИТ-жаргон), можно создать [языковую модель](how-to-customize-language-model.md), чтобы научить службу "Речь" этому словарю. Настраиваемая [звуковая модель](how-to-customize-acoustic-models.md) может помочь службе "Речь" понять неоптимальные телефонные соединения.

Чтобы получить дополнительные сведения об этом сценарии, узнайте больше о [пакетном транскрибировании](batch-transcription.md) с помощью службы "Речь".

### <a name="voice-bots"></a>Голосовые боты

[Чат-боты](https://dev.botframework.com/) — это все более популярный способ предоставления пользователям необходимой информации и помощи клиентам в подборе нужных продуктов и услуг. Добавление пользовательского интерфейса для общения на свой веб-сайт или в приложение упрощает поиск и ускоряет доступ к функциям. С помощью службы "Речь" этот диалог приобретает новую степень глубины благодаря ответу на разговорные запросы с использованием аналогичной терминологии.

Чтобы сделать голос своего бота уникальным (и укрепить свой бренд), ему можно дать собственный голос. Создание пользовательского голоса — это двухэтапный процесс. Сначала вы [записываете](record-custom-voice-samples.md) голос, который хотите использовать. Затем эти [записи отправляются](how-to-customize-voice-font.md) (вместе с транскрипцией) на [портал настройки голоса](https://cris.ai/Home/CustomVoice) службы "Речь", который делает все остальное. Созданный пользовательский голос довольно просто использовать в приложении.

## <a name="next-steps"></a>Дополнительная информация

Получите ключ подписки для службы "Речь".

> [!div class="nextstepaction"]
> [Попробуйте службу распознавания речи бесплатно](get-started.md)
