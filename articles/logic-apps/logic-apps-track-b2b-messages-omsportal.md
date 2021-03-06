---
title: Отслеживание сообщений B2B с помощью Azure Log Analytics в Azure Logic Apps | Документация Майкрософт
description: Отслеживание взаимодействия B2B для учетных записей интеграции и Azure Logic Apps с помощью Azure Log Analytics
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 06/19/2018
ms.openlocfilehash: 5bf5385824eb9b711a2fee547c29d24d7ef5a01d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125774"
---
# <a name="track-b2b-communication-with-azure-log-analytics"></a>Отслеживание взаимодействия B2B с помощью Azure Log Analytics

После настройки взаимодействия B2B между двумя выполняющимися бизнес-процессами или приложениями с помощью учетной записи интеграции эти сущности могут обмениваться сообщениями друг с другом. Для проверки корректности обработки сообщений можно организовать отслеживание сообщений AS2, X12 и EDIFACT с помощью [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Например, для отслеживания сообщений можно использовать следующие функции:

* число и состояние сообщений;
* состояние подтверждений;
* сопоставление сообщений с подтверждениями;
* подробное описание ошибок при сбоях;
* возможности поиска.

## <a name="requirements"></a>Требования

* Приложение логики, настроенное на ведение журнала диагностики. Узнайте подробнее о [создании приложения логики](quickstart-create-first-logic-app-workflow.md) и [настройке ведения журнала для такого приложения логики](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Учетная запись интеграции, настроенная для мониторинга и ведения журнала. Узнайте подробнее о [создании учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) и [настройке мониторинга и ведения журнала для этой учетной записи](../logic-apps/logic-apps-monitor-b2b-message.md).

* Если это еще не сделано, [опубликуйте диагностические данные в службе Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Кроме этого, у вас должна быть рабочая область в Log Analytics. Для отслеживания взаимодействия B2B используйте ту же рабочую область Log Analytics. 
>  
> См. дополнительные сведения о [создании рабочей области Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="add-the-logic-apps-b2b-solution-to-log-analytics"></a>Добавление решения Logic Apps B2B в Log Analytics

Чтобы организовать отслеживание сообщений B2B для приложения логики, необходимо добавить решение **Logic Apps B2B** на портал OMS. Узнайте подробнее о [добавлении решений в Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

1. На [портале Azure](https://portal.azure.com) выберите **Все службы**. Введите в поле поиска "log analytics" и выберите **Log Analytics**, как показано ниже.

   ![Поиск Log Analytics](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. В разделе **Log Analytics** найдите и выберите рабочую область Log Analytics. 

   ![Выбор рабочей области Log Analytics](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. В разделе **Управление** выберите **Обзор**.

   ![Выбор портала Log Analytics](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Когда откроется домашняя страница, выберите **Добавить**, чтобы установить решение Logic Apps B2B.    
   ![Выбор коллекции решений](media/logic-apps-track-b2b-messages-omsportal/add-b2b-solution.png)

5. В разделе **Решения по управлению** найдите и создайте решение **Logic Apps B2B**.     
   ![Выбор Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/create-b2b-solution.png)

   На домашней странице появится плитка **Сообщения B2B приложений логики**. 
   На ней отображается количество обработанных сообщений B2B.

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-log-analytics"></a>Отслеживание состояния и подробностей сообщения в Log Analytics

1. После обработки сообщений B2B отображаются сведения об их состоянии и подробные данные о них. На странице обзора выберите плитку **сообщений Logic Apps B2B**.

   ![Обновленное количество сообщений](media/logic-apps-track-b2b-messages-omsportal/b2b-overview-tile.png)

   > [!NOTE]
   > По умолчанию на плитке **Logic Apps B2B Messages** (Сообщения B2B для Logic Apps) отображаются данные за один день. Чтобы указать другой интервал выборки данных, выберите элемент управления размером выборки данных в верхней части страницы.
   > 
   > ![Изменение области данных](media/logic-apps-track-b2b-messages-omsportal/server-filter.png)
   >

2. После появления панели мониторинга состояния сообщений можно просмотреть дополнительные сведения о сообщениях определенного типа, данные о которых отображаются за один день. Выберите плитку **AS2**, **X12** или **EDIFACT**.

   ![Просмотр состояния сообщения](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Отобразится список сообщений в соответствии с выбранной плиткой. 
   Для получения дополнительных сведений о свойствах для каждого типа сообщений см. указанные ниже описания свойств сообщения:

   * [Свойства сообщения AS2](#as2-message-properties)
   * [Свойства сообщения X12](#x12-message-properties)
   * [Свойства сообщения EDIFACT](#EDIFACT-message-properties)

   Ниже представлен пример списка сообщений AS2.

   ![Просмотр списка сообщений AS2](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Чтобы просмотреть или экспортировать входные и выходные данные для определенных сообщений, выберите требуемые сообщения и нажмите **Загрузить**. При появлении соответствующего запроса сохраните ZIP-файл на локальный компьютер, а затем извлеките его содержимое. 

   В извлеченной папке имеется отдельная папка для каждого выбранного сообщения. 
   Если вы настроили подтверждения, в папку сообщений входят файлы с подробными сведениями для подтверждения. 
   В каждой из папок сообщения находятся как минимум следующие файлы: 
   
   * Удобные для чтения файлы с входными полезными данными и выходными полезными данными
   * Закодированные файлы с входными и выходными данными

   Форматы имен папок и файлов для каждого типа сообщений:

   * [Форматы имен файлов и папок для сообщений AS2](#as2-folder-file-names)
   * [Форматы имен файлов и папок для сообщений X12](#x12-folder-file-names)
   * [Форматы имен файлов и папок для сообщений EDIFACT](#edifact-folder-file-names)

   ![Загрузка файлов сообщений](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Чтобы просмотреть все действия с одинаковым идентификатором выполнения, на странице **Поиск по журналам** выберите сообщения из списка.

   Можно сортировать эти действия по столбцу или выполнить поиск определенных результатов.

   ![Действия с одинаковым идентификатором выполнения](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Для поиска результатов с помощью готовых запросов выберите **Избранное**.

   * Узнайте подробнее о [создании запросов путем добавления фильтров](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Или узнайте о [способах поиска данных с помощью функции поиска по журналам в Log Analytics](../log-analytics/log-analytics-log-searches.md).

   * Чтобы изменить запрос в поле поиска, обновите запрос, указав столбцы и значения, которые необходимо использовать в качестве фильтров.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Описания свойств и форматы имен для сообщений AS2, X12 и EDIFACT

Для каждого типа сообщения ниже приведены описания свойств и форматы имен для загруженных файлов сообщений.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Описания свойств сообщения AS2

Ниже приведены описания свойств для каждого сообщения AS2.

| Свойство | ОПИСАНИЕ |
| --- | --- |
| Отправитель | Гостевой партнер, указанный в **параметрах получения**, или главный партнер, указанный в **параметрах отправки**, для соглашения AS2 |
| Получатель | Главный партнер, указанный в **параметрах получения**, или гостевой партнер, указанный в **параметрах отправки**, для соглашения AS2 |
| приложение логики; | Приложение логики, в котором настроены действия AS2 |
| Status | Состояние сообщения AS2 <br>Success: получено или отправлено корректное сообщение AS2, MDN не настроено. <br>Success: получено или отправлено корректное сообщение AS2, MDN настроено и получено либо отправлено. <br>Failed: получено некорректное сообщение AS2. MDN не настроено. <br>Pending: получено или отправлено корректное сообщение AS2. MDN настроено и ожидается. |
| Ack | Состояние сообщения MDN <br>Accepted: получено или отправлено положительное MDN. <br>Pending: ожидается получение или отправка MDN. <br>Rejected: получено или отправлено отрицательное MDN. <br>Not Required: в соглашении не настроено MDN. |
| Направление | Направление сообщения AS2 |
| Идентификатор корреляции | Идентификатор для корреляции всех триггеров и действий в приложении логики |
| Идентификатор сообщения | Идентификатор сообщения AS2, полученный из заголовков сообщения AS2 |
| Timestamp | Время обработки сообщения действием AS2 |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>Форматы имен AS2 для загруженных файлов сообщений

Ниже приведены форматы имен для загруженных файлов и папок сообщений AS2.

| Папка или файл | Формат имени |
| :------------- | :---------- |
| Папка сообщения | [отправитель]\_[получатель]\_AS2\_[ИД_корреляции]\_[ИД_сообщения]\_[метка времени] |
| Входные данные, выходные данные и файлы подтверждения (если настроено) | **Полезные входные данные**: [отправитель]\_[получатель]\_AS2\_[ИД_корреляции]\_input_payload.txt </p>**Полезные выходные данные**: [отправитель]\_[получатель]\_AS2\_[ИД_корреляции]\_output\_payload.txt </p></p>**Входные данные**: [отправитель]\_[получатель]\_AS2\_[ИД_корреляции]\_inputs.txt </p></p>**Выходные данные**: [отправитель]\_[получатель]\_AS2\_[ИД_корреляции]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Описания свойств сообщения X12

Ниже приведены описания свойств для каждого сообщения X12.

| Свойство | ОПИСАНИЕ |
| --- | --- |
| Отправитель | Гостевой партнер, указанный в **параметрах получения**, или главный партнер, указанный в **параметрах отправки**, для соглашения X12 |
| Получатель | Главный партнер, указанный в **параметрах получения**, или гостевой партнер, указанный в **параметрах отправки**, для соглашения X12 |
| приложение логики; | Приложение логики, в котором настроены действия X12 |
| Status | Состояние сообщения X12 <br>Success: получено или отправлено корректное сообщение X12. Функциональное подтверждение не настроено. <br>Success: получено или отправлено корректное сообщение X12. Функциональное подтверждение настроено и либо получено, либо отправлено. <br>Failed: получено или отправлено некорректное сообщение X12. <br>Pending: получено или отправлено корректное сообщение X12. Функциональное подтверждение настроено и ожидается его получение. |
| Ack | Состояние функционального подтверждения (997). <br>Accepted: получено или отправлено положительное функциональное подтверждение. <br>Rejected: получено или отправлено отрицательное функциональное подтверждение. <br>Pending: ожидается функциональное подтверждение, но оно еще не получено. <br>Pending: функциональное подтверждение создано, но его не удалось отправить в партнер. <br>Not Required: функциональное подтверждение не настроено. |
| Направление | Направление сообщения X12 |
| Идентификатор корреляции | Идентификатор для корреляции всех триггеров и действий в приложении логики |
| Тип сообщения | Тип сообщения EDI X12 |
| ICN | Контрольный номер обмена для сообщения X12 |
| TSCN | Контрольный номер набора транзакций для сообщения X12 |
| Timestamp | Время обработки сообщения действием X12 |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>Форматы имен X12 для загруженных файлов сообщений

Ниже приведены форматы имен для загруженных файлов и папок сообщений X12.

| Папка или файл | Формат имени |
| :------------- | :---------- |
| Папка сообщения | [отправитель]\_[получатель]\_X12\_[контрольный_номер_обмена]\_[глобальный_контрольный_номер]\_[контрольный_номер_набора_транзакций]\_[метка_времени] |
| Входные данные, выходные данные и файлы подтверждения (если настроено) | **Полезные входные данные**: [отправитель]\_[получатель]\_X12\_[контрольный_номер_обмена]\_input_payload.txt </p>**Полезные выходные данные**: [отправитель]\_[получатель]\_X12\_[контрольный_номер_обмена]\_output\_payload.txt </p></p>**Входные данные**: [отправитель]\_[получатель]\_X12\_[контрольный_номер_обмена]\_inputs.txt </p></p>**Выходные данные**: [отправитель]\_[получатель]\_X12\_[контрольный_номер_обмена]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Описания свойств сообщения EDIFACT

Ниже приведены описания свойств для каждого сообщения EDIFACT.

| Свойство | ОПИСАНИЕ |
| --- | --- |
| Отправитель | Гостевой партнер, указанный в **параметрах получения**, или главный партнер, указанный в **параметрах отправки**, для соглашения EDIFACT |
| Получатель | Главный партнер, указанный в **параметрах получения**, или гостевой партнер, указанный в **параметрах отправки**, для соглашения EDIFACT |
| приложение логики; | Приложение логики, в котором настроены действия EDIFACT |
| Status | Состояние сообщения EDIFACT <br>Success: получено или отправлено корректное сообщение EDIFACT. Функциональное подтверждение не настроено. <br>Success: получено или отправлено корректное сообщение EDIFACT. Функциональное подтверждение настроено и либо получено, либо отправлено. <br>Failed: получено или отправлено некорректное сообщение EDIFACT <br>Pending: получено или отправлено корректное сообщение EDIFACT. Функциональное подтверждение настроено и ожидается его получение. |
| Ack | Состояние функционального подтверждения (997). <br>Accepted: получено или отправлено положительное функциональное подтверждение. <br>Rejected: получено или отправлено отрицательное функциональное подтверждение. <br>Pending: ожидается функциональное подтверждение, но оно еще не получено. <br>Pending: функциональное подтверждение создано, но его не удалось отправить в партнер. <br>Not Required: функциональное подтверждение не настроено. |
| Направление | Направление сообщения EDIFACT |
| Идентификатор корреляции | Идентификатор для корреляции всех триггеров и действий в приложении логики |
| Тип сообщения | Тип сообщения EDIFACT |
| ICN | Контрольный номер обмена сообщениями EDIFACT |
| TSCN | Контрольный номер набора транзакций для сообщения EDIFACT |
| Timestamp | Время обработки сообщения действием EDIFACT |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>Форматы имен EDIFACT для загруженных файлов сообщений

Ниже приведены форматы имен для загруженных файлов и папок сообщений EDIFACT.

| Папка или файл | Формат имени |
| :------------- | :---------- |
| Папка сообщения | [отправитель]\_[получатель]\_EDIFACT\_[контрольный_номер_обмена]\_[глобальный_контрольный_номер]\_[контрольный_номер_набора_транзакций]\_[метка_времени] |
| Входные данные, выходные данные и файлы подтверждения (если настроено) | **Полезные входные данные**: [отправитель]\_[получатель]\_EDIFACT\_[контрольный_номер_обмена]\_input_payload.txt </p>**Полезные выходные данные**: [отправитель]\_[получатель]\_EDIFACT\_[контрольный_номер_обмена]\_output\_payload.txt </p></p>**Входные данные**: [отправитель]\_[получатель]\_EDIFACT\_[контрольный_номер_обмена]\_inputs.txt </p></p>**Выходные данные**: [отправитель]\_[получатель]\_EDIFACT\_[контрольный_номер_обмена]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Дополнительная информация

* [Запрос сообщений AS2, X 12 и EDIFACT в Microsoft Operations Management Suite (OMS)](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Схемы отслеживания AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Схемы отслеживания X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Настраиваемые схемы отслеживания](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)