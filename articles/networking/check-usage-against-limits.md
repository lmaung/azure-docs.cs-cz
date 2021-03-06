---
title: Kontrola využití prostředků Azure proti omezení | Dokumentace Microsoftu
description: Zjistěte, jak kontrolovat využití prostředků Azure na limity předplatného Azure.
services: networking
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: networking
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.author: jdial
ms.openlocfilehash: 8546897de029a1df4563a9d2a3353762d5495096
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652310"
---
# <a name="check-resource-usage-against-limits"></a>Kontrola využití prostředků proti omezení

V tomto článku se dozvíte, jak můžete zobrazit počet každý typ síťového prostředku, který jste nasadili v vaše předplatné a co vaše [limity předplatného](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) jsou. Možnost zobrazení využití prostředků proti omezení je vhodné sledovat aktuální využití a plánování pro budoucí použití. Můžete použít [webu Azure Portal](#azure-portal), [PowerShell](#powershell), nebo [rozhraní příkazového řádku Azure](#azure-cli) ke sledování využití.

## <a name="azure-portal"></a>Azure Portal

1. Přihlaste se k Azure [portál](https://portal.azure.com).
2. Horním levém horním rohu na webu Azure portal, vyberte **všechny služby**.
3. Zadejte *předplatná* v **filtr** pole. Když **předplatná** se zobrazí ve výsledcích hledání vyberte ji.
4. Vyberte název předplatného, které chcete zobrazit informace o využití.
5. V části **nastavení**vyberte **kvóty a využití**.
6. Můžete vybrat následující možnosti:
    - **Typy prostředků**: Můžete vybrat všechny typy prostředků nebo vyberte konkrétní typy prostředků, které chcete zobrazit.
    - **Poskytovatelé**: Můžete vybrat všechny poskytovatele prostředků nebo vyberte **Compute**, **sítě**, nebo **úložiště**.
    - **Umístění**: Můžete vybrat všech umístěních Azure, nebo vybrat konkrétní umístění.
    - Můžete vybrat, chcete-li zobrazit všechny prostředky nebo jenom prostředky, ve kterém je nasazená alespoň jeden.

    Příklad na následujícím obrázku zobrazuje všechny síťových prostředků pomocí nejméně jeden prostředek nasazený v oblasti východní USA:

        ![View usage data](./media/check-usage-against-limits/view-usage.png)

    Řazení sloupců tak, že vyberete na záhlaví sloupce. Mezní hodnoty uvedené jsou limity pro vaše předplatné. Pokud potřebujete zvýšit výchozí omezení, vyberte **žádost o zvýšení**, potom vyplňte a odešlete žádost o podporu. Všechny prostředky měly maximální limit uvedené v Azure [omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits). Pokud aktuální limit už maximálního počtu, omezení nejde zvýšit.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Můžete spouštět příkazy, které následují v [Azure Cloud Shell](https://shell.azure.com/powershell), nebo pomocí prostředí PowerShell z vašeho počítače. Azure Cloud Shell je bezplatné interaktivní prostředí. Má předinstalované obecné nástroje Azure, které jsou nakonfigurované pro použití s vaším účtem. Při spuštění PowerShell z počítače, je nutné modul Azure PowerShell verze 1.0.0 nebo novějším. Spustit `Get-Module -ListAvailable Az` v počítači nainstalovanou verzi zjistíte. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-az-ps). Pokud používáte PowerShell místně, musíte také spustit `Login-AzAccount` se přihlaste k Azure.

Zobrazit využití proti omezení s [Get-AzNetworkUsage](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkusage). Následující příklad získá využití prostředků, kde je nasazený aspoň jeden prostředek v umístění východní USA:

```azurepowershell-interactive
Get-AzNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

Zobrazí se výstup ve formátu stejná jako následující příklad výstupu:

```powershell
ResourceType            CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
```

## <a name="azure-cli"></a>Azure CLI

Pokud k dokončení úkolů v tomto článku pomocí příkazů rozhraní příkazového řádku Azure (CLI), buď spusťte příkazy [Azure Cloud Shell](https://shell.azure.com/bash), nebo pomocí rozhraní příkazového řádku z vašeho počítače. Tento článek vyžaduje použití Azure CLI verze 2.0.32 nebo novější. Nainstalovanou verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI](/cli/azure/install-azure-cli). Pokud používáte Azure CLI místně, musíte také spustit `az login` se přihlaste k Azure.

Zobrazit využití proti omezení s [az network list-usages](/cli/azure/network?view=azure-cli-latest#az-network-list-usages). Následující příklad získá využití prostředků v umístění USA – východ:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

Zobrazí se výstup ve formátu stejná jako následující příklad výstupu:

```azurecli
Name                    CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
Load Balancers                     0   100
Application Gateways               0    50
```
