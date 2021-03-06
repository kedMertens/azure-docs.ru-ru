---
title: Тестирование приложения LUIS в Azure | Документы Майкрософт
description: Используйте службу Language Understanding (LUIS) для постоянной работы над приложением, чтобы усовершенствовать его и улучшить его функции распознавания речи.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: diberry
ms.openlocfilehash: d231eaf98358e3f8237a820e59433558d293872f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224353"
---
# <a name="testing-in-luis"></a>Тестирование в LUIS

Тестирование — это процесс предоставления примеров высказываний в LUIS и получение ответа с распознанными службой LUIS намерениями и сущностями. 

Приложение LUIS можно [тестировать](luis-interactive-test.md) в интерактивном режиме, проверяя по одному высказыванию или указав [пакет](luis-concept-batch-test.md) высказываний. При тестировании текущая [активная](luis-concept-version.md#active-version) модель сравнивается с опубликованной моделью. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>
## <a name="what-is-a-score-in-testing"></a>Что такое оценка при тестировании
Дополнительные сведения об оценках прогнозирования см. в статье об [оценке прогнозирования](luis-concept-prediction-score.md).

## <a name="interactive-testing"></a>Интерактивное тестирование
Интерактивное тестирование выполняется на панели **Test** (Тестирование) веб-сайта. Введите высказывание, чтобы увидеть, каким образом происходит определение и оценка намерений и сущностей. Если LUIS не прогнозирует намерения и сущности, как ожидается в высказывании на панели тестирования, скопируйте его на страницу **Намерение** в качестве нового высказывания. Затем пометьте части этого высказывания и обучите LUIS. 

## <a name="batch-testing"></a>Пакетное тестирование
При тестировании нескольких высказываний одновременно см. сведения в статье [Пакетное тестирование](luis-concept-batch-test.md).

## <a name="endpoint-testing"></a>Тестирование конечной точки
Можно выполнить тестирование с помощью [конечной точки](luis-glossary.md#endpoint) и максимум двумя версиями приложения. Используя основную или динамическую версию приложения, заданную в качестве **рабочей** конечной точки, добавьте вторую версию в **промежуточную** конечную точку. В этом случае у вас будет три версии высказывания: текущая модель на панели тестирования на веб-сайте [LUIS](luis-reference-regions.md) и две версии в двух разных конечных точках. 

При тестировании всех конечных точек учитывается квота на использование. 

## <a name="do-not-log-tests"></a>Тестирование без ведения журнала
Если вы тестируете конечную точку и не хотите вести журнал для высказывания, следует использовать конфигурацию строки запроса `logging=false`.

## <a name="where-to-find-utterances"></a>Где найти высказывания
LUIS хранит все зарегистрированные высказывания в журнале запросов, который доступен для скачивания на странице списка **Приложения** веб-сайта [LUIS](luis-reference-regions.md), а также [API-интерфейсы разработки](https://aka.ms/luis-authoring-apis) LUIS. 

Высказывания, неизвестные для LUIS, приведены на странице **[Просмотр фрагментов речи конечной точки](luis-how-to-review-endoint-utt.md)** на веб-сайте [LUIS](luis-reference-regions.md). 

![Просмотр фрагментов речи конечной точки](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>Обязательное обучение
После внесения изменений в модель не забудьте [обучить](luis-how-to-train.md) LUIS. Изменения в приложении LUIS не отображаются при тестировании, пока приложение не будет обучено. 

## <a name="best-practices"></a>Рекомендации
Ознакомьтесь с [рекомендациями](luis-concept-best-practices.md).

## <a name="next-steps"></a>Дополнительная информация

* Узнайте больше о [тестировании](luis-interactive-test.md) высказываний.