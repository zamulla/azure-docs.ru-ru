---
title: Выполнение проверок доступа в ресурсах Azure с помощью управления привилегированными пользователями | Документация Майкрософт
description: Сведения о том, как выполнить проверку доступа в компоненте управления привилегированными пользователями для ресурсов Azure
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: f963e63aa92e15205f20ecb3e502065b8dd2c58c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443075"
---
# <a name="perform-access-reviews-in-azure-resources-by-using-privileged-identity-management"></a>Выполнение проверок доступа в ресурсах Azure с помощью управления привилегированными пользователями
Назначения ролей становятся "устаревшими", когда у пользователей имеются права привилегированного доступа, которые им больше не нужны. Чтобы снизить риск, связанный с "устаревшими" назначениями ролей, администраторы привилегированных ролей должны регулярно проверять роли. В этом документе рассматривается процедура запуска проверки доступа в компоненте управления привилегированными пользователями (PIM) для ресурсов Azure.

На главной странице приложения PIM перейдите к:

* Выберите **Проверки доступа** > **Добавить**.

![Добавление проверок доступа](media/azure-pim-resource-rbac/rbac-access-review-home.png)

После того, как вы нажмете кнопку **Добавить**, появится колонка **создания проверки доступа**. В этой колонке можно ввести имя проверки, настроить ее временной интервал, выбрать проверяемую роль, а также решить, кто будет выполнять проверку.

![Создание проверки доступа](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Настройка проверки
Чтобы создать проверку доступа, сначала необходимо присвоить ей имя и указать даты начала и окончания.

![Настройка проверки — снимок экрана](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Укажите продолжительность проверки, достаточную для ее выполнения пользователями. Если проверка завершена до выбранной даты окончания, то ее всегда можно остановить досрочно.

### <a name="choose-a-role-to-review"></a>Выбор проверяемой роли
Каждая проверка фокусируется только на одной роли. Если проверка доступа не запущена из колонки определенной роли, то на данном этапе необходимо выбрать роль.

1. Перейдите в раздел **Проверка членства в роли**.
   
    ![Проверка членства в роли — снимок экрана](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Выберите роль из списка.

### <a name="decide-who-will-perform-the-review"></a>Выбор пользователя, который будет выполнять проверку
Существует три способа выполнения проверки. Можно назначить выполнение проверки другому пользователю, можно сделать это самостоятельно, или же каждый пользователь может проверить свой доступ.

1. Выберите один из способов:
   
   * **Выбор пользователей**: используйте этот параметр, если вы не знаете, кому нужен доступ. С помощью этого параметра можно назначить выполнение проверки владельцу ресурса или руководителю группы.
   * **Назначенные (самостоятельные)**: выберите этот параметр, чтобы пользователи сами проверяли назначенные им роли.
   
2. Перейдите в раздел **Выбор проверяющих**.
   
    ![Выбор проверяющих — снимок экрана](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>Запуск проверки
И, наконец, можно потребовать от пользователя указать причину для утверждения доступа. При желании добавьте описание проверки. Затем щелкните **Запуск**.

Убедитесь, что пользователи знают об ожидающей их проверке доступа, и покажите им, [как выполнить эту проверку](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Управление проверкой доступа
Отслеживать, как проверяющие выполняют свои проверки, можно на панели мониторинга ресурсов Azure PIM. Права доступа в каталоге не изменяются до [завершения проверки](pim-resource-roles-complete-access-review.md).

Пока не истек заданный временной интервал, можно напомнить пользователям о необходимости завершить проверку. Также в разделе "Проверки доступа" можно остановить выполнение проверки досрочно.

