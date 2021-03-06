---
title: Prostřednictvím kódu programu vytvořit zásady a zobrazit data o dodržování předpisů
description: Tento článek vás provede programově vytváření a Správa zásad pro Azure Policy.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/31/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: bf831936f86de9e43170a87c03b61df9a4865b3d
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56878043"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Prostřednictvím kódu programu vytvořit zásady a zobrazit data o dodržování předpisů

Tento článek vás provede programově vytváření a Správa zásad. Definice zásad u vašich prostředků vynucují různá pravidla a efekty. Vynucení zajišťuje, že prostředky budou odpovídat vašim firemním standardům a smlouvám o úrovni.

Informace o dodržování předpisů najdete v tématu [získávají data dodržování předpisů](getting-compliance-data.md).

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že jsou splněny následující požadavky:

1. Pokud jste to ještě neudělali, nainstalujte si nástroj [ARMClient](https://github.com/projectkudu/ARMClient). Jedná se o nástroj, který posílá žádosti HTTPS do rozhraní API založených na Azure Resource Manageru.

1. Aktualizujte na nejnovější verzi modulu Azure Powershellu. Zobrazit [instalace modulu Azure PowerShell](/powershell/azure/install-az-ps) podrobné informace. Další informace o nejnovější verzi najdete v tématu [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

1. Zaregistrujte poskytovatele prostředků Policy Insights pomocí prostředí Azure PowerShell k ověření, že vaše předplatné spolupracuje s poskytovateli prostředků. Zaregistrovat poskytovatele prostředků, musíte mít oprávnění k provedení operace akce registrace pro poskytovatele prostředků. Tato operace je součástí rolí Přispěvatel a Vlastník. Spuštěním následujícího příkazu zaregistrujte poskytovatele prostředků:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Další informace o registraci a zobrazení poskytovatelů prostředků najdete v tématu [poskytovatelé a typy prostředků](../../../azure-resource-manager/resource-manager-supported-services.md).

1. Pokud jste tak dosud neučinili, nainstalujte Azure CLI. Můžete získat nejnovější verzi na [instalace Azure CLI ve Windows](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Vytvoření a přiřazení definice zásady

Prvním krokem k lepší přehled o vašich prostředků je vytvoření a přiřazení zásad u vašich prostředků. Dalším krokem je zjistěte, jak prostřednictvím kódu programu vytvořit a přiřadit zásady. Příklad zásady auditují účty úložiště, které jsou spuštěné všechny veřejné sítě pomocí Powershellu, rozhraní příkazového řádku Azure a požadavky HTTP.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Vytvoření a přiřazení definice zásady pomocí Powershellu

1. Pomocí následujícího fragmentu kódu JSON vytvoříte soubor JSON s názvem AuditStorageAccounts.json.

   ```json
   {
       "if": {
           "allOf": [{
                   "field": "type",
                   "equals": "Microsoft.Storage/storageAccounts"
               },
               {
                   "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                   "equals": "Allow"
               }
           ]
       },
       "then": {
           "effect": "audit"
       }
   }
   ```

   Další informace o vytváření definice zásady, najdete v části [struktura definic Azure Policy](../concepts/definition-structure.md).

1. Spusťte následující příkaz k vytvoření definice zásady pomocí AuditStorageAccounts.json souboru.

   ```azurepowershell-interactive
   New-AzPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   Příkaz vytvoří definici zásady s názvem _auditu úložiště účtů otevřít k veřejným sítím_.
   Další informace o dalších parametrů, které můžete použít, najdete v části [New-AzPolicyDefinition](/powershell/module/az.resources/new-azpolicydefinition).

   Při volání bez parametrů místo `New-AzPolicyDefinition` výchozí hodnota je ukládání definice zásad ve vybraném předplatném kontextu relace. Pokud chcete uložit definici do jiného umístění, použijte následující parametry:

   - **SubscriptionId** -uložit do jiného předplatného. Vyžaduje _GUID_ hodnotu.
   - **ManagementGroupName** -uložit ve skupině pro správu. Vyžaduje _řetězec_ hodnotu.

1. Po vytvoření definic zásad, můžete vytvořit přiřazení zásady spuštěním následujících příkazů:

   ```azurepowershell-interactive
   $rg = Get-AzResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   Nahraďte _ContosoRG_ s názvem vaší skupiny prostředků určené.

   **Oboru** parametru u `New-AzPolicyAssignment` funguje s skupiny pro správu, předplatné, skupinu prostředků nebo jediný prostředek. Parametr používá cestu úplné prostředku, který **ResourceId** vlastnost `Get-AzResourceGroup` vrátí. Vzor pro **oboru** pro každý kontejner je následujícím způsobem. Nahraďte `{rName}`, `{rgName}`, `{subId}`, a `{mgName}` se název prostředku, skupina prostředků název, ID předplatného a název skupiny pro správu, v uvedeném pořadí.
   `{rType}` by měl být nahrazen **typ prostředku** prostředku, jako například `Microsoft.Compute/virtualMachines` pro virtuální počítač.

   - Prostředek – `/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Skupina prostředků- `/subscriptions/{subId}/resourceGroups/{rgName}`
   - Předplatné – `/subscriptions/{subId}/`
   - Skupina pro správu- `/providers/Microsoft.Management/managementGroups/{mgName}`

Další informace o správě zásad prostředků pomocí modulu Powershellu pro Azure Resource Manager, najdete v části [Az.Resources](/powershell/module/az.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Vytvoření a přiřazení definice zásady pomocí ARMClient

Pomocí následujícího postupu můžete vytvořit definici zásady.

1. Zkopírujte následující fragment kódu JSON vytvoříte soubor JSON. Budete volat ho v dalším kroku.

   ```json
   "properties": {
       "displayName": "Audit Storage Accounts Open to Public Networks",
       "policyType": "Custom",
       "mode": "Indexed",
       "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
       "parameters": {},
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Storage/storageAccounts"
                   },
                   {
                       "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                       "equals": "Allow"
                   }
               ]
           },
           "then": {
               "effect": "audit"
           }
       }
   }
   ```

1. Vytvoření definice zásady pomocí jedné z následující volání:

   ```console
   # For defining a policy in a subscription
   armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

   # For defining a policy in a management group
   armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
   ```

   Nahraďte {subscriptionId} předchozí ID předplatného nebo {managementGroupId} s ID vašich [skupiny pro správu](../../management-groups/overview.md).

   Další informace o struktuře dotazu naleznete v tématu [definice zásad – vytvořit nebo aktualizovat](/rest/api/resources/policydefinitions/createorupdate) a [definice zásad – vytvoření nebo aktualizace ve skupině pro správu](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Použijte následující postup k vytvoření přiřazení zásady a přiřazení definice zásady na úrovni skupiny prostředků.

1. Zkopírujte následující fragment kódu JSON vytvořte soubor JSON přiřazení zásad. Nahraďte informace z příkladu v &lt; &gt; symboly s vlastními hodnotami.

   ```json
   {
       "properties": {
           "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
           "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
           "parameters": {},
           "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
           "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
       }
   }
   ```

1. Vytvořte přiřazení zásad pomocí následujícího volání:

   ```console
   armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
   ```

   Nahraďte informace z příkladu v &lt; &gt; symboly s vlastními hodnotami.

   Další informace o tom, že volání HTTP REST API najdete v tématu [prostředkům Azure REST API](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Vytvoření a přiřazení definice zásady pomocí Azure CLI

Pokud chcete vytvořit definici zásady, použijte následující postup:

1. Zkopírujte následující fragment kódu JSON vytvořte soubor JSON přiřazení zásad.

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

   Další informace o vytváření definice zásady, najdete v části [struktura definic Azure Policy](../concepts/definition-structure.md).

1. Spusťte následující příkaz k vytvoření definice zásady:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

   Příkaz vytvoří definici zásady s názvem _auditu úložiště účtů otevřít k veřejným sítím_.
   Další informace o dalších parametrů, které můžete použít, najdete v části [vytvoření definice zásady az](/cli/azure/policy/definition#az-policy-definition-create).

   Při volání bez parametrů místo `az policy definition creation` výchozí hodnota je ukládání definice zásad ve vybraném předplatném kontextu relace. Pokud chcete uložit definici do jiného umístění, použijte následující parametry:

   - **--předplatné** -uložit do jiného předplatného. Vyžaduje _GUID_ hodnotu pro ID předplatného nebo _řetězec_ hodnotu pro název předplatného.
   - **--skupiny pro správu** -uložit ve skupině pro správu. Vyžaduje _řetězec_ hodnotu.

1. Použijte následující příkaz k vytvoření přiřazení zásady. Nahraďte informace z příkladu v &lt; &gt; symboly s vlastními hodnotami.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

   **--Rozsah** parametru u `az policy assignment create` funguje s skupiny pro správu, předplatné, skupinu prostředků nebo jediný prostředek. Parametr používá prostředků úplné cesty. Vzor pro **--rozsah** pro každý kontejner je následujícím způsobem. Nahraďte `{rName}`, `{rgName}`, `{subId}`, a `{mgName}` se název prostředku, skupina prostředků název, ID předplatného a název skupiny pro správu, v uvedeném pořadí. `{rType}` by měl být nahrazen **typ prostředku** prostředku, jako například `Microsoft.Compute/virtualMachines` pro virtuální počítač.

   - Prostředek – `/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Skupina prostředků- `/subscriptions/{subID}/resourceGroups/{rgName}`
   - Předplatné – `/subscriptions/{subID}`
   - Skupina pro správu- `/providers/Microsoft.Management/managementGroups/{mgName}`

Můžete získat ID definice zásady pomocí Powershellu pomocí následujícího příkazu:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

ID definice zásady, kterou jste vytvořili definice zásad by měl vypadat podobně jako v následujícím příkladu:

```output
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Další informace o tom, jak můžete spravovat zásady prostředků pomocí Azure CLI najdete v tématu [zásady prostředků rozhraní příkazového řádku Azure](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Další postup

Projděte si následující články pro další informace o příkazech a dotazy v tomto článku.

- [Prostředky Azure REST API](/rest/api/resources/)
- [Moduly Azure Powershellu](/powershell/module/az.resources/#policies)
- [Zásady příkazy rozhraní příkazového řádku Azure](/cli/azure/policy?view=azure-cli-latest)
- [Poskytovatel prostředků Insights zásad reference k rozhraní REST API](/rest/api/policy-insights)
- [Uspořádání prostředků se skupinami pro správu Azure](../../management-groups/overview.md)