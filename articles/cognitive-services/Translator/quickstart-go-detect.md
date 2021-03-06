---
title: 'Rychlý start: Identifikujte jazyk z textu, přejděte – Translator Text API'
titleSuffix: Azure Cognitive Services
description: V tomto rychlém startu identifikujete jazyk zdrojového textu pomocí rozhraní Translator Text API a jazyka Go.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 5b9b8632a16989b3451677aeab8a97109894969c
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2019
ms.locfileid: "56736125"
---
# <a name="quickstart-use-the-translator-text-api-to-detect-text-language-using-go"></a>Rychlý start: Zjistit jazyk textu pomocí jazyka Go pomocí rozhraní Translator Text API

V tomto rychlém startu budete zjistěte, jak zjistit jazyk zadaného textu s použitím Go a rozhraní REST Translator Text API.

K tomuto rychlému startu potřebujete [účet služby Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) s prostředkem služby Translator Text. Pokud účet nemáte, můžete k získání klíče předplatného použít [bezplatnou zkušební verzi](https://azure.microsoft.com/try/cognitive-services/).

## <a name="prerequisites"></a>Požadavky

K tomuto rychlému startu potřebujete:

* [Go](https://golang.org/doc/install)
* Klíč předplatného Azure pro službu Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Vytvoření projektu a import požadovaných modulů

Vytvoření nového projektu přejít pomocí Oblíbené prostředí IDE nebo editoru. Pak do svého projektu, do souboru s názvem `detect-language.go`, zkopírujte tento fragment kódu.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Vytvoření funkce main

Tato ukázka se pokusí přečíst klíč předplatného služby Translator Text z proměnné prostředí `TRANSLATOR_TEXT_KEY`. Pokud proměnné prostředí neznáte, můžete hodnotu `subscriptionKey` nastavit jako řetězec a okomentovat podmíněný příkaz.

Zkopírujte do svého projektu tento kód:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our detect function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    detect(subscriptionKey)
}
```

## <a name="create-a-function-to-detect-the-text-language"></a>Vytvoření funkce pro detekci jazyk textu

Pojďme vytvořit funkce, která se rozpoznat jazyk textu. Tato funkce se přijímají jediný argument, váš klíč předplatného Translator Text.

```go
func detect(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Dále můžeme se vytvoří adresa URL. Adresa URL se vytvořil pomocí `Parse()` a `Query()` metody.

Zkopírujte tento kód do `detect` funkce.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/detect?api-version=3.0")
q := u.Query()
u.RawQuery = q.Encode()
```

>[!NOTE]
> Další informace o koncových bodech, cesty a parametry požadavku najdete v tématu [Translator Text API 3.0: Zjištění](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect).

## <a name="create-a-struct-for-your-request-body"></a>Vytvoření struktury vaší tělo žádosti

V dalším kroku vytvoření anonymní struktury tělo žádosti a kódovat jako dokumenty JSON pomocí `json.Marshal()`. Přidejte tento kód `detect` funkce.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "Salve, Mondo!"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Žádost o sestavení

Teď, když jste kódování textu žádosti jako dokumenty JSON, můžete vytvořit váš požadavek POST a volání rozhraní Translator Text API.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Zpracování a tisku odpověď

Přidejte tento kód `detect` funkce, která se dekódovat odpověď JSON a poté ho naformátujte a vytiskne výsledek.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Spojení všech součástí dohromady

To je vše, sestavili jste jednoduchý program, který zavolá službu Translator Text API a vrátí odpověď JSON. Teď je čas program spustit:

```console
go run detect-language.go
```

Pokud chcete porovnat svůj kód s naším, kompletní ukázka je k dispozici na [GitHubu](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Ukázková odpověď

```json
[
  {
    "alternatives": [
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "pt",
        "score": 1
      },
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "en",
        "score": 1
      }
    ],
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "language": "it",
    "score": 1
  }
]
```

## <a name="next-steps"></a>Další postup

Prozkoumejte balíčky Go pro rozhraní API služeb Cognitive Services v tématu [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) na GitHubu.

> [!div class="nextstepaction"]
> [Prozkoumejte balíčky Go na GitHubu](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Další informace najdete v tématech

Další informace o použití rozhraní Translator Text API na:

* [Překlad textu](quickstart-go-translate.md)
* [Transliterace textu](quickstart-go-transliterate.md)
* [Získání alternativních překladů](quickstart-go-dictionary.md)
* [Získání seznamu podporovaných jazyků](quickstart-go-languages.md)
* [Určení délky věty ze vstupu](quickstart-go-sentences.md)
