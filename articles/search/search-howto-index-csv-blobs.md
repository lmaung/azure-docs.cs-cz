---
title: Indexování objektů BLOB CSV pomocí indexeru Azure Search Blob – Azure Search
description: Procházení objektů BLOB CSV v úložišti objektů Blob v Azure pro fulltextové vyhledávání pomocí indexu Azure Search. Indexery můžete automatizovat příjem dat pro vybrané zdroje dat jako úložiště objektů Blob v Azure.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: eac0c1f2f7ded35cf09eec12f0406c754621f49c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465458"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexování objektů BLOB CSV pomocí indexeru Azure Search blob
Ve výchozím nastavení [indexeru Azure Search blob](search-howto-indexing-azure-blob-storage.md) analyzuje oddělený text objektů BLOB jako jediný neodkazovaný blok textu. Nicméně s objekty BLOB, který obsahuje data ve formátu CSV, často chcete zpracovávat každý řádek v objektu blob jako samostatný dokument. Například směru následující text s oddělovači, můžete k analýze do dva dokumenty a každý obsahuje pole "tags", "datePublished" a "id": 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

V tomto článku se dozvíte, jak analyzovat objektů BLOB CSV pomocí indexeru Azure Search blob. 

> [!IMPORTANT]
> Indexování objektů blob CSV je aktuálně ve verzi public preview a neměli byste používat v produkčním prostředí. Další informace najdete v tématu [REST api-version = 2017-11-11-Preview](search-api-2017-11-11-preview.md). 
> 

## <a name="setting-up-csv-indexing"></a>Nastavení indexování sdíleného svazku clusteru
Indexování objektů BLOB CSV, vytvořit nebo aktualizovat definici rozhraní indexeru se `delimitedText` analýze režimu:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Podrobné informace o rozhraní API pro vytvoření indexeru, projděte si [vytvoření indexeru](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` Označuje, že první řádek (Neprázdné) každý objekt blob obsahuje záhlaví.
Pokud objekty BLOB není obsahovat řádek záhlaví počáteční, záhlaví musí být zadán v konfigurace indexeru: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Můžete přizpůsobit pomocí znaku oddělovače `delimitedTextDelimiter` nastavení konfigurace. Příklad:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> V současné době je podporována pouze kódování UTF-8. Pokud potřebujete podporu pro jiné kódování, Hlasujte pro ni [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Při použití text oddělený znaky režim parsování Azure Search se předpokládá, že všechny objekty BLOB ve zdroji dat bude sdílený svazek clusteru. Pokud potřebujete podporovat kombinaci objekty BLOB sdíleného svazku clusteru a jiné než CSV v stejného zdroje dat, prosím Hlasujte pro ni [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Příklady žádosti
Vložení to všechny najednou, tady jsou příklady úplnou datovou část. 

Zdroj dat: 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte požadavky na funkce nebo nápady na vylepšení, zadejte svůj vstup na [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

