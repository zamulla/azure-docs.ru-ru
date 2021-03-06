---
title: Руководство по интеграции Azure Active Directory с BlueJeans | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: d485c16719c07062249e8d40f0feca9685851834
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043471"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Руководство по интеграции Azure Active Directory с BlueJeans

В этом руководстве описано, как интегрировать BlueJeans с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением BlueJeans обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к BlueJeans.
- Вы можете включить автоматический вход пользователей в BlueJeans (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с BlueJeans, вам потребуется:

- подписка Azure AD;
- подписка BlueJeans с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление BlueJeans из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-bluejeans-from-the-gallery"></a>Добавление BlueJeans из коллекции
Чтобы настроить интеграцию BlueJeans с Azure AD, необходимо добавить BlueJeans из коллекции в список управляемых приложений SaaS.

**Чтобы добавить BlueJeans из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Active Directory][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

4. В поле поиска введите **BlueJeans**.

    ![Создание тестового пользователя Azure AD](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

5. На панели результатов выберите **BlueJeans** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в BlueJeans с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в BlueJeans соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в BlueJeans.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в BlueJeans.

Чтобы настроить и проверить единый вход Azure AD в BlueJeans, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя BlueJeans](#creating-a-bluejeans-test-user)** нужно для того, чтобы в BlueJeans также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении BlueJeans.

**Чтобы настроить единый вход Azure AD в BlueJeans, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **BlueJeans** щелкните **Единый вход**.

    ![Настройка единого входа][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.

    ![Настройка единого входа](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. В разделе **Домены и URL-адреса приложения BlueJeans** выполните следующие действия.

    ![Настройка единого входа](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.BlueJeans.com`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки BlueJeans](https://support.bluejeans.com/contact).

4. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/bluejeans-tutorial/tutorial_general_400.png)

6. В разделе **Настройка BlueJeans** щелкните **Настроить BlueJeans**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода, URL-адрес изменения пароля и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. В другом окне веб-браузера войдите на свой корпоративный веб-сайт **BlueJeans** в качестве администратора.

8. Последовательно щелкните **Admin \> Group Settings \> Security** (Администратор > Параметры группы > Безопасность).

   ![Администратор](./media/bluejeans-tutorial/IC785868.png "Администратор")

9. В разделе **Security** (Безопасность) выполните следующие действия.

   ![Единый вход SAML](./media/bluejeans-tutorial/IC785869.png "Единый вход SAML")

   a. Выберите параметр **SAML Single Sign On**(Единый вход SAML).

   b. Установите флажок **Enable automatic provisioning**(Включить автоматическую подготовку).

10. После этого выполните следующие действия.

    ![Путь к сертификату](./media/bluejeans-tutorial/IC785870.png "Путь к сертификату")

    a. Нажмите кнопку **Choose File**(Выбрать файл) и отправьте скачанный сертификат.

    b. Вставьте **URL-адрес службы единого входа SAML** в текстовое поле **Login URL** (URL-адрес входа).

    c. Вставьте **URL-адрес изменения пароля** в текстовое поле **Password Change URL** (URL-адрес изменения пароля).

    d. Вставьте **URL-адрес выхода** в текстовое поле **Logout URL** (URL-адрес выхода).

11. После этого выполните следующие действия.

    ![Сохранение изменений](./media/bluejeans-tutorial/IC785874.png "Сохранение изменений")

    a. В текстовое поле **User id** (Идентификатор пользователя) введите `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. В текстовое поле **Email** (Электронная почта) введите `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Нажмите кнопку **Сохранить изменения**.

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/bluejeans-tutorial/create_aaduser_01.png)

2. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Создание тестового пользователя Azure AD](./media/bluejeans-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.

    ![Создание тестового пользователя Azure AD](./media/bluejeans-tutorial/create_aaduser_03.png)

4. На странице диалогового окна **Пользователь** выполните следующие действия.

    ![Создание тестового пользователя Azure AD](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.

### <a name="creating-a-bluejeans-test-user"></a>Создание тестового пользователя BlueJeans

Цель этого раздела — создать пользователя с именем Britta Simon в BlueJeans. BlueJeans поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](bluejeans-provisioning-tutorial.md).

**Если необходимо создать пользователя вручную, выполните следующие действия:**

1. Выполните вход на веб-сайт компании **BlueJeans** в качестве администратора.

2. Последовательно выберите пункты **Admin \> Manage Users \> Add User** (Администратор > Управление пользователями > Добавить пользователя).

   ![Администратор](./media/bluejeans-tutorial/IC785877.png "Администратор")

   >[!IMPORTANT]
   >Вкладка **Add User** (Добавить пользователя) доступна, только если на вкладке **Security** (Безопасность) снят флажок **Enable automatic provisioning** (Включить автоматическую подготовку). 

3. В разделе **Add User** (Добавление пользователя) выполните следующие действия.

    ![Добавление пользователя](./media/bluejeans-tutorial/IC785886.png "Добавление пользователя")

    a. Введите в текстовых полях **BlueJeans Username** (Имя пользователя BlueJeans), **Email address** (Адрес электронной почты), **BlueJeans Meeting ID** (Идентификатор собрания BlueJeans), **Moderator Passcode** (Секретный код модератора), **Full Name** (Полное имя) и **Company** (Компания) соответствующие данные действующей учетной записи AAD, которую вы хотите подготовить.

    b. Нажмите кнопку **Add User**(Добавить пользователя).

>[!NOTE]
>Вы можете использовать любые другие средства создания учетной записи пользователя BlueJeans или API, предоставляемые BlueJeans для подготовки учетных записей пользователя AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к BlueJeans.

![Назначение пользователя][200]

**Чтобы назначить пользователя Britta Simon в BlueJeans, сделайте следующее.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201]

2. Из списка приложений выберите **BlueJeans**.

    ![Настройка единого входа](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Когда вы щелкните элемент "BlueJeans" на панели доступа, должна появиться страница входа в приложение BlueJeans.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Руководство по настройке Google Apps для автоматической подготовки пользователей](bluejeans-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
