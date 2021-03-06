---
title: Pochopit, jak použít Azure digitální dvojče odkaz Swagger | Dokumentace Microsoftu
description: Ke zjištění, jak používat Azure digitální dvojče Swagger referenční dokumentaci.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/31/2018
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 9a1d328f79405b14ffd84e07cb915566bd686c8e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120950"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Digitální dvojče Swagger referenční dokumentace k Azure

Každá instance zřízené digitální dvojče Azure zahrnuje vlastní automaticky generované referenční dokumentaci Swagger.

[Swagger](https://swagger.io/), nebo [OpenAPI](https://www.openapis.org/), sjednotí komplexní informace o rozhraní API do prostředek referenční dokumentace pro interaktivní a jazykově nezávislé. Swagger obsahuje důležité referenční materiály, o které datové části JSON, metod HTTP a konkrétní koncové body používat k provedení operací k rozhraní API.

> [!IMPORTANT]
> Podpora pro ověřování Swaggeru je dočasně zakázané ve verzi Public Preview.

## <a name="swagger-summary"></a>Souhrn swagger

Swagger poskytuje interaktivní přehled vašeho rozhraní API, která zahrnuje:

* Informace o rozhraní API a objekt modelu.
* Koncových bodů rozhraní REST API, které zadejte požadované žádosti datových částí, záhlaví, parametry, kontext cesty a metody HTTP.
* Testování funkce rozhraní API.
* Informace o odpovědi příklad používá k ověření a potvrďte odpovědi protokolu HTTP.
* Informace o kódu chyby.

Swagger je praktický nástroj, který vám pomůže s vývoj a testování volání rozhraní API Správce Dvojčat digitální Azure.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>Referenční materiál

Automaticky generované referenční materiál Swagger poskytuje rychlý přehled důležitých konceptů, k dispozici koncové body rozhraní API pro správu a popis jednotlivých objektový model pro vývoj a testování.

Stručný přehled popisuje rozhraní API.

![Horní swagger][1]

Správa API objektové modely jsou také uvedeny.

![Modely swagger][2]

Můžete vybrat jednotlivých uvedených objektový model pro podrobnější přehled klíčových atributů.

![Swagger model][3]

Vygenerovaný objektové modely Swaggeru je pohodlné zobrazíte všechny dostupné Azure digitální dvojče [objekty a rozhraní API](./concepts-objectmodel-spatialgraph.md). Při vytváření řešení na Azure digitální dvojče mohou vývojáři použít tento prostředek.

## <a name="endpoint-summary"></a>Souhrn koncového bodu

Swagger obsahuje také důkladný přehled všechny koncové body, které tvoří rozhraní API pro správu.

Každý koncový bod uvedené také zahrnuje požadovaná žádost o informace, jako:

* Požadované parametry.
* Povinný parametr datové typy.
* Metoda HTTP pro přístup k prostředku.

![Swagger koncových bodů][4]

Pokud chcete zobrazit podrobnější přehled, vyberte každého prostředku.

## <a name="use-swagger-to-test-endpoints"></a>Využití Swaggeru k testování koncových bodů

Jednou výkonné funkce, které poskytuje Swaggeru je schopnost testování koncového bodu přímo prostřednictvím uživatelského rozhraní v dokumentaci k rozhraní API.

Když vyberete určitý koncový bod, zobrazí se **vyzkoušet**.

![Zkuste swagger][5]

Rozbalte tuto část, abyste vyvolali vstupní pole pro každý parametr požadované a volitelné. Zadejte správné hodnoty a vyberte **Execute**.

![Pokusili swagger][6]

Po spuštění testu můžete ověřit data odpovědi.

## <a name="swagger-response-data"></a>Data odpovědi swagger

Každý koncový bod uvedené také zahrnuje data těla odpovědi k ověření pro vývoj a testování. Tyto příklady: stavový kód a JSON, které chcete zobrazit pro úspěšné požadavky HTTP.

![Odpověď swagger][7]

Příklady také kódy chyb umožňující ladit nebo vylepšit selhání testů.

## <a name="swagger-oauth-20-authorization"></a>Swagger autorizace OAuth 2.0

Další informace o interaktivním testování požadavků, které jsou chráněné pomocí OAuth 2.0, najdete v článku [oficiální dokumentaci](https://swagger.io/docs/specification/authentication/oauth2/).

> [!NOTE]
> Podpora pro ověřování OAuth 2.0 je dočasně zakázané ve verzi Public Preview.

## <a name="next-steps"></a>Další postup

- Přečtěte si další informace o Azure digitální dvojče objektové modely a Prostorové řady grafu, přečtěte si [Principy Dvojčat digitální Azure objektu modely](./concepts-objectmodel-spatialgraph.md).

- Zjistěte, jak ověřování pomocí rozhraní API pro správu, přečtěte si téma [ověřování pomocí rozhraní API](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG
