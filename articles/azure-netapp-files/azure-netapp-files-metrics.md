---
title: Metriky pro soubory Azure NetApp | Dokumentace Microsoftu
description: Popisuje metriky pro soubory Azure NetApp.
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
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 1563b60ef26ac5e4d40f45095d0109dd9dd71570
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672572"
---
# <a name="metrics-for-azure-netapp-files"></a>Metriky pro Azure NetApp Files

Služba soubory Azure NetApp poskytuje metriky pro přidělené úložiště, použití skutečného úložiště, propustnost svazku, vstupně-výstupních operací a latenci. Díky analýze tyto metriky, můžete získat lepší porozumění na využití vzoru a svazek výkon vašich účtů NetApp.  

## <a name="capacity_pools"></a>Metriky využití pro fondy kapacity

- *Velikost fondu přidělené svazku*  
    Toto je velikost (GiB) zřízená kapacita fondu.  
- *Svazek fondu přidělené použít*  
    Toto je celkový součet kvóty (GiB) v rámci dané kapacitě fondu (to znamená, celkové zřízené velikosti na svazcích ve fondu kapacity). Toto je velikost, kterou jste vybrali při vytváření svazku.  
- *Logická velikost svazku fondu celkem*  
    Toto je celkový součet logické využité (GiB) mezi svazky ve fondu kapacity.  
- *Velikost fondu celkový snímku svazku*  
    Toto je celkový součet přírůstkové logické využitou snímky.  

## <a name="volumes"></a>Metriky využití pro svazky

- *Přidělená velikost svazku*   
    Toto je velikost svazku (kvóty) zřízené v GiB.  
- *Logická velikost svazku*   
    Toto je celkový počet logických využité hromadně (GiB). Tato velikost obsahuje logický prostor využitý aktivní souborové systémy a snímků.  
- *Velikost snímku svazku*   
    Toto je přírůstkové logické využité snímky ve svazku.  

## <a name="next-steps"></a>Další postup

* [Vysvětlení hierarchii úložiště souborů Azure NetApp](azure-netapp-files-understand-storage-hierarchy.md)
* [Nastavení fondu kapacity](azure-netapp-files-set-up-capacity-pool.md)
* [Vytvoření svazku pro Azure NetApp Files](azure-netapp-files-create-volumes.md)
