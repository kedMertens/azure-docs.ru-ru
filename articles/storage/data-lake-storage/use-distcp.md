---
title: Копирование данных в хранилище Azure Data Lake поколения 2 (предварительная версия) с помощью DistCp | Документация Майкрософт
description: Использование средства DistCp для копирования данных из хранилища Azure Data Lake поколения 2 (предварительная версия) и обратно
services: storage
author: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 065c4c4315bda209484cc1b2449980e55d4ac798
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522702"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Использование DistCp для копирования данных между большими двоичными объектами службы хранилища Azure и хранилищем Data Lake поколения 2 (предварительная версия)

При наличии кластера HDInsight с доступом к Azure Data Lake Storage 2-го поколения (предварительная версия) можно использовать такие средства экосистемы Hadoop, как [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html), для копирования данных **из системы хранения данных кластера HDInsight (WASB) в учетную запись с поддержкой Data Lake Storage 2-го поколения и обратно**. В этой статье содержатся инструкции по использованию средства DistCp.

## <a name="prerequisites"></a>Предварительные требования

* **Подписка Azure**. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Учетная запись службы хранилища Azure с включенной функцией хранилища Azure Data Lake (предварительная версия)**. Сведения по созданию учетной записи хранения Azure Data Lake Storage 2-го поколения (предварительная версия) см. [здесь](quickstart-create-account.md).
* **Кластер Azure HDInsight** с доступом к учетной записи хранилища Azure Data Lake. См. раздел [Use Azure Data Lake Storage Gen2 with Azure HDInsight clusters](use-hdi-cluster.md) (Использование хранилища Azure Data Lake поколения 2 с кластерами Azure HDInsight). Убедитесь, что вы включили удаленный рабочий стол для кластера.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Использование Distcp из кластера HDInsight на платформе Linux

В состав кластера HDInsight входит служебная программа Distcp, которую можно использовать для копирования данных из различных источников в кластер HDInsight. Если кластер HDInsight настроен для использования хранилища BLOB-объектов Azure вместе с хранилищем Azure Data Lake, служебную программу DistCp можно использовать для копирования данных в обоих направлениях без дополнительной настройки. В этом разделе мы рассмотрим, как использовать служебную программу DistCp.

1. На своем настольном компьютере используйте SSH, чтобы подключиться к кластеру. См. раздел [Подключение к кластеру HDInsight на основе Linux](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Выполните команды в командной строке SSH.

2. Проверьте, доступны ли вам BLOB-объекты хранилища Azure (WASB). Выполните следующую команду:

        hdfs dfs –ls wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/

    Она должна вывести список содержимого в хранилище BLOB-объектов.

3. Аналогичным образом проверьте, доступна ли учетная запись хранилища Azure Data Lake из кластера. Выполните следующую команду:

        hdfs dfs -ls abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/

    Она должна вывести список файлов и папок в учетной записи хранилища Azure Data Lake.

4. Используйте DistCp для копирования данных из WASB в учетную запись хранилища Azure Data Lake.

        hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

    Эта команда копирует содержимое папки **/example/data/gutenberg/**, расположенной в хранилище BLOB-объектов, в папку **/myfolder**, расположенную в учетной записи хранилища Azure Data Lake.

5. Аналогичным образом используйте DistCp для копирования данных из учетной записи хранилища Azure Data Lake в хранилище BLOB-объектов (WASB).

        hadoop distcp abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg

    Эта команда копирует содержимое папки **/myfolder** в учетной записи Data Lake Store в папку **/example/data/gutenberg/** в WASB.

## <a name="performance-considerations-while-using-distcp"></a>Рекомендации по производительности при использовании DistCp

Так как наименьшей степенью детализации DistCp является один файл, установка максимального количества одновременных копий является самым важным параметром оптимизации для хранилища Data Lake. Количество одновременных копий регулируется установкой параметра, отвечающего за количество модулей сопоставления (**m**), в командной строке. Этот параметр указывает максимальное количество модулей сопоставления, которые используются для копирования данных. Значение по умолчанию — 20.

**Пример**

    hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Как определить количество модулей сопоставления, которые следует использовать?

Ниже представлены некоторые полезные рекомендации.

* **Шаг 1. Определение общего объема памяти YARN.** Первым шагом является определение объема памяти YARN, доступной для кластера, в котором выполняется задание DistCp. Эта информация доступна на портале Ambari, связанном с кластером. Перейдите к YARN и откройте вкладку конфигураций, чтобы определить объем памяти YARN. Чтобы определить общий объем памяти YARN, умножьте объем памяти YARN для одного узла на количество узлов в кластере.

* **Шаг 2. Расчет количества модулей сопоставления.** Чтобы узнать значение **m**, разделите целую часть общего объема памяти YARN на размер контейнера YARN. Сведения о размере контейнера YARN также доступны на портале Ambari. Перейдите к YARN и откройте вкладку конфигураций. Размер контейнера YARN отобразится в этом окне. Уравнение для получения количества модулей сопоставления (**m**):

        m = (number of nodes * YARN memory for each node) / YARN container size

**Пример**

Предположим, что в кластере есть 4 узла D14v2s, и вы пытаетесь передать 10 ТБ данных из 10 разных папок. Эти папки содержат различные объемы данных, и размеры файлов в каждой папке отличаются.

* **Общий объем памяти YARN**. На портале Ambari вы узнали, что объем памяти YARN составляет 96 ГБ для узла D14. Таким образом, общий объем памяти YARN для кластера из четырех узлов равен: 

        YARN memory = 4 * 96GB = 384GB

* **Число модулей сопоставления**. На портале Ambari вы узнали, что размер контейнера YARN равен 3072 для узла кластера D14. Таким образом, число модулей сопоставления равно:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Если другие приложения используют память, то для DistCp можно использовать только часть памяти YARN кластера.

### <a name="copying-large-datasets"></a>Копирование больших наборов данных

Если размер перемещаемого набора данных большой (например, больше 1 ТБ), или в наборе много различных папок, рекомендуем использовать несколько заданий DistCp. Вряд ли это увеличит производительность, однако будет создано несколько заданий. В результате, если возникнет сбой одного из заданий, нужно будет перезапустить только это задание, а не все задание по перемещению.

### <a name="limitations"></a>Ограничения

* DistCp пытается создать модули сопоставления одинакового размера для оптимизации производительности. Увеличение количества модулей сопоставления не всегда приводит к увеличению производительности.

* DistCp ограничивается только одним модулем сопоставления для каждого файла. Поэтому количество модулей сопоставления не должно превышать количество файлов. Так как DistCp может назначать только один модуль сопоставления для одного файла, это ограничивает объем параллелизма, который можно использовать для копирования больших файлов.

* При наличии небольшого количества больших файлов следует разбить их на фрагменты по 256 МБ для повышения степени параллелизма. 
