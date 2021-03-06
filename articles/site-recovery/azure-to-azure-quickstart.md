---
title: Репликация виртуальной машины Azure в другой регион Azure
description: В этом кратком руководстве приводятся действия по репликации виртуальной машины Azure из одного региона Azure в другой.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 07/06/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: abba75e731d2550b4719eec70d475884bd7f3c8e
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2018
ms.locfileid: "42023463"
---
# <a name="replicate-an-azure-vm-to-another-azure-region"></a>Репликация виртуальной машины Azure в другой регион Azure

Служба [Azure Site Recovery](site-recovery-overview.md) помогает реализовать стратегию непрерывности бизнес-процессов и аварийного восстановления (BCDR), обеспечивая работоспособность бизнес-приложений во время запланированных и незапланированных простоев. Site Recovery управляет аварийным восстановлением локальных компьютеров и виртуальных машин Azure, включая операции репликации, отработки отказа и восстановления.

В этом кратком руководстве описывается процесс репликации виртуальной машины Azure в другой регион Azure.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.



## <a name="log-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу http://portal.azure.com.

## <a name="enable-replication-for-the-azure-vm"></a>Включение репликации виртуальной машины Azure

1. На портале Azure щелкните **Виртуальные машины**, а затем выберите виртуальную машину, которую требуется реплицировать.

2. В разделе **Операции** щелкните **Аварийное восстановление**.
3. В разделе **Настройка аварийного восстановления** > **Целевой регион** выберите целевой регион, в который будет выполняться репликация.
4. Для выполнения действий в этом кратком руководстве примите другие значения по умолчанию.
5. Щелкните **Включить репликацию**. Будет запущено задание для включения репликации виртуальной машины.

    ![включение репликации](media/azure-to-azure-quickstart/enable-replication1.png)



## <a name="verify-settings"></a>Проверка параметров

После завершения задания репликации можно проверить состояние репликации, изменить параметры настройки репликации и протестировать развертывание.

1. В меню виртуальной машины щелкните **Аварийное восстановление**.
2. Можно проверить работоспособность репликации, созданные точки восстановления и исходный и целевой регионы на карте.

   ![Состояние репликации](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Репликация виртуальной машины в основном регионе останавливается при отключении репликации:

- Параметры репликации в источнике автоматически очищаются.
- Выставление счетов Site Recovery за виртуальную машину также прекращается.

Остановите репликацию следующим образом.

1. Выберите виртуальную машину.
2. Выберите **Аварийное восстановление** и щелкните **Отключить репликацию**.

   ![Отключение репликации](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы реплицировали виртуальную машину в дополнительный регион.

> [!div class="nextstepaction"]
> [Настройка аварийного восстановления для виртуальных машин Azure](azure-to-azure-tutorial-enable-replication.md)
