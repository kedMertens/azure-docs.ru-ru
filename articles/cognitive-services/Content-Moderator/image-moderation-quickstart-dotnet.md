---
title: 'Azure Content Moderator: модерация изображений с использованием .NET | Документация Майкрософт'
description: Описывается, как модерировать изображения с использованием пакета SDK Azure Content Moderator для .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: cc2329c233029a1ff6bd82da3d090c4e98a8bac8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380229"
---
# <a name="moderate-images-using-net"></a>Модерация изображений с помощью .NET

В этой статье содержатся сведения и примеры кода, которые помогут приступить к работе с пакетом SDK Content Moderator для .NET в следующих целях: 
- проверка изображения на наличие содержимого для взрослых или неприличного содержимого;
- обнаружение и извлечение текста из изображения;
- определение лиц на изображении.

В этой статье предполагается, что вы уже работали с Visual Studio и C#.

## <a name="sign-up-for-content-moderator-services"></a>Регистрация в службах Content Moderator

Прежде чем использовать службы Content Moderator через REST API или пакет SDK, необходимо получить ключ подписки.
Изучите [краткое руководство](quick-start.md) о том, как можно получить ключ.

## <a name="create-your-visual-studio-project"></a>Создание проекта Visual Studio

1. Добавьте новый проект **Консольное приложение (.NET Framework)** в свое решение.

   В примере кода назовите проект **ImageModeration**.

1. Выберите этот проект единственным запускаемым проектом для решения.

1. Добавьте ссылку на сборку проекта **ModeratorHelper**, созданную вами в [кратком руководстве по вспомогательному клиенту Content Moderator](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Установка необходимых пакетов

Установите следующие пакеты NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Обновление инструкций using программы

Измените инструкции using программы.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="initialize-application-specific-settings"></a>Инициализация параметров приложения

Добавьте следующие статические поля в класс **Program** в файле Program.cs.

    ///<summary>
    ///The name of the file that contains the image URLs to evaluate.
    ///</summary>
    ///<remarks>You will need to create an input file and update 
    ///this path accordingly. Paths are relative to the execution directory.
    ///</remarks>
    private static string ImageUrlFile = "ImageFiles.txt";

    ///<summary>
    ///The name of the file to contain the output from the evaluation.
    ///</summary>
    ///<remarks>Paths are relative to the execution directory.
    ///</remarks>
    private static string OutputFile = "ModerationOutput.json";


> [!NOTE]
> Для формирования выходных данных в примере используются следующие изображения.
> - https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
> - https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png

## <a name="store-the-analysis-results"></a>Сохранение результатов анализа

Добавьте следующий класс в класс **Program** . Используйте экземпляр этого класса для записи результатов модерации проверенных изображений.

    /// <summary>
    /// Contains the image moderation results for an image, 
    /// including text and face detection results.
    /// </summary>
    public class EvaluationData
    {
        /// <summary>
        /// The URL of the evaluated image.
        /// </summary>
        public string ImageUrl;

        /// <summary>
        /// The image moderation results.
        /// </summary>
        public Evaluate ImageModeration;

        /// <summary>
        /// The text detection results.
        /// </summary>
        public OCR TextDetection;

        /// <summary>
        /// The face detection results;
        /// </summary>
        public FoundFaces FaceDetection;
    }

## <a name="evaluate-an-individual-image"></a>Оценка отдельного изображения

Добавьте следующий метод в класс **Program**. Этот метод оценивает отдельное изображение и возвращает результаты оценки.

> [!NOTE]
> Для ключа службы Content Moderator установлен лимит числа запросов в секунду (RPS). Если его превысить, пакет SDK породит исключение с кодом ошибки 429. 
>
> Ключ уровня "Бесплатный" предусматривает ограничение в один RPS.


    /// <summary>
    /// Evaluates an image using the Image Moderation APIs.
    /// </summary>
    /// <param name="client">The Content Moderator API wrapper to use.</param>
    /// <param name="imageUrl">The URL of the image to evaluate.</param>
    /// <returns>Aggregated image moderation results for the image.</returns>
    /// <remarks>This method throttles calls to the API.
    /// Your Content Moderator service key will have a requests per second (RPS)
    /// rate limit, and the SDK will throw an exception with a 429 error code 
    /// if you exceed that limit. A free tier key has a 1 RPS rate limit.
    /// </remarks>
    private static EvaluationData EvaluateImage(
    ContentModeratorClient client, string imageUrl)
    {
        var url = new ImageUrl("URL", imageUrl.Trim());

        var imageData = new EvaluationData();

        imageData.ImageUrl = url.Value;

        // Evaluate for adult and racy content.
        imageData.ImageModeration =
            client.ImageModeration.EvaluateUrlInput("application/json", url, true);
        Thread.Sleep(1000);

        // Detect and extract text.
        imageData.TextDetection =
            client.ImageModeration.OCRUrlInput("eng", "application/json", url, true);
        Thread.Sleep(1000);

        // Detect faces.
        imageData.FaceDetection =
            client.ImageModeration.FindFacesUrlInput("application/json", url, true);
        Thread.Sleep(1000);

        return imageData;
    }

Метод **EvaluateUrlInput** — это программа-оболочка для REST API модерации изображений.
Возвращаемое значение содержит объект, возвращенный из вызова API.

Метод **OCRUrlInput** — это программа-оболочка для REST API распознавания текста на изображениях.
Возвращаемое значение содержит объект, возвращенный из вызова API.

Метод **FindFacesUrlInput** — это программа-оболочка для REST API поиска лиц на изображениях.
Возвращаемое значение содержит объект, возвращенный из вызова API.

## <a name="process-the-image-urls-in-your-code"></a>Обработка URL-адресов изображений в коде

Добавьте следующий код в метод **Main**.

    // Create an object to store the image moderation results.
    List<EvaluationData> evaluationData = new List<EvaluationData>();

    // Create an instance of the Content Moderator API wrapper.
    using (var client = Clients.NewClient())
    {
        // Read image URLs from the input file and evaluate each one.
        using (StreamReader inputReader = new StreamReader(ImageUrlFile))
        {
            while (!inputReader.EndOfStream)
            {
                string line = inputReader.ReadLine().Trim();
                if (line != String.Empty)
                {
                    EvaluationData imageData = EvaluateImage(client, line);
                    evaluationData.Add(imageData);
                }
            }
        }
    }

    // Save the moderation results to a file.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        outputWriter.WriteLine(JsonConvert.SerializeObject(
            evaluationData, Formatting.Indented));

        outputWriter.Flush();
        outputWriter.Close();
    }

## <a name="run-the-program-and-review-the-output"></a>Запуск программы и просмотр выходных данных

Следующий объект JSON содержит выходные данные программы.

> [!NOTE]
> `isImageAdultClassified` обозначает потенциальное наличие изображений, которые в некоторых обстоятельствах могут считаться сексуально откровенными или предназначенными только для взрослых.
> `isImageRacyClassified` обозначает потенциальное наличие изображений, которые в некоторых обстоятельствах могут считаться сексуально окрашенными или не предназначенными для детей.
>

    [
    {
    "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
    "ImageModeration": {
      "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
      "result": false,
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "116"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "273405"
        }
      ],
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      }
    },
    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "metadata": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "12"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "273405"
        }
      ],
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
      "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },
    "FaceDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
      "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
      "result": false,
      "count": 0,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "11"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "273405"
        }
      ],
      "faces": []
    }
    },
    {
    "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
    "ImageModeration": {
      "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
      "result": false,
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
      "adultClassificationScore": 0.0035635426174849272,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.021369094029068947,
      "isImageRacyClassified": false,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "109"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "2278902"
        }
      ],
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      }
    },
    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "metadata": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "46"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "2278902"
        }
      ],
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
      "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
      "language": "eng",
      "text": "",
      "candidates": []
    },
    "FaceDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
      "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
      "result": true,
      "count": 6,
      "advancedInfo": [
        {
          "key": "ImageDownloadTimeInMs",
          "value": "60"
        },
        {
          "key": "ImageSizeInBytes",
          "value": "2278902"
        }
      ],
      "faces": [
        {
          "bottom": 598,
          "left": 44,
          "right": 268,
          "top": 374
        },
        {
          "bottom": 620,
          "left": 308,
          "right": 532,
          "top": 396
        },
        {
          "bottom": 575,
          "left": 594,
          "right": 773,
          "top": 396
        },
        {
          "bottom": 563,
          "left": 812,
          "right": 955,
          "top": 420
        },
        {
          "bottom": 611,
          "left": 972,
          "right": 1151,
          "top": 432
        },
        {
          "bottom": 510,
          "left": 1232,
          "right": 1456,
          "top": 286
        }
      ]
    }
    }
    ]


## <a name="next-steps---get-the-source-code"></a>Дальнейшие действия: получение исходного кода

[Скачайте решение Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) с этим и другими краткими руководствами по Content Moderator для .NET и приступите к интеграции.
