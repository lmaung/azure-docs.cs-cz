---
title: Odesílání žádostí o hledání do API pro vyhledávání videí Bingu
titlesuffix: Azure Cognitive Services
description: Přečtěte si o odesílání vyhledávacích dotazů do API pro vyhledávání videí Bingu.
services: cognitive-services
author: aahi
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: aahill
ms.openlocfilehash: e9df2f9cf7a8ecabdce9b5bc877fdc82fd61d1f5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55865926"
---
# <a name="sending-search-requests-to-the-bing-video-search-api"></a>Odesílání žádostí o hledání do API pro vyhledávání videí Bingu

Tento článek popisuje parametry a atributy požadavky odeslané na rozhraní API Bingu pro vyhledávání videa, stejně jako objekt JSON odpovědi se vrátí.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="suggest-search-terms-with-the-bing-autosuggest-api"></a>Navrhnout hledané výrazy s rozhraním API pro automatické návrhy Bingu

Pokud nabízíte vyhledávací pole, do kterého může uživatel zadat hledaný termín, můžete hledání vylepšit s využitím [rozhraní API pro automatické návrhy Bingu](../../bing-autosuggest/get-suggested-search-terms.md). Toto rozhraní API vrací navrhované řetězce dotazů na základě částečné shody hledaných termínů zadávaných uživatelem.

Poté, co uživatel zadá své hledaný termín, kódování URL ho před nastavením [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query) parametr dotazu. Pokud uživatel například zadá *sailing dinghies*, nastavte parametr `q` na hodnotu `sailing+dinghies` nebo `sailing%20dinghies`.

## <a name="sending-a-request"></a>Odesílání požadavku

Pokud chcete získat výsledky hledání videa, odešlete požadavek GET na následující koncový bod:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```
   
Požadavek musí používat protokol HTTPS.

Doporučujeme, aby všechny požadavky pocházely ze serveru. Distribuce klíče v rámci klientské aplikace nabízí více příležitostí pro přístup kyberzločinců. Volání ze serveru také poskytuje jediný bod upgradu pro budoucí verze rozhraní API.

  
Požadavek musí obsahovat parametr dotazu [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query), který obsahuje hledaný termín daného uživatele. Přestože je volitelný, měl by požadavek obsahovat také parametr [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#mkt) dotazu, který identifikuje trh, ze kterého chcete obdržet výsledky. Seznam volitelných parametrů dotazu, jako `pricing`, naleznete v tématu [Parametry dotazu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters). Všechny hodnoty parametru dotazu musí mít kódování URL.  
  
Požadavek musí obsahovat hlavičku [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#subscriptionkey). Přestože jsou volitelné, doporučujeme, aby požadavek obsahoval i následující hlavičky:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#location)  

IP a hlavičky klienta jsou důležité pro vrácení obsahu závislého na umístění.  

Seznam všech hlaviček žádostí a odpovědí najdete v části s [hlavičkami](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#headers).

## <a name="example-search-request"></a>Příklad požadavku hledání

Následuje ukázka požadavku hledání, která obsahuje všechny navrhované parametry a hlavičky dotazu. Pokud voláte některé z rozhraní API Bingu poprvé, nezahrnujte do volání hlavičku ID klienta. ID klienta zahrňte pouze v případě, že jste již dříve volali rozhraní API Bingu a Bing vrátil ID klienta pro příslušnou kombinaci uživatele a zařízení. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

## <a name="example-json-response"></a>Příklad JSON odpovědi

Následující příklad ukazuje odpověď na předchozí požadavek. Příklad také zobrazuje hlavičky odpovědi specifické pro Bing.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D5694...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYWCvh...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjHZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56944...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjBZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y6...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E11E3CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        . . .
    ],
    "nextOffset" : 0,
    "pivotSuggestions" : [
        {
            "pivot" : "sailing",
            "suggestions" : []
        },
        {
            "pivot" : "dinghies",
            "suggestions" : [
                {
                    "text" : "Sailing Cruising",
                    "displayText" : "Cruising",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF754...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Sailing..."
                    }
                },
                . . .
            ]
        }
    ]
}
```

## <a name="next-steps"></a>Další postup

Vyzkoušejte si rozhraní API. Přejděte na téma [Testovací konzola rozhraní API pro vyhledávání videí](https://dev.cognitive.microsoft.com/docs/services/56b43f3ccf5ff8098cef3809/operations/58113fe5e31dac0a1ce6b0a8). 

Podrobnosti o využití objektů odpovědi naleznete v tématu [Hledání videí na webu](../search-the-web.md).

Podrobnosti o získání přehledů o videu, například související hledání, najdete v tématu [Přehledy videí](../video-insights.md).  
  
Podrobnosti o videích, která jsou populární na sociálních médiích, najdete v tématu [Populární videa](../trending-videos.md).  
