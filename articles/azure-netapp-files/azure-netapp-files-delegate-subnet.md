---
title: Delegovat podsítě do služby soubory Azure NetApp | Dokumentace Microsoftu
description: Popisuje, jak delegovat podsítě do služby soubory Azure NetApp.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/13/2018
ms.author: b-juche
ms.openlocfilehash: 8ec41c6db8c8e5c62d15dc0638762f2649c637b8
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2018
ms.locfileid: "53631647"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Delegování podsítě do Azure NetApp Files 

Podsíť, která se soubory Azure NetApp musí delegovat.   Při vytváření svazku je třeba zadat delegované podsítě.

## <a name="about-this-task"></a>O této úloze
* Průvodce vytvořením nové podsítě. výchozí hodnota je/24 je maska sítě, která poskytuje pro 251 dostupných IP adres. Pomocí o velikosti/28 maska sítě, která poskytuje 16 použitelných IP adres, je dostatečná pro službu.
* Nelze určit skupinu zabezpečení sítě nebo služby koncového bodu v delegované podsítě. To způsobí, že podsíť delegování selhání.
* V každé Azure Virtual Network (Vnet) je možné jenom jednu podsíť delegovat do služby soubory Azure NetApp.
* Přístup ke svazku z partnerské virtuální síti se aktuálně nepodporuje.

## <a name="steps"></a>Kroky 
1.  Přejděte **virtuální sítě** okno z webu Azure portal a vyberte virtuální síť, kterou chcete použít pro soubory Azure NetApp.    

1. Vyberte **podsítě** z okna virtuální sítě a klikněte **+ podsíť** tlačítko. 

1. Vytvořte novou podsíť, kterou chcete použít pro soubory Azure NetApp provedením následující požadované pole na stránce Přidat podsíť:
    * **Název**: Zadejte název podsítě.
    * **Rozsah adres**: Zadejte rozsah IP adres.
    * **Delegování podsítě**: Vyberte **Microsoft.NetApp/volumes**. 

      ![Delegování podsítě](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Můžete také vytvořit a delegovat podsítě při vám [vytvoření svazku pro soubory Azure NetApp](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Další postup  
* [Vytvoření svazku pro Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Další informace o integraci virtuální sítě pro služby Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


