---
title: Развертывание Брандмауэра Azure с помощью шаблона
description: Развертывание Брандмауэра Azure с помощью шаблона
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: d32e6e29c287d140c28206743e36dc025b26158b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991340"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Развертывание Брандмауэра Azure с помощью шаблона

Этот шаблон создает брандмауэр и тестовую сетевую среду. Эта сеть содержит одну виртуальную сеть с тремя подсетями: *AzureFirewallSubnet*, *ServersSubnet* и *JumpboxSubnet*. ServersSubnet и JumpboxSubnet содержат по одному 2-ядерному серверу Windows Server.

В AzureFirewallSubnet размещен брандмауэр, которому присваивается коллекция правил приложения, единственное правило которой разрешает доступ к www.microsoft.com.

Создается пользовательский маршрут, направляющий сетевой трафик из ServersSubnet через брандмауэр, где к нему применяются правила брандмауэра.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="template-location"></a>Расположение шаблона

Шаблон размещается по адресу:

[https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox)

Прочитайте вводную информацию, а когда будете готовы развернуть шаблон, щелкните **Развернуть в Azure**.

## <a name="clean-up-resources"></a>Очистка ресурсов

Изучите все ресурсы, созданные вместе с брандмауэром. Когда группа ресурсов, брандмауэр и все связанные с ними ресурсы вам станут не нужны, удалите их командлетом [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="next-steps"></a>Дополнительная информация

Также вы можете изучить журналы брандмауэра Azure.

- [Tutorial: Monitor Azure Firewall logs](./tutorial-diagnostics.md) (Руководство. Мониторинг журналов брандмауэра Azure)

