---
title: Управление Key Vault в Azure Stack с использованием PowerShell | Документация Майкрософт
description: Узнайте, как управлять Key Vault в Azure Stack с использованием PowerShell
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 22B62A3B-B5A9-4B8C-81C9-DA461838FAE5
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.openlocfilehash: 5e9de401f64a835c286c226bfac88caf5168b96e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075771"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>Управление Key Vault в Azure Stack с помощью PowerShell

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

Управлять Key Vault в Azure Stack можно с помощью PowerShell. Узнайте, как использовать командлеты PowerShell для Key Vault, чтобы:

* создать хранилище ключей;
* сохранить криптографические ключи и секреты, а также управлять ими;
* предоставить пользователям и приложениям разрешения на вызов операций в хранилище.

>[!NOTE]
>Командлеты PowerShell для Key Vault, описанные в этой статье, доступны в пакете SDK для Azure PowerShell.

## <a name="prerequisites"></a>предварительным требованиям

* Необходимо подписаться на предложение, включающее службу Azure Key Vault.
* [Установите PowerShell для Azure Stack](azure-stack-powershell-install.md).
* [Настройка пользовательской среды PowerShell в Azure Stack](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Включение операций с Key Vault для клиентской подписки

Чтобы выполнять любые операции с хранилищем ключей, они должны быть разрешены для клиентской подписки. Чтобы убедиться, что хранилище включено, выполните следующую команду:

```PowerShell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

**Выходные данные**

Если для подписки разрешены операции с хранилищем, в выходных данных в столбце RegistrationState для всех типов ресурсов хранилища ключей будет указано значение Registered.

![Состояние регистрации Key Vault](media/azure-stack-kv-manage-powershell/image1.png)

Если операции с хранилищем не разрешены, вызовите следующую команду, чтобы зарегистрировать службу Key Vault в вашей подписке.

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**Выходные данные**

Если регистрация прошла успешно, возвращается следующий результат:

![Регистрация](media/azure-stack-kv-manage-powershell/image2.png) При вызове команд хранилища ключей могут появляться ошибки, например "Подписка не зарегистрирована для использования пространства имен Microsoft.KeyVault". При появлении такой ошибки убедитесь, что вы [включили поставщик ресурсов Key Vault](#enable-your-tenant-subscription-for-vault-operations), как описано выше.

## <a name="create-a-key-vault"></a>Создайте хранилище ключей.

Чтобы создать хранилище ключей, сначала нужно создать группу ресурсов для размещения всех относящихся к хранилищу ключей ресурсов. Выполните следующую команду, чтобы создать новую группу ресурсов:

```PowerShell
New-AzureRmResourceGroup -Name “VaultRG” -Location local -verbose -Force

```

**Выходные данные**

![Создание группы ресурсов](media/azure-stack-kv-manage-powershell/image3.png)

Теперь воспользуйтесь командой **New-AzureRMKeyVault**, чтобы создать хранилище ключей в ранее созданной группе ресурсов. Эта команда принимает три обязательных параметра: имя группы ресурсов, имя хранилища ключей и географическое расположение.

Чтобы создать хранилище ключей, выполните следующую команду:

```PowerShell
New-AzureRmKeyVault -VaultName “Vault01” -ResourceGroupName “VaultRG” -Location local -verbose
```

**Выходные данные**

![Новое хранилище ключей](media/azure-stack-kv-manage-powershell/image4.png)

В выходных данных команды будут указаны свойства хранилища ключей, которое вы создали. Когда приложение обращается к этому хранилищу, ему нужно использовать свойство **Vault URI**. В этом примере оно имеет значение https://vault01.vault.local.azurestack.external.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Развертывание на основе служб федерации Active Directory (AD FS)

При развертывании AD FS может появиться предупреждение: "Access policy is not set. No user or application has access permission to use this vault" (Политика доступа не установлена. Нет пользователей или приложений с правами на использование этого хранилища). Чтобы устранить эту проблему, создайте для хранилища политику доступа с помощью команды [Set-AzureRmKeyVaultAccessPolicy](azure-stack-kv-manage-powershell.md#authorize-an-application-to-use-a-key-or-secret):

```PowerShell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>Управление ключами и секретами

После создания хранилища выполните инструкции ниже, чтобы создать в хранилище ключи и секреты и управлять ими.

### <a name="create-a-key"></a>Создание ключа

Используйте команду **Add-AzureKeyVaultKey**, чтобы создать или импортировать ключ с программной защитой в хранилище ключей.

```PowerShell
Add-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01” -verbose -Destination Software
```

Параметр **Destination** позволяет указать, что ключ имеет программную защиту. После создания ключа команда выводит сведения об успешном выполнении операции.

**Выходные данные**

![Новый ключ](media/azure-stack-kv-manage-powershell/image5.png)

Теперь вы можете использовать созданный ключ, указывая его URI. Если вы создаете или импортируете ключ, имя которого совпадает с именем существующего ключа, все указанные для нового ключа параметры сохраняются в исходном ключе. Информацию о предыдущей версии ключа можно получить с помощью URI для конкретной версии. Например: 

* адрес https://vault10.vault.local.azurestack.external:443/keys/key01 всегда предоставляет актуальную версию;
* адрес https://vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a позволяет получить конкретную версию.

### <a name="get-a-key"></a>Получение ключа

Используйте команду **Get-AzureKeyVaultKey**, чтобы считать ключ и сведения о нем.

```PowerShell
Get-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01”
```

### <a name="create-a-secret"></a>Создание секрета

Используйте команду **Set-AzureKeyVaultSecret**, чтобы создать или обновить секрет в хранилище. Если секрет еще не существует, создается новый секрет. Если секрет уже существует, создается новая версия этого секрета.

```PowerShell
$secretvalue = ConvertTo-SecureString “User@123” -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01” -SecretValue $secretvalue
```

**Выходные данные**

![Создание секрета](media/azure-stack-kv-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Получение секрета

Используйте команду **Get-AzureKeyVaultSecret**, чтобы считать секрет из хранилища ключей. Эта команда может возвращать все версии секрета или только конкретные версии.

```PowerShell
Get-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01”
```

Создав ключи и секреты, вы можете предоставить внешним приложениям права на их использование.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>Авторизация приложения для использования ключа или секрета

Используйте команду **Set-AzureRmKeyVaultAccessPolicy**, чтобы предоставить приложению доступ к ключу или секрету в хранилище ключей.
В следующем примере хранилище имеет имя *ContosoKeyVault*, а вам нужно разрешить его использование для приложения с идентификатором клиента *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*. Чтобы предоставить такое разрешение, выполните следующую команду. Кроме того, вы можете задать конкретные разрешения для пользователя, приложения или группы безопасности с помощью параметра **PermissionsToKeys**.

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Чтобы авторизовать это же приложение для чтения секретов из хранилища, выполните следующий командлет:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Дополнительная информация

* [Создание виртуальной машины с помощью пароля, хранящегося в хранилище ключей](azure-stack-kv-deploy-vm-with-secret.md).
* [Создание виртуальной машины и добавление сертификатов, полученных из хранилища ключей](azure-stack-kv-push-secret-into-vm.md).
