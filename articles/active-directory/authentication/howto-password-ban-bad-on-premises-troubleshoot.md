---
title: Устранение неполадок и ведение журнала в предварительной версии защиты паролем Azure AD
description: Общие сведения об устранении неполадок и ведении журнала в предварительной версии защиты паролем Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 1eea6380d4276644db0c7681f23a4b0c5e79ff09
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187355"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Предварительная версия. Мониторинг, отчетность и устранение неполадок защиты паролем Azure AD

|     |
| --- |
| Защита паролем Azure AD и пользовательский список заблокированных паролей — это функции Azure Active Directory, которые предоставляются в режиме общедоступной предварительной версии. См. дополнительные сведения о [дополнительных условиях использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

После развертывания защиты паролем Azure AD мониторинг и отчетность являются важными задачами. В этой статье мы подробно рассмотрим, где каждая служба регистрирует сведения и создает отчеты об использовании защиты паролем Azure AD.

## <a name="on-premises-logs-and-events"></a>Локальные журналы и события

### <a name="dc-agent-service"></a>Службы агента контроллера домена

На каждом контроллере домена программное обеспечение службы агента контроллера домена записывает результаты проверки паролей (и другие состояния) в локальный журнал событий: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin

События регистрируются различными компонентами агента контроллера домена с использованием следующих диапазонов:

|Компонент |Диапазон идентификаторов событий|
| --- | --- |
|Библиотека DLL фильтрации паролей агента контроллера домена| 10000–19999|
|Процесс размещения службы агента контроллера домена| 20000–29999|
|Логика проверки политики службы агента контроллера домена| 30000–39999|

Обычно для успешной операции проверки пароля из библиотеки DLL фильтрации паролей агента контроллера домена регистрируется одно событие, а в случае неудачной проверки пароля — два: одно из службы агента контроллера домена и одно из библиотеки DLL фильтрации паролей агента контроллера домена.

Дискретные события для фиксации этих ситуаций регистрируются на основе следующих факторов:

* Будет ли настроен или изменен заданный пароль.
* Была ли пройдена проверка данного пароля.
* Произошла ли ошибка проверки из-за глобальной политики Майкрософт или из-за политики организации.
* Включен или выключен в данный момент режим "Только аудит" для текущей политики паролей.

Ключевыми событиями, связанными с проверкой паролей, являются следующие:

|   |Изменение пароля |Настройка пароля|
| --- | :---: | :---: |
|Pass; |10014 |10015|
|Ошибка (не прошел проверку политики паролей клиента)| 10016, 30002| 10017, 30003|
|Ошибка (не прошел проверку политики паролей Майкрософт)| 10016, 30004| 10017, 30005|
|Прошел проверку "Только аудит" (могла возникнуть ошибка проверки политики паролей клиента)| 10024, 30008| 10025, 30007|
|Прошел проверку "Только аудит" (могла возникнуть ошибка проверки политики паролей Майкрософт)| 10024, 30010| 10025, 30009|

> [!TIP]
> Входящие пароли сначала проверяются в соответствии со списком глобальных паролей Майкрософт. Если это не удается, дальнейшая обработка не выполняется. Это то же поведение, что и при изменении пароля в Azure.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Пример сообщения журнала событий для идентификатора события 10014

Измененный пароль для указанного пользователя был подтвержден как совместимый с текущей политикой паролей Azure.

 UserName: BPL_02885102771 FullName:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Пример сообщения журнала событий для идентификатора события 10017 и 30003 (не удалось выполнить настройку пароля)

10017:

Сброшенный пароль для указанного пользователя был отклонен, так как он не соответствовал текущей политике паролей Azure. Дополнительные сведения см. в связанном сообщении журнала событий.

 UserName: BPL_03283841185 FullName:

30003:

Сброшенный пароль для указанного пользователя был отклонен, так как он соответствовал хотя бы одному из токенов, присутствующих в списке заблокированных паролей для каждого клиента текущей политики паролей Azure.

 UserName: BPL_03283841185 FullName:

Некоторые другие ключевые сообщения журнала событий, о которых следует знать:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Пример сообщения журнала событий для идентификатора события 30001

Пароль для указанного пользователя был принят, так как политика паролей Azure пока недоступна

UserName: <user> FullName: <user>

Это условие может быть вызвано одной или несколькими из следующих причин:%n

1. Лес еще не был зарегистрирован в Azure.

   Действия по устранению. Администратор должен зарегистрировать лес, используя командлет Register-AzureADPasswordProtectionForest.

2. Прокси-сервер защиты паролей Azure AD еще не доступен по крайней мере на одном из компьютеров в текущем лесу.

   Действия по устранению. Администратор должен установить и зарегистрировать прокси-сервер, используя командлет Register-AzureADPasswordProtectionProxy.

3. Этот контроллер домена не имеет сетевого подключения к любым экземплярам прокси-сервера защиты паролей Azure AD.

   Действия по устранению. Убедитесь, что сетевое подключение существует по крайней мере для одного экземпляра прокси-сервера защиты паролей Azure AD.

4. Этот контроллер домена не подключен к другим контроллерам домена в домене.

   Действия по устранению. Обеспечьте сетевое подключение к домену.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Пример сообщения журнала событий для идентификатора события 30006

Теперь служба применяет следующую политику паролей Azure.

 AuditOnly: 1

 Дата глобальной политики: 2018-05-15T00:00:00.000000000Z

 Дата политики клиента: 2018-06-10T20:15:24.432457600Z

 Принудительное использование политики клиента: 1

#### <a name="dc-agent-log-locations"></a>Расположения журналов агента контроллера домена

Служба агента контроллера домена также будет регистрировать события, связанные с операциями, в следующем журнале: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

Служба агента контроллера домена также может записывать подробные события трассировки на уровне отладки в следующий журнал: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace

> [!WARNING]
> Журнал трассировки по умолчанию отключен. Когда этот параметр включен, этот журнал получает большой объем событий и может влиять на производительность контроллера домена. Поэтому этот расширенный журнал следует включать, только когда проблема требует более глубокого изучения и только на минимальное время.

### <a name="azure-ad-password-protection-proxy-service"></a>Служба прокси-сервера защиты паролем Azure AD

Служба прокси-сервера защиты паролем выдает минимальный набор событий в следующий журнал событий: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

Служба прокси-сервера защиты паролем выдает минимальный набор событий в следующий журнал событий: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace

> [!WARNING]
> Журнал трассировки по умолчанию отключен. Когда этот параметр включен, этот журнал получает большой объем событий, и это может повлиять на производительность прокси-узла. Поэтому этот журнал следует включать, только когда проблема требует более глубокого изучения и только на минимальное время.

### <a name="dc-agent-discovery"></a>Обнаружение агента контроллера домена

Командлет `Get-AzureADPasswordProtectionDCAgent` может использоваться для отображения базовой информации о различных агентах контроллера домена, работающих в домене или лесу. Эта информация извлекается из объектов serviceConnectionPoint, зарегистрированных запущенными службами агента контроллера домена. Пример выходных данных этого командлета выглядит следующим образом:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Различные свойства обновляются каждой службой агента контроллера домена приблизительно каждый час. Данные по-прежнему зависят от задержки репликации Active Directory.

На объем запроса командлета может влиять использование параметров -Forest или -Domain.

### <a name="emergency-remediation"></a>Аварийное исправление

Если служба агента контроллера домена вызывает проблемы, ее можно немедленно отключить. Библиотека DLL фильтрации паролей агента контроллера домена пытается вызвать нерабочую службу и регистрирует события предупреждения (10012, 10013), но все входящие пароли принимаются в течение этого времени. Затем служба агента контроллера домена также может быть настроена с помощью диспетчера управления службами Windows с типом запуска "Отключено" при необходимости.

### <a name="performance-monitoring"></a>Мониторинг производительности

Программное обеспечение службы агента контроллера домена устанавливает объект счетчика производительности с именем **Azure AD password protection**. В настоящее время доступны следующие счетчики производительности:

|Имя счетчика производительности | ОПИСАНИЕ|
| --- | --- |
|Обработано паролей |Этот счетчик отображает общее количество обработанных паролей (принятых или отклоненных) с момента последнего перезапуска.|
|Принято паролей |Этот счетчик отображает общее количество принятых паролей с момента последнего перезапуска.|
|Отклонено паролей |Этот счетчик отображает общее количество отклоненных паролей с момента последнего перезапуска.|
|Выполняется запросов фильтрации паролей |Этот счетчик отображает количество запросов на фильтрацию паролей, которые в настоящее время выполняются.|
|Пиковое число запросов фильтрации паролей |Этот счетчик отображает максимальное количество одновременных запросов фильтрации паролей с момента последнего перезапуска.|
|Ошибки запросов фильтрации паролей |Этот счетчик отображает общее количество неудачных запросов фильтрации паролей из-за ошибки с момента последнего перезапуска. Ошибки могут возникнуть, если служба агента контроллера домена защиты паролем Azure AD не запущена.|
|Запросы фильтрации паролей/сек |Этот счетчик отображает скорость обработки паролей.|
|Время обработки запроса фильтрации паролей |Этот счетчик отображает среднее время, необходимое для обработки запроса фильтрации паролей.|
|Пиковое количество времени обработки запроса фильтрации паролей |Этот счетчик отображает пиковое количество времени обработки запросов фильтрации паролей с момента последнего перезапуска.|
|Пароли, принятые из-за режима аудита |Этот счетчик отображает общее количество паролей, которые обычно отклоняются, но были приняты, потому что политика паролей была настроена так, чтобы быть в режиме аудита (с момента последнего перезапуска).|

## <a name="directory-services-repair-mode"></a>Режим восстановления служб каталогов

Если контроллер домена загружен в режим восстановления служб каталогов, служба агента контроллера домена обнаруживает это, что приводит к отключению всех проверок паролей или принудительных действий независимо от текущей активной конфигурации политик.

## <a name="domain-controller-demotion"></a>Понижение контроллера домена

Поддерживается понижение контроллера домена, который все еще запускает программное обеспечение агента контроллера. Однако администраторы должны знать, что программное обеспечение агента контроллера домена продолжает работать и применять текущую политику паролей во время процедуры понижения. Новый пароль учетной записи локального администратора (указанный как часть операции понижения) проверяется, как и любой другой пароль. В ходе процедуры понижения контроллера домена корпорация Майкрософт рекомендует выбирать безопасные пароли для локальных учетных записей администратора. Однако проверка нового пароля учетной записи локального администратора с помощью программного обеспечения агента контроллера домена может мешать ранее созданным процедурам понижения.
После успешного понижения роли контроллера домена и перезагрузки и повторного запуска в качестве обычного рядового сервера программное обеспечение агента контроллера домена возвращается к работе в пассивном режиме. Затем его можно удалить в любое время.

## <a name="removal"></a>Удаление

Если было принято решение удалить общедоступную предварительную версию программного обеспечения и очистить все связанные состояния из доменов и леса, эту задачу можно выполнить, сделав следующее:

> [!IMPORTANT]
> Важно выполнить эти шаги по порядку. Если какой-либо экземпляр службы прокси-сервера защиты паролем остается включенным, он будет периодически повторно создавать объект serviceConnectionPoint, а также периодически воссоздавать состояние sysvol.

1. Удалите программное обеспечение защиты паролем со всех компьютеров. Для этого шага **не** требуется перезагрузка компьютера.
2. Удалите программное обеспечение агента контроллера домена со всех контроллеров. Для этого шага **требуется** перезагрузка компьютера.
3. Вручную удалите все точки подключения службы прокси в каждом контексте именования домена. Местоположение этих объектов можно обнаружить с помощью следующей команды Active Directory Powershell:
   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{EBEFB703-6113-413D-9167-9F8DD4D24468}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Не опускайте звездочку (*) в конце значения переменной $keywords.

   Полученный объект, найденный с помощью команды `Get-ADObject`, затем можно переадресовать в `Remove-ADObject` или удалить вручную. 

4. Вручную удалите все точки подключения агента контроллера домена в каждом контексте именования домена. В лесу может быть по одному такому объекту на контроллер домена, в зависимости от того, насколько широко развернута предварительная версия программного обеспечения. Местоположение этого объекта можно обнаружить с помощью следующей команды Active Directory Powershell:

   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Полученный объект, найденный с помощью команды `Get-ADObject`, затем можно переадресовать в `Remove-ADObject` или удалить вручную.

5. Вручную удалите состояние конфигурации уровня леса. Состояние конфигурации леса поддерживается в контейнере в контексте именования конфигурации Active Directory. Его можно обнаружить и удалить следующим образом:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. Вручную удалите все сведения о состоянии sysvol, удалив следующую папку и все ее содержимое:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   При необходимости к этому пути также можно получить доступ локально на данном контроллере домена. Расположение по умолчанию будет выглядеть следующим образом:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Этот путь будет другим, если общий ресурс sysvol настроен в нестандартном местоположении.

## <a name="next-steps"></a>Дополнительная информация

Для получения дополнительной информации о глобальном и пользовательском запрещенных списках паролей см. статью [Запрет неверных паролей в организации](concept-password-ban-bad.md).
