---
title: Руководство по интеграции Azure Active Directory с Communifire | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Communifire.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: de2a164d-2115-43e7-a9ed-e54f483f4aeb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: 590d8fe0e974587effc7d8a3c59546b5684b146c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446237"
---
# <a name="tutorial-azure-active-directory-integration-with-communifire"></a>Руководство. Интеграция Azure Active Directory с Communifire

В этом руководстве описано, как интегрировать Communifire с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Communifire обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Communifire.
- Вы можете включить автоматический вход пользователей в Communifire (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением Communifire, вам потребуется:

- подписка Azure AD;
- подписка Communifire с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Communifire из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-communifire-from-the-gallery"></a>Добавление Communifire из коллекции
Чтобы настроить интеграцию Communifire с Azure AD, вам потребуется добавить Communifire из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Communifire из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

1. В поле поиска введите **Communifire**, выберите **Communifire** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Communifire в списке результатов поиска](./media/communifire-tutorial/tutorial_communifire_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Communifire с использованием тестового пользователя Britta Simon.

Чтобы использовать единый вход, служба Azure AD должна иметь информацию о том, какой пользователь в Communifire соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Communifire.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Communifire.

Чтобы настроить и проверить единый вход Azure AD в Communifire, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя приложения Communifire](#create-a-communifire-test-user)** требуется, чтобы в приложении Communifire существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Communifire.

**Чтобы настроить единый вход Azure AD в Communifire, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Communifire** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/communifire-tutorial/tutorial_communifire_samlbase.png)

1. Если вы хотите настроить приложение в режиме, инициированном поставщиком удостоверений, в разделе **Домены и URL-адреса приложения Communifire** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа приложения Communifire](./media/communifire-tutorial/tutorial_communifire_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<subdomain>.communifire.com`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<subdomain>.communifire.com/SAML/AssertionConsumerService.aspx`.

1. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах единого входа приложения Communifire](./media/communifire-tutorial/tutorial_communifire_url1.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<subdomain>.communifire.com/login`
     
    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить их, обратитесь в [службу поддержки клиентов Communifire](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome). 

1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/communifire-tutorial/tutorial_communifire_certificate.png) 

1.  Установите флажок **Показать дополнительные параметры подписывания сертификатов** и выберите для параметра **Вариант подписывания** значение **Ответ знака SAML и утверждение**.

    ![Вариант с сертификатом](./media/communifire-tutorial/tutorial_communifire_certificateoption.png) 

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/communifire-tutorial/tutorial_general_400.png)
    
1. Чтобы настроить единый вход на стороне **Communifire**, отправьте в [службу поддержки Communifire](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome) скачанный **XML-файл метаданных**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/communifire-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/communifire-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/communifire-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/communifire-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-communifire-test-user"></a>Создание тестового пользователя Communifire

Цель этого раздела — создать пользователя с именем Britta Simon в Communifire. Приложение Communifire поддерживает JIT-подготовку. Эта функция включена по умолчанию. Новый пользователь создается путем сохранения сведений о профиле при первой попытке доступа к Communifire, если такой пользователь еще не существует.

>[!Note]
>Если вы хотите создать пользователя вручную, обратитесь к [группе поддержки Communifire](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome).

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Communifire.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Communifire, выполните указанные ниже действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Communifire**.

    ![Ссылка на Communifire в списке "Приложения"](./media/communifire-tutorial/tutorial_communifire_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Communifire на панели доступа, вы автоматически войдете в приложение Communifire.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/communifire-tutorial/tutorial_general_01.png
[2]: ./media/communifire-tutorial/tutorial_general_02.png
[3]: ./media/communifire-tutorial/tutorial_general_03.png
[4]: ./media/communifire-tutorial/tutorial_general_04.png

[100]: ./media/communifire-tutorial/tutorial_general_100.png

[200]: ./media/communifire-tutorial/tutorial_general_200.png
[201]: ./media/communifire-tutorial/tutorial_general_201.png
[202]: ./media/communifire-tutorial/tutorial_general_202.png
[203]: ./media/communifire-tutorial/tutorial_general_203.png

