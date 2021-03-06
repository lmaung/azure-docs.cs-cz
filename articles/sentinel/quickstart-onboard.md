---
title: Začlenění ve verzi Preview Azure Sentinel | Dokumentace Microsoftu
description: Zjistěte, jak shromažďovat data v Azure Sentinelu.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: d5750b3e-bfbd-4fa0-b888-ebfab7d9c9ae
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 9c5f4c73bb516172773f6aad5e5393db6d40b3d5
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992912"
---
# <a name="on-board-azure-sentinel-preview"></a>Ve verzi Preview připojit Azure Sentinel

> [!IMPORTANT]
> Azure Sentinel je aktuálně ve verzi public preview.
> Tato verze Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro úlohy v produkčním prostředí. Některé funkce se nemusí podporovat nebo mohou mít omezené možnosti. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

V tomto rychlém startu se dozvíte, jak začlenit Sentinelu Azure. 

Do vlaku Sentinelu Azure musíte nejprve připojit ke zdrojům dat. Azure Sentinel se dodává s celou řadou konektorů pro Microsoft solutions, k dispozici z pole a poskytuje v reálném čase, integrace, včetně řešení ochrany před internetovými útoky Microsoft, Microsoft 365 zdrojů, včetně Office 365, Azure AD, Azure ATP a Microsoft Cloud App Security a další. Kromě toho jsou integrované konektory do širšího ekosystému zabezpečení pro řešení jiného subjektu než Microsoft. Můžete také použít běžný formát události Syslog nebo rozhraní REST API pro připojení zdroje dat pomocí Azure Sentinelu.  

Po připojení zdroje dat, vyberte si z Galerie odborně řídicí panely, které přehledy na základě vašich dat. Tyto řídicí panely můžete snadno přizpůsobit svým potřebám.


## <a name="global-prerequisites"></a>Globální požadavky

- Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

- Pracovní prostor log Analytics. Zjistěte, jak [vytvořit pracovní prostor Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md)

- Oprávnění přispěvatele pro vašeho tenanta umožňuje Azure Sentinelu

- Tenant globální nebo oprávnění správce zabezpečení

## <a name="enable-azure-sentinel"></a>Povolit Azure Sentinel

1. Přejděte na web Azure Portal.
2. Ujistěte se, že je vybrané předplatné, ve kterém se vytvoří Sentinelu Azure. 
3. Hledání Azure Sentinel. 
   ![search](./media/quickstart-onboard/search-product.png)

1. Klikněte na tlačítko **+ přidat**.
1. Vyberte pracovní prostor, který chcete použít nebo vytvořit nový. Můžete spustit ověřovací Azure na více než jednomu pracovnímu prostoru, ale data budou izolovaná do jednoho pracovního prostoru.

   ![hledat](./media/quickstart-onboard/choose-workspace.png)

  >[!NOTE] 
  > - **Umístění pracovního prostoru** je důležité pochopit, že všechna data můžete Streamovat do ověřovacích Azure je uložen v zeměpisné oblasti pracovního prostoru, který jste vybrali.  
  > - Výchozí pracovních prostorů vytvořených službou Azure Security Center se nezobrazí v seznamu. Azure Sentinelu nelze nainstalovat na ně.

6. Klikněte na tlačítko **přidat Azure Sentinel**.
  

## <a name="connect-data-sources"></a>Připojení zdroje dat

Azure Sentinel vytvoří připojení ke službám a aplikace připojuje ke službě a jejich předávání událostí a protokolů Sentinelu Azure. Pro počítače a virtuální počítače můžete nainstalovat agenta, který shromažďuje protokoly a předává je na Azure Sentinelu Sentinelu Azure. Brány firewall a proxy servery využívá Azure Sentinelu serveru protokolu Syslog v Linuxu. Je na něm nainstalován agent a ze které agent shromažďuje protokolu, souborů a předává je na Azure Sentinelu. 
 
1. Klikněte na tlačítko **shromažďování dat**.
2. Je dlaždice pro každý zdroj dat, které se můžete připojit.<br>
Klikněte například na **Azure Active Directory**. Pokud propojíte tento zdroj dat, streamování všechny protokoly z Azure AD do Azure Sentinelu. Můžete vybrat typ protokolů, které pokud chcete získat – protokoly přihlášení a/nebo protokoly auditu. <br>
V dolní části Sentinelu Azure poskytuje doporučení, pro které řídicí panely byste měli nainstalovat pro každý konektor je můžete okamžitě získat zajímavé přehledy napříč vašimi daty. <br> Postupujte podle pokynů k instalaci nebo [naleznete v Průvodci příslušné připojení](connect-data-sources.md) Další informace. Informace o datových konektorů najdete v tématu [připojení služby](connect-data-sources.md).

Za data, ke které jsou připojené zdroje vaše data spustí Streamovat do ověřovacích Azure a je připraven k zahájení práce s. Můžete zobrazit v protokolech [integrované řídicí panely](quickstart-get-visibility.md) a začněte vytvářet dotazy v Log Analytics pro [prozkoumat data](tutorial-investigate-cases.md).




## <a name="next-steps"></a>Další postup
V tomto dokumentu jste se dozvěděli o připojení zdroje dat k Sentinelu Azure. Další informace o Azure Sentinelu, naleznete v následujících článcích:
- Zjistěte, jak [umožňuje získat přehled vaše data a potenciální hrozby](quickstart-get-visibility.md).
- Začínáme [detekuje hrozby s využitím Azure Sentinelu](tutorial-detect-threats.md).
- Stream data z [běžný formát chyba zařízení](connect-common-event-format.md) do ověřovacích Azure.
