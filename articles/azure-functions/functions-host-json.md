---
title: referenční materiály k Host.JSON pro Azure Functions 2.x
description: Referenční dokumentace pro Azure Functions host.json soubor s modulem runtime verze 2.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/08/2018
ms.author: glenga
ms.openlocfilehash: e24c5b2be1df41d84fa4461250f51cb009f77529
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331213"
---
# <a name="hostjson-reference-for-azure-functions-2x"></a>referenční materiály k Host.JSON pro Azure Functions 2.x  

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Verze 1](functions-host-json-v1.md)
> * [Verze 2](functions-host-json.md)

*Host.json* soubor metadat obsahuje možnosti globální konfigurace, které ovlivňují všechny funkce aplikace function App. Tento článek obsahuje seznam nastavení, které jsou k dispozici pro modul runtime verze 2.  

> [!NOTE]
> Tento článek je určený pro Azure Functions 2.x.  Pro odkaz host.json ve funkcích 1.x, najdete v článku [referenční materiály k host.json pro Azure Functions 1.x](functions-host-json-v1.md).

Další možnosti konfigurace aplikace funkce se spravují v vaše [nastavení aplikace](functions-app-settings.md).

Některá nastavení host.json se použijí jenom při spuštění místně v [local.settings.json](functions-run-local.md#local-settings-file) souboru.

## <a name="sample-hostjson-file"></a>Ukázkový soubor host.json

Následující ukázka *host.json* soubory mají všechny zadané možnosti.


```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 5
            }
        }
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

Následující části tohoto článku popisují jednotlivé vlastnosti nejvyšší úrovně. Všechny jsou volitelné, pokud není uvedeno jinak.

## <a name="aggregator"></a>aggregator

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

Toto nastavení je podřízeným prvkem [protokolování](#logging).

Ovládací prvky [vzorkování funkcí ve službě Application Insights](./functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "samplingSettings": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

> [!NOTE]
> Vzorkování protokolu může způsobit, že některé spuštění není zobrazí v okně monitorování Application Insights.

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|isEnabled|true (pravda)|Povolí nebo zakáže vzorkování.| 
|maxTelemetryItemsPerSecond|5|Prahová hodnota, na které vzorkování začíná.| 

## <a name="cosmosdb"></a>cosmosDb

Konfigurace nastavení najdete v [Cosmos DB triggerů a vazeb](functions-bindings-cosmosdb-v2.md#host-json).

## <a name="durabletask"></a>durableTask

Konfigurace nastavení najdete v [vazby pro Durable Functions](durable/durable-functions-bindings.md#host-json).

## <a name="eventhub"></a>centra událostí

Konfigurace nastavení se dají najít v [centra Event aktivačními událostmi a vazbami](functions-bindings-event-hubs.md#host-json). 

## <a name="extensions"></a>Rozšíření

Vlastnosti, která vrací objekt, který obsahuje všechna nastavení specifické pro vazbu, například [http](#http) a [eventHub](#eventhub).

## <a name="functions"></a>functions

Seznam funkcí, které spustí úlohu hostitele. Prázdné pole znamená, že spuštění všech funkcí. Určený k použití pouze tehdy, když [spuštěná místně](functions-run-local.md). V aplikace function App v Azure, by měl místo toho podle kroků v [zakázání funkcí ve službě Azure Functions](disable-function.md) zakázat konkrétní funkce místo použití tohoto nastavení.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Určuje dobu trvání časového limitu pro všechny funkce. V plánu Consumption bez serveru platný rozsah je od 1 sekundy do 10 minut a výchozí hodnota je 5 minut. V plánu služby App Service neexistuje žádné omezení celkové a výchozí hodnota závisí na verzi modulu runtime. Ve verzi 2.x, výchozí hodnota pro službu App Service na plán je 30 minut. Ve verzi 1.x, je *null*, což znamená žádný časový limit.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Nastavení konfigurace pro [monitorování stavu hostitelů](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|povoleno|true (pravda)|Určuje, zda je povolena funkce. | 
|healthCheckInterval|10 sekund|Časový interval mezi stavu na pozadí pravidelně kontroluje. | 
|healthCheckWindow|2 minut|Použít ve spojení s klouzavého časového období `healthCheckThreshold` nastavení.| 
|healthCheckThreshold|6|Maximální počet pokusů o kontrolu stavu může selhat, předtím, než je zahájeno recyklace hostitele.| 
|counterThreshold|0.80|Prahová hodnota, na které čítače výkonu se budou považovat za není v pořádku.| 

## <a name="http"></a>http

Konfigurace nastavení se dají najít v [http triggerů a vazeb](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="logging"></a>Protokolování

Řídí chování protokolování aplikace function App, včetně Application Insights.

```json
"logging": {
    "fileLoggingMode": "debugOnly",
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "applicationInsights": {
        ...
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------|
|fileLoggingMode|debugOnly|Definuje, jaké úroveň protokolování do souboru je povolená.  Možnosti jsou `never`, `always`, `debugOnly`. |
|LogLevel|neuvedeno|Objekt, který definuje kategorie protokolu filtrování pro funkce v aplikaci. Verze 2.x následuje rozložení ASP.NET Core pro filtrování kategorie protokolu. To vám umožní filtrovat protokolování pro konkrétní funkce. Další informace najdete v tématu [filtrování protokolu](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) v dokumentaci k ASP.NET Core. |
|konzola|neuvedeno| [Konzoly](#console) nastavení protokolování. |
|applicationInsights|neuvedeno| [ApplicationInsights](#applicationinsights) nastavení. |

## <a name="console"></a>konzola

Toto nastavení je podřízeným prvkem [protokolování](#logging). Určuje konzoly protokolování není v režimu ladění.

```json
{
    "logging": {
    ...
        "console": {
          "isEnabled": "false"
        },
    ...
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|isEnabled|false (nepravda)|Povolí nebo zakáže protokolování konzoly.| 

## <a name="queues"></a>fronty

Konfigurace nastavení se dají najít v [úložiště fronty aktivačními událostmi a vazbami](functions-bindings-storage-queue.md#host-json).  

## <a name="sendgrid"></a>SendGrid

Konfigurace nastavení najdete v [SendGrid aktivačními událostmi a vazbami](functions-bindings-sendgrid.md#host-json).

## <a name="servicebus"></a>serviceBus

Konfigurace nastavení najdete v [aktivační události služby Service Bus a vazby](functions-bindings-service-bus.md#host-json).

## <a name="singleton"></a>singleton

Nastavení konfigurace pro chování zámku typu Singleton. Další informace najdete v tématu [problém Githubu o podpoře typu singleton](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|lockPeriod|00:00:15|Dobu, po kterou úroveň zámků funkce pořízeny pro. Zámky automatického obnovení.| 
|listenerLockPeriod|00:01:00|Období, ve kterém zamkne naslouchací proces pořízeny pro.| 
|listenerLockRecoveryPollingInterval|00:01:00|Časový interval použít pro obnovení zámku naslouchací proces, pokud při spuštění nebylo možné získat zámek naslouchací proces.| 
|lockAcquisitionTimeout|00:01:00|Maximální množství času se modul runtime pokusí se získat zámek.| 
|lockAcquisitionPollingInterval|neuvedeno|Interval mezi pokusy o získání zámku.| 

## <a name="version"></a>version

Řetězec verze `"version": "2.0"` je vyžadován pro aplikaci function app, zaměřuje na modul runtime verze 2.

## <a name="watchdirectories"></a>watchDirectories

Sada [sdílených adresářů kód](functions-reference-csharp.md#watched-directories) , který je potřeba sledovat změny.  Zajišťuje, že při změně kódu v těchto adresářích, změny se prodlouží vašich funkcí.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Zjistěte, jak aktualizovat soubor host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Podívejte se globální nastavení proměnné prostředí](functions-app-settings.md)
