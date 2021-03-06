---
title: 'Azure Cosmos DB: Hromadně prováděcí modul .NET API, SDK a zdroje informací'
description: Přečtěte si všechno o hromadné prováděcí modul .NET API a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi sady Azure Cosmos DB hromadné prováděcí modul .NET SDK.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: 76a40c7e9f32deea798441ce53be7c7ef262e2bd
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2019
ms.locfileid: "55770665"
---
# <a name="net-bulk-executor-library-download-information"></a>Knihovna .NET hromadné prováděcí modul: Stažení informací 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Kanál změn .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Poskytovatel prostředků REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Prováděcí modul hromadného – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Prováděcí modul hromadného – Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
| **Popis**| Knihovna prováděcí modul hromadného umožňuje klientské aplikace provádět hromadné operace s účty služby Azure Cosmos DB. Hromadné prováděcí modul knihovny poskytuje hromadný import BulkUpdate a hromadného odstranění obory názvů. Hromadný import, které můžete hromadně modul ingestování dokumenty optimálního tak, že je zajištěné propustnosti pro kolekce spotřebované na maximální velikost. BulkUpdate, které modul můžete hromadně aktualizovat stávající data v kontejnerech služby Azure Cosmos DB jako opravy. Modul hromadného odstranění můžete hromadně odstranění dokumentů optimálního tak, že je zajištěné propustnosti pro kolekce spotřebované na maximální velikost.|
|**Stažení sady SDK**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **BulkExecutor knihovny v Githubu**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**Dokumentace k rozhraní API**|[Referenční dokumentace rozhraní .net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**Začínáme**|[Začínáme s knihovnou prováděcí modul hromadné sady .NET SDK](bulk-executor-dot-net.md)|
| **Aktuální podporované architektury**| Rozhraní Microsoft .NET Framework 4.5.2, 4.6.1 a .NET Standard 2.0 |

## <a name="release-notes"></a>Poznámky k verzi

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Včetně MongoBulkExecutor podpoře .NET Standard 2.0. Tato funkce je funkčně srovnatelný s 1.3.0 vydání, a uveďte podporuje .NET Standard 2.0 jako cílový rámec.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* Přidání .NET Standard 2.0 jako jeden z podporovaných cílových platforem na nastavit knihovnu BulkExecutor práce s aplikacemi .NET Core.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Přidat přetížení pro operaci hromadného odstranění pro účty SQL API tak, aby přijímal klíč oddílu, n-tic id dokumentu pro odstranění.
* Přidat přetížení pro operaci hromadného odstranění pro účty SQL API tak, aby přijímal RequestOptions, který obsahuje klíč oddílu, který zadáte jako hodnotu klíče oddílu, kromě použití jako filtr ve vstupní dotaz Určuje dokumenty pro odstranění.
* Opravili jsme problém, která způsobila chybu formátování v uživatelský agent používá BulkExecutor.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Provedli zlepšování BulkExecutor importu a aktualizace rozhraní API umožní transparentně reagovat na elastické škálování kontejneru Cosmos DB, když je úložiště větší než aktuální kapacita bez vyvolání výjimky.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Sadu DocumentDB .NET SDK závislosti na verzi 2.1.3.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Opravili jsme problém, která způsobila BulkExecutor vyvolání JSRT Chyba při importu do pevné kolekce.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Přidání podpory pro operaci hromadného odstranění pro účty SQL API služby Azure Cosmos DB.
* Přidání podpory pro hromadný import operaci pro účty s rozhraním API služby Azure Cosmos DB pro MongoDB.
* Sadu závislostí sady DocumentDB .NET SDK verze 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Přidání podpory pro hromadný import operaci pro účty Azure Cosmos DB Gremlin API.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Menší opravy hromadný import operací pro účty SQL API služby Azure Cosmos DB.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Přidání podpory pro hromadný import a BulkUpdate operací pro účty SQL API služby Azure Cosmos DB.

## <a name="next-steps"></a>Další postup

Další informace o knihovně hromadné prováděcí modul Java, najdete v následujícím článku:

[Knihovna Java hromadné prováděcí modul informace o sadě SDK a vydání](sql-api-sdk-bulk-executor-java.md)
