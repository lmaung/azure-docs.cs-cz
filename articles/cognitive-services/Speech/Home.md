---
title: Microsoft Bing Speech Service | Dokumentace Microsoftu
titlesuffix: Azure Cognitive Services
description: Pomocí Microsoft Speech API přidejte do svých aplikací, včetně v reálném čase interakci ze strany uživatelů akce ovládané řečí.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d2c7211831658a18e65e04aa753607f4eb22dac8
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673184"
---
# <a name="what-is-bing-speech"></a>Co je pro zpracování řeči Bingu?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Založené na cloudu Microsoft Bing Speech API poskytuje vývojářům snadný způsob, jak vytvořit výkonné funkce, které podporou řeči ve svých aplikacích, stejně jako ovládací prvek hlasových příkazů, dialogové okno uživatele pomocí konverzace přirozené řeči a přepis řeči a diktování. Rozhraní Speech API Microsoft podporuje obě *převod řeči na Text* a *převod textu na řeč* převodu.

- **Převod řeči na Text** rozhraní API převede lidské řeči na text, který může sloužit jako vstup nebo příkazy pro řízení vaší aplikace.
- **Převod textu na řeč** rozhraní API převede text na zvuk datové proudy, které se dají přehrávat na uživatele vaší aplikace.

## <a name="speech-to-text-speech-recognition"></a>Převod řeči na text (rozpoznávání řeči)

Rozhraní API pro rozpoznávání řeči Microsoft *transcribes* příkaz audiostreamy do textu, který vaše aplikace může zobrazit uživateli nebo adekvátně jako vstup. Poskytuje dva způsoby, jak vývojáři přidat rozpoznávání řeči do svých aplikací: REST API **nebo** ostatní klientské knihovny založené na protokolu Websocket.

- [Rozhraní REST API](GetStarted/GetStartedREST.md): Vývojáři mohou pomocí protokolu HTTP volání ze svých aplikací do služby pro rozpoznávání řeči.
- [Klientské knihovny](GetStarted/GetStartedClientLibraries.md): Pro pokročilé funkce můžete vývojáři stáhnout Microsoft Speech klientských knihoven a propojit do svých aplikací.  Klientské knihovny jsou dostupné na různých platformách (Windows, Android, iOS) používající různé jazyky (C#, Java, JavaScript, ObjectiveC). Na rozdíl od rozhraní REST API využívat klientské knihovny je protokol založený na protokolu Websocket.

| Případy použití | [Rozhraní REST API](GetStarted/GetStartedREST.md) | [Klientské knihovny](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Převést krátké mluvené, například příkazy (s zvuku délka < 15) bez prozatímní výsledky | Ano | Ano |
| Převod dlouhé zvuku (> 15 s) | Ne | Ano |
| Zvukový Stream s prozatímní požadované výsledky | Ne | Ano |
| Vysvětlení text převést z zvuk pomocí služby LUIS | Ne | Ano |

Podle toho, která vývojářům přístup zvolte (rozhraní REST API nebo klientských knihoven), služba speech Microsoft podporuje následující:

- Pokročilé technologie rozpoznávání řeči od Microsoftu, které jsou používány Cortanu, diktování Office, Office Translator a další produkty společnosti Microsoft.
- V reálném čase průběžné rozpoznávání. Rozhraní API pro rozpoznávání řeči umožňuje uživatelům přepisy zvuku na text v reálném čase a podporuje pro příjem mezivýsledků slov, která zatím byly rozpoznány. Služba speech také podporuje zjišťování end řeči. Kromě toho moci uživatelé zvolit další možnosti formátování, jako jsou malá a velká písmena a interpunkční znaménka, maskování vulgárních výrazů a normalizace text.
- Podporuje optimalizované výsledky rozpoznávání řeči pro *interaktivní*, *konverzace*, a *diktování* scénáře. Pro uživatelské scénáře, které vyžadují vlastní jazykové modely a akustických modelů [Custom Speech Service](../custom-speech-service/cognitive-services-custom-speech-home.md) umožňuje vytvářet modely řeči, které přizpůsobená pro vaši aplikaci a uživatele.
- Podporu mnoha jazyků mluvené slovo ve více dialektů. Úplný seznam podporovaných jazyků v jednotlivých režimech rozpoznávání najdete v tématu [rozpoznávání jazyků](api-reference-rest/supportedlanguages.md).
- Integrace se službou language understanding. Kromě převod vstupního zvuku na text, *převod řeči na Text* poskytuje aplikacím další schopností pochopit, co znamená text. Používá [Language Understanding Intelligent Service(LUIS)](../LUIS/what-is-luis.md) extrahovat z textové rozpoznaných záměry a entity.

### <a name="next-steps"></a>Další postup

- Začínáme používat službu rozpoznávání řeči Microsoft s [rozhraní REST API](GetStarted/GetStartedREST.md) nebo [klientské knihovny](GetStarted/GetStartedClientLibraries.md).
- Podívejte se na [ukázkové aplikace](samples.md) v svůj oblíbený programovací jazyk.
- Přejděte do části odkaz k nalezení [Microsoft řeči Protocol](API-Reference-REST/websocketprotocol.md) podrobnosti a Reference k rozhraní API.

## <a name="text-to-speech-speech-synthesis"></a>Převod textu na řeč (syntézu řeči)

*Převod textu na řeč* rozhraní API pro použití služby REST k převedení strukturovaných textových zvukový datový proud. Rozhraní API poskytují převod rychlé textu na řeč v různých jazycích a hlasy. Kromě toho také mít uživatelé možnost změnit zvukové charakteristiky jako výslovnost, svazek, od atd. pomocí SSML značky.

### <a name="next-steps"></a>Další postup

- Začínáme používat službu Microsoft převod textu na řeč: [Reference k rozhraní API pro převod textu na řeč](api-reference-rest/bingvoiceoutput.md). Úplný seznam jazyků a podporuje převod textu na řeč, naleznete v tématu [podporované národní prostředí a hlasová písma](api-reference-rest/bingvoiceoutput.md#SupLocales).
