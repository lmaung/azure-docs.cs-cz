---
title: Kognitivní vyhledávání map rozšiřují vstupní pole pro výstupní pole – Azure Search
description: Extrahovat a rozšiřte pole zdroje dat a mapování pro výstupní pole v indexu Azure Search.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: dd62119b01465392a92c7e68231fed8027b04da2
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2019
ms.locfileid: "56750527"
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Jak namapovat bohatších možností pole prohledávatelný index

V tomto článku se dozvíte, jak mapovat bohatších možností vstupní pole pro výstupní pole v prohledávatelný index. Jakmile budete mít [definované dovedností](cognitive-search-defining-skillset.md), je třeba namapovat pole výstup všechny dovednosti, které přímo přispívá hodnoty pro dané pole v indexu vyhledávání. Mapování polí jsou požadovány pro přesun obsahu z bohatších možností dokumentů do indexu.


## <a name="use-outputfieldmappings"></a>Použití outputFieldMappings
Chcete-li mapování polí, přidejte `outputFieldMappings` do definice indexeru jak je znázorněno níže:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

Text žádosti strukturovaná následujícím způsobem:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Pro každé pole výstup mapování, nastavte název pole bohatších možností (sourceFieldName) a název pole, který jste použili v indexu (targetFieldName).

Cesta v sourceFieldName může představovat jeden element nebo víc elementů. V příkladu výše ```/document/content/sentiment``` představuje jednu číselnou hodnotu, zatímco ```/document/content/organizations/*/description``` představuje několik popis organizace. V případech, kdy existuje několik elementů, jejich se "sloučí" do pole, které obsahuje všechny prvky. Více namítají pro ```/document/content/organizations/*/description``` například dat v *popisy* pole může vypadat třeba bez stromové struktury pole popisů před jejich indexováním:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Další postup
Jakmile bohatších možností pole jsou namapovány na prohledávatelná pole, můžete nastavit atributy pole pro každý prohledávatelná pole [jako součást definice indexu](search-what-is-an-index.md).

Další informace o mapování polí najdete v tématu [mapování polí v indexerech Azure Search](search-indexer-field-mappings.md).
