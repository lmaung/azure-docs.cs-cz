---
title: Vysoká dostupnost ve službě Azure Cosmos DB
description: Tento článek popisuje, jak službu Azure Cosmos DB poskytuje vysokou dostupnost
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 2/13/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: b5e99b421e66f087a1793f5301736e192ef75c08
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311235"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Vysoká dostupnost s využitím Azure Cosmos DB

Azure Cosmos DB transparentně replikuje data napříč všemi oblastmi Azure spojené s vaším účtem Cosmos. Cosmos DB využívá několik vrstev redundance pro vaše data, jak je znázorněno na následujícím obrázku:

![Fyzické dělení](./media/high-availability/cosmosdb-data-redundancy.png)

- Data v rámci Cosmos kontejnerů se horizontálně dělené do oddílů.

- V rámci každé oblasti je každý oddíl chráněný sady replik se všechny operace zápisu replikovat a odolném potvrzení většinou replik. Repliky distribuují napříč doménami selhání až 10-20.

- Každý oddíl napříč všemi oblastmi se replikuje. Každé oblasti obsahuje všechny oddíly data kontejneru Cosmos a můžou akceptovat zápisy a čtení obsluhovat.  

Pokud je váš účet služby Cosmos distribuovaná napříč oblastmi N Azure, bude existovat alespoň N x čtyři kopie všechna vaše data. Kromě poskytování přístupu k datům s nízkou latenci a škálování propustnosti zápisu/čtení v oblastech přidružili k vašemu účtu Cosmos, s více oblastmi (vyšší N) také zlepšuje dostupnost.  

## <a name="slas-for-availability"></a>Smlouvy SLA pro dostupnost

Cosmos DB jakožto globálně distribuovaná databáze, poskytuje komplexní smlouvy SLA, které zahrnuje propustnosti, latenci na úrovni 99. percentilu, konzistence a vysokou dostupnost. Následující tabulka ukazuje záruky pro zajištění vysoké dostupnosti poskytuje jeden a více oblastí pro účty služby Cosmos DB. Pro zajištění vysoké dostupnosti nakonfigurujte účty Cosmos a mít více oblastí zápisu.

|Typ operace  | Jedna oblast |Ve více oblastech (zápisy do jedné oblasti)|Ve více oblastech (více oblastí zápisu) |
|---------|---------|---------|-------|
|Zápisy    | 99.99    |99.99   |99.999|
|Čtení     | 99.99    |99.999  |99.999|

> [!NOTE]
> V praxi dostupnost skutečné zápisu omezená neaktuálnost, relace, modelů konzistence konzistentní Předpona a konečný výsledek je výrazně vyšší než publikované smlouvy o úrovni služeb. Skutečnou dostupnost čtení pro všechny úrovně konzistence je výrazně vyšší než publikované smlouvy o úrovni služeb.

## <a name="high-availability-with-cosmos-db-in-the-face-of-regional-outages"></a>Vysoká dostupnost s využitím Cosmos DB i v případě místních výpadků

Nejsou místních výpadků a Azure Cosmos DB zajišťuje, že vaše databáze budou vždy k dispozici. Následující podrobnosti o zachycení Cosmos DB chování během výpadku, v závislosti na konfiguraci vašeho účtu Cosmos:

- Pomocí služby Cosmos DB než je potvrzení operace zápisu do klienta, data odolném potvrzení podle kvorum replik v rámci oblasti, která přijímá operace zápisu.

- Nakonfigurovaná s několika oblasti účty ve více oblastech budou s vysokou dostupností pro zápisy a čtení. Regionální převzetí služeb při selhání jsou okamžité a nevyžadují žádné změny z aplikace.

- Účty ve více oblastech s jedním zápisu oblastí: Během výpadku oblasti zápisu zůstanou tyto účty s vysokou dostupností pro čtení. Však pro zápis musí "Povolit automatické převzetí služeb při selhání" na vašem účtu Cosmos ovlivněné oblasti do jiné oblasti přidružené převzetí služeb při selhání. Převzetí služeb při selhání dojde v pořadí podle priority oblasti, kterou jste zadali. Nakonec pokud ovlivněné oblasti je zpátky do online režimu, nereplikované data k dispozici v oblasti ovlivněné zápisu během výpadku je k dispozici prostřednictvím konflikty informačního kanálu. Aplikace může číst konflikty informačního kanálu, vyřešte konflikty, na základě specifické pro aplikaci logiky a zapsat aktualizovaná data zpět do kontejneru Cosmos podle potřeby. Po obnoví dříve ovlivněné zápisu oblasti stane automaticky k dispozici jako oblasti čtení. Můžete vyvolat ruční převzetí služeb při selhání a nakonfigurovat ovlivněné oblasti jako oblast pro zápis. Ruční převzetí služeb při selhání můžete provést pomocí [webu Azure portal nebo rozhraní příkazového řádku Azure](how-to-manage-database-account.md#manual-failover). Je **bez ztráty dat nebo dostupnost** před, během nebo po ruční převzetí služeb při selhání. Vaše aplikace i nadále s vysokou dostupností. 

- Účty ve více oblastech s jedním zápisu oblastí: Během výpadku oblasti čtení zůstanou tyto účty s vysokou dostupností pro čtení a zápis. Ovlivněné oblasti se automaticky odpojí z oblasti pro zápis a budou označeny v režimu offline. Cosmos DB SDK přesměruje volání další dostupné oblasti v seznamu preferované oblasti čtení. Pokud žádná z oblasti v seznamu preferované oblasti není k dispozici, volání automaticky vrátit do aktuální oblasti pro zápis. Nevyžaduje žádné změny v kódu aplikace zpracovávat výpadku oblasti čtení. Nakonec při ovlivněné oblasti je zpátky do online režimu, budou automaticky synchronizovat s aktuální oblasti pro zápis dříve ovlivněné oblasti čtení a bude možné znovu obsluhovat požadavky na čtení. Další čtení se přesměrují na obnoveném oblasti nevyžaduje žádné změny kódu aplikace. Během převzetí služeb při selhání i některých dříve nezdařené oblasti záruky konzistence čtení dál respektovat Cosmos DB.

- Účty v jedné oblasti může dojít ke ztrátě dostupnosti po regionálního výpadku. Doporučujeme nastavit alespoň dvou oblastech (pokud možno měly aspoň dva oblasti) pomocí svého účtu Cosmos k zajištění vysoké dostupnosti po celou dobu.

- I v případě velmi vzácné a unfortunate při oblast Azure je trvale nezotavitelnou neexistuje žádné ztrátě dat pokud je nakonfigurovaný účet Cosmos ve více oblastech s výchozí úroveň konzistence silná. V případě trvale nezotavitelnou zápisu oblasti, pro více oblastí účty Cosmos nakonfigurovanou konzistenci omezená neaktuálnost okno potenciální ztráty dat je omezená neaktuálnost okna. pro relace, konzistentní Předpona a konečný výsledek úrovním, okno potenciální ztráty dat je omezen na maximálně pět sekund.

## <a name="building-highly-available-applications"></a>Vytváření aplikací s vysokou dostupností

- K zajištění vysoké zápisu a dostupnost pro čtení, nastavte v účtu Cosmos rozložit alespoň dvě oblasti s několika oblasti. Tato konfigurace zajistí dostupnost, nejnižší latenci a škálovatelnost pro obě operace čtení a zápisu zajištěnými smlouvami SLA. Další informace najdete v tématu Jak [nakonfigurujte svůj účet Cosmos s využitím více oblastí zápisu](tutorial-global-distribution-sql-api.md). Pokud chcete nakonfigurovat více hlavních databází ve svých aplikacích, naleznete v tématu [konfigurace více hlavních databází](how-to-multi-master.md).

- Pro účty Cosmos ve více oblastech, které jsou nakonfigurovány s oblastí zápisu jedním [povolit automatické – převzetí služeb při selhání s využitím webu Azure portal nebo rozhraní příkazového řádku Azure](how-to-manage-database-account.md#automatic-failover). Když povolíte automatické převzetí služeb při selhání, vždycky, když je regionálního, Cosmos DB bude automaticky převzetí služeb při selhání vašeho účtu.  

- I v případě, že je váš účet Cosmos s vysokou dostupností, aplikace nemusí správně navržená k dispozici. K otestování začátku do konce vysoká dostupnost pro aplikace, pravidelně vyvolat [ruční převzetí služeb při selhání s využitím webu Azure portal nebo rozhraní příkazového řádku Azure](how-to-manage-database-account.md#manual-failover), jako součást vašeho testování aplikace nebo zotavení po havárii (DR) cvičení.

- V prostředí s globálně distribuovanou databázi je přímý vztah mezi konzistence odolnosti úroveň a dat za přítomnosti výpadku celé oblasti. Při vývoji plánu provozní kontinuity musíte pochopit maximální přijatelnou dobu, než úplného obnovení aplikace po ničivé události. Čas potřebný pro aplikaci k plnému obnovení se označuje jako plánovaná doba obnovení (RTO). Také musíte pochopit maximální období posledních aktualizací dat aplikace může tolerovat možnost ztráty při obnovení po ničivé události. Časové období aktualizací, které si může dovolit přijít o se označuje jako cíl bodu obnovení (RPO). RPO a RTO pro službu Azure Cosmos DB najdete v tématu [konzistence úrovně a data odolnosti](consistency-levels-tradeoffs.md#rto)

## <a name="next-steps"></a>Další postup

Další informace o škálování propustnosti v následujícím článku:

* [Dostupnost a výkon kompromisy pro různé úrovně konzistence](consistency-levels-tradeoffs.md)
* [Globální škálování zřízená propustnost](scaling-throughput.md)
* [Globální distribuce - pod pokličkou](global-dist-under-the-hood.md)
* [Úrovně konzistence ve službě Azure Cosmos DB](consistency-levels.md)
* [Jak nakonfigurovat více hlavních databází ve svých aplikacích](how-to-multi-master.md)
