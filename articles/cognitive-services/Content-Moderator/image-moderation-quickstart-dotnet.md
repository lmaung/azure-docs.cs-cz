---
title: 'Rychlý start: Díky analýze obrázků na problematický obsah v C# – Content Moderator'
titlesuffix: Azure Cognitive Services
description: Postup analýzy různého problematického materiálu v obsahu obrázků pomocí sady Content Moderator SDK pro .NET
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: bd281191deb1a82d4b1b5247a011d9867f0c9bc4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55879288"
---
# <a name="quickstart-analyze-image-content-for-objectionable-material-in-c"></a>Rychlý start: Analyzovat obsah obrázků pro nežádoucím vC#

Tento článek obsahuje informace a vzorové kódy, které vám pomůžou začít používat [sadu Content Moderator SDK pro .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/). Dozvíte se, jak hledat nevhodný obsah nebo obsah pro dospělé, extrahovatelný text a lidské tváře za účelem moderování potenciálně problematického materiálu.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete. 

## <a name="prerequisites"></a>Požadavky

- Klíč předplatného Content Moderatoru. Podle pokynů v tématu [Vytvoření účtu služeb Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) se přihlaste k odběru Content Moderatoru a získejte svůj klíč.
- Libovolná edice sady [Visual Studio 2015 nebo 2017](https://www.visualstudio.com/downloads/)


> [!NOTE]
> V této příručce se používá předplatné Content Moderatoru úrovně Free. Informace o tom, co je součástí jednotlivých úrovní předplatného, najdete na stránce [Ceny a omezení](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Vytvoření projektu sady Visual Studio

1. V sadě Visual Studio vytvořte nový projekt **Konzolová aplikace (.NET Framework)** a pojmenujte ho **ImageModeration**. 
1. Pokud vaše řešení obsahuje i jiné projekty, vyberte tento projekt jako jediný spouštěný projekt.
1. Získejte požadované balíčky NuGet. Klikněte pravým tlačítkem na svůj projekt v Průzkumníku řešení a vyberte **Spravovat balíčky NuGet**. Potom vyhledejte a nainstalujte následující balíčky:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-image-moderation-code"></a>Přidání kódu pro moderování obrázků

Dále zkopírováním kódu z této příručky a jeho vložením do svého projektu implementujete základní scénář moderování obsahu.

### <a name="include-namespaces"></a>Zahrnutí oborů názvů

Na začátek souboru *Program.cs* přidejte následující příkazy `using`.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=1-8)]

### <a name="create-the-content-moderator-client"></a>Vytvoření klienta Content Moderatoru

Do souboru *Program.cs* přidejte následující kód, který pro vaše předplatné vytvoří zprostředkovatele klienta Content Moderatoru. Přidejte kód společně s třídou **Program** do stejného oboru názvů. Budete muset aktualizovat pole **AzureRegion** a **CMSubscriptionKey** hodnotami identifikátoru oblasti a klíče předplatného.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=84-107)]


### <a name="set-up-input-and-output-targets"></a>Nastavení vstupních a výstupních cílů

Do třídy **Program** v souboru _Program.cs_ přidejte následující statická pole. Tato pole určují soubory pro vstupní obsah obrázku a výstupní obsah JSON.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=49-53)]

Budete muset vytvořit vstupní soubor *ImageFiles.txt* a odpovídajícím způsobem aktualizovat jeho cestu (relativní cesty jsou relativní vzhledem k adresáři provádění). Otevřete soubor _ImageFiles.txt_ a přidejte do něj adresy URL obrázků, které se mají moderovat. V tomto rychlém startu se jako ukázkový vstup používají následující adresy URL.

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="create-a-class-to-handle-results"></a>Vytvoření třídy pro zpracování výsledků

Do souboru *Program.cs* přidejte následující kód společně s třídou **Program** do stejného oboru názvů. Instanci této třídy použijete k zaznamenávání výsledků moderování jednotlivých kontrolovaných obrázků.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=109-124)]


### <a name="define-the-image-evaluation-method"></a>Definování metody pro hodnocení obrázků

Do třídy **Program** přidejte následující metodu. Tato metoda ohodnotí jeden obrázek třemi různými způsoby a vrátí výsledky hodnocení. Další informace o tom, co jednotlivé operace dělají, najdete na odkazu v části [Další kroky](#next-steps).

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=55-81)]

### <a name="load-the-input-images"></a>Načtení vstupních obrázků

Do metody **Main** ve třídě **Program** přidejte následující kód. Tím se program nastaví tak, aby pro všechny adresy URL obrázků ve vstupním souboru načetl data hodnocení. Tato data pak zapíše do jednoho výstupního souboru.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/image-moderation-quickstart-dotnet.cs?range=17-46)]

## <a name="run-the-program"></a>Spuštění programu

Program zapíše řetězcová data JSON do souboru _ModerationOutput.json_. Pro ukázkové obrázky použité v tomto rychlém startu bude výstup následující. Všimněte si, že oba obrázky mají různé části `ImageModeration`, `FaceDetection` a `TextDetection`, které odpovídají třem voláním rozhraní API v metodě **EvaluateImage**.

```json
[{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
  "ImageModeration": {
    "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
    "adultClassificationScore": 0.019196987152099609,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.032390203326940536,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "116"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "12"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
    "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
    "language": "eng",
    "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
    "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
    "result": false,
    "count": 0,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "11"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "faces": []
  }
},
{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
  "ImageModeration": {
    "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
    "adultClassificationScore": 0.0035635426174849272,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.021369094029068947,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "109"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "46"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
    "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
    "language": "eng",
    "text": "",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
    "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
    "result": true,
    "count": 6,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "60"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "faces": [
      {
        "bottom": 598,
        "left": 44,
        "right": 268,
        "top": 374
      },
      {
        "bottom": 620,
        "left": 308,
        "right": 532,
        "top": 396
      },
      {
        "bottom": 575,
        "left": 594,
        "right": 773,
        "top": 396
      },
      {
        "bottom": 563,
        "left": 812,
        "right": 955,
        "top": 420
      },
      {
        "bottom": 611,
        "left": 972,
        "right": 1151,
        "top": 432
      },
      {
        "bottom": 510,
        "left": 1232,
        "right": 1456,
        "top": 286
      }
    ]
  }
}]
```

## <a name="next-steps"></a>Další postup

V tomto rychlém startu jste vytvořili jednoduchou aplikaci v .NET, která s využitím služby Content Moderator vrací relevantní informace o poskytnutém ukázkovém obrázku. Dále si můžete přečíst další informace o tom, co znamenají různé příznaky a klasifikace, abyste se mohli rozhodnout, jaká data potřebujete a jak by je vaše aplikace měla zpracovávat.

> [!div class="nextstepaction"]
> [Příručka moderování obrázků](image-moderation-api.md)
