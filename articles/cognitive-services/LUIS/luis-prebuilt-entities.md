---
title: Předem připravených entit
titleSuffix: Azure Cognitive Services
description: Služba LUIS zahrnuje sadu předem připravených entit pro rozpoznávání běžných typů informací, jako jsou data, časy, čísla, měření a měny. Podpora předem připravených entit se liší podle jazykové verze vaší aplikace LUIS.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 12/05/2018
ms.author: diberry
ms.openlocfilehash: ade09c7527e62112ac7225df21e25d4139d99e58
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878988"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Předem připravených entit rozpoznat běžné typy dat

Služba LUIS zahrnuje sadu předem připravených entit pro rozpoznávání běžných typů informací, jako jsou data, časy, čísla, měření a měny. 

## <a name="add-a-prebuilt-entity"></a>Přidat předem připravených entit

1. Otevřete aplikaci klepnutím na jeho název na **Moje aplikace** stránce a potom klikněte na tlačítko **entity** na levé straně. 

1. Na **entity** klikněte na **přidat předem připravených entit**.

1. V **přidání předem připravených entit** dialogového okna, vyberte datetimeV2 předem připravených entit. 

    ![Přidat – dialogové okno předem připravených entit](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Vyberte **Done** (Hotovo).

## <a name="publish-the-app"></a>Publikování aplikace

Nejjednodušší způsob, jak zobrazit hodnotu z předem připravených entit je dotaz z publikovaných koncového bodu. 

1. V horním panelu nástrojů vyberte **publikovat**. Publikovat do **produkční**. 

1. Jakmile se zobrazí oznámení zelené úspěchu, vyberte **najdete v seznamu koncových bodů** odkaz zobrazíte koncových bodů.

1. Vyberte koncový bod. Do tohoto koncového bodu se otevře na nové kartě prohlížeče. Ponechat otevřenou kartu prohlížeče a i nadále **Test** oddílu.

## <a name="test"></a>Test
Po přidání entity není potřeba trénování aplikace. 

Test nové záměr na koncovém bodu podle přidán hodnotu **q** parametru. Následující tabulku můžete použít pro navrhované projevy pro **q**:

|Utterance testu|Hodnota entity|
|--|:--|
|Kniha let zítra|2018-10-19|
|Zrušit 3. března|Služba LUIS vrátil posledního dne 3 v minulosti (2018-03-03) a dne 3 v budoucnu (2019-03-03) vzhledem k tomu, utterance neurčili, nevložily ročně.|
|Naplánovat schůzku v 10: 00|10:00:00|

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Označování entit, který obsahuje token předem připravených entit
 Pokud máte text, například `HH-1234`, kterou chcete označit jako vlastní entita _a_ máte [předem připravených číslo](luis-reference-prebuilt-number.md) přidá do modelu, není možné označit vlastní entity na portálu služby LUIS. Označte ji pomocí rozhraní API. 

 Pokud chcete aplikaci označit tento typ token, pokud jeho část je již označena s využitím předem připravených entit, odeberte z aplikace LUIS předem připravených entit. Není nutné k trénování aplikace. Označte token s vlastní entitu. Přidejte zpátky do aplikace LUIS předem připravených entit.

 Další příklad vezměte v úvahu utterance jako seznam předvolby třídy: `I want first year spanish, second year calculus, and fourth year english lit.` Pokud má aplikace LUIS řadová číslovka Prebuild přidali, `first`, `second`, a `fourth` se již označen atributem řadové číslovky. Pokud chcete zaznamenat řadová číslovka a třídy, můžete vytvořit složenou entitu a obtékat kolem pořadovém místě předem připravená a vlastní entity pro název třídy.

## <a name="next-steps"></a>Další postup
> [!div class="nextstepaction"]
> [Přehled předem připravených entit](./luis-reference-prebuilt-entities.md)
