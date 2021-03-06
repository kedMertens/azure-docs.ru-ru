---
title: Руководство по MongoDB, Angular и Node для Azure | Документация Майкрософт
description: Узнайте, как создать в Azure Cosmos DB приложение MongoDB с помощью Angular и Node и тех же API-интерфейсов, которые используются для MongoDB в этой серии руководств с видео.
services: cosmos-db
author: johnpapa
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: jopapa
ms.custom: mvc
ms.openlocfilehash: 1a9d608e7f959b3fc164f87d408ccd268e8d2568
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2018
ms.locfileid: "41920610"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db"></a>Создание приложения MongoDB с помощью Angular и Azure Cosmos DB 

Из этого руководства в нескольких частях вы узнаете, как создать приложение [API MongoDB](mongodb-introduction.md) с использованием Express, Angular и Node.js (стек MEAN) и подключить его к базе данных Azure Cosmos DB. Azure Cosmos DB поддерживает клиентские подключения MongoDB. Поэтому вместо MongoDB можно использовать базу данных Azure Cosmos DB, применяя тот же код, что и для приложений MongoDB, но с дополнительными преимуществами. Среди таких преимуществ Azure Cosmos DB — простое развертывание в облаке, масштабирование, защита, глобальная репликация данных, поддержка нескольких моделей и сверхбыстрое выполнение операций чтения и записи. 

Azure Cosmos DB — это глобально распределенная многомодельная служба базы данных Майкрософт. Она позволяет быстро создавать и запрашивать документы, пары "ключ — значение" и базы данных графов, используя преимущества глобального распределения и горизонтального масштабирования Azure Cosmos DB. 

В этом руководстве из нескольких частей рассматриваются следующие задачи:

> [!div class="checklist"]
> * [создание приложения Node.js Express с помощью Angular CLI](tutorial-develop-mongodb-nodejs-part2.md);
> * [создание пользовательского интерфейса с помощью Angular](tutorial-develop-mongodb-nodejs-part3.md);
> * [создание учетной записи Azure Cosmos DB с помощью Azure CLI](tutorial-develop-mongodb-nodejs-part4.md); 
> * [подключение к Azure Cosmos DB с помощью Mongoose](tutorial-develop-mongodb-nodejs-part5.md);
> * [добавление в приложение функций Post, Put и Delete](tutorial-develop-mongodb-nodejs-part6.md).

Хотите создать такое же приложение с помощью React? См. [серию видеоруководств по React](tutorial-develop-mongodb-react.md).

## <a name="video-walkthrough"></a>Видеоруководство

> [!VIDEO https://www.youtube.com/embed/vlZRP0mDabM]

## <a name="finished-project"></a>Готовый проект 

В этом руководстве изложены пошаговые инструкции по созданию приложения. Готовое приложение можно скачать из [репозитория angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) на GitHub.

## <a name="next-steps"></a>Дополнительная информация

В этой части руководства мы выполнили следующую задачу:

> [!div class="checklist"]
> * ознакомились с общими инструкциями по созданию приложения MEAN.js с помощью Azure Cosmos DB. 

Теперь можно приступить к следующей части руководства, чтобы создать приложение Node.js Express.

> [!div class="nextstepaction"]
> [Создание приложения Node.js Express с помощью Angular CLI](tutorial-develop-mongodb-nodejs-part2.md)
