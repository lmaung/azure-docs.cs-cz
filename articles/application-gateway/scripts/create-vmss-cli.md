---
title: Ukázkový skript Azure CLI – Správa webového provozu | Microsoft Docs
description: Ukázkový skript Azure CLI – Správa webového provozu pomocí aplikační brány a škálovací sady virtuálních počítačů
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 99d0939b30d04fbd5c0eb7a287105bb4cf27e9f4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116758"
---
# <a name="manage-web-traffic-using-the-azure-cli"></a>Správa webového provozu s Azure CLI

Tento skript vytvoří aplikační bránu, která pro back-endové servery používá škálovací sadu virtuálních počítačů. Vytvořená aplikační brána se dá nakonfigurovat tak, aby spravovala webový provoz. Po spuštění skriptu můžete otestovat aplikační bránu použitím její veřejné IP adresy.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Pokud chcete skupinu prostředků, aplikační bránu a všechny související prostředky odebrat, spusťte následující příkaz.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript pomocí následujících příkazů vytvoří nasazení. Každá položka v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Vytvoří virtuální síť. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Vytvoří ve virtuální síti podsíť. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest) | Vytvoří veřejnou IP adresu aplikační brány. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest) | Vytvoří aplikační bránu. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss) | Vytvoří škálovací sadu virtuálních počítačů. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip) | Získá veřejnou IP adresu aplikační brány. |

## <a name="next-steps"></a>Další postup

Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Další ukázkové skripty CLI pro aplikační brány najdete v [dokumentaci k virtuálním počítačům Azure s Windows](../cli-samples.md).
