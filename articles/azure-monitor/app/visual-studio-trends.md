---
title: Analýza trendů v sadě Visual Studio | Dokumentace Microsoftu
description: Analyzujte, vizualizujte a zkoumejte trendy v telemetrii služby Application Insights v sadě Visual Studio.
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: carmonm
ms.assetid: 3150c6fc-2691-44f6-a290-fc5cd68e692a
ms.service: application-insights
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/17/2017
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 898f0974a6a29abde5c84d611adc8d50c3873141
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121067"
---
# <a name="analyzing-trends-in-visual-studio"></a>Analýza trendů v sadě Visual Studio
Nástroj Trendy Application Insights vizualizuje průběžné změny důležitých telemetrických událostí ve vaší webové aplikaci. Díky tomu můžete rychle identifikovat problémy a anomálie. Nástroj Trendy vám dodá podrobnější diagnostické informace, abyste mohli zlepšit výkon aplikace, sledovat příčiny výjimek a získat přehledy z vlastních událostí.

![Příklad okna nástroje Trendy](./media/visual-studio-trends/app-insights-trends-hero-750.png)

## <a name="configure-your-web-app-for-application-insights"></a>Konfigurace webové aplikace pro Application Insights

Pokud jste to ještě neudělali, [nakonfigurujte svou webovou aplikaci pro Application Insights](../../azure-monitor/app/app-insights-overview.md). Umožní vám to odesílat telemetrii na portál Application Insights. Z něj čte telemetrii nástroj Trendy.

Nástroj Trendy Application Insights je dostupný v sadě Visual Studio 2015 s aktualizací Update 3 a vyšší.

## <a name="open-application-insights-trends"></a>Otevření nástroje Trendy Application Insights
Pokud chcete otevřít okno nástroje Trendy Application Insights, postupujte následovně:

* na panelu nástrojů Application Insights zvolte **Prozkoumat trendy telemetrie**, nebo
* v místní nabídce projektu zvolte **Application Insights > Prozkoumat trendy telemetrie**, nebo
* v řádku nabídek sady Visual Studio zvolte **Zobrazení > Ostatní okna > Trendy Application Insights**.

Může se zobrazit výzva k výběru prostředku. Klikněte na **Vybrat prostředek**, přihlaste se pomocí předplatného Azure a potom v seznamu vyberte prostředek Application Insights, u kterého chcete analyzovat trendy telemetrie.

## <a name="choose-a-trend-analysis"></a>Výběr analýzy trendů
![Nabídka běžných typů analýz trendů](./media/visual-studio-trends/app-insights-trends-1-750.png)

Začněte výběrem jedné z pěti běžných analýz trendů, kde každá analyzuje data z posledních 24 hodin:

* **Prozkoumat problémy s výkonem požadavků serveru** – požadavky na vaši službu seskupené podle doby odezvy
* **Analyzovat chyby v požadavcích serveru** – požadavky na vaši službu seskupené podle kódu odpovědi HTTP
* **Prozkoumat výjimky v aplikaci** – výjimky služby seskupené podle typu výjimky
* **Zkontrolovat výkon závislostí aplikace** – služby volané vaší službou seskupené podle doby odezvy
* **Zkontrolovat vlastní události** – vlastní události, které jste pro svoji službu nastavili, seskupené podle typu události.

Tyto předdefinované analýzy jsou dostupné i později prostřednictvím tlačítka **Zobrazit běžné typy analýzy telemetrie**, které se nachází v levém horním rohu okna Trendy.

## <a name="visualize-trends-in-your-application"></a>Vizualizace trendů v aplikaci
Nástroj Trendy Application Insights pracuje s telemetrií vaší aplikace a vizualizuje z ní časové řady. Každá vizualizace časové řady zobrazuje jeden typ telemetrie (seskupený podle jedné vlastnosti takové telemetrie) za určité časové období. Můžete například zobrazit požadavky serveru za posledních 24 hodin seskupené podle země původu. V tomto příkladu každá bublina na vizualizaci představuje počet požadavků serveru pro určitou zemi/oblast během jedné hodiny.

Pomocí ovládacích prvků v horní části okna nastavte typy telemetrie, které chcete zobrazit. Nejdřív vyberte typy telemetrie, které vás zajímají:

* **Typ telemetrie** – požadavky serveru, výjimky, závislosti nebo vlastní události
* **Časový rozsah** – jakýkoli od posledních 30 minut po poslední 3 dny.
* **Seskupit podle** – typ výjimky, ID problému, země/oblast a další.

Potom klikněte na **Analyzovat telemetrii** a spusťte dotaz.

Pokud chcete přecházet mezi bublinami ve vizualizaci, postupujte následovně:

* Kliknutím vyberte bublinu, která aktualizuje filtry v dolní části okna a shrnuje jenom události, které nastaly během konkrétního časového období.
* Poklikáním na bublinu přejděte do nástroje hledání a zobrazte všechny jednotlivé telemetrické události, ke kterým došlo během tohoto časového období
* Pokud chcete zrušit výběr bubliny ve vizualizaci, podržte klávesu Ctrl a klikněte na bublinu.

> [!TIP]
> Nástroj Trendy a nástroj hledání vám společně pomáhají mezi tisíci telemetrických událostí identifikovat příčiny problémů vaší služby. Pokud si vaši zákazníci například jedno odpoledne všimnou, že je odezva aplikace pomalejší, spusťte nástroj Trendy. Analyzujte požadavky na vaši službu za posledních několik hodin, seskupené podle doby odezvy. Zjistěte, jestli se neobjevil neobvykle velký cluster pomalých požadavků. Poklikáním na tuto bublinu přejdete do nástroje hledání, který se filtruje podle těchto událostí žádostí. Pomocí hledání můžete prozkoumat obsah těchto požadavků a přejít k problematickému kódu, abyste mohli problém vyřešit.
> 
> 

## <a name="filter"></a>Filtr
Pomocí ovládacích prvků filtru v dolní části okna můžete zjistit konkrétnější trendy. Pokud chcete filtr použít, klikněte na jeho název. Mezi různými filtry můžete rychle přepínat a zjišťovat tak trendy, které se můžou skrývat v konkrétní dimenzi telemetrie. Když filtr použijete v jedné dimenzi, například v typu výjimky, můžete na filtry v ostatních dimenzích stále klikat, i když jsou zobrazené šedě. Pokud chcete zrušit používání filtru, klikněte na něj znovu. Podržením klávesy Ctrl a následným klikáním můžete vybrat několik filtrů ve stejné dimenzi.

![Filtry trendů](./media/visual-studio-trends/TrendsFiltering-750.png)

Jak postupovat, když chcete použít několik filtrů. 

1. Použijte první filtr. 
2. Klikněte na tlačítko **Použít vybrané filtry a znova poslat dotaz** vedle názvu dimenze prvního filtru. Tím bude znovu odeslán dotaz na telemetrii a bude se týkat jenom událostí, které odpovídají prvnímu filtru. 
3. Použijte druhý filtr. 
4. Proces opakujte tak dlouho, až trendy v konkrétní podmnožině telemetrie najdete. Například požadavky serveru s názvem „GET Home/Index“ *a* ty, které přišly z Německa *a* získaly stavový kód 500. 

Pokud chcete zrušit používání jednoho z těchto filtrů, klikněte u příslušné dimenze na tlačítko **Odstranit vybrané filtry a poslat dotaz znova**.

![Několik filtrů](./media/visual-studio-trends/TrendsFiltering2-750.png)

## <a name="find-anomalies"></a>Nalezení anomálií
Nástroj Trendy může zvýraznit bubliny událostí, které jsou ve srovnání s jinými bublinami ve stejné časové řadě neobvyklé. V rozevírací nabídce Typ zobrazení vyberte **Počty v časovém intervalu (zvýraznit anomálie)** nebo **Procenta v časovém intervalu (zvýraznit anomálie)**. Červené bubliny označují anomálie. Anomálie se definují jako bubliny s počty/procenta, vyšší než 2,1 násobek směrodatné odchylky poměru počty/procenta, ke kterým došlo v minulosti dvou časových obdobích (48 hodin, pokud zobrazujete posledních 24 hodin atd.).

![Barevné tečky označují anomálie.](./media/visual-studio-trends/TrendsAnomalies-750.png)

> [!TIP]
> Zvýraznění anomálií může být zvláště užitečné při hledání mimořádných hodnot v časových řadách malých bublin, u kterých by se jinak mohlo zdát, že mají podobnou velikost.  
> 
> 

## <a name="next"></a>Další kroky
|  |  |
| --- | --- |
| **[Práce s Application Insights v sadě Visual Studio](../../azure-monitor/app/visual-studio.md)**<br/>Hledejte telemetrii, zobrazujte data v CodeLens a konfigurujte Application Insights. Vše v sadě Visual Studio. |![Klikněte pravým tlačítkem myši na projekt a vyberte Application Insights, Vyhledávání](./media/visual-studio-trends/34.png) |
| **[Přidání dalších dat](../../azure-monitor/app/asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. |![Visual Studio](./media/visual-studio-trends/64.png) |
| **[Práce s portálem Application Insights](../../azure-monitor/app/app-insights-dashboards.md)**<br/>Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie. |![Visual Studio](./media/visual-studio-trends/62.png) |

