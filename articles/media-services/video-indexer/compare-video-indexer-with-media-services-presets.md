---
title: Porovnání Video Indexer a Azure Media Services v3 předvolby | Dokumentace Microsoftu
description: Toto téma srovnává funkce Video Indexer a Azure Media Services v3 předvolby.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: b4c74a67c66b12e3a7333f3e679e7c61a750cf7e
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "56002387"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Porovnání služeb Azure Media Services v3 přednastavení a Video Indexer 

Tento článek porovnává možnosti **rozhraní Video Indexer API** a **rozhraní API služby Media Services v3**. 

V současné době je k překrytí mezi součástmi, které nabízí [Video Indexer API v2](https://api-portal.videoindexer.ai/) a [rozhraní API služby Media Services v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). Následující tabulka nabízí aktuální obecných zásad pro pochopení rozdíly a podobnosti. 

## <a name="compare"></a>Porovnání

|Funkce|Video Indexer API |Analyzátor videa a zvuku analyzátor přednastavení<br/>v rozhraní API služby Media Services v3|
|---|---|---|
|Media Insights|[Rozšířené](video-indexer-output-json-v2.md) |[Základy](../latest/intelligence-concept.md)|
|Prostředí|Podívejte se na seznam podporovaných funkcí: <br/> [Přehled](video-indexer-overview.md)|Vrátí pouze poznatků z videí|
|Fakturace|[Ceny za Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Ceny za Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Dodržování předpisů|[Dodržování předpisů Azure](https://aka.ms/AzureCompliance)|Služba Media Services je kompatibilní s mnoha certifikace. Podívejte se na [Offerings.pdf dodržování předpisů Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) a vyhledejte položku "Media Services", chcete-li zobrazit, pokud je v souladu s certifikátem, které vás zajímají.|
|Bezplatná zkušební verze|USA – východ|Není k dispozici.|
|Dostupnost |Západní USA, východní Asie, Severní Evropa|Zobrazit [stav Azure](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Další postup

[Přehled Video Indexeru](video-indexer-overview.md)

[Přehled služby Media Services v3](../latest/media-services-overview.md)
