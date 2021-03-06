---
title: Vytvoření clusteru Service Fabric s Windows v Azure | Dokumentace Microsoftu
description: V tomto kurzu se dozvíte, jak nasadit cluster Service Fabric s Windows do virtuální sítě Azure a skupiny zabezpečení sítě s použitím PowerShellu.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/19/2019
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 64ba17053179d428f5ef7e5ce9685240bde6665f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822987"
---
# <a name="tutorial-deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Kurz: Nasazení clusteru Service Fabric s Windows do virtuální sítě Azure

Tento kurz je první částí série. Naučíte se nasadit cluster Service Fabric s Windows do [virtuální sítě Azure](../virtual-network/virtual-networks-overview.md) a [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) s použitím PowerShellu a šablony. Po dokončení budete mít v cloudu spuštěný cluster, do kterého budete moct nasazovat aplikace.  Pokud chcete vytvořit cluster s Linuxem pomocí Azure CLI, přečtěte si téma [Vytvoření zabezpečeného clusteru s Linuxem v Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

Tento kurz popisuje produkční scénář.  Pokud chcete rychle vytvořit cluster menší pro účely testování, najdete v článku [vytvoření testovacího clusteru](./scripts/service-fabric-powershell-create-secure-cluster-cert.md).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuální sítě v Azure pomocí PowerShellu
> * Vytvoření trezoru klíčů a nahrání certifikátu
> * Nastavení ověřování Azure Active Directory
> * Vytvoření zabezpečeného clusteru Service Fabric v Azure PowerShellu
> * Zabezpečení clusteru pomocí certifikátu X.509
> * Připojení ke clusteru pomocí prostředí PowerShell
> * Odebrání clusteru

V této sérii kurzů se naučíte:
> [!div class="checklist"]
> * Vytvoření zabezpečeného clusteru v Azure
> * [Horizontální snížení nebo navýšení kapacity clusteru](service-fabric-tutorial-scale-cluster.md)
> * [Upgrade modulu runtime clusteru](service-fabric-tutorial-upgrade-cluster.md)
> * [Odstranění clusteru](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Požadavky

Než začnete s tímto kurzem:

* Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Nainstalujte sadu [Service Fabric SDK a modul PowerShellu](service-fabric-get-started.md).
* Nainstalujte [modul Azure PowerShellu verze 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps).
* Projděte si klíčové koncepty [Azure clusterů](service-fabric-azure-clusters-overview.md)

Následující postupy vytvářejí sedm uzly clusteru Service Fabric. Pokud chcete vypočítat náklady vzniklé používáním clusteru Service Fabric v Azure, použijte [cenovou kalkulačku funkcí Azure](https://azure.microsoft.com/pricing/calculator/).

## <a name="download-and-explore-the-template"></a>Stažení a prozkoumání šablony

Stáhněte si následující soubory šablon Resource Manageru:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Tato šablona nasadí zabezpečený cluster sedmi virtuálních počítačů a tři typy uzlů do virtuální sítě a skupinu zabezpečení sítě.  Další ukázkové šablony najdete na [GitHubu](https://github.com/Azure-Samples/service-fabric-cluster-templates).  Šablona [azuredeploy.json][template] nasadí řadu prostředků včetně následujících.

### <a name="service-fabric-cluster"></a>Cluster Service Fabric

V prostředku **Microsoft.ServiceFabric/clusters** se konfiguruje cluster s Windows s těmito charakteristikami:

* tři typy uzlů
* pět uzlů primárního typu (možnost konfigurace v parametrech šablony), jeden uzel v jiných typech dvěma uzly
* Operační systém: Windows Server 2016 Datacenter s kontejnery (možnost konfigurace v parametrech šablony)
* zabezpečení pomocí certifikátu (možnost konfigurace v parametrech šablony)
* [reverzní proxy server](service-fabric-reverseproxy.md) je povolen
* [služba DNS](service-fabric-dnsservice.md) je povolena
* bronzová [úroveň odolnosti](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (možnost konfigurace v parametrech šablony)
* stříbrná [úroveň spolehlivosti](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (možnost konfigurace v parametrech šablony)
* koncový bod připojení klienta: 19000 (možnost konfigurace v parametrech šablony)
* Koncový bod brány HTTP: 19080 (možnost konfigurace v parametrech šablony)

### <a name="azure-load-balancer"></a>Nástroj pro vyrovnávání zatížení Azure

V prostředku **Microsoft.Network/loadBalancers** dochází ke konfiguraci nástroje pro vyrovnávání zatížení a k nastavení sond a pravidel pro následující porty:

* koncový bod připojení klienta: 19000
* Koncový bod brány HTTP: 19080
* port aplikací: 80
* port aplikací: 443
* Service Fabric reverzního proxy serveru: 19081

Pokud jsou potřebné další porty aplikací, je potřeba upravit prostředky **Microsoft.Network/loadBalancers** a **Microsoft.Network/networkSecurityGroups**, aby byl povolený příchozí provoz.

### <a name="virtual-network-subnet-and-network-security-group"></a>Virtuální síť, podsíť a skupina zabezpečení sítě

Názvy virtuální sítě, podsítě a skupiny zabezpečení sítě jsou deklarované v parametrech šablony.  Adresní prostory virtuální sítě a podsítě se taky deklarují v parametrech šablony a konfigurují v prostředku **Microsoft.Network/virtualNetworks**:

* adresní prostor virtuální sítě: 172.16.0.0/20
* Adresní prostor podsítě Service Fabric: 172.16.2.0/23

V prostředku **Microsoft.Network/networkSecurityGroups** jsou povolená následující pravidla pro příchozí provoz. Hodnoty portů můžete změnit změnou proměnných šablony.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB: 445
* Internodecommunication – 1025, 1026, 1027
* Rozsah dočasných portů – 49152 až 65534 (potřebných je alespoň 256 portů)
* Porty pro použití aplikací: 80 a 443
* Rozsah portů aplikací – 49152 až 65534 (používají se pro komunikaci služeb, nejsou však otevírány v nástroji pro vyrovnávání zatížení)
* Všechny ostatní porty jsou blokované

Pokud jsou potřebné další porty aplikací, je potřeba upravit prostředky **Microsoft.Network/loadBalancers** a **Microsoft.Network/networkSecurityGroups**, aby byl povolený příchozí provoz.

### <a name="windows-defender"></a>Windows Defender
Ve výchozím nastavení [antivirové ochrany v programu Windows Defender](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016) je nainstalovaná a funkční v systému Windows Server 2016. Uživatelské rozhraní je nainstalované ve výchozím nastavení na některých jednotkách SKU, ale není potřeba.  Pro každý uzel typu/škálovací sady virtuálních počítačů deklarované v šabloně [Azure VM Antimalwarové rozšíření](/azure/virtual-machines/extensions/iaas-antimalware-windows) se používá k vyloučení adresáře Service Fabric a procesy:

```json
{
"name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
"properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
        "AntimalwareEnabled": "true",
        "Exclusions": {
                "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
        },
        "RealtimeProtectionEnabled": "true",
        "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
        }
        },
        "protectedSettings": null
}
}
```

## <a name="set-template-parameters"></a>Nastavení parametrů šablony

Soubor s parametry [azuredeploy.parameters.json][parameters] deklaruje mnoho hodnot používaných pro nasazení clusteru a přidružených prostředků. Některé parametry, které možná budete muset upravit pro své nasazení:

|Parametr|Příklad hodnoty|Poznámky|
|---|---||
|adminUserName|vmadmin| Uživatelské jméno správce pro cluster virtuálních počítačů.[Požadavky na uživatelské jméno pro virtuální počítač](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| Heslo správce pro virtuální počítače clusteru. [Požadavky na heslo pro virtuální počítač](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| Název clusteru. Může obsahovat jenom písmena a číslice. Může mít délku 3 až 23 znaků.|
|location|southcentralus| Umístění clusteru. |
|certificateThumbprint|| <p>Pokud vytváříte certifikát podepsaný svým držitelem nebo poskytujete soubor certifikátu, měla by být hodnota prázdná.</p><p>Pokud chcete použít existující certifikát, který se dříve odeslal do trezoru klíčů, vyplňte hodnotu kryptografického otisku certifikátu SHA1. Například: 6190390162C988701DB5676EB81083EA608DCCF3</p>. |
|certificateUrlValue|| <p>Pokud vytváříte certifikát podepsaný svým držitelem nebo poskytujete soubor certifikátu, měla by být hodnota prázdná. </p><p>Pokud chcete použít existující certifikát, který byl dříve odeslán do trezoru klíčů, vyplňte URL certifikátu. Například https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346.</p>|
|sourceVaultValue||<p>Pokud vytváříte certifikát podepsaný svým držitelem nebo poskytujete soubor certifikátu, měla by být hodnota prázdná.</p><p>Pokud chcete použít existující certifikát, který byl dříve odeslán do trezoru klíčů, vyplňte hodnotu zdrojového trezoru. Například: /subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT</p>|

## <a name="set-up-azure-active-directory-client-authentication"></a>Nastavení ověřování klienta služby Azure Active Directory
Pro clustery Service Fabric nasazené ve veřejné síti hostované v Azure se doporučení pro vzájemné ověřování uzel klienta:
* Použití Azure Active Directory pro identity klienta
* Certifikát pro server identity a šifrování SSL komunikaci pomocí protokolu http

Nastavení služby Azure AD pro ověřování klientů pro cluster Service Fabric je třeba provést před [vytváření clusteru](#createvaultandcert).  Azure AD umožňuje organizacím (označuje se jako tenantů) ke správě přístupu uživatelů k aplikacím. 

Cluster Service Fabric nabízí několik vstupních bodů do jeho funkce správy, včetně webová [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) a [sady Visual Studio](service-fabric-manage-application-in-visual-studio.md). Proto vytvoříte dvě aplikace Azure AD pro řízení přístupu ke clusteru: jeden webové aplikace a jedné nativní aplikace.  Po vytvoření aplikace můžete přiřadit uživatele jen pro čtení a role správce.

> [!NOTE]
> Před vytvořením clusteru, musíte dokončit následující kroky. Vzhledem k tomu, že skripty očekávají názvů clusterů a koncových bodů, hodnoty plánování byste měli využít a ne hodnoty, že jste již vytvořili.

V tomto článku předpokládáme, že jste již vytvořili tenanta. Pokud ne, začněte tím, že čtení [získání tenanta služby Azure Active Directory](../active-directory/develop/quickstart-create-new-tenant.md).

Pro zjednodušení některé kroky při konfiguraci Azure AD s clusterem Service Fabric, vytvořili jsme sadu skriptů prostředí Windows PowerShell. [Stáhněte si skripty](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool) k vašemu počítači.

### <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Vytvoření aplikace Azure AD a přiřazení uživatelů k rolím
Vytvořit dvě aplikace Azure AD pro řízení přístupu ke clusteru: jeden webové aplikace a jedné nativní aplikace. Po vytvoření aplikace pro reprezentaci clusteru přiřadit uživatelům, aby [role podporuje Service Fabric](service-fabric-cluster-security-roles.md): jen pro čtení a správce.

Spustit `SetupApplications.ps1`a zadat tenanta jako parametry ID, název clusteru a adresy URL odpovědi webové aplikace.  Také zadejte uživatelská jména a hesla pro uživatele.  Příklad:

```PowerShell
$Configobj = .\SetupApplications.ps1 -TenantId '<MyTenantID>' -ClusterName 'mysfcluster123' -WebApplicationReplyUrl 'https://mysfcluster123.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Pro národní cloudy (například Azure Government, Azure China, Azure Germany), by měly také určit `-Location` parametru.

Můžete najít vaše *TenantId*, nebo ID adresáře v [webu Azure portal](https://portal.azure.com). Vyberte **Azure Active Directory -> vlastnosti** a zkopírujte **ID adresáře** hodnotu.

*Název clusteru* používat jako předpona aplikací v Azure AD, které jsou vytvořeny skriptem. Nemusí přesně odpovídat skutečný název clusteru. Je určena pouze k usnadňují mapovat do clusteru Service Fabric, který se právě používají s artefakty Azure AD.

*WebApplicationReplyUrl* je výchozí koncový bod, který Azure AD vrací uživatelům po jejich dokončení přihlašování. Nastavte jako koncový bod Service Fabric Exploreru pro váš cluster, který ve výchozím nastavení je tento koncový bod:

https://&lt;cluster_domain&gt;:19080/Explorer

Zobrazí se výzva k přihlášení k účtu, který má oprávnění správce pro tenanta Azure AD. Po přihlášení, skript vytvoří webové a nativní aplikace pro reprezentaci vašeho clusteru Service Fabric. Když se podíváte na aplikace vašeho tenanta v [webu Azure portal](https://portal.azure.com), měli byste vidět dvě nové položky:

   * *Název clusteru*\_clusteru
   * *Název clusteru*\_klienta

Skript vypíše soubor JSON potřebný pomocí šablony Azure Resource Manageru při vytváření clusteru, takže je vhodné ponechat otevřené okno Powershellu.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Přidat konfiguraci Azure AD, které používají Azure AD pro klientský přístup
V [azuredeploy.json][template], nakonfigurujte v Azure AD **Microsoft.ServiceFabric/clusters** části.  Přidat parametry pro tenanta, ID aplikace ID clusteru a ID klienta aplikace.  

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...

    "aadTenantId": {
      "type": "string",
      "defaultValue": "0e3d2646-78b3-4711-b8be-74a381d9890c"
    },
    "aadClusterApplicationId": {
      "type": "string",
      "defaultValue": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
    },
    "aadClientApplicationId": {
      "type": "string",
      "defaultValue": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
    }
  },

...

{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

Přidání hodnoty parametrů v [azuredeploy.parameters.json] [ parameters] soubor parametrů.  Příklad:

```json
"aadTenantId": {
"value": "0e3d2646-78b3-4711-b8be-74a381d9890c"
},
"aadClusterApplicationId": {
"value": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
},
"aadClientApplicationId": {
"value": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
}
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Nasazení virtuální sítě a clusteru

Dále nastavte topologii sítě a nasaďte cluster Service Fabric. Šablona Resource Manageru [azuredeploy.json][template] vytvoří virtuální síť a také podsíť a skupinu zabezpečení sítě pro Service Fabric. Šablona také nasadí cluster s povoleným zabezpečením pomocí certifikátu.  Pro produkční clustery používejte certifikát clusteru od certifikační autority (CA). K zabezpečení testovacích clusterů můžete použít certifikát podepsaný svým držitelem.

Šablona v tomto článku se nasadí cluster, který používá kryptografický otisk certifikátu pro identifikaci certifikátu clusteru.  Žádné dva certifikáty můžou mít se stejným kryptografickým otiskem, což znesnadňuje správy certifikátů. Přepínání nasazeném clusteru pomocí kryptografické otisky certifikátů k běžnému názvu certifikátu pomocí certifikátu značně zjednodušuje správu.  Zjistěte, jak aktualizovat cluster používat běžné názvy certifikátů pro správu certifikátů, přečtěte si téma [clusteru změnit na společný název správy certifikátů](service-fabric-cluster-change-cert-thumbprint-to-cn.md).

### <a name="create-a-cluster-using-an-existing-certificate"></a>Vytvoření clusteru s použitím existujícího certifikátu

Následující skript pomocí rutiny [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) a šablony nasadí nový cluster do Azure. Rutina také vytvoří v Azure nový trezor klíčů a odešle váš certifikát.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # must match the clusterName parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Vytvoření clusteru s použitím nového certifikátu podepsaného svým držitelem

Následující skript pomocí rutiny [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) a šablony nasadí nový cluster do Azure. Rutina také vytvoří v Azure nový trezor klíčů, přidá do něj nový certifikát podepsaný svým držitelem a místně stáhne soubor certifikátu.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Připojení k zabezpečenému clusteru

Připojte se ke clusteru pomocí modulu PowerShellu pro Service Fabric nainstalovaného spolu se sadou Service Fabric SDK.  Nejprve nainstalujte certifikát do osobního úložiště (Moje) aktuálního uživatele na počítači.  Spusťte následující příkaz PowerShellu:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Nyní jste připraveni připojit se ke svému zabezpečenému clusteru.

Modul PowerShellu pro **Service Fabric** poskytuje řadu rutin pro správu clusterů Service Fabric, aplikací a služeb.  Pomocí rutiny [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) se připojte k zabezpečenému clusteru. Podrobnosti o kryptografickém otisku certifikátu SHA1 a koncovém bodu připojení se nacházejí ve výstupu z předchozího kroku.

Pokud jste dříve nastavili ověření klienta AAD, spusťte následující příkaz: 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
        -KeepAliveIntervalInSec 10 `
        -AzureActiveDirectory `
        -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10
```

Pokud jste nenastavili ověření klienta AAD, spusťte následující příkaz:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Pomocí rutiny [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) zkontrolujte, že jste připojeni a cluster je v pořádku.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

V dalších článcích v této sérii kurzů se používá cluster, který jste právě vytvořili. Pokud nechcete ihned pokračovat dalším článkem, můžete [cluster odstranit](service-fabric-cluster-delete.md), aby se vám neúčtovaly poplatky.

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření virtuální sítě v Azure pomocí PowerShellu
> * Vytvoření trezoru klíčů a nahrání certifikátu
> * Nastavení ověřování Azure Active Directory
> * Vytvoření zabezpečeného clusteru Service Fabric v Azure pomocí PowerShellu
> * Zabezpečení clusteru pomocí certifikátu X.509
> * Připojení ke clusteru pomocí prostředí PowerShell
> * Odebrání clusteru

Dále přejděte k následujícímu kurzu a naučte se svůj cluster škálovat.
> [!div class="nextstepaction"]
> [Škálování clusteru](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
