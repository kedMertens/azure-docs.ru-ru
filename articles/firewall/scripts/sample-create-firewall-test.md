---
title: Пример сценария Azure PowerShell по созданию тестовой среды брандмауэра Azure
description: Пример сценария Azure PowerShell по созданию тестовой среды брандмауэра Azure.
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 8/13/2018
ms.author: victorh
ms.openlocfilehash: 23f10280cd34927e2e74cb7c5001850bedc6dd35
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967546"
---
# <a name="create-an-azure-firewall-test-environment"></a>Создание тестовой среды брандмауэра Azure

Этот сценарий создает брандмауэр и тестовую сетевую среду. Сеть содержит одну виртуальную сеть с тремя подсетями: *AzureFirewallSubnet*, *ServersSubnet* и *JumpboxSubnet*. ServersSubnet и JumpboxSubnet содержат по одному 2-ядерному серверу Windows Server.

В AzureFirewallSubnet размещен брандмауэр, которому присваивается коллекция правил приложения, единственное правило которой разрешает доступ к www.microsoft.com.

Создается пользовательский маршрут, направляющий сетевой трафик из ServersSubnet через брандмауэр, где к нему применяются правила брандмауэра.

Скрипт можно выполнить из Azure [Cloud Shell](https://shell.azure.com/powershell) или из локальной установки PowerShell. 

При локальном запуске PowerShell для выполнения этого скрипта понадобится модуль AzureRM PowerShell последней версии. Выполните командлет `Get-Module -ListAvailable AzureRM`, чтобы узнать установленную версию. 

Если необходимо выполнить обновление, можно использовать модуль `PowerShellGet`, встроенный в Windows 10 и Windows Server 2016.

> [!NOTE]
>В других версиях Windows необходимо установить модуль `PowerShellGet` перед его использованием. Можно запустить команду `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path`, чтобы определить установлен ли этот модуль на компьютере. Если выходные данные не заполнены, необходимо установить последнюю версию [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616).

Дополнительные сведения см. в статье [Установка Azure PowerShell в ОС Windows с помощью PowerShellGet](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0)

Любая имеющаяся среда Azure PowerShell, установленная с помощью установщика веб-платформы, будет конфликтовать с установкой PowerShellGet, и ее нужно удалить.

Помните, что при локальном запуске PowerShell необходимо также выполнить командлет `Connect-AzureRmAccount`, чтобы создать подключение к Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта


[!code-azurepowershell-interactive[main](../../../powershell_scripts/firewall/create-fw-test.ps1  "Create a firewall test environment")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы:

```powershell
Remove-AzureRmResourceGroup -Name AzfwSampleScriptEastUS -Force
```

## <a name="script-explanation"></a>Описание скрипта

Для создания группы ресурсов, виртуальной сети и групп безопасности сети в этом скрипте используются приведенные ниже команды. Для каждой команды в следующей таблице приведены ссылки на соответствующую документацию:

| Get-Help | Примечания |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Создает объект конфигурации подсети. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Создает виртуальную сеть Azure и интерфейсную подсеть. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Создает правила безопасности, которые будут добавлены в группу безопасности сети. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Создает правила групп безопасности сети, которые разрешают или блокируют определенные порты для конкретных подсетей. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Связывает группы безопасности сети с подсетями. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Создает общедоступный IP-адрес для доступа к виртуальной машине из Интернета. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Создает виртуальные сетевые интерфейсы и присоединяет их к интерфейсной и внутренней подсети виртуальной сети. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Создает конфигурацию виртуальной машины. Эта конфигурация включает в себя такие сведения, как имя виртуальной машины, операционную систему и учетные данные администратора. Данная конфигурации используется при создании виртуальной машины. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Создайте виртуальную машину. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Удаляет группу ресурсов и все ресурсы, содержащиеся в ней. |
|[New-AzureRmFirewall](https://github.com/Azure/azure-powershell/blob/Networking-AzureFirewall/src/ResourceManager/Network/Commands.Network/help/New-AzureRmFirewall.md)| Создает экземпляр службы "Брандмауэр Azure".|
|[Get-AzureRmFirewall](https://github.com/Azure/azure-powershell/blob/Networking-AzureFirewall/src/ResourceManager/Network/Commands.Network/help/Get-AzureRmFirewall.md)|Получает объект "Брандмауэр Azure".|
|[New-AzureRmFirewallApplicationRule](https://github.com/Azure/azure-powershell/blob/Networking-AzureFirewall/src/ResourceManager/Network/Commands.Network/help/New-AzureRmFirewallApplicationRule.md)|Создает правило приложения службы "Брандмауэр Azure".|
|[Set-AzureRmFirewall](https://github.com/Azure/azure-powershell/blob/Networking-AzureFirewall/src/ResourceManager/Network/Commands.Network/help/Set-AzureRmFirewall.md)|Фиксирует изменения в объекте "Брандмауэр Azure".|


## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](/powershell/azure/overview).

