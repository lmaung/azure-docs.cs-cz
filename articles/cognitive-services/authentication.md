---
title: Authentication
titleSuffix: Cognitive Services - Azure
description: 'Existují tři způsoby, jak ověřit požadavek na prostředek Azure Cognitive Services: klíč předplatného, nosný token nebo víc služeb předplatného. V tomto článku se dozvíte o jednotlivých metod a jak vytvořit žádost.'
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: erhopf
ms.openlocfilehash: 2f9b477e076b038a6a695952ee3f770b30ad179b
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429464"
---
# <a name="authenticate-requests-to-azure-cognitive-services"></a>Ověření požadavků ve službě Azure Cognitive Services

Každý požadavek do služby Azure Cognitive musí obsahovat hlavičku ověřování. Této hlavičky předá klíč předplatného nebo přístupový token, který se používá k ověření vašeho předplatného služby nebo skupiny služeb. V tomto článku se dozvíte o tři způsoby, jak ověřit požadavek a požadavky pro každý.

* [Ověření s klíčem jednoúčelovou předplatného](#authenticate-with-a-single-service-subscription-key)
* [Ověření s klíčem víc služeb předplatného](#authenticate-with-a-multi-service-subscription-key)
* [Ověřování pomocí tokenu](#authenticate-with-an-authentication-token)

## <a name="prerequisites"></a>Požadavky

Před provedením požadavku, budete potřebovat účet Azure a předplatné služeb Azure Cognitive Services. Pokud již máte účet, neváhejte a přejděte k další části. Pokud nemáte účet, máme Tento průvodce vám několika minut: [Vytvoření účtu služeb Cognitive Services pro Azure](cognitive-services-apis-create-account.md).

## <a name="authentication-headers"></a>Ověřovací hlavičky

Pojďme ověřovací hlavičky, které jsou k dispozici pro použití se službou Azure Cognitive Services rychle.

| Hlavička | Popis |
|--------|-------------|
| OCP-Apim-Subscription-Key | Tuto hlavičku používají k ověření pomocí klíče předplatného pro konkrétní službu nebo klíč víc služeb předplatného. |
| OCP-Apim předplatné – oblasti | Tato hlavička se pouze při použití klíče víc služeb předplatného se vyžaduje [Translator Text API](./Translator/reference/v3-0-reference.md). Tuto hlavičku používají k určení oblasti předplatného. |
| Autorizace | Tuto hlavičku používají, pokud používáte ověřovací token. V následujících oddílech jsou podrobně popsané kroky nutné k provedení výměny tokenu. Zadaná hodnota následující formát: `Bearer <TOKEN>`. |

## <a name="authenticate-with-a-single-service-subscription-key"></a>Ověření s klíčem jednoúčelovou předplatného

První možností je žádost o ověření pomocí klíče předplatného pro konkrétní službu, jako je Translator Text. Klíče jsou k dispozici na webu Azure Portal pro každý prostředek, který jste vytvořili. Žádost o ověření pomocí klíče předplatného, je nutné předají se jako `Ocp-Apim-Subscription-Key` záhlaví.

Tyto požadavky na ukázky, ukazuje, jak používat `Ocp-Apim-Subscription-Key` záhlaví. Mějte na paměti, při použití této ukázky, budete muset zahrnovat klíč platné předplatné.

Toto je ukázka volání rozhraní API webové vyhledávání Bingu:
```cURL
curl -X GET 'https://api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Toto je ukázka volání rozhraní Translator Text API:
```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

Toto video ukazuje použití klíče služeb Cognitive Services.

## <a name="authenticate-with-a-multi-service-subscription-key"></a>Ověření s klíčem víc služeb předplatného

>[!WARNING]
> V tuto chvíli tyto služby **není** podporovat víc služeb klíče: Nástroj QnA Maker, hlasových služeb a Custom Vision.

Tato možnost také používá k ověřování žádostí zasílaných klíč předplatného. Hlavní rozdíl je, že klíč předplatného se neváže ke konkrétní službě, jeden klíč můžete místo toho použije k ověření žádosti pro více služeb Cognitive Services. Zobrazit [ceny služeb Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) informace o místní dostupnosti, podporované funkce a ceny.

Klíč předplatného je součástí každého požadavku, jako `Ocp-Apim-Subscription-Key` záhlaví.

[![Víc služeb předplatného ukázku klíče pro služby Cognitive Services](./media/index/single-key-demonstration-video.png)](https://www.youtube.com/watch?v=psHtA1p7Cas&feature=youtu.be)

### <a name="supported-regions"></a>Podporované oblasti

Při použití klíče víc služeb předplatného na vytvořit žádost o `api.cognitive.microsoft.com`, musí obsahovat oblast v adrese URL. Například: `westus.api.cognitive.microsoft.com`.

Pokud používáte víc služeb předplatného klíč s rozhraním Translator Text API, je nutné zadat oblast předplatné s `Ocp-Apim-Subscription-Region` záhlaví.

Ověřování víc služeb se podporuje v těchto oblastech:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |


### <a name="sample-requests"></a>Požadavky na ukázky

Toto je ukázka volání rozhraní API webové vyhledávání Bingu:

```cURL
curl -X GET 'https://YOUR-REGION.api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Toto je ukázka volání rozhraní Translator Text API:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Ocp-Apim-Subscription-Region: YOUR_SUBSCRIPTION_REGION' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-an-authentication-token"></a>Ověřování pomocí tokenu ověření

Několik služeb Azure Cognitive Services přijmout a v některých případech vyžadovat, ověřovací token. Tyto služby v současné době podporují ověřování tokenů:

* Text Translation API
* Hlasové služby: Rozhraní REST Speech to text API
* Hlasové služby: Převod textu na řeč REST API

>[!NOTE]
> Nástroj QnA Maker také používá hlavičku autorizace, ale vyžaduje klíč rozhraní koncového bodu. Další informace najdete v tématu [QnA Maker: Získání odpovědí ze znalostní báze](./qnamaker/quickstarts/get-answer-from-kb-using-curl.md).

>[!WARNING]
> Služby, které podporují ověřování tokenů může v průběhu času měnit, prosím odkaz rozhraní API pro kontrolu služby před použitím této metody ověřování.

Obě jedné služby a klíče víc služeb předplatného může probíhat pro ověřování tokenů. Tokeny ověřování jsou platné po dobu 10 minut.

Tokeny ověřování jsou součástí žádost jako `Authorization` záhlaví. Poskytnutá hodnota tokenu musí předcházet párový příkaz `Bearer`, například: `Bearer YOUR_AUTH_TOKEN`.

### <a name="sample-requests"></a>Požadavky na ukázky

Pomocí této adresy URL k výměně klíče předplatného pro ověřovací token: `https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken`.

```cURL
curl -v -X POST \
"https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Content-length: 0" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

Tyto víc služeb regiony podporují průběhu výměny tokenů:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |

Po získání ověřovacího tokenu, je potřeba předat v každé žádosti o jako `Authorization` záhlaví. Toto je ukázka volání rozhraní Translator Text API:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Authorization: Bearer YOUR_AUTH_TOKEN' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="see-also"></a>Další informace najdete v tématech

* [Co je služba Cognitive Services?](welcome.md)
* [Ceny služeb cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/)
* [Vytvoření účtu](cognitive-services-apis-create-account.md)
