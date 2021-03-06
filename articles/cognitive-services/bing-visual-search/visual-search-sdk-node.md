---
title: 'Rychlý start: Získejte přehledy obrázků pomocí sady SDK vizuální vyhledávání Bingu pro Node.js'
titleSuffix: Azure Cognitive Services
description: Nahrát image pomocí sady SDK vizuální vyhledávání Bingu a získávat přehledy o něm.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/18/2018
ms.author: v-gedod
ms.openlocfilehash: c46c33b81a1162bdb30edc7a39a04fea8c0abb7d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866929"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-nodejs"></a>Rychlý start: Získejte přehledy obrázků pomocí sady SDK vizuální vyhledávání Bingu pro Node.js

V tomto rychlém startu pomocí zahájíte získat přehledy obrázků ze služby pro vizuální vyhledávání Bingu, pomocí sady Node.js SDK. Zatímco kompatibilní s Většina programovacích jazyků rozhraní REST API pro vizuální vyhledávání Bingu, sada SDK poskytuje snadný způsob, jak do svých aplikací integrovat službu. Zdrojový kód pro tuto ukázku můžete najít na [Githubu](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/visualSearch.js). 

## <a name="prerequisites"></a>Požadavky
* [Node.js](https://www.nodejs.org/)
* Vizuální vyhledávání Bingu sady SDK pro Node.js
    * Chcete-li nastavit aplikaci konzoly pomocí sady SDK vizuální vyhledávání Bingu, spusťte následující příkazy:
        1. `npm install ms-rest-azure`
        2. `npm install azure-cognitiveservices-search-visualSearch`.


[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

<a name="client"></a>

## <a name="create-and-initialize-the-application"></a>Vytvoření a inicializace aplikace

1. Vytvořte nový soubor JavaScript ve vašich oblíbených prostředím IDE nebo editorem a přidejte následující požadavky. Vytvořte proměnné pro váš klíč předplatného, ID konfigurace vlastní a cesta k souboru na obrázek, který chcete nahrát. 

    ```javascript
    const os = require("os");
    const async = require('async');
    const fs = require('fs');
    const Search = require('azure-cognitiveservices-visualsearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    
    let keyVar = 'YOUR-VISUAL-SEARCH-ACCESS-KEY';
    let credentials = new CognitiveServicesCredentials(keyVar);
    let filePath = "../Data/image.jpg";
    ```

2. Vytvořit instanci klienta.

    ```javascript
    let visualSearchClient = new Search.VisualSearchClient(credentials);
    ```

## <a name="search-for-images"></a>Vyhledávání obrázků

1. Použití `fs.createReadStream()` ke čtení v souboru bitové kopie a vytváření proměnných pro požadavek hledání a výsledky. Klient pak použijte pro vyhledávání obrázků.

    ```javascript
    let fileStream = fs.createReadStream(filePath);
    let visualSearchRequest = JSON.stringify({});
    let visualSearchResults;
    try {
        visualSearchResults = await visualSearchClient.images.visualSearch({
            image: fileStream,
            knowledgeRequest: visualSearchRequest
        });
        console.log("Search visual search request with binary of image");
    } catch (err) {
        console.log("Encountered exception. " + err.message);
    }
    ```

2. Parsujte výsledky předchozího dotazu:

    ```javascript
    // Visual Search results
    if (visualSearchResults.image.imageInsightsToken) {
        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
    }
    else {
        console.log("Couldn't find image insights token!");
    }
    
    // List of tags
    if (visualSearchResults.tags.length > 0) {
        let firstTagResult = visualSearchResults.tags[0];
        console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);
    
        // List of actions in first tag
        if (firstTagResult.actions.length > 0) {
            let firstActionResult = firstTagResult.actions[0];
            console.log(`First tag action count: ${firstTagResult.actions.length}`);
            console.log(`First tag action type: ${firstActionResult.actionType}`);
        }
        else {
            console.log("Couldn't find tag actions!");
        }
    
    }
    else {
        console.log("Couldn't find image tags!");
    }
    
    ```

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Vytvoření webové jednostránkové aplikace](tutorial-bing-visual-search-single-page-app.md)
