---
title: Azure Event Grid doručování a opakované pokusy
description: Popisuje, jak Azure Event Grid zajišťují událostí a jak zpracovává nedoručených zpráv.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: spelluru
ms.openlocfilehash: 6dfa84eff8dcc104ae6f9c16262f3b1c697df6c1
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56991202"
---
# <a name="event-grid-message-delivery-and-retry"></a>Doručování zpráv Event Grid a zkuste to znovu

Tento článek popisuje, jak služby Azure Event Grid zpracovává události při doručení není potvrzeny.

Event gridu poskytuje odolné doručování. Poskytuje každou zprávu alespoň jednou pro každé předplatné. Události se posílají okamžitě registrované koncový bod každé předplatné. Pokud koncový bod nebude obdržel událost, služby Event Grid zopakuje pokus o doručení událostí.

V současné době služby Event Grid odesílá každé události jednotlivě odběratelům. Odběratel přijímá pole obsahující jednu událost.

## <a name="retry-schedule-and-duration"></a>Plán opakování a dobu trvání

Event Grid používá zásady opakování exponenciálního omezení rychlosti pro doručování událostí. Pokud koncový bod nereaguje nebo vrací kód chyby, služby Event Grid se opakuje doručování na jak kapacita systému dovolí podle následujícího plánu:

1. 10 sekund
1. 30 sekund
1. 1 minuta
1. 5 minut
1. 10 minut
1. 30 minut
1. 1 hodina
1. Každou hodinu po dobu až 24 hodin

Služby Event Grid přidá všechny kroky opakovat malé náhodné a může tj přeskočte určité opakovaných pokusů, pokud koncový bod je konzistentně není v pořádku, mimo provoz delší dobu, nebo se zdá být předešla zahlcení.

Deterministické chování, nastavte události time to live a maximální počet doručení pokusů [zásady opakování předplatné](manage-event-delivery.md).

Ve výchozím nastavení služby Event Grid vyprší platnost všech událostí, které nejsou doručeny do 24 hodin. Je možné [přizpůsobit zásady opakování](manage-event-delivery.md) při vytváření odběru událostí. Zadejte maximální počet pokusů o doručení (výchozí hodnota je 30) a události time to live (výchozí hodnota je 1440 minut).

## <a name="dead-letter-events"></a>Události onta nedoručených zpráv

Když Event Grid doručit událost, kterou může odesílat nedoručené událostí do účtu úložiště. Tento proces se označuje jako dead-lettering. Ve výchozím nastavení služby Event Grid nebude zapnout dead-lettering. Ho Pokud chcete povolit, musíte zadat účet úložiště pro uložení nedoručené události při vytváření odběru událostí. O přijetí změn události z tohoto účtu úložiště, chcete-li vyřešit doručení.

Když se všechny jeho opakované pokusy služby Event Grid odešle událost do umístění onta nedoručených zpráv. Pokud služby Event Grid přijme 400 (Chybný požadavek) nebo 413 (příliš velký požadavek Entity) kód odpovědi, okamžitě se událost odešle do koncového bodu onta nedoručených zpráv. Tyto kódy odpovědí znamenat, že nikdy úspěšné dodání události.

Existuje pět minut, než mezi poslední pokus o události a kdy se doručí do umístění onta nedoručených zpráv. Toto zpoždění má snížit počet operací úložiště objektů Blob. Pokud umístění onta nedoručených zpráv není k dispozici čtyři hodiny, události se zahodí.

Před nastavením umístění onta nedoručených zpráv, musíte mít účet úložiště s kontejnerem. Zadejte koncový bod pro tento kontejner, při vytváření odběru událostí. Koncový bod je ve formátu: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Můžete být upozorněni na události odeslala do umístění nedoručených zpráv. Použití služby Event Grid pro reakci na nedoručené události [vytvoření odběru událostí](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) pro úložiště objektů blob onta nedoručených zpráv. Pokaždé, když vaše úložiště objektů blob onta nedoručených zpráv přijme nedoručené událost, Event Grid oznámí obslužnou rutinu. Obslužné rutiny jsou reaguje s akcemi, které chcete provést pro vyřešení nedoručené události.

Příklad nastavení umístění nedoručených zpráv, najdete v části [Dead písmeno a zásady opakování](manage-event-delivery.md).

## <a name="message-delivery-status"></a>Stav doručování zpráv

Event Grid používá kódy odpovědí protokolu HTTP na vědomí přijetí události. 

### <a name="success-codes"></a>Úspěch kódy

Následující kódy odpovědi HTTP označuje, že událost má úspěšně doručit do vašeho webhooku. Event Grid považovat doručování za dokončený.

- 200 OK
- 202 přijato

### <a name="failure-codes"></a>Kód selhání

Následující kódy odpovědi HTTP označuje, že pokus o doručení událostí se nezdařilo.

- 400 – Chybný požadavek
- 401 Neautorizováno
- 404 – Nenalezeno
- 408 časový limit žádosti
- Entita 413 požadavku je moc velká
- Identifikátor URI 414 příliš dlouhý
- 429 příliš mnoho požadavků
- 500 – Interní chyba serveru
- 503 – Nedostupná služba
- 504 – Časový limit brány

## <a name="next-steps"></a>Další postup

* Chcete-li zobrazit stav doručení událostí, naleznete v tématu [doručování zpráv služby Event Grid monitorování](monitor-event-delivery.md).
* Přizpůsobit možnosti doručování událostí, přečtěte si článek [Dead písmeno a zásady opakování](manage-event-delivery.md).