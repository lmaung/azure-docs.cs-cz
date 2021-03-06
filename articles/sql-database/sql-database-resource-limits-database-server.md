---
title: Omezení prostředků serveru Azure SQL Database | Dokumentace Microsoftu
description: Tento článek obsahuje přehled serveru Azure SQL Database omezení prostředků pro izolované databáze a elastické fondy. Poskytuje také informace o co se stane, když jsou zásahů nebo překročení těchto omezení prostředků.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan,moslake
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 670ca1b8ba16122d4e969a41f8679e1a6d1b27c6
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990100"
---
# <a name="sql-database-resource-limits-for-azure-sql-database-server"></a>Limity prostředků SQL Database pro server Azure SQL Database

Tento článek obsahuje přehled služby SQL Database omezení prostředků pro server služby SQL Database, která spravuje izolované databáze a elastických fondů. Poskytuje také informace o co se stane, když jsou zásahů nebo překročení těchto omezení prostředků.

> [!NOTE]
> Spravované instance omezení najdete v tématu [limity pro spravované instance SQL Database prostředků](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>Omezení maximální prostředků

| Prostředek | Omezení |
| :--- | :--- |
| Databází na serveru | 5000 |
| Výchozí počet serverů na předplatné v libovolné oblasti | 20 |
| Maximální počet serverů na předplatné v libovolné oblasti | 200 |  
| DTU nebo eDTU kvóta na serveru | 54,000 |  
| vCore kvóty podle serveru či instance | 540 |
| Maximální počet fondů na serveru | Počet je omezen počet Dtu nebo virtuálních jader. Například pokud každý fond je 1 000 Dtu, pak server podporují 54 fondy.|
||||

> [!NOTE]
> K získání další /eDTU kvóty DTU, vCore kvóty nebo více serverů než výchozí dobu, můžete odeslat novou žádost o podporu na webu Azure Portal pro předplatné se typ problému "Quota". DTU / limitu eDTU kvótu a databáze na serveru omezuje počet elastických fondů na jeden server.
> [!IMPORTANT]
> Jako počet databází, které se blíží limitu na serveru služby SQL Database, může dojít následujících:
> - Zvyšování latence ve spuštěném dotazy v hlavní databázi.  To zahrnuje zobrazení statistik využití prostředků, jako je například sys.resource_stats.
> - Zvyšuje latenci při operacích správy a vykreslování portálu přehledů, které se týkají databáze na serveru.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Co se stane, když je dosaženo omezení prostředků databáze

### <a name="compute-dtus-and-edtus--vcores"></a>COMPUTE (počet jednotek Dtu a Edtu / virtuálních jader)

Jakmile výpočetní využití databáze (měří podle počtu jednotek Dtu a Edtu nebo virtuálních jader) je vysoké, zvyšuje latence dotazu a může dokonce vypršet časový limit. Dotazů za těchto podmínek může být zařazené do fronty služby a jsou k dispozici prostředky pro spuštění jako prostředek stane zdarma.
Při zjištění vysokou výpočetní využití, možnosti omezení rizik:

- Zvětšení velikosti výpočetního databáze nebo elastického fondu pro databázi poskytnout další výpočetní prostředky. Zobrazit [škálování izolované databáze prostředků](sql-database-single-database-scale.md) a [škálování elastického fondu prostředků](sql-database-elastic-pool-scale.md).
- Optimalizace dotazů, aby se snížilo využití prostředků každého dotazu. Další informace najdete v tématu [dotazu ladění/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Když databáze využité dosáhne limitu maximální velikost, vloží databáze a aktualizací, které zvyšují velikost dat a klienti obdrží [chybová zpráva](sql-database-develop-error-messages.md). Databáze VYBERE a odstraní se nadále probíhat úspěšně.

Pokud dochází k vysoké využití, možnosti omezení rizik:

- Zvýšení maximální velikosti databáze nebo elastického fondu nebo přidat další úložiště. Zobrazit [škálování izolované databáze prostředků](sql-database-single-database-scale.md) a [škálování elastického fondu prostředků](sql-database-elastic-pool-scale.md).
- Pokud je databáze v elastickém fondu, pak také databázi lze přesunout mimo fond tak, aby její prostor úložiště se nesdílí s jinými databázemi.
- Zmenšit databázi uvolnění nevyužívaného místa. Další informace najdete v tématu [spravovat místo souborů ve službě Azure SQL Database](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>Relace a pracovních procesů (požadavků)

Maximální počet relací a pracovních procesů se určují podle úrovně služeb a vypočítat velikost (v jednotkách Dtu a Edtu). Nové požadavky byly zamítnuty, když je dosaženo omezení relace nebo pracovního procesu a klienti obdrží chybovou zprávu. Když počet připojení, které jsou k dispozici, které mohou být řízena aplikace, je často obtížné odhadnout a řídit počet souběžných pracovních procesů. To platí zejména během špiček zatížení kdy je dosaženo omezení prostředků databáze a pracovní procesy hromadí kvůli delší dobu spouštění dotazů.

Pokud dochází k vysoké využití relace nebo pracovního procesu, možnosti omezení rizik:

- Zvýšení službu úroveň nebo velikost databázi nebo elastický fond – compute úrovně. Zobrazit [škálování izolované databáze prostředků](sql-database-single-database-scale.md) a [škálování elastického fondu prostředků](sql-database-elastic-pool-scale.md).
- Optimalizace dotazů, aby se snížilo využití prostředků každého dotazu, je-li příčinou využití zvýšenou pracovního procesu je z důvodu kolize pro výpočetní prostředky. Další informace najdete v tématu [dotazu ladění/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Další postup

- Informace o obecných omezeních Azure najdete v tématu [předplatného Azure a limity, kvóty a omezení](../azure-subscription-service-limits.md).
- Informace o jednotkách Dtu a Edtu najdete v tématu [jednotkách Dtu a Edtu](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Informace o omezení velikosti databáze tempdb, naleznete v tématu [databáze TempDB ve službě Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
