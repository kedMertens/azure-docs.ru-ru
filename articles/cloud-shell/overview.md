---
title: Обзор Azure Cloud Shell | Документация Майкрософт
description: Обзор Azure Cloud Shell.
services: ''
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: juluk
ms.openlocfilehash: 9588bebdc827760f0e0d3e2aadccbff5f24723f1
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258931"
---
# <a name="overview-of-azure-cloud-shell"></a>Обзор Azure Cloud Shell
Azure Cloud Shell — это интерактивная доступная браузеру оболочка для управления ресурсами Azure.
Она предоставляет гибкие возможности при выборе оболочки, соответствующей вашим методам работы.
Пользователи Linux могут использовать Bash, а пользователи Windows — PowerShell.

Выполните запуск через shell.azure.com, нажав кнопку ниже.

[![](https://shell.azure.com/images/launchcloudshell.png "Запуск Azure Cloud Shell")](https://shell.azure.com)

Выполните запуск через портал Azure с помощью значка Cloud Shell.

![Запуск с помощью портала](media/overview/portal-launch-icon.png)

## <a name="features"></a>Функции
### <a name="browser-based-shell-experience"></a>Оболочка на основе браузера
Cloud Shell предоставляет доступ к браузерному интерфейсу командной строки, созданному с учетом задач управления Azure.
Используйте Cloud Shell для работы из локального компьютера с неограниченными возможностями, которые может обеспечить только облако.

### <a name="choice-of-preferred-shell-experience"></a>Выбор предпочтительной оболочки
Пользователи Linux могут использовать Bash в Cloud Shell, а пользователи Windows могут выбрать PowerShell в Cloud Shell (предварительная версия) в раскрывающемся списке оболочек.

![Bash в Cloud Shell](media/overview/overview-bash-pic.png)

![PowerShell в Cloud Shell (предварительная версия)](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Настроенная и аутентифицированная рабочая станция Azure
Службой Cloud Shell управляет Майкрософт, поэтому в ней присутствуют популярные программы командной строки и реализована поддержка различных языков. Кроме того, Cloud Shell автоматически и безопасно проходит аутентификацию, тем самым обеспечивая мгновенный доступ к ресурсам с помощью Azure CLI 2.0 или командлетов Azure PowerShell.

Просмотрите полный список [средств, установленных в Cloud Shell.](features.md#tools)

### <a name="integrated-cloud-shell-editor"></a>Интегрированный редактор Cloud Shell
Cloud Shell предоставляет интегрированный графический текстовый редактор на основе редактора Monaco с открытым кодом. Просто создайте и измените файлы конфигурации, выполнив `code .` для простого развертывания с помощью Azure CLI 2.0 или Azure PowerShell.

[Подробнее о редакторе Cloud Shell.](using-cloud-shell-editor.md)

### <a name="multiple-access-points"></a>Доступность в нескольких расположениях
Cloud Shell представляет собой гибкий инструмент, который можно использовать с помощью таких элементов:
* [portal.azure.com](https://portal.azure.com);
* [shell.azure.com](https://shell.azure.com);
* [ознакомительная документация по Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest);
* [мобильные приложения Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/);
* [расширение VS Code для учетной записи Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

### <a name="connect-your-microsoft-azure-files-storage"></a>Подключение хранилища файлов Microsoft Azure
Компьютеры Cloud Shell являются временными и требуют подключения файлового ресурса Azure как `clouddrive` для хранения файлов.

При первом запуске Cloud Shell предлагает создать группу ресурсов, учетную запись хранения и файловый ресурс Azure от вашего имени. Это одноразовое действие, которое автоматически применяется для всех сеансов. Вы можете подключить один файловый ресурс, который будет использоваться как при работе с Bash, так и при работе с PowerShell в Cloud Shell (предварительная версия).

#### <a name="create-new-storage"></a>Создание хранилища
![](media/overview/basic-storage.png)

Учетная запись локально избыточного хранилища (LRS) и файловый ресурс Azure могут быть созданы от вашего имени. Файловый ресурс Azure будет использоваться для сред Bash и PowerShell, если вы решите использовать обе эти среды. Применяются расходы на обычное хранение.

От вашего имени будет создано три ресурса:
1. Группа ресурсов с именем: `cloud-shell-storage-<region>`
2. Учетная запись хранения с именем: `cs<uniqueGuid>`
3. Общая папка с именем: `cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash в Cloud Shell также создает образ диска по умолчанию размером в 5 ГБ для хранения `$Home`. Все файлы в каталоге $Home, такие как ключи SSH, сохраняются в образе диска пользователя, который хранится в подключенном файловом ресурсе Azure. Соблюдайте рекомендации при сохранении файлов в каталоге $Home и подключенном файловом ресурсе Azure.

#### <a name="use-existing-resources"></a>Использование существующих ресурсов
![](media/overview/advanced-storage.png)

В Cloud Shell доступна дополнительная возможность, которая позволяет связывать существующие ресурсы с Cloud Shell.
При появлении запроса на настройку хранилища щелкните "Показать дополнительные настройки", чтобы отобразить дополнительные параметры.

> [!Note]
> Раскрывающиеся меню фильтруются с учетом предварительно назначенного региона Cloud Shell и учетных записей локально или глобально избыточных хранилищ, либо хранилищ, избыточных в пределах зоны.

[Дополнительные сведения о хранилище Cloud Shell, обновлении файловых ресурсов Azure, а также о передаче и скачивании файлов](persisting-shell-storage.md).

## <a name="concepts"></a>Основные понятия
* Cloud Shell работает на временном узле, предоставляемом для каждого сеанса и для каждого пользователя отдельно.
* Время ожидания Cloud Shell истекает через 20 минут при отсутствии интерактивных действий.
* Для Cloud Shell требуется подключение файлового ресурса Azure.
* Cloud Shell использует один и тот же файловый ресурс Azure для Bash и PowerShell.
* Cloud Shell назначается один компьютер на учетную запись пользователя.
* Cloud Shell сохраняет каталог $Home с помощью образа размером 5 ГБ, размещенного в общей папке.
* Разрешения задаются как для обычного пользователя Linux в Bash.

Узнайте больше о возможностях [Bash в Cloud Shell](features.md) и [PowerShell в Cloud Shell (предварительная версия)](features-powershell.md).

## <a name="pricing"></a>Цены
За использование компьютера, на котором размещена среда Cloud Shell, плата не взимается. На этом компьютере должен быть подключен файловый ресурс Azure. Применяются расходы на обычное хранение.

## <a name="next-steps"></a>Дополнительная информация
[Краткое руководство по использованию Bash в Azure Cloud Shell](quickstart.md) <br>
[Краткое руководство по использованию PowerShell в Cloud Shell (предварительная версия)](quickstart-powershell.md)