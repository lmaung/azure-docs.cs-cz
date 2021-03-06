---
title: Import dat pro příjem dat do indexu vyhledávání – Azure Search
description: Naplnit a nahrát data z externích zdrojů dat do indexu ve službě Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 85a2810e8ab8de5ad2967aaf17f421d871368063
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958452"
---
# <a name="indexing-external-data-for-queries-in-azure-search"></a>Indexování externích dat u dotazů ve službě Azure Search

Ve službě Azure Search se dotazy provádějí nad obsahem nahrán a uložili v [indexu vyhledávání](search-what-is-an-index.md). Tento článek zkoumá dva základní přístupy pro naplňování indexu: *nabízených* vaše data do indexu prostřednictvím kódu programu, nebo přejděte [indexeru Azure Search](search-indexer-overview.md) na podporovaný zdroj dat k  *o přijetí změn* v datech.

U obou přístupu cílem je *načtení dat* z externího zdroje dat do indexu Azure Search. Služba Azure Search vám umožní vytvořit prázdný index, ale dokud nabízená nebo načítat data do ní není dotazovatelné.

## <a name="pushing-data-to-an-index"></a>Nabídka dat do indexu
Model Push, který slouží k odesílání dat do služby Azure Search prostřednictvím kódu programu, je nejflexibilnějším přístupem. Za prvé u něj neplatí žádná omezení týkající se typu zdroje dat. Do indexu Azure Search je možné nabídnout jakoukoli datovou sadu skládající se z dokumentů JSON za předpokladu, že každý dokumente v datové sadě obsahuje mapování polí na pole definovaná ve schématu vašeho indexu. Za druhé u něj neplatí žádná omezení týkající se četnosti provádění. Do indexu můžete nabízet změny, jak často chcete. U aplikací vyžadujících velmi nízkou latenci (např. když potřebujete, aby operace hledání byly synchronizované s dynamickými databázemi zásob) je model Push vaší jedinou možností.

Tento přístup je flexibilnější než model Pull, protože můžete nahrávat dokumenty samostatně nebo v dávkách (jednotlivé dávky můžou obsahovat až 1000 dokumentů nebo 16 MB, podle toho, kterého omezení dosáhnete dříve). Model Push vám taky umožňuje nahrát dokumenty do služby Azure Search bez ohledu na to, kde se data nacházejí.

### <a name="how-to-push-data-to-an-azure-search-index"></a>Nabídka dat do indexu Azure Search

Pomocí následujících rozhraní API můžete do indexu načíst jeden nebo několik dokumentů:

+ [Přidávání, aktualizace a odstraňování dokumentů (REST API)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [Třída indexAction](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) nebo [třída indexBatch](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Vkládání dat prostřednictvím portálu není aktuálně podporováno.

Úvod k jednotlivým metodologiím najdete v tématu [Import dat pomocí REST](search-import-data-rest-api.md) nebo [Import dat pomocí .NET](search-import-data-dotnet.md).


## <a name="pulling-data-into-an-index"></a>Přetáhnutí dat do indexu
Model Pull prochází podporovaný zdroj dat a automaticky nahrává data do vašeho indexu. Ve službě Azure Search je tato schopnost implementovaná prostřednictvím *indexerů*, které jsou aktuálně dostupné pro tyto platformy:

+ [Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Table Storage](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](https://aka.ms/documentdb-search-indexer)
+ [Azure SQL Database a SQL Server na virtuálních počítačích Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Indexery propojují index se zdrojem dat (obvykle tabulka, zobrazení nebo ekvivalentní struktura) a mapují pole zdroje na odpovídající pole v indexu. Během provádění je sada řádků automaticky převedena na formát JSON a načtena do určeného indexu. Všechny indexery podporují plánování, takže můžete určit, jak často se data budou aktualizovat. Většina indexerů umožňuje sledování změn dat, pokud ho zdroj dat podporuje. Indexery sledují změny a odstranění ve stávajících dokumentech a rozpoznávají nové dokumenty, a díky tomu není potřeba aktivně spravovat data v indexu. 


### <a name="how-to-pull-data-into-an-azure-search-index"></a>Přetáhnutí dat do indexu Azure Search

Funkce indexeru jsou přístupné pomocí webu [Azure Portal](search-import-data-portal.md), rozhraní [REST API](/rest/api/searchservice/Indexer-operations) a sady [.NET SDK](/dotnet/api/microsoft.azure.search.indexersoperationsextensions). 

Výhodou použití portálu je, že Azure Search většinou za vás dokáže vygenerovat výchozí schéma indexu podle metadat zdrojové datové sady. Vygenerovaný index můžete upravit až do zpracování indexu. Poté jsou povoleny jen takové změny schématu, které nevyžadují přeindexování. Pokud provedené změny přímo ovlivní schéma indexu, bude nutné index znovu sestavit. 

## <a name="verify-data-import-with-search-explorer"></a>Ověření importu dat pomocí Průzkumníka služby Search

Rychlý způsob, jak provést předběžnou kontrolu při odeslání dokumentu je použití **Průzkumníka služby Search** na portálu. Průzkumníka můžete použít k zadávání dotazů na index, aniž byste museli programovat. Funkce vyhledávání je založena na výchozím nastavení, jako je [jednoduchá syntaxe](/rest/api/searchservice/simple-query-syntax-in-azure-search) a výchozí [parametr dotazu searchMode](/rest/api/searchservice/search-documents). Výsledky jsou vráceny ve formátu JSON, abyste si mohli prohlédnout celý dokument.

> [!TIP]
> Jako výchozí bod můžete využít celou řadu [ukázek kódu pro Azure Search](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) zahrnujících vložené nebo snadno dostupné datové sady. Na portálu také najdete ukázkový indexer a zdroj dat, obsahující datovou sadu malé realitní kanceláře (s názvem realestate-us-sample). Když spustíte předkonfigurovaný indexer na vzorový zdroj dat, je index a načítají s dokumenty, které je pak možné zadávat dotazy v Průzkumníku služby Search nebo pomocí kódu, který píšete.

## <a name="see-also"></a>Další informace najdete v tématech

+ [Přehled indexeru](search-indexer-overview.md)
+ [Průvodce portálem: vytvoření, načtení a dotazování indexu](search-get-started-portal.md)
