---
title: Настройка подключения к гибридному облаку в Azure и Azure Stack | Документация Майкрософт
description: Сведения о настройке подключения к гибридному облаку в Azure и Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/25/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 652d39b4d15569b9365543e02f170664a88715fa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953265"
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Руководство по настройке подключения к гибридному облаку в Azure и Azure Stack

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Доступ к ресурсам безопасности в глобальных Azure и Azure Stack можно получить, используя модель гибридного подключения.

В рамках этого руководства вы создадите пример среды и выполните в ней следующие действия:

> [!div class="checklist"]
> - Сохранять данные локально в соответствии с требованиями конфиденциальности или нормативными требованиями, сохраняя доступ к глобальным ресурсам Azure.
> - Сохранять устаревшую систему при использовании ресурсов и развертывании приложения в масштабе облака, а также хранить ресурсы в глобальной среде Azure.

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack — это расширение Azure.re. Azure Stack позволяет использовать гибкие и инновационные функции облачных вычислений в локальной среде, а также предоставляет единое гибридное облако для создания и развертывания гибридных приложений в любом расположении.  
> 
> В [рекомендациях по проектированию гибридных приложений](https://aka.ms/hybrid-cloud-applications-pillars) описаны характеристики качественного программного обеспечения (размещение, масштабируемость, доступность, устойчивость, управляемость и безопасность), которых следует придерживаться при разработке, развертывании и использовании гибридных приложений. Рекомендации по разработке позволяют оптимизировать создание гибридных приложений, сводя к минимуму проблемы, возникающие в рабочих средах.


## <a name="prerequisites"></a>Предварительные требования

Для развертывания гибридного подключения требуется несколько компонентов. Подготовка некоторых из них требует определенного времени, и это необходимо учитывать при планировании.

**Azure Stack**

Партнер-поставщик OEM или оборудования может развернуть Azure Stack для рабочей среды, а все пользователи могут развернуть пакет средств разработки Azure Stack (ASDK).

**Компоненты Azure Stack**

Оператор Azure Stack должен развернуть службу приложений, создать планы и предложения, создать подписку клиента и добавить образ Windows Server 2016. Если у вас уже есть некоторые из этих компонентов, перед началом работы с руководством убедитесь, что они отвечают требованиям.

В этом руководстве также предполагается, что вы знакомы с Azure и Azure Stack. Перед началом работы прочтите следующие статьи:

 - [Введение в Azure](https://azure.microsoft.com/overview/what-is-azure/).
 - [Основные понятия Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features).

### <a name="azure"></a>Таблицы Azure

 - Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.
 - Создайте [веб-приложение](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) в Azure. Запишите URL-адрес веб-приложения. Он пригодится вам дальше при работе с этим руководством.

### <a name="azure-stack"></a>Azure Stack

 - Используйте рабочую среду Azure Stack или разверните пакет средств разработки Azure Stack отсюда: https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1.
   >[!Note]
   >Развертывание ASDK может занять 7 часов. Учитывайте это при планировании.

 - Разверните службы PaaS [службы приложений](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) в Azure Stack.
 - [Создание планов и предложений](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) в среде Azure Stack.
 - Создайте [подписку клиента](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) в рамках среды Azure Stack.

### <a name="before-you-begin"></a>Перед началом работы

Перед началом настройки гибридного облачного подключения убедитесь, что выполняются следующие условия:

 - У вас есть внешний общедоступный IPV4-адрес для VPN-устройства. Этот IP-адрес не может быть за устройством NAT.
 - Все ресурсы должны быть развернуты в одном регионе и (или) расположении.

#### <a name="tutorial-example-values"></a>Примеры значений для руководства

В примерах этого руководства мы используем следующие значения. Вы можете создать на их основе тестовую среду или просто изучить для лучшего понимания примеров. Общие сведения о параметрах VPN-шлюза см. в статье [Сведения о параметрах конфигурации VPN-шлюза](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

Спецификации подключения:

 - **Тип VPN**: на основе маршрутов
 - **Тип подключения**: "сеть — сеть" (IPsec)
 - **Тип шлюза**: VPN
 - **Имя подключения Azure**: Azure-Gateway-AzureStack-S2SGateway (портал заполнит это значение автоматически)
 - **Имя подключения Azure Stack**: AzureStack-Gateway-Azure-S2SGateway (портал заполнит это значение автоматически)
 - **Общий ключ**: все совместимые с оборудованием VPN с соответствующими значениями на обеих сторонах подключения
 - **Подписка**: любая подписка
 - **Группа ресурсов**: инфраструктура для тестирования

IP-адреса сетей и подсетей:

| Подключение Azure или Azure Stack | ИМЯ | Подсеть | IP-адрес |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Виртуальная сеть Azure | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | Подсеть шлюза<br>10.100.103.0/24 |  |
| Виртуальная сеть Azure Stack | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | Подсеть шлюза <br>10.100101.0/24 |  |
| Шлюз виртуальной сети Azure | Azure-Gateway |  |  |
| Шлюз виртуальной сети Azure Stack | AzureStack-Gateway |  |  |
| Общедоступный IP-адрес Azure | Azure-GatewayPublicIP |  | Определяется в момент создания |
| Общедоступный IP-адрес Azure Stack | AzureStack-GatewayPublicIP |  | Определяется в момент создания |
| Шлюз локальной сети Azure | AzureStack-S2SGateway<br>   10.100.100.0/23 |  | Значение общедоступного IP-адреса Azure Stack |
| Шлюз локальной сети Azure Stack | Azure-S2SGateway<br>10.100.102.0/23 |  | Значение общедоступного IP-адреса Azure |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Создание виртуальной сети в глобальных Azure и Azure Stack

Чтобы создать виртуальную сеть с помощью портала, сделайте следующее. Если вы используете статью для обучения, примените эти [примеры значений](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values). Но если вы настраиваете по нашим инструкциям рабочую среду, замените эти примеры фактическими значениями.

> [!IMPORTANT]
> Следите за тем, чтобы IP-адреса в адресных пространствах виртуальной сети Azure и (или) Azure Stack не перекрывались.

Чтобы создать виртуальную сеть в Azure, сделайте следующее:

1. В браузере откройте [портал Azure](http://portal.azure.com/) и выполните вход с учетной записью Azure.
2. Выберите **Создать ресурс**. В поле **Поиск в Marketplace** введите `virtual network`. В списке результатов найдите **Виртуальная сеть** и выберите **Виртуальная сеть**.
3. В списке **Выбор модели развертывания** выберите **Resource Manager** и щелкните **Создать**.
4. На странице **Создание виртуальной сети** настройте параметры виртуальной сети. Красной звездочкой здесь отмечены обязательные для заполнения поля.  Когда вы введете в такое поле допустимое значение, звездочка сменится зеленым флажком.

Чтобы создать виртуальную сеть в Azure Stack, сделайте следующее:

* Повторите предыдущие шаги (1–4) на **портале клиента** Azure Stack.

## <a name="add-a-gateway-subnet"></a>Добавление подсети шлюза

Прежде чем подключать шлюз к виртуальной сети, следует создать подсеть шлюза для виртуальной сети, к которой вы хотите его подключить. Службы шлюза используют IP-адреса, указанные для подсети шлюза.

На [портале Azure](http://portal.azure.com/) перейдите к виртуальной сети Resource Manager, в которой вы хотите создать шлюз виртуальной сети.

1. Выберите виртуальную сеть, чтобы открыть страницу **Виртуальная сеть**.
2. В разделе **Параметры** выберите **Подсети**.
3. На странице **Подсети** щелкните **+Gateway subnet** (+Подсеть шлюза). Откроется страница **Добавление подсети**.

    ![Добавление подсети шлюза](media/azure-stack-solution-hybrid-connectivity/image4.png)

4. Поле **Имя** для подсети автоматически заполнится значением GatewaySubnet. По этому имени Azure идентифицирует подсеть как подсеть шлюза.
5. Измените значения **Диапазон адресов** так, чтобы они соответствовали требованиям к конфигурации, а затем нажмите кнопку **ОК**.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Создание шлюза виртуальной сети в Azure и Azure Stack

Чтобы создать виртуальную сеть в Azure, сделайте следующее:

1. На странице портала слева щелкните **+** и введите "шлюз виртуальной сети" в поле поиска.
2. В списке **результатов** выберите **Шлюз виртуальной сети**.
3. На странице **Шлюз виртуальной сети** щелкните **Создать**, чтобы открыть страницу **Создание шлюза виртуальной сети**.
4. На странице **Создание шлюза виртуальной сети** укажите для шлюза сети значения, которые предложены в **примере значений для руководства** и следующие дополнительные значения:

    - **Номер SKU**. Базовый.
    - **Виртуальная сеть**. Выберите созданную ранее виртуальную сеть. Автоматически выберется созданная ранее подсеть шлюза.
    - **Первая IP-конфигурация**. Общедоступный IP-адрес шлюза.
        - Щелкните **Создать IP-конфигурацию шлюза**, чтобы перейти на страницу **Выбор общедоступного IP-адреса**.
        - Выберите **+Создать**, чтобы открыть страницу **Создание общедоступного IP-адреса**.
        - Укажите **имя** для общедоступного IP-адреса. Сохраните для номера SKU значение **Базовый**, а затем нажмите кнопку **ОК**, чтобы сохранить изменения.

       > [!Note]
       > Сейчас VPN-шлюз поддерживает только динамическое выделение общедоступных IP-адресов. Но это не означает, что IP-адрес изменится после того, как он будет назначен VPN-шлюзу. Общедоступный IP-адрес изменяется только после удаления и повторного создания шлюза. IP-адрес VPN-шлюза не изменяется при изменении размера, сбросе или других внутренних операциях обслуживания или обновления.

4. Проверьте параметры шлюза.
5. Выберите **Создать** для создания VPN-шлюза. Будут проверены параметры шлюза и на панели мониторинга появится плитка "Развертывание шлюза виртуальной сети".

   >[!Note]
   >Создание шлюза может занять до 45 минут. Вам понадобится обновить страницу портала, чтобы увидеть готовое состояние.

    После создания шлюза можно узнать назначенный ему IP-адрес, просмотрев виртуальную сеть на портале. Шлюз будет отображаться как подключенное устройство. Чтобы просмотреть дополнительные сведения о шлюзе, выберите устройство.

6. Повторите описанные выше шаги (1–5) для развертывания Azure Stack.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Создание шлюза локальной сети в Azure и Azure Stack

Обычно термин "шлюз локальной сети" означает локальное расположение. Присвойте сайту имя, которое могут использовать Azure и (или) Azure Stack, а затем укажите следующие данные:

- IP-адрес локального VPN-устройства, для которого вы создаете подключение.
- Префиксы IP-адресов, которые будут направляться через VPN-шлюз к VPN-устройству. Указываемые префиксы адресов расположены в локальной сети.

  >[!Note]
  >Если изменятся параметры локальной сети или потребуется изменить общедоступный IP-адрес для VPN-устройства, эти значения можно легко обновить позже.

1. На портале выберите **+ Создать ресурс**.
2. В поле поиска введите **шлюз локальной сети** и нажмите клавишу **ВВОД**. Будет возвращен список результатов.
3. Выберите **Шлюз локальной сети**, а затем щелкните **Создать**, чтобы открыть страницу **Создание шлюза локальной сети**.
4. На странице **Создание шлюза локальной сети** укажите значения для локального сетевого шлюза, представленные в **примере значений для руководства**. Включите следующие дополнительные значения.

    - **IP-адрес** — это общедоступный IP-адрес VPN-устройства, к которому вы хотите подключить Azure или Azure Stack. Укажите допустимый общедоступный IP-адрес, не расположенный за устройством NAT, чтобы обеспечить к нему доступ для Azure. Если у вас еще нет IP-адреса, вы можете временно использовать значения из примера, а позже вернуться к этому разделу и заменить временное значение IP-адреса общедоступным IP-адресом конкретного VPN-устройства. Azure не сможет подключиться к устройству, пока вы не предоставите правильный адрес.
    - **Адресное пространство**. Это диапазон адресов для сети, которую представляет эта локальная сеть. Можно добавить несколько диапазонов пространства адресов. Убедитесь, что указанные диапазоны не перекрывают диапазоны других сетей, к которым вы хотите подключиться. Azure будет маршрутизировать указанный диапазон адресов на IP-адрес локального VPN-устройства. Чтобы подключиться к локальному сайту, используйте здесь значения для локального сайта, а не значения из примера.
    - **Настроить параметры BGP** — используйте только при настройке BGP. В противном случае не выбирайте этот параметр.
    - **Подписка** — убедитесь, что указана правильная подписка.
    - **Группа ресурсов** — выберите нужную группу ресурсов. Вы можете создать новую группу ресурсов или выбрать уже существующую.
    - **Расположение** — укажите расположение, в котором будет создан этот объект. Вы можете выбрать расположение, в котором размещена виртуальная сеть, но это не обязательно.
5. Завершив ввод необходимых значений, выберите **Создать** для создания шлюза локальной сети.
6. Повторите эти шаги (1–5) для развертывания Azure Stack.

## <a name="configure-your-connection"></a>Настройка подключения

Для подключения типа "сеть — сеть" к локальной сети требуется VPN-устройство. Настроенное VPN-устройство здесь обозначается как "Подключение". Для настройки подключения вам потребуется следующее:

- Общий ключ. Это тот же общий ключ, который указывается при создании VPN-подключения "сеть — сеть". В наших примерах мы используем простые общие ключи. Для практического использования рекомендуется создавать более сложные ключи.
- Общедоступный IP-адрес шлюза виртуальной сети. Общедоступный IP-адрес можно просмотреть с помощью портала Azure, PowerShell или CLI. Чтобы найти общедоступный IP-адрес VPN-шлюза с помощью портала Azure, перейдите к разделу "Шлюзы виртуальной сети" и щелкните имя шлюза.

Используйте следующую процедуру, чтобы создать VPN-подключение "сеть — сеть" между шлюзом виртуальной сети и локальным VPN-устройством.

1. На портале Azure выберите **+Создать ресурс**.
2. Выполните поиск по строке **подключения**.
3. В **списке результатов** выберите **Подключения**.
4. На странице **Подключение** выберите **Создать**.
5. На странице **Создание подключения** настройте следующие параметры:

    - **Тип подключения**. Выберите "сеть — сеть" (IPSec).
    - **Группа ресурсов**. Выберите группу ресурсов для тестирования.
    - **Шлюз виртуальной сети**. Выберите созданный шлюз виртуальной сети.
    - **Шлюз локальной сети**. Выберите созданный шлюз локальной сети.
    - **Имя подключения**. Это поле заполняется автоматически значениями для двух шлюзов.
    - **Общий ключ**. Это значение должно соответствовать ключу, настроенному для локального VPN-устройства. В этом примере используется значение abc123, но вы можете (и мы настоятельно рекомендуем) создать и использовать что-нибудь посложнее. Здесь важно, чтобы заданное значение совпадало со значением, указанным при настройке VPN-устройства.
    - Значения для **подписки**, **группы ресурсов** и **расположения** являются фиксированными.

6. Щелкните **ОК** для создания подключения.

Данные созданного подключения появятся на странице **Подключения** для шлюза виртуальной сети. Состояние изменится с *Unknown* (Неизвестно) на *Подключение*, а затем — *Успешно*.

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения о шаблонах для облака Azure см. в статье [Конструктивные шаблоны облачных решений](https://docs.microsoft.com/azure/architecture/patterns).
