---
title: Архитектура Azure HDInsight с корпоративным пакетом безопасности
description: Дополнительные сведения о планировании безопасности HDInsight с корпоративным пакетом безопасности.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 975a4f7b15d1e1c13767cd7026e961e9d4227603
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998939"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Корпоративный пакет безопасности для HDInsight

Стандартный кластер Azure HDInsight рассчитан на одного пользователя. Он подходит для большинства организаций с небольшими отделами по работе с приложениями, создающими объемные рабочие нагрузки данных. Для каждого пользователя по запросу может быть создан выделенный кластер, который может быть удален, когда перестанет быть нужен. 

Многие организации перешли на новую модель. В ней команды ИТ-специалистов управляют кластерами, которые совместно используются несколькими командами по разработке приложений. Таким крупным предприятиям требуется многопользовательский доступ к кластерам в Azure HDInsight.

HDInsight использует наиболее популярный поставщик удостоверений — управляемую службу Active Directory. Благодаря интеграции HDInsight с [доменными службами Azure Active Directory (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md) доступ к кластерам можно получить с помощью учетных данных домена. 

Виртуальные машины в HDInsight присоединены к указанному домену. Поэтому все службы, запущенные в HDInsight (Ambari, сервер Hive, Ranger, сервер Spark Thrift и т. д.), работают прозрачно для аутентифицированного пользователя. Администраторы могут настраивать строгие политики авторизации на основе Apache Ranger, чтобы применять управление доступом на основе ролей для ресурсов в кластере.


## <a name="integrate-hdinsight-with-active-directory"></a>Интеграция HDInsight с Active Directory

Hadoop с открытым кодом использует протокол Kerberos для аутентификации и обеспечения безопасности. Таким образом, узлы кластера HDInsight с корпоративным пакетом безопасности присоединены к домену под управлением Azure AD DS. Для включенных в кластер компонентов Hadoop настраиваются параметры безопасности Kerberos. 

Для каждого компонента Hadoop автоматически создается субъект-служба. Кроме того, соответствующий субъект компьютера создается для каждого компьютера, присоединенного к домену. Для хранения этих субъектов служб и компьютеров необходимо предоставить подразделение в контроллере домена (Azure AD DS), в котором эти субъекты размещены. 

Кратко перечислим, что вам нужно создать в сетевом окружении:

- Домен Active Directory (под управлением Azure AD DS).
- В Azure AD DS должен быть включен протокол LDAPS.
- Соответствующее сетевое подключение виртуальной сети HDInsight к виртуальной сети Azure AD DS, если вы выбрали для них отдельные виртуальные сети. Виртуальная машина в виртуальной сети HDInsight должна иметь возможность напрямую обращаться к Azure AD DS через пиринг виртуальных сетей. Если HDInsight и Azure AD DS развернуты в одной виртуальной сети, подключение предоставляется автоматически и никаких дальнейших действий не требуется.
- [В Azure AD DS должно быть создано](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) подразделение.
- учетную запись службы со следующими разрешениями:
    - на создание субъектов-служб в подразделении;
    - на присоединение компьютеров к домену и создание субъектов компьютеров в подразделении.

На приведенном ниже снимке экрана представлено такое подразделение в домене contoso.com. На нем также показаны некоторые из субъектов-служб и субъектов компьютеров.

![Подразделение для присоединенных к домену кластеров HDInsight с ESP](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

## <a name="set-up-different-domain-controllers"></a>Настройка разных контроллеров домена
HDInsight в настоящее время поддерживает только Azure AD DS в качестве основного контроллера домена, используемого кластером для передачи данных по протоколу Kerberos. Но возможны и другие сложные конфигурации Active Directory, при условии, что они обеспечивают доступ к HDInsight для Azure AD DS.

### <a name="azure-active-directory-domain-services"></a>Доменные службы Azure Active Directory
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) предоставляет управляемый домен, который полностью совместим с Windows Server Active Directory. Корпорация Майкрософт управляет доменом, применяет к нему исправления и осуществляет его мониторинг в конфигурации высокой доступности. Вы можете развернуть свой кластер, не беспокоясь о поддержке контроллеров домена. 

Пользователи, группы и пароли синхронизируются из Azure Active Directory (Azure AD). Однонаправленная синхронизация из экземпляра Azure AD в Azure AD DS позволяет пользователям входить в кластер, используя те же учетные данные организации. 

Дополнительные сведения см. в разделе [Настройка кластеров HDInsight с ESP с помощью доменных служб Azure AD DS](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>Локальная служба Active Directory или служба Active Directory на виртуальных машинах IaaS

Если у вас используется локальный экземпляр Active Directory или более сложная конфигурация Active Directory для домена, то вы можете синхронизировать их удостоверения в Azure AD с помощью Azure AD Connect. Затем вы можете включить Azure AD DS на этом клиенте Active Directory. 

Так как в протоколе Kerberos используются хэши паролей, необходимо будет [включить синхронизацию хэша пароля в Azure AD DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Если вы используете федерацию на основе служб федерации Active Directory (AD FS), то у вас есть возможность настроить синхронизацию паролей для дополнительной защиты на случай сбоя в инфраструктуре AD FS. Дополнительные сведения см. в статье [Реализация синхронизации хэшированных паролей в службе синхронизации Azure AD Connect](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

Использование только локальной службы Active Directory или Active Directory на виртуальных машинах IaaS, без Azure AD и Azure AD DS, не поддерживается для кластеров HDInsight с ESP.

## <a name="next-steps"></a>Дополнительная информация

* [Настройка кластеров HDInsight c ESP](apache-domain-joined-configure-using-azure-adds.md)
* [Настройка политик Hive в кластерах HDInsight с ESP](apache-domain-joined-run-hive.md)
* [Настройка кластеров HDInsight с помощью ESP](apache-domain-joined-manage.md) 
