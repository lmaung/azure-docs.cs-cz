---
title: Vytváření a správa zásad pro vynucování dodržování předpisů
description: Pomocí služby Azure Policy můžete vynucovat standardy, plnit legislativní požadavky na audit a dodržování předpisů, řídit náklady, udržovat konzistentní zabezpečení a výkon a uplatňovat principy návrhu v rámci celého podniku.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/04/2019
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: e49cffc5ba08d400c733ef7c211132c4909f9ef4
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343557"
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Vytváření a správa zásad pro vynucování dodržování předpisů

Pochopení způsobu, jakým se v Azure vytvářejí a spravují zásady, je důležité pro zajištění souladu s firemními standardy a smlouvami o úrovni služeb. V tomto kurzu se dozvíte, jak pomocí služby Azure Policy provádět některé běžné úlohy související s vytvářením, přiřazováním a správou zásad v rámci celé organizace, jako například:

> [!div class="checklist"]
> - Přiřazení zásady vynucující podmínku u prostředků, které vytvoříte v budoucnu
> - Vytvoření a přiřazení definice iniciativy pro sledování dodržování předpisů u několika prostředků
> - Řešení problému s prostředkem nedodržujícím předpisy nebo zamítnutým prostředkem
> - Implementace nové zásady v rámci celé organizace

Pokud chcete přiřadit zásadu pro identifikaci aktuálního stavu dodržování předpisů u vašich existujících prostředků, postup najdete v článcích Rychlý start. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="assign-a-policy"></a>Přiřazení zásady

Prvním krokem při vynucování dodržování předpisů pomocí služby Azure Policy je přiřazení definice zásady. Definice zásady definuje, za jakých podmínek se zásada vynucuje a jaký účinek se má projevit. V tomto příkladu přiřadíte předdefinovanou definici zásady *Vyžadovat SQL Server verze 12.0*, která bude vynucovat podmínku, že všechny databáze SQL Serveru musí být verze 12.0, aby dodržovaly předpisy.

1. Spusťte službu Azure Policy na webu Azure Portal tak, že kliknete na **Všechny služby** a pak vyhledáte a vyberete **Zásady**.

   ![Hledání zásad](../media/create-and-manage/search-policy.png)

1. Na levé straně stránky služby Azure Policy vyberte **Přiřazení**. Přiřazení je zásada, která byla přiřazena, aby proběhla v rámci zadaného oboru.

   ![Výběr přiřazení](../media/create-and-manage/select-assignments.png)

1. V horní části stránky **Zásady – Přiřazení** vyberte **Přiřadit zásadu**.

   ![Přiřazení definice zásady](../media/create-and-manage/select-assign-policy.png)

1. Na stránce **Přiřadit zásadu** vyberte **Obor** tak, že kliknete na tři tečky a vyberete skupinu pro správu nebo předplatné. Volitelně můžete vybrat skupinu prostředků. Obor určuje, pro které prostředky nebo skupiny prostředků se toto přiřazení zásady bude vynucovat.  Pak v dolní části stránky **Obor** klikněte na **Vybrat**.

   V tomto příkladu se používá předplatné **Contoso**. Vaše předplatné se bude lišit.

1. Prostředky je možné vyloučit na základě **oboru**.  **Vyloučení** začínají na úrovni o jednu nižší, než je úroveň **oboru**. **Vyloučení** jsou volitelná, takže toto pole prozatím ponechte prázdné.

1. Výběrem tří teček **Definice zásady** otevřete seznam dostupných definic. Můžete nastavit filtr pro **Typ** definic zásad na *Předdefinované* a zobrazit všechny definice zásad a přečíst si jejich popisy.

1. Vyberte zásadu **Vyžadovat SQL Server verze 12.0**. Pokud nemůžete najít to okamžitě, zadejte **vyžaduje systém sql server** do vyhledávacího pole a potom stiskněte klávesu ENTER nebo klikněte na z vyhledávacího pole. Jakmile najdete a vyberete definici zásady, v dolní části stránky **Dostupné definice** klikněte na **Vybrat**.

   ![Vyhledání zásady](../media/create-and-manage/select-available-definition.png)

1. Do pole **Název přiřazení** se automaticky vyplní název vybrané zásady, který však můžete změnit. Pro účely tohoto příkladu ponechte název *Vyžadovat SQL Server verze 12.0*. Volitelně můžete přidat také **Popis**. Popis obsahuje podrobnosti o tomto přiřazení zásady.  **Přiřazené podle** se automaticky vyplní podle toho, který je přihlášen. Toto pole je volitelné, takže do něj můžete zadávat vlastní hodnoty.

1. Políčko **Vytvořit spravovanou identitu** ponechte nezaškrtnuté. Toto políčko _musí_ být vráceny, pokud obsahuje zásady nebo iniciativa přiřazení zásad s [deployIfNotExists](../concepts/effects.md#deployifnotexists) vliv. Jak zásady používané pro účely tohoto kurzu není, ponechte prázdné. Další informace najdete v tématech věnovaných [spravovaným identitám](../../../active-directory/managed-identities-azure-resources/overview.md) a [principu fungování zabezpečení náprav](../how-to/remediate-resources.md#how-remediation-security-works).

1. Klikněte na **Přiřadit**.

## <a name="implement-a-new-custom-policy"></a>Implementace nové vlastní zásady

Teď, když jste přiřadili předdefinovanou definici zásady, můžete se službou Azure Policy provádět další akce. Dále vytvořte novou vlastní zásadu pro úsporu nákladů tak, že virtuální počítače vytvořené ve vašem prostředí nemůže být v řadě G series. Díky tomu se zamítnou všechny žádosti uživatelů ve vaší organizaci o vytvoření virtuálního počítače v řadě G Series.

1. Na levé straně stránky služby Azure Policy v části **Vytváření obsahu** vyberte **Definice**.

   ![Definice v části Vytváření obsahu](../media/create-and-manage/definition-under-authoring.png)

1. V horní části stránky vyberte **+ Definice zásady**. Toto tlačítko otevře **definice zásady** stránky.

1. Zadejte následující informace:

   - Skupina pro správu nebo předplatné, ve kterém je definice zásady uložená. Výběr proveďte pomocí tří teček v části **Umístění definice**.

     > [!NOTE]
     > Pokud se chystáte tuto definici zásady použít pro více předplatných, umístěním musí být skupina pro správu obsahující předplatná, ke kterým zásadu přiřadíte. Totéž platí i pro definici iniciativy.

   - Název definice zásady – *Vyžadovat nižší skladové položky virtuálních počítačů než G Series*.
   - Popis účelu definice zásady – *Tato definice zásady za účelem snížení nákladů vynucuje, aby všechny virtuální počítače vytvořené v tomto oboru měly skladové položky nižší než G Series.*
   - Zvolte některou z existujících možností (například _Compute_) nebo pro tuto definici zásady vytvořte novou kategorii.
   - Zkopírujte následující kód JSON a pak v něm podle potřeby aktualizujte:
      - Parametry zásady.
      - Pravidla a podmínky zásady, v tomto případě – velikost skladové položky virtuálního počítače se rovná řadě G Series.
      - Účinek zásady, v tomto případě – **Deny** (Zamítnutí).

    Tady je ukázka kódu JSON. Vložte svůj upravený kód na web Azure Portal.

    ```json
    {
        "policyRule": {
            "if": {
                "allOf": [{
                        "field": "type",
                        "equals": "Microsoft.Compute/virtualMachines"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/sku.name",
                        "like": "Standard_G*"
                    }
                ]
            },
            "then": {
                "effect": "deny"
            }
        }
    }
    ```

    *Pole* vlastnost v pravidlu zásad musí být jedna z následujících hodnot: Název, typ, umístění, značky nebo alias. Příkladem aliasu může být `"Microsoft.Compute/VirtualMachines/Size"`.

    Další ukázky zásad Azure najdete v [ukázkách pro Azure Policy](../samples/index.md).

1. Vyberte **Uložit**.

## <a name="create-a-policy-definition-with-rest-api"></a>Vytvoření definice zásady pomocí rozhraní REST API

Zásadu můžete vytvořit i pomocí rozhraní REST API pro definice zásad. Toto rozhraní API umožňuje vytvářet a odstraňovat definice zásad a získávat informace o existujících definicích. Pokud chcete vytvořit definici zásady, použijte následující příklad:

```http-interactive
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Přiložte podobný text žádosti jako v následujícím příkladu:

```json
{
    "properties": {
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>Vytvoření definice zásady pomocí PowerShellu

Než budete pokračovat k příkladu Powershellu, ujistěte se, že jste nainstalovali nejnovější verzi Azure Powershellu. Parametry zásady byly přidány ve verzi 3.6.0. Pokud máte starší verzi, uvedené příklady vrátí chybu s informacemi, že parametr nebyl nalezen.

Definici zásady můžete vytvořit pomocí rutiny `New-AzPolicyDefinition`.

Pokud chcete vytvořit definici zásady ze souboru, předejte cestu k souboru. Pro externí soubor použijte následující příklad:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Pro místní soubor použijte následující příklad:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

Pokud chcete vytvořit definici zásady s vloženým pravidlem, použijte následující příklad:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

Výstup se ukládá v objektu `$definition`, který se používá při přiřazování zásady. Následující příklad vytvoří definici zásady zahrnující parametry:

```azurepowershell-interactive
$policy = '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of locations that can be specified when deploying storage accounts.",
            "strongType": "location",
            "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>Zobrazení definic zásad pomocí PowerShellu

Pokud chcete zobrazit všechny definice zásad ve svém předplatném, použijte následující příkaz:

```azurepowershell-interactive
Get-AzPolicyDefinition
```

Příkaz vrátí všechny dostupné definice zásad, včetně předdefinovaných zásad. Všechny zásady se vrátí v následujícím formátu:

```output
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Vytvoření definice zásady pomocí Azure CLI

V Azure CLI můžete vytvořit definici zásady pomocí příkazu policy definition. Pokud chcete vytvořit definici zásady s vloženým pravidlem, použijte následující příklad:

```azurecli-interactive
az policy definition create --name 'denyCoolTiering' --description 'Deny cool access tiering for storage' --rules '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

### <a name="view-policy-definitions-with-azure-cli"></a>Zobrazení definic zásad pomocí Azure CLI

Pokud chcete zobrazit všechny definice zásad ve svém předplatném, použijte následující příkaz:

```azurecli-interactive
az policy definition list
```

Příkaz vrátí všechny dostupné definice zásad, včetně předdefinovaných zásad. Všechny zásady se vrátí v následujícím formátu:

```json
{
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",
    "displayName": "Allowed locations",
    "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "policyRule": {
        "if": {
            "not": {
                "field": "location",
                "in": "[parameters('listOfAllowedLocations')]"
            }
        },
        "then": {
            "effect": "Deny"
        }
    },
    "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Vytvoření a přiřazení definice iniciativy

Pomocí definice iniciativy můžete seskupit několik definic zásad za účelem dosažení jednoho zastřešujícího cíle. Iniciativa vyhodnotí jako prostředky v rámci oboru přiřazení pro zahrnuté dodržovat. Další informace o definicích iniciativ najdete v tématu [Přehled služby Azure Policy](../overview.md).

### <a name="create-an-initiative-definition"></a>Vytvoření definice iniciativy

1. Na levé straně stránky služby Azure Policy v části **Vytváření obsahu** vyberte **Definice**.

   ![Výběr definic](../media/create-and-manage/definition-under-authoring.png)

1. V horní části stránky vyberte **+ Definice iniciativy** a otevřete stránku **Definice iniciativy**.

   ![Definice iniciativy](../media/create-and-manage/initiative-definition.png)

1. Pomocí tří teček **Umístění definice** vyberte skupinu pro správu nebo předplatné, kam se definice uloží. Pokud jste na předchozí stránce omezili obor na jednu skupinu pro správu nebo jedno předplatné, **Umístění definice** se vyplní automaticky.

1. Zadejte **Název** a **Popis** iniciativy.

   V tomto příkladu ověřuje, zda jsou prostředky v souladu s definicemi zásad zajištěním zabezpečení. Název iniciativy **zajištění zabezpečení** a nastavit popis jako: **Tato iniciativa byla vytvořená za účelem zpracování všech definic zásad souvisejících se zabezpečením prostředků**.

1. V části **Kategorie** zvolte některou z existujících možností nebo vytvořte novou kategorii.

1. Projděte seznam **Dostupné definice** (pravá polovina stránky **Definice iniciativy**) a vyberte definice zásad, které chcete přidat do této iniciativy. Pro iniciativu **Zajištění zabezpečení** přidejte následující předdefinované definice zásad kliknutím na **+** vedle informací o definici zásady nebo kliknutím na řádek definice zásady a pak na stránce s podrobnostmi na možnost **+ Přidat**:

   - Vyžadovat SQL Server verze 12.0
   - [Preview]: Monitor unprotected web applications in Security Center.
   - [Preview]: Monitor permissive network across in Security Center.
   - [Preview]: Monitor possible app Whitelisting in Security Center.
   - [Preview]: Monitor unencrypted VM Disks in Security Center.

   Po výběru definici zásady ze seznamu, přidá se v rámci **zásady a parametry**.

   ![Definice iniciativ](../media/create-and-manage/initiative-definition-2.png)

1. Pokud se přidávají do iniciativy definice zásady obsahuje parametry, jsou uvedené v části název zásady v **zásady a parametry** oblasti. _Hodnotu_ je možné nastavit na možnost Nastavit hodnotu (pevně zakódovaná pro všechna přiřazení této iniciativy) nebo Použít parametr iniciativy (nastaví se při každém přiřazení iniciativy). Pokud je zaškrtnuto políčko "Hodnotu" rozevírací seznam napravo od _hodnoty_ umožňuje zadáním nebo výběrem hodnoty. Pokud vyberete možnost Použít parametr iniciativy, zobrazí se část **Parametry iniciativy**, kde můžete definovat parametr, který se nastaví během přiřazení iniciativy. Povolené hodnoty pro tento parametr iniciativy můžou dále omezit možnosti nastavení během přiřazení iniciativy.

   ![Parametry definice iniciativy](../media/create-and-manage/initiative-definition-3.png)

   > [!NOTE]
   > U některých parametrů `strongType` není možné automaticky určit seznam hodnot. V těchto případech se napravo od řádku parametru zobrazí tři tečky. Po kliknutí na tyto tři tečky se otevře stránka Obor parametru (&lt;název_parametru&gt;). Na této stránce vyberte předplatné, které chcete použít k zadání možností hodnot. Tento obor parametru se používá pouze během vytváření definice iniciativy a nemá žádný vliv na vyhodnocování zásad ani na obor iniciativy po přiřazení.

1. Klikněte na **Uložit**.

### <a name="assign-an-initiative-definition"></a>Přiřazení definice iniciativy

1. Na levé straně stránky služby Azure Policy v části **Vytváření obsahu** vyberte **Definice**.

1. Vyhledejte definici iniciativy **Zajištění zabezpečení**, kterou jste vytvořili dříve, a klikněte na ni. Vyberte **přiřadit** v horní části stránky a otevře **zajištění zabezpečení: Přiřadit iniciativu** stránky.

   ![Přiřazení definice](../media/create-and-manage/assign-definition.png)

   Můžete také kliknout pravým tlačítkem na vybraný řádek nebo levým tlačítkem myši na tři tečky na konci řádku kontextové nabídky.  Pak vyberte **Přiřadit**.

   ![Kliknutí pravým tlačítkem na řádek](../media/create-and-manage/select-right-click.png)

1. Vyplňte **zajištění zabezpečení: Přiřadit iniciativu** stránky zadáním následujících ukázkových údajů. Můžete použít vlastní údaje.

   - Rozsah: Skupiny pro správu nebo předplatného, které jste si uložili iniciativa zaměřená na stane výchozí.  Obor můžete změnit a přiřadit tak iniciativu k předplatnému nebo ke skupině prostředků v rámci umístění pro uložení.
   - Vyloučení: Konfigurujte všechny prostředky v rámci oboru, aby se zabránilo přiřazení iniciativy se použily k nim.
   - Definice iniciativy a název úlohy: Získáte zabezpečené (předem vyplní názvem přiřazení iniciativy).
   - Popis: Toto přiřazení iniciativy je přizpůsobené k vynucování této skupiny definic zásad.
   - Přiřadil: Automaticky vyplní podle toho, který je přihlášen. Toto pole je volitelné, takže do něj můžete zadávat vlastní hodnoty.

1. Políčko **Vytvořit spravovanou identitu** ponechte nezaškrtnuté. Toto políčko _musí_ být vráceny, pokud obsahuje zásady nebo iniciativa přiřazení zásad s [deployIfNotExists](../concepts/effects.md#deployifnotexists) vliv. Jak zásady používané pro účely tohoto kurzu není, ponechte prázdné. Další informace najdete v tématech věnovaných [spravovaným identitám](../../../active-directory/managed-identities-azure-resources/overview.md) a [principu fungování zabezpečení náprav](../how-to/remediate-resources.md#how-remediation-security-works).

1. Klikněte na **Přiřadit**.

## <a name="check-initial-compliance"></a>Kontrola počátečního dodržování předpisů

1. Na levé straně stránky služby Azure Policy vyberte **Dodržování předpisů**.

1. Vyhledejte iniciativu **Zajištění zabezpečení**. Bude pravděpodobně stále v _stavu dodržování předpisů_ z **Nezahájeno**. Kliknutím na iniciativu zobrazte úplné podrobnosti o průběhu přiřazení.

   ![Dodržování předpisů – Nezahájeno](../media/create-and-manage/compliance-status-not-started.png)

1. Po dokončení přiřazení iniciativy se na stránce Dodržování předpisů aktualizuje _Stav dodržování předpisů_ na **Vyhovuje**.

   ![Dodržování předpisů – Vyhovuje](../media/create-and-manage/compliance-status-compliant.png)

1. Po kliknutím na jakoukoli zásadu na stránce Dodržování předpisů iniciativy se otevře stránka podrobností o dodržování předpisů pro příslušnou zásadu. Tato stránka obsahuje podrobnosti o dodržování předpisů na úrovni prostředku.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Vyloučení prostředku nedodržujícího předpisy nebo zamítnutého prostředku s využitím vyloučení

Ve výše uvedeném příkladu se po přiřazení definice zásady vyžadující SQL Server verze 12.0 zamítne vytvoření SQL Serveru s jinou verzí než 12.0. V této části si projdete řešení zamítnutého žádost o vytvoření SQL serveru tak, že vytvoříte vyloučení na jedinou skupinu prostředků. Vyloučení zabrání vynucování zásady (nebo iniciativy) pro příslušný prostředek.
V následujícím příkladu je v jedné skupině prostředků povolená jakákoli verze SQL Serveru. Vyloučení se může vztahovat na předplatné nebo skupinu prostředků nebo ho můžete zúžit na jednotlivé prostředky.

Nasazení brání přiřazené zásady nebo iniciativa si můžou prohlédnout ve dvou umístěních:

- Cílem nasazení skupiny prostředků: Vyberte **nasazení** v levé části stránky klikněte a klikněte na kartu **název nasazení** selhání nasazení. U zamítnutého prostředku je uvedený stav _Zakázáno_. Pokud chcete určit zásadu nebo iniciativu a přiřazení, které prostředek zamítlo, klikněte na **Selhání. Podrobnosti zobrazíte kliknutím sem ->** na stránce Přehled nasazení. Na pravé straně stránky se otevře okno s informacemi o chybě. V části **podrobnosti o chybě** jsou identifikátory GUID objektů souvisejících zásad.

  ![Nasazení zamítnuté přiřazením zásady](../media/create-and-manage/rg-deployment-denied.png)

- Na stránce Azure Policy: Vyberte **dodržování předpisů** v levé části stránky a klikněte na kartu **vyžadovat SQL Server verze 12.0** zásad. Na stránce, která se otevře, se zobrazí zvýšení počtu **Zamítnutí**. V části **události** kartu, také uvidíte, kteří se pokusili nasazení, který byl odepřen v zásadách.

  ![Přehled dodržování předpisů přiřazené zásady](../media/create-and-manage/compliance-overview.png)

V tomto příkladu dělal Trent Svoboda, jeden hlavní virtualizace odborníků společnosti Contoso, požadovanou práci. Budeme muset udělit Trent výjimku, ale nechceme systémy SQL Server verze 12.0 v právě libovolné skupině prostředků. Vytvořili jsme novou skupinu prostředků **SQLServers_Excluded** a teď jí udělíme výjimku pro toto přiřazení zásady.

### <a name="update-assignment-with-exclusion"></a>Aktualizace přiřazení o vyloučení

1. Na levé straně stránky služby Azure Policy v části **Vytváření obsahu** vyberte **Přiřazení**.

1. Projděte všechna přiřazení zásad a otevřete přiřazení *Vyžadovat SQL Server verze 12.0*.

1. Nastavte **Vyloučení** tak, že kliknete na tři tečky a vyberete skupinu prostředků, kterou chcete vyloučit, v tomto příkladu *SQLServers_Excluded*.

   ![Žádost o vyloučení](../media/create-and-manage/request-exclusion.png)

   > [!NOTE]
   > V závislosti na zásadě a jejím účinku je možné udělit vyloučení také konkrétním prostředkům v rámci skupiny prostředků v oboru přiřazení. Vzhledem k tomu, že se v tomto kurzu použil účinek **Zamítnutí**, nedávalo by smysl nastavit vyloučení pro konkrétní prostředek, který již existuje.

1. Klikněte na **Vybrat** a potom klikněte na **Uložit**.

V této části jste vyřešili odepření požadavku tak, že vytvoříte vyloučení na jedinou skupinu prostředků.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud to uděláte práci s prostředky z tohoto kurzu, pomocí následujícího postupu odstraňte všechna přiřazení a definice vytvořili výše:

1. Vyberte **definice** (nebo **přiřazení** Pokud se pokoušíte odstranit přiřazení) v části **Authoring** v levé části na stránku služby Azure Policy.

1. Vyhledejte novou definici iniciativy nebo zásady (nebo přiřazení), kterou chcete odebrat.

1. Klikněte na řádek pravým tlačítkem nebo vyberte tři tečky na konci definice (nebo přiřazení) a pak vyberte **Odstranit definici** (nebo **Odstranit přiřazení**).

## <a name="next-steps"></a>Další postup

V tomto kurzu jste úspěšně provedli následující úlohy:

> [!div class="checklist"]
> - Přiřadili jste zásadu vynucující podmínku u prostředků, které vytvoříte v budoucnu.
> - Vytvořili a přiřadili jste definici iniciativy pro sledování dodržování předpisů u několika prostředků.
> - Vyřešili jste problém s prostředkem nedodržujícím předpisy nebo zamítnutým prostředkem.
> - Implementovali jste novou zásadu v rámci celé organizace.

Další informace o strukturách definic zásad najdete v tomto článku:

> [!div class="nextstepaction"]
> [Struktura definic Azure Policy](../concepts/definition-structure.md)
