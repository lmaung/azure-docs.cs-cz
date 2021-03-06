---
title: Vytvoření Load Balanceru s zónově redundantních front-endu – Azure PowerShell
titlesuffix: Azure Load Balancer
description: Zjistěte, jak vytvořit veřejný Load balancer úrovně Standard s zónově redundantní veřejné IP adresy front-endu pomocí Powershellu
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: kumud
ms.openlocfilehash: aa042237eaf3afb219a7ac3260d6e16a77cc8719
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2019
ms.locfileid: "56671756"
---
#  <a name="create-a-standard-load-balancer-with-zone-redundant-frontend-using-azure-powershell"></a>Vytvoření Load Balanceru úrovně Standard s zónově redundantních front-endu pomocí Azure Powershellu

Tento článek popisuje postup vytvořením veřejné [Load balanceru úrovně Standard](https://aka.ms/azureloadbalancerstandard) s zónově redundantních front-endu pomocí standardní veřejné IP adresy.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

> [!NOTE]
 Podpora pro zóny dostupnosti je k dispozici pro vyberte prostředky Azure a oblasti a velikostní řady virtuálních počítačů. Další informace o tom, jak začít a které prostředky Azure, oblasti a velikostní řady virtuálních počítačů můžete vyzkoušet zóny dostupnosti s, najdete v části [přehledu zón dostupnosti](https://docs.microsoft.com/azure/availability-zones/az-overview). Pokud budete potřebovat podporu, můžete kontaktovat [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) nebo [otevřít lístek podpory Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k předplatnému Azure pomocí příkazu `Connect-AzAccount` a postupujte podle pokynů na obrazovce.

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí následujícího příkazu:

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Vytvoření veřejného standardní IP 
Vytvořte veřejné IP Standard pomocí následujícího příkazu:

```azurepowershell-interactive
$publicIp = New-AzPublicIpAddress -ResourceGroupName myResourceGroup -Name 'myPublicIP' `
  -Location westeurope -AllocationMethod Static -Sku Standard
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Vytvořte front-endovou konfiguraci protokolu IP pro web

Vytvoření konfigurace IP front-endu pomocí následujícího příkazu:

```azurepowershell-interactive
$feip = New-AzLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>Vytvoření fondu back endových adres

Vytvořte fond back-end adres pomocí následujícího příkazu:

```azurepowershell-interactive
$bepool = New-AzLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Vytvoření testu paměti nástroje pro vyrovnávání zatížení na portu 80

Vytvoření sondy stavu na portu 80 pro nástroj pro vyrovnávání zatížení pomocí následujícího příkazu:

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Vytvoření pravidla nástroje pro vyrovnávání zatížení
 Vytvořte pravidlo nástroje pro vyrovnávání zatížení pomocí následujícího příkazu:

```azurepowershell-interactive
   $rule = New-AzLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Vytvoření nástroje pro vyrovnávání zatížení
Vytvořte Standard pro vyrovnávání zatížení pomocí následujícího příkazu:

```azurepowershell-interactive
$lb = New-AzLoadBalancer -ResourceGroupName myResourceGroup -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Další postup
- Další informace o [Load balanceru úrovně Standard a dostupnost zóny](load-balancer-standard-availability-zones.md).