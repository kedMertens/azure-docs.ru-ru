---
title: Краткое руководство по Azure Cognitive Services и API "Поиск сущностей Bing" для Python | Документация Майкрософт
description: Получите информацию и примеры кода, которые помогут вам приступить к работе с API Bing для поиска сущностей с использованием Microsoft Cognitive Services в Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-jaswel
ms.openlocfilehash: 88e954af0254b158ea59a88ed4523e3b141a135e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382308"
---
# <a name="quickstart-for-microsoft-bing-entity-search-api-with-python"></a>Краткое руководство по API Bing для поиска сущностей с использованием Python 
<a name="HOLTop"></a>

В этой статье описано, как использовать [API Bing для поиска сущностей](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) с Python.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения этого кода потребуется [Python 3.x](https://www.python.org/downloads/).

Необходима [учетная запись API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с **API Bing для поиска сущностей**. Для данного краткого руководства достаточно [бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api). Требуется ключ доступа, предоставляемый при активации бесплатной пробной версии. Можно также использовать ключ платной подписки, указанный на панели мониторинга Azure.

## <a name="search-entities"></a>Сущности для поиска

Чтобы запустить это приложение, сделайте следующее.

1. Создайте проект Python в используемой вами интегрированной среде разработки.
2. Добавьте указанный ниже код.
3. Замените значение `key` ключом доступа, допустимым для своей подписки.
4. Запустите программу.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse
import json

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/entities'

mkt = 'en-US'
query = 'italian restaurants near me'

params = '?mkt=' + mkt + '&q=' + urllib.parse.quote (query)

def get_suggestions ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_suggestions ()
print (json.dumps(json.loads(result), indent=4))
```

**Ответ**

Успешный ответ возвращается в формате JSON, как показано в примере ниже: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "http://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

[Вверх](#HOLTop)

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Руководство по API Bing для поиска сущностей](../tutorial-bing-entities-search-single-page-app.md)
> [Общие сведения об API Bing для поиска сущностей](../search-the-web.md )
> [Справочник по API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
