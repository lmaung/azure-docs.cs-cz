---
title: Osvědčené postupy Azure aplikace Service Fabric a clusteru | Dokumentace Microsoftu
description: Doporučené postupy pro správu clusterů Service Fabric a aplikací.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: a0174a5442de7a10e45e9dc2a2f43f5f401d1a69
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805066"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Osvědčené postupy Azure aplikace Service Fabric a clusteru

Ke správě aplikace Azure Service Fabric a clustery úspěšně, jsou operace, které důrazně doporučujeme že provést k optimalizaci spolehlivosti provozním prostředí. provedení operace definované v tomto dokumentu a vyberte jednu z našich [Cluster Service Fabric pomocí Azure Ukázky šablon](https://github.com/Azure-Samples/service-fabric-cluster-templates) začít návrhu řešení pro výrobu nebo upravit stávající šablonu začlenit tyto postupy.

## <a name="security"></a>Zabezpečení 

* [Osvědčené postupy pro zabezpečení](service-fabric-best-practices-security.md)

## <a name="networking"></a>Sítě

* [Osvědčené postupy pro síť](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>Škálování a plánování výpočetních prostředků

* [Osvědčené postupy pro změnu velikosti výpočetní prostředky](service-fabric-best-practices-capacity-scaling.md)
* [COMPUTE plánování kapacity](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Infrastruktura jako kód

* [Osvědčené postupy při implementaci infrastruktury jako kódu](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>Monitorování a diagnostika

* [Osvědčené postupy pro monitorování clusteru a Diagnostika](service-fabric-best-practices-monitoring.md)

## <a name="checklist"></a>Kontrolní seznam

Po dokončení všechny výše uvedené oddíly, ujistěte se, integrované všech osvědčených postupů v seznamu úkolů připravenosti produkčního prostředí:
* [Azure Service Fabric produkční připravenosti kontrolní seznam](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Další postup

* Vytvoření clusteru na virtuální počítače nebo počítače se systémem Windows Server: [Vytvoření clusteru Service Fabric pro Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Vytvoření clusteru na počítače s Linuxem nebo virtuální počítače: [Vytvoření clusteru s Linuxem](service-fabric-cluster-creation-via-portal.md)
* Řešení potíží: [Průvodce odstraňováním potíží pro Service Fabric](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)