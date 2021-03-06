---
title: Статья по обновлению планов и предложений в Azure Stack | Документация Майкрософт
description: В этой статье описан просмотр и изменение имеющихся планов и предложений по Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 07/30/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a35ba993e6fd1162fa4a18bc0d6bc9351fe7dfa2
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358287"
---
# <a name="azure-stack-add-on-plans"></a>Дополнительные планы Azure Stack

Как оператор Azure Stack вы можете создать дополнительные планы, чтобы изменить [*базовый план*](azure-stack-create-plan.md), если вы хотите предложить дополнительные услуги или расширить квоты *компьютера*, *хранилища* или *сети* до значений, превышающих начальное предложение базового плана. Дополнительные планы изменяют базовый план и являются дополнительными расширениями, на которые пользователи могут подписаться. 

Иногда объединение всех предложений в один план является оптимальным решением. В других случаях вы можете использовать базовый план, а затем предложить дополнительные услуги с помощью дополнительных планов. К примеру, можно предложить службы IaaS в составе базового плана, а все службы PaaS – через дополнительные планы.

Еще одна причина использовать дополнительные планы — помочь пользователям в соответствующем использовании ресурсов. Чтобы сделать это, можно начать с базового плана, который включает относительно небольшие квоты (в зависимости от требуемых служб). Затем, когда пользователи достигнут ограничения емкости, они получат предупреждение о том, что использовали распределение ресурсов в рамках назначенного плана. Тогда они могут выбрать дополнительный план, который предоставляет дополнительные ресурсы.

> [!NOTE]
> Если вы не хотите использовать дополнительный план для расширения квоты, вы также можете [изменить исходную конфигурацию квоты](azure-stack-quota-types.md#to-edit-a-quota). 

Если пользователь воспользуется дополнительным планом к основной подписке, тогда соответствующие запрошенные ресурсы появятся в его распоряжении не позднее, чем через час. 

## <a name="create-an-add-on-plan"></a>Создание дополнительного плана
Дополнительные планы создаются путем изменения имеющегося предложения.

1. Войдите на портал администрирования Azure Stack с учетной записью администратора облака.
2. Для создания служб предложения плана, не предлагавшихся ранее, выполните те же действия, что и по [созданию нового базового плана](azure-stack-create-plan.md). В этом примере будут включены в новый план службы Key Vault (Microsoft.KeyVault).
3. На портале администратора щелкните **Предложения**, а затем выберите вариант обновления предложения с использованием дополнительного плана.

   ![](media/create-add-on-plan/1.PNG)

4.  Прокрутите свойства предложения вниз и выберите **Дополнительные планы**. Щелкните **Добавить**.
   
    ![](media/create-add-on-plan/2.PNG)

5. Выберите план для добавления. В этом примере выбран план с именем **План хранилища ключей**. После выбора плана щелкните **Выбрать**, чтобы добавить его в предложение. Вы должны получить уведомление о том, что план успешно добавлен в предложение.
   
    ![](media/create-add-on-plan/3.PNG)

6. Просмотрите список дополнительных планов, включенных в предложение, чтобы убедиться, что новый дополнительный план находится в списке.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Дополнительная информация
[Создание предложения](azure-stack-create-offer.md)