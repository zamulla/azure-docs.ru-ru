---
title: 'Пример политики Azure в формате JSON: NSG x для каждой подсети | Документация Майкрософт'
description: Этот пример политики в формате JSON требует, чтобы определенная группа безопасности сети использовалась для каждой виртуальной подсети.
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 10/30/2017
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: c8e711a67422e338830cdcaba6e225921511df04
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601876"
---
# <a name="nsg-x-on-every-subnet"></a>NSG x для каждой подсети

Эта политика требует, чтобы определенная группа безопасности сети использовалась для каждой виртуальной подсети. Вы можете указать идентификатор требуемой группы безопасности сети.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Пример шаблона

[!code-json[main](../../../policy-templates/samples/Network/enforce-nsg-on-subnet/azurepolicy.json "NSG X on every subnet")]

Можно развернуть шаблон с помощью [портала Azure](#deploy-with-the-portal), [PowerShell](#deploy-with-powershell) или [интерфейса командной строки Azure](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Развертывание с помощью портала

[![Развертывание в Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fenforce-nsg-on-subnet%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Развертывание с помощью PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "enforce-nsg-on-subnet" -DisplayName "NSG X on every subnet" -description "This policy enforces a specific NSG on every subnet" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -nsgId <NSG Id> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>Очистка развертывания, выполненного с помощью PowerShell

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Развертывание с помощью интерфейса командной строки Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'enforce-nsg-on-subnet' --display-name 'NSG X on every subnet' --description 'This policy enforces a specific NSG on every subnet' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/enforce-nsg-on-subnet/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "enforce-nsg-on-subnet"
```

### <a name="clean-up-azure-cli-deployment"></a>Очистка развертывания, выполненного с помощью интерфейса командной строки Azure

Выполните следующую команду, чтобы удалить группу ресурсов, виртуальную машину и все связанные с ней ресурсы.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Дополнительная информация

- См. другие [примеры шаблонов для службы "Политика Azure"](../json-samples.md).