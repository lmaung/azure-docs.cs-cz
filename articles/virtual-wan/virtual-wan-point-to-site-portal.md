---
title: Vytvoření připojení typu point-to-site k Azure pomocí služby Azure Virtual WAN | Microsoft Docs
description: V tomto kurzu zjistíte, jak pomocí služby Azure Virtual WAN vytvořit připojení VPN typu point-to-site k Azure.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: alzam
Customer intent: As someone with a networking background, I want to connect remote users to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: 87b8543d8cb658b46ab5e589a310a17a69508a47
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411386"
---
# <a name="tutorial-create-a-point-to-site-connection-using-azure-virtual-wan-preview"></a>Kurz: Vytvoření připojení typu Point-to-Site pomocí Azure virtuální sítě WAN (Preview)

V tomto kurzu se dozvíte, jak se pomocí služby Virtual WAN připojit ke svým prostředkům v Azure přes připojení VPN IPsec/IKE (IKEv2) nebo OpenVPN. Tento typ připojení vyžaduje, aby byl na klientském počítači nakonfigurovaný klient. Další informace o službě Virtual WAN najdete v tématu [Přehled služby Virtual WAN](virtual-wan-about.md).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření sítě WAN
> * Vytvoření konfigurace P2S
> * Vytvoření rozbočovače
> * Použití konfigurace P2S na rozbočovač
> * Připojení virtuální sítě k rozbočovači
> * Stažení a použití konfigurace klienta VPN
> * Zobrazení virtuální sítě WAN
> * Zobrazení stavu prostředků
> * Monitorování připojení

> [!IMPORTANT]
> Tato verze Public Preview se poskytuje bez smlouvy o úrovni služeb a neměla by se používat pro úlohy v produkčním prostředí. Některé funkce nemusí být podporované, můžou mít omezené možnosti nebo nemusí být dostupné ve všech umístěních Azure. Podrobnosti najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Před zahájením

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Registrace této funkce

Kliknutím na **Vyzkoušet** zaregistrujete tuto funkci snadno pomocí prostředí Azure Cloud Shell. Pokud by místo toho spustíte PowerShell místně, ujistěte se, že máte nejnovější verzi a přihlaste se pomocí **Connect-AzureRmAccount** a **Select-AzureRmSubscription** příkazy.

>[!NOTE]
>Pokud tuto funkci nezaregistrujete, nebudete ji moci použít ani ji neuvidíte na portálu.
>
>

Po kliknutí na **Vyzkoušet** se otevře Azure Cloud Shell. Zkopírujte a vložte následující příkazy:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```
 
```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Jakmile bude tato funkce zaregistrovaná, znovu zaregistrujte předplatné do oboru názvů Microsoft.Network.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Vytvoření virtuální sítě

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Vytvoření virtuální sítě WAN

V prohlížeči přejděte na [Azure Portal (Preview)](https://aka.ms/azurevirtualwanpreviewfeatures) a přihlaste se pomocí svého účtu Azure.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Vytvoření rozbočovače

> [!NOTE]
> V tomto kroku nevybírejte nastavení Zahrnout bránu point-to-site.
>

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="site"></a>4. Vytvoření konfigurace P2S

Konfigurace P2S definuje parametry pro připojení vzdálených klientů.

1. Přejděte na **All resources** (Všechny prostředky).
2. Klikněte na virtuální síť WAN, kterou jste vytvořili.
3. V části **Architektura virtuální sítě WAN** klikněte na **Konfigurace point-to-site**.
4. Kliknutím na **+ Přidat konfiguraci point-to-site** v horní části okna otevřete stránku **Vytvořit novou konfiguraci point-to-site**.
5. Na stránce **Vytvořit novou konfiguraci point-to-site** vyplňte následující pole:

  *  **Název konfigurace** – Název, kterým chcete na konfiguraci odkazovat.
  *  **Typ tunelu** – Protokol, který se má použít pro tunel.
  *  **Fond adres** – Fond IP adres, ze kterého se klientům budou přiřazovat IP adresy.
  *  **Název kořenového certifikátu** – Popisný název certifikátu.
  *  **Data kořenového certifikátu** – Data certifikátu X.509 s kódováním Base-64.

5. Kliknutím na **Vytvořit** vytvořte konfiguraci.

## <a name="hub"></a>5. Úprava přiřazení k rozbočovači

1. Na stránce virtuální sítě WAN klikněte na **Hubs** (Rozbočovače).
2. Vyberte rozbočovač, ke kterému chcete konfiguraci point-to-site přiřadit.
3. Klikněte na **...** a vyberte **Edit virtual hub** (Upravit virtuální rozbočovač).
4. Zaškrtněte možnost **Include point-to-site gateway** (Zahrnout bránu point-to-site).
5. Vyberte **Gateway scale units** (Jednotky škálování brány) a pro klienty vyberte **Point-to-site configuration** (Konfigurace point-to-site) a **Address pool** (Fond adres).
6. Klikněte na **Confirm** (Potvrdit). 
7. Operace může trvat až 30 minut.

## <a name="vnet"></a>6. Připojení virtuální sítě k rozbočovači

V tomto kroku vytvoříte partnerské připojení mezi rozbočovačem a určitou virtuální sítí. Uvedený postup zopakujte pro všechny virtuální sítě, které chcete připojit.

1. Na stránce vaší virtuální sítě WAN klikněte na **Virtual network connection** (Připojení k virtuální síti).
2. Na stránce připojení k virtuální síti klikněte na **+Add connection** (Přidat připojení).
3. Na stránce **Add connection** (Přidat připojení) zadejte údaje do následujících polí:

    * **Connection name** (Název připojení) – zadejte název připojení.
    * **Hubs** (Rozbočovače) – vyberte rozbočovač, který chcete k tomuto připojení přidružit.
    * **Subscription** (Předplatné) – ověřte předplatné.
    * **Virtual network** (Virtuální síť) – vyberte virtuální síť, kterou chcete připojit k tomuto rozbočovači. Virtuální síť nesmí mít existující bránu virtuální sítě.

## <a name="device"></a>7. Stažení profilu sítě VPN

Pomocí profilu sítě VPN nakonfigurujte své klienty.

1. Na stránce virtuální sítě WAN klikněte na **Hubs** (Rozbočovače).
2. Vyberte rozbočovač, pro který chcete profil stáhnout.
3. Klikněte na **...** a vyberte **Download profile** (Stáhnout profil). 
4. Jakmile se dokončí vytváření souboru, můžete ho kliknutím na odkaz stáhnout.
4. Pomocí souboru profilu nakonfigurujte klienty point-to-site.

## <a name="device"></a>8. Konfigurace klientů point-to-site
Pomocí staženého profilu nakonfigurujte klienty pro vzdálený přístup. Postup se v jednotlivých operačních systémech liší, postupujte tedy podle odpovídajících pokynů uvedených níže:

### <a name="microsoft-windows"></a>Microsoft Windows
#### <a name="openvpn"></a>OpenVPN

1.  Z oficiálního webu si stáhněte klienta OpenVPN a nainstalujte ho.
2.  Stáhněte si profil sítě VPN pro bránu. To můžete udělat na kartě Konfigurace Point-to-site na webu Azure portal nebo New-AzureRmVpnClientConfiguration v prostředí PowerShell.
3.  Rozbalte profil. V Poznámkovém bloku otevřete konfigurační soubor vpnconfig.ovpn ze složky OpenVPN.
4.  V části klientského certifikátu P2S vyplňte veřejný klíč klientského certifikátu P2S v kódování Base-64. V případě certifikátu s formátem PEM stačí otevřít soubor .cer a zkopírovat klíč Base-64 uvedený mezi hlavičkami certifikátu. Tady zjistíte, jak exportovat certifikát a získat zakódovaný veřejný klíč.
5.  V části privátního klíče vyplňte privátní klíč klientského certifikátu P2S v kódování Base-64. Tady zjistíte, jak extrahovat privátní klíč.
6.  Ostatní pole ponechte beze změny. S použitím vyplněné konfigurace ve vstupu klienta se připojte k síti VPN.
7.  Zkopírujte soubor vpnconfig.ovpn do složky C:\Program Files\OpenVPN\config.
8.  Klikněte pravým tlačítkem na ikonu OpenVPN na hlavním panelu a klikněte na Připojit.

#### <a name="ikev2"></a>IKEv2

1. Vyberte konfigurační soubory klienta VPN, které odpovídají architektuře počítače s Windows. V případě 64bitové architektury procesoru zvolte instalační balíček VpnClientSetupAmd64. V případě 32bitové architektury procesoru zvolte instalační balíček VpnClientSetupX86.
2. Dvakrát klikněte na balíček a nainstalujte ho. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na Další informace a pak na Přesto spustit.
3. Na klientském počítači přejděte do Nastavení sítě a klikněte na Síť VPN. Připojení k síti VPN zobrazuje název virtuální sítě, ke které se připojuje.
4. Než se pokusíte o připojení, ověřte, že jste na klientském počítači nainstalovali klientský certifikát. Klientský certifikát se vyžaduje k ověřování při použití typu nativního ověřování certifikátů Azure. Další informace o generování certifikátů najdete v tématu Generování certifikátů. Informace o instalaci klientského certifikátu najdete v tématu Instalace klientského certifikátu.

### <a name="mac-os-x"></a>Mac (OS X)
#### <a name="openvpn"></a>OpenVPN

1.  Stáhněte a nainstalujte si klienta OpenVPN, například TunnelBlik z webu https://tunnelblick.net/downloads.html. 
2.  Stáhněte si profil sítě VPN pro bránu. To můžete udělat na kartě Konfigurace Point-to-site na webu Azure portal nebo New-AzureRmVpnClientConfiguration v prostředí PowerShell.
3.  Rozbalte profil. V Poznámkovém bloku otevřete konfigurační soubor vpnconfig.ovpn ze složky OpenVPN.
4.  V části klientského certifikátu P2S vyplňte veřejný klíč klientského certifikátu P2S v kódování Base-64. V případě certifikátu s formátem PEM stačí otevřít soubor .cer a zkopírovat klíč Base-64 uvedený mezi hlavičkami certifikátu. Tady zjistíte, jak exportovat certifikát a získat zakódovaný veřejný klíč.
5.  V části privátního klíče vyplňte privátní klíč klientského certifikátu P2S v kódování Base-64. Tady zjistíte, jak extrahovat privátní klíč.
6.  Ostatní pole ponechte beze změny. S použitím vyplněné konfigurace ve vstupu klienta se připojte k síti VPN.
7.  Dvakrát klikněte na soubor profilu a vytvořte profil v nástroji Tunnelblik.
8.  Spusťte Tunnelblik ze složky Aplikace.
9.  Klikněte na ikonu nástroje Tunnelblik na hlavním panelu a vyberte Připojit.

#### <a name="ikev2"></a>IKEv2

Azure neposkytuje soubor mobileconfig pro nativní ověřování certifikátů Azure. Na každém počítači Mac, který se bude připojovat k Azure, je potřeba ručně nakonfigurovat nativního klienta IKEv2 VPN. Všechny potřebné informace ke konfiguraci najdete ve složce Generic. Pokud složku Generic mezi staženými soubory nevidíte, pravděpodobně jste jako typ tunelu nevybrali IKEv2. Jakmile vyberete IKEv2, znovu vygenerujte soubor zip a načtěte složku Generic. Složka Generic obsahuje následující soubory:

- VpnSettings.xml – obsahuje důležitá nastavení, jako je adresa serveru a typ tunelu.
- VpnServerRoot.cer – obsahuje kořenový certifikát požadovaný k ověření služby Azure VPN Gateway při nastavování připojení P2S.

## <a name="viewwan"></a>9. Zobrazení virtuální sítě WAN

1. Přejděte na virtuální síť WAN.
2. Na stránce Overview (Přehled) každý bod na mapě představuje jeden rozbočovač. Podržením ukazatele na některém z těchto bodů zobrazíte souhrn stavu rozbočovače.
3. V části Hubs and connections (Rozbočovače a připojení) můžete zjistit stav rozbočovače, lokalitu, oblast, stav připojení VPN a přijaté a odeslané bajty.

## <a name="viewhealth"></a>10. Zobrazení stavu prostředků

1. Přejděte na svoji síť WAN.
2. Na stránce sítě WAN v části **SUPPORT + Troubleshooting** (Podpora a řešení potíží) klikněte na **Health** (Stav) a prohlédněte si stav svého prostředku.

## <a name="connectmon"></a>11. Monitorování připojení

Vytvořte připojení pro monitorování komunikace mezi virtuálním počítačem Azure a vzdálenou lokalitou. Informace o tom, jak nastavit monitorování připojení, najdete v článku [Monitorování síťové komunikace](~/articles/network-watcher/connection-monitor.md). Do pole zdroje zadejte IP adresu virtuálního počítače v Azure a cílovou IP adresou je IP adresa lokality.

## <a name="cleanup"></a>12. Vyčištění prostředků

Pokud už tyto prostředky nepotřebujete, můžete k odebrání skupiny prostředků a všech prostředků, které obsahuje, použít rutinu [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). Položku myResourceGroup nahraďte názvem vaší skupiny prostředků a spusťte následující příkaz PowerShellu:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření sítě WAN
> * Vytvoření lokality
> * Vytvoření rozbočovače
> * Připojení rozbočovače k lokalitě
> * Připojení virtuální sítě k rozbočovači
> * Stažení a použití konfigurace zařízení VPN
> * Zobrazení virtuální sítě WAN
> * Zobrazení stavu prostředků
> * Monitorování připojení

Další informace o službě Virtual WAN najdete v článku [Přehled služby Virtual WAN](virtual-wan-about.md).
