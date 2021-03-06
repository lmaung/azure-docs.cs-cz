---
title: 'Rychlý start: Získání odpovědí ze znalostní báze knowledge base - REST, Python – QnA Maker'
titlesuffix: Azure Cognitive Services
description: V tomto rychlém startu založené na protokolu REST Python provede získat odpověď ze znalostní báze, prostřednictvím kódu programu.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: 403886f543b21d2f9ed8ef457838fe2c93b7ca73
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875463"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-python"></a>Získejte odpovědi na dotaz zadaný ze znalostní báze pomocí Pythonu

Tento rychlý start vás provede prostřednictvím kódu programu získávání odpovědi z publikovaných znalostní báze QnA Maker. Služba QnA Maker automaticky extrahuje otázky a odpovědi z částečně strukturovaného obsahu, jako jsou třeba časté otázky, ze [zdrojů dat](../Concepts/data-sources-supported.md). V textu požadavku rozhraní API se odešle dotaz ve formátu JSON. 

## <a name="prerequisites"></a>Požadavky

* [Python 3.6 nebo novější](https://www.python.org/downloads/)
* [Visual Studio Code](https://code.visualstudio.com/)
* Potřebujete [službu QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Chcete-li načíst kód Product key, vyberte **klíče** pod **správy prostředků** v řídicím panelu Azure pro prostředek nástroje QnA Maker. 
* **Publikování** stránce nastavení. Pokud nemáte publikované znalostní báze, vytvořte prázdný znalostní báze a potom importujte znalostní báze na **nastavení** stránce a potom publikovat. Můžete stáhnout a použít [tento základní znalostní báze](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    Stránka nastavení publikování zahrnují hodnoty trasy příspěvek, hodnota hostitele a EndpointKey hodnoty. 

    ![Nastavení publikování](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Kód v tomto rychlém startu se [ https://github.com/Azure-Samples/cognitive-services-qnamaker-python ](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/tree/master/documentation-samples/quickstarts/get-answer) úložiště. 

## <a name="create-a-python-file"></a>Vytvořte soubor Pythonu

Otevřete VSCode a vytvořte nový soubor s názvem `get-answer-3x.py`.

## <a name="add-the-required-dependencies"></a>Přidání požadovaných závislostí

V horní části `get-answer-3x.py` potřebné závislosti přidejte do projektu:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=1-2 "Add the required dependencies")]

Hostitele a tras se můžou lišit od jak se zobrazují na **publikovat** stránky. Je to proto, že knihovna python neumožňuje, směrování na hostiteli. Směrování, která se zobrazí na **publikovat** stránce jako součást hostitele byl přesunut na trasu.

## <a name="add-the-required-constants"></a>Přidání požadovaných konstant

Přidejte požadované konstanty na používání nástroje QnA Maker. Tyto hodnoty jsou na **publikovat** stránce po publikování znalostní báze. 

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=5-25 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>Přidání požadavku POST k odesílání otázku a odpověď

Následující kód odešle požadavek HTTPS API nástroje QnA Maker odeslat dotaz do znalostní báze a přijímat odpovědi:

[!code-python[Add a POST request to send question to knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=27-48 "Add a POST request to send question to knowledge base")]

`Authorization` Hodnotu hlavičky obsahuje řetězec `EndpointKey `. 

## <a name="run-the-program"></a>Spuštění programu

Spusťte program z příkazového řádku. Automaticky se odešle požadavek rozhraní API nástroje QnA Maker a vytiskne se do okna konzoly.

Spusťte soubor:

```bash
python get-answer-3x.py
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Reference k rozhraní REST API služby QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
