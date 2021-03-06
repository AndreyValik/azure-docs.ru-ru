---
title: Руководство по интеграции Azure Active Directory с Clear Review | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Clear Review.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8264159a-11a2-4a8c-8285-4efea0adac8c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: jeedes
ms.openlocfilehash: 604a557a91176c08a361ffd058adda63f53b30fc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433026"
---
# <a name="tutorial-azure-active-directory-integration-with-clear-review"></a>Руководство по интеграции Azure Active Directory с Clear Review

В этом руководстве описано, как интегрировать Clear Review с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Clear Review обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Clear Review.
- Вы можете включить автоматический вход пользователей в Clear Review (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Clear Review, вам потребуется:

- подписка Azure AD;
- подписка Clear Review с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Clear Review из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-clear-review-from-the-gallery"></a>Добавление Clear Review из коллекции
Чтобы настроить интеграцию Clear Review с Azure AD, необходимо добавить Clear Review из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Clear Review из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

1. В поле поиска введите **Clear Review**, выберите **Clear Review** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Clear Review в списке результатов](./media/clearreview-tutorial/tutorial_clearreview_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описано, как настроить и проверить единый вход Azure AD в Clear Review с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в Clear Review соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Clear Review.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Clear Review.

Чтобы настроить и проверить единый вход Azure AD в Clear Review, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Clear Review](#create-a-clear-review-test-user)** требуется для того, чтобы в Clear Review существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
1. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Clear Review.

**Чтобы настроить единый вход Azure AD в Clear Review, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Clear Review** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/clearreview-tutorial/tutorial_clearreview_samlbase.png)

1. Чтобы настроить приложение **в режиме, инициированном поставщиком удостоверений**, в разделе **Домены и URL-адреса приложения Clear Review** сделайте следующее:

    ![Сведения о домене и URL-адресах единого входа для приложения Clear Review](./media/clearreview-tutorial/tutorial_clearreview_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<customer name>.clearreview.com/sso/metadata/`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<customer name>.clearreview.com/sso/acs/`.

1. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах единого входа для приложения Clear Review](./media/clearreview-tutorial/tutorial_clearreview_url_sp.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<customer name>.clearreview.com`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Укажите вместо них фактические значения URL-адреса для входа, идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу поддержки Clear Review](https://clearreview.com/contact/).

1. Приложение Clear Review ожидает в утверждении идентификатора имени значение уникального идентификатора пользователя. Необходимо сопоставить значение идентификатора пользователя со значением **user.mail**.

    ![Раздел "Атрибут"](./media/clearreview-tutorial/attribute.png)


1. В разделе **Сертификат для подписи токена SAML** щелкните **Certificate (Base64)** (Сертификат (Base64)), а затем сохраните файл сертификата на компьютере.

    ![Ссылка для скачивания сертификата](./media/clearreview-tutorial/tutorial_clearreview_certificate.png)

1. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/clearreview-tutorial/tutorial_general_400.png)

1. В разделе **Настройка Clear Review** щелкните **Настроить Clear Review**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, идентификатор сущности SAML и URL-адрес службы единого входа SAML**  из раздела **Краткий справочник**.

    ![Настройка Clear Review](./media/clearreview-tutorial/tutorial_clearreview_configure.png) 

1. Чтобы настроить единый вход на стороне **Clear Review** откройте портал **Clear Review** с учетными данными администратора.

1. Выберите **Admin** (Администрирование) в области навигации слева.

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/clearreview-tutorial/tutorial_clearreview_app_admin1.png)

1. В нижней части страницы выберите действие **Change** (Изменить).

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/clearreview-tutorial/tutorial_clearreview_app_admin2.png)

1. На странице **Single Sign-On Settings** (Параметры единого входа) выполните следующие действия.

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/clearreview-tutorial/tutorial_clearreview_app_admin3.png)

    a. В текстовое поле **Issuer URL** (URL-адрес издателя) вставьте значение **идентификатора сущности SAML**, скопированное на портале Azure.

    b. В текстовое поле **SAML Endpoint** (Конечная точка SAML) вставьте значение **URL-адреса службы единого входа SAML**, скопированное на портале Azure.    

    c. В текстовое поле **SLO Endpoint** (Конечная точка единого выхода) вставьте значение **URL-адреса службы единого входа**, скопированное на портале Azure. 

    d. Откройте скачанный сертификат в Блокноте, скопируйте его содержимое и вставьте в текстовое поле **X.509 Certificate** (Сертификат X.509).   

1. Выберите команду **Сохранить**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/clearreview-tutorial/create_aaduser_01.png)

1. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/clearreview-tutorial/create_aaduser_02.png)

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/clearreview-tutorial/create_aaduser_03.png)

1. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/clearreview-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
  
### <a name="create-a-clear-review-test-user"></a>Создание тестового пользователя Clear Review

В этом разделе описано, как создать пользователя Britta Simon в приложении Clear Review. Обратитесь в [службу поддержки Clear Review](https://clearreview.com/contact/), чтобы добавить пользователей на платформу Clear Review.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Clear Review.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Clear Review, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **Clear Review**.

    ![Ссылка на Clear Review в списке приложений](./media/clearreview-tutorial/tutorial_clearreview_app.png)  

1. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Clear Review на панели доступа, вы автоматически войдете в приложение Clear Review.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/clearreview-tutorial/tutorial_general_01.png
[2]: ./media/clearreview-tutorial/tutorial_general_02.png
[3]: ./media/clearreview-tutorial/tutorial_general_03.png
[4]: ./media/clearreview-tutorial/tutorial_general_04.png

[100]: ./media/clearreview-tutorial/tutorial_general_100.png

[200]: ./media/clearreview-tutorial/tutorial_general_200.png
[201]: ./media/clearreview-tutorial/tutorial_general_201.png
[202]: ./media/clearreview-tutorial/tutorial_general_202.png
[203]: ./media/clearreview-tutorial/tutorial_general_203.png
