---
title: Планирование загрузки кластера в Azure HDInsight
description: Описывается, как настроить кластер HDInsight для обеспечения необходимой емкости и производительности.
services: hdinsight
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/22/2017
ms.author: maxluk
ms.openlocfilehash: 4438cff0dcf5e896f39729d9871d4deb3207b4b8
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "43108001"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Планирование загрузки кластеров HDInsight

Перед развертыванием кластера HDInsight следует спланировать его предполагаемую загрузку, определив необходимые производительность и масштаб. Это помогает оптимизировать удобство использования и затраты. Некоторые решения относительно емкости кластера нельзя изменить после развертывания. В случае изменения параметров производительности кластер можно отключить, а затем повторно создать без потери хранимых в нем данных.

Ниже приведены основные вопросы о планировании загрузки.

* В географическом регионе следует развернуть кластер?
* Какой объем хранилища требуется?
* Какой тип кластера следует выбрать для развертывания?
* Какие размер и тип виртуальной машины следует использовать на узлах кластера?
* Сколько рабочих узлов должен включать в себя кластер?

## <a name="choose-an-azure-region"></a>Выбор региона Azure

Регион Azure определяет место физической подготовки кластера. Чтобы свести к минимуму задержки при чтении и записи, кластер должен быть расположен близко к вашим данным.

Служба HDInsight доступна во многих регионах Azure. Чтобы найти ближайший регион, ознакомьтесь с пунктом *HDInsight под управлением Linux* в разделе *Данные и аналитика* на странице [Доступность продуктов по регионам](https://azure.microsoft.com/regions/services/).

## <a name="choose-storage-location-and-size"></a>Выбор расположения и размера хранилища

### <a name="location-of-default-storage"></a>Расположение хранилища по умолчанию

Хранилище по умолчанию (учетная запись хранения Azure или Azure Data Lake Store) должно находиться том же расположении, что и кластер. Служба хранилища Azure доступна во всех расположениях. Хранилище Data Lake Store доступно в некоторых регионах. Ознакомьтесь с актуальными сведениями о доступности Data Lake Store в разделе *Хранилище* на странице [Доступность продуктов по регионам](https://azure.microsoft.com/regions/services/).

### <a name="location-of-existing-data"></a>Расположение существующих данных

Если у вас уже есть учетная запись хранения или хранилище Data Lake Store с вашими данными и вы хотите использовать это хранилище в качестве хранилища по умолчанию для кластера, то кластер необходимо развернуть в том же расположении.

### <a name="storage-size"></a>Размер хранилища

После развертывания кластера HDInsight можно подключить дополнительные учетные записи хранения Azure или обращаться к другим хранилищам Data Lake Store. Все учетные записи хранения должны находиться в том же расположении, что и кластер. Data Lake Store может находиться в другом расположении, хотя это может привести к задержке при чтении и записи некоторых данных.

Служба хранилища Azure накладывает некоторые [ограничения емкости](../azure-subscription-service-limits.md#storage-limits), тогда как емкость Data Lake Store практически не ограничена.

Кластер может обращаться к разным учетным записям хранения. Некоторые распространенные примеры:

* когда объем данных, вероятнее всего, превышает емкость одного контейнера больших двоичных объектов;
* когда скорость доступа к контейнеру больших двоичных объектов может превышать пороговое значение, что активирует регулирование;
* когда необходимо, чтобы данные, уже переданные в контейнер больших двоичных объектов, стали доступными для кластера;
* когда требуется изолировать различные части хранилища из соображений безопасности или для упрощения администрирования.

Для кластера с 48 узлами рекомендуется использовать 4–8 учетных записей хранения. Хотя общий объем хранилища уже может быть вполне достаточным, каждая учетная запись хранения обеспечивает дополнительную пропускную способность сети для вычислительных узлов. При наличии нескольких учетных записей хранения используйте случайное имя без префикса для каждой из них. Назначение случайного именования — уменьшить вероятность возникновения узких мест в хранилище (и регулирования) и типичных сбоев во всех учетных записях. Для повышения производительности используйте только один контейнер на учетную запись хранения.

## <a name="choose-a-cluster-type"></a>Выбор типа кластера

Тип кластера определяет рабочую нагрузку, для выполнения которой настроен кластер HDInsight. Примеры: Hadoop, Storm, Kafka, Spark. Подробное описание доступных типов кластеров см. в статье [Общие сведения об Azure HDInsight и стеке технологий Hadoop и Spark](hadoop/apache-hadoop-introduction.md#cluster-types-in-hdinsight). Каждый тип кластера имеет определенную топологию развертывания, которая включает в себя требования к размеру и количеству узлов.

## <a name="choose-the-vm-size-and-type"></a>Выбор типа и размера виртуальной машины

У каждого типа кластера имеется набор типов узлов, и у каждого типа узла имеются определенные параметры, определяющие размер и тип виртуальной машины.

Чтобы определить оптимальный размер кластера для приложения, можно измерить производительность кластера и увеличить его размер в соответствии с полученными рекомендациями. Например, можно использовать имитацию рабочей нагрузки или *запрос предохранителя*. Имитация рабочей нагрузки позволяет выполнять ожидаемую рабочую нагрузки на кластерах разного размера, постепенного увеличивая их размер, пока не будет достигнута требуемая производительность. Запрос предохранителя можно периодически выполнять наряду с прочими рабочими запросами, чтобы определять, достаточно ли в кластере ресурсов.

Размер и тип виртуальной машины определяется вычислительной мощностью ЦП, объемом оперативной памяти и задержкой сети.

* ЦП: размер виртуальной машины определяет число ядер. Чем больше ядер, тем более высокой степени распараллеливания вычислений может достичь каждый узел. Кроме того, в некоторых типах виртуальных машин используются более быстрые ядра.

* ОЗУ: размер виртуальной машины также определяет ее объем ОЗУ. В случае рабочих нагрузок, требующих хранения данных в памяти для обработки, а не их чтения с диска, следует убедиться, что на рабочих узлах достаточно памяти для размещения данных.

* Сеть: для большинства типов кластера данные, обрабатываемые кластером, находятся не на локальном диске, а во внешней службе хранилища, например в хранилище Data Lake Store или службе хранилища Azure. Оцените пропускную способность сети и пропускную способность между виртуальной машиной узла и службой хранилища. Как правило, пропускная способность сети виртуальной машины большего размера также выше. Дополнительные сведения см. в разделе [Размеры виртуальных машин Linux в Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

## <a name="choose-the-cluster-scale"></a>Выбор масштаба кластера

Масштаб кластера определяется количеством его узлов виртуальных машин. Для всех типов кластеров есть типы узлов со специальным масштабом и типы узлов, которые поддерживают развертывание (увеличение масштаба). Например,в кластере может потребоваться в точности три узла ZooKeeper или два головного узла. Развертывание рабочих узлов, которые выполняют распределенную обработку данных, может повысить производительность благодаря добавлению дополнительных рабочих узлов.

В зависимости от того, какой тип кластера используется, при увеличении числа рабочих узлов добавляются дополнительные вычислительные ресурсы (например, дополнительные ядра), но также может увеличиваться общий объем памяти, необходимой всему кластеру для поддержки хранения обрабатываемых данных в памяти. Как и в случае выбора размера и типа виртуальной машины, выбор правильного масштаба кластера обычно осуществляется эмпирически, с помощью имитации рабочей нагрузки или запросов предохранителя.

Можно развернуть кластер, чтобы справиться с пиковыми нагрузками, а затем уменьшить его масштаб, когда эти дополнительные узлы уже будут не нужны. Дополнительные сведения см. в статье [Масштабирование кластеров HDInsight](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Жизненный цикл кластера

Плата взимается на протяжении времени существования кластера. Если работающий кластер требуется вам только в определенные моменты времени, можно [создавать кластеры по требованию с помощью фабрики данных Azure](hdinsight-hadoop-create-linux-clusters-adf.md). Можно также создать сценарии PowerShell, которые подготавливают и удаляют кластер, и запланировать выполнение этих сценариев с помощью [службы автоматизации Azure](https://azure.microsoft.com/services/automation/).

> [!NOTE]
> При удалении кластера также удаляется его метахранилище Hive по умолчанию. Чтобы сохранить метахранилище для следующего повторного создания кластера, используйте внешнее хранилище метаданных, такое как база данных Azure или Oozie.
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Изоляция ошибок заданий кластера

Иногда ошибки могут происходить из-за параллельного выполнения нескольких компонентов map и reduce в кластере с несколькими узлами. Чтобы изолировать проблему, попробуйте выполнить распределенное тестирование, одновременно выполнив несколько заданий на кластере с одним узлом, затем расширьте этот же прием, одновременно выполнив несколько заданий на кластерах, содержащих более одного узла. Чтобы создать кластер HDInsight с одним узлом в Azure, используйте параметр *Дополнительно*.

Можно также установить среду разработки в кластере с одним узлом на локальном компьютере и тестировать решение на нем. Hortonworks предоставляет локальную среду разработки в кластере с одним узлом, предназначенную для решений на основе Hadoop. Эта среда удобна для начальной разработки, подтверждения концепции и тестирования. Дополнительные сведения доступны на сайте [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox/).

Чтобы определить проблему на локальном кластере с одним узлом, можно повторно выполнить неудачно завершившиеся задания и адаптировать входные данные или использовать меньшие наборы данных. Способ выполнения этих заданий зависит от платформы и типа приложения.

## <a name="quotas"></a>Квоты

После определения размера виртуальных машин, масштаба и типа целевого кластера проверьте текущие квоты емкости своей подписки. После достижения квоты вы не сможете развертывать новые кластеры или масштабировать существующие кластеры, добавляя дополнительные рабочие узлы. Чаще всего достигается квота ядер ЦП, устанавливаемая на уровне подписки, региона и серии виртуальных машин. Например, у подписки может быть общий лимит в 200 ядер, в вашем регионе — лимит в 30 ядер и лимит в 30 ядер на экземплярах виртуальных машин. Вы можете [обратиться в службу поддержки, чтобы запросить увеличение квоты](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

Однако существуют некоторые фиксированные квоты, например, одна подписка Azure может содержать не более 10 000 ядер. Сведения об этих ограничениях см. в статье [Подписка Azure, границы, квоты и ограничения службы](https://docs.microsoft.com/azure/azure-subscription-service-limits#limits-and-the-azure-resource-manager).

## <a name="next-steps"></a>Дополнительная информация

* [Установка кластеров в HDInsight с использованием Hadoop, Spark, Kafka и других технологий](hdinsight-hadoop-provision-linux-clusters.md): узнайте, как установить и настроить кластеры в HDInsight с использованием Hadoop, Spark, Kafka, Interactive Hive, HBase, службы машинного обучения или Storm.
* [Мониторинг производительности кластера](hdinsight-key-scenarios-to-monitor.md): изучите основные сценарии, которые могут влиять на емкость кластера HDInsight и требуют его мониторинга.
