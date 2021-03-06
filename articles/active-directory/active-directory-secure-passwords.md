---
title: Многоуровневая защита паролей в Azure AD | Документация Майкрософт
description: В этой статье объясняется, как Azure AD применяет надежные пароли и защищает пароли пользователей от киберпреступников.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/28/2017
ms.author: lizross
ms.openlocfilehash: e3e97a5a9b768b3b9d4a36627d28955be1f3d9df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056925"
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a>Многоуровневый подход к безопасности паролей Azure AD

В этой статье приведены рекомендации для пользователей и администраторов по защите учетных записей Azure Active Directory (Azure AD) и Майкрософт.

 > [!NOTE]
 > **Вы здесь потому, что возникают проблемы при входе?** Если это так, [с помощью этих инструкций можно изменить и сбросить пароль](user-help/active-directory-passwords-update-your-own-password.md).
 >
 > Администраторы Azure AD могут сбрасывать пароли пользователей, используя руководство в статье [Сброс пароля пользователя в общедоступной предварительной версии Azure Active Directory](fundamentals/active-directory-users-reset-password-azure-portal.md).
 >

## <a name="password-requirements"></a>Требования к паролю

Для защиты паролей в Azure AD используются следующие распространенные методы:

* настройка требований к длине пароля;
* настройка требований к сложности пароля;
* регулярная и периодическая смена пароля.

Сведения о сбросе пароля в Azure Active Directory см. в статье [Самостоятельный сброс пароля в Azure AD для ИТ-специалистов](user-help/active-directory-passwords-update-your-own-password.md).

## <a name="azure-ad-password-protections"></a>Защита паролей в Azure AD

Azure AD и система учетных записей Майкрософт используют проверенные методы, чтобы обеспечить защиту паролей пользователей и администраторов. Эти методы описаны ниже.

* Динамическая блокировка паролей
* Smart Password Lockout

Сведения об управлении паролями на основе проведенных исследований см. в [рекомендациях для управления паролями](https://aka.ms/passwordguidance).

### <a name="dynamically-banned-passwords"></a>Динамическая блокировка паролей

Azure AD и учетные записи Майкрософт обеспечивают защиту паролей, динамически запрещая все часто используемые пароли. Команда разработчиков защиты идентификации Azure регулярно анализирует списки заблокированных паролей, чтобы запретить пользователям использовать стандартные пароли. Эта служба доступна для всех клиентов Azure AD и службы учетных записей Майкрософт.

Администраторы должны следить за тем, чтобы пользователи создавали пароли, которые содержат уникальное сочетание букв, цифр и символов или слов. Такой подход делает подбор пароля практически невозможным, но помогает пользователям запоминать пароли.

#### <a name="password-breaches"></a>Нарушение безопасности паролей

Корпорация Майкрософт всегда старается идти на шаг впереди киберпреступников.

Команда защиты идентификации Azure AD постоянно анализирует часто используемые пароли. Киберпреступники также применяют подобные методы (такие как создание [радужной таблицы](https://en.wikipedia.org/wiki/Rainbow_table) для взлома хэшей паролей) для подготовки своих атак.

Корпорация Майкрософт постоянно анализирует [утечки данных](https://www.privacyrights.org/data-breaches), чтобы поддерживать динамически обновляемый список паролей, что позволяет блокировать уязвимые пароли, прежде чем они станут потенциальной угрозой для клиентов Azure AD. Дополнительные сведения о нашей текущей деятельности по обеспечению безопасности см. в этом [отчете корпорации Майкрософт](https://www.microsoft.com/security/sir/default.aspx).

### <a name="smart-password-lockout"></a>Smart Password Lockout

Когда Azure AD обнаружит, что потенциальный киберпреступник пытается взломать пароль пользователя, учетная запись пользователя будет автоматически заблокирована с помощью Smart Password Lockout. Azure AD позволяет определить риск, связанный с конкретным сеансом входа. Используя самые актуальные данные безопасности, мы применяем семантику блокировки, позволяющую остановить киберугрозы.

Если пользователь заблокирован в Azure AD, его экран будет выглядеть следующим образом:

  ![Блокировка в Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

Для других учетных записей Майкрософт экран будет выглядеть следующим образом:

  ![Блокировка учетной записи Майкрософт](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Сведения о сбросе пароля в Azure Active Directory см. в статье [Самостоятельный сброс пароля в Azure AD для ИТ-специалистов](user-help/active-directory-passwords-update-your-own-password.md).

  >[!NOTE]
  >Если вы являетесь администратором Azure AD, возможно, вам понадобится использовать [Windows Hello](https://www.microsoft.com/windows/windows-hello), чтобы избежать создания традиционных паролей пользователями.
  >

## <a name="next-steps"></a>Дополнительная информация

* [Как изменить свой пароль](user-help/active-directory-passwords-update-your-own-password.md)
* [Основы управления удостоверениями Azure](fundamentals-identity.md)
* [Приступая к работе с Azure](authentication/howto-sspr-reporting.md)
