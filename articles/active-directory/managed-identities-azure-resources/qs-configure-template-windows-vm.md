---
title: Postup konfigurace spravovaných identit pro prostředky Azure na Virtuálním počítači Azure pomocí šablony
description: Podrobné pokyny ke konfiguraci spravovaných identit pro prostředky Azure na virtuálním počítači Azure pomocí šablony Azure Resource Manageru.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: priyamo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1abdfc377c40e37f01fbbbbd695e949671d40a51
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820123"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-a-templates"></a>Konfigurace spravovaných identit pro prostředky Azure na Virtuálním počítači Azure pomocí šablony

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Spravované identity pro prostředky Azure poskytuje služby Azure se automaticky spravované identity v Azure Active Directory. Tuto identitu můžete použít k ověření na libovolnou službu, která podporuje ověřování Azure AD, aniž by bylo přihlašovací údaje ve vašem kódu. 

V tomto článku pomocí šablony nasazení Azure Resource Manageru, se dozvíte, jak k provádění následujících spravovaných identit pro prostředky Azure operace na Virtuálním počítači Azure:

## <a name="prerequisites"></a>Požadavky

- Pokud nejste obeznámeni s použitím šablony nasazení Azure Resource Manageru, podívejte se [oddílu přehled](overview.md). **Nezapomeňte si přečíst [rozdíl mezi systém přiřadil a uživatelsky přiřazené identity spravované](overview.md#how-does-it-work)**.
- Pokud ještě nemáte účet Azure, [zaregistrujte si bezplatný účet](https://azure.microsoft.com/free/) před tím, než budete pokračovat.

## <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru

Stejně jako webu Azure portal a skriptování, [Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md) šablony poskytují možnost nasazení nové nebo upravené zdroje, které jsou definované ve skupině prostředků Azure. Několik možností, jak jsou k dispozici pro úpravy šablony a nasazení, místních i založené na portálu, včetně:

   - Použití [vlastní šablonu z Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), která umožňuje vytvoření zcela nové šablony, nebo ji založit na existující běžné nebo [šablonu pro rychlý Start](https://azure.microsoft.com/documentation/templates/).
   - Odvozování z existující skupinu prostředků, tak, že vyexportujete šablonu buď z [původního nasazení](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates), nebo [aktuální stav nasazení](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).
   - Pomocí místní [editor JSON (například VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)a nahrání a nasazení pomocí Powershellu nebo rozhraní příkazového řádku.
   - Pomocí sady Visual Studio [projekt skupiny prostředků Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) jak vytvořit a nasadit šablonu.  

Bez ohledu na vámi zvolené možnosti syntaxe šablony je stejný během počátečního nasazení a opětovné nasazení. Povolení systém nebo uživatel přiřazenou spravovaná identita nového nebo existujícího virtuálního počítače se provádí stejným způsobem. Také ve výchozím nastavení, provede Azure Resource Manageru [přírůstkové aktualizace](../../azure-resource-manager/deployment-modes.md) až po nasazení.

## <a name="system-assigned-managed-identity"></a>Systém přiřadil spravované identity

V této části se povolí a zakáže systém přiřadil spravovanou identitu pomocí šablony Azure Resource Manageru.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Povolit systém přiřadil spravovanou identitu při vytváření virtuálního počítače Azure nebo z existujícího virtuálního počítače

Pokud chcete povolit systém přiřadil spravovaná identita na virtuálním počítači, musí váš účet [Přispěvatel virtuálních počítačů](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) přiřazení role.  Žádné další Azure AD přiřazení rolí adresáře se vyžadují.

1. Ať už místně se přihlaste do Azure nebo prostřednictvím portálu Azure portal pomocí účtu, který je přidružený k předplatnému Azure, která obsahuje virtuální počítač.

2. Pokud chcete povolit systém přiřadil spravovanou identitu, načtení šablony do editoru, vyhledejte `Microsoft.Compute/virtualMachines` prostředků zájmu v rámci `resources` a přidejte `"identity"` vlastnost na stejné úrovni jako `"type": "Microsoft.Compute/virtualMachines"` vlastnost. Použijte následující syntaxi:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   },
   ```

3. (Volitelné) Přidání virtuálních počítačů spravovaných identit pro rozšíření prostředků Azure jako `resources` elementu. Tento krok je volitelný, i identitu koncového bodu Azure Instance Metadata služby (IMDS), můžete použít k získání tokenů také.  Použijte následující syntaxi:

   >[!NOTE] 
   > V následujících příkladech se předpokládá rozšíření virtuálního počítače Windows (`ManagedIdentityExtensionForWindows`) se nasazuje. Můžete také nakonfigurovat pro Linux s použitím `ManagedIdentityExtensionForLinux` namísto toho `"name"` a `"type"` elementy. Rozšíření virtuálního počítače je naplánovaná na vyřazení v lednu 2019.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2018-06-01",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

4. Jakmile budete hotovi, v dalších částech měla být přidána do `resource` část šablony a to by měl vypadat takto:

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
                },
            },
            {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
                }
            }
        }
    ]
   ```

### <a name="assign-a-role-the-vms-system-assigned-managed-identity"></a>Přiřazení role Virtuálního počítače, systém přiřadil spravované identity

Po povolení systém přiřadil spravovaná identita na vašem virtuálním počítači, můžete jí udělit roli, jako **čtečky** přístup do skupiny prostředků, ve kterém byla vytvořena.

Přiřazení role na systém přiřadil identitu Virtuálního počítače, musí váš účet [správce uživatelských přístupů](/azure/role-based-access-control/built-in-roles#user-access-administrator) přiřazení role.

1. Ať už místně se přihlaste do Azure nebo prostřednictvím portálu Azure portal pomocí účtu, který je přidružený k předplatnému Azure, která obsahuje virtuální počítač.
 
2. Načíst šablonu do [editor](#azure-resource-manager-templates) a přidejte následující informace poskytují vašemu virtuálnímu počítači **čtečky** přístup do skupiny prostředků, ve kterém byla vytvořena.  Struktury vaší šablony může lišit v závislosti na editoru a model nasazení, které zvolíte.
   
   V části `parameters` části přidejte následující:

    ```JSON
    "builtInRoleType": {
          "type": "string",
          "defaultValue": "Reader"
        },
        "rbacGuid": {
          "type": "string"
        }
    ```

    V části `variables` části přidejte následující:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    V části `resources` části přidejte následující:

    ```JSON
    {
        "apiVersion": "2017-09-01",
         "type": "Microsoft.Authorization/roleAssignments",
         "name": "[parameters('rbacGuid')]",
         "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
                "scope": "[resourceGroup().id]"
          },
          "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ]
    }
    ```

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-vm"></a>Zakázat systém přiřadil spravovanou identitu z virtuálního počítače Azure

Z virtuálního počítače odebrat systém přiřadil spravovanou identitu, musí váš účet [Přispěvatel virtuálních počítačů](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) přiřazení role.  Žádné další Azure AD přiřazení rolí adresáře se vyžadují.

1. Ať už místně se přihlaste do Azure nebo prostřednictvím portálu Azure portal pomocí účtu, který je přidružený k předplatnému Azure, která obsahuje virtuální počítač.

2. Načíst šablonu do [editor](#azure-resource-manager-templates) a vyhledejte `Microsoft.Compute/virtualMachines` prostředků zájmu v rámci `resources` oddílu. Pokud máte virtuální počítač, který má jenom systém přiřadil spravovanou identitu, můžete jej zakázat tak, že změníte typ identity k `None`.  
   
   **Microsoft.Compute/virtualMachines rozhraní API verze 2018-06-01**

   Pokud váš virtuální počítač má systém a uživatelsky přiřazené identity spravované, odeberte `SystemAssigned` z typ identity a zachovat `UserAssigned` spolu s `userAssignedIdentities` slovník hodnot.

   **Microsoft.Compute/virtualMachines rozhraní API verze 2018-06-01**
   
   Pokud vaše `apiVersion` je `2017-12-01` a váš virtuální počítač má systém i uživatelsky přiřazené identity spravované odebrat `SystemAssigned` z typ identity a zachovat `UserAssigned` spolu s `identityIds` pole uživatel přiřazenou spravovaných identit.  
   
Následující příklad ukazuje, jak odebrat spravovanou identitu systém přiřadil z virtuálního počítače se žádné spravované identity přiřazené uživateli:

```JSON
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[parameters('vmName')]",
    "location": "[resourceGroup().location]",
    "identity": { 
        "type": "None"
}
```

## <a name="user-assigned-managed-identity"></a>Spravovaná identita přiřazená uživateli

V této části přiřadíte spravovanou identitu uživatelsky přiřazené k virtuálnímu počítači Azure pomocí šablony Azure Resource Manageru.

> [!Note]
> Vytvoření uživatelsky přiřazené spravovanou identitu pomocí šablony Azure Resource Manageru najdete v tématu [vytvoření uživatelsky přiřazené identity spravované](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

### <a name="assign-a-user-assigned-managed-identity-to-an-azure-vm"></a>Spravované identity přiřazené uživateli přiřadit Virtuálním počítači Azure

K virtuálnímu počítači přiřadit uživatelsky přiřazené identity, musí váš účet [Přispěvatel virtuálních počítačů](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) a [operátor spravovaných identit](/azure/role-based-access-control/built-in-roles#managed-identity-operator) přiřazení rolí. Žádné další Azure AD přiřazení rolí adresáře se vyžadují.

1. V části `resources` prvku, přidejte následující položku přiřazení spravovanou identitu uživatelsky přiřazené k vašemu virtuálnímu počítači.  Nezapomeňte nahradit `<USERASSIGNEDIDENTITY>` názvem uživatel přiřazenou spravované identity, které jste vytvořili.

   **Microsoft.Compute/virtualMachines rozhraní API verze 2018-06-01**

   Pokud vaše `apiVersion` je `2018-06-01`, uživatelsky přiřazené spravovaných identit jsou uloženy v `userAssignedIdentities` slovníku formátu a `<USERASSIGNEDIDENTITYNAME>` hodnota musí být uložen v proměnné definované v `variables` vaší šablony.

   ```json
   {
       "apiVersion": "2018-06-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachines rozhraní API verze 2017-12-01**
    
   Pokud vaše `apiVersion` je `2017-12-01`, uživatelsky přiřazené spravovaných identit jsou uloženy v `identityIds` pole a `<USERASSIGNEDIDENTITYNAME>` hodnota musí být uložen v proměnné definované v `variables` vaší šablony.
    
   ```json
   {
       "apiVersion": "2017-12-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
           ]
       }
   }
   ```
       

2. (Volitelné) Části `resources` prvku, přidejte následující položku přiřazení spravovanou identitu rozšíření k vašemu virtuálnímu počítači (plánovaná k převedení na zastaralého v lednu 2019). Tento krok je volitelný, i identitu koncového bodu Azure Instance Metadata služby (IMDS), můžete použít k získání tokenů také. Použijte následující syntaxi:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
3. Jakmile budete hotovi, v dalších částech měla být přidána do `resource` část šablony a to by měl vypadat takto:
   
   **Microsoft.Compute/virtualMachines rozhraní API verze 2018-06-01**    

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "userAssignedIdentities": {
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```
   **Microsoft.Compute/virtualMachines rozhraní API verze 2017-12-01**
   
   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "identityIds": [
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Odebrání virtuálního počítače Azure spravované identity přiřazené uživateli

Odebrání virtuálního počítače uživatelsky přiřazené identity, musí váš účet [Přispěvatel virtuálních počítačů](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) přiřazení role. Žádné další Azure AD přiřazení rolí adresáře se vyžadují.

1. Ať už místně se přihlaste do Azure nebo prostřednictvím portálu Azure portal pomocí účtu, který je přidružený k předplatnému Azure, která obsahuje virtuální počítač.

2. Načíst šablonu do [editor](#azure-resource-manager-templates) a vyhledejte `Microsoft.Compute/virtualMachines` prostředků zájmu v rámci `resources` oddílu. Pokud máte virtuální počítač, který má uživatel přiřazenou spravované identity jenom, můžete jej zakázat tak, že změníte typ identity k `None`.
 
   Následující příklad ukazuje, jak odebrat všechny přiřazené uživatele spravovaných identit z virtuálního počítače se žádný systém přiřadil spravovaných identit:
   
   ```json
    {
      "apiVersion": "2018-06-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```
   
   **Microsoft.Compute/virtualMachines rozhraní API verze 2018-06-01**
    
   Z virtuálního počítače odebrat jeden uživatel přiřazenou spravovanou identitu, odeberte ho z `useraAssignedIdentities` slovníku.

   Pokud máte systém přiřadil spravovanou identitu, uložte ho v `type` pod `identity` hodnotu.
 
   **Microsoft.Compute/virtualMachines rozhraní API verze 2017-12-01**

   Z virtuálního počítače odebrat jeden uživatel přiřazenou spravovanou identitu, odeberte ho z `identityIds` pole.

   Pokud máte systém přiřadil spravovanou identitu, uložte ho v `type` pod `identity` hodnotu.
   
## <a name="next-steps"></a>Další postup

- [Spravovaných identit pro prostředky Azure přehled](overview.md).

