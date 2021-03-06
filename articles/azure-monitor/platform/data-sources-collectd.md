---
title: Shromažďování dat z shromážděná ve službě Azure Monitor | Dokumentace Microsoftu
description: Shromážděná je linuxového démona otevřít zdroj, který pravidelně shromažďuje data z aplikací a informace na úrovni systému.  Tento článek obsahuje informace o shromažďování dat z shromážděná ve službě Azure Monitor.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/27/2018
ms.author: magoedte
ms.openlocfilehash: b6785dc06107424344f0a6af775abe9b1c956f70
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "55999313"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-azure-monitor"></a>Shromažďovat data shromážděná na agentech pro Linux ve službě Azure Monitor
[Shromážděná](https://collectd.org/) je linuxového démona otevřít zdroj, který pravidelně shromažďuje metriky výkonu z aplikace a informace na úrovni systému. Příklad aplikace obsahují Java Virtual Machine (JVM), MySQL Server a Nginxu. Tento článek obsahuje informace o shromažďování dat výkonu z shromážděná ve službě Azure Monitor.

Úplný seznam dostupných modulů plug-in najdete v [tabulky moduly plug-in](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Přehled shromážděná](media/data-sources-collectd/overview.png)

Následující konfigurace shromážděná je součástí agenta Log Analytics pro Linux ke směrování shromážděná data do Log Analytics agenta pro Linux.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Kromě toho pokud se používá verzích shromážděná před 5.5 místo toho použijte následující konfiguraci.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

Konfigurace shromážděná používá výchozí`write_http` modul plug-in, který odešlete data metrik výkonu přes port 26000 do agenta Log Analytics pro Linux. 

> [!NOTE]
> V případě potřeby lze nastavit na vlastní port tohoto portu.

Agenta Log Analytics pro Linux také naslouchá na portu 26000 shromážděná metriky a převede je do schématu metrik Azure monitoru. Tady je agenta Log Analytics pro Linux konfiguraci `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Verze podporováno
- Azure Monitor v současné době podporuje shromážděná verze 4,8 a vyšší.
- Je vyžadováno pro shromažďování metrik shromážděná agenta log Analytics pro Linux v1.1.0-217 nebo novější.


## <a name="configuration"></a>Konfigurace
Tady jsou základní kroky pro konfiguraci kolekce shromážděná data ve službě Azure Monitor.

1. Nakonfigurujte shromážděná k odesílání dat do agenta Log Analytics pro Linux s využitím modulu plug-in write_http.  
2. Konfigurace agenta Log Analytics pro Linux pro naslouchání shromážděná data na příslušný port.
3. Restartujte agenta shromážděná a Log Analytics pro Linux.

### <a name="configure-collectd-to-forward-data"></a>Konfigurace shromážděná k předávání dat 

1. K postupu shromážděná data do Log Analytics agenta pro Linux `oms.conf` musí být přidán do vaší shromážděná konfigurační adresář. Cílem tohoto souboru závisí na distribuce Linuxu vašeho počítače.

    Pokud je ve /etc/collectd.d/ adresáři shromážděná config:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Pokud je ve /etc/collectd/collectd.conf.d/ adresáři shromážděná config:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Pro verze shromážděná před 5.5 budete muset upravit značky v `oms.conf` jak je znázorněno výše.
    >

2. Zkopírujte collectd.conf do požadovaného pracovního prostoru omsagent konfigurační adresář.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Restartujte agenta shromážděná a Log Analytics pro Linux pomocí následujících příkazů.

    sudo služby shromážděná restartování sudo /opt/microsoft/omsagent/bin/service_control restartování

## <a name="collectd-metrics-to-azure-monitor-schema-conversion"></a>Shromážděná metriky k převodu schématu Azure Monitor
Chcete-li zachovat známým modelem mezi metriky infrastruktury ještě shromažďují pomocí agenta Log Analytics pro Linux a nové metriky shromážděné shromážděná následující schéma mapování se používá:

| Metrika shromážděná pole | Azure Monitor pole |
|:--|:--|
| hostitel | Počítač |
| Modul plug-in | Žádný |
| plugin_instance | Název instance<br>Pokud **plugin_instance** je *null* pak InstanceName = "*_celkem*" |
| type | Název objektu |
| type_instance | Hodnota counterName<br>Pokud **type_instance** je *null* pak CounterName =**prázdné** |
| [] dsnames | Hodnota counterName |
| dstypes | Žádný |
| hodnoty] | CounterValue |

## <a name="next-steps"></a>Další postup
* Další informace o [protokolu dotazy](../log-query/log-query-overview.md) analyzovat data shromážděná ze zdrojů dat a jejich řešení. 
* Použití [vlastní pole](custom-fields.md) analyzovat data ze záznamů protokolu syslog do jednotlivých polí.