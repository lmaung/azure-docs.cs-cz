---
title: Transformace a úlohy ve službě Azure Media Services | Dokumentace Microsoftu
description: Při použití služby Media Services, je potřeba vytvořit transformace pro popis pravidla nebo specifikace pro zpracování vašeho videa. Tento článek obsahuje přehled transformace je a jak ji používat.
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
ms.openlocfilehash: 1c2ec576211741390ef91233101261a7881e4180
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652208"
---
# <a name="transforms-and-jobs"></a>Transformace a úlohy

Toto téma obsahuje podrobnosti o [transformuje](https://docs.microsoft.com/rest/api/media/transforms) a [úlohy](https://docs.microsoft.com/rest/api/media/jobs) a popisuje vztah mezi těmito entitami. Následující diagram znázorňuje transformací/úlohy pracovního postupu.

![Transformace](./media/encoding/transforms-jobs.png)

> [!NOTE]
> Vlastnosti **transformace** a **úlohy** jsou DateTime typu jsou vždy ve formátu UTC.

## <a name="transforms"></a>Transformace

Použití **transformuje** konfigurace běžné úlohy kódování nebo analyzovat videa. Každý **transformace** popisuje nebudou tím správným nebo pracovního postupu úloh zpracování videa nebo zvukových souborů. Jeden transformace můžete použít více než jedno pravidlo. Transformace může například určit, že každé video zakódovat do souboru MP4 v daném s přenosovou rychlostí a že obrázek miniatury budou generovat z prvního rámce videa. Měli byste přidat jednu položku TransformOutput pro každé pravidlo, které chcete zahrnout do vaší transformace. Vytvoření transformací v účtu Media Services pomocí rozhraní API služby Media Services v3 nebo některou z publikované sady SDK. Media Services v3, které vychází rozhraní API pomocí Azure Resource Manageru, šablon Resource Manageru můžete také použít k vytvoření a nasazení se transformuje na svém účtu Media Services. Řízení přístupu na základě role je možné zamezit přístup k transformací.

Operace aktualizace na [transformace](https://docs.microsoft.com/rest/api/media/transforms) entity je určená pro provádění změn popis nebo priority základní TransformOutputs. Doporučujeme provádět tyto aktualizace po dokončení všech úloh v průběhu. Pokud chcete přepsat receptem, musíte vytvořit nová transformace.

## <a name="jobs"></a>Úlohy

A **úlohy** je skutečnou žádost do služby Azure Media Services použít **transformace** do daného vstupního videa nebo zvukový obsah. Po vytvoření transformace, můžete odeslat úlohy pomocí rozhraní API služby Media Services nebo libovolného z publikované sady SDK. **Úlohy** Určuje informace, jako jsou umístění vstupním videu a umístění pro výstup. Můžete zadat umístění váš vstup videa pomocí: Adresy URL HTTPS, adresy URL SAS, nebo [prostředky](https://docs.microsoft.com/rest/api/media/assets).  

Použití [úlohy vstup z HTTPS](job-input-from-http-how-to.md) Pokud váš obsah je dostupný prostřednictvím adresy URL a není nutné uložit zdrojový soubor v Azure (například importovat z S3). Tato metoda je také vhodná, pokud se požadovaný obsah nachází ve službě Azure Blob storage, ale nepotřebujete ho v prostředku. Tato metoda v současné době podporuje pouze jeden soubor pro vstup.
 
Použití [Asset jako vstup úlohy](job-input-from-local-file-how-to.md) Pokud vstupní obsah je již v prostředku nebo obsah uložený v místním souboru. Je také vhodné Pokud hodláte publikovat vstupní asset pro streamování nebo stažení (Dejme tomu, že chcete publikovat mp4 ke stažení, ale také chcete provést převod řeči na text nebo pro rozpoznávání tváře detekce). Tato metoda podporuje více souborové prostředky (třeba streamování sad, které byly místně zakódovány MBR).
 
Monitorování událostí pomocí služby Event Grid je možné získat průběh a stav úloh. Další informace najdete v tématu [sledovat události pomocí EventGrid](job-state-events-cli-how-to.md).

Operace aktualizace na [úlohy](https://docs.microsoft.com/rest/api/media/jobs) entity lze použít k úpravě *popis*a *priority* vlastnosti po odeslání úlohy. Ke změně *priority* vlastnost je platná pouze v případě, že úloha je stále ve stavu zařazení do fronty. Pokud úloha zahájil zpracování nebo dokončení, změna priority nemá žádný vliv.

## <a name="typical-workflow"></a>Typický pracovní postup

1. Vytvoření transformace 
2. Odesílání úloh v rámci této transformace 
3. Seznam transformací 
4. Transformace, odstraňte, pokud nemáte v úmyslu používat v budoucnosti. 

### <a name="example"></a>Příklad:

Předpokládejme, že chcete extrahovat první snímek všech videí jako obrázek miniatury – kroky, které byste prováděli jsou: 

1. Definovat předpisu nebo pravidla pro zpracování videí – "pomocí prvního rámce videa jako miniatury". 
2. Pro každé video budou předávat služby: 
    1. Kde najít tohoto videa  
    2. Kde má být zapsán výstup miniaturu. 

A **transformace** vám pomůže vytvořit jednou předpisu (krok 1) a odeslání úlohy pomocí tohoto předpisu (krok 2).

## <a name="job-error-codes"></a>Kódy chyb úlohy

Zobrazit [kódy chyb](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="paging"></a>Stránkování

Zobrazit [filtrování, řazení, stránkování, Media Services entit](entities-overview.md).

## <a name="configure-media-reserved-units"></a>Konfigurovat rezervované jednotky médií

Pro analýzu zvuku a videa analytických úloh, které jsou aktivovány Media Services v3 nebo Video Indexer důrazně doporučujeme pro účet zřídit s 10 rezervované jednotky médií S3 (použité položky). Pokud potřebujete více než 10 použité položky S3, otevřete lístek podpory pomocí [webu Azure portal](https://portal.azure.com/).

Podrobnosti najdete v tématu [škálování zpracování médií pomocí rozhraní příkazového řádku](media-reserved-units-cli-how-to.md).

## <a name="next-steps"></a>Další postup

- [Kurz: Nahrávání, kódování a streamování videí pomocí .NET](stream-files-tutorial-with-api.md)
- [Kurz: Analýza videa pomocí Media Services v3 pomocí .NET](analyze-videos-tutorial-with-api.md)
