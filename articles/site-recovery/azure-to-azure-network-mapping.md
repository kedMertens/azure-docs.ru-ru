---
title: Сопоставление виртуальных сетей между двумя регионами Azure в Azure Site Recovery | Документация Майкрософт
description: Azure Site Recovery координирует репликацию, отработку отказа и восстановление виртуальных машин и физических серверов. Узнайте о функции отработки отказа в Azure или в дополнительный центр обработки данных.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: aed804a257376308c668ce0c2f3e8ce652ee9b3f
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "42140103"
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Сопоставление виртуальных сетей в разных регионах Azure


В этой статье описывается, как сопоставлять два экземпляра виртуальной сети Azure в разных регионах Azure. Сетевое сопоставление гарантирует, что при создании реплицированной виртуальной машины в целевом регионе Azure, виртуальная машина создается также в виртуальной сети, сопоставленной с виртуальной сетью исходной виртуальной машины.  

## <a name="prerequisites"></a>Предварительные требования
Перед началом сопоставления сетей убедитесь, что вы создали [виртуальную сеть Azure](../virtual-network/virtual-networks-overview.md) в исходном и целевом регионе Azure.

## <a name="map-virtual-networks"></a>Сопоставление виртуальных сетей

Чтобы сопоставить виртуальную сеть Azure в одном регионе Azure (исходная сеть) с виртуальной сетью в другом регионе (целевая сеть) для виртуальных машин Azure, выберите **Site Recovery Infrastructure** (Инфраструктура Site Recovery)  > **Сетевое сопоставление**. Создайте сетевое сопоставление.

![Окно сопоставления сетей — создание сетевого сопоставления](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


В следующем примере виртуальная машина выполняется в регионе "Восточная Азия" и реплицируется в регион "Юго-Восточная Азия".

Чтобы выполнить сетевое сопоставление из региона "Восточная Азия" в регион "Юго-Восточная Азия", выберите расположения исходной и целевой сети. Нажмите кнопку **ОК**.

![Окно добавления сетевого сопоставления — выбор исходного и целевого расположений для исходной сети](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Повторите предыдущие действия, чтобы создать сетевое сопоставление из региона "Юго-Восточная Азия" в регион "Восточная Азия".

![Окно добавления сетевого сопоставления — выбор исходного и целевого расположений для целевой сети](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Сопоставление сети при включении репликации

Если при первой репликации виртуальной машины из одного региона Azure в другой сопоставление сети не существует, вы можете задать целевую сеть во время настройки репликации. На основе этого параметра Site Recovery создает сетевое сопоставление из исходного региона в целевой и наоборот.   

![Область настройки параметров — выбор целевого расположения](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

По умолчанию Site Recovery создает сеть в целевом регионе, идентичную исходной. Для этого к имени исходной сети добавляется суффикс **-asr**. Чтобы выбрать созданную сеть, щелкните **Customize** (Настроить).

![Область настройки целевых параметров — настройка имени целевой группы ресурсов и целевой виртуальной сети](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Если сетевое сопоставление уже выполнено, изменить целевую виртуальную сеть при включении репликации нельзя. Таким образом, чтобы изменить целевую виртуальную сеть, измените имеющееся сетевое сопоставление.  

![Область настройки целевых параметров — настройка имени целевой группы ресурсов](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Область изменения сопоставления сети — изменение имени имеющейся целевой виртуальной сети](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Если вы изменили сетевое сопоставление из региона A в регион B, убедитесь, что сетевое сопоставление из региона B в регион А было также изменено.
>
>


## <a name="subnet-selection"></a>Выбор подсети
Подсеть целевой виртуальной машины выбирается на основе имени подсети исходной виртуальной машины. Если доступна подсеть, имеющая одинаковое имя с исходной виртуальной машиной в целевой сети, она будет установлена для целевой виртуальной машины. Когда в целевой сети нет подсети с тем же именем, в качестве целевой подсети будет выбрана первая подсеть в алфавитном порядке.

Чтобы изменить подсеть, перейдите в раздел параметров **Compute and Network** (Вычисления и сеть) для виртуальной машины.

![Окно вычислительных параметров Compute and Network (Вычисления и сеть)](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-адрес

IP-адрес для каждого сетевого интерфейса целевой виртуальной машины задается, как описано в следующих разделах.

### <a name="dhcp"></a>DHCP
Если сетевой интерфейс исходной виртуальной машины использует DHCP, то сетевому интерфейсу целевой виртуальной машины будет также задан DHCP.

### <a name="static-ip-address"></a>Статический IP-адрес
Если сетевой интерфейс исходной виртуальной машины использует статический IP-адрес, то сетевому интерфейсу целевой виртуальной машины будет также задан статический IP-адрес. В следующих разделах описано, как задать статический IP-адрес.

### <a name="ip-assignment-behavior-during-failover"></a>Реакция на событие назначенного IP-адреса во время отработки отказа
#### <a name="1-same-address-space"></a>1. Одинаковое пространство адресов

Если исходная и целевая подсети имеют одинаковое адресное пространство, в качестве целевого будет использоваться тот же IP-адрес, что и в сетевом интерфейсе исходной виртуальной машины. Если же этот IP-адрес недоступен, в качестве целевого будет задан следующий доступный IP-адрес.

#### <a name="2-different-address-spaces"></a>2. Разные адресные пространства

Если исходная и целевая подсети имеют разные адресные пространства, в качестве целевого будет задан следующий доступный IP-адрес в целевой подсети.


### <a name="ip-assignment-behavior-during-test-failover"></a>Реакция на событие назначенного IP-адреса во время тестовой отработки отказа
#### <a name="1-if-the-target-network-chosen-is-the-production-vnet"></a>1. Если выбранная целевая сеть является рабочей виртуальной сетью
- IP-адрес для восстановления (целевой IP-адрес) станет статическим IP-адресом, при этом он **будет отличаться от IP-адреса**, зарезервированного для отработки отказа.
- Значению назначенного IP-адреса будет соответствовать значение следующего IP-адреса, доступного с конца диапазона адресов подсети.
- Например, если статический IP-адрес исходной виртуальной машины сконфигурирован как 10.0.0.19 и попытка проверки отказа была выполнена из сконфигурированной производственной сети ***dr-PROD-nw*** с диапазоном подсети 10.0.0.0/24. </br>
Виртуальной машине, на которой была выполнена отработка отказа, будет назначен следующий доступный IP-адрес с конца диапазона адресов подсети, например 10.0.0.254 </br>

**Примечание.** Терминология **рабочей виртуальной сети** относится к "целевой сети", сопоставляемой во время конфигурации аварийного восстановления.
####<a name="2-if-the-target-network-chosen-is-not-the-production-vnet-but-has-the-same-subnet-range-as-production-network"></a>2. Если выбранная целевая сеть не является рабочей виртуальной сетью, но обладает тем же диапазоном подсетей, что и рабочая сеть 

- IP-адрес для восстановления (целевой IP-адрес) станет статическим IP-адресом **с тем же IP-адресом** (например, настроенный статический IP-адрес), который был зарезервирован для отработки отказа. При условии наличия одного и того же IP-адреса.
- Если настроенный статический IP-адрес уже назначен другой виртуальной машине или устройству, то IP-адрес восстановления будет следующим доступным IP-адресом с конца диапазона адресов подсети.
- Например, если статический IP-адрес исходной виртуальной машины сконфигурирован как 10.0.0.19 и попытка тестовой отработки отказа была выполнена из тестовой сети ***dr-NON-PROD-nw*** с таким же диапазоном подсети, как и у производственной сети — 10.0.0.0/24. </br>
  Виртуальной машине, на которой выполнялась отработка отказа, будет присвоен следующий статический IP-адрес </br>
    - настроенный статический IP-адрес: 10.0.0.19, если он доступен.
    - Если IP-адрес 10.0.0.19 уже используется, следующим доступным IP-адресом станет 10.0.0.254.


Чтобы изменить целевой IP-адрес каждого сетевого интерфейса, перейдите в раздел параметров **Compute and Network** (Вычисления и сеть) для виртуальной машины.</br>
Для выполнения тестовой отработки отказа всегда рекомендуется выбирать тестовую сеть.
## <a name="next-steps"></a>Дополнительная информация

* Ознакомьтесь со статьей [Сети для репликации Azure — Azure](site-recovery-azure-to-azure-networking-guidance.md)
