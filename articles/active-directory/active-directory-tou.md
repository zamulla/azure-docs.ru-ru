---
title: Функция "Условия использования Azure Active Directory" | Документация Майкрософт
description: Функция "Условия использования Azure AD" позволяет вам и вашей компании предоставлять условия использования пользователям служб Azure AD.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 06/29/2018
ms.author: rolyon
ms.openlocfilehash: 7833c9da2303d119f0cb421f21bea455ab449898
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856422"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Функция "Условия использования Azure Active Directory"
Функция "Условия использования Azure AD" — это простой способ, которым организации могут предоставлять сведения своим пользователям. Благодаря этой презентации пользователи видят соответствующие заявления об отказе согласно юридическим требованиям и для соответствия стандартам. В этой статье описывается, как приступить к работе с условиями использования в Azure AD.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-can-i-do-with-terms-of-use"></a>Что можно делать с помощью условий использования?
Условия использования в Azure AD позволяют выполнять следующее:
- требовать у сотрудников или гостей принять ваши условия использования, чтобы получить доступ;
- предоставлять общие условия использования для всех пользователей в вашей организации;
- предоставлять определенные условия использования на основе атрибутов пользователя (например, для медсестер и врачей или местных и международных сотрудников, что выполняется с помощью [динамических групп](users-groups-roles/groups-dynamic-membership.md));
- отображать определенные условия использования при доступе к особо важным для бизнеса приложениям, таким как Salesforce;
- отображать условия использования на разных языках;
- составлять список тех, кто принял или не принял условия использования;
- отображать журнал аудита действий с условиями использования.

## <a name="prerequisites"></a>предварительным требованиям
Чтобы настроить условия использования Azure AD и пользоваться ими, необходимо иметь следующее:

- Подписка Azure AD Premium P1, P2, EMS E3 или EMS E5.
    - Если у вас нет одной из этих подписок, вы можете [получить подписку Azure AD Premium](fundamentals/active-directory-get-started-premium.md) или [активировать пробную версию Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Одна из следующих учетных записей администратора для каталога, который нужно настроить.
    - Глобальный администратор.
    - администратор безопасности;
    - Администратор условного доступа

## <a name="terms-of-use-document"></a>Документ "Условия использования"

Для условий использования Azure AD применяется формат PDF. Это может быть любое содержимое, например существующая контрактная документация, позволяющее собрать пользовательские соглашения во время входа пользователя. Рекомендованный размер шрифта в PDF-файле: 24.

## <a name="add-terms-of-use"></a>Добавление условий использования
Когда ваш документ "Условия использования" будет готов, используйте следующую процедуру, чтобы добавить его.

1. Войдите на портал Azure от имени глобального администратора, администратора безопасности или администратора условного доступа.

1. Перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

    ![Колонка "Условия использования"](media/active-directory-tou/tou-blade.png)

1. Щелкните **Добавить условия**.

    ![Добавление условий использования](media/active-directory-tou/new-tou.png)

1. Введите **название** для условий использования.

2. Введите **отображаемое имя**.  Это заголовок, который пользователи видят после входа.

3. **Перейдите** к PDF-файлу с окончательными условиями использования и выберите его.

4. **Выберите** язык для условий использования.  Настроив параметр языка, вы сможете передать несколько экземпляров условий использования на разных языках.  Версия условий использования, отображаемая для пользователя, будет зависеть от настроек браузера.

5. Включите или отключите параметр **Требовать, чтобы пользователи развернули условия использования**.  Если этот параметр включен, пользователи должны будут просмотреть условия использования, прежде чем принять их.

6. В разделе **Условный доступ** вы можете **принудительно применить** отправленные условия использования, выбрав в раскрывающемся списке шаблон или пользовательскую политику условного доступа.  Пользовательские политики условного доступа реализуют детализированные условия использования, вплоть до конкретного облачного приложения или группы пользователей.  Дополнительные сведения см. в статье [Рекомендации по работе с условным доступом в Azure Active Directory](active-directory-conditional-access-best-practices.md).

    >[!IMPORTANT]
    >Элементы управления политиками условного доступа (включая условия использования) не поддерживают принудительное применение в учетных записях служб.  Рекомендуется исключить все учетные записи служб из политики условного доступа.

7. Нажмите кнопку **Создать**.

8. Если выбран пользовательский шаблон условного доступа, откроется новый экран, где можно настроить политику условного доступа.

    Теперь вы увидите ваши новые условия использования.

    ![Добавление условий использования](media/active-directory-tou/create-tou.png)

## <a name="view-who-has-accepted-and-declined"></a>Просмотр тех, кто принял или отклонил Условия использования
В колонке "Условия использования" показано количество пользователей, которые приняли или отклонили условия использования. Имена и количество тех, кто принял или отклонил условия, хранятся в течение жизненного цикла Условий использования.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

    ![Событие аудита](media/active-directory-tou/view-tou.png)

1. Чтобы просмотреть текущее состояние пользователей, щелкните числа возле счетчика пользователей, которые **приняли** или **отклонили** условия.

    ![Событие аудита](media/active-directory-tou/accepted-tou.png)

## <a name="view-audit-logs"></a>Просмотр журналов аудита
Если вы хотите просмотреть дополнительные действия, в Условиях использования Azure AD предусмотрены журналы аудита. Принятие условий каждым пользователем инициирует событие, которое регистрируется в журналах аудита и хранится в течение 30 дней. Эти журналы можно просмотреть на портале или загрузить в виде CSV-файла.

Чтобы приступить к излучению журналов аудита, используйте следующую процедуру.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

1. Нажмите кнопку **Просмотреть журналы аудита**

    ![Событие аудита](media/active-directory-tou/audit-tou.png)

1. На экране "Журналы аудита" Azure AD можно фильтровать информацию с помощью раскрывающихся списков, чтобы получить информацию журнала аудита для конкретного целевого объекта.

    ![Событие аудита](media/active-directory-tou/audit-logs-tou.png)

1. Вы также можете скачать данные в CSV-файл для использования на локальном компьютере, щелкнув **Загрузить**.

## <a name="what-terms-of-use-looks-like-for-users"></a>Как условия использования выглядят для пользователей
После создания и применения условий использования охватываемые во время входа пользователи увидят следующие экраны.

![Событие аудита](media/active-directory-tou/user-tou.png)

На рис. ниже показано, как выглядят условия использования на мобильных устройствах.

![Событие аудита](media/active-directory-tou/mobile-tou.png)

### <a name="how-users-can-review-their-terms-of-use"></a>Каким образом пользователи могут изменить свое решение об условиях использования
Чтобы изменить решение об условиях использования, принятое пользователем, ему необходимо сделать следующее.

1. Войдите на портал [https://myapps.microsoft.com](https://myapps.microsoft.com).

1. В правом верхнем углу щелкните свое имя, а затем в раскрывающемся меню выберите **Профиль**.

    ![Профиль](media/active-directory-tou/tou14.png)

1. На странице профиля щелкните **Ознакомьтесь с условиями использования**.

    ![Событие аудита](media/active-directory-tou/tou13a.png)

1. Здесь можно просмотреть принятые вами условия использования. 

## <a name="delete-terms-of-use"></a>Удаление условий использования
Вы можете удалить старые условия использования, используя следующую процедуру.

1. Войдите в Azure и перейдите к документу **Условия использования** в разделе [https://aka.ms/catou](https://aka.ms/catou).

1. Выберите условия использования, которые вы хотите удалить.

1. Нажмите кнопку **Удалить условия**.

1. В сообщении, которое появится на экране с вопросом о том, нужно ли продолжить, нажмите кнопку **Да**.

    ![Добавление условий использования](media/active-directory-tou/delete-tou.png)

    Ваши новые условия использования больше не должны отображаться.

## <a name="deleted-users-and-active-terms-of-use"></a>Удаленные пользователи и активные условия использования
По умолчанию удаленный пользователь будет оставаться удаленным в Azure AD в течение 30 дней, на протяжении которых его при необходимости может восстановить администратор.  По истечении 30 дней пользователь будет окончательно удален.  Кроме того, с помощью портала Azure Active Directory глобальный администратор может явно [навсегда удалить недавно удаленного пользователя](fundamentals/active-directory-users-restore.md) до истечения этого периода времени.  Если пользователь удален навсегда, впоследствии данные об этом пользователе будут удалены из условий использования Active Directory.  Сведения аудита об удаленных пользователях остаются в журнале аудита.

## <a name="policy-changes"></a>Изменения политики
Политики условного доступа вступают в силу немедленно. В этом случае администратор начнет видеть "грустные облака" или сообщение типа "Проблемы с токеном Azure AD". Администратору необходимо выйти и войти снова для удовлетворения требований новой политики.

>[!IMPORTANT]
> Охватываемым пользователям необходимо выйти и снова войти для удовлетворения требований новой политики, если:
> - политика условного доступа включена для условий использования;
> - созданы другие условия использования.

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

**В. Как узнать, что пользователь принял условия использования и в какое время?**</br>
Ответ. В колонке "Условия использования" щелкните число под надписью **Принято**. Вы также можете просмотреть действие принятия или выполнить его поиск в журналах аудита. Дополнительные сведения см. в разделах [Просмотр тех, кто принял или отклонил Условия использования](#view-who-has-accepted-and-declined) и [Просмотр журналов аудита](#view-audit-logs).

**Вопрос. Если вы изменяете условия использования, необходимо ли пользователям принять их еще раз?**</br>
Ответ. Да, администратор может изменить условия использования. В этом случае пользователям нужно принять новые условия.

**В. Могут ли условия отображаться на нескольких языках?**</br>
Ответ. Да.  Сейчас администратор может настроить для одного набора условий 18 разных языков. 

**В. Когда активируются условия использования?**</br>
О. Условия использования активируются, когда пользователь входит в систему.

**В. Для каких приложений можно применять условия использования?**</br>
О. Вы можете создать политику условного доступа для корпоративных приложений, использующих современную проверку подлинности.  Дополнительные сведения см. в статье [Просмотр всех корпоративных приложений, которыми вы можете управлять в Azure Active Directory](./manage-apps/view-applications-portal.md).

**В. Можно ли добавить несколько экземпляров условий использования для определенного пользователя или приложения?**</br>
О. Да, путем создания нескольких политик условного доступа, предназначенных для этих групп или приложений. Если пользователь подпадает под несколько разных условий использования, он может принять только один экземпляр условий использования одновременно.
 
**В. Что произойдет, если пользователь отклонит условия использования?**</br>
О. Пользователь не сможет получить доступ к приложению. Чтобы получить доступ, ему необходимо повторно войти и принять условия.
 
**Вопрос. Можно ли отказаться от согласия с ранее принятыми условиями использования?**</br>
Ответ. Вы можете [просмотреть ранее принятые условия использования](#how-users-can-review-their-terms-of-use), но отказаться от согласия с ними в настоящее время нельзя.
 
**В. Как долго хранится информация?**</br>
О. Имена и количество пользователей, которые приняли или отклонили условия, хранятся в течение жизненного цикла Условий использования. Журналы аудита хранятся в течение 30 дней.
