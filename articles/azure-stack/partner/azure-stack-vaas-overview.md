---
title: Общие сведения о модели "проверка как услуга" для Azure Stack | Документация Майкрософт
description: Общие сведения об известных проблемах модели "проверка как услуга" для Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 56251245a23df031f3bc8fe3d36de43e194fbcc7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159679"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>Что такое проверка как услуга для Azure Stack?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Проверка как услуга (VaaS) — это собственная служба Azure, предназначенная для партнеров по разработке решений, которые создают предложения Azure Stack совместно с корпорацией Майкрософт. Партнеры по разработке решений могут использовать эту службу, чтобы проверить, соответствуют ли их решения требованиям Майкрософт и работают ли они с Azure Stack должным образом.

Основные сценарии использования VaaS:

- Проверка новых решений Azure Stack.
- Проверка изменений, внесенных в программное обеспечение Azure Stack.
- Получение пакетов партнерских решений с цифровой подписью, используемых во время развертывания.
- Вспомогательная проверка предварительной версии Azure Stack.

## <a name="validate-new-azure-stack-solution"></a>Проверка нового решения Azure Stack

Партнеры используют рабочий процесс проверки решения для новых решений Azure Stack. Решение должно пройти необходимые тесты компонентов Hardware Lab Kit (HKL) Azure Stack. Вам нужно всего лишь дважды запустить рабочий процесс для каждого нового решения: раз для минимальной и раз для максимальной конфигурации.

Дополнительные сведения см. в статье [Проверка нового решения Azure Stack](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Проверка изменений, внесенных в программное обеспечение Azure Stack

Партнеры используют рабочий процесс проверки пакетов, чтобы проверить, что их решение работает с последним обновлением программного обеспечения Azure Stack. Как минимум, рабочий процесс проверки пакета должен выполняться в аппаратной среде, рекомендованной Майкрософт, и в решении, в котором были применены исправления и обновления. Вы должны запустить проверку пакетов в базовой сборке.

Дополнительные сведения см. в статье [Проверка обновлений программного обеспечения от корпорации Майкрософт](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Получение пакетов партнерских решений с цифровой подписью

В дополнение к проверке обновлений Azure Stack вы можете использовать рабочий процесс проверки пакетов, чтобы проверять обновления пакетов настройки OEM, включая драйверы, связанные с партнерами Azure Stack, встроенное ПО и другое программное обеспечение, используемое во время развертывания программного обеспечения Azure Stack. Разверните проверяемый пакет в текущей версии программного обеспечения Azure Stack, используя хотя бы решение минимального размера, которое будет поддерживаться. Пакет обновления должен быть загружен в службу до начала теста. Если тесты выполнены успешно, напишите нам по адресу vaashelp@microsoft.com. Сообщите партнеру Azure Stack, что пакет завершил тестирование и должен быть подписан с помощью цифровой подписи Azure Stack. Корпорация Майкрософт подписывает пакет и уведомляет партнера Azure Stack о том, что пакет доступен для скачивания на портале.

Дополнительные сведения см. в статье [Проверка пакетов OEM](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-azure-stack-validation-collateral"></a>Вспомогательная проверка предварительной версии Azure Stack.

Корпорация Майкрософт регулярно создает новые функции для Azure Stack. В рамках процесса разработки для выпуска этих функций на рынок новые вспомогательные тесты добавляются в рабочий процесс выполнения тестирования. Рабочий процесс выполнения тестирования включает в себя вспомогательные тесты из других рабочих процессов, что позволяет выполнять неофициальные проверки. Не используйте рабочий процесс выполнения тестирования для подачи результатов на утверждение. Чтобы получить официальное утверждение вашего решения, используйте рабочий процесс проверки решений и пакетов.

## <a name="next-steps"></a>Дополнительная информация

- Инструкции по началу работы см. в статье [Проверка нового решения Azure Stack](azure-stack-vaas-validate-solution-new.md).
