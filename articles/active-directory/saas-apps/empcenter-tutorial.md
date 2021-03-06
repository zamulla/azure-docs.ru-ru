---
title: Учебник. Интеграция Azure Active Directory с EmpCenter | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в EmpCenter.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 36ff091cc5a6b475482436b7b290398971883db7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225377"
---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Руководство. Интеграция Azure Active Directory с EmpCenter

В этом руководстве описано, как интегрировать EmpCenter с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением EmpCenter обеспечивает перечисленные ниже преимущества.

- С помощью Azure AD вы можете контролировать доступ к EmpCenter.
- Вы можете включить автоматический вход пользователей в EmpCenter (единый вход) с применением учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>предварительным требованиям

Чтобы настроить интеграцию Azure AD с EmpCenter, вам потребуется:

- подписка Azure AD;
- Подписка с поддержкой единого входа EmpCenter

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление EmpCenter из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-empcenter-from-the-gallery"></a>Добавление EmpCenter из коллекции
Чтобы настроить интеграцию EmpCenter с Azure AD, необходимо добавить EmpCenter из коллекции в список управляемых приложений SaaS.

**Чтобы добавить EmpCenter из коллекции, выполните указанные ниже действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

4. В поле поиска введите **EmpCenter**.

    ![Создание тестового пользователя Azure AD](./media/empcenter-tutorial/tutorial_EmpCenter_search.png)

5. На панели результатов выберите **EmpCenter** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/empcenter-tutorial/tutorial_EmpCenter_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в EmpCenter с помощью тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в EmpCenter соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в EmpCenter.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в EmpCenter.

Чтобы настроить и проверить единый вход Azure AD в EmpCenter, вам потребуется выполнить действия в перечисленных ниже стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя EmpCenter](#creating-an-empcenter-test-user)** требуется для создания пользователя Britta Simon в EmpCenter, связанного с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении EmpCenter.

**Чтобы настроить единый вход Azure AD в EmpCenter, выполните указанные ниже действия.**

1. На портале Azure на странице интеграции с приложением **EmpCenter** щелкните **Единый вход**.

    ![Настройка единого входа][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/empcenter-tutorial/tutorial_EmpCenter_samlbase.png)

3. В разделе **Домены и URL-адреса приложения EmpCenter** выполните указанные ниже действия.

    ![Настройка единого входа](./media/empcenter-tutorial/tutorial_EmpCenter_url.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате:
    | |
    |--|
    | `https://<subdomain>.EmpCenter.com/<instancename>` |
    | `https://<subdomain>.workforcehosting.com/<instancename>` |

    > [!NOTE] 
    > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов EmpCenter](http://www.workforcesoftware.com/services/customer-support/). 
 
4. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/empcenter-tutorial/tutorial_EmpCenter_certificate.png) 

5. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/empcenter-tutorial/tutorial_general_400.png)

6. Чтобы настроить единый вход на стороне **EmpCenter**, отправьте в [службу поддержки EmpCenter](http://www.workforcesoftware.com/services/customer-support/) скачанный **XML-файл метаданных**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/empcenter-tutorial/create_aaduser_01.png) 

2. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/empcenter-tutorial/create_aaduser_02.png) 

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/empcenter-tutorial/create_aaduser_03.png) 

4. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/empcenter-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    Б. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-an-empcenter-test-user"></a>Создание тестового пользователя EmpCenter

Чтобы пользователи Azure AD могли выполнять вход в EmpCenter, они должны быть подготовлены в EmpCenter. В случае EmpCenter учетные записи пользователей должны быть созданы [службой поддержки EmpCenter](http://www.workforcesoftware.com/services/customer-support/).

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя EmpCenter или API, предоставляемые EmpCenter для подготовки учетных записей пользователя Azure Active Directory.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к EmpCenter.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению EmpCenter, выполните указанные ниже действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **EmpCenter**.

    ![Настройка единого входа](./media/empcenter-tutorial/tutorial_EmpCenter_app.png) 

3. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа


Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку EmpCenter на панели доступа, вы автоматически войдете в приложение EmpCenter.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/EmpCenter-tutorial/tutorial_general_01.png
[2]: ./media/EmpCenter-tutorial/tutorial_general_02.png
[3]: ./media/EmpCenter-tutorial/tutorial_general_03.png
[4]: ./media/EmpCenter-tutorial/tutorial_general_04.png

[100]: ./media/EmpCenter-tutorial/tutorial_general_100.png

[200]: ./media/EmpCenter-tutorial/tutorial_general_200.png
[201]: ./media/EmpCenter-tutorial/tutorial_general_201.png
[202]: ./media/EmpCenter-tutorial/tutorial_general_202.png
[203]: ./media/EmpCenter-tutorial/tutorial_general_203.png

