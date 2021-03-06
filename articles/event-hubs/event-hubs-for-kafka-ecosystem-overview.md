---
title: Použití centra událostí z aplikací Apache Kafka – Azure Event Hubs | Dokumentace Microsoftu
description: Tento článek obsahuje informace o podporovaných Apache Kafka se službou Azure Event Hubs.
services: event-hubs
documentationcenter: .net
author: shvija
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: 8bf381e7c66e06bbaa140ed865f0f7c9b4f001af
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452339"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Použití služby Azure Event Hubs z aplikací Apache Kafka
Služba Event Hubs poskytuje Kafka koncového bodu, který lze použít ve stávajících Kafka aplikací založených na jako alternativu ke spuštění clusteru Kafka. Podporuje Služba Event Hubs [protokolu Apache Kafka 1.0 nebo novější](https://kafka.apache.org/documentation/)a funguje s vašimi stávajícími aplikacemi Kafka, včetně nástroje MirrorMaker.  

## <a name="what-does-event-hubs-for-kafka-provide"></a>Co Event Hubs pro systém Kafka poskytuje?

Služba Event Hubs pro funkci Kafka poskytuje head protokolu nad Azure Event Hubs, která je binární kompatibilní s Kafka verze 1.0 a novějším pro čtení i zápis témat Kafka. Můžete začít pomocí koncového bodu Kafka z vašich aplikací se žádné změny kódu, ale minimální konfiguraci změnit. Můžete aktualizovat připojovací řetězec v konfiguracích tak, aby odkazoval na koncový bod Kafka vystavené vašeho centra událostí místo směřující do vašeho clusteru Kafka. Pak můžete začít, streamování událostí z aplikací, které používají protokol Kafka do služby Event Hubs. Tato integrace také podporuje architektury, jako je [připojení Kafka](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect), která je aktuálně ve verzi preview. 

Koncepčně Kafka a Event Hubs jsou téměř identické: jsou obě dělené protokoly vytvořené pro streamovaná data. Následující tabulka mapuje koncepty mezi Kafka a Event Hubs.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka a Centrum událostí koncepční mapování

| Koncept Kafka | Event Hubs koncept|
| --- | --- |
| Cluster | Obor názvů |
| Téma | Centrum událostí |
| Oddíl | Oddíl|
| Skupina uživatelů | Skupina uživatelů |
| Posun | Posun|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Hlavní rozdíly mezi Kafka a Event Hubs

Zatímco [Apache Kafka](https://kafka.apache.org/) je software, který můžete spustit bez ohledu na to zvolíte, Event Hubs je Cloudová služba, která je podobná do Azure Blob Storage. Neexistují žádné servery nebo sítě pro správu a bez zprostředkovatele konfigurace. Vytvoření oboru názvů, což je plně kvalifikovaný název domény, ve kterém témat pracovat, a pak vytvořte službu Event Hubs nebo témat v daném oboru názvů. Další informace o Event Hubs a obory názvů, naleznete v tématu [funkcích služby Event Hubs](event-hubs-features.md#namespace). Jako cloudová služba a Služba Event Hubs využívá jednu stabilní virtuální IP adresu jako koncový bod, tak klienty není potřeba vědět o zprostředkovatele nebo počítače v rámci clusteru. 

Škálování ve službě Event Hubs je řízen prostřednictvím kolik jednotek propustnosti nákupu, s každou jednotkou propustnosti opravňující je 1 MB za sekundu nebo 1000 událostí za sekundu z příchozího přenosu. Ve výchozím nastavení, služby Event Hubs škálování jednotek propustnosti při dosažení limitu se [automatické rozšiřování](event-hubs-auto-inflate.md) funkce; tato funkce také pomocí služby Event Hubs pro funkci Kafka. 

### <a name="security-and-authentication"></a>Zabezpečení a ověřování

Azure Event Hubs vyžaduje SSL nebo TLS pro veškerou komunikaci a používá sdílených přístupových podpisů (SAS) pro ověřování. Tento požadavek platí taky pro koncový bod Kafka v rámci služby Event Hubs. Z důvodu kompatibility s využitím Kafka Služba Event Hubs využívá SASL prostý pro ověřování a SASL protokolu SSL pro zabezpečení přenosu. Další informace o zabezpečení ve službě Event Hubs najdete v tématu [ověřování služby Event Hubs a zabezpečení](event-hubs-authentication-and-security-model-overview.md).

## <a name="other-event-hubs-features-available-for-kafka"></a>Další funkce Event Hubs k dispozici pro systém Kafka

Event Hubs pro funkci Kafka umožňuje zápis s jeden protokol a čtení sebou, tak, aby vaše aktuální Kafka výrobci můžete pokračovat v publikování prostřednictvím Kafka, a přidáte čtenáře s využitím služby Event Hubs, jako je Azure Stream Analytics nebo Azure Functions. Kromě toho Služba Event Hubs funkce, jako [zachycení](event-hubs-capture-overview.md) a [geografického zotavení po havárii](event-hubs-geo-dr.md) spolupracovat také se službami Event Hubs pro funkci Kafka.

## <a name="features-that-are-not-yet-supported"></a>Funkce, které se zatím nepodporují 

Tady je seznam Kafka funkce, které se zatím nepodporují:

*   Výrobce Idempotentní
*   Transakce
*   Komprese
*   Uchovávání informací podle velikosti
*   Komprese protokolu
*   Přidání oddílů do stávajícího tématu
*   Podpora HTTP Kafka API
*   Rozhraní Kafka Streams

## <a name="next-steps"></a>Další postup

Tento článek poskytuje úvod do služby Event Hubs pro systém Kafka. Další informace najdete v následujících tématech:

- [Vytvoření služby Event Hubs s podporou Kafka](event-hubs-create-kafka-enabled.md)
- [Streamování do služby Event Hubs z aplikací Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Zrcadlení zprostředkovatele Kafka v centru událostí s podporou Kafka](event-hubs-kafka-mirror-maker-tutorial.md)
- [Připojení Apache Sparku k centru událostí s podporou Kafka](event-hubs-kafka-spark-tutorial.md)
- [Připojení Apache Flinku k centru událostí s podporou Kafka](event-hubs-kafka-flink-tutorial.md)
- [Integrace připojení Kafka s centrem událostí s podporou Kafka](event-hubs-kafka-connect-tutorial.md)
- [Připojení Akka Streams k centru událostí s podporou Kafka](event-hubs-kafka-akka-streams-tutorial.md)
- [Prozkoumejte ukázky na našem GitHubu](https://github.com/Azure/azure-event-hubs-for-kafka)


