---
title: Использование пользовательской конечной точки распознавания речи с помощью Пользовательской службы распознавания речи в Azure | Документация Майкрософт
description: Узнайте, как использовать пользовательскую конечную точку преобразования речи в текст с помощью Пользовательской службы распознавания речи в Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 55583952df3b83331f1f622a4fce269713ecf2a6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966526"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Использование пользовательской конечной точки преобразования речи в текст
Запросы к пользовательской конечной точке преобразования речи в текст Azure можно отправлять точно так же, как к используемой по умолчанию конечной точке распознавания речи Cognitive Services. Эти конечные точки функционально идентичны конечным точкам SAPI. Поэтому функциональные возможности, предоставляемые SAPI посредством клиентской библиотеки или REST API, доступны и для вашей пользовательской конечной точки.

Конечные точки, созданные с помощью этой службы, могут обрабатывать различное число параллельных запросов. Этот объем зависит от ценовой категории, связанной с вашей подпиской. Если получено слишком много запросов, возникает ошибка. Для уровня "Бесплатный" действует месячный лимит запросов.

Служба предполагает, что данные передаются в режиме реального времени. Если они отправляются быстрее, то запрос считается выполняющимся, пока не истечет длительность его звука в режиме реального времени.

> [!NOTE]
> Мы еще поддерживаем [новые веб-сокеты](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol). Если вы планируете использовать веб-сокеты с пользовательской конечной точкой распознавания речи, следуйте приведенным далее инструкциям.
>
> Поддержка нового [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) ожидается в ближайшее время. Если вы планируете вызывать пользовательскую конечную точку распознавания речи по протоколу HTTP, следуйте приведенным далее инструкциям.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Отправка запросов с помощью клиентской библиотеки распознавания речи

Чтобы отправлять запросы к пользовательской конечной точке с помощью клиентской библиотеки распознавания речи, запустите клиент распознавания. Используйте пакет SDK распознавания речи для клиента из [NuGet](http://nuget.org/). Выполните поиск *speech recognition* и выберите пакет распознавания речи от корпорации Майкрософт для своей платформы. Примеры исходного кода можно найти на сайте [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). Пакет SDK распознавания речи для клиента предоставляет класс фабрики **SpeechRecognitionServiceFactory**, который предоставляет следующие методы.

  *   ```CreateDataClient(...)```: клиент распознавания данных.
  *   ```CreateDataClientWithIntent(...)```: клиент распознавания данных с намерением.
  *   ```CreateMicrophoneClient(...)```: клиент распознавания с поддержкой микрофона.
  *   ```CreateMicrophoneClientWithIntent(...)```: клиент распознавания с поддержкой микрофона с намерением.

Подробное описание представлено в документации по [API распознавания речи Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home). Конечные точки Пользовательской службы распознавания речи поддерживают тот же пакет SDK.

Клиент распознавания данных подходит для распознавания речи на основе данных, например файла или другого источника звука. Клиент распознавания с поддержкой микрофона подходит для распознавания речи через микрофон. Использование намерения в любом из клиентов позволяет возвращать структурированные данные намерений из [Интеллектуальной службы распознавания речи](https://www.luis.ai/) (LUIS), если для вашего сценария создается приложение LUIS.

Клиенты всех четырех типов могут быть созданы двумя способами. В первом способе используется стандартный SAPI Cognitive Services. Второй способ позволяет указать URL-адрес, соответствующий вашей пользовательской конечной точке, созданной с помощью Пользовательской службы распознавания речи.

Например, можно создать клиент **DataRecognitionClient**, который отправляет запросы к пользовательской конечной точке, используя следующий метод.

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

Параметры **your_subscriptionId** и **endpointURL** — это ключ подписки и URL-адрес веб-сокета соответственно (см. страницу **Сведения о развертывании**).

**AuthenticationUri** используется для получения маркера от службы аутентификации. Этот универсальный код ресурса (URI) должен устанавливаться отдельно, как показано в следующем примере кода.

В этом примере кода показано, как использовать пакет SDK для клиента.

```csharp
var dataClient = SpeechRecognitionServiceFactory.CreateDataClient(
  SpeechRecognitionMode.LongDictation,
  "en-us",
  "your_subscriptionId",
  "your_subscriptionId",
  "endpointURL");
// set the authorization Uri
dataClient.AuthenticationUri = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
```

> [!NOTE]
> При использовании методов **Create** из пакета SDK необходимо дважды указать идентификатор подписки ввиду перегрузки этих методов **Create**.
>

Пользовательская служба распознавания речи использует два различных URL-адреса для распознавания кратких и полных форм. Они указаны на странице **Развертывания**. Используйте правильный URL-адрес конечной точки для конкретных форм, которые вы хотите использовать.

Дополнительные сведения о вызове различных клиентов распознавания с помощью пользовательской конечной точки можно получить из описания класса [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop). В документации на этой странице упоминается адаптация акустических моделей, но она применяется ко всем конечным точкам, созданным с помощью Пользовательской службы распознавания речи.

## <a name="send-requests-by-using-the-speech-protocol"></a>Отправка запросов с помощью протокола распознавания речи

Конечные точки, показанные для [протокола распознавания речи](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol), — это конечные точки для протокола распознавания речи для веб-сокетов с открытым кодом.

В настоящее время единственной официальной реализацией клиента является клиент [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Если требуется приступить к работе с помощью предоставленного здесь примера, внесите в его код следующие изменения и повторно выполните сборку примера.

1. В _src\sdk\speech.browser\SpeechConnectionFactory.ts_ замените имя узла wss://speech.platform.bing.com именем узла, отображенным на странице сведений о развертывании. Вставляйте не полный универсальный код ресурса (URI), а только что схему протокола *wss* и имя узла. Например: 

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Настройте параметр _recognitionMode_ в _samples\browser\Samples.html_ согласно вашим требованиям.
    * _RecognitionMode.Interactive_ поддерживает запросы длительностью до 15 секунд.
    * _RecognitionMode.Conversation_ и _RecognitionMode.Dictation_ (в Пользовательской службе распознавания речи их значения равны) поддерживают запросы длительностью до 10 минут.

3. Выполните сборку примера с помощью gulp build, прежде чем использовать его.

> [!NOTE]
> Убедитесь, что используется правильный универсальный код ресурса (URI) для этого протокола. Требуется схема *wss* (а не *http*, как в протоколе клиента). 

Дополнительные сведения см. в [документации по API распознавания речи Bing](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries).

## <a name="send-requests-by-using-http"></a>Отправки запросов по протоколу HTTP

Отправка запроса к пользовательской конечной точке с помощью HTTP-запроса POST аналогична отправке запроса по протоколу НТТР в API распознавания речи Bing в Cognitive Services. Измените URL-адрес с учетом адреса пользовательского развертывания.

Существуют некоторые ограничения для запросов, отправляемых по протоколу HTTP, для конечной точки Cognitive Services и пользовательских конечных точек, созданных с помощью этой службы. HTTP-запрос не может возвращать частичные результаты в процессе распознавания. Кроме того, длительность запросов ограничена 10 секундами для звукового содержимого и 14 секундами для любого содержимого.

Чтобы создать запрос POST, следуйте процедуре для API распознавания речи Cognitive Services.

1. Получите маркер доступа, используя идентификатор своей подписки. Этот маркер нужен для доступа к конечной точке распознавания. Он может быть многократно использован в течение 10 минут.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      Для параметра **SubscriptionId** нужно задать идентификатор подписки, используемой для этого развертывания. Ответом является открытый маркер, необходимый для следующего запроса.

2. Еще раз отправьте звуковые данные на конечную точку с помощью POST.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    Значение **token** — это маркер доступа, полученный вместе с предыдущим вызовом. Значение **https_endpoint** — это полный адрес пользовательской конечной точки преобразования речи в текст, отображенный на странице **Сведения о развертывании**.

Дополнительные сведения о параметрах HTTP-запросов POST и формате ответа см. в описании [API распознавания речи Bing в Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Отправка запросов с помощью библиотеки службы
Библиотека службы позволяет вашей службе использовать облако транскрибирования Microsoft Speech для преобразование речи в текст в режиме реального времени, чтобы клиентское приложение могло отправлять звуковые данные и одновременно получать частичные результаты распознавания в асинхронном режиме. Сведения о пакете SDK службы можно найти [здесь](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary).

> [!NOTE]
> При использовании библиотеки службы необходимо изменить универсальный код ресурса (URI) поставщика авторизации в реализации **IAuthorizationProvider**, указав https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Дополнительная информация
* Повысьте точность своей [пользовательской акустической модели](cognitive-services-custom-speech-create-acoustic-model.md).
* Повысьте точность своей [пользовательской языковой модели](cognitive-services-custom-speech-create-language-model.md).
