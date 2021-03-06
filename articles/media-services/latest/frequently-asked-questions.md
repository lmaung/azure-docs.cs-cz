---
title: Azure Media Services v3 – nejčastější dotazy | Dokumentace Microsoftu
description: Tento článek obsahuje odpovědi na Azure Media Services v3 – nejčastější dotazy.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/20/2019
ms.author: juliako
ms.openlocfilehash: c0dc67ddf0f1de9ca72fd14a9113219209a6bda0
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674206"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Nejčastější dotazy k Azure Media Services v3

Tento článek obsahuje odpovědi na nejčastější dotazy v3 Azure Media Services (AMS).

## <a name="v3-apis"></a>rozhraní API v3

### <a name="how-do-i-configure-media-reserved-units"></a>Jak nakonfigurovat rezervovaných jednotek médií?

Pro analýzu zvuku a videa analytických úloh, které jsou aktivovány Media Services v3 nebo Video Indexer důrazně doporučujeme pro účet zřídit s 10 použité položky S3. Pokud potřebujete více než 10 použité položky S3, otevřete lístek podpory pomocí [webu Azure portal](https://portal.azure.com/).

Podrobnosti najdete v tématu [škálování zpracování médií pomocí rozhraní příkazového řádku](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Co je doporučovaná metoda pro proces videa?

Použití [transformuje](https://docs.microsoft.com/rest/api/media/transforms) konfigurace běžné úlohy kódování nebo analyzovat videa. Každý **transformace** popisuje nebudou tím správným nebo pracovního postupu úloh zpracování videa nebo zvukových souborů. A [úlohy](https://docs.microsoft.com/rest/api/media/jobs) je skutečnou žádost služby Media Services použít **transformace** do daného vstupního videa nebo zvukový obsah. Po vytvoření transformace, můžete odeslat úlohy pomocí rozhraní API služby Media Services nebo libovolného z publikované sady SDK. Další informace najdete v tématu [transformuje a úlohy](transforms-jobs-concept.md).

### <a name="how-does-pagination-work"></a>Jak funguje stránkování?

Při použití stránkování, vždy používejte odkaz na další a výčet kolekce není závislý na konkrétní stránce velikost. Podrobnosti a příklady najdete v tématu [filtrování, řazení, stránkování](entities-overview.md).

## <a name="live-streaming"></a>Živé streamování 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Jak vložit konce, videa a obrázků slaty během živé streamování?

Služba Media Services v3 kódování v reálném čase zatím nepodporuje vkládání video nebo obrázkových slaty během živého datového proudu. 

Můžete použít [live encoder místní](recommended-on-premises-live-encoders.md) přepnutí zdrojového videa. Mnoho aplikací poskytují možnost přepnout zdrojů, včetně Telestream Wirecast, přepínání Studio (v Iosu), OBS Studio (bezplatnou aplikaci) a mnoho dalších.

## <a name="content-protection"></a>Ochrana obsahu

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Jak a kde se získat token JWT před jeho použitím žádosti o licenci nebo klíč?

1. Pro produkční prostředí musíte mít zabezpečení Token služby (STS) (webová služba) který vystaví token JWT na vyžádání protokolu HTTPS. Pro testování, můžete použít kód zobrazený v **GetTokenAsync** metody definované v [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. Přehrávač, bude nutné vytvořit žádost, po ověření uživatele, na službu STS pro takový token a přiřaďte ji jako hodnotu tokenu. Můžete použít [rozhraní API služby Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/).

* Příkladem s symetrické i asymetrické klíč služby tokenů zabezpečení, najdete v tématu [ http://aka.ms/jwt ](https://aka.ms/jwt). 
* Příklad přehrávač založené na Azure Media Player pomocí těchto tokenu JWT, naleznete v tématu [ http://aka.ms/amtest ](https://aka.ms/amtest) (expand na token vstup odkaz "player_settings").

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>Jak autorizujete požadavky na datový proud videa s využitím šifrování AES

Správný přístup je využívat služby tokenů zabezpečení (zabezpečení Token Service):

Služba tokenů zabezpečení v závislosti na profil uživatele, přidejte různé deklarace (například "Premium User", "Základní uživatel", "Uživatele bezplatné zkušební verze"). S jinou deklarací v token JWT uživatel uvidí jiný obsah. Samozřejmě pro jiný obsah nebo prostředek, bude mít ContentKeyPolicyRestriction odpovídající RequiredClaims.

Licence/klíč API služby Azure Media Services použijte ke konfiguraci doručování a šifruje vaše prostředky (jak je znázorněno v [této ukázce](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Další informace naleznete v tématu:

- [Přehled ochrany obsahu](content-protection-overview.md)
- [Návrh systému s více variantami DRM ochrany obsahu pomocí řízení přístupu](design-multi-drm-system-with-access-control.md)

## <a name="media-services-v2-vs-v3"></a>Media Services v2 a v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Můžete použít na webu Azure portal ke správě prostředků v3

Zatím ne. Můžete použít jednu z podporovaných sad SDK. Prohlédněte si kurzy a ukázky v tomto dokumentu.

### <a name="is-there-an-assetfile-concept-in-v3"></a>Je ve verzi 3 koncept AssetFile?

AssetFiles byly odebrány z rozhraní API pro AMS, aby bylo možné oddělit Media Services od závislostí sady SDK služby Storage. Teď úložiště, ne Media Services, uchová informace, které patří úložiště. 

Další informace najdete v tématu [migrace do služby Media Services v3](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Kde najdu šifrování na straně klienta úložiště?

Nyní se doporučuje použít šifrování úložiště na straně serveru (který je ve výchozím). Další informace najdete v tématu [šifrování služby Azure Storage pro neaktivní uložená Data](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Další postup

[Přehled služby Media Services v3](media-services-overview.md)
