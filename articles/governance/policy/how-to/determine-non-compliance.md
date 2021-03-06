---
title: Určení příčiny nedodržování předpisů
description: Pokud prostředek je nedodržují předpisy, existuje mnoho důvodů, proč je to možné. Zjistěte, jak zjistit, co způsobilo nedodržení předpisů.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: bdc7e635537eb4f836f0d4b5e2c18cc62b7fd310
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56890423"
---
# <a name="determine-causes-of-non-compliance"></a>Určení příčiny nedodržování předpisů

Při prostředku Azure je určena jako nevyhovující pravidla zásad, je vhodné pochopit, jaká část pravidla prostředek není kompatibilní s. Je také užitečné porozumět, jaké změny změnit dříve odpovídal požadavkům prostředek k němu nedodržující předpisy. Existují dva způsoby, jak najít tyto informace:

> [!div class="checklist"]
> - [Podrobnosti o dodržování předpisů](#compliance-details)
> - [Historie změn (Preview)](#change-history-preview)

## <a name="compliance-details"></a>Podrobnosti dodržování předpisů

Pokud prostředek je nedodržují předpisy, podrobnosti o dodržování předpisů pro tento prostředek jsou k dispozici **dodržování zásad** stránky. V podokně Podrobnosti dodržování předpisů zahrnuje následující informace:

- Podrobnosti o prostředku, jako je například název, typ, umístění a ID prostředku
- Stav dodržování předpisů a časové razítko posledního vyhodnocení na aktuální přiřazení zásad
- Seznam _důvodů_ prostředek nedodržení předpisů

> [!IMPORTANT]
> Jako podrobnosti o dodržování předpisů pro _nekompatibilní_ prostředků zobrazuje aktuální hodnota vlastnosti na tento prostředek, musí mít uživatel **čtení** operace **typ** z prostředek. Například pokud _nekompatibilní_ prostředek je **Microsoft.Compute/virtualMachines** musí mít uživatel **Microsoft.Compute/virtualMachines/read** operace. Pokud uživatel nemá potřebné operace, zobrazí se chyba přístupu.

Chcete-li zobrazit podrobnosti o dodržování předpisů, postupujte podle těchto kroků:

1. Spusťte službu Azure Policy na webu Azure Portal tak, že kliknete na **Všechny služby** a pak vyhledáte a vyberete **Zásady**.

1. Na **přehled** nebo **dodržování předpisů** vyberte zásadu v **stavu dodržování předpisů** , který je _nekompatibilní_.

1. V části **dodržování předpisů prostředkem** karty **dodržování zásad** stránky, klikněte pravým tlačítkem myši nebo vyberte tři tečky prostředku v **stavu dodržování předpisů** , který je  _Nedodržující předpisy_. Potom vyberte **zobrazit podrobnosti o dodržování předpisů**.

   ![Zobrazit možnost Podrobnosti dodržování předpisů](../media/determine-non-compliance/view-compliance-details.png)

1. **Podrobnosti o dodržování předpisů** podokně se zobrazí informace z nejnovější vyhodnocení prostředku do aktuálního přiřazení zásad. V tomto příkladu pole **Microsoft.Sql/servers/version** bude zjištěna _12.0_ při definici zásad, byl očekáván _14.0_. Pokud prostředek je jako nevyhovující z několika důvodů, každý je uvedený v tomto podokně.

   ![Podokno Podrobnosti dodržování předpisů](../media/determine-non-compliance/compliance-details-pane.png)

   Pro **auditIfNotExists** nebo **deployIfNotExists** údaje obsahují definice zásad **details.type** vlastnost a všechny volitelné vlastnosti. Seznam najdete v tématu [auditIfNotExists vlastnosti](../concepts/effects.md#auditifnotexists-properties) a [deployIfNotExists vlastnosti](../concepts/effects.md#deployifnotexists-properties). **Naposledy vyhodnoceno prostředků** souvisejících prostředků z je **podrobnosti** část definice.

   Příklad částečné **deployIfNotExists** definice:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   ![Podokno Podrobnosti dodržování předpisů – * ifNotExists](../media/determine-non-compliance/compliance-details-pane-existence.png)

> [!NOTE]
> K ochraně dat, pokud je hodnota vlastnosti _tajný kód_ hvězdičky z obou stran zobrazí aktuální hodnotu.

Tyto podrobnosti vysvětlit, proč prostředek aktuálně nedodržují předpisy, ale nezobrazovat při provedení změny na prostředek, který způsobil, že se jako nevyhovující. Informace najdete v tématu [změnit historii (Preview)](#change-history-preview) níže.

### <a name="compliance-reasons"></a>Dodržováním předpisů

Následující matice mapuje každé možné _důvod_ na příslušný [podmínku](../concepts/definition-structure.md#conditions) v definici zásad:

|Důvod | Podmínka |
|-|-|
|Aktuální hodnota musí jako klíč obsahovat cílovou hodnotu. |containsKey nebo **není** notContainsKey |
|Aktuální hodnota musí obsahovat cílovou hodnotu. |obsahuje nebo **není** notContains |
|Aktuální hodnota musí být shodná s cílovou hodnotou. |se rovná nebo **není** notEquals |
|Aktuální hodnota musí existovat. |Existuje |
|Aktuální hodnota musí být v cílové hodnotě. |v nebo **není** notIn |
|Aktuální hodnota musí být jako cílová hodnota. |například nebo **není** notLike |
|Aktuální hodnota musí rozlišovat malá a velká písmena cílovou hodnotu. |odpovídat nebo **není** notMatch |
|Aktuální hodnota musí být malá a velká písmena cílovou hodnotu. |matchInsensitively nebo **není** notMatchInsensitively |
|Aktuální hodnota nesmí jako klíč obsahovat cílovou hodnotu. |notContainsKey nebo **není** containsKey|
|Aktuální hodnota nesmí obsahovat cílovou hodnotu. |notContains nebo **není** obsahuje |
|Aktuální hodnota nesmí být shodná s cílovou hodnotou. |notEquals nebo **není** rovná se |
|Aktuální hodnota nesmí existovat. |**není** existuje  |
|Aktuální hodnota nesmí být v cílové hodnotě. |notIn nebo **není** v |
|Aktuální hodnota nesmí být jako cílová hodnota. |notLike nebo **není** jako |
|Aktuální hodnota musí není velká a malá písmena rozlišovat cílovou hodnotu. |notMatch nebo **není** odpovídat |
|Aktuální hodnota musí být cílová hodnota není malá a velká písmena. |notMatchInsensitively nebo **není** matchInsensitively |
|Žádné související prostředky neodpovídají podrobnostem účinku v definici zásad. |Prostředek typu definovaného v **then.details.type** a související na prostředek definovaný v **Pokud** část tohoto pravidla zásady neexistuje. |

## <a name="change-history-preview"></a>Historie změn (Preview)

Jako součást nového **ve verzi public preview**, posledních 14 dní změny není k dispozici pro všechny prostředky Azure, které podporují historie [dokončení odstranění režimu](../../../azure-resource-manager/complete-mode-deletion.md). Historie změn při byla zjištěna změna a poskytuje podrobnosti o _visual diff_ pro každou změnu. Detekce změn se aktivuje při přidání, odebrání nebo změnit vlastnosti správce prostředků.

1. Spusťte službu Azure Policy na webu Azure Portal tak, že kliknete na **Všechny služby** a pak vyhledáte a vyberete **Zásady**.

1. Na **přehled** nebo **dodržování předpisů** vyberte zásadu v libovolném **stavu dodržování předpisů**.

1. V části **dodržování předpisů prostředkem** karty **dodržování zásad** vyberte prostředek.

1. Vyberte **historii změn (preview)** kartě **dodržování předpisů prostředkem** stránky. Seznam zjistila změny, pokud jsou zobrazeny všechny existují.

   ![Historie změn zásad – karta](../media/determine-non-compliance/change-history-tab.png)

1. Vyberte jednu z nalezenými změnami. _Visual diff_ pro prostředek se zobrazí na **historii změn** stránky.

   ![Historie změn zásad - Visual Diff](../media/determine-non-compliance/change-history-visual-diff.png)

_Visual diff_ formě identifikovat změny prostředku. Byly zjištěny změny nemusí souviset s aktuální stav dodržování předpisů prostředku.

## <a name="next-steps"></a>Další postup

- Projděte si příklady v [ukázek Azure Policy](../samples/index.md)
- Zkontrolujte [struktura definic zásad](../concepts/definition-structure.md)
- Kontrola [Principy účinky zásad](../concepts/effects.md)
- Pochopit postup [programové vytváření zásad](programmatically-create.md)
- Zjistěte, jak [získat data o dodržování předpisů](getting-compliance-data.md)
- Zjistěte, jak [opravit nekompatibilní prostředky](remediate-resources.md)
- Připomenutí skupin pro správu v článku [Uspořádání prostředků pomocí skupin pro správu Azure](../../management-groups/overview.md)