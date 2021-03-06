---
title: Shromažďování dat DNS ve verzi Preview Sentinelu Azure | Dokumentace Microsoftu
description: Zjistěte, jak shromažďovat data DNS v Azure Sentinelu.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 4687841beb848a147691bfffbcc2bce0feba0a4c
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56993094"
---
# <a name="connect-your-domain-name-server"></a>Připojení serveru název domény

> [!IMPORTANT]
> Azure Sentinel je aktuálně ve verzi public preview.
> Tato verze Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro úlohy v produkčním prostředí. Některé funkce se nemusí podporovat nebo mohou mít omezené možnosti. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Libovolný Server DNS (Domain Name) běží na Windows Azure Sentinelu se můžete připojit. To provádí instalací agenta na počítači DNS. Protokoly pomocí DNS, můžete získat zabezpečení, výkon a operace související informace o infrastruktuře DNS vaší organizace pomocí shromažďování, analýza, a korelace analýzy a protokoly auditu a dalších souvisejících dat ze serverů DNS.

Pokud povolíte shromažďování protokolů DNS můžete:
- Identifikace klientů, které se pokoušejí přeložit škodlivé domény názvy
- Identifikace zastaralých záznamů
- Identifikujte často doménový názvy a prahová klienti DNS
- Zobrazení žádosti o zatížení serverů DNS
- Selhání registrace DNS dynamického zobrazení

## <a name="how-it-works"></a>Jak to funguje

Po instalaci agenta do počítače DNS se provádí kolekce DNS. Agent si vyžádá události DNS a předává je do Log Analytics.

## <a name="connect-your-dns-appliance"></a>Připojit zařízení DNS

1. Na portálu Azure Sentinelu vyberte **shromažďování dat** a zvolte **DNS** dlaždici.
1. Pokud jsou vaše počítače DNS v Azure:
    1. Klikněte na tlačítko **stáhnout a nainstalovat agenta pro Windows virtual machines**.
    1. V **virtuálních počítačů** seznamu, vyberte počítač DNS chcete Streamovat do ověřovacích Azure. Ujistěte se, že jde o virtuálního počítače s Windows.
    1. V okně, které se otevře pro tento virtuální počítač, klikněte na tlačítko **připojit**.  
    1. Klikněte na tlačítko **povolit** v **DNS konektor** okna. 

2. Pokud váš počítač DNS není virtuální počítač Azure:
    1. Klikněte na tlačítko **stáhnout a nainstalovat agenta pro počítače mimo Azure Windows**.
    1. V **přímý agent** okna, vyberte buď **Windows stáhnout agenta (64bitová verze)** nebo **Windows stáhnout agenta (32 bitů)**.
    1. Nainstalujte agenta na počítači DNS. Kopírovat **ID pracovního prostoru**, **primární klíč**, a **sekundární klíč** a využít při zobrazení výzvy v průběhu instalace.

3. Chcete-li použít příslušné schéma v Log Analytics pro protokoly DNS, vyhledejte **DnsEvents**.

## <a name="validate"></a>Ověření 

Ve službě Log Analytics vyhledejte schématu **DnsEvents** a ujistěte se, že existují události.

## <a name="next-steps"></a>Další postup
V tomto dokumentu jste zjistili, jak se připojit na místním zařízení DNS k Sentinelu Azure. Další informace o Azure Sentinelu, naleznete v následujících článcích:
- Zjistěte, jak [umožňuje získat přehled vaše data a potenciální hrozby](quickstart-get-visibility.md).
- Začínáme [detekuje hrozby s využitím Azure Sentinelu](tutorial-detect-threats.md).
