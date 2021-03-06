---
title: Уточнение группы оценки с помощью сопоставления зависимостей группы в службе "Миграция Azure" | Документация Майкрософт
description: В этой статье описывается, как уточнить оценку с помощью сопоставления зависимостей группы в службе "Миграция Azure".
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 37c4ce8638c8f0481151449317d6cd387b61b256
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622904"
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Уточнение группы с помощью сопоставления зависимостей группы

В этой статье описывается, как уточнить группу путем визуализации зависимостей всех компьютеров в группе. Этот метод обычно используется, когда необходимо уточнить членство в существующей группе, выполнив перекрестную проверку зависимостей группы перед выполнением оценки. Уточнение группы с помощью визуализации зависимостей помогает эффективно cпланировать миграцию в Azure. Так можно обнаружить все взаимозависимые системы, которые необходимо переносить вместе, гарантируя полный перенос без простоев. 


> [!NOTE]
> Группы, для которых необходимо визуализировать зависимости, должны содержать не более 10 компьютеров. Если в группе более 10 компьютеров, рекомендуется разделить ее на меньшие группы, чтобы задействовать возможности визуализации зависимостей.


# <a name="prepare-the-group-for-dependency-visualization"></a>Подготовка группы к визуализации зависимостей
Чтобы просмотреть зависимости группы, необходимо скачать и установить агенты на каждом локальном компьютере, который входит в эту группу. Кроме того, если у вас есть компьютеры без подключения к Интернету, на них необходимо скачать и установить [шлюз OMS](../log-analytics/log-analytics-oms-gateway.md).

### <a name="download-and-install-the-vm-agents"></a>Скачивание и установка агентов виртуальной машины
1. В области **Обзор** щелкните **Управление** > **Группы** и перейдите к необходимой группе.
2. В списке компьютеров в столбце **Агент зависимостей** щелкните **Требует установки** для просмотра инструкций о том, как скачать и установить агенты.
3. На странице **Зависимости** скачайте и установите Microsoft Monitoring Agent (MMA) и агент зависимостей на каждую виртуальную машину, которая входит в группу.
4. Скопируйте идентификатор и ключ рабочей области. Они требуются для установки MMA на локальных компьютерах.

### <a name="install-the-mma"></a>Установка MMA

Вот как можно установить агент на компьютере Windows.

1. Дважды щелкните скачанный файл агента.
2. На странице **приветствия** нажмите кнопку **Далее**. На странице **Условия лицензии** нажмите кнопку **Принимаю**, чтобы принять условия лицензии.
3. В поле **Конечная папка** оставьте или измените папку установки по умолчанию. Нажмите кнопку **Далее**. 
4. В разделе **Параметры установки агента** последовательно выберите **Azure Log Analytics** > **Далее**. 
5. Щелкните **Добавить**, чтобы добавить новую рабочую область Log Analytics. Вставьте идентификатор и ключ рабочей области, скопированные на портале. Щелкните **Далее**.


Вот как можно установить агент на компьютере Linux.

1. Перенесите соответствующий пакет (x86 или x64) на компьютер Linux с помощью scp или sftp.
2. Установите пакет, используя аргумент --install.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Установка агента зависимостей
1. Чтобы установить агент зависимостей на компьютере Windows, дважды щелкните файл установки и следуйте инструкциям мастера.
2. Чтобы установить агент зависимостей на компьютере Linux, сделайте это с правами привилегированного пользователя, используя следующую команду.

    ```sh InstallDependencyAgent-Linux64.bin```

Узнайте больше о поддержке агента зависимостей для ОС [Windows](../monitoring/monitoring-service-map-configure.md#supported-windows-operating-systems) и [Linux](../monitoring/monitoring-service-map-configure.md#supported-linux-operating-systems).

## <a name="refine-the-group-based-on-dependency-visualization"></a>Уточнение группы на основе визуализации зависимостей
После установки агентов на всех компьютерах группы можно визуализировать зависимости группы и уточнить ее, выполнив приведенные ниже действия.

1. В проекте службы "Миграция Azure" в разделе **Управление** щелкните  **Группы** и выберите группу.
2. На странице группы щелкните  **Просмотреть зависимости**, чтобы открыть сопоставление зависимостей группы.
3. На карте зависимостей группы появятся следующие сведения:
    - Входящие (клиентские) и исходящие (серверные) TCP-подключения ко всем компьютерам, входящим в группу, и от них.
        - Зависимые компьютеры, на которых не установлен агент MMA и агент зависимостей, будут сгруппированы по номерам портов.
        - Зависимые компьютеры, на которых установлен агент MMA и агент зависимостей, будут отображаться в отдельных полях. 
    - Чтобы просмотреть процессы, запущенные на компьютере, разверните поле каждого компьютера.
    - Чтобы просмотреть сведения о таких свойствах, как полное доменное имя (FQDN), операционная система и MAC-адрес каждого компьютера, щелкните поле каждого компьютера.

     ![Просмотр зависимостей группы](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Чтобы просмотреть более подробные сведения о зависимостях, щелкните диапазон времени, чтобы изменить его. По умолчанию диапазон равен одному часу. Можно изменить диапазон времени или указать даты начала и окончания и длительность.
4. Проверьте зависимые компьютеры, процесс, выполняющийся внутри каждого компьютера, и определите компьютеры, которые следует добавить в группу или удалить из нее.
5. Чтобы выбрать несколько компьютеров на карте для их добавления или удаления, щелкните их, удерживая клавишу CTRL.
    - Добавить можно только компьютеры, которые были обнаружены.
    - Добавление и удаление компьютеров из группы делает недействительными ее предыдущие оценки.
    - При необходимости можно создать новую оценку при изменении группы.
5. Нажмите кнопку **ОК**, чтобы сохранить группу.

    ![Добавление или удаление компьютеров](./media/how-to-create-group-dependencies/add-remove.png)

Если вы хотите проверить зависимости конкретного компьютера, который отображается в сопоставлении зависимостей группы, [настройте сопоставление зависимостей компьютера](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Дополнительная информация

[Узнайте больше](concepts-assessment-calculation.md) о вычислении оценок.
