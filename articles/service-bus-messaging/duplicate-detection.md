---
title: Detekce duplicitních zpráv ve službě Azure Service Bus | Dokumentace Microsoftu
description: Zjišťování duplicitních zpráv služby Service Bus
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: c419ee1eec9e451cad835d8b4a56818101dc853a
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856209"
---
# <a name="duplicate-detection"></a>Vyhledávání duplicit

Pokud aplikace selže kvůli závažné chybě ihned po odešle zprávu a instance aplikace restartovala chybně domnívá, že předchozí zprávu doručování nedošlo k, následné odeslat způsobí, že stejná zpráva se zobrazí v systému dvakrát.

Je také možné chyby na úrovni klienta nebo síti dojde k chvíli předtím a odeslané zprávy, která potvrdí do fronty se potvrzení úspěšně vrácen do klienta. Tento scénář ponechá klienta v výsledek operace odeslání.

Vyhledávání duplicit trvá nejisté z těchto situací tím, že odesílatel znovu odeslal stejnou zprávu a fronty nebo tématu zahodí všechny duplicitní kopie.

Když se povolí zjišťování duplicit pomáhá udržovat přehled o řízené aplikací *MessageId* všech zpráv odeslaných do fronty nebo tématu během zadaného časového intervalu. Pokud se neposílají žádné nové zprávy *MessageId* , která se zaznamenala během časového intervalu, zpráva se oznámilo, přijato (úspěšné operaci odeslání), ale nově odeslané zprávě okamžitě ignorována a vyřadit. Žádné jiné části zprávy jiné než *MessageId* jsou považovány za.

Řízení aplikací v identifikátoru je nezbytné, protože pouze to, že umožňuje, aby aplikace a jejich zapojení *MessageId* na obchodní proces kontext, ze kterého ho můžete předvídatelně znovu vytvořena po dojde k chybě.

Pro obchodní proces, ve kterém jsou odesílány více zpráv v průběhu zpracování některých kontext aplikace *MessageId* může být složené identifikátor kontextu úrovni aplikace, jako je například čísla nákupních objednávek a předmět zprávy, například **12345.2017/platební**.

*MessageId* může vždy být některé identifikátor GUID, ale ukotvení identifikátor obchodního procesu poskytuje předvídatelný opakovatelnosti, který je požadován pro efektivní využití funkce vyhledávání duplicit.

## <a name="enable-duplicate-detection"></a>Povolit vyhledávání duplicit

Na portálu, tato funkce je zapnutá během vytváření entit s **povolit vyhledávání duplicit** zaškrtávací políčko, které je ve výchozím nastavení vypnuté. Je ekvivalentní nastavení pro vytvoření nových témat.

![][1]

> [!IMPORTANT]
> Je nemůže povolit nebo zakázat zjišťování duplicit po vytvoření fronty. Můžete tak učinit pouze v době vytváření fronty. 

Prostřednictvím kódu programu, nastavte příznak s [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) vlastnost na úplné rozhraní .NET API. Pomocí rozhraní API Azure Resource Manageru, je hodnota nastavena s [queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) vlastnost.

Historie vyhledávání duplicit čas výchozí hodnota je 30 sekund u front a témat, přičemž maximální hodnota sedm dní. Můžete změnit toto nastavení v okně Vlastnosti frontu a téma na webu Azure Portal.

![][2]

Prostřednictvím kódu programu, můžete nakonfigurovat velikost okna zjišťování duplicit, během kterého se zachovají identifikátory zpráv, pomocí [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) vlastnost s úplné rozhraní .NET Framework API . Pomocí rozhraní API Azure Resource Manageru, je hodnota nastavena s [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) vlastnost.

Povolení zjišťování duplicit a velikost okna přímý vliv na propustnosti fronty (a téma), protože všechny zaznamenané ID zprávy musí být hledána identifikátor nově odeslané zprávy.

Udržování okno malé znamená, že méně identifikátory zpráv musí být zachovány a odpovídající a propustnost je ovlivněný menší. Vysoká propustnost entit, které vyžadují vyhledávání duplicit byste měli mít v okně co nejmenší.

## <a name="next-steps"></a>Další postup

Další informace o zasílání zpráv Service Bus, najdete v následujících tématech:

* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Začínáme s frontami služby Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak používat témata a odběry Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
