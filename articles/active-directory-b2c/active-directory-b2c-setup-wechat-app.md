---
title: Настройка WeChat в Azure Active Directory B2C | Документация Майкрософт
description: Обеспечение регистрации и входа для пользователей с учетными записями WeChat в приложениях, защищенных с помощью Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a18d41a4f45b147790a17664156659d282e710d4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445967"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a>Azure Active Directory B2C: организация регистрации и входа для потребителей с учетными записями WeChat

> [!NOTE]
> Эта функция предоставляется в предварительной версии.
> 

## <a name="create-a-wechat-application"></a>Создание приложения WeChat

Чтобы использовать WeChat в качестве поставщика удостоверений в Azure Active Directory (Azure AD) B2C, необходимо сначала создать приложение WeChat и задать в нем правильные параметры. Для этого потребуется учетная запись WeChat. Если у вас ее нет, эту учетную запись можно получить, зарегистрировавшись с помощью одного из мобильных приложений WeChat или своего номера QQ. После этого зарегистрируйте учетную запись в программе для разработчиков WeChat. Дополнительные сведения см. [здесь](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Регистрация приложения WeChat

1. Перейдите на сайт [https://open.weixin.qq.com/](https://open.weixin.qq.com/) и войдите в систему.
2. Щелкните **管理中心** (Центр управления).
3. Выполните отображаемые инструкции, чтобы зарегистрировать новое приложение.
4. В поле **授权回调域** (URL-адрес обратного вызова) введите `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Например если `tenant_name` — contoso.onmicrosoft.com, задайте URL-адрес `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Найдите и скопируйте **идентификатор приложения** и **ключ приложения**. Они вам потребуются позднее.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Настройка WeChat в качестве поставщика удостоверений в клиенте
1. Выполните эти действия, чтобы [перейти к колонке функций B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) на портале Azure.
2. В колонке функций B2C щелкните **Поставщики удостоверений**.
3. Нажмите **+Добавить** в верхней части колонки.
4. Укажите понятное **имя** конфигурации поставщика удостоверений. Например, введите "WeChat".
5. Щелкните **Тип поставщика удостоверений**, выберите **WeChat** и нажмите кнопку **ОК**.
6. Щелкните **Настроить этот поставщик удостоверений**.
7. Введите **ключ приложения**, скопированный ранее, в поле **Идентификатор клиента**.
8. Введите **секрет приложения**, скопированный ранее, в поле **Секрет клиента**.
9. Нажмите кнопку **ОК**, а затем — **Создать**, чтобы сохранить конфигурацию WeChat.

