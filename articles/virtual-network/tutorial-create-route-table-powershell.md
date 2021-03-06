---
title: Směrování síťového provozu Azure Powershellu | Dokumentace Microsoftu
description: V tomto článku se dozvíte, jak ke směrování síťového provozu s směrovací tabulky pomocí prostředí PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: ad09d41b004fe2b8a4090dce16a7a70f9c57b1f3
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56651495"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Směrování provozu sítě s směrovací tabulky pomocí Powershellu

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure ve výchozím nastavení automaticky směruje provoz mezi všemi podsítěmi v rámci virtuální sítě. Můžete vytvořit vlastní trasy a přepsat tak výchozí směrování Azure. Možnost vytvářet vlastní trasy je užitečná například v případě, že chcete směrovat provoz mezi podsítěmi přes síťové virtuální zařízení. V tomto článku získáte informace o těchto tématech:

* Vytvoření směrovací tabulky
* Vytvoření trasy
* Vytvoření virtuální sítě s několika podsítěmi
* Přidružení směrovací tabulky k podsíti
* Vytvoření síťového virtuálního zařízení, které směruje provoz
* Nasazení virtuálních počítačů do různých podsítí
* Směrování provozu z jedné podsítě do jiné přes síťové virtuální zařízení

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte modul Azure PowerShell verze 1.0.0 nebo novějším. Nainstalovanou verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable Az`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-az-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Connect-AzAccount` pro vytvoření připojení k Azure.

## <a name="create-a-route-table"></a>Vytvoření směrovací tabulky

Než vytvoříte směrovací tabulku, vytvořte skupinu prostředků s [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* pro všechny prostředky vytvořené v tomto článku.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Vytvoření směrovací tabulky s [New-AzRouteTable](/powershell/module/az.network/new-azroutetable). Následující příklad vytvoří směrovací tabulku s názvem *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Vytvoření trasy

Vytvořit trasu načtením objekt směrovací tabulky s [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable), vytvořit trasu s [přidat AzRouteConfig](/powershell/module/az.network/add-azrouteconfig), pak zápis konfigurace trasy do směrovací tabulky s [ Set-AzRouteTable](/powershell/module/az.network/set-azroutetable).

```azurepowershell-interactive
Get-AzRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Přidružení směrovací tabulky k podsíti

Než budete moct přidružit směrovací tabulky k podsíti, budete muset vytvořit virtuální síť a podsíť. Vytvoření virtuální sítě s [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Následující příklad vytvoří virtuální síť s názvem *myVirtualNetwork* s předponou adresy *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Vytvořte tři podsítě tak, že vytvoříte tři Konfigurace podsítí s [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Následující příklad vytvoří tři Konfigurace podsítí pro *veřejné*, *privátní*, a *DMZ* podsítě:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Zápis Konfigurace podsítí pro virtuální síť s [Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork), vytváří podsítí ve virtuální síti:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

Přidružit *myRouteTablePublic* tabulky tras, abyste *veřejné* podsíť s [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) a zapište konfiguraci podsítě, která virtuální síť s [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork).

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzVirtualNetwork
```

## <a name="create-an-nva"></a>Vytvoření síťového virtuálního zařízení

Síťové virtuální zařízení je virtuální počítač, který provádí síťovou funkci, jako je směrování, brána firewall nebo optimalizace sítě WAN.

Před vytvořením virtuálního počítače, vytvořte síťové rozhraní.

### <a name="create-a-network-interface"></a>Vytvořte síťové rozhraní

Před vytvořením síťového rozhraní, je nutné načíst virtuální sítě s Id [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork), pak podsíť Id s [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig). Vytvořte síťové rozhraní s [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) v *DMZ* podsítě s povoleným předáváním IP:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>Vytvoření virtuálního počítače

Chcete-li vytvořit virtuální počítač a připojit se k němu existující síťové rozhraní, musíte nejdřív vytvořit konfiguraci virtuálního počítače s [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig). Konfigurace zahrnuje síťové rozhraní vytvořené v předchozím kroku. Po zobrazení výzvy k zadání uživatelského jména a hesla, vyberte uživatelské jméno a heslo, které chcete se přihlásit k virtuálnímu počítači s.

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzVMNetworkInterface -Id $nic.Id
```

Vytvoření virtuálního počítače pomocí konfigurace virtuálního počítače s [rutiny New-AzVM](/powershell/module/az.compute/new-azvm). Následující příklad vytvoří virtuální počítač s názvem *myVmNva*.

```azurepowershell-interactive
$vmNva = New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

`-AsJob` Možnost se virtuální počítač vytvoří na pozadí, takže můžete pokračovat k dalšímu kroku.

## <a name="create-virtual-machines"></a>Vytvoření virtuálních počítačů

Vytvoření dvou virtuálních počítačů ve virtuální síti, abyste mohli ověřit provozu z *veřejné* podsítě se směruje na *privátní* podsítě přes síťové virtuální zařízení v pozdějším kroku.

Vytvářet virtuální počítače *veřejné* podsíť s [rutiny New-AzVM](/powershell/module/az.compute/new-azvm). Následující příklad vytvoří virtuální počítač s názvem *myVmPublic* v *veřejné* podsíti *myVirtualNetwork* virtuální sítě.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

Vytvářet virtuální počítače *privátní* podsítě.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

Vytvoření virtuálního počítače trvá několik minut. Nemusíte pokračovat dalším krokem, dokud se vytvoří virtuální počítač a prostředí PowerShell vrátí výstupní Azure.

## <a name="route-traffic-through-an-nva"></a>Směrování provozu přes síťové virtuální zařízení

Použití [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) vrátit veřejnou IP adresu *myVmPrivate* virtuálního počítače. Následující příklad vrátí veřejnou IP adresu *myVmPrivate* virtuálního počítače:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Použijte následující příkaz k vytvoření relace vzdálené plochy s *myVmPrivate* virtuálního počítače ze svého místního počítače. Nahraďte `<publicIpAddress>` IP adresou vrácenou předchozím příkazem.

```
mstsc /v:<publicIpAddress>
```

Otevřete stažený soubor RDP. Pokud se zobrazí výzva, vyberte **Připojit**.

Zadejte uživatelské jméno a heslo, které jste zadali při vytváření virtuálního počítače (abyste mohli zadat přihlašovací údaje, které jste zadali při vytváření virtuálního počítače, možná budete muset vybrat **Další možnosti** a pak **Použít jiný účet**), a pak vyberte **OK**. Během procesu přihlášení se může zobrazit upozornění certifikátu. Vyberte **Ano** a pokračujte v připojování.

V pozdějším kroku `tracert.exe` příkaz slouží k otestování směrování. Tracert používá ovládací prvek zpráva ICMP (Internet Protocol), který byl odepřen přes bránu Windows Firewall. Povolte průchod protokolu ICMP bránou Windows Firewall zadáním následujícího příkazu v PowerShellu na virtuálním počítači *myVmPrivate*:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

Přestože k otestování směrování v tomto článku se používá trasování tras, povolení průchodu protokolu ICMP bránou Windows Firewall v produkčních prostředích se nedoporučuje.

Jste povolili předávání IP v rámci Azure pro síťové rozhraní Virtuálního počítače v povolit předávání IP. Operační systém nebo aplikace spuštěná v rámci virtuálního počítače musí také být schopné směrovat síťový provoz. Povolení předávání IP v rámci operačního systému *myVmNva*.

Z příkazového řádku na *myVmPrivate* virtuálního počítače, připojení k vzdálené plochy *myVmNva*:

``` 
mstsc /v:myvmnva
```

Pokud chcete povolit předávání IP v rámci operačního systému, v PowerShellu na virtuálním počítači *myVmNva* zadejte následující příkaz:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```

Restartujte virtuální počítač *myVmNva*. Tím se také odpojí relace vzdálené plochy.

Zatímco jste stále připojeni k virtuálnímu počítači *myVmPrivate*, po restartování virtuálního počítače *myVmNva* vytvořte relaci vzdálené plochy k virtuálnímu počítači *myVmPublic*:

``` 
mstsc /v:myVmPublic
```

Povolte průchod protokolu ICMP bránou Windows Firewall zadáním následujícího příkazu v PowerShellu na virtuálním počítači *myVmPublic*:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Pokud chcete otestovat směrování síťového provozu z virtuálního počítače *myVmPublic* do virtuálního počítače *myVmPrivate*, v PowerShellu na virtuálním počítači *myVmPublic* zadejte následující příkaz:

```
tracert myVmPrivate
```

Odpověď bude podobná jako v následujícím příkladu:

```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:

1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4

Trace complete.
```

Jak vidíte, první segment směrování je 10.0.2.4, což je privátní IP adresa síťového virtuálního zařízení. Druhý segment směrování je 10.0.1.4, což je privátní IP adresa virtuálního počítače *myVmPrivate*. Trasa přidaná do směrovací tabulky *myRouteTablePublic* a přidružená k podsíti *Public* způsobila, že Azure směruje provoz přes síťové virtuální zařízení, a ne přímo do podsítě *Private*.

Ukončete relaci vzdálené plochy k virtuálnímu počítači *myVmPublic*. Stále zůstanete připojeni k virtuální síti *myVmPrivate*.

Pokud chcete otestovat směrování síťového provozu z virtuálního počítače *myVmPrivate* do virtuálního počítače *myVmPublic*, na příkazovém řádku na virtuálním počítači *myVmPrivate* zadejte následující příkaz:

```
tracert myVmPublic
```

Odpověď bude podobná jako v následujícím příkladu:

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:

1     1 ms     1 ms     1 ms  10.0.0.4

Trace complete.
```

Jak vidíte, provoz se směruje přímo z virtuálního počítače *myVmPrivate* do virtuálního počítače *myVmPublic*. Azure ve výchozím nastavení směruje provoz přímo mezi podsítěmi.

Ukončete relaci vzdálené plochy k virtuálnímu počítači *myVmPrivate*.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, použijte [odebrat AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup) k odebrání skupiny prostředků a všech prostředků, které obsahuje.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Další postup

V tomto článku jste vytvořili směrovací tabulku a přidruženou k podsíti. Vytvořili jste jednoduché síťové virtuální zařízení, které směrovalo provoz z veřejné podsítě do privátní podsítě. Nasadit různá předem nakonfigurovaná síťová virtuální zařízení, která provádí síťové funkce, jako jsou brány firewall a optimalizace sítě WAN z [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Další informace o směrování najdete v tématech [Přehled směrování](virtual-networks-udr-overview.md) a [Správa směrovací tabulky](manage-route-table.md).

Přestože v rámci virtuální sítě můžete nasadit řadu prostředků Azure, prostředky některých služeb Azure PaaS do virtuální sítě nasadit nejde. Přesto můžete omezit přístup k prostředkům některých služeb Azure PaaS pouze pro provoz z podsítě virtuální sítě. Další informace o postupu [omezení síťového přístupu k prostředkům PaaS](tutorial-restrict-network-access-to-resources-powershell.md).