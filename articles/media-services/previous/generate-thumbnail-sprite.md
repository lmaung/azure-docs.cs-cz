---
title: Generování miniatur sprite pomocí Azure Media Services | Dokumentace Microsoftu
description: Toto téma ukazuje, jak generovat miniatury sprite pomocí Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: b4d760d8520f43b223665f17c85d3932761ebe17
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "56003104"
---
# <a name="generate-a-thumbnail-sprite"></a>Generování miniatur sprite  

Media Encoder Standard můžete použít ke generování miniatur sprite, což je soubor ve formátu JPEG, který obsahuje více miniatur malé rozlišení spojených dohromady do jedné image (velké) společně s VTT souboru. Tento soubor VTT Určuje časový rozsah v vstupního videa, která představuje jednotlivé miniatury, společně s velikostí a souřadnice této miniaturu v rámci velkých souborů ve formátu JPEG. Přehrávačů videa pomocí image Souborová služba a sprite VTT zobrazit "visual" seekbar prohlížeč zobrazovaly vizuální zpětnou vazbu, když scrubbingu zpět a vpřed podél časové osy videa.

Chcete-li použít k vygenerování Thumbnail Sprite, přednastavený kontext kodéru Media Encoder Standard:

1. Musíte použít formát JPG obrázek miniatury
2. Musíte zadat Start/krok/rozsah hodnot jako buď časová razítka nebo hodnoty % (a ne počty rámce) 
    
    1. Je možné kombinovat časová razítka a % hodnoty

3. Musí mít hodnotu SpriteColumn jako nezáporné číslo větší než nebo rovno 1

    1. Pokud SpriteColumn nastavená na M > = 1, obdélník se sloupci M je výstupní image. Pokud počet miniatur vygenerovanou přes #2 není násobkem M, poslední řádek bude nekompletní a levém s černým pixelů.  

Zde naleznete příklad:

```json
{
    "Version": 1.0,
    "Codecs": [
    {
      "Start": "00:00:01",
      "Type": "JpgImage",
      "Step": "5%",
      "Range": "100%",
      "JpgLayers": [
        {
          "Type": "JpgLayer",
          "Width": "10%",
          "Height": "10%",
          "Quality": 90
        }
      ],
      "SpriteColumn": 10
    }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
   ]
}
```

## <a name="known-issues"></a>Známé problémy

1.  Není možné vygenerovat sprite image s jeden řádek obrázků (SpriteColumn = 1 výsledky do image s jedním sloupcem).
2.  Bloků sprite imagí do středně velkých obrázků JPEG se ještě nepodporuje. Proto musí být věnovat pozornost omezit počet miniatur a jejich velikost tak, aby výsledný stitched Sprite miniaturu po 8 pixelech M nebo i rychleji.
3.  Azure Media Player podporuje prvky, které budou v prohlížeči Microsoft Edge, Chrome a Firefox. Analýza VTT není podporována v IE11.

## <a name="next-steps"></a>Další postup

[Kódování obsahu](media-services-encode-asset.md)
