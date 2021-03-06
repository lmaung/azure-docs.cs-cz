---
title: Shromažďování dat Office 365 ve verzi Preview Sentinelu Azure | Dokumentace Microsoftu
description: Zjistěte, jak shromažďovat data Office 365 v ověřovacích Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ff7c862e-2e23-4a28-bd18-f2924a30899d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 42d2bb69ac5f1e6669d82a3fc7236e967a6d9366
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992919"
---
# <a name="collect-data-from-office-365-logs"></a>Shromažďovat data z Office 365 protokolů

> [!IMPORTANT]
> Azure Sentinel je aktuálně ve verzi public preview.
> Tato verze Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro úlohy v produkčním prostředí. Některé funkce se nemusí podporovat nebo mohou mít omezené možnosti. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Můžete Streamovat protokoly auditu z [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) do Azure Sentinelu jediným kliknutím. Protokoly auditu z více tenantů do jednoho pracovního prostoru v ověřovacích Azure můžete Streamovat. Konektor protokolů aktivit Office 365 poskytuje podrobné informace o probíhající uživatelských aktivit. Zobrazí se informace o různé uživatele, správce, systému a akce zásad a události z Office 365. Propojením protokolů Office 365 do ověřovacích Azure slouží tato data k zobrazení řídicích panelů, vytvářet vlastní výstrahy a zlepšení procesu šetření.


## <a name="prerequisites"></a>Požadavky

- Musíte být globálním správcem nebo správcem zabezpečení ve svém tenantovi


## <a name="connect-to-office-365"></a>Připojení k Office 365

1. V Azure Sentinelu, vyberte **shromažďování dat** a potom klikněte na tlačítko **Office 365** dlaždici.

2. Pokud jste ještě nepovolili, v části **připojení** použít **povolit** tlačítko povolíte řešení Office 365. Pokud je už povolená, budou označeny v okně připojení, které jsou už povolená.
1. Office 365 umožňuje streamování dat z více tenantů do Azure Sentinelu. Pro každého klienta, které se chcete připojit, přidání tenanta pod **připojení klientů k Azure Sentinelu**. 
1. Otevře se obrazovka Active Directory. Zobrazí se výzva k ověření uživatele globální správce na každého klienta, který chcete připojit k Azure Sentinelu a poskytnout oprávnění Azure ověřovací protokoly číst. 
5. V části protokoly aktivit Stream Office 365, klikněte na **vyberte** zvolit typy protokolů, které chcete Streamovat do ověřovacích Azure. V současné době Sentinelu Azure podporuje Exchange a SharePoint.

4. Klikněte na tlačítko **použít změny**.

3. Chcete-li použít příslušné schéma v Log Analytics pro protokoly služeb Office 365, vyhledejte **OfficeActivity**.


## <a name="next-steps"></a>Další postup
V tomto dokumentu jste zjistili, jak se připojit k Azure Sentinelu Office 365. Další informace o Azure Sentinelu, naleznete v následujících článcích:
- Zjistěte, jak [umožňuje získat přehled vaše data a potenciální hrozby](quickstart-get-visibility.md).
- Začínáme [detekuje hrozby s využitím Azure Sentinelu](tutorial-detect-threats.md).

