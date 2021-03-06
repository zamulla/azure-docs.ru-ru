---
title: Использование смарт-блокировки Azure AD для предотвращения атак методом подбора
description: Смарт-блокировка Azure Active Directory помогает защитить вашу организацию от атак методом подбора
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: d5beb5ce6e167cd100bec2ed54dc6ea0e78ba37b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035200"
---
# <a name="azure-active-directory-smart-lockout"></a>Смарт-блокировка Azure Active Directory

Смарт-блокировка использует возможности облачной аналитики для блокировки злоумышленников, которые пытаются угадать пароли пользователей или использовать методы подбора пароля для входа. Облачная аналитика позволяет распознавать входные сигналы от действительных пользователей и относиться к ним иначе, нежели к злоумышленникам и другим неизвестным источникам. Механизм смарт-блокировки будет блокировать злоумышленников, пытающихся войти, позволяя обычным пользователям спокойно входить в учетные записи и продолжать работать.

Для всех клиентов Azure AD, у которых установлены настройки по умолчанию, обеспечивающие правильное сочетание безопасности и удобства при использовании, смарт-блокировка всегда включена. Чтобы настроить параметры организации для смарт-блокировки, пользователям требуются лицензии Azure AD Basic или выше.

Смарт-блокировку можно интегрировать в гибридные среды, используя синхронизацию хэшей паролей или сквозную проверку подлинности для защиты локальных учетных записей Active Directory от блокировки злоумышленниками. Если правильно установить политики смарт-блокировки в Azure AD, то прежде, чем атаки смогут достичь локальной среды Active Directory, они будут отфильтрованы.

При использовании [сквозной аутентификации](../connect/active-directory-aadconnect-pass-through-authentication.md) необходимо убедиться, что:

   * Пороговое значение блокировки Azure AD должно быть **меньше**, чем пороговое значение блокировки учетных записей Active Directory. Необходимо, чтобы пороговое значение блокировки учетных записей Active Directory как минимум в два-три раза превышало пороговое значение блокировки Azure AD. 
   * Продолжительность блокировки Azure AD, **в секундах**, должна **быть больше**, чем время до сброса счетчика блокировки учетных записей Active Directory, **в минутах**.

> [!IMPORTANT]
> Сейчас администраторы не могут разблокировать учетные записи пользователей в облаке, если они заблокированы с помощью смарт-блокировки. Администраторы должны подождать, пока не завершится срок действия блокировки.

## <a name="verify-on-premises-account-lockout-policy"></a>Проверка локальных политик блокировки учетных записей

Чтобы проверить локальную политику блокировки учетных записей Active Directory, сделайте следующее:

1. Откройте средства управления групповыми политиками.
2. Измените групповую политику, которая включает в себя политики блокировки учетных записей организации, например **политику домена по умолчанию**.
3. Последовательно выберите **Конфигурация компьютера** > **Политики** > **Параметры Windows** > **Параметры безопасности** > **Политики учетных записей** > **Политика блокировки учетной записи**.
4. Проверьте **пороговое значение блокировки учетных записей** и **время до сброса счетчика блокировки учетных записей**.

![Изменение локальной политики блокировки учетной записи Active Directory с помощью объекта групповой политики](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Управление значениями смарт-блокировки Azure AD

Исходя из требований организации, возможно потребуется настроить значение смарт-блокировки. Чтобы настроить параметры организации для смарт-блокировки, пользователям требуются лицензии Azure AD Basic или выше.

Чтобы проверить или изменить значения смарт-блокировки для организации, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com) и щелкните **Azure Active Directory**, а затем **Методы проверки подлинности**.
1. В зависимости от того, сколько попыток неудачного входа разрешено до первой блокировки учетной записи, установите **пороговое значение блокировки**. По умолчанию задано значение 10.
1. Чтобы указать время для каждой блокировки (в секундах), установите параметр **Длительность блокировки в секундах**.

> [!NOTE]
> Если первый вход после блокировки завершится ошибкой, учетная запись будет снова заблокирована. Если учетная запись будет заблокирована несколько раз, продолжительность блокировки увеличится.

![Настройка политики смарт-блокировки Azure AD на портале Azure](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Дополнительная информация

[Настройка пользовательского списка заблокированных паролей](howto-password-ban-bad.md)

[Быстрое развертывание самостоятельного сброса пароля в Azure AD](quickstart-sspr.md)