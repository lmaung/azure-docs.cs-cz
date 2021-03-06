---
title: Nabídka služeb ve službě Azure Stack | Dokumentace Microsoftu
description: Jako operátor cloudu můžete nabízet služby pro vaše uživatele.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.lastreviewed: 09/17/2018
ms.openlocfilehash: 4deb72eae7dffac6eabb34b18a9e879ac1fd8113
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56179956"
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Přehled nabízených služeb ve službě Azure Stack

*Platí pro: Azure Stack integrované systémy a Azure Stack Development Kit*

[Microsoft Azure Stack](azure-stack-poc.md) je hybridní Cloudová platforma, která vám umožní doručovat služby z vašeho datového centra. Jako poskytovatel služeb můžou nabízet služby vašim klientům. V rámci firmy nebo státním úřadu může nabídnout místních služeb na svoje zaměstnance. 

Můžete nabídnout [infrastruktura jako služba](https://azure.microsoft.com/overview/what-is-iaas/) služby (IaaS), které umožňují uživatelům vytvářet výpočetní infrastrukturu na vyžádání, zřídit a spravovat na portálu user portal Azure Stack.

Můžete taky nasadit [platforma jako služba](https://azure.microsoft.com/overview/what-is-paas/) služby (PaaS) pro Azure Stack od Microsoftu a dalších poskytovatelů 3. stran. Služby, které můžete doručovat patří, ale nejsou omezené na:

- [Přidání poskytovatele prostředků App Service do Azure Stacku](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-overview)

- [Přidat poskytovatele prostředků SQL serveru do služby Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-sql-resource-provider-deploy)

- [Přidat poskytovatele prostředků MySQL serveru do služby Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-mysql-resource-provider-deploy)


Můžete dokonce kombinovat služby integrovat a vytvářet komplexní řešení pro různé uživatele.

Chcete-li tyto služby poskytovat vašim uživatelům, musíte nejdřív vytvořit [plánů, nabídek a kvót](azure-stack-plan-offer-quota-overview.md). Uživatelé můžou potom přihlásit k odběru nabídek, aby mohli využívat služby.

## <a name="plan-your-service-offers"></a>Plánování vaší nabídky služby

Když máte v plánu vaší nabídky, berte v úvahu následující body:

**Nabídky zkušebních verzí**: Nabídky zkušebních verzí můžete použít jak zaujmout nové uživatele, kteří potom můžete upgradovat na další služby. Vytvořit nabídku na zkušební verzi, vytvořte malé [základní plán](azure-stack-plan-offer-quota-overview.md#base-plan) s volitelné větší doplňkový plán.

**Plánování kapacity**: Můžete mít obavy o uživatelích, které vzít velké množství prostředků a zdokonaleným systému pro všechny uživatele. Chcete-li vám pomůže zvýšit výkon, můžete [nakonfigurovat své plány s kvótami](azure-stack-plan-offer-quota-overview.md#plans) limit využití.

**Delegované poskytovatele**: Můžete udělit jiným, které nabízí možnost vytvořit ve vašem prostředí. Například pokud jste poskytovatel služeb, můžete [delegovat](azure-stack-delegated-provider.md) tato schopnost váš prodejce. Nebo, pokud už organizace, můžete delegovat na jiné oddělení a pobočky.

## <a name="next-steps"></a>Další postup

[Vytvoření nabídky ve službě Azure Stack](azure-stack-create-offer.md)
