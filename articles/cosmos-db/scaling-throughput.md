---
title: Škálování propustnosti ve službě Azure Cosmos DB
description: Tento článek popisuje, jak službu Azure Cosmos DB Elasticky škáluje propustnost
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: dd17b08a16dedf474b2a1eca8fa8034672610c1f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/31/2019
ms.locfileid: "55454432"
---
# <a name="globally-scale-provisioned-throughput"></a>Globálně škálujte zřízená propustnost 

Ve službě Azure Cosmos DB, zřízená propustnost je vyjádřena jako požadavek jednotek za sekundu (RU/s, plural: RU). Měření náklady na čtení a zápisu operace kontejneru Cosmos, jak je znázorněno na následujícím obrázku:

![Požadované jednotky](./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png)

Můžete zřídit RU v kontejneru Cosmos nebo v databázi Cosmos. RU zřízených v kontejneru je dostupná výhradně pro operace prováděné v tomto kontejneru. RU zřízených pro databázi jsou sdílena mezi všechny kontejnery v rámci této databáze (s výjimkou všechny kontejnery s exkluzivně přiřazená ru).

Elasticky škálovat propustnost, můžete zvýšit nebo snížit RU/s zřízených v každém okamžiku. Další informace najdete v tématu [postupy: poskytování propustnost](set-throughput.md) a Elasticky škálovat Cosmos kontejnerů a databáze. Globální škálování propustnosti, můžete přidání nebo odebrání oblastí na vašem účtu Cosmos kdykoli. Další informace najdete v tématu [přidat nebo odebrat oblasti ze svého účtu databáze](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Přidružení účtu Cosmos více oblastí je důležité pro mnoho scénářů s nízkou latencí dosáhnout a [vysoké dostupnosti](high-availability.md) po celém světě.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>jak zřízená propustnost je distribuovaná napříč oblastmi

Pokud zřizujete "R" ru na databázi Cosmos kontejneru (nebo), Cosmos DB zajišťuje, že jsou k dispozici v "R" ru *každý* oblasti přidružené k účtu Cosmos. Pokaždé, když přidáte novou oblast ke svému účtu služby Cosmos DB automaticky zřídí ru "R" v nově přidaném oblasti. Operací provedených v kontejneru Cosmos je zaručeno, chcete-li získat ru "R" v jednotlivých oblastech. Nelze přiřadit selektivně ru určité oblasti. Pro všemi oblastmi spojenými s vaším účtem Cosmos jsou zřízené ru zřízených pro databázi Cosmos kontejneru (nebo).

Za předpokladu, že se v kontejneru Cosmos nakonfigurují "R" ru a existují n oblasti přidružené k účtu Cosmos, pak:

- Pokud je nakonfigurovaný účet Cosmos s oblastí jeden zápis, celkový počet ru dostupná globálně v kontejneru = R x N.

- Pokud je nakonfigurovaný účet Cosmos s využitím více oblastí zápisu, celkový počet ru dostupná globálně v kontejneru R = x (N + 1). Další jednotky ru R budou automaticky přiřazeni k procesu aktualizace je v konfliktu a proti entropie provoz napříč regiony.

Podle vaší volby [model pro zajištění konzistence](consistency-levels.md) ovlivní také propustnosti. Pro relace, konzistentní Předpona a konečné konzistence ve srovnání s omezená neaktuálnost nebo silné konzistence můžete získat přibližně 2 × propustnost čtení.

## <a name="next-steps"></a>Další postup

Dále můžete zjistěte, jak nakonfigurovat propustnost díky pomoci v následujícím článku:

* [Získání a nastavení propustnosti pro kontejnery a databáze](set-throughput.md) 

