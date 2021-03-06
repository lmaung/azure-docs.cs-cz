---
title: Ceny a omezení – Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Další informace o omezení a kvóty pro službu Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: anroth
ms.openlocfilehash: 8e96447f2a1b5b68ebf99ab4006fe43937f4c75f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884422"
---
# <a name="pricing-and-limits"></a>Ceny a omezení

Existují tři úrovně klíčů pro Custom Vision service. Prostředky projektu omezené zkušební verze jsou připojené k vaší vize vlastní přihlášení (to znamená, že účet služby Azure Active Directory nebo účet MSA). Jsou určeny pro krátké zkušební verze služby. Můžete se zaregistrovat pro F0 (zdarma) nebo S0 (standard) předplatné na webu Azure portal. Zobrazit odpovídající [stránce ceny služby Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) podrobnosti o cenách a transakce.

Účty vytvořené během dřívější bezplatná verze preview, před zavedením Azure verze Preview (1. března 2018), si zachovají své předchozí kvóty omezené zkušební verze. 

Počet trénovacích obrázků na projekt a značky pro každý projekt se očekává se zvýšení časem pro projekty S0.

||**Omezené zkušební verze**|**F0**|**S0**|
|-----|-----|-----|-----|
|Projekty|2|2|100|
|Trénovacích obrázků na projekt, klasifikace|5 000|5 000|50,000|
|Trénovacích obrázků na projekt, detekce objektů|5 000|5 000|10 000|
|Predikce / měsíc|10 000 |10 000|Unlimited|
|Značky / project|50|50|250|
|Iterace |10|10|10|
|Minimální označené obrázky značek klasifikace (50 + doporučeno) |5|5|5|
|Minimální označené obrázky na značku, detekce objektů (50 + doporučeno)|15|15|15|
|Jak dlouho se uchovávají předpovědi imagí|30 dní|30 dní|30 dní|
|[Predikce](https://go.microsoft.com/fwlink/?linkid=865445) operace s úložištěm (transakcí za sekundu)|2|2|10|
|[Predikce](https://go.microsoft.com/fwlink/?linkid=865445) operace bez úložiště (transakcí za sekundu)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (volání za sekundu rozhraní API)|2|2|10|
|[Další volání rozhraní API](https://go.microsoft.com/fwlink/?linkid=865446) (transakcí za sekundu)|10|10|10|
|Maximální velikost obrázku (školení pro nahrání image) |6 MB|6 MB|6 MB|
|Maximální velikost obrázku (předpověď)|4 MB|4 MB|4 MB|


