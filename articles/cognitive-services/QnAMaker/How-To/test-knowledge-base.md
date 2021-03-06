---
title: Как протестировать базу знаний с помощью QnA Maker и Azure Cognitive Services | Документация Майкрософт
description: Перед публикацией протестируйте базу знаний.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: cffb63666edab25e1b3b0739d0e0f2f828600f3a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381821"
---
# <a name="test-your-knowledge-base"></a>Тестирование базы знаний

Тестирование базы знаний QnA Maker является важной частью итерационного процесса для повышения точности возвращаемых ответов. Вы можете протестировать базу знаний через расширенный интерфейс чата, который также позволяет вносить правки.

## <a name="test-answer-matching"></a>Соответствие тестового ответа

1.  Получите доступ к базе знаний, выбрав ее имя на странице **Мои базы знаний**.
2.  Чтобы открыть выдвигающуюся панель тестирования, щелкните **Тестировать** на верхней панели приложения.

    ![Доступ к тестированию](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Введите запрос в текстовом поле и нажмите клавишу ВВОД.

4.  Ответ наилучшего соответствия из базы знаний возвращается как отклик.

## <a name="clear-test-panel"></a>Очистка панели тестирования

Чтобы очистить все введенные тестовые запросы и их результаты из консоли тестирования, выберите **Начать сначала** в левом верхнем углу панели тестирования.

## <a name="close-test-panel"></a>Закрытие панели тестирования

Чтобы закрыть панель тестирования, еще раз нажмите кнопку **Тестировать**. При открытой панели тестирования нельзя изменить содержимое базы знаний.

## <a name="inspect-score"></a>Проверка оценки

Проверка сведений в результате тестирования выполняется на панели проверки.

1.  На открытой выдвигающейся панели тестирования нажмите кнопку **Проверка** для получения более подробной информации об этом ответе.

    ![Проверка ответов](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Появится панель проверки. На панели находится намерение с высокой оценкой, а также любые идентифицированные сущности. На панели отображается результат выбранного высказывания.

## <a name="correct-the-top-scoring-answer"></a>Исправьте ответы с высокой оценкой

Если ответы с высокой оценкой неверны, выберите правильный ответ из списка и нажмите **Сохранить и Обучать**.

![Доступ к тестированию](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Добавление альтернативных вопросов

Можно добавить альтернативные формы вопроса для данного ответа. Чтобы добавить альтернативные ответы, введите их в текстовое поле и нажмите "Ввод". Для сохранения обновлений выберите **Сохранить и Обучать**.

![Доступ к тестированию](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Добавление нового ответа

Можно добавить новый ответ, если любой из существующих ответов, которые были сопоставлены, неверный или не существует ответа в базе знаний (в КБ нет хорошего совпадения). Чтобы добавить новый ответ на текущий вопрос, введите его в текстовое поле и нажмите "Ввод". 

Для сохранения этого ответа выберите **Сохранить и Обучать**. Новая пара вопросов и ответов теперь будет добавлена в базу знаний.

![Доступ к тестированию](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Все изменения в базе знаний сохраняются только при нажатии кнопки **Сохранить и Обучать**.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Публикация базы знаний](./publish-knowledge-base.md)
