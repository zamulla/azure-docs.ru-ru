---
title: Как блокировать пароли в Azure AD
description: Блокируйте ненадежные пароли в своей среде с помощью динамической блокировки паролей Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 0c1517f94d4a6d59077b62614eec8fef665b1529
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296861"
---
# <a name="configuring-the-custom-banned-password-list"></a>Настройка пользовательского списка заблокированных паролей

|     |
| --- |
| Защита паролем Azure AD и пользовательский список заблокированных паролей — это функции Azure Active Directory, которые предоставляются в режиме общедоступной предварительной версии. См. дополнительные сведения о [дополнительных условиях использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Во многих организациях пользователи создают пароли с характерными распространенными словами, например названием школы, спортивной команды или именем известного человека, из-за чего эти пароли легко угадать. Кроме глобального списка заблокированных паролей, корпорация Майкрософт предлагает пользовательский список заблокированных паролей. Он позволяет организациям добавлять строки для оценки и блокировки, когда пользователи и администраторы пытаются изменить или сбросить пароль.

## <a name="add-to-the-custom-list"></a>Добавление в пользовательский список

Чтобы настроить пользовательский список заблокированных паролей, вам потребуется лицензия Azure Active Directory Premium P1 или P2. Дополнительные сведения о лицензировании Azure Active Directory см. на [странице цен на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

1. Войдите на [портал Azure](https://portal.azure.com) и перейдите к **Azure Active Directory**, **Способы проверки подлинности**, а затем выберите **Защита паролем (предварительная версия)**.
1. Для параметра **Принудительный настраиваемый список** задайте значение **Да**.
1. Добавьте по одной строке на каждую линию в **пользовательский список заблокированных паролей**.
   * Этот список может содержать до 1000 слов.
   * В нем также учитывается регистр.
   * В пользовательском списке заблокированных паролей предусмотрена подстановка стандартных знаков.
      * Пример: o и 0 или а и @.
   * Минимальная длина строки составляет четыре символа, а максимальная — 16 символов.
1. Добавив все строки, нажмите кнопку **Сохранить**.

> [!NOTE]
> Применение изменений для пользовательского списка заблокированных паролей может занять несколько часов.

![Изменение пользовательского списка заблокированных паролей в разделе "Способы проверки подлинности" на портале Azure](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Принцип работы

Каждый раз, когда пользователь или администратор сбрасывает или изменяет пароль в Azure AD, пароль проходит проверку по спискам заблокированных паролей для подтверждения его отсутствия в списке. Эту проверку проходят все пароли, заданные или измененные с помощью Azure AD.

## <a name="what-do-users-see"></a>Что видят пользователи?

Когда пользователь попытается сбросить пароль на такой, который может быть заблокированным, появится следующее сообщение об ошибке:

К сожалению, ваш пароль содержит слово, фразу или сочетание символов, которые легко угадать. Попробуйте ввести другой пароль.

## <a name="next-steps"></a>Дополнительная информация

[Общие сведения о списках запрещенных паролей](concept-password-ban-bad.md)

[Предварительная версия. Применение защиты паролем Azure AD для Windows Server Active Directory](concept-password-ban-bad-on-premises.md)

[Развертывание предварительной версии защиты паролей Azure AD](howto-password-ban-bad-on-premises.md)
