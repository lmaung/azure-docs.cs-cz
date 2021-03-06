---
title: Shromažďování dat ochrany ATP v programu Azure ve verzi Preview Sentinelu Azure | Dokumentace Microsoftu
description: Zjistěte, jak shromažďovat data služby Azure ATP v ověřovacích Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: 4415e8527852955f93402abb6aa068b9dd6b1197
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992905"
---
# <a name="collect-data-from-azure-advanced-threat-protection-atp"></a>Shromažďování dat z Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Azure Sentinel je aktuálně ve verzi public preview.
> Tato verze Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro úlohy v produkčním prostředí. Některé funkce se nemusí podporovat nebo mohou mít omezené možnosti. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Můžete Streamovat protokoly z [rozšířené ochrany před internetovými útoky pro Azure](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) do Azure Sentinelu jediným kliknutím.

## <a name="prerequisites"></a>Požadavky

- Uživatel se oprávnění správce zabezpečení nebo globální správce
- Musí být ve verzi private preview zákazníkem služby Azure ATP

## <a name="connect-to-azure-atp"></a>Připojení k Azure ATP

Ujistěte se, že je ve verzi private preview verze služby Azure ATP [povolena ve vaší síti](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Pokud nasazení ochrany ATP v programu Azure a ingestuje data podezřelé výstrahy můžete snadno Streamovat do ověřovacích Azure. Může trvat až 24 hodin pro výstrahy, které chcete spustit streamování do ověřovacích Azure.



1. V Azure Sentinelu, vyberte **shromažďování dat** a potom klikněte na tlačítko **ochrany ATP v programu Azure** dlaždici.

2. Klikněte na **Připojit**.


## <a name="next-steps"></a>Další postup
V tomto dokumentu jste zjistili, jak se připojit k Azure Sentinelu rozšířené ochrany před internetovými útoky pro Azure. Další informace o Azure Sentinelu, naleznete v následujících článcích:
- Zjistěte, jak [umožňuje získat přehled vaše data a potenciální hrozby](quickstart-get-visibility.md).
- Začínáme [detekuje hrozby s využitím Azure Sentinelu](tutorial-detect-threats.md).

