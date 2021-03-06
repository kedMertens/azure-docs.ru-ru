---
title: Приступая к работе с предварительно настроенными решениями | Документация Майкрософт
description: Следуйте инструкциям этого учебника, чтобы научиться развертывать предварительно настроенные решения набора IoT Microsoft Azure
services: iot-suite
ms.service: iot-suite
author: dominicbetts
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 0c499d5f4d1d6256294e25921cef1fb0dfed0c05
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2018
ms.locfileid: "43185612"
---
# <a name="get-started-with-the-preconfigured-solutions"></a>Приступая к работе с предварительно настроенными решениями

Набор [предварительно настроенных решений][lnk-preconfigured-solutions] Интернета вещей Azure включает несколько служб Интернета вещей Azure, используемых при создании комплексных решений для реализации распространенных бизнес-сценариев Интернета вещей. Предварительно настроенное решение для *удаленного мониторинга* подключается к устройствам и отслеживает их работу. Это решение можно использовать, чтобы анализировать поток данных, поступающих с устройств, и улучшать результаты работы компании, организовав автоматическое реагирование процессов на этот поток данных.

В этом руководстве показано, как подготовить предварительно настроенное решение для удаленного мониторинга. Кроме того, здесь описываются основные функции этого решения. Многие функции доступны на *панели мониторинга* решения, которая развертывается вместе с предварительно настроенным решением.

![Панель мониторинга предварительно настроенного решения для удаленного мониторинга][img-dashboard]

Для работы с этим руководством требуется активная подписка Azure.

> [!NOTE]
> Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в статье о [бесплатной пробной версии Azure][lnk_free_trial].

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a>Обзор сценария

Развертывание предварительно настроенного решения удаленного мониторинга уже содержит ресурсы, которые позволяют выполнять распространенный сценарий удаленного мониторинга. В этом сценарии несколько устройств, подключенных к решению, передают непредвиденные значения температуры. В следующих разделах описано, как:

* определить устройства, которые отправляют непредвиденные значения температуры;
* настроить эти устройства для отправки подробных данных телеметрии;
* устранять проблемы путем обновления встроенного ПО на этих устройствах;
* проверять устранение проблемы.

Ключевой особенностью этого сценария является возможность удаленно выполнять все эти действия с помощью панели мониторинга решения. Физический доступ к устройствам не обязателен.

## <a name="view-the-solution-dashboard"></a>Просмотр панели мониторинга решения

Панель мониторинга решения позволяет управлять развернутым решением. Например, можно просматривать данные телеметрии, добавлять устройства и настраивать правила.

1. Когда подготовка завершится и на плитке предварительно настроенного решения появится надпись **Готово**, нажмите кнопку **Запустить**. Панель мониторинга решения откроется в новой вкладке.

    ![Запуск предварительно настроенного решения][img-launch-solution]

1. По умолчанию на портале решения отображается *панель мониторинга*. Вы можете перейти к другим областям портала решения с помощью меню в левой части страницы.

    ![Панель мониторинга предварительно настроенного решения для удаленного мониторинга][img-menu]

На панели мониторинга отображаются следующие сведения:

* Карта, на которой показано расположение каждого устройства, подключенного к решению. При первом запуске решения отобразятся 25 виртуальных устройств, реализованных в виде веб-заданий Azure. Для отображения сведений на карте в решении используется интерфейс API для Карт Bing. В разделе [Часто задаваемые вопросы][lnk-faq] содержатся сведения о том, как сделать карту динамической.
* На панели **Журнал телеметрии** отображаются графики влажности и температуры с выбранного устройства в близком к реальному времени, а также сводные данные, например максимальное, минимальное и среднее значения влажности.
* На панели **Журнал оповещений** отображаются последние события, когда значение телеметрии превышает пороговое значение. В дополнение к оповещениям, созданным в предварительно настроенном решении, можно задать собственные оповещения.
* В области **Задания** отображаются сведения о запланированных заданиях. Вы можете запланировать собственные задания на странице **заданий управления**.

## <a name="view-alarms"></a>Просмотр оповещений

На панели журнала оповещений показано, что пять устройств передают значения телеметрии, которые превышают ожидаемые.

![TODO Журнал оповещений на панели мониторинга решения][img-alarms]

> [!NOTE]
> Эти оповещения создаются правилом, которое содержится в предварительно настроенном решении. Это правило создает оповещение, когда значение температуры, передаваемое устройством, превышает 60. Вы можете определить собственные правила и действия, выбрав элементы [Правила](#add-a-rule) и [Действия](#add-an-action) в меню слева.

## <a name="view-devices"></a>Просмотр устройств

В *списке устройств* показаны все зарегистрированные устройства в решении. Там вы можете просматривать и изменять метаданные устройств, добавлять или удалять их, а также вызывать методы на устройствах. Этот список можно фильтровать и сортировать. Можно также настроить столбцы, которые отображаются в списке устройств.

1. Выберите **Устройства** для отображения списка устройств этого решения.

   ![Просмотр списка устройств на портале решения][img-devicelist]

1. Изначально список устройств отображает 25 виртуальных устройств, созданных в ходе подготовки. Вы можете добавить в решение дополнительные виртуальные и физические устройства.

1. Чтобы просмотреть сведения об устройстве, щелкните его в списке устройств.

   ![Просмотр дополнительных сведений об устройствах на портале решения][img-devicedetails]

Область **Сведения об устройстве** содержит шесть разделов.

* Коллекция ссылок, позволяющих настроить значок устройства, отключить устройство, добавить правило, вызвать метод или отправить команду. Сведения о сравнении команд (сообщения из устройства в облако) и методов (прямые методы) см. в [руководстве о связи между облаком и устройством][lnk-c2d-guidance].
* Раздел **Device Twin - Tags** (Двойник устройства — Теги) позволяет изменять значения тегов для устройства. Вы можете отобразить значения тегов в списке устройств и использовать их для фильтрации этого списка.
* Раздел **Device Twin - Desired Properties** (Двойник устройства — Требуемые свойства) позволяет задавать значения свойств, отправляемые на устройство.
* В разделе **Device Twin - Reported Properties** (Двойник устройства — Переданные свойства) отображаются значения свойств, отправляемые из устройства.
* В разделе **Свойства устройства** отображаются сведения из реестра удостоверений, например идентификатор устройства и ключи проверки подлинности.
* В разделе **Последние задания** отображаются сведения обо всех заданиях, недавно назначенных этому устройству.

## <a name="filter-the-device-list"></a>Фильтрация списка устройств

С помощью фильтра можно отобразить только те устройства, которые передают непредвиденные значения температуры. Предварительно настроенное решение удаленного мониторинга содержит фильтр **Неработоспособные устройства**, отображающий устройства, передающие среднее значение температуры выше 60. Также можно [создать собственные фильтры](#add-a-filter).

1. Выберите **Открыть сохраненный фильтр**, чтобы отобразить список доступных фильтров. Выберите **Неработоспособные устройства**, чтобы применить фильтр:

    ![Отображение списка фильтров][img-unhealthy-filter]

1. В списке устройств теперь отображаются только те устройства, которые передают среднее значение температуры выше 60.

    ![Просмотр отфильтрованного списка устройств, в котором отображаются неработоспособные устройства][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a>Обновление требуемых свойств

Вы определили набор устройств, которые необходимо исправить. Предположим, что 15-секундного интервала обновления данных недостаточно для точной диагностики проблемы. Увеличение частоты телеметрии до пяти секунд для получения дополнительных данных поможет лучше определить проблему. Вы можете применить это изменение конфигурации к удаленным устройствам с помощью портала решения. Можно применить изменение, оценить его влияние, а затем действовать в соответствии с полученными результатами.

Выполните следующие действия, чтобы запустить задание, которое изменяет требуемое свойство **TelemetryInterval** для затрагиваемых устройств. При получении нового значения свойства **TelemetryInterval** устройства изменят свою конфигурацию и будут передавать данные телеметрии каждые пять секунд вместо 15.

1. Когда в списке устройств отображаются неработоспособные устройства, выберите **Планировщик заданий**, а затем **Изменить двойник устройства**.

1. Вызовите задание **изменения интервала телеметрии**.

1. Задайте для значения **требуемого свойства** **desired.Config.TelemetryInterva** пять секунд.

1. Выберите **Расписание**.

    ![Изменение значения свойства TelemetryInterval на пять секунд][img-change-interval]

1. Вы можете отслеживать ход выполнения заданий на странице **Задания управления** на портале.

> [!NOTE]
> Если вы хотите изменить значение требуемого свойства для отдельных устройств, вместо выполнения задания используйте раздел **Требуемые свойства** на панели **Сведения об устройстве**.

Это задание задает значение требуемого свойства **TelemetryInterval** на двойнике устройства для всех устройств, выбранных с помощью фильтра. Устройства получают это значение от двойника устройства и обновляют свое поведение. Когда устройство получает от двойника устройства требуемое свойство и обрабатывает его, оно задает соответствующее значение сообщаемого свойства.

## <a name="invoke-methods"></a>Вызов методов

Во время выполнения задания в списке неработоспособных устройств вы заметите, что эти устройства работают под управлением старой версии встроенного ПО (ниже версии 1.6).

![Просмотр версии сообщаемого встроенного ПО неработоспособных устройств][img-old-firmware]

Устаревшая версия встроенного ПО может стать основной причиной получения непредвиденных значений температуры, так как известно, что ПО других работоспособных устройств было недавно обновлено до версии 2.0. Можно воспользоваться встроенным фильтром **Old firmware devices** (Устройства с устаревшим встроенным ПО), чтобы определить все устройства с устаревшими версиями встроенного ПО. На портале можно удаленно обновить все устройства, которые продолжают работать под управлением старых версий встроенного ПО.

1. Выберите **Открыть сохраненный фильтр**, чтобы отобразить список доступных фильтров. Выберите **Old firmware devices** (Устройства с устаревшим встроенным ПО) для применения фильтра:

    ![Отображение списка фильтров][img-old-filter]

1. Теперь в списке устройств отображаются только устройства со старыми версиями встроенного ПО. Этот список содержит пять устройств, определенных с помощью фильтра **Неработоспособные устройства**, и три дополнительных устройства.

    ![Просмотр отфильтрованного списка устройств, в котором отображаются устройства с устаревшими версиями встроенного ПО][img-filtered-old-list]

1. Выберите **Планировщик заданий**, а затем — **Вызвать метод**.

1. В качестве **имени задания** введите **Обновление встроенного ПО до версии 2.0**.

1. Выберите **InitiateFirmwareUpdate** в качестве **метода**.

1. Задайте для параметра **FwPackageUri** значение **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.

1. Выберите **Расписание**. По умолчанию для задания используется значение "Запустить сейчас".

    ![Создание задания для обновления встроенного ПО выбранных устройств][img-method-update]

> [!NOTE]
> Если вы хотите вызвать метод для отдельного устройства, вместо выполнения задания на панели **Сведения об устройстве** выберите **Методы**.

Это задание вызывает прямой метод **InitiateFirmwareUpdate** на всех устройствах, выбранных с помощью фильтра. Устройства немедленно отвечают Центру Интернета вещей и асинхронно инициируют процесс обновления встроенного ПО. Устройства предоставляют сведения о ходе процесса обновления встроенного ПО с помощью значений сообщаемых свойств, как показано на следующих снимках экрана. Щелкните значок **Обновить**, чтобы обновить данные в списках устройств и заданий.

![Список заданий, отображающий выполнение обновления встроенного ПО][img-update-1]
![Список устройств, отображающий состояние обновления встроенного ПО][img-update-2]
![Список заданий, отображающий завершение обновления встроенного ПО][img-update-3]

> [!NOTE]
> В рабочей среде можно запланировать выполнение заданий во время периода планового обслуживания.

## <a name="scenario-review"></a>Разбор сценария

В этом сценарии вы определили потенциальную проблему с некоторыми удаленными устройствами, используя фильтр и журнал оповещений на панели мониторинга. Затем с помощью фильтра и задания вы удаленно выполнили настройку устройств, чтобы получить дополнительные сведения для диагностики проблемы. Наконец, вы использовали фильтр и задание, чтобы запланировать обслуживание затрагиваемых устройств. На панели мониторинга можно проверить, что от устройств в решении оповещения больше не поступают. Вы можете использовать фильтр, чтобы проверить актуальность встроенного ПО на всех устройствах в решении, а также чтобы убедиться в отсутствии неработоспособных устройств.

![Фильтр, показывающий, что все устройства имеют актуальную версию встроенного ПО][img-updated]

![Фильтр, показывающий, что все устройства работоспособны][img-healthy]

## <a name="other-features"></a>Другие возможности

В следующих разделах приведены некоторые дополнительные возможности предварительно настроенного решения удаленного мониторинга, которые не описаны в предыдущем сценарии.

### <a name="customize-columns"></a>Настройка столбцов

Вы можете настроить сведения, отображающиеся в списке устройств, щелкнув **Редактор столбцов**. Кроме того, можно добавлять и удалять столбцы, отображающие полученные значения тегов и свойств, а также изменять порядок столбцов и переименовывать их.

   ![Редактор столбцов в списке устройств][img-columneditor]

### <a name="customize-the-device-icon"></a>Настройка значка устройства

Вы можете настроить значок устройства, отображающийся в списке устройств, в области **Сведения об устройстве**.

1. Щелкните значок карандаша, чтобы открыть для устройства область **Изменить изображение**.

   ![Открытие редактора изображений устройств][img-startimageedit]

1. Отправьте новое изображение или используйте одно из существующих и нажмите кнопку **Сохранить**.

   ![Редактор изменения изображений устройства][img-imageedit]

1. Теперь в столбце **Значок** для устройства отображается изображение, которое вы выбрали.

> [!NOTE]
> Изображение хранится в хранилище BLOB-объектов. Тег в двойнике устройства содержит ссылку на изображение в хранилище BLOB-объектов.

### <a name="add-a-device"></a>Добавление устройства

При развертывании предварительно настроенного решения автоматически подготавливаются 25 примеров устройств, которые вы можете увидеть в списке устройств. Это *виртуальные устройства*, работающие в веб-задании Azure. Виртуальные устройства позволяют легко экспериментировать с предварительно настроенным решением без развертывания физических устройств. Если вы хотите подключить к решению физическое устройство, см. руководство [Подключение устройства к предварительно настроенному решению для удаленного мониторинга (Windows)][lnk-connect-rm].

Далее объясняется, как добавить виртуальное устройство к решению.

1. Вернитесь к списку устройств.

1. В левом нижнем углу щелкните **+ Add a Device** (+ Добавить устройство), чтобы добавить устройство.

   ![Добавление устройства в предварительно настроенное решение][img-adddevice]

1. На плитке **Имитированное устройство** щелкните **Добавить**.

   ![Настройка сведений о новом устройстве на панели мониторинга][img-addnew]

   Помимо создания виртуального устройства, можно также добавить физическое устройство на панели **Custom Device** (Пользовательское устройство). Дополнительные сведения о подключении к решению физических устройств см. в статье [Подключение устройства к предварительно настроенному решению для удаленного мониторинга (Windows)][lnk-connect-rm].

1. Выберите пункт **Let me define my own Device ID** (Задать идентификатор устройства самостоятельно) и укажите имя уникального идентификатора устройства: **mydevice_01**.

1. Выберите **Создать**.

   ![Сохранение нового устройства][img-definedevice]

1. На шаге 3 действия **Add a simulated device** (Добавить имитированное устройство) щелкните **Готово**, чтобы вернуться к списку устройств.

1. В списке устройств состояние устройства изменится на **Работает** .

    ![Просмотр нового устройства в списке устройств][img-runningnew]

1. На панели мониторинга можно просмотреть данные телеметрии нового виртуального устройства:

    ![Просмотр данных телеметрии с нового устройства][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a>Отключение и удаление устройства

Можно отключить устройство, а после отключения его можно удалить.

![Отключение и удаление устройства][img-disable]

### <a name="add-a-rule"></a>Добавление правила

Только что созданное устройство пока не регулируется никакими правилами. В этом разделе описано, как добавить правило, которое активирует оповещение, если значение температуры, сообщаемое новым устройством, превышает 47 градусов. Прежде чем добавить правило, зайдите в раздел истории телеметрии на панели мониторинга и убедитесь, что температура нового устройства никогда не превышает 45 градусов.

1. Вернитесь к списку устройств.

1. Чтобы добавить для устройства правило, выберите новое устройство в **списке устройств**, а затем щелкните **Добавить правило**.

1. В окне создания правила в поле данных выберите значение **Температура** и укажите **AlarmTemp** в качестве выходных данных правила при превышении 47 градусов:

    ![Добавление правила устройства][img-adddevicerule]

1. Чтобы сохранить изменения, нажмите кнопку **Сохранить и просмотреть правила**.

1. В области сведений о новом устройстве щелкните **Команды**.

    ![Добавление правила устройства][img-adddevicerule2]

1. В списке команд выберите **ChangeSetPointTemp** и укажите в поле **SetPointTemp** значение 45. Выберите **Отправить команду**.

    ![Добавление правила устройства][img-adddevicerule3]

1. Вернитесь на панель мониторинга. Вскоре, когда температура, сообщаемая новым устройством, превысит пороговое значение в 47 градусов, на панели **Журнал оповещений** появится новая запись.

    ![Добавление правила устройства][img-adddevicerule4]

1. На странице **Правила** панели мониторинга можно просматривать и изменять все правила.

    ![Список правил устройства][img-rules]

1. На странице **Действия** панели мониторинга можно просматривать и изменять все действия, которые могут быть предприняты при выполнении условия, заданного правилом.

    ![Список действий с устройством][img-actions]

> [!NOTE]
> Вы можете определить действия, при которых будут отправляться сообщения или SMS в случае выполнения условия, заданного правилом. Кроме того, действия можно интегрировать с бизнес-системой с помощью [приложения логики][lnk-logic-apps]. Дополнительные сведения см. в статье [Руководство. Подключение приложения логики к предварительно настроенному решению для удаленного мониторинга Azure IoT Suite][lnk-logicapptutorial].

### <a name="manage-filters"></a>Управление фильтрами

В списке устройств вы можете создавать, сохранять и обновлять фильтры, чтобы отобразить настраиваемый список устройств, подключенных к центру. Чтобы создать фильтр, сделайте следующее:

1. Щелкните значок изменения фильтра, расположенный над списком устройств.

    ![Открытие редактора фильтров][img-editfiltericon]

1. В разделе **Редактор фильтров** можно добавить поля, операторы и значения для фильтрации списка устройств. Вы можете добавить несколько предложений для уточнения фильтра. Щелкните параметр **Фильтр**, чтобы применить фильтр.

    ![Создание фильтра][img-filtereditor]

1. В этом примере список фильтруется по изготовителю и номеру модели.

    ![Фильтрованный список][img-filterelist]

1. Чтобы сохранить фильтр с пользовательским именем, щелкните значок **Сохранить как**.

    ![Сохранение фильтра][img-savefilter]

1. Чтобы повторно применить ранее сохраненный фильтр, щелкните значок **Открыть сохраненный фильтр**.

    ![Открытие фильтра][img-openfilter]

Вы можете создавать фильтры на основе идентификатора устройства, его состояния, требуемых или переданных свойств и тегов. Добавьте собственные настраиваемые теги для устройства в разделе **Теги** панели **Сведения об устройстве** или выполните задание, которое обновит теги на нескольких устройствах.

> [!NOTE]
> Если вам необходимо изменить текст запроса, это можно сделать непосредственно, использовав **расширенное представление** в **редакторе фильтров**.

### <a name="commands"></a>Команды

В области **Сведения об устройстве** можно отправить команды на устройство. При первом запуске устройство отправляет решению сведения о командах, которые оно поддерживает. Описание различий между командами и методами см. в статье [Параметры отправки сообщений из облака на устройство с помощью Центра Интернета вещей Azure][lnk-c2d-guidance].

1. В области **Сведения об устройстве** выбранного устройства щелкните **Команды**.

   ![Команды устройств на панели мониторинга][img-devicecommands]

1. В списке команд выберите **PingDevice** .

1. Щелкните **Отправить команду**.

1. В журнале команд отобразится состояние команды.

   ![Состояние команды на панели мониторинга][img-pingcommand]

Решение отслеживает состояние каждой отправляемой команды. Изначальное состояние команды — **Ожидание**. Когда устройство сообщает о выполнении команды, значение состояния изменяется на **Успешно**.

## <a name="behind-the-scenes"></a>Сопутствующие ресурсы

При развертывании предварительно настроенного решения создается несколько ресурсов в выбранной подписке Azure. Эти ресурсы можно просмотреть на [портале Azure][lnk-portal]. Имя **группы ресурсов** , создаваемой при развертывании, зависит от имени, выбранного для предварительно настроенного решения.

![Предварительно настроенное решение на портале Azure][img-portal]

Параметры каждого ресурса можно просмотреть, выбрав его в списке в группе ресурсов.

Можно также просмотреть исходный код предварительно настроенного решения. Исходный код предварительно настроенного решения для удаленного мониторинга находится в репозитории GitHub [azure-iot-remote-monitoring][lnk-rmgithub].

* В папке **DeviceAdministration** содержится исходный код для панели мониторинга.
* В папке **Simulator** содержится исходный код для виртуального устройства.
* В папке **EventProcessor** содержится исходный код для фонового процесса, который обрабатывает входящие данные телеметрии.

Завершив работу, можно удалить предварительно настроенное решение из подписки Azure на сайте [azureiotsuite.com][lnk-azureiotsuite]. На этом сайте можно легко удалить все ресурсы, подготовленные при создании предварительно настроенного решения.

> [!NOTE]
> Чтобы полностью удалить все ресурсы, связанные с предварительно настроенным решением, а не группу ресурсов на портале, удалите эти ресурсы на сайте [azureiotsuite.com][lnk-azureiotsuite].

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы развернули рабочее предварительно настроенное решение, вы можете продолжить знакомство с IoT Suite. См. следующие статьи.

* [Пошаговое руководство по работе с настроенным решением для удаленного мониторинга][lnk-rm-walkthrough]
* [Подключение устройства к предварительно настроенному решению для удаленного мониторинга][lnk-connect-rm]
* [Разрешения на сайте azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-v1-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-v1-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-v1-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-v1-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-v1-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-v1-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-v1-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-v1-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-v1-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-v1-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-v1-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-v1-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-v1-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-v1-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-v1-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-v1-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-v1-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-v1-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-v1-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-v1-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-v1-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-v1-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-v1-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-v1-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-v1-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-v1-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-v1-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-v1-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-v1-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-v1-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-v1-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-v1-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-v1-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-v1-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-v1-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-v1-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-v1-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-v1-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-v1-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-v1-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-v1-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-v1-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-v1-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-v1-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-v1-connecting-devices.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-v1-faq.md