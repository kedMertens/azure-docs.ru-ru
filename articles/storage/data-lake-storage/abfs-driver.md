---
title: Драйвер файловой системы больших двоичных объектов Azure для хранилища Azure Data Lake Storage Gen2 (предварительная версия)
description: Драйвер файловой системы Hadoop ABFS
services: storage
keywords: ''
author: jamesbak
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: dedf398064dd0a49e5691e952ea7c9b6d16e34fd
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "42144526"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Драйвер файловой системы больших двоичных объектов Azure (ABFS): выделенный драйвер хранилища Azure для Hadoop

Один из основных способов доступа к данным в хранилище Azure Data Lake Storage Gen2 (предварительная версия) связан с использованием [файловой системы Hadoop](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Хранилище Azure Data Lake Storage Gen2 имеет связанный драйвер — драйвер файловой системы больших двоичных объектов Azure или `ABFS`. ABFS является частью Apache Hadoop и входит во многие коммерческие дистрибутивы Hadoop. При использовании этого драйвера многие приложения и платформы могут получить доступ к данным в хранилище Data Lake Gen2 без какого-либо кода, явно ссылающегося на службу хранилища Data Lake Gen2.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Предыдущая возможность: драйвер Windows Azure Storage Blob

Драйвер Windows Azure Storage Blob или [WASB](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) предоставлял оригинальную поддержку больших двоичных объектов хранилища Azure. Этот драйвер выполнял сложную задачу сопоставления семантики файловой системы (как это требуется интерфейсом файловой системы Hadoop) с семантикой интерфейса стиля хранилища объектов, предоставляемого хранилищем BLOB-объектов Azure. Этот драйвер продолжает поддерживать эту модель, обеспечивая высокопроизводительный доступ к данным, хранящимся в больших двоичных объектах. Также он содержит значительный объем кода, который выполняет это сопоставление, что и затрудняет его техническое обслуживание. Кроме того, при некоторых применяемых к каталогам операциях, таких как [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) и [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive), требуется, чтобы драйвер выполнил большое количество операций (из-за того, что объекты хранилища не поддерживают каталоги), что часто приводит к снижению производительности. Для преодоления присущих WASB недостатков разработана новая служба Azure Data Lake Storage.

## <a name="the-azure-blob-file-system-driver"></a>Драйвер файловой системы больших двоичных объектов Azure

[Интерфейс REST хранилища Azure Data Lake Storage](https://docs.microsoft.com/en-us/rest/api/storageservices/data-lake-storage-gen2) поддерживает семантику файловой системы с помощью хранилища BLOB-объектов Azure. Учитывая, что файловая система Hadoop предназначена для поддержки той же семантики, сложное сопоставление в драйвере не требуется. Таким образом, драйвер файловой системы больших двоичных объектов Azure (или ABFS) является простой оболочкой клиента для интерфейса REST API.

Однако существуют некоторые функции, которые драйвер все же должен выполнять:

### <a name="uri-scheme-to-reference-data"></a>Схема URI для ссылки на данные

Как и другие реализации файловой системы в Hadoop, драйвер ABFS определяет свою собственную схему URI, чтобы к ресурсам (каталоги и файлы) можно было прямо обращаться. Схему URI см. в статье [Использование URI в хранилище Azure Data Lake Storage Gen2](./introduction-abfs-uri.md). URI имеет такую структуру: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Применяя вышеуказанный формат URI, стандартные инструменты и платформы Hadoop могут использоваться для ссылки на эти ресурсы:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

На внутреннем уровне драйвер ABFS преобразует ресурсы, указанные в URI, в файлы и каталоги и вызывает REST API хранилища Azure Data Lake Storage с использованием этих ссылок.

### <a name="authentication"></a>Authentication

Драйвер ABFS поддерживает две формы проверки подлинности, поэтому приложение Hadoop может безопасно обращаться к ресурсам, содержащимся в учетной записи, совместимой с Data Lake Storage 2-го поколения. Дополнительные сведения о схемах проверки подлинности приведены в [Руководстве по безопасности службы хранилища Azure](../common/storage-security-guide.md). К ним относятся:

- **Общий ключ** позволяет пользователям получать доступ ко всем ресурсам в учетной записи. Ключ шифруется и сохраняется в конфигурации Hadoop.

- **Токен носителя OAuth Azure Active Directory**. Токен носителя Azure AD приобретается и обновляется с помощью драйвера, используя либо идентификатор пользователя, либо настроенный субъект-службу. С помощью этой модели проверки подлинности доступ разрешен для каждого вызова, использующего идентификатор, который связан с предоставленным токеном и оценивается с помощью назначенного списка управления доступом POSIX (ACL).

### <a name="configuration"></a>Параметр Configuration

Все настройки драйвера ABFS хранятся в файле конфигурации <code>core-site.xml</code>. В дистрибутивах Hadoop с [Ambarі](http://ambari.apache.org/) конфигурацией также можно управлять с помощью веб-портала или REST API Ambari.

Подробная информация обо всех поддерживаемых элементах конфигурации указана в [официальной документации по Hadoop](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Документация Hadoop

Все сведения о драйвере ABFS см. в [официальной документации по Hadoop](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

## <a name="next-steps"></a>Дополнительная информация

- [Краткое руководство. Настройка кластеров в HDInsight](./quickstart-create-connect-hdi-cluster.md)
- [Краткое руководство. Запуск задания Spark в Azure Databricks с помощью портала Azure](./quickstart-create-databricks-account.md)
- [Использование URI в хранилище Azure Data Lake Storage Gen2](./introduction-abfs-uri.md)
