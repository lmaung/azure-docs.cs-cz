---
title: 'Příklad: Volání rozhraní API image Analyze - pro počítačové zpracování obrazu'
titlesuffix: Azure Cognitive Services
description: Přečtěte si, jak použít k volání rozhraní API pro počítačové zpracování obrazu REST v Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 01/20/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: abbdda96acabb44a225ce16b4750726d37ff86b0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55882858"
---
# <a name="example-how-to-call-the-computer-vision-api"></a>Příklad: Jak volat rozhraní API pro počítačové zpracování obrazu

V tomto průvodci si ukážeme, jak použít REST k volání rozhraní API pro počítačové zpracování obrazu. Ukázky jsou napsané v jazyce C# pomocí klientské knihovny rozhraní API pro počítačové zpracování obrazu a také jako volání HTTP s operacemi POST/GET. Na co se zaměříme:

-   Jak získat značky, popis a kategorie.
-   Jak získat informace o určité doméně (celebrity).

### <a name="Prerequisites">Požadavky</a> 
Adresa URL obrázku nebo cesta k místně uloženému obrázku.
  * Podporované metody zadávání znaků: Nezpracovaná binární ve formě application/octet-stream adresa URL obrázku nebo image
  * Podporované formáty bitové kopie: JPEG, PNG, GIF, BMP
  * Velikost souboru obrázku: Méně než 4MB
  * Dimenze bitové kopie: Větší než 50 × 50 pixelů
  
V následujících příkladech si ukážeme tyto funkce:

1. analýza obrazu, získání pole značek a vrácení popisu.
2. Analýza obrazu s použitím modelu určité domény (konkrétně modelu „celebrities“) a získání odpovídajících výsledků ve formátu JSON.

Funkce můžeme rozdělit na:

  * **Možnost 1:** Obor analýzy – analýza daný model
  * **Option Two:** Rozšířená analýza - analýza poskytnout další podrobnosti se [taxonomie kategorií 86](../Category-Taxonomy.md)
  
### <a name="Step1">Krok 1: Povolit volání rozhraní API</a> 
Ke každému volání rozhraní API pro počítačové zpracování obrazu potřebujete klíč předplatného. Klíč je potřeba předat buď jako parametr řetězce dotazu, nebo ho zadat v hlavičce požadavku. 

Informace o získání klíče předplatného najdete v tématu [Jak získat klíče předplatného](../Vision-API-How-to-Topics/HowToSubscribe.md
).

**1.** Předání klíče předplatného prostřednictvím řetězce dotazu je vidět v následujícím příkladu rozhraní API pro počítačové zpracování obrazu:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

**2.** Klíč předplatného také můžete zadat v hlavičce požadavku HTTP:

```ocp-apim-subscription-key: <Your subscription key>```

**3.** Pokud použijete klientskou knihovnu, předáte klíč předplatného prostřednictvím konstruktoru VisonServiceClient:

```var visionClient = new VisionServiceClient(“Your subscriptionKey”);```

### <a name="Step2">Krok 2: Nahrajte image do služby rozhraní API pro počítačové zpracování obrazu a vrátili se značky, popisy a celebrit</a>
Základní způsob, jak volat rozhraní API pro počítačové zpracování obrazu, spočívá přímo v nahrání obrázku. Provede se to odesláním požadavku POST s typem obsahu application/octet-stream společně s daty přečtenými z obrázku. V případě značek a popisu bude metoda odeslání stejná pro všechna volání rozhraní API pro počítačové zpracování obrazu. Jediný rozdíl bude v parametrech dotazu, které zadá uživatel. 

Tady je postup, jak získat k danému obrázku značky a popis:

**Možnost 1:** Získejte seznam "Tags" a jedné "Popis"
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```
```
using Microsoft.ProjectOxford.Vision;
using Microsoft.ProjectOxford.Vision.Contract;
using System.IO;

AnalysisResult analysisResult;
var features = new VisualFeature[] { VisualFeature.Tags, VisualFeature.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.AnalyzeImageAsync(fs, features);
}
```
**Druhá možnost:** Získání jenom seznamu „Tags“ (Značky) nebo jenom seznamu „Description“ (Popis):

###### <a name="tags-only"></a>Jenom značky:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Jenom popis:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```
### <a name="here-is-how-to-get-domain-specific-analysis-in-our-case-for-celebrities"></a>Tady je postup, jak získat analýzu určité domény (v našem případě jsou to celebrity).

**Možnost 1:** Obor analýzy – analýza daný model
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```
U této možnosti jsou všechny ostatní parametry dotazu (visualFeatures, details) neplatné. K zobrazení všech podporovaných modelů použijte: 
```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```
**Option Two:** Rozšířená analýza - analýza poskytnout další podrobnosti se [taxonomie kategorií 86](../Category-Taxonomy.md)

Pokud chcete kromě podrobností z jednoho nebo několika modelů určitých domén získat i obecnou analýzu obrázku, rozšířili jsme verzi 1 rozhraní API o parametr dotazu do modelů.
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```
Při volání této metody se napřed volá klasifikátor 86 kategorií. Pokud některé kategorie odpovídají známým/shodným modelům, bude následovat druhé volání klasifikátoru. Například pokud „details=all“ nebo „details“ obsahují „celebrities“, budeme po volání klasifikátoru 86 kategorií volat model celebrit, aby výsledek obsahoval kategorii osob. Tím se u uživatelů, kteří se zajímají o celebrity, zvýší v porovnání s první možností latence.

Všechny parametry dotazu verze 1 se v tomto případě budou chovat stejně.  Pokud není zadaná možnost visualFeatures=categories, bude povolena implicitně.

### <a name="Step3">Krok 3: Získávání a pochopení výstup JSON pro analyze & visualFeatures značky, popis =</a>

Tady je příklad:
```
  {
    “tags”: [
    {
    "name": "outdoor",
        "score": 0.976
    },
    {
    "name": "bird",
        "score": 0.95
    }
            ],
    “description”: 
    {
    "tags": [
    "outdoor",
    "bird"
            ],
    "captions": [
    {
    "text”: “partridge in a pear tree”,
            “confidence”: 0.96
    }
                ]
    }
  }
```
Pole   | Type  | Obsah
------|------|------|
Značky    | objekt    | Objekt nejvyšší úrovně pro pole značek.
tags[].Name | řetězec    | Klíčové slovo z klasifikátoru značek.
tags[].Score    | číslo    | Skóre spolehlivosti z intervalu 0 až 1.
description  | objekt   | Objekt nejvyšší úrovně pro popis.
description.tags[] |    řetězec  | Seznam značek.  Pokud vytvořený popisek není dostatečně spolehlivý, může mít volající k dispozici jenom značky.
description.captions[].text | řetězec    | Výraz, který popisuje obrázek.
description.captions[].confidence   | číslo    | Spolehlivost výrazu.

### <a name="Step4">Krok 4: Získávání a pochopení výstup JSON doménově specifické modely</a>

**Možnost 1:** Obor analýzy – analýza daný model

Výstupem bude pole značek. Tady je příklad:
```
  { 
    "result": [ 
      { 
    "name": "golden retriever", 
    "score": 0.98
    },
    { 
    "name": "Labrador retriever", 
    "score": 0.78
    }
              ]
  }
```

**Option Two:** Rozšířená analýza - analýza taxonomie kategorií 86 poskytnout další podrobnosti

Při použití druhé možnosti (rozšířená analýza) u modelů určitých domén je rozšířený typ vrácených kategorií. Příklad:
```
  {
    "requestId": "87e44580-925a-49c8-b661-d1c54d1b83b5",
    "metadata":     {
      "width": 640,
      "height": 430,
      "format": "Jpeg"
                    },
    "result": {
      "celebrities": 
      [
        {
        "name": "Richard Nixon",
        "faceRectangle": {
          "left": 107,
          "top": 98,
          "width": 165,
          "height": 165
                         },
        "confidence": 0.9999827
        }
      ]
  }
```

Pole kategorií představuje seznam jedné nebo několika z [86 kategorií](../Category-Taxonomy.md) původní taxonomie. Všimněte si také, že kategorie končící podtržítkem odpovídají dané kategorii a podřízeným prvkům (příklad: people_ a people_group v modelu celebrities).

Pole   | Type  | Obsah
------|------|------|
categories | objekt | Objekt nejvyšší úrovně
categories[].name    | řetězec   | Název z taxonomie 86 kategorií
categories[].score  | číslo    | Skóre spolehlivosti z intervalu 0 až 1
categories[].detail  | objekt?      | Volitelné podrobnosti o objektu

Všimněte si, že pokud vyhovuje více kategorií (klasifikátor 86 kategorií například vrátí při použití modelu celebrities skóre pro people_ i people_young), připojí se podrobnosti k nejobecnější shodné značce (v tomto příkladu people_).

### <a name="Errors">Chybové odpovědi</a>
Jsou stejné jako v případě vision.analyze, ale obsahují další chybu NotSupportedModel (HTTP 400), která může být vrácena v obou scénářích (první i druhá možnost). Pokud při druhé možnosti (rozšířená analýza) nejsou rozpoznané žádné zadané modely, vrátí rozhraní API chybu NotSupportedModel i v případě platného modelu nebo více platných modelů.  Pokud uživatelé chtějí zjistit podporované modely, můžou volat listModels.

### <a name="Summary">Souhrn</a>

Základními funkcemi rozhraní API pro počítačové zpracování obrazu je nahrání obrázků a následné načtení cenných metadat.

Pokud chcete použít REST API, přejděte k [referenčním informacím k rozhraní API pro počítačové zpracování obrazu](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
 
