---
title: Руководство. Интеграция Azure Active Directory с Ceridian Dayforce HCM | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Ceridian Dayforce HCM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 6577955b275adfda3f0cfafe99a8f95efd16403c
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714586"
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Учебник. Интеграция Azure Active Directory с Ceridian Dayforce HCM

В этом учебнике описано, как интегрировать Ceridian Dayforce HCM с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Ceridian Dayforce HCM обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Ceridian Dayforce HCM.
- Вы можете включить автоматический вход пользователей в Ceridian Dayforce HCM (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Ceridian Dayforce HCM, вам потребуется:

- подписка Azure AD;
- подписка Ceridian Dayforce HCM с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Ceridian Dayforce HCM из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Добавление Ceridian Dayforce HCM из коллекции
Чтобы настроить интеграцию Ceridian Dayforce HCM с Azure AD, необходимо добавить Ceridian Dayforce HCM из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Ceridian Dayforce HCM из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

1. В поле поиска введите **Ceridian Dayforce HCM**, на панели результатов выберите **Ceridian Dayforce HCM**, а затем нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Ceridian Dayforce HCM в списке результатов](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Ceridian Dayforce HCM с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Ceridian Dayforce HCM соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Ceridian Dayforce HCM.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Ceridian Dayforce HCM.

Чтобы настроить и проверить единый вход Azure AD в Ceridian Dayforce HCM, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Ceridian Dayforce HCM](#create-a-ceridian-dayforce-hcm-test-user)** требуется для создания пользователя Britta Simon в Ceridian Dayforce HCM, связанного с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Ceridian Dayforce HCM.

**Чтобы настроить единый вход Azure AD в Ceridian Dayforce HCM, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Ceridian Dayforce HCM** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Ceridian Dayforce HCM** выполните указанные ниже действия.

    ![Настройка единого входа](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    a. В текстовом поле **URL-адрес входа** введите URL-адрес, используемый пользователями для входа в приложение Ceridian Dayforce HCM.
    
    | Среда | URL-адрес |
    | :-- | :-- |
    | Рабочая среда | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | Тестирование | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате:
    
    | Среда | URL-адрес |
    | :-- | :-- |
    | Рабочая среда | `https://ncpingfederate.dayforcehcm.com/sp` |
    | Тестирование | `https://fs-test.dayforcehcm.com/sp` |
    
    c. В текстовом поле **URL-адрес ответа** введите URL-адрес, используемый Azure AD для размещения ответа.
    
    | Среда | URL-адрес |
    | :-- | :-- |
    | Рабочая среда | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | Тестирование | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените их на фактические значения идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь в [службу поддержки Ceridian Dayforce HCM](https://www.ceridian.com/support).

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

1. Приложение Ceridian Dayforce HCM ожидает утверждения SAML в определенном формате. Сначала обратитесь в [службу поддержки Ceridian Dayforce HCM](https://www.ceridian.com/support), чтобы определить правильный идентификатор пользователя. Корпорация Майкрософт рекомендует использовать атрибут **"name"** в качестве идентификатора пользователя. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На следующем снимке экрана приведен пример.  

    ![Настройка единого входа](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

1. В разделе **Атрибуты пользователя** диалогового окна **Единый вход** настройте атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия:
    
    | Имя атрибута  | Значение атрибута |
    | --------------- | -------------------- |    
    | name  | user.extensionattribute2 |    

    a. Щелкните **Добавить атрибут**, чтобы открыть диалоговое окно **Добавление атрибута**.

    ![Настройка единого входа](./media/ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Настройка единого входа](./media/ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. В списке **Значение** выберите атрибут пользователя, который вы хотите использовать в своей реализации.
    Например, если в качестве уникального идентификатора пользователя вы хотите использовать EmployeeID и сохранили значение атрибута в ExtensionAttribute2, выберите **user.extensionattribute2**.
    
    d. Нажмите кнопку **ОК**.

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
1. В разделе **Настройка Ceridian Dayforce HCM** щелкните **Настроить Ceridian Dayforce HCM**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка Ceridian Dayforce HCM](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

1. Чтобы настроить единый вход на стороне **Ceridian Dayforce HCM**, нужно отправить скачанный **XML-файл метаданных**, **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML** в [службу поддержки Ceridian Dayforce HCM](https://www.ceridian.com/support).

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/ceridiandayforcehcm-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/ceridiandayforcehcm-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/ceridiandayforcehcm-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a>Создание тестового пользователя Ceridian Dayforce HCM

Цель этого раздела — создать пользователя с именем Britta Simon в Ceridian Dayforce HCM. Обратитесь в [службу поддержки Ceridian Dayforce HCM](https://www.ceridian.com/support), чтобы добавить пользователей в приложение Ceridian Dayforce HCM. 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Ceridian Dayforce HCM, чтобы он мог использовать единый вход Azure.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Ceridian Dayforce HCM, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Ceridian Dayforce HCM**.

    ![Настройка единого входа](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Ceridian Dayforce HCM, чтобы он мог использовать единый вход Azure.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Ceridian Dayforce HCM, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Ceridian Dayforce HCM**.

    ![Ссылка "Ceridian Dayforce HCM" в списке приложений](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.  
Щелкнув элемент Ceridian Dayforce HCM на панели доступа, вы автоматически войдете в приложение Ceridian Dayforce HCM. 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_203.png

