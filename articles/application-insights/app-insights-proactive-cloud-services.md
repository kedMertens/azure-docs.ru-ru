---
title: Оповещение о проблемах в облачных службах Azure с использованием интеграции системы диагностики Azure с помощью Azure Application Insights | Документация Майкрософт
description: Мониторинг проблем, таких как сбои при запуске, аварийное завершение и циклы повторного использования роли, в облачных службах Azure с помощью Azure Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.reviewer: harelbr
ms.author: mbullwin
ms.openlocfilehash: cdb395a590fb200a24c68e56728a270b968e53b5
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091429"
---
# <a name="alert-on-issues-in-azure-cloud-services-using-the-azure-diagnostics-integration-with-azure-application-insights"></a>Оповещение о проблемах в облачных службах Azure с использованием интеграции системы диагностики Azure с помощью Azure Application Insights

В этой статье описывается, как настроить правила генерации оповещений, которые отслеживают такие проблемы, как сбои при запуске, аварийное завершение и циклы повторного использования роли, в облачных службах Azure (Интернет и рабочие роли).

Метод, описанный в этой статье, основан на [интеграции системы диагностики Azure с помощью Application Insights](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/) и недавно выпущенных возможностей [оповещений журнала для Application Insights](https://azure.microsoft.com/blog/log-alerts-for-application-insights-preview/).

## <a name="define-a-base-query"></a>Определение базового запроса

Для начала необходимо определить базовый запрос, который извлекает события журнала событий Windows из канала Azure Windows, которые записываются в Application Insights в качестве записей трассировки.
Эти записи можно использовать для обнаружения различных проблем в облачных службах Azure, таких как сбои при запуске, ошибки среды выполнения и циклы повторного использования.

> [!NOTE]
> Базовый запрос, показанный ниже, проверяет наличие проблем в 30-минутном окне времени и предполагает 10-минутную задержку для приема записей телеметрии. Эти значения по умолчанию можно настроить по своему усмотрению.

```
let window = 30m;
let endTime = ago(10m);
let EventLogs = traces
| where timestamp > endTime - window and timestamp < endTime
| extend channel = tostring(customDimensions.Channel), eventId = tostring(customDimensions.EventId)
| where channel == 'Windows Azure' and isnotempty(eventId)
| where tostring(customDimensions.DeploymentName) !contains 'deployment' // discard records captured from local machines
| project timestamp, channel, eventId, message, cloud_RoleInstance, cloud_RoleName, itemCount;
```

## <a name="check-for-specific-event-ids"></a>Проверка определенных идентификаторов событий

После получения событий журнала событий Windows определенные проблемы могут быть обнаружены путем проверки их соответствующих идентификаторов событий и свойств сообщений (см. примеры ниже).
Просто объедините базовый запрос выше с одним из запросов ниже и используйте этот комбинированный запрос при определении правила генерации оповещений журнала.

> [!NOTE]
> В приведенных ниже примерах проблема будет обнаружена, если в течение анализируемого временного окна обнаружено более трех событий. Это значение по умолчанию можно настроить для изменения чувствительности правила генерации оповещений.

```
// Detect failures in the OnStart method
EventLogs
| where eventId == '2001'
| where message contains '.OnStart()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures during runtime
EventLogs
| where eventId == '2001'
| where message contains '.Run()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures when running a startup task
EventLogs
| where eventId == '1000'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect recycle loops
EventLogs
| where eventId == '1006'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

## <a name="create-an-alert"></a>Создание оповещения

В меню навигации в ресурсе Application Insights перейдите к **Оповещение**, а затем выберите **Новое правило генерации оповещений**.

![Снимок экрана "Создание правила"](./media/app-insights-proactive-cloud-services/001.png)

В окне **Создать правило** в разделе **Определение условия оповещения** нажмите **Add criteria** (Добавить критерий), а затем выберите **Custom log search** (Пользовательский поиск журнала).

![Снимок экрана "Определение критериев условия для оповещения"](./media/app-insights-proactive-cloud-services/002.png)

В поле **Поисковый запрос** вставьте комбинированный запрос, который был подготовлен на предыдущем шаге.

Затем перейдите к **Пороговому значению** и установите значение, равное 0. При необходимости можно изменить **Период** и периодичность **полей**.
Нажмите кнопку **Done**(Готово).

![Снимок экрана "Настройка логики сигналов запроса"](./media/app-insights-proactive-cloud-services/003.png)

В разделе **Определение сведений об оповещении** укажите **Имя** и **Описание** правила генерации оповещений и установите ему **Серьезность**.
Кроме того, убедитесь, что кнопка **Включить правило при создании** установлена в положение **Да**.

![Снимок экрана "Сведения об оповещении"](./media/app-insights-proactive-cloud-services/004.png)

В разделе **Определение группы действий** можно выбрать существующую **Группу действий** или создать новую.
Можно выбрать, чтобы группа действий содержала несколько действий различных типов.

![Снимок экрана "Группа действий"](./media/app-insights-proactive-cloud-services/005.png)

После определения группы действий подтвердите свои изменения и нажмите **Создать правило генерации оповещений**.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об автоматическом обнаружении.

[Аномальные сбои](app-insights-proactive-failure-diagnostics.md)
[Утечка памяти](app-insights-proactive-potential-memory-leak.md)
[Аномалии производительности](app-insights-proactive-performance-diagnostics.md)

