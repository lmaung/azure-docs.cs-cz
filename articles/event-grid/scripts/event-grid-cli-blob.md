---
title: Ukázkový skript Azure CLI – Přihlášení k odběru účtu úložiště objektů blob | Microsoft Docs
description: Ukázkový skript Azure CLI – Přihlášení k odběru účtu úložiště objektů blob
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: tomfitz
ms.openlocfilehash: f352542f72226358cd700359eb5aac16e1aa8ad5
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652480"
---
# <a name="subscribe-to-events-for-a-blob-storage-account-with-azure-cli"></a>Přihlášení k odběru událostí účtu úložiště objektů blob pomocí Azure CLI

Tento skript vytvoří odběr Event Gridu pro události účtu úložiště objektů blob. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-blob-storage/subscribe-to-blob-storage.sh "Subscribe to Blob storage")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript k vytvoření odběru událostí používá následující příkaz. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | Vytvoří odběr Event Gridu. |
| [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) – verze rozšíření | Vytvoří odběr Event Gridu. |

## <a name="next-steps"></a>Další postup

* Informace o dotazování předplatných najdete v tématu [Dotazování odběrů Event Gridu](../query-event-subscriptions.md).
* Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](https://docs.microsoft.com/cli/azure).