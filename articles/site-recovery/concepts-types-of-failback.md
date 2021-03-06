---
title: Восстановление размещения в Azure Site Recovery | Документация Майкрософт
description: В этой статье представлен обзор различных типов восстановления размещения и предупреждений, которые необходимо учитывать при восстановлении размещения на локальный сайт с помощью службы Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: guaravd
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: rajanki
ms.openlocfilehash: 2a9ee380fc16c4088d98875dd465509c4023d037
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920395"
---
# <a name="overview-of-failback"></a>Общие сведения о восстановлении размещения

После отработки отказа в Azure вы можете восстановить размещение на локальном сайте. В Azure Site Recovery возможны два типа восстановления размещения. 

- Восстановление размещения в исходное расположение. 
- Восстановление размещения в альтернативное расположение.

При отработке отказа виртуальной машины VMware восстановление размещения можно выполнить на той же исходной локальной виртуальной машине, если она по-прежнему доступна. В нашем случае будет выполнена репликация только изменившихся данных. Этот сценарий называется **восстановлением в исходное расположение**. Если локальная виртуальная машина не существует, **восстановление выполняется в альтернативное расположение**.

> [!NOTE]
> Вы можете восстановить размещение только на исходные сервер vCenter и сервер конфигурации. Невозможно развернуть новый сервер конфигурации и восстановить размещение с его помощью. Кроме того, невозможно добавить новый сервер vCenter на имеющийся сервер конфигурации и восстановить размещение на этот сервер vCenter.

## <a name="original-location-recovery-olr"></a>Восстановление в исходное расположение (OLR)
Если вы выбрали восстановление размещения на исходной виртуальной машине, необходимо, чтобы были соблюдены следующие условия.

* Если виртуальной машиной управляет сервер vCenter, то узел ESX главного целевого сервера должен иметь доступ к хранилищу данных виртуальной машины.
* Если виртуальная машина находится на узле ESX, но ею управляет не сервер vCenter, жесткий диск виртуальной машины должен находиться в хранилище данных, к которому имеет доступ узел главного целевого сервера.
* Если виртуальная машина находится на узле ESX и не использует vCenter, прежде чем повторно включить защиту, следует выполнить обнаружение узла ESX главного целевого сервера. Это применимо, если выполняется восстановление размещения и для физических серверов.
* Восстанавливать размещение можно в виртуальную сеть хранения данных (vSAN) или на диски RDM, если эти диски уже существуют и подключены к локальной виртуальной машине.

> [!IMPORTANT]
> Важно включить disk.enableUUID = TRUE, чтобы во время восстановления размещения служба Azure Site Recovery могла идентифицировать исходный VMDK на виртуальной машине, на которую будут записываться ожидающие изменения. Если значение TRUE не задано, служба пытается определить соответствующий локальный VMDK. Если правильный VMDK не найден, она создает дополнительный диск для записи данных.

## <a name="alternate-location-recovery-alr"></a>Восстановление в альтернативное расположение (ALR)
Сценарий, в котором локальная виртуальная машина не существует перед повторным включением защиты виртуальной машины, называется восстановлением в альтернативное расположение. В этом случае рабочий процесс повторной защиты создает локальную виртуальную машину еще раз. Это также приведет к скачиванию всех данных.

* Если восстановление размещения выполняется в альтернативное расположение, виртуальная машина будет восстановлена на том же узле ESX, на котором развернут главный целевой сервер. Для создания диска будет использовано хранилище данных, выбранное при повторном включении защиты виртуальной машины.
* Восстановление размещения можно выполнить только в хранилище данных файловой системы виртуальной машины (VMFS) или хранилище данных vSAN. При наличии RDM повторно включить защиту и восстановить размещение не удастся.
* При повторном включении защиты выполняется одна первоначальная передача большого объема данных, за которой следует передача изменившихся данных. Этот процесс обусловлен тем, что виртуальная машина не существует в локальной среде. При этом необходимо выполнить репликацию полного набора данных. Такое повторное включение защиты занимает больше времени, чем восстановление в исходном расположении.
* Восстановление размещения невозможно выполнить на диски RDM. В хранилище данных VMFS или vSAN можно создавать только новые диски виртуальной машины (VMDK-файлы).

> [!NOTE]
> Если выполнена отработка отказа физического компьютера в Azure, то восстановить его размещение можно только в качестве виртуальной машины VMware. Это соответствует рабочему процессу восстановления в альтернативное расположение. Убедитесь, что у вас есть хотя бы один главный целевой сервер и необходимые узлы ESX (ESXi), на которых необходимо восстановить размещение.

## <a name="next-steps"></a>Дополнительная информация

[Восстановление размещения из Azure на локальный сайт](vmware-azure-failback.md).

