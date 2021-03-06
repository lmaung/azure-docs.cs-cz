---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 28aab15dc67e051190e8d4e35e92240a56fe54a6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262923"
---
Dále, pokud všechny servery v clusteru se systémem Windows Server 2008 R2 nebo Windows Server 2012, musíte ověřit, že oprava hotfix [KB2854082](https://support.microsoft.com/kb/2854082) je nainstalovaný na všech na místních serverech nebo virtuálních počítačích Azure, které jsou součástí clusteru. Libovolný server nebo virtuální počítač, který je v clusteru, ale není ve skupině dostupnosti musí být také tato oprava hotfix nainstalovaná.

V této relaci vzdálené plochy pro každý z uzlů clusteru, stáhněte si [KB2854082](https://support.microsoft.com/kb/2854082) do místního adresáře. Pak nainstalujte opravu hotfix na všech uzlech clusteru postupně. Pokud se Clusterová služba aktuálně běží na uzlu clusteru, na konci instalace opravy hotfix restartování serveru.

> [!WARNING]
> Zastavením Clusterové služby nebo restartování serveru má vliv na stav kvora clusteru a skupiny dostupnosti a může způsobit clusteru do režimu offline. Chcete-li udržet vysokou dostupnost clusteru během instalace, ujistěte se, že:
> 
> * Cluster je ve stavu optimální kvora. 
> * Před instalací opravy hotfix ve všech uzlech, jsou všechny uzly clusteru online.
> * Před instalací opravy hotfix na další uzel v clusteru povolit instalace opravy hotfix pro spuštění na dokončení na jednom uzlu, včetně plně restartování serveru.
> 
> 

