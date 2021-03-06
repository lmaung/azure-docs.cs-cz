---
title: Azure a nabídky na webu Marketplace AppSource | Dokumentace Microsoftu
description: Vytváření a správa nabídek na Azure a tržišť s řešeními AppSource
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: pbutlerm
ms.openlocfilehash: f13d49fde7f0e40f6dcb026fcb20cb11c028c64b
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100877"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Azure a nabídky na webu Marketplace AppSource

Této první části této části se zavádí celkovém provozu použít k vytvoření a správa nabídek pro Azure a tržišť s řešeními AppSource.  Tato část obsahuje, je třeba porozumět ke správě určité typy nabídek na pozadí, jakož i technické informace, které jsou společné pro všechny typy nabízejí.  Většina Tato část obsahuje podrobné pokyny o tom, jak vytvářet a spravovat určité typy nabídek.  

Následující video představuje různé možnosti a typy různé nabídky dostupné v Azure Marketplace nebo AppSource.  Věnuje se také důležité technické a obchodní aspektů publikování aplikace nebo služby, v těchto tržišť s řešeními.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Vytváření aplikací a služeb pro Azure Marketplace a AppSource – sestavení 2018**

Další informace o těchto tržišť s řešeními najdete v tématu [Průvodce publikováním webu Azure Marketplace a AppSource](../marketplace-publishers-guide.md).


## <a name="common-offer-operations"></a>Běžné operace nabídky

Procesem vytvoření nové nabídky se značně liší mezi typy nabídek, například mezi [nabídku aplikací Azure](./azure-applications/cpp-azure-app-offer.md) a [konzultační nabídky](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md).  Naproti tomu spoustu dalších operací je provést v rámci nabídky v [portál partnerů cloudu](https://cloudpartner.azure.com) jsou poměrně standardizované mezi typy nabídek.  Tyto běžné operace – včetně publikovat, zobrazení stavu, update a delete, jsou popsané v části [správa nabídek](./manage-offers/cpp-manage-offers.md)


## <a name="test-drive"></a>Testovací verze

*Vyzkoušejte si* je marketplace funkce, která poskytuje zákazníkům možnost ukázku "před zakoupením vyzkoušet" pro každou nabídku, proto je povoleno.  Testovací verze funkce je omezená na následující dílčí sadu typy nabídek: [Aplikace Azure](./azure-applications/cpp-azure-app-offer.md), [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md), [Dynamics 365 for Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md), [Dynamics 365 pro Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md), [ Aplikace SaaS](./saas-app/cpp-saas-offer.md), a [virtuálních počítačů](./virtual-machine/cpp-virtual-machine-offer.md).  Tato funkce vyžaduje publisher k vytvoření šablony Test Drive, přizpůsobit jeho nabídky.  Další informace najdete v části [Test Drive](../cloud-partner-portal-orig/what-is-test-drive.md).

Můžete procházet existující nabídky marketplace, které mají testovací verze ukázky použitím [test jednotky filtr](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=test-drive). 


## <a name="azure-marketplace-and-appsource-offer-types"></a>Azure Marketplace a AppSource nabízí typy

Následující tabulka uvádí aktuální nabídka typů podporovaných [portál partnerů cloudu](https://cloudpartner.azure.com).  Pro každý typ nabídky uvádí marketplace(s), kde je možné uvést nabídky, stejně jako obecný popis technologie nabídky řešení.

|                Typ nabídky                |  Marketplace  |   Popis                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Aplikace Azure](./azure-applications/cpp-azure-app-offer.md) | Azure | Řešení se skládá z jednoho nebo více virtuálních počítačů (VM), volitelné vlastního kódu v Azure, nasazení pomocí šablony Azure Resource Manageru.  Nasazení může být buď zákazník šablonu řešení nebo spravuje vydavatel. Tento typ se používá k zajištění více flexibility než typ nabídky zadaný virtuální počítač.  |
| [Konzultační služby](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | Obojí | Poradci Microsoftu kvalifikovaný můžete zobrazit seznam svých služeb specifického pro doménu v Azure Marketplace nebo AppSource.  Své znalosti pomáhá zákazníkům vyhodnocení jejich problémy a vytváření a nasazování správné řešení pro splnění svých obchodních cílů.  |
| [Kontejner](./containers/cpp-containers-offer.md)  | Azure | Řešení je zřízená jako nějakou službu založenou na Kubernetes nebo Azure Container instances image kontejneru Dockeru. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Balíček, který rozšiřuje toto plánování podnikových zdrojů (ERP) a systém správy firmy. |
| [Dynamics 365 for Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Balíček, který rozšiřuje tohoto zákazníka systému pro správu (CRM) zdroje, prodej, služby, služba projektu a modulů služby pole.  |
| [Dynamics 365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Balíček, který rozšiřuje tento podnikových zdrojů (ERP) služba, která podporuje rozšířené plánování finance, operations, výroba a správa dodavatelského řetězce. |
| [Modul IoT Edge](./iot-edge-module/cpp-offer-process-parts.md) | Azure | Kompatibilní s Dockerem kontejner, který běží na zařízení IoT Edge.  Obsahuje malé výpočetní modulů, které používají kombinace vlastního kódu, dalších služeb Azure a služby 3. stran. |
| [Aplikace Power BI](./power-bi/cpp-power-bi-offer.md) | AppSource | Balíček, který používá toky dat pro připojení k datům ve společném úložišti dat sestavy a řídicí panely. |
| [Aplikace SaaS](./saas-app/cpp-saas-offer.md) | Azure | Řešení je software-as-a-service předplatné, spravuje vydavatele, kteří uživatelé připojují prostřednictvím vlastní rozhraní, které využívá službu Azure Active Directory. |
| [Virtuální počítač](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Řešení je obsažena v jednom virtuálním počítači nasadit do předplatného zákazníka.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Další informace najdete v tématu [podle typu nabídky Průvodce publikováním](../publisher-guide-by-offer-type.md).


## <a name="next-steps"></a>Další postup

Se dozvíte o celkovém provozu můžete provádět na nabídky na webu marketplace a jejich společné technické atributy a prostředky v tomto tématu [správa nabídek](./manage-offers/cpp-manage-offers.md).
