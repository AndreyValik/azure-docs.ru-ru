---
title: Учебник. Интеграция Azure Active Directory с Cisco Spark | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Cisco Spark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: bebc8d674d7448ea0ce6a1f11b7ae80335df9cdc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431704"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Учебник. Интеграция Azure Active Directory с Cisco Spark

В этом учебнике описано, как интегрировать Cisco Spark с Azure Active Directory (Azure AD).

Интеграция Azure AD с Cisco Spark обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Cisco Spark.
- Вы можете включить автоматический вход пользователей в Cisco Spark (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Cisco Spark, вам потребуется:

- подписка Azure AD;
- подписка Cisco Spark с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Cisco Spark из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-cisco-spark-from-the-gallery"></a>Добавление Cisco Spark из коллекции
Чтобы настроить интеграцию Cisco Spark с Azure AD, необходимо добавить Cisco Spark из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Cisco Spark из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Cisco Spark**.

    ![Создание тестового пользователя Azure AD](./media/cisco-spark-tutorial/tutorial_ciscospark_search.png)

1. На панели результатов выберите **Cisco Spark** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Cisco Spark с использованием тестового пользователя Britta Simon.

Для работы единого входа Azure AD необходимо знать, какой пользователь в Cisco Spark соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Cisco Spark.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Cisco Spark.

Чтобы настроить и проверить единый вход Azure AD в Cisco Spark, потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Cisco Spark](#creating-a-cisco-spark-test-user)** нужно для того, чтобы в Cisco Spark также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Cisco Spark.

**Чтобы настроить единый вход Azure AD в Cisco Spark, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Cisco Spark** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

1. В разделе **Домены и URL-адреса приложения Cisco Spark** выполните следующие действия:

    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_ciscospark_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в формате `https://web.ciscospark.com/#/signin`.

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://idbroker.webex.com/<companyname>`

    > [!NOTE] 
    > Это значение приведено для справки. Вместо него нужно указать фактический идентификатор. Для получения этого значения обратитесь в [службу поддержки клиентов Cisco Spark](https://support.ciscospark.com/). 
 
1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

1. Приложение Cisco Spark ожидает, что утверждения SAML должны содержать определенные атрибуты. Настройте приведенные ниже атрибуты для этого приложения. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На следующем снимке экрана приведен пример.
    
    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_ciscospark_07.png) 

1. В разделе **Атрибуты пользователя** диалогового окна **Единый вход** настройте атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия:
    
    | Имя атрибута  | Значение атрибута |
    | --------------- | -------------------- |    
    |   uid    | user.userprincipalname |   

    a. Щелкните **Добавить атрибут**, чтобы открыть диалоговое окно **Добавление атрибута**.

    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_attribute_05.png)
    
    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.
    
    c. В списке **Значение** выберите значение атрибута, отображаемое для этой строки.
    
    d. Нажмите кнопку **ОК**.

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_general_400.png)

1. Войдите в [службу Cisco для управления совместной работой в облаке](https://admin.ciscospark.com/), указав учетные данные администратора с полными правами.

1. Выберите **Settings** (Параметры) и в разделе **Authentication** (Аутентификация) щелкните **Modify** (Изменить).
   
    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
1. Выберите **Integrate a 3rd-party identity provider. (Advanced)** (Интеграция стороннего поставщика удостоверений. (Дополнительно)) и перейдите к следующему экрану.

1. На странице **Import Idp Metadata** (Импорт метаданных IdP) либо перетащите файл метаданных Azure AD на страницу, либо воспользуйтесь обозревателем файлов, чтобы найти и передать файл метаданных Azure AD. Затем выберите **Require certificate signed by a certificate authority in Metadata (more secure)** (Требовать наличия в метаданных сертификата, подписанного центром сертификации) и нажмите кнопку **Next** (Далее). 
    
    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_cisco_spark_11.png)

1. Выберите **Test SSO Connection** (Проверить подключение для единого входа) и, когда откроется новая вкладка браузера, пройдите аутентификацию Azure AD, выполнив вход.

1. Вернитесь к на вкладку браузера **Служба Cisco для управления совместной работой в облаке**. Если проверка прошла успешно, выберите **This test was successful. Enable Single Sign-On option** (Этот тест прошел успешно. Включить единый вход) и нажмите кнопку **Next** (Далее).

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/cisco-spark-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/cisco-spark-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/cisco-spark-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/cisco-spark-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-cisco-spark-test-user"></a>Создание тестового пользователя Cisco Spark

В этом разделе описано, как создать пользователя Britta Simon в Cisco Spark. В этом разделе описано, как создать пользователя Britta Simon в Cisco Spark.

1. Войдите в [службу Cisco для управления совместной работой в облаке](https://admin.ciscospark.com/), указав учетные данные администратора с полными правами.

1. Щелкните **Пользователи** и **Управление пользователями**.
   
    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

1. В окне **Manage User** (Управление пользователем) выберите **Manually add or modify users** (Добавить или изменить пользователей вручную) и нажмите кнопку **Next** (Далее).

1. Выберите **Names and Email address** (Имя, фамилия и электронный адрес). Затем заполните текстовые поля следующим образом.
   
    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    a. В текстовом поле **Имя** введите **Britta**. 
    
    b. В текстовом поле **Фамилия** введите **Simon**.
    
    c. В текстовом поле **Email address** (Электронный адрес) введите **britta.simon@contoso.com**.

1. Щелкните знак "плюс", чтобы добавить пользователя Britta Simon. Затем щелкните **Далее**.

1. В окне **Add Services for Users** (Добавление служб для пользователей) нажмите кнопку **Save** (Сохранить), затем — кнопку **Finish** (Готово).

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как позволить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Cisco Spark.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Cisco Spark, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **Cisco Spark**.

    ![Настройка единого входа](./media/cisco-spark-tutorial/tutorial_ciscospark_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "Cisco Spark" на панели доступа, вы автоматически войдете в приложение Cisco Spark.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/cisco-spark-tutorial/tutorial_general_203.png

