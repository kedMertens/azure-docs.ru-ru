---
title: Краткое руководство. Создание назначения политики для идентификации ресурсов, которые не соответствуют требованиям, в среде Azure с помощью PowerShell
description: В этом кратком руководстве с помощью PowerShell вы создадите назначение в службе "Политика Azure", позволяющее определить ресурсы, которые не соответствуют требованиям.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 3a71815e3a400b4366a2f60637cb0daab77b1cd6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946176"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-using-the-azure-rm-powershell-module"></a>Краткое руководство. Создание назначения политики для идентификации ресурсов, которые не соответствуют требованиям, с помощью модуля Azure RM PowerShell

Чтобы понять, соответствуют ли ресурсы требованиям в Azure, прежде всего нужно определить их состояние. В этом кратком руководстве вы создадите назначение политики для выявления виртуальных машин, которые не используют управляемые диски. После выполнения задач в руководстве вы сможете выявлять виртуальные машины, которые *не соответствуют* назначению политики.

Модуль AzureRM PowerShell используется для создания и администрирования ресурсов Azure с использованием командной строки или скриптов. В этом руководстве объясняется, как при помощи AzureRM создавать назначение политики. Политика выявляет в среде Azure ресурсы, не соответствующие требованиям.

Если у вас еще нет подписки Azure, создайте [бесплатную](https://azure.microsoft.com/free/) учетную запись Azure, прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

- Установите [ARMClient](https://github.com/projectkudu/ARMClient), если его у вас еще нет. Это средство, которое отправляет HTTP-запросы к API-интерфейсам на основе Azure Resource Manager.
- Перед началом работы убедитесь, что установлена последняя версия PowerShell. Дополнительные сведения см. в статье [Общие сведения об Azure PowerShell](/powershell/azureps-cmdlets-docs).
- Обновите свой модуль AzureRM PowerShell до последней версии. Если вам необходимо выполнить установку или обновление, см. статью [об установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps).
- Зарегистрируйте поставщик ресурсов Policy Insights с помощью Azure PowerShell. Регистрация поставщика ресурсов необходима для надлежащей работы вашей подписки с этим поставщиком. Чтобы зарегистрировать поставщик ресурсов, необходимо иметь разрешение на действие регистрации для поставщика ресурсов. Эта операция включается в роли участника и владельца. Выполните указанную ниже команду для регистрации поставщика ресурсов.

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Дополнительные сведения о регистрации и просмотре поставщиков ресурсов см. в статье [Поставщики и типы ресурсов](../../azure-resource-manager/resource-manager-supported-services.md).

## <a name="create-a-policy-assignment"></a>Создание назначения политики

В этом кратком руководстве вы создадите назначение политики и назначите определение *Audit Virtual Machines without Managed Disks*. Это определение политики идентифицирует ресурсы, которые не соответствуют заданным в нем условиям.

Чтобы создать назначение политики, выполните следующие команды:

```azurepowershell-interactive
$rg = Get-AzureRmResourceGroup -Name '<resourceGroupName>'
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs that do not use managed disks' }
New-AzureRmPolicyAssignment -Name 'audit-vm-manageddisks' -DisplayName 'Audit Virtual Machines without Managed Disks Assignment' -Scope $rg.ResourceId -PolicyDefinition $definition
```

В указанных выше командах используются следующие сведения:

- **Name** — фактическое имя назначения.  В этом примере использовалось имя *audit-vm-manageddisks*.
- **DisplayName** — отображаемое имя назначения политики. В этом случае используйте определение *Audit Virtual Machines without Managed Disks*.
- **Definition.** Определение политики, на основе которого вы создаете назначение. В нашем случае это идентификатор определения политики *Аудит виртуальных машин, которые не используют управляемые диски*.
- **Scope.** Область определяет, к каким ресурсам или группе ресурсов принудительно применяется назначение политики. Политика может назначаться разным ресурсам: от подписки до групп ресурсов. Обязательно замените значение &lt;scope&gt; именем своей группы ресурсов.

Теперь все готово к выявлению ресурсов, которые не соответствуют требованиям, что позволит оценить состояние соответствия в среде.

## <a name="identify-non-compliant-resources"></a>Выявление несоответствующих ресурсов

Используйте приведенные ниже сведения для идентификации ресурсов, которые не соответствуют созданному вами назначению политики. Выполните следующие команды:

```azurepowershell-interactive
$policyAssignment = Get-AzureRmPolicyAssignment | Where-Object { $_.Properties.DisplayName -eq 'Audit Virtual Machines without Managed Disks Assignment' }
$policyAssignment.PolicyAssignmentId
```

Дополнительные сведения об идентификаторах назначения политики см. в описании командлета [Get-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment).

Теперь выполните следующую команду, чтобы вывести идентификаторы несовместимых ресурсов в JSON-файле:

```
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

Результаты должны выглядеть примерно так:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 3,
    "value": [{
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
        }

    ]
}
```

Результаты похожи на содержимое списка **несовместимых ресурсов** в представлении портала Azure.

## <a name="clean-up-resources"></a>Очистка ресурсов

Следующие руководства из этой серии являются продолжением этого документа. Если вы собираетесь продолжать работу с ними, не удаляйте ресурсы, которые создали при работе с этим руководством. В противном случае удалите созданное назначение, выполнив следующую команду:

```azurepowershell-interactive
Remove-AzureRmPolicyAssignment -Name 'audit-vm-manageddisks' -Scope '/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>'
```

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы назначили определение политики для идентификации ресурсов, не соответствующих требованиям, в среде Azure.

Следующее руководство серии содержит сведения о назначении политик, обеспечивающих соответствие требованиям для **новых** ресурсов:

> [!div class="nextstepaction"]
> [Создание политик и управление ими](./tutorials/create-and-manage.md)