---
title: Monitorování výkonu webových aplikací Java v Linuxu – Azure | Dokumentace Microsoftu
description: Rozšířené monitorování výkonu aplikací z vašeho webu Java pomocí modul plug-in shromážděná pro službu Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: c8320a0f504927830c47400f1f1ef0369c0e1cad
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2019
ms.locfileid: "54116530"
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>shromážděná: Metriky výkonu systému Linux ve službě Application Insights


Prozkoumat metriky výkonu systému Linux v [Application Insights](../../azure-monitor/app/app-insights-overview.md), nainstalujte [shromážděná](https://collectd.org/)společně s jeho modul plug-in Application Insights. Toto řešení open source shromáždí různé systémové a síťové statistiky.

Obvykle použijete shromážděná, pokud už máte [instrumentována webová služba jazyka Java pomocí Application Insights][java]. Poskytuje víc dat, která umožňují zvýšit výkon vaší aplikace nebo diagnostiky problémů. 

![Ukázkové grafy](./media/java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>Získejte klíč instrumentace
V [portálu Microsoft Azure](https://portal.azure.com), otevřete [Application Insights](../../azure-monitor/app/app-insights-overview.md) prostředků, ve kterém chcete data zobrazit. (Nebo [vytvořit nový prostředek](../../azure-monitor/app/create-new-resource.md ).)

Pořiďte si klíč instrumentace, který identifikuje prostředek.

![Procházet vše, otevřete prostředek a pak v Essentials rozevíracího seznamu, vyberte a zkopírujte klíč instrumentace](./media/java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a>Nainstalujte shromážděná a modulu plug-in
Na počítačích serveru Linux:

1. Nainstalujte [shromážděná](https://collectd.org/) verze 5.4.0 nebo vyšší.
2. Stáhněte si [modul plug-in zapisovače shromážděná Application Insights](https://aka.ms/aijavasdk). Poznamenejte si číslo verze.
3. Modul plug-in JAR do kopie `/usr/share/collectd/java`.
4. Upravit `/etc/collectd/collectd.conf`:
   * Ujistěte se, že [modul Java plug-in](https://collectd.org/wiki/index.php/Plugin:Java) je povolená.
   * Aktualizujte JVMArg pro java.class.path zahrnout následující soubor JAR. Aktualizujte tak, aby odpovídaly ten, který jste si stáhli číslo verze:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Přidejte tento fragment kódu používá klíč instrumentace z vašeho prostředku:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Tady je součástí ukázkového souboru konfigurace:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Konfigurace dalších [moduly plug-in shromážděná](https://collectd.org/wiki/index.php/Table_of_Plugins), což může shromažďovat data z různých zdrojů.

Restartujte shromážděná podle jeho [ruční](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-the-data-in-application-insights"></a>Zobrazení dat ve službě Application Insights
V prostředku Application Insights, otevřete [Průzkumníka metrik a panel přidávat grafy][metrics], výběr metriky, které chcete zobrazit v kategorii Vlastní.

![](./media/java-collectd/result.png)

Ve výchozím nastavení metriky se agregují napříč všechny hostitelské počítače, ze kterých byly shromážděny metriky. Pokud chcete zobrazit metriky podle hostitelů, v okně podrobností graf, zapněte seskupování a pak se rozhodnout Seskupit podle shromážděná hostitele.

## <a name="to-exclude-upload-of-specific-statistics"></a>Vyloučit nahrávání konkrétní Statistika
Ve výchozím nastavení modul plug-in Application Insights odesílá všechna data shromažďovaná společností povoleno shromážděná čtení moduly plug-in. 

Vyloučení dat z konkrétních modulů plug-in nebo zdroje dat:

* Upravte konfigurační soubor. 
* V `<Plugin ApplicationInsightsWriter>`, přidejte direktivu řádky takto:

| – Direktiva | Efekt |
| --- | --- |
| `Exclude disk` |Vyloučení všech dat shromážděných službou `disk` modulu plug-in |
| `Exclude disk:read,write` |Vyloučení zdrojů s názvem `read` a `write` z `disk` modulu plug-in. |

Samostatné direktivy pomocí nového řádku.

## <a name="problems"></a>Problémy?
*Nevidím žádná data na portálu*

* Otevřít [hledání] [ diagnostic] zobrazíte, pokud byly přijaty nezpracovaných událostí. Někdy jsou trvat delší dobu se zobrazí v Průzkumníku metrik.
* Možná budete muset [nastavit výjimky brány firewall pro odchozích dat](../../azure-monitor/app/ip-addresses.md)
* Povolení trasování v modulu plug-in Application Insights. Přidejte tento řádek v rámci `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Otevřete terminál a spusťte v režimu s komentářem, chcete-li zobrazit všechny problémy, které se hlásí shromážděná:
  * `sudo collectd -f`

## <a name="known-issue"></a>Známý problém

Modul plug-in Application Insights zápisu není kompatibilní s některé moduly plug-in pro čtení. Některé moduly plug-in někdy odeslat "NaN", kde modul plug-in Application Insights očekává číslo s plovoucí desetinnou čárkou.

Příznaky: Shromážděná protokol zobrazuje chyby, které zahrnují "AI:... Chyba syntaxe: Neočekávaný token N".

Alternativní řešení: Vylučte data shromážděná službou moduly plug-in zápisu problém. 

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md


