---
title: Установка новой версии прокси приложения Azure AD | Документация Майкрософт
description: Выберите, какой прокси-сервер лучше всего подходит для обновления Microsoft Forefront или Unified Access Gateway.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: cf1c39d0270dcb7de3ae8ff487b21874b23db74a
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161527"
---
# <a name="compare-remote-access-solutions"></a>Сравнение решений для удаленного доступа

Одним из двух решений для удаленного доступа, которые предоставляет Майкрософт, является прокси-служба приложения Azure Active Directory. Другое решение — прокси-служба веб-приложения (локальная версия). Эти два решения заменяют более ранние продукты Майкрософт — Microsoft Forefront Threat Management Gateway (TMG) и Unified Access Gateway (UAG). В этой статье сравниваются упомянутые четыре решения. Тем, кто до сих пор пользуется устаревшими решениями TMG или UAG, статья поможет спланировать миграцию на один из вариантов прокси-службы приложений. 


## <a name="feature-comparison"></a>Сравнение возможностей

В таблице ниже приведено сравнение продуктов Threat Management Gateway (TMG), Unified Access Gateway (UAG), прокси-службы веб-приложения (WAP) и прокси-службы Azure AD (AP).

| Функция | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Проверка подлинности на основе сертификата | Yes | Yes | - | - |
| Выборочная публикация браузерных приложений | Yes | Yes | Yes | Yes |
| Предварительная аутентификация и единый вход | Yes | Yes | Yes | Yes | 
| Брандмауэр уровня 2/3 | Yes | Yes | - | - |
| Возможности прокси-службы пересылки | Yes | - | - | - |
| Возможности VPN | Yes | Yes | - | - |
| Расширенная поддержка протоколов | - | Yes | Да, при запуске поверх HTTP | Да, при запуске поверх HTTP или через шлюз удаленных рабочих столов |
| Служит прокси-сервером ADFS | - | Yes | Yes | - |
| Единый портал для доступа к приложениям | - | Yes | - | Yes |
| Преобразование ссылки в текст ответа | Yes | Yes | - | Yes | 
| Аутентификация с помощью заголовков | - | Yes | - | Да, с использованием PingAccess | 
| Безопасность в масштабе облака | - | - | - | Yes | 
| Условный доступ | - | Yes | - | Yes |
| Без компонентов в демилитаризованной зоне (DMZ) | - | - | - | Yes |
| Без входящих соединений | - | - | - | Yes |

В большинстве сценариев рекомендуется прокси-служба приложений Azure AD как наиболее современное решение. Прокси-служба веб-приложений предпочтительна только в ситуациях, когда требуется прокси-сервер для AD FS, а задействовать пользовательские домены в Azure Active Directory нет возможности. 

Прокси-служба приложений Azure AD в сравнении с похожими продуктами предоставляет уникальные преимущества, в том числе:

- Расширение Azure AD для использования с локальными ресурсами
   - Безопасность и защита в масштабе облака
   - Простое включение таких функций, как условный доступ и многофакторная проверка подлинности
- Отсутствие компонентов в демилитаризованной зоне
- Отсутствие необходимости во входящих соединениях
- Единая панель доступа для пользователей всех приложений, включая O365, интегрированные с Azure AD приложения SaaS и веб-приложения локальной среды. 


## <a name="next-steps"></a>Дополнительная информация

- [Обеспечение безопасного удаленного доступа к локальным приложениям с помощью службы приложений Azure AD](application-proxy.md)
- [Переход с Forefront TMG и UAG на прокси-службу приложений](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
