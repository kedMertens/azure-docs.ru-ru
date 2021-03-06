---
title: Резервное копирование и восстановление зашифрованных виртуальных машин с помощью службы Azure Backup
description: В этой статье приведены сведения о резервном копировании и восстановлении виртуальных машин, зашифрованных с помощью шифрования дисков Azure.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 7/10/2018
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b060fc3d273a0243271d2c38f90e81f83857e79
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420334"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Резервное копирование и восстановление зашифрованных виртуальных машин с помощью службы Azure Backup
В этой статье рассказывается о действиях по резервному копированию и восстановлению виртуальных машин с помощью службы Azure Backup. Кроме того, здесь приведены поддерживаемые сценарии, предварительные требования и сведения об устранении неполадок при возникновении ошибок.

## <a name="supported-scenarios"></a>Поддерживаемые сценарии использования.

 Резервное копирование и восстановление зашифрованных виртуальных машин поддерживается только для виртуальных машин, использующих модель развертывания с помощью Azure Resource Manager, и не поддерживается для виртуальных машин, использующих классическую модель развертывания. Резервное копирование и восстановление зашифрованных виртуальных машин поддерживается для виртуальных машин Windows и Linux, использующих шифрование дисков Azure. Шифрование дисков использует стандартный для отрасли компонент BitLocker в Windows и DM-Crypt в Linux. В приведенной ниже таблице показаны тип шифрования и поддержка виртуальных машин.

   |  | Виртуальные машины BEK + KEK | Виртуальные машины, зашифрованные только с помощью BEK |
   | --- | --- | --- |
   | **Неуправляемые виртуальные машины**  | Yes | Yes  |
   | **Управляемые виртуальные машины**  | Yes | Yes  |

## <a name="prerequisites"></a>Предварительные требования
* Виртуальная машина была зашифрована с помощью [шифрования дисков Azure](../security/azure-security-disk-encryption.md).

* Хранилище служб восстановления создано и репликация хранилища установлена согласно инструкциям в статье [Подготовка среды к архивации виртуальных машин, развернутых с помощью Resource Manager](backup-azure-arm-vms-prepare.md).

* Службе Azure Backup предоставлены [разрешения для доступа к хранилищу ключей](#provide-permissions-to-backup), содержащему ключи и секреты для зашифрованных виртуальных машин.

## <a name="backup-encrypted-vm"></a>Резервное копирование зашифрованной виртуальной машины
Выполните приведенные ниже действия, чтобы выбрать цель резервного копирования, определить политику, настроить элементы и активировать резервное копирование.

### <a name="configure-backup"></a>Настройка резервного копирования
1. Если хранилище служб восстановления уже открыто, перейдите к следующему шагу. Если хранилище служб восстановления не открыто, на портале Azure выберите **Все службы**.

   a. В списке ресурсов введите **Службы восстановления**.

   b. Как только вы начнете вводить символы, список отфильтруется соответствующим образом. Когда вы увидите пункт **Хранилища служб восстановления**, щелкните его.

      ![Хранилище служб восстановления](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. После этого отобразится список хранилищ служб восстановления, Выберите хранилище из списка.

     Затем откроется панель мониторинга выбранного хранилища.
1. В открывшемся списке элементов хранилища щелкните **Резервное копирование**, чтобы начать резервное копирование зашифрованной виртуальной машины.

      ![Колонка резервного копирования](./media/backup-azure-vms-encryption/select-backup.png)
1. В элементе **Резервное копирование** выберите **Backup goal** (Цель резервного копирования).

      ![Колонка сценариев](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
1. В раскрывающемся списке **Where is your workload running?** (Где выполняется рабочая нагрузка?) выберите **Azure**. В раскрывающемся списке **What do you want to backup?** (Что необходимо архивировать?) выберите **Виртуальная машина**. Нажмите кнопку **ОК**.

   ![Открытие колонки сценария](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
1. В раскрывающемся списке **Choose backup policy** (Выберите политику резервного копирования) выберите политику резервного копирования для своего хранилища. Нажмите кнопку **ОК**.

      ![Выбор политики резервного копирования](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Указаны сведения о политике по умолчанию. Если вы хотите создать политику, в раскрывающемся списке выберите **Создать**. Как только вы нажмете кнопку **ОК**, политика резервного копирования будет связана с хранилищем.

1. Выберите зашифрованные виртуальные машины, чтобы связать их с указанной политикой, и нажмите кнопку **OК**.

      ![Выбор зашифрованных виртуальных машин](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
1. На этой странице приведены сведения о хранилище ключей, связанном с выбранными зашифрованными виртуальными машинами. Для резервного копирования требуется доступ только для чтения к ключам и секретам в хранилище ключей. Эти разрешения используются, чтобы создать резервную копию ключа и секрета, а также связанных виртуальных машин.<br>
Если вы являетесь **участником**, при включении резервного копирования будет без труда получен доступ к хранилищу ключей. Это позволит выполнить резервное копирование зашифрованных виртуальных машин без вмешательства пользователя.

   ![Сообщение для зашифрованных виртуальных машин](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   Если вы являетесь **гостевым пользователем**, необходимо предоставить службе резервного копирования разрешения для доступа к хранилищу ключей (для выполнения резервного копирования). Эти разрешения можно предоставить, следуя [инструкциям в следующем разделе](#provide-permissions-to-backup).

   ![Сообщение для зашифрованных виртуальных машин](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)
 
    Теперь, когда все параметры для хранилища заданы, в нижней части страницы нажмите кнопку **Включить резервное копирование**. После нажатия кнопки **Включить резервное копирование** для хранилища и виртуальных машин будет развернута политика.
  
1. На следующем шаге подготовки нужно установить агент виртуальной машины или убедиться, что он установлен. Для этого выполните действия, указанные в статье [Подготовка среды к архивации виртуальных машин, развернутых с помощью Resource Manager](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>активация задания архивации;
Следуйте указаниям в статье [Архивация виртуальных машин Azure в хранилище служб восстановления](backup-azure-arm-vms.md), чтобы активировать задание резервного копирования.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Продолжение резервного копирования уже архивированных виртуальных машин с включенным шифрованием  
Если у вас есть виртуальные машины, которые уже архивируются в хранилище служб восстановления и разрешены для дальнейшего шифрования, нужно предоставить службе резервного копирования разрешения на доступ к хранилищу ключей, чтобы архивация продолжилась. Эти разрешения можно предоставить, следуя [действиям, описанным в следующем разделе](#provide-permissions-to-azure-backup). Или можно выполнить действия PowerShell из раздела о включении резервного копирования в [документации по PowerShell](backup-azure-vms-automation.md). 

## <a name="provide-permissions-to-backup"></a>Предоставление разрешений для службы резервного копирования
Чтобы предоставить службе резервного копирования соответствующие разрешения для доступа к хранилищу ключей и архивации зашифрованных виртуальных машин, выполните следующие действия:
1. Выберите **Все службы** и найдите **хранилища ключей**.

    ![Хранилища ключей](./media/backup-azure-vms-encryption/search-key-vault.png)
    
1. В списке хранилищ ключей выберите хранилище ключей, связанное с зашифрованной виртуальной машиной, которую требуется архивировать.

     ![Выбор хранилища ключей](./media/backup-azure-vms-encryption/select-key-vault.png)
     
1. Выберите **Политики доступа**, а затем щелкните **Добавить**.

    ![Добавить](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
1. Выберите **Выбор субъекта** и в поле поиска введите **Служба управления резервным копированием**. 

    ![Поиск службы резервного копирования](./media/backup-azure-vms-encryption/search-backup-service.png)
    
1. Выберите **службу управления резервным копированием**, а затем нажмите кнопку **Выбрать**.

    ![Выбор службы резервного копирования](./media/backup-azure-vms-encryption/select-backup-service.png)
    
1. В раскрывающемся списке **Настроить при помощи шаблона (необязательно)** выберите **Служба архивации Azure**. Необходимые разрешения автоматически заполняются в раскрывающихся списках **Разрешения ключей** и **Разрешения секретов**. Если виртуальная машина зашифрована с использованием **только BEK**, необходимо отменить выбор **разрешений для ключей**, так как разрешения требуются только для секретов.

    ![Выбор службы Azure Backup](./media/backup-azure-vms-encryption/select-backup-template.png)
    
1. Нажмите кнопку **ОК**. Обратите внимание, что в колонке **Политики доступа** добавлена **служба управления резервным копированием**. 

    ![Политики доступа](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
1. Выберите **Сохранить**, чтобы предоставить необходимые разрешения для службы резервного копирования.

    ![Политика доступа для службы резервного копирования](./media/backup-azure-vms-encryption/save-access-policy.png)

После предоставления разрешений можно приступить к включению резервного копирования для зашифрованных виртуальных машин.

## <a name="restore-an-encrypted-vm"></a>Восстановление зашифрованной виртуальной машины
Для восстановления зашифрованной виртуальной машины сначала необходимо восстановить диски, выполнив инструкции по восстановлению заархивированных дисков в разделе [Выбор конфигурации восстановления для виртуальной машины](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). После этого можно выбрать один из следующих вариантов:

* Выполните команды PowerShell, описанные в разделе [Создание виртуальной машины с восстановленного диска](backup-azure-vms-automation.md#create-a-vm-from-restored-disks), чтобы создать полную виртуальную машину из восстановленных дисков.
* Или [используйте шаблоны для настройки восстановленной виртуальной машины](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm), чтобы создать виртуальные машины из восстановленных дисков. Шаблоны можно использовать только для точек восстановления, созданных после 26 апреля 2017 года.

## <a name="troubleshooting-errors"></a>Устранение ошибок
| Операция | Сведения об ошибке | Способы устранения: |
| --- | --- | --- |
|Azure Backup | Служба резервного копирования не имеет достаточных разрешений к хранилищу ключей для резервного копирования зашифрованных виртуальных машин. | Службе необходимо предоставить эти разрешения, следуя [действиям в предыдущем разделе](#provide-permissions-to-azure-backup). Или можно выполнить действия с PowerShell в подразделе о включении защиты документации PowerShell из раздела [Резервное копирование виртуальных машин Azure](backup-azure-vms-automation.md#back-up-azure-vms). |  
| восстановление; |Вы не можете восстановить эту зашифрованную виртуальную машину, так как с ней связано хранилище ключей. |Создайте хранилище ключей с помощью действий, описанных в статье [Приступая к работе с хранилищем ключей Azure](../key-vault/key-vault-get-started.md). Сведения о том, как восстановить ключ и секрет, если они отсутствуют, см. в статье [Восстановление ключа и секрета в хранилище ключей для зашифрованных виртуальных машин с помощью службы архивации Azure](backup-azure-restore-key-secret.md). |
| восстановление; |Вы не можете восстановить эту зашифрованную виртуальную машину, так как с ней не связаны ключ и секрет. |Сведения о том, как восстановить ключ и секрет, если они отсутствуют, см. в статье [Восстановление ключа и секрета в хранилище ключей для зашифрованных виртуальных машин с помощью службы архивации Azure](backup-azure-restore-key-secret.md). |
| восстановление; |У службы резервного копирования нет разрешения на доступ к ресурсам в вашей подписке. |Как упоминалось ранее, сначала восстановите диски, выполнив инструкции по восстановлению заархивированных дисков в разделе [Выбор конфигурации восстановления для виртуальной машины](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). После этого с помощью PowerShell [создайте виртуальную машину из восстановленного диска](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
