---
title: Azure AD Connect Health – data služby Health service není datum upozornění | Dokumentace Microsoftu
description: Tento dokument popisuje příčinu výstrahy "data služby Health service není aktuální." a jak řešit potíže se.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: SamuelD
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ad829b976d8b712ee8027c89fb618c6c07de1bc
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429006"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Data služby Health service není aktuální výstrahy

## <a name="overview"></a>Přehled
Agenty na místní počítače, které Azure AD Connect Health monitoruje pravidelně odesílá data do služby Azure AD Connect Health. Pokud služba nepřijímá data z agenta, bude zastaralé informace uvedené na portálu. Abyste měli na očích problém, se vyvolat službu **data služby Health service není aktuální** upozornění. Tím se vygeneruje, když služba neobdržel data v posledních dvou hodin.  

* **Upozornění** výstraha o stavu je vyvoláno, pokud Connect Health přijme prvky částečná data ze serveru odesílají dvě hodiny. Stav upozornění neaktivuje e-mailová oznámení pro správce tenanta.
* **Chyba** stav upozornění je vyvoláno, pokud Connect Health přijme všechny datové prvky odeslaných ze serveru pro dvě hodiny. Chyba stavu aktivuje upozornění e-mailová oznámení správce tenanta.


## <a name="troubleshooting-steps"></a>Postup při řešení potíží 

> [!IMPORTANT] 
> Toto oznámení následuje Connect Health [zásady uchovávání dat](reference-connect-health-user-privacy.md#data-retention-policy)

* Ujistěte se, že na počítači běží služby agenti Azure AD Connect Health. Connect Health pro AD FS by měl mít například tři služby.  
  ![Ověření Azure AD Connect Health](./media/how-to-connect-health-agent-install/install5.png)

* Ujistěte se, že se přenášejí prostřednictvím a vyhovět [části věnované požadavkům](how-to-connect-health-agent-install.md#requirements).
* Použití [nástroj pro testování připojení](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) ke zjišťování problémů s připojením.
* Pokud máte proxy server HTTP, postupujte podle těchto [kroky konfigurace](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 

Okno podrobností výstrahy uvádí chybějící data elementy ze serveru. V následující tabulce vám pomohou eliminovat další potíže. 
### <a name="connect-health-for-sync"></a>Connect Health pro synchronizaci

| Datových prvků | Postup při řešení potíží |
| --- | --- | 
| PerfCounter | - [Odchozí připojení ke koncovému bodu služby Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Kontrola protokolu SSL pro odchozí provoz je filtrovaná nebo zakázaná](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Porty brány firewall na serveru se spuštěným agentem](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
| AadSyncService SynchronizationRules, <br /> AadSyncService-Connectors, <br /> AadSyncService GlobalConfigurations, <br /> AadSyncService-RunProfileResults, <br /> AadSyncService ServiceConfigurations, <br /> AadSyncService-ServiceStatus | - [Odchozí připojení ke koncovému bodu služby Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Porty brány firewall na serveru se spuštěným agentem](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>Connect Health pro AD FS

Další kroky k ověření pro službu AD FS a pracovní postup v [nápovědy k AD FS](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

| Datových prvků | Postup při řešení potíží |
| --- | --- | 
| PerfCounter, TestResult | - [Odchozí připojení ke koncovému bodu služby Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Kontrola protokolu SSL pro odchozí provoz je filtrovaná nebo zakázaná](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Porty brány firewall na serveru se spuštěným agentem](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
|  Adfs-UsageMetrics | Odchozí připojení na základě IP adres najdete [rozsahy IP adres Azure](https://www.microsoft.com/download/details.aspx?id=41653) | 

### <a name="connect-health-for-adds"></a>Connect Health pro AD DS

| Datových prvků | Postup při řešení potíží |
| --- | --- | 
| PerfCounter, Adds-TopologyInfo-Json, Common-TestData-Json | - [Odchozí připojení ke koncovému bodu služby Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Porty brány firewall na serveru se spuštěným agentem](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |


## <a name="next-steps"></a>Další postup
* [Azure AD Connect Health – nejčastější dotazy](reference-connect-health-faq.md)
