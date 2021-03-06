---
title: Pochopit, jak auditovat obsah virtuálního počítače
description: Zjistěte, jak Azure Policy používá hostovaný konfigurace auditování nastavení ve virtuálním počítači Azure.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/27/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: e6621172734ea02f971bd5064b403ad4844210a3
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960752"
---
# <a name="understand-azure-policys-guest-configuration"></a>Porozumět konfiguraci hosta Azure Policy

Kromě auditování a [oprava](../how-to/remediate-resources.md) prostředky Azure, Azure Policy můžete auditovat nastavení uvnitř virtuálního počítače. Ověření se provede tak, že rozšíření konfigurace hosta a klienta. Toto rozšíření prostřednictvím klienta, ověří nastavení jako konfigurace operačního systému, konfigurace aplikace nebo přítomnost, nastavení prostředí a další.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="extension-and-client"></a>Rozšíření a klienta

Auditování nastavení uvnitř virtuálního počítače, [rozšíření virtuálního počítače](../../../virtual-machines/extensions/overview.md) je povolená. Rozšíření stahuje použitelné zásady přiřazení a odpovídající definici konfigurace.

### <a name="register-guest-configuration-resource-provider"></a>Registrace poskytovatele prostředků konfigurace hosta

Před použitím konfigurace hosta, zaregistrujte poskytovatele prostředků. Můžete zaregistrovat prostřednictvím portálu nebo pomocí Powershellu. Poskytovatel prostředků se zaregistruje automaticky, pokud přiřazení zásady Konfigurace hosta se provádí prostřednictvím portálu.

#### <a name="registration---portal"></a>Registrace – portál

Registrace poskytovatele prostředků pro konfiguraci hostovaný na webu Azure portal, postupujte podle těchto kroků:

1. Spusťte na webu Azure portal a klikněte na **všechny služby**. Vyhledejte a vyberte **předplatná**.

1. Vyhledejte a klikněte na předplatné, které chcete povolit konfiguraci hosta pro.

1. V levé nabídce **předplatné** klikněte na **poskytovatelů prostředků**.

1. Filtrovat nebo můžete najít pomocí posuvníku **Microsoft.GuestConfiguration**, pak klikněte na tlačítko **zaregistrovat** na stejném řádku.

#### <a name="registration---powershell"></a>Registrace – PowerShell

Zaregistrovat poskytovatele prostředků pro konfiguraci typu Host pomocí prostředí PowerShell, spusťte následující příkaz:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Nástroje pro ověření

Ve virtuálním počítači hosta konfigurace klienta použije místní nástroje pro spuštění auditu.

V následující tabulce je seznam nástrojů pro místní použít na všech podporovaných operačních systémech:

|Operační systém|Nástroje pro ověření|Poznámky|
|-|-|-|
|Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| Ruby a Python instaluje rozšíření konfigurace hosta. |

### <a name="validation-frequency"></a>Frekvence ověření

Klient hosta konfigurace kontroluje nový obsah každých 5 minut. Po přijetí hosta přiřazení nastavení kontroluje v intervalech 15 minut. Výsledky se posílají hosta konfigurace zprostředkovatele prostředků poté, co se dokončí auditu. Když zásadu [vyhodnocení trigger](../how-to/get-compliance-data.md#evaluation-triggers) dojde, stav počítače se zapisují do hostovaného konfigurace zprostředkovatele prostředků. Tato událost způsobí, že Azure Policy k vyhodnocení vlastností Azure Resource Manageru. Vyhodnocení zásad na vyžádání načte poslední hodnotu z hosta konfigurace zprostředkovatele prostředků. Ale neaktivuje nové auditu konfigurace v rámci virtuálního počítače.

### <a name="supported-client-types"></a>Podporované klientské typy

Následující tabulka uvádí seznam podporovaný operační systém v imagích Azure:

|Vydavatel|Název|Verze|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|credativ|Debian|8, 9|
|Microsoft|Windows Server|Datového centra 2012, 2012 R2 Datacenter, 2016 Datacenter|
|OpenLogic|CentOS|7.3, 7.4, 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|SuSE|SLES|12 SP3|

> [!IMPORTANT]
> Konfigurace typu Host můžete auditovat jakéhokoli serveru se systémem podporovaný operační systém.  Pokud chcete auditovat servery, které používají vlastní image, budete muset duplicitní **DeployIfNotExists** definice a upravovat **Pokud** část vaší vlastnosti bitové kopie.

### <a name="unsupported-client-types"></a>Nepodporované klientské typy

Následující tabulka uvádí operační systémy, které nejsou podporovány:

|Operační systém|Poznámky|
|-|-|
|Klient Windows | Klientské operační systémy (například Windows 7 a Windows 10) nejsou podporovány.
|Windows Server 2016 Nano Server | Nepodporuje se.|

### <a name="guest-configuration-extension-network-requirements"></a>Požadavky na síť hosta konfigurace rozšíření

Ke komunikaci s poskytovatelem prostředků konfigurace hostovaného v Azure, virtuální počítače vyžadují odchozí přístup k datovým centrům Azure na portu **443**. Pokud používáte privátní virtuální síť v Azure a nechcete povolit odchozí přenosy, musí být nakonfigurované výjimky pomocí [skupinu zabezpečení sítě](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) pravidla. V současné době neexistuje značku služby pro konfiguraci hosta zásad Azure.

Pro seznamy adres IP, si můžete stáhnout [Microsoft Azure rozsahů IP adres Datacentra](https://www.microsoft.com/download/details.aspx?id=41653). Tento soubor se každý týden aktualizuje a má aktuálně nasazené rozsahy a všechny nadcházející změny rozsahů IP adres. Potřebujete povolit odchozí přístup k IP adresy ve stejné oblasti, ve které jsou nasazené virtuální počítače.

> [!NOTE]
> Soubor XML adres Azure Datacenter IP obsahuje rozsahy IP adres, které se používají v datacentrech Microsoft Azure. Soubor obsahuje rozsahy compute, SQL a úložiště.
> Aktualizovaný soubor každý týden se zveřejňuje. Soubor odráží aktuálně nasazené rozsahy a všechny nadcházející změny rozsahů IP adres. Nové rozsahy, které se zobrazují v souboru nejsou používány v datových centrech alespoň jeden týden.
> Je vhodné Stáhněte nový soubor XML každý týden. Potom aktualizujte váš web pro zajištění správné identifikace služeb spuštěných v Azure. Uživatelé Azure ExpressRoute upozorňujeme ale, že tento soubor se používá k aktualizaci inzerování protokolu BGP (Border Gateway) prostoru Azure probíhá první týden každého měsíce.

## <a name="guest-configuration-definition-requirements"></a>Požadavky na konfiguraci hosta definice

Každý audit spuštění hosta konfigurace vyžaduje dvě definice zásad **DeployIfNotExists** definice a **auditu** definice. **DeployIfNotExists** definice slouží k přípravě virtuálního počítače s agentem hosta konfigurace a další komponenty pro podporu [ověřovacích nástrojů](#validation-tools).

**DeployIfNotExists** definici zásad ověří a řeší následující položky:

- Ověření virtuální počítač má přiřazenou konfiguraci, kterou chcete vyhodnotit. Pokud aktuálně neexistuje žádná přiřazení, získejte přiřazení a příprava virtuálního počítače podle:
  - Ověřování pomocí virtuálního počítače [spravované identity](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Instalace nejnovější verze **Microsoft.GuestConfiguration** rozšíření
  - Instalace [ověřovacích nástrojů](#validation-tools) a závislostí, v případě potřeby

Pokud **DeployIfNotExists** přiřazení je nekompatibilní, [úloha opravy](../how-to/remediate-resources.md#create-a-remediation-task) lze použít.

Jednou **DeployIfNotExists** přiřazení je kompatibilní, **auditu** přiřazení zásady používá nástroje pro místní ověřování k určení, zda je přiřazení konfigurace vyhovující nebo nevyhovující předpisům.
Nástroj ověření poskytuje výsledky klientovi Configuration hosta. Klient předává výsledky hosta rozšíření, které zpřístupní je prostřednictvím poskytovatele prostředků konfigurace hosta.

Služba Azure Policy používá poskytovatele prostředků hosta konfigurace **complianceStatus** vlastností na sestavu dodržování předpisů v **dodržování předpisů** uzlu. Další informace najdete v tématu [získávají data dodržování předpisů](../how-to/getting-compliance-data.md).

> [!NOTE]
> Pro každou definici typu Host konfigurace i **DeployIfNotExists** a **auditu** definice zásad musí existovat.

Všechny integrované zásady pro konfiguraci hosta jsou součástí iniciativy do definice pro použití v přiřazení skupiny. Předdefinované *[Preview]: Audit zabezpečení hesla uvnitř virtuálního počítače s Linuxem a Windows* iniciativy obsahuje 18 zásady. Obsahuje šest **DeployIfNotExists** a **auditu** páry definici zásady pro Windows a tři páry pro Linux.
U každé **DeployIfNotExists** [pravidlo definice zásad](definition-structure.md#policy-rule) omezuje systémy vyhodnocen.

## <a name="next-steps"></a>Další postup

- Projděte si příklady v [ukázek Azure Policy](../samples/index.md)
- Zkontrolujte [struktura definic zásad](definition-structure.md)
- Kontrola [Principy účinky zásad](effects.md)
- Pochopit postup [programové vytváření zásad](../how-to/programmatically-create.md)
- Zjistěte, jak [získat data o dodržování předpisů](../how-to/getting-compliance-data.md)
- Zjistěte, jak [opravit nekompatibilní prostředky](../how-to/remediate-resources.md)
- Připomenutí skupin pro správu v článku [Uspořádání prostředků pomocí skupin pro správu Azure](../../management-groups/index.md)
