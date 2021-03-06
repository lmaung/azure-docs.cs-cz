---
title: Upozornění protokolů ve službě Azure Monitor
description: Aktivační událost e-mailů, oznámení, volání adresy URL webů (webhooky), nebo automatizace při splnění zadané podmínky analytického dotazu pro Azure Alerts.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 2/20/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 194fba3296359f5f7d29a37425a938fe08f1332b
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/20/2019
ms.locfileid: "56452878"
---
# <a name="log-alerts-in-azure-monitor"></a>Upozornění protokolů ve službě Azure Monitor

Tento článek obsahuje podrobnosti o upozornění protokolů jsou jedním z typů výstrah, které jsou podporovány v rámci [Azure Alerts](../../azure-monitor/platform/alerts-overview.md) a umožnit uživatelům použít analytické platformy Azure jako základ pro generování výstrah.

Upozornění protokolu se skládá z prohledávání protokolů pravidel vytvořených pro [protokoly Azure monitoru](../../azure-monitor/learn/tutorial-viewdata.md) nebo [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events). Další informace o jejich využití najdete v tématu [vytvoření upozornění protokolu v Azure](../../azure-monitor/platform/alerts-log.md)

> [!NOTE]
> Data z oblíbených protokolů [protokoly Azure monitoru](../../azure-monitor/learn/tutorial-viewdata.md) je teď taky dostupná na platformě metriky ve službě Azure Monitor. Pro zobrazení podrobností o [upozornění metriky pro protokoly](../../azure-monitor/platform/alerts-metric-logs.md)


## <a name="log-search-alert-rule---definition-and-types"></a>Hledání pravidel upozornění protokolů – definice a typy

Pravidla prohledávání protokolu vytváří služba Azure Alerts pro automatické spouštění zadaných dotazů na protokoly v pravidelných intervalech.  Pokud výsledky dotazu na protokol splňují konkrétní kritéria, vytvoří se záznam upozornění. Pravidlo potom může automaticky spustit jednu nebo více akcí pomocí [skupin akcí](../../azure-monitor/platform/action-groups.md). [Azure Monitoring Přispěvatel](../../azure-monitor/platform/roles-permissions-security.md) role pro vytváření, úpravu a aktualizaci upozornění protokolů se může vyžadovat; spolu s přístup & dotazu provádění práva pro cíle analytics v pravidlo upozornění nebo dotaz na upozornění. Pokud uživatel vytváření nemá přístup k všechny cíle analytics v pravidlo upozornění nebo dotaz na upozornění – vytvoření pravidla může selhat nebo pravidel upozornění protokolů se spustí pomocí částečných výsledků.

Pravidla vyhledávání protokolů jsou definovány následující podrobnosti:

- **Protokolování dotazu**.  Dotaz, který se spustí pokaždé, když se pravidlo upozornění aktivuje.  Vrácené tímto dotazem záznamy slouží k určení, jestli je výstraha aktivovat. Analytický dotaz je možné pro konkrétní pracovní prostor Log Analytics nebo aplikaci Application Insights a dokonce i pracovat nad více [více prostředky Log Analytics a Application Insights](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) zadaný uživatel má přístup stejně jako práva na všechny dotazy prostředky. 
    > [!IMPORTANT]
    > Upozornění protokolu **nejsou** podporují použití [funkce](../log-query/functions.md) z bezpečnostních důvodů. Také [napříč prostředky dotazu](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) podpora v upozornění protokolu pro Application Insights a upozornění protokolů pro [Log Analytics nakonfigurované pomocí rozhraní API scheduledQueryRules](../../azure-monitor/platform/alerts-log-api-switch.md) pouze.

    Některé analytické příkazy a kombinace nejsou kompatibilní s využitím v upozornění protokolu; pro další podrobnosti zobrazíte [protokolu dotazy výstrah ve službě Azure Monitor](../../azure-monitor/platform/alerts-log-query.md).

- **Časové období**.  Určuje časový rozsah dotazu. Dotaz vrátí pouze záznamy vytvořené v tomto rozsahu před aktuálním časem. Časové období omezuje data načtena dotaz protokolu, aby se zabránilo zneužití a připojení, vyřešíte nepřetržitou jakýkoli příkaz času (například před) používaných v dotazu protokolu. <br>*Například toto časové období je nastavený na 60 minut a spuštění dotazu v 13:15, se vrátí pouze záznamy vytvořené mezi 12:15 PM a 1:15 PM do spuštění protokolu dotazu. Když teď dotaz protokolu používá čas příkazu, jako jsou před (7d), protokolu by spustí dotaz pouze pro data mezi 12:15 PM a 1:15 PM - jakoby dat existuje pouze posledních 60 minut. A ne po dobu sedmi dní dat, jak je uvedeno v protokolu dotazu.*

- **Frekvence**.  Určuje, jak často se má spustit dotaz. Může být libovolná hodnota mezi 5 minutami a 24 hodinami. By měla být větší nebo menší než toto časové období.  Pokud je hodnota větší než časové období, riskujete záznamy se vynechalo.<br>*Představte si třeba po dobu 30 minut a frekvenci 60 minut.  Pokud je dotaz spuštěn v 1:00, vrátí záznamy 12:30 až 1:00 PM.  Při příštím spuštění dotazu by je 2:00, kdy vracel záznamy 1:30 až 2:00.  Záznamy, které vytvoří až 1:00, 1:30 by nikdy být vyhodnocena.*

- **Prahová hodnota**.  K určení, zda má být vytvořena výstraha se vyhodnocují výsledky prohledávání protokolů.  Prahová hodnota se liší pro různé typy pravidel upozornění na prohledávání protokolu.

Pravidla vyhledávání protokolů už to pro [protokoly Azure monitoru](../../azure-monitor/learn/tutorial-viewdata.md) nebo [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events), mohou být dvou typů. Každý z těchto typů je podrobně popsány v následující části.

- **[Počet výsledků](#number-of-results-alert-rules)**. Jediná výstraha vytvořena, když počet záznamů vrácených prohledávání protokolů překročí zadaný počet.
- **[Metriky měření](#metric-measurement-alert-rules)**.  Upozornění bylo vytvořeno pro každý objekt ve výsledcích prohledávání protokolů s hodnotami, které překročí zadanou prahovou hodnotu.

Rozdíly mezi typy pravidla upozornění jsou následující.

- *Počet výsledků* pravidla upozornění vždy vytvoří jednu výstrahu, nějakou *měření metriky* pravidlo výstrahy vytvoří upozornění pro každý objekt, který překračuje prahovou hodnotu.
- *Počet výsledků* pravidla upozornění vytvořit upozornění, když je prahová hodnota překročena jednorázově. *Metriky měření* pravidla upozornění může vytvořit upozornění, pokud je překročena prahová hodnota počtu dobu v konkrétním časovém intervalu.

### <a name="number-of-results-alert-rules"></a>Počet pravidel upozornění výsledky

**Počet výsledků** pravidla upozornění vytvoření jedinou výstrahu, když počet záznamů vrácených dotazem vyhledávání překročí zadanou prahovou hodnotu. Tento typ upozornění pravidla je ideální pro práci s událostmi, jako jsou protokoly událostí Windows, Syslog, odpovědi webové aplikace a vlastní protokoly.  Chcete vytvořit upozornění, když se vytvoří událost konkrétní chyby nebo při vytváření více chybové události v konkrétním časovém období.

**Prahová hodnota**: Prahová hodnota pro počet pravidel upozornění výsledků je větší nebo menší než jednu konkrétní hodnotu.  Pokud počet záznamů vrácených prohledávání protokolů, které splňují tato kritéria, vytvoří se výstraha.

Výstrahy v rámci jediné události, nastavte počet výsledků na hodnotu větší než 0 a zkontrolujte výskyt jedné události, který byl vytvořen od posledního spuštění dotazu. Některé aplikace může zaznamenat občasné chybu, která by neměla být nutně vygenerovat výstrahu.  Aplikace může například opakujte proces, který vytvoří událost chyby a potom příště se úspěšně provedla.  V takovém případě nemusí chtít vytvořit výstrahu, pokud více událostí jsou vytvořené v konkrétním časovém období.  

V některých případech můžete chtít vytvořit výstrahu chybí událost.  Například může proces protokolu běžné události k označení, že funguje správně.  Pokud není některá z těchto událostí protokolu v konkrétním časovém období, je třeba vytvořit výstrahu.  V takovém případě můžete nastavit mez na **menší než 1**.

#### <a name="example-of-number-of-records-type-log-alert"></a>Příklad upozornění protokolu typu počet záznamů

Představte si třeba situaci, ve kterém chcete vědět, když vaše webové aplikace poskytuje odpověď pro uživatele, kteří kód 500 (to znamená) vnitřní chyba serveru. Vytvoříte pravidlo upozornění s následujícími podrobnostmi:  

- **Dotaz:** požadavky | kde kód výsledku == "500"<br>
- **Časové období:** 30 minut<br>
- **Četnosti upozornění:** pěti minut<br>
- **Prahová hodnota:** Větší než 0.<br>

Výstraha by spusťte dotaz každých 5 minut, 30 minut dat – vás pod rouškou u jakéhokoli záznamu, kde byl výsledný kód 500. Pokud ještě jeden takový záznam najde, aktivuje se upozornění a aktivuje akci nakonfigurovanou.

### <a name="metric-measurement-alert-rules"></a>Pravidla upozornění metriky měření

**Metriky měření** pravidla upozornění vytvořit upozornění pro každý objekt v dotazu s hodnotou, která překročí zadanou prahovou hodnotu.  Mají odlišné následující rozdíly proti **počet výsledků** pravidla upozornění.

- **Agregační funkce**: Určuje, která se provede výpočet a potenciálně číselná pole agregovat.  Například **count()** vrátí počet záznamů v dotazu **avg(CounterValue)** vrací průměr pole CounterValue v intervalu. V dotazu je agregační funkce musí být s názvem/volá: AggregatedValue a poskytnout číselnou hodnotu. 

- **Pole Seskupit**: Pro každou instanci tohoto pole se vytvoří záznam s agregovanou hodnotu a pro každý může být vygenerována výstraha.  Například pokud chcete generovat výstrahy pro každý počítač, můžete využít **počítačem**. V případě více skupin polí zadaná v oznámení dotazu se uživatel může určit, jaké pole se má použít k řazení výsledků pomocí **agregační na** parametr (metricColumn)

    > [!NOTE]
    > *Agregace na* (metricColumn) možnost je dostupná pro upozornění protokolu typ měření metriky pro Application Insights a upozornění protokolů pro [Log Analytics nakonfigurované pomocí rozhraní API scheduledQueryRules](../../azure-monitor/platform/alerts-log-api-switch.md) pouze.

- **Interval**:  Definuje časový interval, po kterou se data agregují.  Například, pokud jste zadali **pět minut**, by se vytvořily záznam pro každou instanci tohoto pole Skupina v 5minutových intervalech agregován v časovém období zadaný pro oznámení.

    > [!NOTE]
    > Funkce Bin musí použít v dotazu k určení intervalu. Jak bin() může vést k nerovnost časové intervaly – výstraha se automaticky převede bin příkaz bin_at příkazu s odpovídajícím časem za běhu, zajistit výsledky s pevnému bodu. Typ metriky měření upozornění protokolu je navržen pro práci s dotazy s až tři výskyty bin() příkaz
    
- **Prahová hodnota**: Prahová hodnota pro pravidla upozornění metriky měření je definován tak, že agregované hodnoty a celou řadu.  Pokud libovolný datový bod v prohledávání protokolu překročí tuto hodnotu, se považuje za porušení zabezpečení.  Pokud počet narušení v pro libovolný objekt ve výsledcích překročí zadanou hodnotu, je vytvořit výstrahu pro daný objekt.

Chybná konfigurace *agregační na* nebo *metricColumn* volba může způsobit pravidla upozornění na misfire. Další informace najdete v tématu [řešení potíží při měření metriky pravidlo výstrahy je nesprávný](alert-log-troubleshoot.md#metric-measurement-alert-rule-is-incorrect).

#### <a name="example-of-metric-measurement-type-log-alert"></a>Příklad upozornění protokolu typ měření metriky

Představte si třeba situaci, kde chcete výstrahu Pokud jakýkoli počítač překročí 90 % třikrát více než 30 minut využití procesoru.  Vytvoříte pravidlo upozornění s následujícími podrobnostmi:  

- **Dotaz:** Perf | kde ObjectName == "Procesor pro" a hodnota CounterName == "čas procesoru v %" | summarize AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 5m), počítač<br>
- **Časové období:** 30 minut<br>
- **Četnosti upozornění:** pěti minut<br>
- **Alert Logic - podmínku a prahovou hodnotu:** Větší než 90<br>
- **Pole skupiny (agregaci na):** Počítač
- **Aktivovat upozornění na základě:** Celkový počet poruší větší než 2<br>

Dotaz by vytvořit průměrnou hodnotu pro každý počítač v intervalech 5 minut.  Tento dotaz se spustí každých 5 minut, než se data shromážděná za předchozí 30 minut. Protože je pole Skupina (agregaci v), zvolené úložiště se sloupcovou strukturou "Computer" - pro různé hodnoty "Computer" je rozdělit AggregatedValue a průměrné využití procesoru pro každý počítač, je určen pro bin dobu 5 minut.  (Řekněme) tři počítače, bude výsledkem vzorku dotazu pro níže.


|TimeGenerated [UTC] |Počítač  |AggregatedValue  |
|---------|---------|---------|
|20xx-xx-xxT01:00:00Z     |   srv01.contoso.com      |    72     |
|20xx-xx-xxT01:00:00Z     |   srv02.contoso.com      |    91     |
|20xx-xx-xxT01:00:00Z     |   srv03.contoso.com      |    83     |
|...     |   ...      |    ...     |
|20xx-xx-xxT01:30:00Z     |   srv01.contoso.com      |    88     |
|20xx-xx-xxT01:30:00Z     |   srv02.contoso.com      |    84     |
|20xx-xx-xxT01:30:00Z     |   srv03.contoso.com      |    92     |

Pokud výsledek dotazu bude zobrazena, se zobrazí jako.

![Ukázkové výsledky dotazu](media/alerts-unified-log/metrics-measurement-sample-graph.png)

V tomto příkladu vidíme v přihrádkách 5 minut pro všechny tři počítače - průměrné využití procesoru jako vypočítanou pro 5 minut. Prahová hodnota 90 porušovány podle srv01 pouze jednou v 1:25 bin. Ve srovnání s srv02 překročí 90 prahovou hodnotu na 1:10, 1:15 a 1:25 přihrádky; zatímco srv03 překročí 90 prahovou hodnotu na 1:10, 1:15, 1:20 a 1:30.
Protože je nakonfigurované upozornění tak aktivační událost vycházející celkový počet porušení jsou větší než dvě, vidíme, že srv02 a srv03 pouze splňují kritéria. Samostatné výstrahy proto by se vytvořily pro srv02 a srv03 vzhledem k tomu, u nichž nebyla dodržena 90 % prahové hodnoty dvakrát napříč více času přihrádky.  Pokud *aktivovat upozornění na základě:* parametr místo toho se nakonfigurovalo pro *průběžné porušení* možnost, pak by se aktivuje výstrahu **pouze** pro srv03, protože to u nichž nebyla dodržena Prahová hodnota pro tři po sobě jdoucích přihrádkách z 1:10 1:20. A **není** pro srv02, protože u nichž nebyla dodržena prahová hodnota pro dva přihrádky po sobě jdoucích z 1:10 do 1:15.

## <a name="log-search-alert-rule---firing-and-state"></a>Hledání pravidel upozornění protokolů – stav a jeho spuštění

Pravidel upozornění protokolů vyhledávání funguje na logiku zabezpečuje pomocí vhodné uživatelem závislosti na konfiguraci a vlastní analýzy dotazech. Od logiky přesné podmínky nebo důvod, proč byste pravidlo upozornění, je trigger zapouzdřena v dotazu Analytics – které se můžou lišit v jednotlivých pravidel upozornění protokolů. Upozornění v Azure, má omezené informace určité základní příčiny v protokolu výsledků je dosažená nebo Překročená prahová hodnota podmínky pravidel upozornění protokolů hledání. Proto upozornění protokolů se označují položky podle stavu bez a se aktivuje pokaždé, když se překročí prahová hodnota uvedená v upozornění protokolu stačí výsledek hledání v protokolu *počet výsledků* nebo *měření metriky* typu podmínky. MSMQEvent_Arrived; pravidel upozornění protokolů se průběžně zachovat, tak dlouho, dokud se vyskytl výstražný stav splníte tak, že výsledek vlastního analytického dotazu k dispozici; bez upozornění každou získávání přeložit. Jako logic přesnou příčinu selhání monitorování se maskované uvnitř dotazu analytics zadané uživatelem. neexistuje žádné prostředky Azure oznámení k potvrzením odvození, zda výsledek hledání v protokolu nesplnění podmínek smluv prahová hodnota označuje řešení problému.

Nyní předpokládejme máme pravidel upozornění protokolů volá *Contoso upozornění protokolu*, protože podle konfigurace v [ukázkové chatovací aplikaci obsažené upozornění protokolu typu Number výsledků](#example-of-number-of-records-type-log-alert). 
- Výsledek hledání v protokolu: 05: 00 provedení Contoso upozornění protokolu pomocí výstrah služby Azure, vrátil 0 záznamů. pod prahovou hodnotu a proto se nespustí, výstrahu. 
- Výsledek hledání v protokolu na další iteraci: 10: 00 provedení Contoso upozornění protokolu pomocí výstrah služby Azure k dispozici 5 záznamů. překračující prahovou hodnotu a aktivaci výstrahy, krátce po aktivací [skupiny akcí](../../azure-monitor/platform/action-groups.md) spojené. 
- Výsledek hledání v protokolu v 1:15 PM provedení Contoso upozornění protokolu pomocí výstrah služby Azure, poskytuje 2 záznamů. překračující prahovou hodnotu a aktivaci výstrahy, krátce po aktivací [skupiny akcí](../../azure-monitor/platform/action-groups.md) spojené.
- Nyní na další iteraci: 20: 00 provedení upozornění protokolu Contoso upozorněním na Azure, poskytuje výsledek hledání v protokolu znovu 0 záznamů. pod prahovou hodnotu a proto se nespustí, výstrahu.

Ale v případě výše uvedené v 1:15 PM - upozornění Azure nemůže určit, že základní problémy zaznamenáno v 1:10 přetrvávají a pokud je net nové neúspěšné; uživatelem zadaný dotaz může být zohlední starší záznamy - jistotu upozornění v Azure. Proto k chyba na straně upozornění, když Contoso upozornění protokolu spuštění 1:15 PM, nakonfigurované [skupiny akcí](../../azure-monitor/platform/action-groups.md) se znovu aktivuje. Nyní: 20: 00 Když jsou vidět žádné záznamy – upozornění Azure nemůže být jisti, že příčinou záznamy vyřešen; proto bude Contoso upozornění protokolu nebyl změněn na vyřešený v řídicím panelu Azure výstrahy a/nebo oznámení odeslaných s informacemi o tom vyřešení upozornění.


## <a name="pricing-and-billing-of-log-alerts"></a>Ceny a fakturace protokol výstrah

Ceny za upozornění protokolů použít je uvedeno v [ceny služby Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/) stránky. V Azure účtuje poplatky za upozornění protokolů jsou reprezentovány jako typ `microsoft.insights/scheduledqueryrules` pomocí:

- Upozornění protokolů Application insights, zobrazí se přesný název výstrahy spolu s skupinu prostředků a vlastnosti výstrahy
- Upozornění protokolů v Log Analytics, zobrazí se přesný název výstrahy spolu s skupinu prostředků a vlastnosti výstrahy; Při použití [scheduledQueryRules rozhraní API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

[Starší verze rozhraní API pro analýzu protokolů](../../azure-monitor/platform/api-alerts.md) má akce výstrah a plány jako součást Log Analytics uložené hledání a není správné [prostředků Azure](../../azure-resource-manager/resource-group-overview.md). Proto k povolení fakturace pro tyto starší verze protokolu výstrahy vytvořené pro Log Analytics pomocí webu Azure Portal **bez** [přepnutí na nové rozhraní API](../../azure-monitor/platform/alerts-log-api-switch.md) nebo prostřednictvím [starší verze rozhraní API pro analýzu protokolů](../../azure-monitor/platform/api-alerts.md) - pravidla upozornění skryté pseudo jsou vytvořeny na `microsoft.insights/scheduledqueryrules` fakturace v Azure. Pravidla upozornění skryté pseudo vytvořená pro fakturaci na `microsoft.insights/scheduledqueryrules` uvedeno jako `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` společně se skupinu prostředků a vlastnosti výstrahy.

> [!NOTE]
> Pokud platné znaky, jako `<, >, %, &, \, ?, /` jsou k dispozici, nahradí se `_` v pravidle výstrahy skryté pseudo název a proto také ve službě Azure fakturovat.

Odebrat prostředky skryté scheduleQueryRules vytvořené pro účely fakturace pomocí pravidel upozornění [starší verze rozhraní API pro analýzu protokolů](api-alerts.md), uživatel provádět následující:

- Buď uživatel může [přepnout předvoleb rozhraní API pro pravidla upozornění v pracovním prostoru Log Analytics](../../azure-monitor/platform/alerts-log-api-switch.md) a bez ztráty jejich upozornění pravidel nebo monitorování Přesun do Azure Resource Manageru kompatibilní [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). A tím eliminuje nutnost pseudo skrytá pravidla výstrah pro účely fakturace.
- Nebo pokud uživatel nechce přepnout předvoleb rozhraní API, uživatel bude muset **odstranit** původní plán a akce upozornění pomocí [starší verze rozhraní API pro analýzu protokolů](api-alerts.md) nebo odstranit v [webu Azure portal původní pravidel upozornění protokolů](../../azure-monitor/platform/alerts-log.md#view--manage-log-alerts-in-azure-portal)

## <a name="next-steps"></a>Další postup

* Další informace o [vytváření v protokolu upozornění v Azure](../../azure-monitor/platform/alerts-log.md).
* Vysvětlení [webhooky v protokolu upozornění v Azure](alerts-log-webhook.md).
* Další informace o [upozornění Azure](../../azure-monitor/platform/alerts-overview.md).
* Další informace o [Application Insights](../../azure-monitor/app/analytics.md).
* Další informace o [Log Analytics](../../azure-monitor/log-query/log-query-overview.md).