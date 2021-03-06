---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 30f6feb920d78c9c325c5556f5530ac4c8d9459b
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "29955658"
---
<!-- Not used for Ls-series -->

## <a name="size-table-definitions"></a>Definice tabulky velikostí

- Kapacita úložiště je v jednotkách GiB, tj. 1024^3 bajtů. Při porovnávání diskové kapacity měřené v GB (1000^3 B) s kapacitou měřenou v GiB (1024^3) pamatujte, že údaj v GiB je číselně menší. Například 1023 GiB = 1098,4 GB
- Propustnost disku se měří v počtu V/V operací za sekundu (IOPS) a v MB/s, kde 1 MB/s = 10^6 bajtů/s.
- Disky pro ukládání dat můžou fungovat v režimu s mezipamětí, nebo bez ní. Pro diskové operace s mezipamětí je možné nastavit mezipaměť na hostiteli **jen na čtení** nebo **na čtení i zápis**.  Pro diskové operace bez mezipaměti je mezipaměť na hostiteli nastavená na **žádná**.
-   Pokud chcete získat nejlepší výkon pro vaše virtuální počítače, byste měli omezit počet datových disků na 2 disky na virtuální procesor.
- **Byl očekáván šířky pásma sítě** je maximální agregovaná [šířka pásma přidělená podle typu virtuálního počítače](../articles/virtual-network/virtual-machine-network-throughput.md) pro všechny síťové karty pro všechny cíle. Horní omezení nejsou garantována, ale slouží jako vodítko pro výběr správného typu virtuálního počítače pro příslušnou aplikaci. Skutečný výkon sítě bude záviset na řadě faktorů, včetně zahlcení sítě, zatížení aplikací a nastavení sítě. Informace o optimalizaci propustnosti sítě najdete v tématu [Optimalizace propustnosti sítě pro Windows a Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Pro dosažení očekávaného výkonu sítě na Linuxu nebo ve Windows může být nutné vybrat konkrétní verzi nebo optimalizovat virtuální počítač. Další informace najdete v tématu [Spolehlivé testování propustnosti virtuálního počítače](../articles/virtual-network/virtual-network-bandwidth-testing.md).



