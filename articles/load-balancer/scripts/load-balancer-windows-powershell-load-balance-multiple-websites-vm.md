---
title: Ukázka PowerShellu – vyrovnávání zatížení několika webů pomocí Azure PowerShellu | Microsoft Docs
description: Tento ukázkový skript Azure PowerShellu provádí vyrovnávání zatížení několika webů na stejném virtuálním počítači.
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: jeconnoc
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: kumud
ms.openlocfilehash: 6bedba2c68c05ee7ed6518996e54d007f3e83dc1
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56591632"
---
# <a name="azure-powershell-script-example-load-balance-multiple-websites"></a>Ukázkový skript Azure Powershellu: Vyvažovat zatížení u více webů

Tento ukázkový skript Azure PowerShellu vytvoří virtuální síť se dvěma virtuálními počítači, které jsou členy skupiny dostupnosti. Nástroj pro vyrovnávání zatížení přesměruje provoz pro dvě samostatné IP adresy na tyto dva virtuální počítače. Po spuštění skriptu můžete na tyto virtuální počítače nasadit software webového serveru a hostovat více webů, přičemž každý z nich bude mít vlastní IP adresu.

V případě potřeby nainstalujte Azure PowerShell podle pokynů uvedených v [příručce k Azure PowerShellu](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) a pak spuštěním rutiny `Connect-AzAccount` vytvořte připojení k Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spuštěním následujícího příkazu odeberte skupinu prostředků, virtuální počítač a všechny související prostředky.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá k vytvoření skupiny prostředků, virtuální sítě, nástroje pro vyrovnávání zatížení a všech souvisejících prostředků následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset) | Vytvoří skupinu dostupnosti Azure pro zajištění vysoké dostupnosti. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Vytvoří konfiguraci podsítě. Tato konfigurace se použije v procesu vytváření virtuální sítě. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Vytvoří virtuální síť. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Vytvoří veřejnou IP adresu. |
| [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | Vytvoří konfiguraci front-endových IP adres pro nástroj pro vyrovnávání zatížení. |
| [New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | Vytvoří konfiguraci back-endového fondu adres pro nástroj pro vyrovnávání zatížení. |
| [New-AzLoadBalancerProbeConfig](/powershell/module/az.network/new-azloadbalancerprobeconfig) | Vytvoří sondu NLB. Sonda NLB slouží k monitorování všech virtuálních počítačů v sadě NLB. Pokud nějaký virtuální počítač přestane být přístupný, nebude se do něj směrovat provoz. |
| [New-AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerruleconfig) | Vytvoří pravidlo NLB. V této ukázce se vytvoří pravidlo pro port 80. Když NLB přijme přenos HTTP, přesměruje ho na port 80 některého z virtuálních počítačů v sadě NLB. |
| [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer) | Vytvoří nástroj pro vyrovnávání zatížení. |
| [New-AzNetworkInterfaceIpConfig](/powershell/module/az.network/new-aznetworkinterfaceipconfig) | Definuje upřesňující funkce pro virtuální síťové rozhraní. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Vytvoří síťové rozhraní. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Vytvoří konfiguraci virtuálního počítače. Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a přihlašovací údaje pro správu. Tato konfigurace se použije při vytváření virtuálního počítače. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Vytvoří virtuální počítač. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Odebere skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další postup

Další informace o Azure PowerShellu najdete v [dokumentaci k Azure PowerShellu](https://docs.microsoft.com/powershell/azure/overview).

Další ukázkové skripty PowerShellu pro sítě najdete v [dokumentaci se základními informacemi k sítím Azure](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
