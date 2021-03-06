---
title: Пользовательская служба DNS для Управляемого экземпляра Базы данных SQL Azure | Документация Майкрософт
description: В этой статье описаны параметры конфигурации пользовательской службы DNS для Управляемого экземпляра Базы данных SQL Azure.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 09/23/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 2d1bb7e8522da32dd33933261ea41b578f8afac1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949491"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Настройка пользовательской службы DNS для Управляемого экземпляра Базы данных SQL Azure

Управляемый экземпляр базы данных SQL Azure должен быть развернут в [виртуальной сети (VNet)](../virtual-network/virtual-networks-overview.md). Существует несколько сценариев (например, связанные серверы с другими экземплярами SQL в облачной или гибридной среде), для которых требуется, чтобы имена частных узлов разрешались из Управляемого экземпляра. В таком случае в Azure необходимо настроить пользовательскую службу DNS. Так как для внутренней работы в Управляемом экземпляре используется одна служба DNS, конфигурация DNS виртуальной сети должна быть совместима с Управляемым экземпляром. 

Чтобы обеспечить совместимость конфигурации пользовательской службы DNS с Управляемым экземпляром, выполните следующие действия: 
- Настроить пользовательскую службу DNS-сервера для разрешения имен общедоступных доменов. 
- Поместить IP-адрес DNS рекурсивного сопоставителя Azure 168.63.129.16 в конец списка DNS виртуальной сети. 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Настройка конфигурации пользовательской службы DNS-сервера

1. На портале Azure найдите параметр "Пользовательская служба DNS" для своей виртуальной сети.

   ![Параметр "Пользовательская служба DNS"](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Установите переключатель в положение Custom (Пользовательская) и введите пользовательский IP-адрес DNS-сервера, а также IP-адрес рекурсивных сопоставителей Azure 168.63.129.16. 

   ![Параметр "Пользовательская служба DNS"](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Если не указать в списке DNS рекурсивный сопоставитель Azure, Управляемый экземпляр перейдет в состояние сбоя. Чтобы восстановить решение из этого состояния, возможно, в виртуальной сети потребуется создать новый экземпляр с соответствующими политиками сети и данные уровня экземпляра, а также восстановить базы данных. См. статью о [конфигурации виртуальной сети](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения см. в статье [Общие сведения об Управляемом экземпляре Базы данных SQL Azure (предварительная версия)](sql-database-managed-instance.md).
- См. дополнительные сведения о [создании Управляемого экземпляра](sql-database-managed-instance-get-started.md).
- См. дополнительные сведения о [настройке виртуальной сети для Управляемого экземпляра](sql-database-managed-instance-vnet-configuration.md).
