---
title: Omezení a hranice – QnA Maker
titleSuffix: Azure Cognitive Services
description: Nástroj QnA Maker má meta omezení pro části znalostní báze knowledge base a služby. Je důležité udržovat znalostní báze v rámci tyto limity, aby bylo možné testovat a publikovat.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/26/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: fe15cafceea8128735f7241fa5e4187d4d9c47a9
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960050"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Omezení nástroje QnA Maker znalostní báze knowledge base a hranice
Úplný seznam omezení napříč QnA Maker.

## <a name="knowledge-bases"></a>Znalostních bází

* Maximální počet znalostních bází na základě [omezení vrstvy Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Vrstva Azure Search** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD, HIGH DENSITY**|
|---|---|---|---|---|---|----|
|Maximální počet publikovaných znalostních bází povoleno|2|14|49|199|199|2,999|

 Pokud svou úroveň má 15 povolené indexy, můžete publikovat 14 znalostních bází (1 index na publikované znalostní báze). Patnáctý index `testkb`, se používá pro všechny znalostních bází, vytváření a testování. 

## <a name="extraction-limits"></a>Extrakce omezení
* Maximální počet souborů, které může být extrahována a maximální velikost souboru: Zobrazit [ceny QnA maker](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)
* Maximální počet hloubkové odkazy, které je možné procházet pro extrakci maximálně ze stránek HTML – nejčastější dotazy: 20

## <a name="metadata-limits"></a>Omezení metadat
* Maximální počet polí metadat za znalostní báze podle [omezení vrstvy Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Vrstva Azure Search** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD, HIGH DENSITY**|
|---|---|---|---|---|---|----|
|Pole maximální metadat na službu QnA Maker (v rámci všech znalostní báze)|1 000|100 *|1 000|1 000|1 000|1 000|

## <a name="knowledge-base-content-limits"></a>Omezení obsahu znalostní báze
Celkové limitů na obsah znalostní báze knowledge base:
* Délka textu odpovědi: 25,000
* Délka text otázky: 1 000
* Délka textu klíč/hodnota metadat: 100
* Podporované znaky pro název metadat: Písmena abecedy, číslice a _  
* Podporované znaky pro hodnota metadat: Všechny s výjimkou: a | 
* Délka názvu souboru: 200
* Podporované formáty souborů: "TSV", ".pdf", ".txt", ".docx", ".xlsx".
* Maximální počet alternativní otázek: 100
* Maximální počet dvojic otázka – odpověď: Závisí [vrstva Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits) zvolili. Pár otázek a odpovědí se mapuje na dokument v indexu Azure Search. 

## <a name="create-knowledge-base-call-limits"></a>Vytvoření znalostní báze volání omezení:
Tyto představují vytvořit omezení pro každou akci znalostní báze; To znamená, že kliknete na *vytvořit KB* nebo volání rozhraní API CreateKnowledgeBase.
* Maximální počet alternativní otázky na odpovědi: 100
* Maximální počet adres URL: 10
* Maximální počet souborů: 10

## <a name="update-knowledge-base-call-limits"></a>Aktualizovat omezení volání znalostní báze
Představují limity pro každou akci aktualizace; To znamená, že kliknete na *uložit a jejich trénování* nebo volání rozhraní API UpdateKnowledgeBase.
* Délka názvu každého zdroje: 300
* Maximální počet otázek alternativní přidány nebo odstraněny: 100
* Maximální počet polí metadat přidány nebo odstraněny: 10
* Maximální počet adres URL, které lze aktualizovat: 5

## <a name="next-steps"></a>Další postup

Zjistěte, kdy a jak změnit úrovně služeb:

* [Nástroj QnA Maker](how-to/upgrade-qnamaker-service.md#upgrade-qna-maker-management-sku): Při musíte mít další zdrojové soubory nebo větší dokumenty ve znalostní bázi, nad aktuální úrovní, upgradu služby QnA Maker cenovou úroveň.
* [App Service](how-to/upgrade-qnamaker-service.md#upgrade-app-service): Když znalostní báze potřebuje více požadavků z aplikace pro klienta, upgrade vaše cenová úroveň služby app service.
* [Azure Search](how-to/upgrade-qnamaker-service.md#upgrade-azure-search-service): Když budete chtít mít mnoho znalostních bází, upgradujte cenovou úroveň služby Azure Search.
