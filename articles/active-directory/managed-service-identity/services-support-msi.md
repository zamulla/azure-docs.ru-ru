---
title: Службы Azure, поддерживающие управляемое удостоверение службы
description: Список служб, которые поддерживают управляемое удостоверения службы и аутентификацию Azure AD.
services: active-directory
author: daveba
ms.author: daveba
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: cdef4cf16fc688b61f367a77267be821ee84bf56
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902016"
---
# <a name="services-that-support-managed-service-identity"></a>Службы, поддерживающие управляемое удостоверение службы 

Управляемое удостоверение службы предоставляет службы Azure с автоматически управляемыми удостоверениями в Azure Active Directory. Управляемое удостоверение можно использовать для аутентификации в любой службе, которая поддерживает аутентификацию Azure AD, не храня учетные данные в коде. Мы работаем над интеграцией MSI и аутентификации Azure AD на платформе Azure. Почаще проверяйте наличие обновлений.

## <a name="azure-services-that-support-managed-service-identity"></a>Службы Azure, поддерживающие управляемое удостоверение службы

Ниже приведены службы Azure, которые поддерживают управляемое удостоверение службы.

| Service | Status | Дата | Настройка | Получение маркера |
| ------- | ------ | ---- | --------- | ----------- |
| Виртуальные машины Azure | Предварительный просмотр | Сентябрь 2017 г. | [портал Azure](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[интерфейс командной строки Azure](qs-configure-cli-windows-vm.md)<br>[Шаблоны диспетчера ресурсов Azure](qs-configure-template-windows-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[GO](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Служба приложений Azure | Доступна | Июнь 2018 г. | [портал Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[интерфейс командной строки Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Шаблон Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell)  |
| Функции Azure | Доступна | Июнь 2018 г. | [портал Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[интерфейс командной строки Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Шаблон Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell) |
| Фабрика данных Azure версии 2 | Предварительный просмотр | Ноябрь 2017 г. | [портал Azure](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[Пакет SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Службы Azure, поддерживающие аутентификацию Azure AD

Ниже приведены службы, поддерживающие аутентификацию Azure AD, которые были проверены с помощью служб клиента, использующих управляемое удостоверение службы.

| Service | Идентификатор ресурса | Status | Дата | Назначение доступа |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Доступна | Сентябрь 2017 г. | [портал Azure](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[интерфейс командной строки Azure](howto-assign-access-CLI.md) |
| Хранилище ключей Azure | https://vault.azure.net | Доступна | Сентябрь 2017 г. | |
| Azure Data Lake; | https://datalake.azure.net/ | Доступна | Сентябрь 2017 г. | |
| Azure SQL | https://database.windows.net/ | Доступна | Октябрь 2017 г. | |
| Концентраторы событий Azure | https://eventhubs.azure.net | Доступна | Декабрь 2017 г. | |
| Azure Service Bus | https://servicebus.azure.net | Доступна | Декабрь 2017 г. | |
| Хранилище Azure | https://storage.azure.com/ | Предварительный просмотр | Май 2018 г. | |
