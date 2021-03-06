---
title: Přístup k virtuálním sítím Azure z Azure Logic Apps se prostředí integrační služby (ISEs)
description: Tento přehled popisuje, jak pomocí prostředí integrační služby (ISEs) přístup k virtuálním sítím Azure (Vnet) aplikace logiky
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 02/24/2019
ms.openlocfilehash: ff43a55429eb69a7f98fd4da7fdbeb5d4e51513c
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56991244"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Přístup k prostředkům Azure Virtual Network v Azure Logic Apps s využitím prostředí integrační služby (ISEs)

> [!NOTE]
> Tato funkce je v [ *ve verzi public preview*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

V některých případech logic apps a účty pro integraci potřebují přístup k zabezpečeným prostředkům, jako jsou virtuální počítače (VM) a jiné systémy nebo služby, v [virtuální síť Azure](../virtual-network/virtual-networks-overview.md). Chcete-li nastavit tento přístup můžete [vytvořit *prostředí integrační služby* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) pro spouštění vašich logic apps a účty pro integraci. Při vytváření ISE Azure nasadí privátní a izolované instance služby Logic Apps do vaší virtuální sítí Azure. Tato privátní instance používá vyhrazené prostředky, jako jsou úložiště a běží odděleně od veřejné "globální" služba Logic Apps. Oddělení vaší izolované instance privátní a veřejné globální instanci také pomáhá snižovat dopad, který jiných tenantů Azure může mít na výkon vaší aplikace, která je také označována jako ["" hlučným sousedům"" efekt](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Po vytvoření vašeho ISE, když přejdete k vytvoření logiky aplikace nebo integračního účtu, můžete vybrat vaše ISE jako umístění logic app nebo integračního účtu:

![Vyberte prostředí integrační služby](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Aplikace logiky teď přímo přístupné systémy, které jsou uvnitř nebo připojené k vaší virtuální sítě pomocí některého z těchto položek:

* Konektor služby správy verzí ISE pro daný systém, například SQL Server
* Integrované triggeru nebo akce, jako je například HTTP triggeru nebo akce.
* Vlastní konektor

Tento přehled popisuje další podrobnosti o jak ISE poskytuje aplikace logiky a integrace účty přímý přístup ke službě Azure virtual network a porovnává rozdíly mezi ISE a globální služba Logic Apps.
Pro místní systémy, které nejsou připojené k virtuální síti nebo nemají ISE verze konektorů, můžete připojit k těmto systémům podle [nastavování a pomocí místní brány dat](../logic-apps/logic-apps-gateway-install.md).

> [!IMPORTANT]
> Logic apps, integrované akce a konektory, na kterých běží vaše ISE používá jiný cenový plán není založenou na skutečné spotřebě cenového plánu. Další informace najdete v tématu [ceny Logic Apps](../logic-apps/logic-apps-pricing.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Izolovaná a globální

Když vytvoříte integrované služby prostředí (ISE) v Azure, můžete vybrat virtuální síť Azure, ve které chcete *vložit* vaše ISE. Azure pak vloží nebo nasadí privátní instanci služby Logic Apps do vaší virtuální sítě. Tato akce vytvoří izolovaného prostředí, ve kterém můžete vytvořit a spustit aplikace logiky na vyhrazených prostředcích. Když vytvoříte aplikaci logiky, vyberte vaše ISE jako umístění vaší aplikace, která poskytuje přímý přístup vašich logic app k vaší virtuální sítě a prostředky v této síti.

Aplikace logiky do ISE poskytují stejné uživatelské prostředí a podobné funkce jako globální služba Logic Apps. Nejen můžete použijete stejné integrované akce a konektory v globální službě Logic Apps, ale můžete také použít konkrétní ISE konektory. Tady je příklad, některé standardní konektory, které nabízí verze, na kterých běží v prostředí ISE:

* Azure Blob Storage, File Storage a Table Storage
* Fronty Azure, Azure Service Bus, Azure Event Hubs a IBM MQ
* FTP a SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X 12 a EDIFACT

Rozdíl mezi konektory ISE a jiných ISE je v umístění, kde spouštění triggerů a akcí:

* V ISE, integrované triggery a akce, jako je HTTP vždy spustit v prostředí ISE stejné jako aplikace logiky.

* Pro konektory, které nabízejí dvě verze jednu verzi běží ISE, zatímco běží jiné verze v globální službě Logic Apps.  

  Konektory, které mají **ISE** popisek vždy spustit v prostředí ISE stejné jako aplikace logiky. Konektory bez **ISE** popisek spustit v globální službě Logic Apps.

  ![Výběr konektorů ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Konektory, které běží v prostředí ISE jsou dostupné v globální služba Logic Apps.

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Oprávnění pro přístup k virtuální síti

Abyste mohli vybrat virtuální síť Azure pro vkládání prostředí, musíte vytvořit oprávnění řízení přístupu na základě Role (RBAC) ve vaší virtuální síti pro službu Azure Logic Apps. Tato úloha vyžaduje, abyste přiřadili **Přispěvatel sítě** a **Přispěvatel modelu Classic** role ve službě Azure Logic Apps.
Nastavení těchto oprávnění naleznete v tématu [připojit k virtuálním sítím Azure z aplikací logiky](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#vnet-access).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Účty pro integraci s ISE

Můžete použít účty pro integraci s logic apps uvnitř prostředí integrační služby (ISE). Ale musí používat tyto účty pro integraci *stejné ISE* jako propojené logic apps. Aplikace logiky do ISE může odkazovat pouze tyto účty pro integraci, které jsou ve stejném ISE. Při vytváření účtu pro integraci, můžete vybrat vaše ISE jako umístění pro váš účet integrace.

## <a name="get-support"></a>Získat podporu

* Pokud máte dotazy, navštivte <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">fórum Azure Logic Apps</a>.
* Pokud chcete zanechat své nápady na funkce nebo hlasovat, navštivte <a href="https://aka.ms/logicapps-wish" target="_blank">web zpětné vazby od uživatelů Logic Apps</a>.

## <a name="next-steps"></a>Další postup

* Zjistěte, jak [připojit k virtuálním sítím Azure z izolované logic apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Další informace o [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Další informace o [integrace služby virtual network pro služby Azure](../virtual-network/virtual-network-for-azure-services.md)
