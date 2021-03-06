---
title: Улучшение классификатора с помощью Пользовательской службы визуального распознавания в Azure Cognitive Services | Документация Майкрософт
description: Узнайте, как повысить качество классификатора Пользовательской службы визуального распознавания.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 07/05/2018
ms.author: nolachar
ms.openlocfilehash: 7c6cbd996d0c35b96fde78daf391bebb36feddce
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888186"
---
# <a name="how-to-improve-your-classifier"></a>Как улучшить классификатор

Узнайте, как повысить качество классификатора Пользовательской службы визуального распознавания. Качество классификатора зависит от объема, качества и разнообразия предоставленных ему данных с метками и от того, насколько набор данных сбалансирован. Обычно у хорошего классификатора есть сбалансированный набор учебных материалов, который представляет то, что будет находиться в классификаторе. Часто процесс компиляции такого классификатора итеративный. Общей практикой для достижения ожидаемых результатов считается предварительное выполнение нескольких сеансов обучения.

Далее приведены основные шаги, направленные на улучшение классификатора. Эти действия — это не строго определенные правила, а эвристические правила, которые помогут создать лучший классификатор.

1. Первый сеанс обучения
1. Добавление дополнительных изображений и сбалансированных данных
1. Повторное обучение
1. Добавление изображений с различным фоном, освещением, размером объекта, углом камеры и стилем
1. Повторное обучение и ввод изображений для прогнозирования
1. Изучение результатов прогнозирования
1. Изменение существующих данных обучения

## <a name="data-quantity-and-data-balance"></a>Количество и балансировка данных

Самое главное — загрузить достаточное количество изображений для обучения классификатора. В качестве начальной точки рекомендуется использовать обучающий набор, в котором одной метке соответствует не менее 50 изображений. Если изображений будет меньше, существует серьезный риск возникновения лжевзаимосвязей. В то время как показатели производительности могут свидетельствовать о хорошем качестве, с данными в реальном мире могут возникать трудности. Увеличение количества изображений при обучении классификатора повысит точность результатов прогнозирования.

Также следует учитывать то, что данные должны быть сбалансированными. Например, если для одной метки есть 500 изображений, а для другой — 50, это создает несбалансированный набор данных для обучения и приведет к тому, что результаты прогнозирования модели для одной из меток будут более точными. Если оставить соотношение 1:2 между метками с наименьшим количеством изображений и метками с наибольшим количеством изображений, то скорее всего, вы увидите наилучший результат. Например, если ярлык с наибольшим количеством изображений имеет 500 изображений, то для обучения метка с наименьшим количеством изображений должна содержать не менее 250 изображений.

## <a name="train-more-diverse-images"></a>Обучение на базе более разнообразных изображений

Предоставьте изображения, максимально похожие на конкретное содержимое, которое будет передаваться в классификатор при его использовании. Например, при обучении "яблочного" классификатора, он может оказаться не настолько точным, если для его обучения использовались фотографии яблок в тарелках, а прогноз он делает по фотографиям яблок на деревьях. Включение различных изображений гарантирует, что классификатор не будет предвзятым и может успешно делать общие выводы. Некоторые из способов, с помощью которых можно сделать набор для обучения более разнообразным, приведены ниже.

__Фон__. Предоставьте изображения объекта на разном фоне (например, фрукты на тарелке и фрукты в продуктовой сумке). Фотографии в контексте лучше, чем фотографии на нейтральном фоне, так как они предоставляют больше информации для классификатора.

![Изображение примеров фонов](./media/getting-started-improving-your-classifier/background.png)

__Освещение__. Предоставьте изображения объекта с различным освещением (например, с использованием вспышки, высокой экспозиции и т.д.), особенно, когда у изображений, используемых для прогнозирования, разнообразное освещение. Также полезно включать изображения с разнообразной насыщенностью, оттенком и яркостью.

![Изображение примеров освещения](./media/getting-started-improving-your-classifier/lighting.png)

__Размер объекта__. Предоставьте изображения, в которых объекты имеют разный размер, захватывая разные части объекта. Например, фотография нескольких бананов и крупный план одного банана. Различные размеры объектов помогают классификатору делать общие выводы.

![Изображение примеров размеров](./media/getting-started-improving-your-classifier/size.png)

__Угол камеры__. Предоставьте изображения объекта, снятые под разными углами. Если все фотографии сделаны с помощью набора неподвижных камер (например, камер видеонаблюдения), убедитесь, что каждой из камер была назначена иная метка, даже если они захватывают одинаковые объекты. Это позволит избежать формирования лжевзаимосвязей — процесс моделирования несвязанных объектов (таких как фонарные столбы), как ключевой особенности.

![Изображение примеров объектов под разными углами](./media/getting-started-improving-your-classifier/angle.png)

__Стиль__. Предоставьте изображения объекта одного класса, выполненные в разных стилях (например, различные виды цитрусовых). Тем не менее, если присутствуют изображения объектов совершенно разных стилей (например, Микки-Маус и реальная крыса), рекомендуется отметить их как отдельные классы, что позволит лучше представлять их различные функции.

![Изображение примеров стилей](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-images-submitted-for-prediction"></a>Использование изображений, представленных для прогнозирования

Пользовательская служба визуального распознавания сохраняет изображения, переданные в конечную точку прогнозирования. Чтобы использовать эти изображения для улучшения классификатора, выполните следующие шаги.

1. Чтобы просмотреть отправленные в классификатор изображения, откройте [веб-страницу Пользовательской службы визуального распознавания](https://customvision.ai), перейдите к проекту и выберите вкладку __Predictions__ (Прогнозы). Представление по умолчанию содержит изображения из текущей итерации. Поле с раскрывающимся списком __Iteration__ (Итерация) позволяет перейти к изображениям, переданным в предыдущих итерациях.

    ![Изображение вкладки прогнозов](./media/getting-started-improving-your-classifier/predictions.png)

2. Наведите указатель мыши на изображение, чтобы увидеть спрогнозированные классификатором теги. Изображения для классификатора ранжируются по полезности, то есть самые полезные расположены вверху списка. Чтобы выбрать другой вариант сортировки, используйте раздел __Sort__ (Сортировка). Чтобы добавить изображение в существующий набор данных для обучения, выберите изображение, правильный тег и нажмите кнопку __Save and close__ (Сохранить и закрыть). Выбранное изображение будет удалено из списка __Predictions__ (Прогнозы) и добавлено к изображениям для обучения. Теперь его можно просмотреть на вкладке __Training Images__ (Изображения для обучения).

    ![Изображение страницы для управления тегами](./media/getting-started-improving-your-classifier/tag.png)

3. Нажмите кнопку __Train__ (Обучение), чтобы переобучить классификатор.

## <a name="visually-inspect-predictions"></a>Визуальная проверка прогнозов

Чтобы просмотреть прогнозы по изображениям, выберите вкладку __Training Images__ (Изображения для обучения) и щелкните __Iteration History__ (Журнал итераций). В этом представлении изображения с неправильным прогнозом выделяются красными прямоугольниками.

![Изображение журнала итераций](./media/getting-started-improving-your-classifier/iteration.png)

В некоторых случаях визуальная проверка позволяет заметить в этих ошибках закономерность и исправить ее, добавив дополнительные данные для обучения или изменив существующие данные. Например, классификатор яблока и лайма может неправильно отметить все зеленые яблоки и обозначить их как лайм. Чтобы устранить эту проблему, следует добавить и применить данные для обучения с изображениями зеленых яблок.

## <a name="unexpected-classification"></a>Неожиданная классификация

Иногда классификатор неверно обнаруживает общие характеристики предоставленных изображений. Например, при создании классификатора яблок и цитрусовых, имея изображения яблок в руках, а цитрусовых на белых тарелках, классификатор может обучаться на руках и белых тарелках вместо яблок и цитрусовых.

![Изображение неожиданной классификации](./media/getting-started-improving-your-classifier/unexpected.png)

Чтобы устранить эту проблему, следуйте подробным рекомендациям по использованию более разнообразных изображений: с разным ракурсом, разным фоном, разными размерами объекта, в группах и другие варианты.

## <a name="negative-image-handling"></a>Отрицательные примеры изображений

Пользовательская служба визуального распознавания поддерживает автоматическое применение отрицательных примеров изображений. В случае, когда компилируют классификатор для различения винограда и банана, то для фотографии ботинка он должен предоставить оценку 0% для обеих категорий.

С другой стороны, в тех случаях, когда отрицательные изображения являются лишь вариацией изображений, используемых в обучении, вполне вероятно, что из-за большого сходства модель будет классифицировать отрицательные изображения как отмеченные классы. Например, если классификатору апельсина и грейпфрута предложить для сравнения изображение клементина, он может оценить его как апельсин. Это может произойти, так как многие особенности клементина (цвет, форма, текстура, естественная среда обитания и т.д.) напоминают апельсины.  Если отрицательные изображения имеют такую природу, рекомендуется создать один или несколько отдельных тегов ("другое") и во время обучения пометить ими отрицательные изображения. Такие действия помогут модели лучше различать эти классы.

## <a name="next-steps"></a>Дополнительная информация

Узнайте, как можно протестировать изображения программными средствами, отправив их в API прогнозирования.

> [!div class="nextstepaction"]
[Использование API-интерфейса прогнозирования](use-prediction-api.md)
