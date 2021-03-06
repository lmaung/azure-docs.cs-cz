---
title: Doba pro příjem dat protokolu ve službě Azure Monitor | Dokumentace Microsoftu
description: Vysvětluje různé faktory, které mají vliv latence ve shromažďování dat protokolu ve službě Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2019
ms.author: bwren
ms.openlocfilehash: ba9a0ab775e062f21a058b537e289fe3ea2b40bb
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000042"
---
# <a name="log-data-ingestion-time-in-azure-monitor"></a>Čas příjem dat protokolu ve službě Azure Monitor
Azure Monitor je služba vysokou škálovatelností dat, která slouží tisíce zákazníků odesílání terabajty dat měsíčně rostoucí tempem. Jsou často dotazy týkající se čas potřebný pro data protokolu k dispozici po shromáždění zpracovat. Tento článek vysvětluje různé faktory ovlivňující tuto latenci.

## <a name="typical-latency"></a>Typické latence
Latence odkazuje na čas, který data se vytvoří v monitorovaném systému a čas, který je k dispozici pro analýzy ve službě Azure Monitor. Typické latence pro příjem dat protokolu je mezi 2 až 5 minut. Konkrétní latence pro všechna data, zejména se liší v závislosti na řadě faktorů, které je popsáno níže.


## <a name="factors-affecting-latency"></a>Faktory ovlivňující latence
Ingestování celkový čas pro konkrétní sadu data dají rozdělit do následujících oblastech vyšší úrovně. 

- Agent čas – čas zjištění událost, shromažďování vyžadováno a následně je odeslat do Azure monitoru bodem ingestování jako záznam protokolu. Ve většině případů se tento proces zařizuje Služba agenta.
- Kanál čas – čas potřebný pro ingestování kanálu ke zpracování záznamu protokolu. Jedná se o analýze vlastnosti události a potenciálně přidáním počítaných informací.
- Indexování čas – čas strávený na příjem záznam protokolu do služby Azure Monitor ukládat velké objemy dat.

Podrobnosti o různých latencí v tomto procesu jsou popsané níže.

### <a name="agent-collection-latency"></a>Latence kolekce agenta
Agenti a řešení pro správu používají různé strategie ke shromažďování dat z virtuálního počítače, který může mít vliv na latenci. Některé konkrétní příklady patří:

- Okamžitě se shromažďují události Windows, události procesu syslog a metriky výkonu. Čítače výkonu systému Linux jsou dotazovat každých 30 sekund.
- Jakmile se změní jejich časového razítka se shromažďují protokoly služby IIS a vlastní protokoly. Pro protokoly služby IIS, to je ovlivněno [nakonfigurovaný ve službě IIS plán výměny](data-sources-iis-logs.md). 
- Řešení Active Directory replikace provede posouzení každých pět dní, během řešení Active Directory Assessment provádí týdenní hodnocení infrastruktury služby Active Directory. Agent bude shromažďovat protokoly pouze po dokončení hodnocení.

### <a name="agent-upload-frequency"></a>Frekvence odesílání agenta
K zajištění, že agenta Log Analytics je jednoduché, agent ukládá do vyrovnávací paměti protokoly a pravidelně odesílá je do Azure monitoru. Nahrát frekvence se pohybuje mezi 30 sekund a 2 minuty. záleží na typu dat. Většina dat je nahraný v části 1 minuta. Stavy sítě může mít nepříznivý vliv latence těchto dat k dosažení bodem ingestování Azure Monitor.

### <a name="azure-activity-logs-diagnostic-logs-and-metrics"></a>Protokoly aktivit Azure, diagnostické protokoly a metriky
Azure data přidá další čas k dispozici v Log Analytics ingestování okamžiku ke zpracování:

- Data z diagnostické protokoly trvat 2 až 15 minut, v závislosti na službu Azure. Najdete v článku [dotazu níže](#checking-ingestion-time) ke kontrole latence ve vašem prostředí
- Platforma Azure metriky trvat 3 minuty k odeslání do Log Analytics ingestování datových bodů.
- Data protokolu aktivit, bude trvat přibližně 10 – 15 minutách k odeslání do Log Analytics ingestování datových bodů.

Jakmile je k dispozici v okamžiku ingestování dat trvá dalších 2 až 5 minut k dispozici pro dotazování.

### <a name="management-solutions-collection"></a>Kolekce řešení správy
Některá řešení neshromažďují svá data z agenta a mohou používat metodu kolekce, která zavádí další latenci. Některá řešení bez pokusu o kolekci téměř v reálném čase shromažďovat data v pravidelných intervalech. Konkrétní příklady patří:

- Řešení pro Office 365 se dotazuje správu aktivity rozhraní API Office 365, který aktuálně neposkytuje záruky latence jakékoli téměř v reálném čase pomocí protokolů aktivit.
- Řešení na denní frekvence shromažďuje data řešení (informace o kompatibilitě pro příklad) Windows Analytics.

Naleznete v dokumentaci pro každé řešení určit její interval shromažďování.

### <a name="pipeline-process-time"></a>Čas zpracování kanálu
Jakmile jsou záznamy protokolu přijatých do kanálu Azure Monitor, jsou napsaná do dočasného úložiště zajistit izolaci klientů a ujistěte se, že data nejsou ztracena. Tento postup přidá obvykle 5 až 15 sekund. Některá řešení pro správu implementují těžší algoritmy ke shromáždění dat a vyvoďte z nich jako streamování dat v. Například monitorování výkonu sítě agreguje příchozích dat přes 3minutové intervaly efektivně přidání 3minutové latence. Jiný proces, který zvyšuje latenci je proces, který zpracuje vlastní protokoly. V některých případech může tento proces přidat několik minut, latenci na protokoly, které se shromažďují ze souborů pomocí agenta.

### <a name="new-custom-data-types-provisioning"></a>Nové vlastní datové typy zřizování
Při vytvoření nového typu vlastních dat z [vlastního protokolu](data-sources-custom-logs.md) nebo [rozhraní API kolekce dat](data-collector-api.md), systém vytvoří kontejner vyhrazeného úložiště. Toto je jednorázová režijní náklady, ke které dochází pouze na první výskyt tento typ dat.

### <a name="surge-protection"></a>Nárůst ochrany
Hlavní prioritou služby Azure Monitor je zajistit, že žádná zákaznická data nejsou ztracena, tak systém obsahuje integrovanou ochranu pro data nárůstů. To zahrnuje vyrovnávací paměti k zajištění, že i v rámci obrovské zatížení, systém bude nadále funkční. Při normálním zatížení tyto ovládací prvky přidat méně než minutu, ale v extrémní podmínky a selhání, přidat spoustu času, zatímco je zajištěna dat je bezpečné.

### <a name="indexing-time"></a>Indexování čas
Je integrované vyrovnávání pro každou platformu pro velké objemy dat mezi analytics a na rozdíl od okamžitý přístup k datům poskytuje pokročilé vyhledávací funkce. Azure Monitor umožňuje spouštět výkonné dotazy na miliard záznamů a získání výsledků během pár sekund. To je provést je to možné, protože infrastruktury výrazně transformuje data během jeho ingestování a ukládá ho do struktury jedinečné compact. Systém ukládá do vyrovnávací paměti dat. dokud dost není k dispozici k vytvoření těchto struktur. Je nutné dokončit předtím, než se ve výsledcích hledání zobrazí záznam protokolu.

Tento proces aktuálně trvá přibližně 5 minut. Pokud je malé množství dat, ale méně času na vyšší datové sazby. To zdá být neintuitivní, ale tento proces umožní optimalizace v případě velkého objemu produkční úlohy.



## <a name="checking-ingestion-time"></a>Kontrola ingestování času
Ingestování doba může lišit pro různé prostředky za různých okolností. Protokol dotazů můžete použít k identifikaci konkrétní chování vašeho prostředí.

### <a name="ingestion-latency-delays"></a>Zpoždění latence příjmu
Můžete měření latence konkrétní záznam porovnáním výsledek [ingestion_time()](/azure/kusto/query/ingestiontimefunction) funkce _TimeGenerated_ pole. Tato data je možné pomocí různých agregacích najít chování latence příjmu dat. Prozkoumejte některé percentilu času ingestování získat přehledy pro velký objem dat. 

Například následující dotaz zobrazí počítače, které má nejvyšší doba ingestování za aktuální den: 

``` Kusto
Heartbeat
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by Computer 
| top 20 by percentile_E2EIngestionLatency_95 desc  
```
 
Pokud chcete k podrobnostem na příjem čas pro určitý počítač po určitou dobu, použijte následující dotaz, který také vizualizuje data v grafu: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(24h) and Computer == "ContosoWeb2-Linux"  
| extend E2EIngestionLatencyMin = todouble(datetime_diff("Second",ingestion_time(),TimeGenerated))/60 
| summarize percentiles(E2EIngestionLatencyMin,50,95) by bin(TimeGenerated,30m) 
| render timechart  
```
 
Chcete-li zobrazit čas ingestování počítače podle země, že se nacházejí ve které je na základě jejich IP adresy použijte tento dotaz: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by RemoteIPCountry 
```
 
Různé datové typy, které pocházejí z agenta může mít různé ingestování čekací dobu, tak předchozí dotazy můžete použít jiné typy. Použijte tento dotaz k prozkoumání době příjmu různé služby Azure: 

``` Kusto
AzureDiagnostics 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by ResourceProvider
```

### <a name="resources-that-stop-responding"></a>Prostředky, které přestanou reagovat 
V některých případech se může zastavit prostředku odesílá data. Vysvětlení, pokud prostředek je odesílání dat, nebo Ne, podívejte se na jeho poslední záznam, který lze identifikovat podle standardu _TimeGenerated_ pole.  

Použití _prezenčního signálu_ tabulky ke kontrole dostupnosti virtuálního počítače, protože prezenční signál agentovi odesílají jednou za minutu. Použijte tento dotaz můžete vytvořit seznam aktivních počítačů, kteří nehlásili nedávno prezenčního signálu: 

``` Kusto
Heartbeat  
| where TimeGenerated > ago(1d) //show only VMs that were active in the last day 
| summarize NoHeartbeatPeriod = now() - max(TimeGenerated) by Computer  
| top 20 by NoHeartbeatPeriod desc 
```

## <a name="next-steps"></a>Další postup
* Čtení [smlouvu o úrovni (SLA) služeb](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) pro Azure Monitor.

