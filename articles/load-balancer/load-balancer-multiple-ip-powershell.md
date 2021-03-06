---
title: Vyrovnávání zatížení na více konfigurací protokolu IP – rozhraní příkazového řádku Azure
titlesuffix: Azure Load Balancer
description: Vyrovnávání zatížení napříč primární a sekundární konfigurací IP.
services: load-balancer
documentationcenter: na
author: anavinahar
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: annahar
ms.openlocfilehash: 1b130dff9d290dcce566570fd015d227d44a1310
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/01/2019
ms.locfileid: "57191897"
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>Vyrovnávání zatížení na více konfigurací protokolu IP pomocí Powershellu

> [!div class="op_single_selector"]
> * [Azure Portal](load-balancer-multiple-ip.md)
> * [Rozhraní příkazového řádku](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)


Tento článek popisuje, jak používat Azure Load Balancer s několika IP adresami na sekundární síťové rozhraní (NIC). V tomto scénáři máme dva virtuální počítače se systémem Windows, každý s primární a sekundární síťové rozhraní Každá sekundární síťová rozhraní obsahuje dvěma konfiguracemi IP. Každý virtuální počítač hostuje websites contoso.com a fabrikam.com. Každý z těchto webů je vázán na jednu z konfigurace protokolu IP na sekundární síťové rozhraní Nástroj pro vyrovnávání zatížení Azure používáme k vystavení dva front-endové IP adresy, jeden pro každý web, za účelem distribuce provozu do příslušné konfigurace protokolu IP pro web. Tento scénář používá stejné číslo portu mezi jak front-endů, tak i back-endový fond IP adres.

![Obrázek scénář LB](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Postup pro více konfigurací IP Vyrovnávání zatížení

Postupujte podle následujících kroků, abyste dosáhli scénář popsaný v tomto článku:

1. Nainstalujte Azure PowerShell. Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.
2. Vytvořte skupinu prostředků pomocí následujících nastavení:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Další informace najdete v kroku 2 [vytvořte skupinu prostředků](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Vytvořit skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) tak, aby obsahovala vaše virtuální počítače. V tomto scénáři použijte následující příkaz:

    ```powershell
    New-AzAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Postupujte podle pokynů kroky 3 až 5 v [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) článku Příprava vytvoření virtuálního počítače s jedním síťovým adaptérem. Provedení kroku 6.1 a použít místo kroku 6.2 následující:

    ```powershell
    $availset = Get-AzAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Dokončete [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) 6.3 prostřednictvím 6.8 kroky.

5. Přidáte druhá konfigurace IP adresy pro každý virtuální počítač. Postupujte podle pokynů v [přiřadit několik IP adres k virtuálním počítačům](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) článku. Použijte následující nastavení:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    Nemusíte sekundární konfigurace IP přidružení veřejné IP adresy pro účely tohoto kurzu. Upravte příkaz odebrat části přidružení veřejné IP.

6. Zopakujte kroky 4 až 6 tohoto článku pro VM2. Nezapomeňte nahradit název virtuálního počítače pro VM2 při tomto postupu. Všimněte si, že není nutné k vytvoření virtuální sítě pro druhý virtuální počítač. Může nebo nemůže vytvořit novou podsíť podle vašemu případu použití.

7. Vytvořte dvě veřejné IP adresy a uložit je do příslušných proměnných, jak je znázorněno:

    ```powershell
    $publicIP1 = New-AzPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. Vytvořte dvě konfigurace protokolu IP front-endu:

    ```powershell
    $frontendIP1 = New-AzLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. Vytvoření vaší back-endové fondy adres, testu a vaše pravidla Vyrovnávání zatížení:

    ```powershell
    $beaddresspool1 = New-AzLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Jakmile budete mít tyto prostředky, vytvořte nástroj pro vyrovnávání zatížení:

    ```powershell
    $mylb = New-AzLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. Přidejte nově vytvořený load balancer druhý back-end adres fondu a front-endové konfigurace protokolu IP:

    ```powershell
    $mylb = Get-AzLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzLoadBalancer

    $mylb | Add-AzLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzLoadBalancer
    
    Add-AzLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzLoadBalancer
    ```

12. Níže uvedených příkazů získat síťové karty a potom přidat obě konfigurace IP z každé sekundární síťové rozhraní do back-endového fondu adres nástroje pro vyrovnávání zatížení:

    ```powershell
    $nic1 = Get-AzNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzLoadBalancer

    $nic1 | Set-AzNetworkInterface
    $nic2 | Set-AzNetworkInterface
    ```

13. Nakonec musíte nakonfigurovat záznamy prostředků DNS tak, aby odkazoval na IP adresu odpovídající front-endu nástroje pro vyrovnávání zatížení. Může hostovat domény v Azure DNS. Další informace o použití Azure DNS s nástrojem pro vyrovnávání zatížení najdete v tématu [pomocí Azure DNS s ostatními službami Azure](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Další postup
- Další informace o tom, jak kombinací služeb Vyrovnávání zatížení v Azure v [pomocí služby Vyrovnávání zatížení v Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Zjistěte, jak pomocí různých typů protokolů v Azure pro správu a řešení potíží s nástroj pro vyrovnávání zatížení v [protokoly Azure monitoru pro Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).
