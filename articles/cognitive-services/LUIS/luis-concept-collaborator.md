---
title: Общие сведения о совместной работе над приложением LUIS в Azure | Документы Майкрософт
description: Для приложений LUIS требуется один владелец и необязательные участники совместной работы.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: fe5e35c2dcb08cdff9d92142558cf8d7ec81c36c
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399577"
---
# <a name="collaborating"></a>Совместная работа

LUIS предоставляет возможности совместной работы, позволяющие группе людей разрабатывать приложения.

## <a name="luis-account"></a>Учетная запись LUIS
Учетная запись LUIS связана с одной учетной записью [Microsoft Live](https://login.live.com/). Каждой учетной записи LUIS предоставляется бесплатный [ключ разработки](luis-concept-keys.md#authoring-key) для создания всех приложений LUIS, к которым имеет доступ учетная запись. 

У учетной записи LUIS может быть много приложений LUIS.

Дополнительные сведения см. в разделе [Совместная работа с другими пользователями над приложениями Интеллектуальной службы распознавания речи (LUIS)](luis-how-to-collaborate.md#azure-active-directory-tenant-user). 

## <a name="luis-app-owner"></a>Владелец приложения LUIS
Владельцем является учетная запись, которая создает приложение. У каждого приложения есть один владелец. Владелец указан в разделе **[Параметры](luis-how-to-collaborate.md)** приложения. Это учетная запись, которая может удалить приложение. Она также получает сообщение электронной почты, когда квота конечной точки достигает 75-процентного ежемесячного лимита. 

## <a name="transfer-ownership"></a>Передача владения
LUIS не обеспечивает передачу прав собственности, однако любой участник совместной работы может экспортировать приложение, а затем создать приложение, импортировав его. Имейте в виду, что новое приложение имеет другой идентификатор. Новое приложение необходимо обучить и опубликовать. Кроме того, следует использовать новую конечную точку.

## <a name="luis-app-collaborators"></a>Участники совместной работы над приложением LUIS
Владелец приложения может добавлять в приложение участников совместной работы. Ему необходимо добавить адрес электронной почты участника совместной работы в разделе **[Параметры](luis-how-to-collaborate.md)** приложения. Участник совместной работы имеет полный доступ к приложению. Если участник совместной работы удаляет приложение, оно удаляется из учетной записи участника совместной работы, но остается в учетной записи владельца. 

Чтобы предоставить участникам совместной работы доступ к нескольким приложениям, в каждое приложение следует добавить адрес электронной почты участника. 

## <a name="managing-multiple-authors"></a>Управление несколькими разработчиками
Сейчас на веб-сайте [LUIS](luis-reference-regions.md#luis-website) отсутствует возможность разработки на уровне транзакций. Вы можете разрешить разработчикам работать в версиях, независимых от базовой версии. В следующих разделах описаны два разных метода.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Управление несколькими версиями внутри одного приложения
Начните с [клонирования](luis-how-to-manage-versions.md#clone-a-version) из базовой версии. Выполните эту операцию для каждого разработчика. 

Каждый разработчик вносит изменения в собственную версию приложения. Если модель устраивает всех разработчиков, экспортируйте новые версии в JSON-файлы.  

Экспортированные приложения представляют собой файлы формата JSON, в которых можно сравнивать изменения. Объедините файлы, чтобы создать один JSON-файл новой версии. Измените свойство **versionId** в JSON-файле для обозначения новой объединенной версии. Импортируйте эту версию в исходное приложение. 

Этот метод позволяет иметь одну активную версию, одну промежуточную версию и одну опубликованную версию. Вы можете сравнивать результаты на панели интерактивного тестирования в трех версиях.

### <a name="manage-multiple-versions-as-apps"></a>Управление несколькими версиями как приложениями
[Экспортируйте](luis-how-to-manage-versions.md#export-version) базовую версию. Каждый разработчик импортирует версию. Человек, который импортирует приложение, является владельцем версии. Когда разработчики завершат вносить изменения в приложение, экспортируйте версию. 

Экспортированные приложения представляют собой файлы формата JSON, которые можно сравнивать с базовой версией. Объедините файлы, чтобы создать один JSON-файл новой версии. Измените свойство **versionId** в JSON-файле для обозначения новой объединенной версии. Импортируйте эту версию в исходное приложение.

## <a name="next-steps"></a>Дополнительная информация

Поймите принципы [управления версиями](luis-concept-version.md). 

Сведения об управлении участниками совместной работы в приложении LUIS см. в статье о [параметрах приложения](luis-how-to-collaborate.md).

Ознакомьтесь с API разработки в статье о [добавлении адреса электронной почты в список доступа](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342).