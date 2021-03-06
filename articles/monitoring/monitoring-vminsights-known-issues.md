---
title: Известные проблемы с решением "Azure Monitor для виртуальных машин" | Документация Майкрософт
description: Azure Monitor для виртуальных машин — это решение в Azure, которое отслеживает работоспособность и производительность ОС виртуальных машин Azure, автоматически обнаруживает компоненты приложений и их зависимости от других ресурсов, а также строит схему каналов связи между ними. В этой статье рассматриваются известные проблемы с этим решением.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: c03adc239ea7025fe154db315daa17b26f8237f1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980219"
---
# <a name="known-issues-with-azure-monitor-for-vms"></a>Известные проблемы с решением "Azure Monitor для виртуальных машин"

Ниже перечислены известные проблемы с функцией проверки работоспособности решения "Azure Monitor для виртуальных машин":

- В этом выпуске невозможно изменить период времени и частоту проверки работоспособности. 
- Критерии работоспособности нельзя отключить. 
- После подключения решения может потребоваться время, прежде чем данные начнут отображаться в разделе "Azure Monitor -> Виртуальные машины -> Работоспособность" или на странице "Аналитика", которую можно открыть из колонки ресурсов виртуальной машины.
- Интерфейс диагностики работоспособности обновляется быстрее, чем любое другое представление, поэтому при переключении между представлениями могут возникать задержки отображения информации.  
- Сводка оповещений, приведенная в представлении работоспособности решения "Azure Monitor для виртуальных машин", касается оповещений, вызванных проблемами работоспособности, которые были обнаружены на отслеживаемых виртуальных машинах Azure.
- На странице представления **списка оповещений** на одной виртуальной машине и в Azure Monitor отображаются оповещения, состояние монитора которых имело значение срабатывания за последние 30 дней.  Эти представления не настраиваются. Однако, если щелкнуть сводку, откроется страница представления **списка оповещений**, где можно изменить значение фильтра как для состояния монитора, так и для диапазона времени.
- Мы рекомендуем не изменять фильтры **типа ресурса**, **ресурса** и **мониторинга службы** на странице представления **списка оповещений**, так как эти фильтры настроены специально для решения (в этом представлении списка отображаются некоторые дополнительные поля по сравнению с представлением, которое открывается через колонку Azure Monitor).    
- На виртуальных машинах Linux в представлении **Диагностика работоспособности** указано полное доменное имя виртуальной машины вместо пользовательского.
- В результате завершения работы виртуальных машин некоторые критерии работоспособности обновляются до критического состояния, а другие — до исправного состояния, при этом общее состояние виртуальной машины указано как критическое.
- Степень серьезности оповещения о работоспособности нельзя изменить. Его можно только включить или отключить.  Кроме того, некоторые степени серьезности обновляются на основе состояния критериев работоспособности.
- Изменение любого параметра экземпляра критерия работоспособности приведет к изменению этого же параметра во всех экземплярах критериев работоспособности того же типа на виртуальной машине. Например, если изменяется пороговое значение экземпляра критерия работоспособности диска, соответствующего логическому диску C, этот порог будет применяться ко всем другим логическим дискам, обнаруженным и контролируемым для этой же виртуальной машины.   
- Пороговые значения для некоторых критериев работоспособности Windows, таких как служба работоспособности DNS-клиента, не изменяются, потому что их состояние работоспособности уже зафиксировано как **выполняется** или **доступное** для службы или сущности в зависимости от контекста.  Вместо этого значение представлено цифрой 4. В будущем выпуске оно будет преобразовано в фактическую строку отображения.  
- Пороговые значения для некоторых критериев работоспособности Linux, например, работоспособности логического диска, не изменяются, так как уже настроена активация оповещения в случае неработоспособного состояния.  Эти значения (1 или 0) указывают, в каком режиме (в сети или в автономном режиме) и состоянии (включен ли отключен) находится объект.
- Обновление фильтра группы ресурсов в любой группе ресурсов при использовании представления "Azure Monitor -> Виртуальные машины -> Работоспособность -> любое представление списка с предварительно выбранной подпиской и группой ресурсов" в масштабе приведет к тому, что в представлении списка не будет отображаться **результат**.  Вернитесь на вкладку "Azure Monitor -> Виртуальные машины -> Работоспособность" и выберите нужную подписку и группу ресурсов, а затем перейдите к представлению списка.
