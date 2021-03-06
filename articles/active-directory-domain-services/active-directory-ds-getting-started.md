---
title: 'Доменные службы Azure Active Directory: начало работы | Документы Майкрософт'
description: Включение доменных служб Azure Active Directory с помощью портала Azure.
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: maheshu
ms.openlocfilehash: b6651c038a2b3abd15b8b0587e6a0e95832401b1
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502317"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Включение доменных служб Azure Active Directory с помощью портала Azure.
В этой статье показано, как включить доменные службы Azure Active Directory (Azure AD DS) с помощью портала Azure.


## <a name="before-you-begin"></a>Перед началом работы
Для выполнения задач, перечисленных в этой статье, необходимо следующее:

* Действующая **подписка Azure**.
* **Каталог Azure AD** — синхронизированный с локальным каталогом или каталогом только для облака.
* **Подписка Azure, связанная с каталогом Azure AD**.
* Права **глобального администратора** в каталоге Azure AD, которые требуются, чтобы включить доменные службы Azure AD.


## <a name="enable-azure-ad-domain-services"></a>Включение доменных служб Azure AD

Чтобы запустить мастер **включения доменных служб Azure AD**, выполните указанные ниже действия.

1. Перейдите на [портал Azure](https://portal.azure.com).
2. В области слева щелкните **Создать ресурс**.
3. На странице **Создать** введите на панели поиска текст **Доменные службы**.

    ![Поиск доменных служб](./media/getting-started/search-domain-services.png)

4. В списке результатов поиска выберите **Доменные службы Azure AD**. На странице **Доменные службы Azure AD** нажмите кнопку **Создать**.

    ![Представление "Доменные службы"](./media/getting-started/domain-services-blade.png)

5. Запустится мастер **включения доменных служб Azure AD**.


## <a name="task-1-configure-basic-settings"></a>Задача 1. Настройка основных параметров
На странице **Основные сведения** мастера укажите DNS-имя управляемого домена. Вы также можете выбрать группу ресурсов и расположение Azure, в которых должен быть развернут управляемый домен.

![Настройка основных сведений](./media/getting-started/domain-services-blade-basics.png)

1. Выберите **DNS-имя** управляемого домена.

   > [!NOTE]
   > **Рекомендации по выбору доменного имени DNS**
   > * **Встроенное доменное имя.** По умолчанию мастер задает стандартное (встроенное) доменное имя для каталога (с суффиксом **.onmicrosoft.com**). Если вы решили включить защищенный доступ LDAP к управляемому домену через Интернет, могут возникнуть проблемы при создании общедоступной записи DNS или получении сертификата защищенного протокола LDAP от общедоступного ЦС для этого доменного имени. Домен *.onmicrosoft.com* принадлежит корпорации Майкрософт, и ЦС не выдают сертификаты для подтверждения этого домена.
   * **Пользовательские доменные имена.** Можно также ввести пользовательское доменное имя. В этом примере в качестве имени пользовательского домена мы используем *contoso100.com*.
   * **Суффиксы доменов, не поддерживающие маршрутизацию.** Обычно не рекомендуется использовать суффиксы в доменных именах, не поддерживающие маршрутизацию. Например, лучше не создавать домены с доменным именем DNS contoso.local. DNS-суффикс .local не маршрутизируется и может привести к ошибкам, связанными с разрешением DNS.
   * **Ограничения для префикса домена.** Длина указанного префикса доменного имени (например, *contoso100* для доменного имени *contoso100.com*) не должна превышать 15 символов. Невозможно создать управляемый домен с префиксом длиннее 15 символов.
   * **Конфликты имен в сети.** Убедитесь, что выбранное имя домена DNS для управляемого домена не существует в виртуальной сети. В частности, убедитесь в следующем:
       * в виртуальной сети уже существует домен Active Directory с таким же DNS-именем домена;
       * для виртуальной сети, в которой планируется использовать управляемый домен, установлено VPN-подключение к локальной сети; в этом случае необходимо убедиться в том, что в локальной сети нет домена с таким же DNS-именем домена;
       * в виртуальной сети существует облачная служба с таким же именем.
    >

2. Выберите **подписку** Azure, в которой следует создать управляемый домен.

3. Выберите **группу ресурсов**, к которой должен относиться управляемый домен. При выборе группы ресурсов используйте команду **Создать** или **Использовать существующую**.

4. Выберите **расположение** Azure, в котором необходимо создать управляемый домен. На странице **Сеть** мастера приводятся только те виртуальные сети, которые относятся к выбранному расположению.

5. Нажмите кнопку **ОК** для перехода на страницу **Сеть** мастера.


## <a name="next-step"></a>Дальнейшие действия
[Задача 2. Настройка сетевых параметров](active-directory-ds-getting-started-network.md)
