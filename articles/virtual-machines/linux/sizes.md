---
title: Размеры виртуальных машин Linux в Azure | Документация Майкрософт
description: Список различных размеров виртуальных машин Linux в Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 5ee3d29ceada238c5e4a633b501f63ed307ba76e
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900873"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Размеры виртуальных машин Linux в Azure
В этой статье описаны доступные размеры и разновидности виртуальных машин Azure, которые можно использовать для запуска приложений и рабочих нагрузок Linux. Здесь также предоставлены рекомендации по развертыванию, которые нужно учитывать при планировании использования этих ресурсов. Также доступна версия этой статьи для [виртуальных машин Windows](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| type                     | Размеры           |    ОПИСАНИЕ       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Универсальные](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, Av2  | Сбалансированное соотношение ресурсов ЦП и памяти. Идеальное решение для тестирования и разработки, небольших и средних баз данных, а также веб-серверов с небольшим или средним объемом трафика. |
| [Оптимизированные для вычислений](sizes-compute.md)        | Fsv2 Fs, F             | Высокое соотношение ресурсов ЦП и памяти. Подходят для веб-серверов со средним объемом трафика, сетевых устройств, пакетных процессов и серверов приложений.        |
| [Оптимизированные для памяти](sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, Dv2  | Высокое соотношение ресурсов памяти и ядра. Отлично подходят для серверов реляционной базы данных, кэша среднего и большого объема, а также выполняющейся в памяти аналитики.                 |
| [Оптимизированные для хранилища](sizes-storage.md)        | Ls                | Высокая пропускная способность дисков и количество операций ввода-вывода. Идеальный вариант для работы с большими данными, а также с базами данных SQL и NoSQL.                                                         |
| [GPU](sizes-gpu.md)            | NV, NC, NCv2, NCv3 и ND            | Специализированные виртуальные машины, предназначенные для ресурсоемкой отрисовки изображений и редактирования видео, а также для обучения моделей и формирования выводов с помощью глубокого обучения. Доступны виртуальные машины одним или несколькими GPU.       |
| [Для высокопроизводительных вычислений](sizes-hpc.md) | H       | Виртуальные машины с самыми быстрыми и мощными ЦП, для которых можно настроить сетевые интерфейсы с высокой пропускной способностью (RDMA). 

<br>

- Подробнее о ценах на различные размеры см. в разделе [Цены на виртуальные машины](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Доступность размеров виртуальных машин см. в статье [Доступность продуктов по регионам](https://azure.microsoft.com/regions/services/).
- Сведения об общих ограничениях виртуальных машин Azure см. в статье [Подписка Azure, границы, квоты и ограничения службы](../../azure-subscription-service-limits.md).
- Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.


## <a name="rest-api"></a>ИНТЕРФЕЙС REST API

Сведения об использовании Rest API, чтобы получить сведения о размерах виртуальных машин, см. в следующих статьях:

- [List available virtual machine sizes for resizing](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes) (Вывод доступных размеров виртуальных машин для изменения размеров)
- [List available virtual machine sizes for a subscription](https://docs.microsoft.com/rest/api/compute/virtualmachines/listall) (Вывод доступных размеров виртуальных машин для подписки)
- [List available virtual machine sizes in an availability set](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes) (Вывод доступных размеров виртуальных машин в группе доступности)

## <a name="acu"></a>ACU

Узнайте больше о том, как с помощью [единиц вычислений Azure (ACU)](acu.md) сравнить производительность вычислений для различных номеров SKU Azure.

## <a name="benchmark-scores"></a>Результаты теста производительности

Узнайте больше о вычислительной мощности виртуальных машин Linux с помощью [результатов теста производительности CoreMark](compute-benchmark-scores.md).

## <a name="next-steps"></a>Дополнительная информация

Узнайте больше о различных доступных размерах виртуальных машин.
- [Универсальные](sizes-general.md)
- [Оптимизированные для вычислений](sizes-compute.md)
- [Оптимизированные для памяти](sizes-memory.md)
- [Оптимизированные для хранилища](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Для высокопроизводительных вычислений](sizes-hpc.md)
- Описание серий A Standard, Dv1 (D1–4 и D11–14 v1) и A8–A11 приведено на странице [Предыдущее поколение](sizes-previous-gen.md).



