---
title: Копирование данных с помощью средства копирования данных Azure | Документация Майкрософт
description: Создание фабрики данных Azure и применение средства копирования данных для копирования данных из хранилища BLOB-объектов Azure в базу данных SQL.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: jingwang
ms.openlocfilehash: 1be4769a8a07ac5d4a968ed5aa15ed2e0a2b6db2
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "43086832"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-the-copy-data-tool"></a>Копирование данных из хранилища BLOB-объектов Azure в базу данных SQL Azure с помощью средства копирования данных
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Версия 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Текущая версия](tutorial-copy-data-tool.md)

В этом руководстве вы создадите фабрику данных с помощью портала Azure. После этого вы примените средство копирования данных для копирования данных из хранилища BLOB-объектов Azure в базу данных SQL. 

> [!NOTE]
> Если вы еще не работали с фабрикой данных Azure, ознакомьтесь со статьей [Введение в фабрику данных Azure](introduction.md).

Вот какие шаги выполняются в этом учебнике:

> [!div class="checklist"]
> * Создали фабрику данных.
> * Создание конвейера с помощью средства копирования данных.
> * Мониторинг конвейера и выполнения действий.

## <a name="prerequisites"></a>Предварительные требования

* **Подписка Azure**. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/), прежде чем начинать работу.
* **Учетная запись хранения Azure**. В этом руководстве в качестве _исходного_ используйте хранилище BLOB-объектов. Если у вас нет учетной записи хранения Azure, см. инструкции по [ее созданию](../storage/common/storage-quickstart-create-account.md).
* **База данных SQL Azure**. Базу данных SQL Azure используйте в качестве _принимающего_ хранилища данных. Если у вас нет базы данных SQL, см. инструкции по [ее созданию](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Создание большого двоичного объекта и таблицы SQL

Подготовьте хранилище BLOB-объектов и базу данных SQL к изучению этого руководства, выполнив следующие действия.

#### <a name="create-a-source-blob"></a>Создание исходного большого двоичного объекта

1. Запустите **Блокнот**. Скопируйте следующий текст и сохраните его в файл с именем **inputEmp.txt** на диске.

    ```
    John|Doe
    Jane|Doe
    ```

1. Создайте контейнер **adfv2tutorial** и отправьте в него файл inputEmp.txt. Это можно сделать при помощи таких средств, как [обозреватель службы хранилища Azure](http://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Создание таблицы-приемника SQL

1. Используйте следующий скрипт SQL, чтобы создать таблицу **dbo.emp** в базе данных SQL.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

1. Предоставьте службам Azure доступ к серверу SQL Server. Убедитесь, что параметр **Разрешить доступ к службам Azure** включен для вашего сервера SQL Server. Этот параметр позволяет фабрике данных записывать данные в экземпляр SQL Server. Чтобы проверить и при необходимости включить этот параметр, сделайте следующее.

    a. Слева выберите **Больше служб**, а затем — **Серверы SQL Server**.

    b. Выберите свой сервер, а затем — **Параметры** > **Брандмауэр**.

    c. На странице **Параметры брандмауэра** выберите **Вкл.** для параметра **Разрешить доступ к службам Azure**.

## <a name="create-a-data-factory"></a>Создание фабрики данных

1. В меню слева выберите **+ Создать** > **Данные и аналитика** > **Фабрика данных**. 
   
   ![Создание фабрики данных](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
1. На странице **Новая фабрика данных** в поле **Имя** введите **ADFTutorialDataFactory**. 
      
     ![Новая фабрика данных](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Имя фабрики данных должно быть _глобально уникальным_. Вы можете получить следующее сообщение об ошибке.
   
   ![Сообщение об ошибке фабрики данных](./media/tutorial-copy-data-tool/name-not-available-error.png)

   Если вы увидите следующую ошибку касательно значения имени, введите другое имя фабрики данных. Например, _**ваше_имя**_**ADFTutorialDataFactory**. Правила именования для артефактов службы "Фабрика данных" см. в [этой](naming-rules.md) статье.
1. Выберите **подписку** Azure, в которой нужно создать фабрику данных. 
1. Для **группы ресурсов** выполните одно из следующих действий:
     
    a. Выберите **Использовать существующую**и укажите существующую группу ресурсов в раскрывающемся списке.

    b. Выберите **Создать новую**и укажите имя группы ресурсов. 
         
    Сведения о группах ресурсов см. в статье [Общие сведения об Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

1. В качестве **версии** выберите **V2**.
1. В качестве **расположения** выберите расположение фабрики данных. В раскрывающемся списке отображаются только поддерживаемые расположения. Хранилища данных (например, служба хранилища Azure и база данных SQL) и вычислительные ресурсы (например, Azure HDInsight), используемые фабрикой данных, могут располагаться в других регионах или расположениях.
1. Кроме того, установите флажок **Закрепить на панели мониторинга**. 
1. Нажмите кнопку **Создать**.
1. На панели мониторинга на плитке **Deploying Data Factory** (Развертывание фабрики данных) отображается состояние процесса.

    ![Плитка Deploying data factory (Развертывание фабрики данных)](media/tutorial-copy-data-tool/deploying-data-factory.png)
1. Когда создание завершится, откроется домашняя страница **Фабрика данных**.
   
    ![Домашняя страница фабрики данных](./media/tutorial-copy-data-tool/data-factory-home-page.png)
1. Щелкните плитку **Author & Monitor** (Создание и мониторинг), чтобы запустить на отдельной вкладке пользовательский интерфейс фабрики данных Azure. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Создание конвейера с помощью средства копирования данных

1. Чтобы запустить средство копирования данных, на странице **Let's get started** (Начало работы) выберите плитку **Копирование данных**. 

   ![Плитка средства копирования данных](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
1. На странице **Свойства** в разделе **Имя задачи** введите **CopyFromBlobToSqlPipeline**. Затем нажмите кнопку **Далее**. Пользовательский интерфейс фабрики данных создаст конвейер с указанным именем задачи. 

    ![Страница свойств](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
1. На странице **Исходное хранилище данных** сделайте следующее:

    a. Щелкните **+Создать подключение**, чтобы добавить подключение.

    ![Новая связанная служба-источник](./media/tutorial-copy-data-tool/new-source-linked-service.png)

    b. В коллекции выберите **Хранилище BLOB-объектов Azure**, а затем нажмите кнопку **Далее**.

    ![Выбор источника больших двоичных объектов](./media/tutorial-copy-data-tool/select-blob-source.png)

    c. На странице **New Linked Service** (Новая связанная служба) выберите учетную запись из списка **Имя учетной записи хранения**, а затем щелкните **Готово**.

    ![Настройка хранилища Azure](./media/tutorial-copy-data-tool/configure-azure-storage.png)

    d. Выберите созданную связанную службу в качестве источника, а затем нажмите кнопку **Далее**.

    ![Выбор исходной связанной службы](./media/tutorial-copy-data-tool/select-source-linked-service.png)

1. На странице **Choose the input file or folder** (Выбор входного файла или папки) выполните следующие действия:
    
    a. Нажмите кнопку **Обзор**, чтобы перейти к папке **adfv2tutorial/input**, выделите файл **inputEmp.txt** и щелкните **Выбрать**.

    ![Выбор файла или папки входных данных](./media/tutorial-copy-data-tool/specify-source-path.png)

    b. Чтобы перейти к следующему шагу, нажмите кнопку **Далее**.

1. На странице **File format settings** (Параметры формата файла) убедитесь, что средство правильно автоматически определило разделители столбцов и строк. Щелкните **Далее**. Кроме того, на этой странице вы можете просмотреть данные и схему входных данных. 

    ![Параметры формата файла](./media/tutorial-copy-data-tool/file-format-settings-page.png)
1. На странице **Целевое хранилище данных** сделайте следующее:

    a. Щелкните **+Создать подключение**, чтобы добавить подключение.

    ![Новая связанная служба-приемник](./media/tutorial-copy-data-tool/new-sink-linked-service.png)

    b. В коллекции выберите **Хранилище BLOB-объектов Azure**, а затем нажмите кнопку **Далее**.

    ![Выбор Базы данных SQL Azure](./media/tutorial-copy-data-tool/select-azure-sql-db.png)

    c. На странице **New Linked Service** (Новая связанная служба) выберите из раскрывающегося списка имя сервера и имя базы данных, укажите имя пользователя и пароль, а затем щелкните **Готово**.    

    ![Настройка Базы данных SQL Azure](./media/tutorial-copy-data-tool/config-azure-sql-db.png)

    d. Выберите созданную связанную службу в качестве приемника, а затем нажмите кнопку **Далее**.

    ![Выбор связанной службы-приемника](./media/tutorial-copy-data-tool/select-sink-linked-service.png)

1. На странице **Сопоставление таблицы** выберите **[dbo].[emp]** и нажмите кнопку **Далее**. 

    ![Сопоставление таблицы](./media/tutorial-copy-data-tool/table-mapping.png)
1. На странице **сопоставления схем** вы можете увидеть, что первый и второй столбцы файла входных данных сопоставлены со столбцами **FirstName** и **LastName** в таблице **EMP**. Щелкните **Далее**.

    ![Страница сопоставления схем](./media/tutorial-copy-data-tool/schema-mapping.png)
1. На странице **Параметры** выберите **Далее**. 
1. Просмотрите параметры на странице **Сводка**, а затем нажмите кнопку **Далее**.

    ![Страница "Сводка"](./media/tutorial-copy-data-tool/summary-page.png)
1. На **странице развертывания** выберите **Мониторинг**, чтобы отслеживать созданный конвейер (задачу).

    ![Страница развертывания](./media/tutorial-copy-data-tool/deployment-page.png)
1. Обратите внимание, что слева автоматически выбирается вкладка **Мониторинг**. В столбце **действий** отображаются ссылки для просмотра сведений о запусках действий и (или) повторного выполнения на конвейере. Щелкните **Обновить**, чтобы обновить список. 

    ![Мониторинг выполнений конвейера](./media/tutorial-copy-data-tool/pipeline-monitoring.png)
1. Щелкните ссылку **View Activity Runs** (Просмотр запусков действий) в столбце **Действия**, чтобы просмотреть запуски действий, связанные с этим запуском конвейера. Чтобы увидеть сведения об операции копирования, щелкните ссылку **Сведения** (значок очков) в столбце **Действия**. Чтобы вернуться к представлению **запусков конвейера**, щелкните ссылку **Конвейеры** в верхней части окна. Чтобы обновить список, нажмите кнопку **Обновить**. 

    ![Мониторинг выполнений действий](./media/tutorial-copy-data-tool/activity-monitoring.png)

    ![Копирование сведений о действии](./media/tutorial-copy-data-tool/copy-execution-details.png)

1. Убедитесь, что данные вставлены в таблицу **EMP** в базе данных SQL.

    ![Проверка результатов в SQL](./media/tutorial-copy-data-tool/verify-sql-output.png)

1. Выберите вкладку **Автор** слева, чтобы переключиться в режим правки. В этом редакторе вы можете обновлять параметры связанных служб, наборов данных и конвейеров, созданных с помощью средства. Дополнительные сведения о редактировании этих сущностей с помощью пользовательского интерфейса фабрики данных вы найдете в [версии этого руководства для портала Azure](tutorial-copy-data-portal.md).

## <a name="next-steps"></a>Дополнительная информация
Конвейер из этого примера копирует данные из хранилища BLOB-объектов в базу данных SQL. Вы научились выполнять следующие задачи: 

> [!div class="checklist"]
> * Создали фабрику данных.
> * Создание конвейера с помощью средства копирования данных.
> * Мониторинг конвейера и выполнения действий.

Перейдите к следующему руководству, чтобы узнать о копировании данных из локальной среды в облако: 

> [!div class="nextstepaction"]
>[Копирование данных из локальной базы данных SQL Server в хранилище BLOB-объектов Azure с помощью средства копирования данных](tutorial-hybrid-copy-data-tool.md)
