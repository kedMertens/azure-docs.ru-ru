---
title: Руководство по интеграции Azure Active Directory со SkyDesk Email | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в SkyDesk Email.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 058aad72ea8e5741bc632b3c27c032613683ae78
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444088"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Учебник. Интеграция Azure Active Directory со SkyDesk Email

В этом руководстве описано, как интегрировать SkyDesk Email с Azure Active Directory (Azure AD).

Интеграция SkyDesk Email с Azure AD дает приведенные ниже преимущества.

- С помощью Azure AD вы можете контролировать доступ к SkyDesk Email.
- Вы можете включить автоматический вход пользователей в SkyDesk Email (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD со SkyDesk Email, вам потребуется:

- подписка Azure AD;
- подписка SkyDesk Email с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление SkyDesk Email из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-skydesk-email-from-the-gallery"></a>Добавление SkyDesk Email из коллекции
Чтобы настроить интеграцию SkyDesk Email с Azure AD, необходимо добавить приложение SkyDesk Email из коллекции в список управляемых приложений SaaS.

**Чтобы добавить SkyDesk Email из коллекции, выполните указанные ниже действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **SkyDesk Email**.

    ![Создание тестового пользователя Azure AD](./media/skydeskemail-tutorial/tutorial_skydeskemail_search.png)

1. На панели результатов выберите **SkyDesk Email** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в SkyDesk Email с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходима информация о том, какой пользователь в SkyDesk Email соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SkyDesk Email.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в SkyDesk Email.

Чтобы настроить и проверить единый вход в Azure AD в SkyDesk Email, вам потребуется выполнить действия в приведенных ниже стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя SkyDesk Email](#creating-a-skydesk-email-test-user)** требуется для того, чтобы в SkyDesk Email существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении SkyDesk Email.

**Чтобы настроить единый вход в Azure AD в SkyDesk Email, выполните указанные ниже действия.**

1. На портале Azure на странице интеграции с приложением **SkyDesk Email** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

1. В разделе **Домены и URL-адреса приложения SkyDesk Email** выполните следующие действия.

    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь к [группе поддержки клиентов SkyDesk Email](https://www.skydesk.sg/support/). 
 
1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_general_400.png)

1. В разделе **Конфигурация SkyDesk Email** щелкните **Настроить SkyDesk Email**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода и URL-адрес службы единого входа SAML** из раздела **Quick Reference** (Краткий справочник).

    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

1. Чтобы включить единый вход в **SkyDesk Email**, выполните указанные ниже действия.

    a. Войдите в учетную запись SkyDesk Email от имени администратора.

    b. В меню в верхней части страницы щелкните **Setup** (Настройка) и выберите **Org** (Организация). 
    
      ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. На панели слева выберите **Domains** (Домены).
    
      ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Щелкните **Add Domain** (Добавить домен).
    
      ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    д. Введите доменное имя, а затем проверьте домен.
    
      ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    Е. На панели слева выберите пункт **SAML Authentication** (Аутентификация SAML).
    
      ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. На странице **SAML Authentication** выполните указанные ниже действия.
   
      ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >Для использования проверки подлинности на основе SAML у вас должен быть настроен **проверенный домен** или **URL-адрес портала**. Для URL-адреса портала можно настроить уникальное имя.
    > 
    > 
   
    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. В текстовое поле **Login URL** (URL-адрес входа) вставьте значение **SAML Single Sign-On Service URL** (URL-адрес службы единого входа SAML), скопированное на портале Azure.
   
    b. В текстовое поле **Logout** (Выход) вставьте значение **URL-адрес выхода**, скопированное на портале Azure.

    c. **Изменить URL-адрес пароля** является необязательным, поэтому оставьте его пустым.

    d. Щелкните **Get Key From File** (Получить ключ из файла), чтобы выбрать сертификат, скачанный с портала Azure, затем нажмите кнопку **Open** (Открыть), чтобы передать этот сертификат.

    д. В поле **Алгоритм** задайте значение **RSA**.

    Е. Нажмите кнопку **ОК** , чтобы сохранить изменения.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/skydeskemail-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/skydeskemail-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/skydeskemail-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/skydeskemail-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-skydesk-email-test-user"></a>Создание тестового пользователя SkyDesk Email

В этом разделе описано, как создать пользователя Britta Simon в приложении SkyDesk Email.

1. На левой панели в SkyDesk Email выберите пункт **User Access** (Доступ пользователя), а затем введите свое имя пользователя. 

    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Если вам нужно массово создать пользователей, обратитесь к [группе поддержки клиентов SkyDesk Email](https://www.skydesk.sg/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к SkyDesk Email.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению SkyDesk Email, выполните указанные ниже действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **SkyDesk Email**.

    ![Настройка единого входа](./media/skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент SkyDesk Email на панели доступа, вы автоматически войдете в приложение SkyDesk Email.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/skydeskemail-tutorial/tutorial_general_203.png

