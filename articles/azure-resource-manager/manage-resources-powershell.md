---
title: Správa prostředků Azure pomocí Azure Powershellu | Dokumentace Microsoftu
description: Ke správě prostředků pomocí Azure Powershellu a Azure Resource Manageru.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 241b820122fe1c82b9a68829db87635745c051d9
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56825154"
---
# <a name="manage-azure-resources-by-using-azure-powershell"></a>Správa prostředků Azure pomocí Azure Powershellu

Zjistěte, jak pomocí Azure Powershellu s [Azure Resource Manageru](resource-group-overview.md) ke správě vašich prostředků Azure. Skupiny prostředků, přečtěte si téma [skupinami prostředků spravovat Azure pomocí Azure Powershellu](./manage-resource-groups-powershell.md).

Další články o správě prostředků:

- [Správa prostředků Azure pomocí webu Azure portal](./manage-resources-portal.md)
- [Správa prostředků Azure pomocí Azure Powershellu](./manage-resources-powershell.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>Nasazení prostředků do existující skupiny prostředků

Můžete nasazujte prostředky Azure přímo pomocí Azure Powershellu nebo nasazovat šablony Resource Manageru k vytvoření prostředků Azure.

### <a name="deploy-a-resource"></a>Nasazení prostředku

Tento skript vytvoří účet úložiště.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
  -Name $storageAccountName `
  -Location $location `
  -SkuName "Standard_LRS"

# Retrieve the context.
$ctx = $storageAccount.Context
```

### <a name="deploy-a-template"></a>Nasazení šablony

Tento skript vytvoří nasazení šablony rychlý start k vytvoření účtu úložiště. Další informace najdete v tématu [rychlý start: Vytváření šablon Azure Resource Manageru pomocí Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell).

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -Location $location
```

Další informace najdete v tématu [nasazení prostředků pomocí šablon Resource Manageru a prostředí Azure PowerShell](./resource-group-template-deploy.md).

## <a name="deploy-a-resource-group-and-resources"></a>Nasazení skupiny prostředků a prostředků

Můžete vytvořit skupinu prostředků a nasazení prostředků do skupiny. Další informace najdete v tématu [vytvořte skupinu prostředků a nasazení prostředků](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>Nasazení prostředků do více předplatných nebo skupinách prostředků

Zpravidla nasazujete, všechny prostředky ve vaší šabloně do jedné skupiny prostředků. Existují ale scénáře, ve které chcete nasadit sadu prostředků společně, ale je umístit do různých skupin prostředků nebo předplatných. Další informace najdete v tématu [prostředků nasazení Azure k několika předplatných nebo skupinách prostředků](./resource-manager-cross-resource-group-deployment.md).

## <a name="delete-resources"></a>Odstranění prostředků

Tento skript ukazuje, jak odstranit účet úložiště.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Remove-AzStorageAccount -ResourceGroupName $resourceGroupName -AccountName $storageAccountName
```

Další informace o tom, jak Azure Resource Manageru orders odstranění prostředků najdete v tématu [odstranění skupiny prostředků Azure Resource Manageru](./resource-group-delete.md).

## <a name="move-resources"></a>Přesunutí prostředků

Tento skript ukazuje, jak odebrat účet úložiště z jedné skupiny prostředků do jiné skupiny prostředků.

```azurepowershell-interactive
$srcResourceGroupName = Read-Host -Prompt "Enter the source Resource Group name"
$destResourceGroupName = Read-Host -Prompt "Enter the destination Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$storageAccount = Get-AzResource -ResourceGroupName $srcResourceGroupName -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $destResourceGroupName -ResourceId $storageAccount.ResourceId
```

Absolvovat kurz, naleznete v tématu [kurzu: Přesunutí prostředků Azure do jiné skupiny prostředků nebo předplatného](./resource-manager-tutorial-move-resources.md). 

Další informace najdete v tématu, které se zabývá [přesunutím prostředků do nové skupiny prostředků nebo předplatného](resource-group-move-resources.md).

## <a name="lock-resources"></a>Uzamčení prostředků

Zamknutí zabrání ostatním uživatelům ve vaší organizaci omylem odstranit nebo upravit důležité prostředky, jako je předplatné Azure, skupinu prostředků nebo prostředek. 

Následující skript zamkne účet úložiště, aby účet nelze odstranit.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

New-AzResourceLock -LockName LockStorage -LockLevel CanNotDelete -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts 
```

Následující skript načte všech zámků pro účet úložiště:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts
```

Následující skript odstraní zámek účtu úložiště:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$lockId = (Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts).LockId
Remove-AzResourceLock -LockId $lockId
```

Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Označení prostředků

Označování pomáhá logicky uspořádání skupinu prostředků a prostředky. Informace najdete v tématu [použití značek k uspořádání prostředků Azure](./resource-group-using-tags.md#powershell).

## <a name="manage-access-to-resources"></a>Správa přístupu k prostředkům

[Řízení přístupu na základě role (RBAC)](../role-based-access-control/overview.md) je způsob správy přístupu k prostředkům v Azure. Další informace najdete v tématu [správě přístupu pomocí RBAC a prostředí Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).

## <a name="next-steps"></a>Další postup

- Další Azure Resource Manageru najdete v tématu [přehled Azure Resource Manageru](./resource-group-overview.md).
- Seznamte se se syntaxí šablony Resource Manageru, najdete v článku [Princip struktury a syntaxe šablon Azure Resource Manageru](./resource-group-authoring-templates.md).
- Zjistěte, jak vyvíjet šablony, najdete v článku [podrobné kurzy](/azure/azure-resource-manager/).
- Schémata šablon Azure Resource Manageru najdete v tématu [referenčními informacemi k šablonám](/azure/templates/).