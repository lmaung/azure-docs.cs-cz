---
title: Odesílání událostí pomocí Javy – Azure Event Hubs | Dokumentace Microsoftu
description: Tento článek obsahuje názorný vytváření aplikace v Javě, která zasílá události do služby Azure Event Hubs.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 4da89e0f99832c429091e0a028fafd8943df811a
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2019
ms.locfileid: "55300111"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Odesílání událostí do služby Azure Event Hubs pomocí Javy

Azure Event Hubs je platforma pro streamování velkých objemů dat a služba pro ingestování událostí, která je schopná přijmout a zpracovat miliony událostí za sekundu. Služba Event Hubs dokáže zpracovávat a ukládat události, data nebo telemetrické údaje produkované distribuovaným softwarem a zařízeními. Data odeslaná do centra událostí je možné transformovat a uložit pomocí libovolného poskytovatele analýz v reálném čase nebo adaptérů pro dávkové zpracování a ukládání. Podrobnější přehled služby Event Hubs najdete v tématech [Přehled služby Event Hubs](event-hubs-about.md) a [Funkce služby Event Hubs](event-hubs-features.md).

Tento kurz ukazuje, jak odesílat události do centra událostí pomocí konzolové aplikace napsané v jazyce Java. 

> [!NOTE]
> Tento rychlý start si můžete stáhnout jako ukázku z [GitHubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), nahradit řetězce `EventHubConnectionString` a `EventHubName`, hodnotami pro vaše centrum událostí a spustit. Alternativně můžete vytvořit vlastní řešení podle kroků v tomto kurzu.

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu musí být splněné následující požadavky:

* Vývojové prostředí Java. Tento kurz používá [Eclipse](https://www.eclipse.org/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Vytvoření oboru názvů Event Hubs a centra událostí
Prvním krokem je použití webu [Azure Portal](https://portal.azure.com) k vytvoření oboru názvů typu Event Hubs a získání přihlašovacích údajů pro správu, které vaše aplikace potřebuje ke komunikaci s centrem událostí. Pokud chcete vytvořit obor názvů a centra událostí, postupujte podle pokynů v [v tomto článku](event-hubs-create.md).

Získání hodnoty přístupový klíč pro Centrum událostí podle pokynů v článku: [Získání připojovacího řetězce](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). V kódu, který napíšete později v tomto kurzu použijete přístupový klíč. Je výchozí název klíče: **RootManageSharedAccessKey**.

Nyní postupujte podle následující kroků v tomto kurzu.

## <a name="add-reference-to-azure-event-hubs-library"></a>Přidat odkaz na knihovnu služby Azure Event Hubs

Klientská knihovna Java pro Event Hubs je k dispozici pro použití v projektech Maven z [centrálního úložiště Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Můžete využít tuto knihovnu pomocí následující deklarace závislostí uvnitř souboru projektu Maven:

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
```

Pro různé typy prostředí sestavení, můžete explicitně získat nejnovější vydané soubory JAR z [centrálního úložiště Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Pro jednoduché události vydavatele, import *com.microsoft.azure.eventhubs* balíček pro klientské třídy služby Event Hubs a *com.microsoft.azure.servicebus* jako balíček pro nástroj třídy běžné výjimky, které jsou sdíleny s klienta zasílání zpráv Azure Service Bus. 

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Napsání kódu pro odesílání zpráv do centra událostí

Pro následující příklad nejprve vytvořte nový projekt Maven pro aplikaci konzoly nebo prostředí v oblíbeném vývojovém prostředí Java. Přidejte třídu pojmenovanou `SimpleSend`a přidejte následující kód do třídy:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Vytvoření připojovacího řetězce

ConnectionStringBuilder třídu použijte k sestavení kompletních hodnotu připojovacího řetězce k předání do instance služby Event Hubs klienta. Nahraďte zástupné symboly hodnotami, které jste získali při vytváření obor názvů a Centrum událostí:

```java
        final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                .setNamespaceName("speventhubns") 
                .setEventHubName("speventhub")
                .setSasKeyName("RootManageSharedAccessKey")
                .setSasKey("2+WMsyyy1XmUtEnRsfOmTTyGasfJgsVjGAOIN20J1Y8=");
```

### <a name="send-events"></a>Odesílání událostí

Vytvořte jednotný událost pomocí transformace řetězce do jeho kódování bajtů kódování UTF-8. Potom vytvořte nové instance služby Event Hubs klienta z připojovacího řetězce a odeslat zprávu:   

```java 
        final Gson gson = new GsonBuilder().create();

        // The Executor handles all asynchronous tasks and this is passed to the EventHubClient instance.
        // This enables the user to segregate their thread pool based on the work load.
        // This pool can then be shared across multiple EventHubClient instances.
        // The following sample uses a single thread executor, as there is only one EventHubClient instance,
        // handling different flavors of ingestion to Event Hubs here.
        final ScheduledExecutorService executorService = Executors.newScheduledThreadPool(4);

        // Each EventHubClient instance spins up a new TCP/SSL connection, which is expensive.
        // It is always a best practice to reuse these instances. The following sample shows this.
        final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);


        try {
            for (int i = 0; i < 10; i++) {

                String payload = "Message " + Integer.toString(i);
                byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
                EventData sendEvent = EventData.create(payloadBytes);

                // Send - not tied to any partition
                // Event Hubs service will round-robin the events across all Event Hubs partitions.
                // This is the recommended & most reliable way to send to Event Hubs.
                ehClient.sendSync(sendEvent);
            }

            System.out.println(Instant.now() + ": Send Complete...");
            System.out.println("Press Enter to stop.");
            System.in.read();
        } finally {
            ehClient.closeSync();
            executorService.shutdown();
        }

``` 

Sestavení a spusťte program a ujistěte se, že zde nejsou žádné chyby.

Blahopřejeme! Nyní jste odeslali zprávy do centra událostí.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Dodatek: Směrování zpráv do oddílů centra událostí

Předtím, než zprávy jsou načítána pro spotřebitele, mají být publikována do oddílů nejprve podle vydavatele. Po publikování zprávy do centra událostí synchronně pomocí metody sendSync() com.microsoft.azure.eventhubs.EventHubClient objektu, může být zprávu odeslat do konkrétního oddílu ani distribuován do všech dostupných oddílech způsobem kruhové dotazování v závislosti na tom, jestli je nebo není zadána klíč oddílu.

Pokud je zadán řetězec představující klíč oddílu, klíč se k určení oddíl, který se k odeslání události do hashovat.

Pokud není nastaven klíč oddílu, pak zprávy budou kruhové robined na všechny dostupné oddíly.

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>Další postup

V tomto rychlém startu jste odeslali zprávy do centra událostí pomocí Javy. Další způsob příjmu událostí z centra událostí pomocí Javy najdete v tématu [přijímat události z centra událostí – Java](event-hubs-java-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

