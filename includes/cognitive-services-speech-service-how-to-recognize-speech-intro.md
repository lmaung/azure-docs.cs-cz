---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 31482cc7850c574b952c454021af729da324ba15
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2018
ms.locfileid: "52978764"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Cognitive Services [sadou SDK pro řeč](~/articles/cognitive-services/speech-service/speech-sdk.md) poskytuje nejjednodušší způsob, jak používat **převod řeči na Text** ve vaší aplikaci se všemi funkcemi.

1. Vytvořte konfiguraci řeči a zadejte klíč předplatného Speech service (nebo autorizační token) a [oblasti](~/articles/cognitive-services/speech-service/regions.md) jako parametry. Podle potřeby změňte konfiguraci. Například zadejte vlastní koncový bod k určení koncového bodu služby nestandardní, nebo vyberte mluvený jazyk vstupní nebo výstupní formát.

1. Vytvořte nástroj pro rozlišení řeči z konfigurace řeči. Pokud chcete zjistit z jiného zdroje než výchozí mikrofon (například zvukový datový proud nebo zvukový soubor), poskytují zvuku konfigurace.

1. V případě potřeby vytížit události pro asynchronní operaci. Nástroj pro rozpoznávání pak zavolá obslužné rutiny události je dočasný a finální výsledky. V opačném případě aplikace přijímá pouze určené k transkripci konečný výsledek.

1. Začněte rozpoznávání. Rozpoznávání jednorázová, jako je například rozpoznávání příkaz nebo dotaz, použijte `RecognizeOnceAsync()` (nebo ekvivalentní language) metody. Tato metoda vrátí první rozpoznaný utterance. Pro dlouho běžící rozpoznávání jako určené k transkripci, použijte `StartContinuousRecognitionAsync()` (nebo ekvivalentní language) metody. Spojit události pro asynchronní rozpoznávání výsledky.

Viz následující fragmenty kódu pro scénáře rozpoznávání řeči, používající sadou SDK pro řeč.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
