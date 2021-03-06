---
title: Защита ресурсов Azure CDN с помощью аутентификации на основе маркеров | Документация Майкрософт
description: Узнайте, как защитить доступ к ресурсам Azure CDN с помощью аутентификации на основе маркеров.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: ''
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/17/2017
ms.author: mezha
ms.openlocfilehash: aaec713a7680aeda8317f5af41b9b99bcbdca4b7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30905152"
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Защита ресурсов Azure CDN с помощью аутентификации на основе маркеров

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Обзор

Аутентификация на основе маркеров — это механизм, позволяющий предотвратить доступ неавторизованных клиентов к ресурсам сети доставки содержимого Azure (CDN). Обычно аутентификация на основе маркеров применяется во избежание *создания активных ссылок* на содержимое. В этом случае другой сайт (например, доска сообщений) использует ваши ресурсы без разрешения. Создание активных ссылок может повлиять на стоимость доставки содержимого. Если в CDN включена аутентификация на основе маркеров, то перед тем, как CDN доставляет содержимое, запросы аутентифицируются пограничным сервером CDN. 

## <a name="how-it-works"></a>Принцип работы

При аутентификации на основе маркеров проверяется, отправлены ли запросы с доверенного сайта. Для этого запросы должны содержать значение маркера, содержащее закодированные сведения об инициаторе запроса. Содержимое передается инициатору запроса, только если сведения соответствуют требованиям. В противном случае запросы отклоняются. Эти требования можно настроить, используя один или несколько следующих параметров.

- "Страна": разрешение или отклонение запросов, поступающих из стран, указанных по их [коду страны](https://msdn.microsoft.com/library/mt761717.aspx).
- "URL-адрес": разрешение только тех запросов, которые соответствуют указанному ресурсу или пути.
- "Узел". Разрешение или отклонение запросов, в заголовке которых используются указанные узлы.
- "Источник ссылки". Разрешение или отклонение запроса из указанного источника ссылки.
- "IP-адрес". Разрешение только запросов, отправленных с конкретного IP-адреса или IP-подсети.
- "Протокол". Разрешение или отклонение запросов на основе протокола, использованного для запроса содержимого.
- "Время истечения срока действия". Задайте дату и время, чтобы убедиться, что ссылка будет действовать только в течение ограниченного периода времени.

Чтобы узнать больше, ознакомьтесь с подробными примерами конфигурации для каждого параметра в разделе [Настройка аутентификации на основе маркеров](#setting-up-token-authentication).

>[!IMPORTANT] 
> Если для какого-либо пути этой учетной записи включена авторизация по маркеру, для кэширования строки запроса можно использовать только режим standard-cache. Дополнительные сведения см. в статье [Управление режимом кэширования Azure CDN с помощью строк запросов](cdn-query-string-premium.md).

## <a name="reference-architecture"></a>Эталонная архитектура

На следующей схеме рабочего процесса показано, как CDN использует аутентификацию на основе маркеров для работы с веб-приложением.

![Рабочий процесс аутентификации на основе маркеров в CDN](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Логика проверки маркеров в конечной точке CDN
    
На следующей схеме показано, как Azure CDN проверяет запросы клиентов, если для конечной точки CDN настроена аутентификация на основе маркеров.

![Логика проверки маркеров в CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Настройка аутентификации на основе маркеров

1. На [портале Azure](https://portal.azure.com) перейдите к профилю CDN и нажмите кнопку **Управление**, чтобы запустить дополнительный портал.

    ![Кнопка "Управление" для профиля CDN](./media/cdn-token-auth/cdn-manage-btn.png)

2. Наведите указатель мыши на элемент **Большой HTTP-объект** и щелкните **Аутентификация на основе маркеров** во всплывающем элементе. Затем можно настроить ключ шифрования и параметры шифрования следующим образом.

    1. Создайте один или несколько ключей шифрования. В ключе шифрования учитывается регистр, и он может содержать любое сочетание буквенно-цифровых символов. Любые другие типы символов, включая пробелы, не допускаются. Максимальная длина составляет 250 символов. Чтобы обеспечить случайный характер ключей шифрования, рекомендуется создавать их с помощью [инструмента OpenSSL](https://www.openssl.org/). 

       В инструменте OpenSSL используется следующий синтаксис:

       ```rand -hex <key length>```

       Например: 

       ```OpenSSL> rand -hex 32``` 

       Чтобы избежать простоя, создайте основной и резервный ключи. Резервный ключ обеспечивает бесперебойный доступ к содержимому во время обновления первичного ключа.
    
    2. Введите уникальный ключ шифрования в поле **Первичный ключ**. При необходимости введите также резервную копию ключа в поле **Backup Key** (Резервная копия ключа).

    3. Выберите минимальную версию шифрования для каждого ключа в списке **Минимальная версия шифрования**, а затем щелкните **Обновить**.
       - **V2**: указывает, что ключ можно использовать для создания маркеров версии 2.0 и 3.0. Этот режим следует использовать только при переходе с ключа шифрования устаревшей версии 2.0 на ключ версии 3.0.
       - **V3** (рекомендуется): указывает, что ключ можно использовать только для создания маркеров версии 3.0.

    ![Настройка ключей для аутентификации на основе маркеров CDN](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    4. Используйте инструмент шифрования для настройки параметров шифрования и создания маркера. С его помощью можно настроить разрешение или отклонение запросов с учетом времени истечения срока действия, страны, источника ссылки, протокола и IP-адреса клиента (в любых сочетаниях). Хотя количество и сочетание параметров, которые можно объединить для формирования маркера, не ограничено, общая длина маркера не должна превышать 512 символов. 

       ![Инструмент шифрования CDN](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       Введите значения для одного или нескольких указанных ниже параметров шифрования в разделе **Encrypt Tool** (Инструмент шифрования). 

       > [!div class="mx-tdCol2BreakAll"] 
       > <table>
       > <tr>
       >   <th>Имя параметра</th> 
       >   <th>ОПИСАНИЕ</th>
       > </tr>
       > <tr>
       >    <td><b>ec_expire</b></td>
       >    <td>Задает срок действия маркера. Запросы, отправленные после истечения срока действия, отклоняют. Этот параметр использует метку времени Unix, основанную на числе секунд с начала стандартной эпохи Unix, `1/1/1970 00:00:00 GMT`. (Преобразование стандартного времени и времени Unix можно осуществлять с помощью онлайн-инструментов.)> 
       >    Например, если требуется, чтобы маркер действовал до `12/31/2016 12:00:00 GMT`, введите значение метки времени Unix `1483185600`. 
       > </tr>
       > <tr>
       >    <td><b>ec_url_allow</b></td> 
       >    <td>Позволяет настроить маркеры для определенного ресурса или пути. Этот параметр ограничивает доступ к запросам, URL-адрес которых начинается с определенного относительного пути. В URL-адресах учитывается регистр. Введите несколько путей через запятую без пробелов. В зависимости от требований можно настроить разные значения для обеспечения различных уровней доступа.> 
       >    Например, для URL-адреса `http://www.mydomain.com/pictures/city/strasbourg.png` эти запросы допустимы при следующих входных значениях: 
       >    <ul>
       >       <li>Водное значение `/` — все запросы разрешаются.</li>
       >       <li>Водное значение `/pictures` — разрешаются следующие запросы: <ul>
       >          <li>`http://www.mydomain.com/pictures.png`</li>
       >          <li>`http://www.mydomain.com/pictures/city/strasbourg.png`</li>
       >          <li>`http://www.mydomain.com/picturesnew/city/strasbourgh.png`</li>
       >       </ul></li>
       >       <li>Водное значение `/pictures/`: разрешаются только запросы, содержащие путь `/pictures/`. Например, `http://www.mydomain.com/pictures/city/strasbourg.png`.</li>
       >       <li>Входное значение `/pictures/city/strasbourg.png`: разрешаются только запросы для этого конкретного пути и ресурса.</li>
       >    </ul>
       > </tr>
       > <tr>
       >    <td><b>ec_country_allow</b></td> 
       >    <td>Разрешает запросы, отправленные из одной или нескольких указанных стран. Запросы, отправленные из других стран, отклоняются. Укажите двухбуквенный [код каждой страны в формате ISO 3166](https://msdn.microsoft.com/library/mt761717.aspx), разделяя значения запятой. Не добавляйте пробелы. Например, если вы хотите разрешить доступ только из США и Франции, введите в поле `US,FR`.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_country_deny</b></td> 
       >    <td>Отклоняет запросы, отправленные из одной или нескольких указанных стран. Запросы, отправленные из других стран, разрешаются. Реализация аналогична параметру <b>ec_country_allow</b>. Если код страны указан в параметрах <b>ec_country_allow</b> и <b>ec_country_deny</b>, то параметр <b>ec_country_allow</b> имеет больший приоритет.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_ref_allow</b></td>
       >    <td>Разрешает запросы только из указанного источника ссылки. Источник ссылки определяет URL-адрес веб-страницы, связанной с запрашиваемым ресурсом. Значение параметра не должно содержать протокол.>    
       >    Допускаются приведенные ниже типы входных данных.
       >    <ul>
       >       <li>Имя узла или имя узла и путь.</li>
       >       <li>Несколько источников ссылок. Чтобы добавить несколько источников ссылок, укажите их через запятую без пробелов. Если значение источника ссылки указано, но сведения о нем не отправляются в запросе из-за конфигурации браузера, то такие запросы отклоняются по умолчанию.</li> 
       >       <li>Запросы с пустыми или отсутствующими сведениями об источнике ссылки. По умолчанию параметр <b>ec_ref_allow</b> блокирует запросы таких типов. Чтобы разрешить такие запросы, введите текст "missing" либо пустое значение (с запятой в конце).</li> 
       >       <li>Поддомены. Чтобы разрешить поддомены, введите звездочку (\*). Например, чтобы разрешить все поддомены `contoso.com`, введите `*.contoso.com`.</li>
       >    </ul>     
       >    Например, чтобы разрешить доступ для запросов с сайта `www.contoso.com`, всех поддоменов в `contoso2.com` и для запросов с пустым или отсутствующим источником ссылки, введите `www.contoso.com,*.contoso.com,missing`.</td>
       > </tr>
       > <tr> 
       >    <td><b>ec_ref_deny</b></td>
       >    <td>Отклоняет запросы из указанного источника ссылки. Реализация аналогична параметру <b>ec_ref_allow</b>. Если источник ссылки указан в параметрах <b>ec_ref_allow</b> и <b>ec_ref_deny</b>, то параметр <b>ec_ref_allow</b> имеет больший приоритет.</td>
       > </tr>
       > <tr> 
       >    <td><b>ec_proto_allow</b></td> 
       >    <td>Разрешает только запросы, полученные по указанному протоколу. Допустимые значения: `http`, `https` или `http,https`.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_proto_deny</b></td>
       >    <td>Отклоняет запросы, полученные по указанному протоколу. Реализация аналогична параметру <b>ec_proto_allow</b>. Если протокол указан в параметрах <b>ec_proto_allow</b> и <b>ec_proto_deny</b>, то параметр <b>ec_proto_allow</b> имеет больший приоритет.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_clientip</b></td>
       >    <td>Ограничивает доступ по IP-адресу указанного инициатора запроса. Поддерживаются протоколы IPV4 и IPV6. Можно указать отдельный IP-адрес запроса или IP-адреса, связанные с определенной подсетью. Например, `11.22.33.0/22` разрешает запросы с IP-адресов 11.22.32.1–11.22.35.254.</td>
       > </tr>
       > </table>

    5. После ввода значений параметров шифрования выберите ключ для шифрования (если вы создали и первичный, и резервный ключи) из списка **Key To Encrypt** (Ключ шифрования).
    
    6. Выберите версию шифрования из списка **Encryption Version** (Версия шифрования): **V2** для версии 2 или **V3** для версии 3 (рекомендуется). 

    7. Нажмите кнопку **Шифровать** для создания маркера.

    После создания маркер отобразится в поле **Generated Token** (Созданный маркер). Чтобы использовать маркер, присоедините его как строку запроса в конец URL-пути к файлу. Например, `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.
        
    8. При необходимости проверьте маркер с помощью инструмента расшифровки, чтобы просмотреть параметры этого маркера. Вставьте значение маркера в поле **Token to Decrypt** (Маркер для расшифровки). Выберите использование ключа шифрования из списка **Ключ для расшифровки**, а затем нажмите кнопку **Расшифровать**.

    После расшифровки маркера его параметры отобразятся в поле **Original Parameters** (Исходные параметры).

    9. Дополнительно можно настроить тип кода ответа, который возвращается при отклонении запроса. Выберите **Enabled** (Включено), затем выберите код ответа из списка **Response Code** (Код ответа). Параметру **Header Name** (Имя заголовка) автоматически присваивается значение **Location** (Расположение). Нажмите кнопку **Сохранить**, чтобы реализовать новый код ответа. Для некоторых кодов ответа нужно также ввести URL-адрес страницы ошибки в поле **Header Value** (Значение заголовка). Код ответа **403** (Запрещено) выбран по умолчанию. 

3. В разделе **Большой HTTP-объект** щелкните вкладку **Подсистема правил**. С помощью подсистемы правил можно определить путь для применения функции, включить функцию аутентификации на основе маркеров, а также включить дополнительные возможности, связанные с этой функцией. Дополнительные сведения см. в [справочнике по подсистеме правил](cdn-rules-engine-reference.md).

    1. Выберите существующее или создайте новое правило, чтобы определить ресурс или путь, для которого требуется применить аутентификацию на основе маркеров. 
    2. Чтобы включить аутентификацию на основе маркеров в правиле, выберите **[Token Auth](cdn-rules-engine-reference-features.md#token-auth)** (Аутентификация на основе маркеров) из списка **Features** (Функции), а затем выберите **Enabled** (Включено). Щелкните **Обновить**, если вы обновляете правило, или щелкните **Добавить**, если вы создаете его.
        
    ![Пример включения аутентификации на основе маркеров в подсистеме правил CDN](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. В подсистеме правил можно также включить дополнительные функции аутентификации на основе маркеров. Чтобы включить какую-либо из приведенных ниже функций, выберите ее из списка **Features** (Функции), а затем выберите **Enabled** (Включено).
    
    - **[Token Auth Denial Code](cdn-rules-engine-reference-features.md#token-auth-denial-code)** (Код отказа при аутентификации на основе маркеров): определяет тип ответа, возвращаемого пользователю при отклонении запроса. Заданные здесь правила переопределяют код ответа, настроенный в разделе **Custom Denial Handling** (Пользовательская обработка отказов) на странице аутентификации на основе маркеров.

    - **[Token Auth Ignore URL Case](cdn-rules-engine-reference-features.md#token-auth-ignore-url-case)** (Не учитывать регистр при аутентификации на основе маркеров): определяет, учитывается ли регистр в URL-адресе, используемом для проверки маркера.

    - **[Token Auth Parameter](cdn-rules-engine-reference-features.md#token-auth-parameter)** (Параметр аутентификации на основе маркеров): переименовывает параметр строки запроса аутентификации на основе маркеров, отображающийся в запрошенном URL-адресе. 
        
    ![Пример параметров аутентификации на основе маркеров в подсистеме правил CDN](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Можно настроить маркер, воспользовавшись исходным кодом на сайте [GitHub](https://github.com/VerizonDigital/ectoken).
Доступны такие языки:
    
   - C
   - C#
   - PHP
   - Perl;
   - Java
   - Python 

## <a name="azure-cdn-features-and-provider-pricing"></a>Цены поставщиков на возможности Azure CDN

См. дополнительные сведения о [возможностях CDN Azure](cdn-features.md). Сведения о ценах см. на странице [Цены на сеть доставки содержимого](https://azure.microsoft.com/pricing/details/cdn/).
