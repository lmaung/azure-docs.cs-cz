---
title: Shromažďovat data Syslogu ve verzi Preview Sentinelu Azure | Dokumentace Microsoftu
description: Zjistěte, jak shromažďovat data protokolu Syslog v ověřovacích Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 51549f1cc07569ac6474f2dd1a8d9a7adeee5de9
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56993010"
---
# <a name="connect-your-external-solution-using-syslog"></a>Připojení externích řešení pomocí protokolu Syslog

> [!IMPORTANT]
> Azure Sentinel je aktuálně ve verzi public preview.
> Tato verze Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro úlohy v produkčním prostředí. Některé funkce se nemusí podporovat nebo mohou mít omezené možnosti. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Všechny místní zařízení, která podporuje Syslog Sentinelu Azure se můžete připojit. To se provádí pomocí agenta založené na počítači s Linuxem mezi zařízením a Sentinelu Azure. Pokud je počítač s Linuxem v Azure, můžete Streamovat protokoly ze zařízení nebo aplikaci k vyhrazený pracovní prostor vytváření v Azure a jejím připojení. Pokud není počítač s Linuxem v Azure, můžete Streamovat protokoly z vašich zařízení k vyhrazené místního virtuálního počítače nebo počítač, na který instalujete agenta pro Linux. 

> [!NOTE]
> Pokud vaše zařízení podporuje formát CEF Syslog, připojení je kompletní a měli byste tuto možnost zvolte a postupujte podle pokynů v [shromažďování dat z formátu CEF](connect-common-event-format.md).

## <a name="how-it-works"></a>Jak to funguje

Shromažďování Syslogu se provádí pomocí agenta pro Linux. Ve výchozím nastavení agenta pro Linux přijímá události z démona Syslog přes protokol UDP, ale v případech, kde se počítače s Linuxem očekává shromažďovat k velkému počtu události procesu Syslog, jako když agenta pro Linux je přijímáte události z jiných zařízení, konfigurace je upravit tak, aby pomocí přenosu protokolu TCP mezi démona Syslogu a agenta.

## <a name="connect-your-syslog-appliance"></a>Připojit zařízení Syslog

1. Na portálu Azure Sentinelu vyberte **shromažďování dat** a zvolte **Syslog** dlaždici.
2. Pokud není počítač s Linuxem v Azure, stáhněte a nainstalujte Azure Sentinelu **agenta pro Linux** na vaše zařízení. 
1. Pokud pracujete v Azure, vyberte nebo vytvořte virtuální počítač, který v rámci pracovního prostoru Sentinelu Azure, který je vyhrazen pro příjem zprávy Syslog. Vyberte virtuální počítač v Azure ověřovacích pracovních prostorech a klikněte na tlačítko **připojit** v horní části levého podokna.
3. Klikněte na tlačítko **konfigurace protokolů, které se mají shromažďovat** zpět v nastavení konektoru Syslog. 
4. Klikněte na tlačítko **stiskněte sem a otevřete tak okno Konfigurace**.
1. Vyberte **Data** a potom **Syslog**.
   - Zajistěte, aby každé zařízení, které e-mail posíláte podle Syslog je v tabulce. Pro každé zařízení chcete monitorovat, nastavit závažnost. Klikněte na tlačítko **Použít**.
1. Ve vašem počítači Syslog Ujistěte se, že posíláte těchto zařízení. 

3. Použít příslušné schéma v Log Analytics pro protokoly Syslog, vyhledejte **Syslog**.




## <a name="next-steps"></a>Další postup
V tomto dokumentu jste zjistili, jak se připojit Syslog na místní zařízení k Azure Sentinelu. Další informace o Azure Sentinelu, naleznete v následujících článcích:
- Zjistěte, jak [umožňuje získat přehled vaše data a potenciální hrozby](quickstart-get-visibility.md).
- Začínáme [detekuje hrozby s využitím Azure Sentinelu](tutorial-detect-threats.md).
