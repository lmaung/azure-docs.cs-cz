---
title: Zabránit obsahu překladu – Translator Text API
titlesuffix: Azure Cognitive Services
description: Zakázat posunutí obsahu s rozhraním Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: cc06e020001e0e0696fe1e89863f7df705d7fe98
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726972"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Jak zabránit překlad obsahu s rozhraním Translator Text API

Translator Text API vám umožní obsah značky tak, aby není přeložen. Můžete například označit kód, název značky nebo slovo nebo fráze, který nemá smysl při lokalizována.

## <a name="methods-for-preventing-translation"></a>Metody jak zabránit překladu
1. Řídicí klíčové slovo Twitteru @somethingtopassthrough nebo #somethingtopassthrough. Zrušit řídicí po převodu.

2. Obsah s `notranslate`.

   Příklad:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Použití [dynamický slovník](dynamic-dictionary.md) stanovit konkrétní překladu.

4. Nepředávejte řetězec rozhraní Translator Text API pro překlad.

5. Vlastní Translator: Použití [slovníku v vlastní Translator](custom-translator/what-is-dictionary.md) stanovit překladu věty se 100 % pravděpodobností.


## <a name="next-steps"></a>Další postup
> [!div class="nextstepaction"]
> [Vyhněte se překlad do volání rozhraní Translator API](reference/v3-0-translate.md)
