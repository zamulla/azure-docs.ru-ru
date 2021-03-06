---
title: 'Доменные службы Azure Active Directory. Устранение неполадок: предупреждения | Документация Майкрософт'
description: Устранение неполадок. Предупреждения для доменных служб Azure AD
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: ergreenl
ms.openlocfilehash: 9eefbdb8acd7dff14817c8358a05ae0f91e5eb11
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218883"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Доменные службы Azure Active Directory. Устранение неполадок: предупреждения
В этой статье содержатся руководства по устранению любых неполадок, возникающих в управляемом домене.


Выберите действия по устранению неполадок, соответствующие идентификатору или тексту оповещения.

| **Идентификатор оповещения** | **Оповещение** | **Способы устранения:** |
| --- | --- | :--- |
| AADDS001 | *Защищенный протокол LDAP через Интернет включен в управляемом домене. Однако доступ к порту 636 не заблокирован с помощью группы безопасности сети. Это может привести к тому, что учетные записи пользователей могут подвергнуться атакам с использованием метода подбора пароля.* | [Неправильная конфигурация протокола LDAP](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Возможно, каталог Azure AD, связанный с управляемым доменом, был удален. Управляемый домен больше не является поддерживаемой конфигурацией. Корпорация Майкрософт не может отслеживать, исправлять и синхронизировать управляемый домен, а также управлять им.* | [Отсутствующий каталог](#aadds100-missing-directory) |
| AADDS101 | *В каталоге Azure AD B2C невозможно включить доменные службы Azure AD* | [В этом каталоге выполняется Azure AD B2C](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Субъект-служба, необходимая для правильной работы доменных служб Azure AD, удалена из каталога Azure AD. Эта конфигурация влияет на возможность корпорации Майкрософт наблюдать за управляемым доменом, управлять им, обновлять и синхронизировать его.* | [Отсутствует субъект-служба](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Диапазон IP-адресов виртуальной сети, в которой включены доменные службы Azure AD, находится в общедоступном диапазоне IP-адресов. Доменные службы Azure AD необходимо включить в виртуальной сети с диапазоном закрытых IP-адресов. Эта конфигурация влияет на возможность корпорации Майкрософт наблюдать за управляемым доменом, управлять им, обновлять и синхронизировать его.* | [Адрес находится в диапазоне общедоступных IP-адресов](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Корпорация Майкрософт не может получить доступ к контроллерам домена этого управляемого домена. Это может произойти, если группа безопасности сети (NSG), настроенная в виртуальной сети, блокирует доступ к управляемому домену. Другая возможная причина заключается в том, что имеется определенный пользователем маршрут, который блокирует входящий трафик из Интернета.* | [Ошибка сети](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *Субъект-служба с идентификатором приложения d87dcbc6-a371-462e-88e3-28ad15ec4e64 был удален и создан снова. Повторное создание позволяет избавиться от несогласованных разрешений для ресурсов доменных служб Azure AD, необходимых для обслуживания управляемого домена. Это может повлиять на синхронизацию паролей для управляемого домена.* | [Приложение синхронизации паролей устарело](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS500 | *Управляемый домен последний раз синхронизировался с Azure AD [дата]. Возможно, пользователям не удастся выполнить вход на управляемый домен или членства в группах могут быть не синхронизированы с Azure AD.* | [Синхронизация не выполнена спустя некоторое время](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Последняя резервная копия управляемого домена создана [дата].* | [Резервная копия не создана спустя некоторое время](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Срок действия защищенного сертификата LDAP для управляемого домена истекает XX.* | [Истечение срока действия безопасного сертификата LDAP](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Управляемый домен блокируется, так как подписка Azure, связанная с доменом, неактивна.* | [Блокировка из-за неактивной подписки](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Управляемый домен блокируется из-за неправильной конфигурации. Служба не могла исправлять и обновлять контроллеры управляемого домена, а также управлять ими в течение долгого времени.* | [Блокировка из-за неправильной конфигурации](#aadds504-suspension-due-to-an-invalid-configuration) |


## <a name="aadds100-missing-directory"></a>AADDS100: отсутствующий каталог
**Оповещение.**

*Возможно, каталог Azure AD, связанный с управляемым доменом, был удален. Управляемый домен больше не является поддерживаемой конфигурацией. Корпорация Майкрософт не может отслеживать, исправлять и синхронизировать управляемый домен, а также управлять им.*

**Способы устранения:**

Эта ошибка обычно вызвана неправильным перемещением подписки Azure в новый каталог Azure AD и удалением старого каталога Azure AD, который все еще связан с доменными службами Azure AD.

Эта ошибка является неисправимой. Чтобы устранить ее, необходимо [удалить имеющийся управляемый домен](active-directory-ds-disable-aadds.md) и повторно создать его в новом каталоге. Если у вас возникли трудности с удалением, свяжитесь с командой разработки продукта, отвечающей за доменные службы Azure Active Directory [для поддержки](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C выполняется в этом каталоге
**Оповещение.**

*В каталоге Azure AD B2C невозможно включить доменные службы Azure AD*

**Способы устранения:**

>[!NOTE]
>Чтобы продолжить использовать доменные службы Azure AD, необходимо повторно создать экземпляр доменных служб Azure AD в каталоге, отличном от Azure AD B2C.

Чтобы восстановить службу, сделайте следующее:

1. [Удалите свой управляемый домен](active-directory-ds-disable-aadds.md) из имеющегося каталога Azure AD.
2. Создайте каталог, который не является каталогом Azure AD B2C.
3. Следуйте шагам, описанным в руководстве [Enable Azure Active Directory Domain Services using the Azure portal](active-directory-ds-getting-started.md) (Включение доменных служб Azure Active Directory с помощью портала Azure), чтобы повторно создать управляемый домен.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: адрес находится в общедоступном диапазоне IP-адресов

**Оповещение.**

*Диапазон IP-адресов виртуальной сети, в которой включены доменные службы Azure AD, находится в общедоступном диапазоне IP-адресов. Доменные службы Azure AD необходимо включить в виртуальной сети с диапазоном закрытых IP-адресов. Эта конфигурация влияет на возможность корпорации Майкрософт наблюдать за управляемым доменом, управлять им, обновлять и синхронизировать его.*

**Способы устранения:**

> [!NOTE]
> Чтобы устранить эту проблему, необходимо удалить имеющийся управляемый домен и повторно создать его в виртуальной сети с закрытым диапазоном IP-адресов. Этот процесс может помешать работе.

Прежде чем начать, ознакомьтесь с разделом об **адресном пространстве закрытых IP-адресов версии 4** [в этой статье](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Внутри виртуальной сети компьютеры могут выполнять запросы к ресурсам Azure, которые находятся в одном диапазоне IP-адресов, настроенном для подсети. Тем не менее, так как для данного диапазона настроена виртуальная сеть, эти запросы будут направлены в нее и не достигнут целевых веб-ресурсов. Эта конфигурация может привести к непредвиденным ошибкам в работе доменных служб Azure AD.

**Если вам принадлежит диапазон IP-адресов в Интернете, настроенный в виртуальной сети, то это оповещение можно игнорировать. Тем не менее доменные службы Azure AD не смогут обеспечить выполнение [соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/) в этой конфигурации, так как она может привести к непредвиденным ошибкам.**


1. [Удалите свой управляемый домен](active-directory-ds-disable-aadds.md) из имеющегося каталога.
2. Исправьте диапазон IP-адресов для подсети
  1. Перейдите [на страницу "Виртуальные сети" на портале Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Выберите виртуальную сеть, которую планируется использовать для доменных служб Azure Active Directory.
  3. В разделе "Параметры" щелкните **Адресное пространство**.
  4. Обновите диапазон адресов, щелкнув соответствующий диапазон адресов и изменив его или добавив дополнительный диапазон адресов. Убедитесь, что новый диапазон адресов находится в закрытом диапазоне IP-адресов. Сохраните изменения.
  5. На панели навигации слева щелкните **Подсети**.
  6. Щелкните подсеть, которую нужно изменить в таблице.
  7. Обновите диапазон адресов и сохраните изменения.
3. Следуйте руководству [Enable Azure Active Directory Domain Services using the Azure portal](active-directory-ds-getting-started.md) (Включение доменных служб Azure Active Directory с помощью портала Azure), чтобы повторно создать управляемый домен. Убедитесь, что выбрана виртуальная сеть с закрытым диапазоном IP-адресов.
4. Чтобы соединить виртуальные машины с доменом следуйте [этому руководству](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Чтобы убедиться, что причина оповещения устранена, проверьте работоспособность домена через два часа.

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: синхронизация не выполнена спустя некоторое время

**Оповещение.**

*Управляемый домен последний раз синхронизировался с Azure AD [дата]. Возможно, пользователям не удастся выполнить вход на управляемый домен или членства в группах могут быть не синхронизированы с Azure AD.*

**Способы устранения:**

[Проверьте работоспособность своего домена](active-directory-ds-check-health.md) для всех предупреждений, которые могут указывать на проблемы в конфигурации управляемого домена. Иногда из-за проблем с конфигурацией корпорации Майкрософт не удается синхронизировать управляемый домен. Если вы можете разрешить предупреждения, подождите два часа и снова проверьте, выполнена ли синхронизация.


## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: резервная копия не создана спустя некоторое время

**Оповещение.**

*Последняя резервная копия управляемого домена создана [дата].*

**Способы устранения:**

[Проверьте работоспособность своего домена](active-directory-ds-check-health.md) для всех предупреждений, которые могут указывать на проблемы в конфигурации управляемого домена. Иногда из-за проблем с конфигурацией корпорации Майкрософт не удается синхронизировать управляемый домен. Если вы можете разрешить предупреждения, подождите два часа и снова проверьте, выполнена ли синхронизация.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: блокировка из-за неактивной подписки

**Оповещение.**

*Управляемый домен блокируется, так как подписка Azure, связанная с доменом, неактивна.*

**Способы устранения:**

Чтобы восстановить службу, [возобновите подписку Azure](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable), связанную с управляемым доменом.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: блокировка из-за неправильной конфигурации

**Оповещение.**

*Управляемый домен блокируется из-за неправильной конфигурации. Служба не могла исправлять и обновлять контроллеры управляемого домена, а также управлять ими в течение долгого времени.*

**Способы устранения:**

[Проверьте работоспособность своего домена](active-directory-ds-check-health.md) для всех предупреждений, которые могут указывать на проблемы в конфигурации управляемого домена. Если вы можете разрешить какое-либо из этих предупреждений, сделайте это. Затем обратитесь в службу поддержки, чтобы снова включить подписку.

## <a name="contact-us"></a>Свяжитесь с нами
Чтобы [оставить отзыв или обратиться за помощью](active-directory-ds-contact-us.md), свяжитесь с командой разработки продукта, отвечающей за доменные службы Azure Active Directory.
