---
title: Часто задаваемые вопросы о Службах мультимедиа Azure версии 3 | Документация Майкрософт
description: В этой статье предоставлены ответы на часто задаваемые вопросы о Службах мультимедиа Azure версии 3
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/29/2018
ms.author: juliako
ms.openlocfilehash: 098a34aba8e5ce23f64d4bb07e3b9622aa2adb8e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110426"
---
# <a name="azure-media-services-v3-preview-frequently-asked-questions"></a>Часто задаваемые вопросы о Службах мультимедиа Azure версии 3 (предварительная версия)

В этой статье предоставлены ответы на часто задаваемые вопросы о Службах мультимедиа Azure (AMS) версии 3.

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Могу ли я использовать портал Azure для управления ресурсами версии 3?

Пока нет. Вы можете использовать один из поддерживаемых пакетов SDK. Ознакомьтесь с руководствами и примерами в этом наборе документов.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Существует ли API для настройки зарезервированных единиц мультимедиа?

Команда разработчиков служб мультимедиа исключает ЕЗ в версии 3. Однако необходимая работа по обслуживанию не завершена. До тех пор клиенты должны использовать портал Azure или API AMS версии 2 для настройки ЕЗ (как описано в статье [Обзор масштабирования обработки мультимедиа](../previous/media-services-scale-media-processing-overview.md). 

Если используется **VideoAnalyzerPreset** и (или) **AudioAnalyzerPreset**, настройте 10 зарезервированных единиц мультимедиа S3 для учетной записи Служб мультимедиа.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>У ресурсов версии 3 нет концепции AssetFile?

Элементы AssetFiles были удалены из API AMS, чтобы отделить Службы мультимедиа от зависимости пакета SDK службы хранилища. Теперь служба хранилища, а не службы мультимедиа, сохраняет данные, которые относятся к службе хранилища. 

## <a name="where-did-client-side-storage-encryption-go"></a>Где происходит шифрование хранилища на стороне клиента?

Теперь мы рекомендуем использовать шифрование хранилища на стороне сервера (которое включено по умолчанию). Дополнительные сведения см. в статье [Шифрование службы хранилища Azure для неактивных данных (предварительная версия)](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>Каков рекомендуемый метод загрузки?

Мы рекомендуем использовать входные данные HTTP. Дополнительные сведения см. в статье [Создание входных данных задания из URL-адреса HTTP (HTTPS)](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Как работает разбиение на страницы?

Службы мультимедиа поддерживают запрос $top для ресурсов, поддерживающих OData, но значение, переданное в $top, должно быть меньше 1000 (например, размер страницы для разбиения на страницы).

Это позволяет либо получить небольшой пример элементов, используя запрос $top (например, 100 самых последних элементов), либо разбить все элементы, используя разбиение на страницы. 

Службы мультимедиа не поддерживают разбиение данных на страницы с помощью указанного пользователем размера страницы.

Дополнительные сведения см. в разделе [Поддержка фильтрации, упорядочения и разбиения на страницы](assets-concept.md#filtering-ordering-paging).

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Как получить сущность в Службах мультимедиа версии 3?

Версия 3 создана на основе унифицированной области API, что позволяет использовать функции управления и операции, встроенные в **Azure Resource Manager**. В соответствии с **Azure Resource Manager** имена ресурсов всегда уникальны. Таким образом, вы можете использовать любые уникальные строки идентификаторов (например, GUID) для имен ваших ресурсов. 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Что такое Службы мультимедиа Azure версии 3?](media-services-overview.md)
