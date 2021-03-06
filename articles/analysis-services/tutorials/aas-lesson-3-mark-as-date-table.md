---
title: 'Учебник по службам Azure Analysis Services: занятие 3 "Обозначение таблицы дат" | Документы Майкрософт'
description: Описывает обозначение таблицы дат в учебном проекте служб Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6627cf74154e66a84f34d5d5bc1d78ed3e0c38a2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443605"
---
# <a name="mark-as-date-table"></a>Обозначение таблицы дат

В занятии 2 "Получение данных" вы импортировали таблицу измерения DimDate. Хотя в вашей модели эта таблица имеет имя DimDate, она также может называться *таблицей дат*, так как содержит данные о датах и времени.  
  
Каждый раз при использовании функций логики операций со временем DAX (вы займетесь этим чуть позже при создании мер) нужно указывать такие свойства, как *Таблица дат* и уникальный идентификатор *Столбец даты* в ней.
  
На этом занятии вы пометите таблицу DimDate как *таблицу дат*, а столбец даты как *Столбец даты* (уникальный идентификатор).  

Прежде чем помечать таблицу дат и столбец дат, рекомендуется выполнить несколько служебных действий, чтобы упростить восприятие модели. Обратите внимание на столбец с именем **FullDateAlternateKey** в таблице DimDate. Он содержит по одной строке для каждого дня каждого календарного года, включенного в таблицу. Этот столбец часто используется в формулах мер и отчетах. Однако FullDateAlternateKey нельзя назвать уместным идентификатором для этого столбца. Его имя следует изменить на **Date**, чтобы упростить поиск и использование в формулах. По возможности рекомендуется переименовывать такие объекты, как таблицы и столбцы, чтобы упростить их поиск в SSDT и клиентских приложениях, таких как Power BI и Excel. 
  
Предполагаемое время выполнения этого занятия: **3 минуты**  
  
## <a name="prerequisites"></a>предварительным требованиям  
Этот раздел входит в учебник по табличному моделированию, который следует изучать в предложенном порядке. Прежде чем выполнять задачи в этом разделе, нужно завершить предыдущее занятие: [Занятие 2. Получение данных](../tutorials/aas-lesson-2-get-data.md). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>Чтобы переименовать столбец FullDateAlternateKey, сделайте следующее:

1.  В конструкторе моделей щелкните таблицу **DimDate**.

2.  Дважды щелкните заголовок для столбца **FullDateAlternateKey** и измените имя на **Date**.

  
### <a name="to-set-mark-as-date-table"></a>Чтобы обозначить таблицу дат, сделайте следующее:  
  
1.  Выберите столбец **Date** и затем в разделе **Тип данных** окна **Свойства** выберите параметр **Date**.  
  
2.  Щелкните меню **Таблица**, а затем выберите **Date** и **Пометить как таблицу дат**.  
  
3.  В списке **Date** диалогового окна **Пометить как таблицу дат** выберите столбец **Date** в качестве уникального идентификатора. Обычно он уже выбран по умолчанию. Последовательно выберите **ОК**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Что дальше?
[Занятие 4. Создание связей](../tutorials/aas-lesson-4-create-relationships.md).
  
