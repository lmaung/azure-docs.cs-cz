---
title: Škálování izolované databáze prostředků – Azure SQL Database | Dokumentace Microsoftu
description: Tento článek popisuje, jak škálovat výpočetní a úložné prostředky dostupné pro izolované databáze ve službě Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
manager: craigg
ms.date: 02/25/2019
ms.openlocfilehash: 40c72c8a3ac4b60c6361e0f5ca3ac16ccb78b05c
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883994"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Škálování izolované databáze prostředků ve službě Azure SQL Database

Tento článek popisuje, jak škálovat výpočetní a úložné prostředky dostupné pro izolované databáze ve službě Azure SQL Database.

> [!IMPORTANT]
> Se vám účtovat každá hodina existence databáze pomocí nejvyšší úroveň služby a vypočítat velikost, která během této hodiny využívalo, bez ohledu na využití nebo na to, jestli byl databáze aktivní kratší dobu než hodinu. Například pokud vytvoření izolované databáze a po pěti minutách ji odstraníte vyúčtování projeví účtovat jedna hodina používání databáze.

## <a name="vcore-based-purchasing-model-change-storage-size"></a>nákupní model založený na virtuálních jádrech: Změnit velikost úložiště

- Úložiště lze zřídit až po limit maximální velikosti 1 GB přírůstcích pomocí. Minimální konfigurovatelné datové úložiště je 5 GB
- Zvýšením nebo snížením jeho maximální velikost pomocí je možné zřídit úložiště pro izolovanou databázi [webu Azure portal](https://portal.azure.com), [příkazů jazyka Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [Powershellu](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Rozhraní příkazového řádku azure](/cli/azure/sql/db#az-sql-db-update), nebo [rozhraní REST API](https://docs.microsoft.com/rest/api/sql/databases/update).
- SQL Database automaticky přiděluje 30 % další úložiště pro soubory protokolů a 32GB za – vCore pro databázi TempDB, ale která nepřekročí 384GB. Databáze TempDB je umístěná na připojené SSD ve všech úrovních služby.
- Ceny úložišť pro izolované databáze je součet množství dat úložiště a protokol úložiště vynásobí jednotkovou cenu úložiště na úrovni služby. Cena databáze tempdb je zahrnutá v ceně – vCore. Podrobnosti o cenách dodatečného úložiště najdete v tématu [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Za určitých okolností budete muset zmenšit databázi uvolnění nevyužívaného místa. Další informace najdete v tématu [spravovat místo souborů ve službě Azure SQL Database](sql-database-file-space-management.md).

## <a name="vcore-based-purchasing-model-change-compute-resources"></a>nákupní model založený na virtuálních jádrech: Změna výpočetní prostředky

Po počátečním výběru počet virtuálních jader, můžete vertikálně izolovanou databázi směrem nahoru nebo dolů dynamicky na základě aktuálních zkušeností pomocí [webu Azure portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [příkazů jazyka Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [Powershellu](/powershell/module/azurerm.sql/set-azurermsqldatabase), [rozhraní příkazového řádku Azure](/cli/azure/sql/db#az-sql-db-update), nebo [rozhraní REST API](https://docs.microsoft.com/rest/api/sql/databases/update).

Změna služby vrstvy a/nebo vypočítat velikost databáze vytvoří replika původní databáze na novou velikost výpočetních a následně se přepnou připojení na repliku. Během tohoto procesu se neztratí žádná data, ale během krátké chvíle, kdy se přepíná na repliku, jsou zakázána připojení k databázi, takže může dojít k vrácení některých probíhajících transakcí zpět. Doba pro přechod se liší, ale je obecně méně než 30 sekund 99 % času. Pokud existují velké množství transakcí za pochodu okamžiku zákazu připojení probíhá jsou zakázané, může být delší dobu pro přechod.

Doba trvání celého procesu horizontálního navýšení se obecně závisí na obou velikosti a úrovni služeb databáze před a po provedení změny. Například jakékoli velikosti databáze, která se mění velikost výpočty v rámci úrovně služeb by se měla dokončit během několika minut pro obecné účely na druhé straně, latence, chcete-li změnit výpočetní velikost v rámci důležité obchodní informace úroveň je obecně 90 minut nebo i rychleji na každých 100 GB.

> [!TIP]
> Monitorování operací v průběhu najdete v tématu: [Správa operací pomocí rozhraní SQL API REST](https://docs.microsoft.com/rest/api/sql/operations/list), [správě operací pomocí rozhraní příkazového řádku](/cli/azure/sql/db/op), [sledování operací s použitím jazyka T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) a tyto dva příkazy Powershellu: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) a [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

- Pokud provádíte upgrade na vyšší úroveň služby nebo vypočítat velikost, maximální velikost databáze se nezvyšuje, pokud explicitně neurčíte větší velikost (maxsize).
- Na starší verzi databáze, databáze používá prostor musí být menší než maximální povolená velikost cílové úrovni služeb a výpočetního prostředí.
- Když upgradujete databázi s [geografickou replikaci](sql-database-geo-replication-portal.md) povolena, upgradovat její sekundární databáze na úrovni požadované služby a vypočítat velikost před upgradem primární databáze (Obecné pokyny pro zajištění nejlepšího výkonu). Při upgradu na jiný, upgrade sekundární databáze nejprve je povinný.
- Při downgradu databáze s [geografickou replikaci](sql-database-geo-replication-portal.md) povolena, downgradovat na úroveň požadovanou službu její primární databáze a vypočítat velikost před snížením sekundární databáze (Obecné pokyny pro zajištění nejlepšího výkonu). Při downgradu na jinou edici, downgradu primární databáze nejprve se vyžaduje.
- Nové vlastnosti databáze se nepoužijí, dokud nebudou změny dokončeny.

## <a name="dtu-based-purchasing-model-change-storage-size"></a>Nákupní model založený na DTU: Změnit velikost úložiště

- Cena za DTU pro izolovanou databázi zahrnuje objem úložiště bez dalších poplatků. Dodatečné úložiště nad rámec objemu zahrnutého v ceně je možné zřídit za poplatek až po limit maximální velikosti, v přírůstcích po 250 GB až 1 TB a potom dokupuje se násobek 256 GB nad rámec 1 TB. Částky zahrnutého úložiště a omezení maximální velikosti najdete v tématu [izolované databáze: Velikosti úložiště a výpočty velikostí](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Dodatečné úložiště pro izolovanou databázi je možné zřídit zvýšením jeho maximální velikost pomocí webu Azure portal, [příkazů jazyka Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [rozhraní příkazového řádku Azure](/cli/azure/sql/db#az-sql-db-update), nebo [ Rozhraní REST API](https://docs.microsoft.com/rest/api/sql/databases/update).
- Cena dodatečného úložiště pro izolovanou databázi se velikost dodatečného úložiště vynásobí jednotkovou cenu dodatečné úložiště na úrovni služby. Podrobnosti o cenách dodatečného úložiště najdete v tématu [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Za určitých okolností budete muset zmenšit databázi uvolnění nevyužívaného místa. Další informace najdete v tématu [spravovat místo souborů ve službě Azure SQL Database](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-change-compute-resources-dtus"></a>Nákupní model založený na DTU: Změna výpočetních prostředků (Dtu)

Po počátečním výběru úrovně služeb, výpočetního prostředí a velikost úložiště, můžete vertikálně izolovanou databázi směrem nahoru nebo dolů dynamicky na základě aktuálních zkušeností pomocí webu Azure portal, [příkazů jazyka Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [Powershellu](/powershell/module/azurerm.sql/set-azurermsqldatabase), [rozhraní příkazového řádku Azure](/cli/azure/sql/db#az-sql-db-update), nebo [rozhraní REST API](https://docs.microsoft.com/rest/api/sql/databases/update).

Následující video ukazuje dynamické změny služby vrstvy a vypočítat velikost zvýšit dostupné Dtu pro izolovanou databázi.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Změna služby vrstvy a/nebo vypočítat velikost databáze vytvoří replika původní databáze na novou velikost výpočetních a následně se přepnou připojení na repliku. Během tohoto procesu se neztratí žádná data, ale během krátké chvíle, kdy se přepíná na repliku, jsou zakázána připojení k databázi, takže může dojít k vrácení některých probíhajících transakcí zpět. Doba pro přechod se liší, ale je méně než 30 sekund 99 % času. Pokud existují velké množství transakcí za pochodu okamžiku zákazu připojení probíhá jsou zakázané, může být delší dobu pro přechod.

Délka trvání celého procesu vertikálního navyšování kapacity závisí na velikosti a úrovni služeb databáze před změnou a po ní. Například databázi 250 GB, která se mění na, z nebo v rámci úrovně služeb Standard, by se měla dokončit během šesti hodin. Pro databázi stejnou velikost, která se mění velikosti výpočty v rámci úrovně služeb Premium vertikálně navýšit by se měla dokončit během tří hodin.

> [!TIP]
> Monitorování operací v průběhu najdete v tématu: [Správa operací pomocí rozhraní SQL API REST](https://docs.microsoft.com/rest/api/sql/operations/list), [správě operací pomocí rozhraní příkazového řádku](/cli/azure/sql/db/op), [sledování operací s použitím jazyka T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) a tyto dva příkazy Powershellu: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) a [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

- Pokud provádíte upgrade na vyšší úroveň služby nebo vypočítat velikost, maximální velikost databáze se nezvyšuje, pokud explicitně neurčíte větší velikost (maxsize).
- Na starší verzi databáze, databáze používá prostor musí být menší než maximální povolená velikost cílové úrovni služeb a výpočetního prostředí.
- Při downgradu z verze **Premium** k **standardní** vrstvy, náklady na úložiště platí v případě, že (1) na maximální velikost databáze je podporováno v cílovou velikost výpočetních i (2) je maximální velikost překročí zahrnutou velikost úložiště cíle výpočty velikosti. Například pokud je databáze P1 a maximální velikosti 500 GB downsized na S3, pak náklady na úložiště platí od S3 podporuje maximální velikosti 500 GB a jeho velikost zahrnutého úložiště je pouze 250 GB. Ano velikost dodatečného úložiště je 500 GB – 250 GB = 250 GB. Ceny za dodatečné úložiště, najdete v části [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/). Pokud se skutečné množství využité je menší než velikost zahrnutého úložiště, pak toho dalších poplatků se lze vyvarovat snížením maximální velikost databáze na objemu zahrnutého v ceně.
- Když upgradujete databázi s [geografickou replikaci](sql-database-geo-replication-portal.md) povolena, upgradovat její sekundární databáze na úrovni požadované služby a vypočítat velikost před upgradem primární databáze (Obecné pokyny pro zajištění nejlepšího výkonu). Při upgradu na jiný, upgrade sekundární databáze nejprve je povinný.
- Při downgradu databáze s [geografickou replikaci](sql-database-geo-replication-portal.md) povolena, downgradovat na úroveň požadovanou službu její primární databáze a vypočítat velikost před snížením sekundární databáze (Obecné pokyny pro zajištění nejlepšího výkonu). Při downgradu na jinou edici, downgradu primární databáze nejprve se vyžaduje.
- Nabídky služeb pro obnovení se u různých úrovní služeb liší. Pokud přecházíte na **základní** vrstvy, je zkrátit období uchovávání záloh. Zobrazit [záloh Azure SQL Database](sql-database-automated-backups.md).
- Nové vlastnosti databáze se nepoužijí, dokud nebudou změny dokončeny.

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Nákupní model založený na DTU: Omezení pro P11 a P15 při maximální velikosti větší než 1 TB

Více než 1 TB úložiště na úrovni Premium je aktuálně k dispozici ve všech oblastech s výjimkou: Čína – východ, Čína – sever, Německo – střed, Německo – severovýchod, střed USA – Západ, oblastí pro úlohy ministerstva obrany USA a US Government centrální. V těchto oblastech je úložiště na úrovni Premium omezeno na 1 TB. Další informace najdete v tématu [aktuálních omezení pro P11 – P15](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb). Následující požadavky a omezení platí pro databáze P11 a P15 s maximální velikostí větší než 1 TB:

- Pokud vyberete možnost maximální velikosti větší než 1 TB, při vytváření databáze (s použitím hodnotu 4 TB nebo 4096 GB), příkazu pro vytvoření selže s chybou, pokud je databáze zřízena v nepodporované oblasti.
- U existující databáze P11 a P15 žijí v podporované oblasti, můžete zvýšit maximální úložiště přesáhne 1 TB dokupuje se násobek 256 GB až 4 TB. Chcete-li zjistit, jestli podporuje větší velikost ve vaší oblasti, použijte [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) funkce nebo zkontrolujte velikost databáze na webu Azure Portal. Upgrade existující P11 nebo P15 databáze lze provést pouze pomocí hlavního přihlášení na úrovni serveru nebo členové databázové role dbmanager.
- Pokud se operace upgradu provádí v podporované oblasti je ihned aktualizovat konfiguraci. Během procesu upgradu zůstává online databáze. Však nemůže využít v plné výši úložiště přesáhne 1 TB úložiště, dokud skutečné databázové soubory se upgradovaly na nový maximální velikost. Délka čas potřebný závisí na velikosti databáze se upgraduje.
- Při vytváření nebo aktualizaci databáze P11 nebo P15, můžete zvolit pouze maximální velikost 1 TB nebo 4 TB dokupuje se násobek 256 GB. Při vytváření P11 nebo P15 je předem vybraná výchozí možnost úložiště o velikosti 1 TB. U databáze nachází v jednom z podporovaných oblastí můžete zvýšit maximální úložiště až do maximálního počtu 4 TB pro nové nebo existující izolované databáze. Pro všechny ostatní oblasti nelze zvýšit maximální velikost 1 TB. Cena se nemění, když vyberete 4 TB zahrnutého úložiště.
- Pokud nastavena maximální velikost databáze je větší než 1 TB, pak jej nelze změnit na 1 TB i v případě, že skutečné využití úložiště je nižší než 1 TB. Proto nelze downgradovat P11 nebo P15 s maximální velikostí větší než 1 TB na 1 TB P11 nebo TB P15 1 nebo snížení výpočetních velikost, například P1 – P6). Toto omezení platí také pro obnovení a kopírování scénářů, jako jsou třeba bodu v čase, geografické obnovení, dlouhé-dlouhodobé zálohování uchovávání a kopie databáze. Jakmile je databáze nakonfigurovaná s maximální velikostí větší než 1 TB, musí být spuštěn všechny operace obnovení databáze do P11 nebo P15 s maximální velikostí větší než 1 TB.
- Pro scénáře aktivní geografickou replikaci:
  - Nastavení relaci geografické replikace: Pokud je primární databáze P11 nebo P15, secondary(ies) musí být také P11 nebo P15; menší velikost výpočetního odmítají jako sekundární databáze, protože nejsou schopný zajistit podporu více než 1 TB.
  - Upgrade primární databázi v relaci geografické replikace: Změna maximální velikosti větší než 1 TB na primární databázi aktivuje stejnou změnu v sekundární databázi. I upgrady musí být úspěšná, aby se změny na primárním se projeví. Platí omezení oblasti pro možnost více než 1 TB. Je-li sekundární v oblasti, která nepodporuje více než 1 TB, není aktualizován primární.
- Použití služby Import/Export pro načítání databáze P11 a P15 s více než 1 TB není podporováno. Použít SqlPackage.exe k [importovat](sql-database-import.md) a [exportovat](sql-database-export.md) data.

## <a name="next-steps"></a>Další postup

Celkové omezení prostředků najdete v tématu [omezení prostředků založený na virtuálních jádrech SQL Database – izolované databáze](sql-database-vcore-resource-limits-single-databases.md) a [omezení prostředků založený na DTU databáze SQL – elastických fondů](sql-database-dtu-resource-limits-single-databases.md).
