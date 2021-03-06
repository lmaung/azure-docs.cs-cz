---
title: Ukázkový skript Azure Powershellu – odstranění kontejnerů na základě předpony | Dokumentace Microsoftu
description: Kontejnery objektů blob ve službě Azure Storage můžete odstranit na základě předpony názvu kontejneru.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: e0c41ed1fc2cdb7559d36a7056ebb7b9a6ecc186
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242766"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Odstranění kontejnerů na základě předpony názvu kontejneru

Tento skript odstraní kontejnery v Azure Blob storage na základě předpony v názvu kontejneru.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.ps1 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spuštěním následujícího příkazu odeberte skupinu prostředků, zbývající kontejnery a všechny související prostředky.

```powershell
Remove-AzResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript k odstranění kontejnerů na základě předpony názvu kontejneru používá následující příkazy. Každá položka v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) | Získá zadaný účet úložiště nebo všechny účty úložišť ve skupině prostředků nebo předplatného. |
| [Get-AzStorageContainer](/powershell/module/az.storage/Get-AzStorageContainer) | Vypíše kontejnery úložiště přidružené k účtu úložiště. |
| [Remove-AzStorageContainer](/powershell/module/az.storage/Remove-AzStorageContainer) | Odebere zadané úložiště kontejnerů. |

## <a name="next-steps"></a>Další postup

Další informace o modulu Azure PowerShellu najdete v [dokumentaci k Azure PowerShellu](/powershell/azure/overview).

Další ukázkové skripty PowerShellu pro úložiště najdete v tématu [Ukázky PowerShellu pro úložiště objektů blob v Azure](../blobs/storage-samples-blobs-powershell.md).
