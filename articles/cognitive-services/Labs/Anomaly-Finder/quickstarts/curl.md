---
title: Jak používat rozhraní API pro vyhledávač anomálií pomocí cURL – Microsoft Cognitive Services | Dokumentace Microsoftu
description: Získat informace, které vám pomohou rychle začít používat cURL a rozhraní API vyhledávač anomálií ve službě Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 7a4996c83d57b34fcfcff43650b21359acb4e65e
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2019
ms.locfileid: "56730881"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>Použití rozhraní API vyhledávač anomálií pomocí cURL

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Tento článek obsahuje informace a ukázky kódu, které vám pomohou rychle začít používat rozhraní API pro vyhledávač anomálií pomocí cURL k provedení úlohy získání výsledku anomálií dat časových řad.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Začínáme s rozhraním API pro vyhledávač anomálií pomocí příkazu cURL anomálií body 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Příklad dat časových řad

Následuje příklad času datových bodů řady.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Analýza dat a získat body anomálií příklad cURL

Kroky v příkladu jsou následující.

1. Hodnotu `[YOUR_SUBSCRIPTION_KEY]` nahraďte platným klíčem předplatného.
2. Nahradit `[YOUR_REGION]` a použít tak umístění, kde jste získali klíče předplatného.
3. Nahradit `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` pomocí příkladu nebo vlastních datových bodů.
4. Spustit a zkontrolovat odpovědi.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Příklad odpovědi
Úspěšná odpověď se vrátí ve formátu JSON. Příklad odpovědi je následujícím způsobem: [!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [REST API – referenční informace](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
