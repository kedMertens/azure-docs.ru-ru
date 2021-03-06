---
title: Мониторинг обновлений в Azure Stack с помощью привилегированной конечной точки | Документация Майкрософт
description: Узнайте, как использовать привилегированную конечную точку для мониторинга состояния обновления интегрированных систем Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2018
ms.author: mabrigg
ms.openlocfilehash: 8f384a79811c9a9b104acb98c8f6b6e162946ab8
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2018
ms.locfileid: "41947950"
---
# <a name="monitor-updates-in-azure-stack-using-the-privileged-endpoint"></a>Мониторинг обновлений в Azure Stack с помощью привилегированной конечной точки

*Область применения: интегрированные системы Azure Stack*

Привилегированную конечную точку можно использовать для мониторинга хода выполнения обновления Azure Stack или возобновления невыполненного обновления с последнего успешного шага, если портал Azure Stack стал недоступным.  Использование портала Azure Stack — рекомендуемый метод для управления обновлениями в Azure Stack.

В обновление 1710 интегрированных систем Azure Stack включены следующие новые командлеты PowerShell для управления обновлениями.

| Командлет  | ОПИСАНИЕ  |
|---------|---------|
| `Get-AzureStackUpdateStatus` | Возвращает состояние выполняемого в настоящее время, завершенного или невыполненного обновления. Предоставляет общее состояние операции обновления и XML-документ, описывающий текущий шаг и соответствующее состояние. |
| `Resume-AzureStackUpdate` | Возобновляет невыполненные обновления с точки, в которой произошел сбой. В некоторых сценариях может потребоваться выполнить шаги по устранению рисков, прежде чем продолжить обновление.         |
| | |

## <a name="verify-the-cmdlets-are-available"></a>Проверка доступности командлетов
Так как командлеты появились в пакете обновления 1710 для Azure Stack, процессу обновления 1710 необходимо достигнуть определенной точки прежде, чем возможность мониторинга станет доступна. Как правило, командлеты доступны, если состояние на портале администратора указывает, что обновление 1710 находится на шаге **Restart Storage Hosts** (Перезапуск узлов хранения). В частности, обновление командлетов происходит на шаге **Выполнение шага 2.6. Обновление списка разрешений привилегированной конечной точки**.

Вы также можно определить, доступны ли командлеты, программным способом, запросив список команд из привилегированной конечной точки. Для этого запустите следующие команды с узла жизненного цикла оборудования или с рабочей станции привилегированного доступа. Кроме того, убедитесь, что привилегированной конечной точкой является доверенный узел. Дополнительные сведения см. на шаге 1 раздела о процедуре [доступа к привилегированной конечной точке](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). 

1. Создайте сеанс PowerShell на любой из виртуальных машин ERCS в среде Azure Stack (*префикс*-ERCS01, *префикс*-ERCS02 или *префикс*-ERCS03). Замените *префикс* строкой префикса виртуальной машины, которая соответствует используемой среде.

   ```powershell
   $cred = Get-Credential

   $pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
   ```
   При запросе учетных данных используйте учетную запись "&lt;*домен Azure Stack*&gt;\cloudadmin" или учетную запись участника группы администраторов облака. Для учетной записи cloudadmin введите пароль, который использовался во время установки учетной записи администратора домена AzureStackAdmin.

2. Получите полный список команд, доступных в привилегированной конечной точке. 

   ```powershell
   $commands = Invoke-Command -Session $pepSession -ScriptBlock { Get-Command } 
   ```
3. Определите, была ли обновлена привилегированная конечная точка.

   ```powershell
   $updateManagementModuleName = "Microsoft.Azurestack.UpdateManagement"
    if (($commands | ? Source -eq $updateManagementModuleName)) {
   Write-Host "Privileged endpoint was updated to support update monitoring tools."
    } else {
   Write-Host "Privileged endpoint has not been updated yet. Please try again later."
    } 
   ```

4. Выведите список команд модуля Microsoft.AzureStack.UpdateManagement.

   ```powershell
   $commands | ? Source -eq $updateManagementModuleName 
   ```
   Например: 
   ```powershell
   $commands | ? Source -eq $updateManagementModuleName
   
   CommandType     Name                                               Version    Source                                                  PSComputerName
    -----------     ----                                               -------    ------                                                  --------------
   Function        Get-AzureStackUpdateStatus                         0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   Function        Resume-AzureStackUpdate                            0.0        Microsoft.Azurestack.UpdateManagement                   Contoso-ercs01
   ``` 

## <a name="use-the-update-management-cmdlets"></a>Использование командлетов для управления обновлениями

> [!NOTE]
> Запустите следующие команды с узла жизненного цикла оборудования или с рабочей станции привилегированного доступа. Кроме того, убедитесь, что привилегированной конечной точкой является доверенный узел. Дополнительные сведения см. на шаге 1 раздела о процедуре [доступа к привилегированной конечной точке](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).

### <a name="connect-to-the-privileged-endpoint-and-assign-session-variable"></a>Подключение к привилегированной конечной точке и присвоение переменной сеанса

Выполните следующие команды, чтобы создать сеанс PowerShell на любой из виртуальных машин ERCS в среде Azure Stack (*префикс*-ERCS01, *префикс*-ERCS02 или *префикс*-ERCS03) и присвоить переменную сеанса.

```powershell
$cred = Get-Credential

$pepSession = New-PSSession -ComputerName <Prefix>-ercs01 -Credential $cred -ConfigurationName PrivilegedEndpoint 
```
 При запросе учетных данных используйте учетную запись "&lt;*домен Azure Stack*&gt;\cloudadmin" или учетную запись участника группы администраторов облака. Для учетной записи cloudadmin введите пароль, который использовался во время установки учетной записи администратора домена AzureStackAdmin.

### <a name="get-high-level-status-of-the-current-update-run"></a>Получение общего состояния текущего выполнения обновления 

Чтобы получить общее состояние текущего выполнения обновления, выполните следующие команды: 

```powershell
$statusString = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus -StatusOnly }

$statusString.Value 
```

Ниже перечислены возможные значения.

- Выполнение
- Завершено
- Сбой 
- Canceled

Можно выполнить эти команды несколько раз, чтобы отслеживать последнее состояние. Для повторной проверки не нужно повторно устанавливать подключение.

### <a name="get-the-full-update-run-status-with-details"></a>Получение полного состояния выполнения обновления с подробными сведениями 

Вы можете получить полную сводку по выполнению обновления в виде строки XML. Можно записать строку в файл для проверки или преобразовать ее в XML-документ и проанализировать с помощью PowerShell. Следующая команда выполняет синтаксический анализ XML-файла для получения иерархического списка выполняющихся действий.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']")
```

В следующем примере шаг верхнего уровня (обновление облака) имеет дочерний план для обновления и перезапуска узлов хранения. В нем видно, что план перезапуска узлов хранения обновляет службу хранилища BLOB-объектов на одном из узлов.

```powershell
[xml]$updateStatus = Invoke-Command -Session $pepSession -ScriptBlock { Get-AzureStackUpdateStatus }

$updateStatus.SelectNodes("//Step[@Status='InProgress']") 

    FullStepIndex : 2
    Index         : 2
    Name          : Cloud Update
    Description   : Perform cloud update.
    StartTimeUtc  : 2017-10-13T12:50:39.9020351Z
    Status        : InProgress
    Task          : Task
    
    FullStepIndex  : 2.9
    Index          : 9
    Name           : Restart Storage Hosts
    Description    : Restart Storage Hosts.
    EceErrorAction : Stop
    StartTimeUtc   : 2017-10-13T15:44:06.7431447Z
    Status         : InProgress
    Task           : Task
    
    FullStepIndex : 2.9.2
    Index         : 2
    Name          : PreUpdate ACS Blob Service
    Description   : Check function level, update deployment artifacts, configure Blob service settings
    StartTimeUtc  : 2017-10-13T15:44:26.0708525Z
    Status        : InProgress
    Task          : Task
```

### <a name="resume-a-failed-update-operation"></a>Возобновление операции в случае сбоя

В случае сбоя обновления можно возобновить обновление с точки, где оно остановилось.

```powershell
Invoke-Command -Session $pepSession -ScriptBlock { Resume-AzureStackUpdate } 
```

## <a name="troubleshoot"></a>Устранение неполадок

Привилегированная конечная точка доступна на всех виртуальных машинах ERCS в среде Azure Stack. Так как подключение к конечной точке высокой доступности не установлено, возможны нерегулярные перерывы в работе, предупреждения или сообщения об ошибках. Эти сообщения могут свидетельствовать о том, что сеанс был отключен или что произошла ошибка связи со службой ECE. Это ожидаемое поведение. Можно повторить операцию через несколько минут или создать сеанс привилегированной конечной точки на одной из других виртуальных машин ERCS. 

## <a name="next-steps"></a>Дополнительная информация

- [Общие сведения об управлении обновлениями в Azure Stack](azure-stack-updates.md) 


