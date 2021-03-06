---
title: Zabezpečení Azure a dodržování předpisů – PaaS webové aplikace pro FedRAMP
description: Zabezpečení Azure a dodržování předpisů – PaaS webové aplikace pro FedRAMP
services: security
author: jomolesk
ms.assetid: 41570fc1-4d74-48ed-afb0-ef1be857029e
ms.service: security
ms.topic: article
ms.date: 06/01/2018
ms.author: jomolesk
ms.openlocfilehash: 9bfd748e6b10106587e3fce64e15d7f1f6d50e59
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/14/2019
ms.locfileid: "54261583"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-fedramp"></a>Zabezpečení Azure a dodržování předpisů: PaaS webové aplikace pro FedRAMP

## <a name="overview"></a>Přehled

[Federal Risk and Authorization Management Program (FedRAMP)](https://www.fedramp.gov/) je program státní instituce USA, který přináší standardizovaný přístup k posuzování zabezpečení, autorizaci a nepřetržitému monitorování pro cloud produkty a služby. Toto zabezpečení Azure a dodržování předpisů poskytuje pokyny, jak doručovat platformě Microsoft Azure jako služba (PaaS) architektura, která pomáhá implementovat podmnožinu FedRAMP High ovládací prvky. Toto řešení obsahuje pokyny k nasazení a konfiguraci prostředků Azure běžné referenční architekturu, představením způsobů, ve které zákazníci mohou pokrýt konkrétní požadavky na zabezpečení a dodržování předpisů a slouží jako základ pro zákazníky vytvořit a konfigurovat jejich vlastní řešení v Azure.

Tato referenční architektura, příručky k implementaci přidružený ovládací prvek a modely hrozeb mají sloužit jako základ pro zákazníky, chcete-li upravit jejich konkrétním požadavkům a nepoužívali-je v produkčním prostředí. Nasazení aplikace do tohoto prostředí bez jakýchkoli úprav je dostatečná k zcela požadavkům FedRAMP High standardních hodnot. Je třeba počítat s následujícím:
- Tato architektura poskytuje standardní hodnoty pomáhá zákazníkům nasadit úlohy o velikosti do Azure v podobě odpovídajícího FedRAMP.
- Zákazníci odpovídají za provedení zabezpečení a hodnocení dodržování předpisů řešení vytvořené pomocí této architektury, jako požadavky se mohou lišit podle konkrétní implementace jednotlivých zákazníků.

## <a name="architecture-diagram-and-components"></a>Diagram architektury a komponenty
Toto řešení poskytuje referenční architektura pro webové aplikace PaaS s back-endu Azure SQL Database. Webové aplikace je hostována v izolované Azure App Service Environment, což je privátním a vyhrazeném prostředí v datovém centru Azure. Prostředí rozloží provoz pro webovou aplikaci ve virtuálních počítačích spravovaných na Azure. Tato architektura také zahrnuje skupiny zabezpečení sítě, Application Gateway, Azure DNS a nástroje pro vyrovnávání zatížení. Kromě toho Azure Monitor poskytuje analýzy v reálném čase o stavu systému. **Azure doporučuje nakonfigurovat připojení VPN nebo ExpressRoute pro správu a data importovat do podsítě referenční architektury.**

![PaaS webové aplikace pro FedRAMP diagram referenční architektury](images/fedramp-paaswa-architecture.png?raw=true "PaaS webové aplikace pro FedRAMP diagram referenční architektury")

Toto řešení používá následující služby Azure. Podrobnosti o architektura nasazení jsou umístěné v [architektura nasazení](#deployment-architecture) oddílu.

- Azure Active Directory
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) web Application Firewall
        - Režim brány firewall: ochrany před únikem informací
        - Sada pravidel: OWASP 3.0
        - Naslouchacího procesu: port 443
- Virtuální síť Azure
- Skupiny zabezpečení sítě
- Azure DNS
- Azure Storage
- Azure Monitor
- App Service Environment v2
- Nástroj pro vyrovnávání zatížení Azure
- Webové aplikace Azure
- Azure Resource Manager

## <a name="deployment-architecture"></a>Architektura nasazení
Následující část podrobně popisuje nasazení a implementaci prvky.

**Azure Resource Manager**: [Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) umožňuje zákazníkům pracovat s prostředky v řešení jako se skupinou. Zákazníci mohou nasadit, aktualizovat nebo odstranit všechny prostředky pro řešení v rámci jediné koordinované operace. Zákazníci pomocí šablony pro nasazení a tato šablona může fungovat v různých prostředích, jako je například testovacím, přípravném nebo produkčním prostředí. Resource Manager poskytuje zabezpečení, auditování a označování příznaky funkcí, které pomáhá zákazníkům spravovat svoje prostředky po nasazení.

**App Service Environment v2**: [Azure App Service Environment (ASE)](https://docs.microsoft.com/azure/app-service/environment/intro) je funkce služby App Service, která poskytuje plně izolované a vyhrazené prostředí pro bezpečné spouštění aplikací služby App Service ve velkém měřítku.

Služby ase jsou izolované, aby se spustily jenom aplikace jednoho zákazníka a vždycky jsou nasazené do virtuální sítě. Zákazníci mají podrobnou kontrolu nad síťovými přenosy v obou vstupní a výstupní aplikace a aplikace můžete vytvářet vysokorychlostní zabezpečená připojení prostřednictvím virtuálních sítí a místním podnikovým prostředkům.

Použití služby ase pro tuto architekturu jsou povolené pro následující ovládací prvky/konfigurace:

- Hostování v zabezpečené virtuální síti Azure a pravidel zabezpečení sítě
- Služba ASE nakonfigurovaný s ILB certifikát podepsaný svým držitelem pro komunikaci přes protokol HTTPS
- [Interní režim vyrovnávání zatížení](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Zakázat [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Změna [šifer TLS](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Ovládací prvek [příchozí provoz N/W porty](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Firewall webových aplikací – omezení dat](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Povolit [provoz Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

[Pokyny a doporučení](#guidance-and-recommendations) oddíl obsahuje další informace o služby ase.

**Webové aplikace Azure**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) umožňuje zákazníkům vytvářet a hostovat webové aplikace v programovacím jazyce podle svého výběru bez nutnosti spravovat infrastrukturu. Nabízí automatické škálování a vysokou dostupnost, podporuje systémy Windows a Linux a umožňuje automatizované nasazení z Githubu, Azure DevOps nebo libovolného úložiště Git.

### <a name="virtual-network"></a>Virtual Network
Architektura definuje privátní virtuální síť s adresním prostorem 10.200.0.0/16.

**Skupiny zabezpečení sítě**: [Skupiny zabezpečení sítě (Nsg)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) obsahují seznamy řízení přístupu, které povolují nebo zakazují provoz v rámci virtuální sítě. Skupiny zabezpečení sítě slouží k zabezpečení provozu na úrovni jednotlivých virtuálních počítačů nebo podsítě. Existují následující skupiny zabezpečení sítě:
- 1 skupiny zabezpečení sítě pro službu Application Gateway
- 1 skupina NSG pro App Service Environment
- 1 skupiny zabezpečení sítě pro službu Azure SQL Database

Skupiny zabezpečení sítě mají konkrétní porty a protokoly, otevřete řešení mohli pracovat zabezpečeně a správně. Kromě toho jsou povoleny následující konfigurace pro jednotlivé skupiny NSG:
  - [Diagnostické protokoly a události](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) jsou povolené a uložená v účtu úložiště
  - Log Analytics je připojen k [skupin zabezpečení sítě a Diagnostika](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Podsítě**: Každá podsíť je přidružený jeho odpovídající skupiny zabezpečení sítě.

**Azure DNS**: Domain Name System nebo DNS, je zodpovědný za převod (nebo řešení) názvu webu nebo služby k její IP adrese. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) je hostitelská služba určená pro domény DNS, která nabízí řešení názvů pomocí infrastruktury Azure. Hostování domény v Azure, uživatelé spravovat záznamy DNS pomocí stejných přihlašovacích údajů, rozhraní API, nástrojů a fakturačních podmínek jako u ostatních služeb Azure. Azure DNS podporuje také privátní DNS domény.

**Azure Load Balancer**: [Nástroj Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) umožňuje zákazníkům škálovat svoje aplikace a poskytovat vysokou dostupnost služeb. Nástroj pro vyrovnávání zatížení podporuje další scénáře využití příchozí i odchozí a poskytuje nízkou latenci a vysokou propustnost a škálování až na úrovni milionů toků pro všechny aplikace TCP a UDP.

### <a name="data-in-transit"></a>Přenášená data
Azure šifruje veškerá komunikace do a z datových center Azure ve výchozím nastavení. Všechny transakce do služby Azure Storage na webu Azure portal nastat prostřednictvím protokolu HTTPS.

### <a name="data-at-rest"></a>Neaktivní uložená data
Architektura chrání data při nečinnosti pomocí šifrování, auditování databáze a jiné míry.

**Azure Storage**: Pro splnění šifrovaná data na požadavky rest, všechny [služby Azure Storage](https://azure.microsoft.com/services/storage/) používá [šifrování služby Storage](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) využívá funkci BitLocker systému Windows, zajišťuje šifrování pro datové disky. Řešení se integruje se službou Azure Key Vault a pomáhá řídit a spravovat klíče pro šifrování disku.

**Azure SQL Database**: Instance Azure SQL Database používá následující bezpečnostní opatření databáze:
-   [Ověřování a autorizace AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) umožňuje správu identit uživatelů databáze a další služby Microsoftu v jednom centrálním místě.
-   [Auditování služby SQL database](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) sleduje události databáze a zapisuje je do auditu protokolu v účtu služby Azure storage.
-   Azure SQL Database je nakonfigurován na použití [transparentní šifrování dat (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), který provádí v reálném čase šifrování a dešifrování databáze, přidružené zálohy a soubory transakčních protokolů k ochraně informací v rozhraní rest.
-   [Pravidla brány firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) brání veškerému přístupu k databázové servery, dokud se správná oprávnění. Brána firewall uděluje přístup k databázím v závislosti na zdrojové IP adrese každého požadavku.
-   [Detekce hrozeb SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) umožňuje detekce a reakce na potenciální hrozby, jakmile nastanou díky poskytování upozornění zabezpečení pro podezřelých databázových aktivitách, potenciálních ohroženích zabezpečení, útoky prostřednictvím injektáže SQL a neobvyklých databázových přístup vzory.
-   [Vždy šifrované sloupce](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Ujistěte se, že citlivá data nezobrazí jako prostý text v databázi systému. Po povolení šifrování dat, můžou jenom klientských aplikací nebo aplikačních serverů s přístupu ke klíčům k datům ve formátu prostého textu.
- [Zabezpečení na úrovní řádků](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) umožňuje uživatelům definovat zásady pro omezení přístupu k datům a ukončí zpracování.
- [Maskování dynamických dat SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) lze provést po referenční architektura nasazuje. Zákazníci, bude nutné upravit nastavení dodržovat svoje schéma databáze maskování dynamických dat.

### <a name="identity-management"></a>Správa identit
Tyto technologie nabízejí identity možnosti správy v prostředí Azure:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) je služba Microsoftu pro více tenantů založené na cloudu adresáře a identity management. Všechny uživatele tohoto řešení se vytvoří ve službě AAD, včetně uživatele, kteří používají Azure SQL Database.
-   Do aplikace ověřování pomocí AAD. Další informace najdete v tématu [integrace aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Kromě toho šifrování sloupců databáze používá Azure Active Directory k ověření aplikace ke službě Azure SQL Database. Další informace najdete v tématu Jak [chránit citlivá data ve službě Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Řízení přístupu na základě rolí Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) umožňuje přesněji zaměřené access management pro službu Azure. Přístup k předplatnému je omezená na správce předplatného a přístup k prostředkům lze omezit na základě role uživatele.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) umožňuje zákazníkům minimalizovat počet uživatelů, kteří mají přístup k určité informace.  Mohou správci AAD Privileged Identity Management zjišťovat, omezovat a monitorovat privilegované identity a jejich přístup k prostředkům. Tato funkce slouží taky vynutit na vyžádání, just-in-time pro správu v případě potřeby přístup.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) zjistí potenciální ohrožení zabezpečení dopadem na identity vaší organizace, nakonfiguruje automatické odpovědi na zjištěné podezřelé akce související s identity ve vaší organizaci a prověří podezřelé incidenty na přijmout vhodná opatření k jejich řešení.

### <a name="security"></a>Zabezpečení
**Správa tajných kódů** toto řešení využívá [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) pro správu klíčů a tajných kódů. Azure Key Vault pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami. Následující funkce služby Azure Key Vault zákazníkům pomáhat s ochranou dat a přístupu na tyto údaje:
- Pokročilé zásady přístupu jsou nakonfigurované na základě potřeba.
- Zásady přístupu trezoru klíčů jsou definovány minimální požadovaná oprávnění pro klíče a tajné kódy.
- Všechny klíče a tajné kódy ve službě Key Vault mít data vypršení platnosti.
- Všechny klíče ve službě Key Vault jsou chráněné moduly specializované hardwarového zabezpečení (HSM). Typ klíče je HSM chráněné 2048bitový klíč RSA.
- Minimální požadovaná oprávnění pomocí řízení přístupu na základě rolí jsou udělena všechny uživatele a identity.
- Jsou povolené diagnostické protokoly pro Key Vault s dobu uchování o délce alespoň 365 dnů.
- Povolené kryptografických operací s klíči jsou omezené na ty povinné.

**Služba Application Gateway** architekturu snižuje riziko ohrožení zabezpečení pomocí služby Application Gateway Firewall webových aplikací a pravidel OWASP povolena. Další možnosti patří:
- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Povolit [přesměrování zpracování SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Zakázat [TLS verze 1.0 a verze 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Firewall webových aplikací](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
- [Režim ochrany před únikem informací](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) pomocí sady pravidel OWASP 3.0
- Povolit [protokolování diagnostiky](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Vlastních testů stavu paměti](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) a [Azure Advisoru](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) poskytují dodatečnou ochranu a oznámení. Azure Security Center také poskytuje systém pověst.

### <a name="logging-and-auditing"></a>Protokolování a auditování
Platforma Azure Monitor poskytuje podrobné protokolování systémových a uživatelských aktivit a také stav systému. Shromažďuje a analyzuje data vygenerovaná prostředky v Azure a místním prostředí.
- **Protokoly aktivit**: [Protokoly aktivit](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) poskytují přehled o operace prováděné s prostředky v rámci předplatného. Protokoly aktivit, vám pomůže určit operace iniciátoru čas výskytu a stav.
- **Diagnostické protokoly**: [Diagnostické protokoly](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) zahrnout všechny protokoly, protože ho vygeneroval každý prostředek. Tyto protokoly patří protokoly událostí systému Windows, protokoly služby Azure Storage, protokoly auditu služby Key Vault a protokolů Application Gateway přístup a brány firewall.
- **Archivace protokolu**: Všechny diagnostické protokoly zápis do účtu centralizovaný a šifrovaného úložiště Azure pro archivaci. Uchovávání je uživatelem konfigurovatelné, až do 730 dnů pro splnění požadavků na uchovávání specifické pro organizaci. Tyto protokoly se připojit ke službě Azure Log Analytics pro zpracování, ukládání a vytváření sestav na řídicím.

Kromě toho jsou součástí této architektury řešení následující monitorování:
-   [Služby Active directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Kontrola stavu služby Active Directory řešení posuzuje rizika a stav prostředí serveru v pravidelných intervalech a poskytuje uspořádaný seznam doporučení, které jsou specifické pro nasazenou serverové infrastruktury.
-   [Posouzení antimalwaru](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): Antimalwarové řešení hlásí stav malwaru, hrozeb a ochranu.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Řešení služby Azure Automation ukládá, spouští a spravuje sady runbook. V tomto řešení sady runbook pomůže shromažďovat protokoly z Application Insights a Azure SQL Database.
-   [Zabezpečení a Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Řídicí panel zabezpečení a Audit poskytuje podrobný pohled na stav zabezpečení prostředků tím, že poskytuje metriky na zabezpečení domény, významné problémy, detekce, analýza hrozeb a běžné dotazy na zabezpečení.
-   [Posouzení SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): Řešení SQL Health Check posuzuje v pravidelných intervalech rizika a stav prostředí serveru a poskytuje zákazníkům s uspořádaný seznam doporučení, které jsou specifické pro nasazenou serverové infrastruktury.
-   [Správa aktualizací](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): Řešení Update Management umožňuje správu zákazníka aktualizace zabezpečení operačního systému, včetně proces instalace požadovaných aktualizací a stav dostupných aktualizací.
-   [Stav agenta](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Řešení Agent Health sestavy, kolik agentů je nasazených a zeměpisné rozšíření, jakož i kolik agentů, které jsou reagovat a počet agentů, které jsou odesílání provozních dat.
-   [Protokoly aktivit Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Řešení Activity Log Analytics pomáhá s analýzou protokolů aktivit Azure napříč všemi předplatnými Azure zákazníka.
-   [Sledování změn](https://docs.microsoft.com/azure/automation/automation-change-tracking): Řešení Change Tracking umožňuje zákazníkům snadno identifikovat změny v prostředí.

**Azure Monitor**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) pomáhá uživatelům sledování výkonu, zabezpečení a rozpoznávání trendů tím, že umožňuje organizacím auditovat, vytvářet upozornění a archivovat data, včetně sledování volání rozhraní API v prostředcích zákazníků Azure.

## <a name="threat-model"></a>Model hrozeb

Diagram toku dat pro tuto referenční architekturu je k dispozici pro [Stáhnout](https://aka.ms/fedrampPaaSWebAppDFD) nebo najdete níže. Tento model může pomoci zákazníkům pochopit body potenciální riziko v infrastruktuře systému při provádění změn.

![Webová aplikace PaaS pro model hrozeb FedRAMP](images/fedramp-paaswa-threat-model.png?raw=true "PaaS webové aplikace pro FedRAMP model hrozeb")

## <a name="compliance-documentation"></a>Dokumentace ke službě dodržování předpisů
[Azure zabezpečení a dodržování předpisů – FedRAMP vysokou zákazníka odpovědnost matice](https://aka.ms/blueprinthighcrm) vypisuje všechny ovládací prvky zabezpečení vyžadované FedRAMP High směrného plánu. Matice označuje, zda provádění každý ovládací prvek zodpovídá za Microsoft, Zákazník, nebo sdílet mezi nimi.

[Azure zabezpečení a dodržování předpisů – FedRAMP PaaS WebApp vysokou ovládací prvek implementace matice](https://aka.ms/fedrampPaaSWebAppCIM) vypisuje všechny ovládací prvky zabezpečení vyžadované FedRAMP High směrného plánu. Matice obsahuje informace, na které se vztahují ovládacích prvků PaaS architektura pro webové aplikace, včetně podrobného popisu způsobu implementace splňuje požadavky každého zahrnutého ovládacího prvku.

## <a name="guidance-and-recommendations"></a>Pokyny a doporučení
### <a name="vpn-and-expressroute"></a>Připojení VPN a ExpressRoute
Zabezpečené tunelové propojení sítě VPN nebo [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) musí být nakonfigurovaný pro zabezpečené připojení k prostředky nasazené jako součást této referenční architektury PaaS webové aplikace. Odpovídajícím způsobem nastavíte síť VPN nebo ExpressRoute, můžete zákazníky přidat vrstvu ochrany dat během přenosu.

Implementací zabezpečeného tunelového propojení sítě VPN s využitím Azure můžete vytvořit připojení VPN mezi místní sítí a virtuální sítí Azure. Toto připojení probíhá přes Internet a umožňuje zákazníkům bezpečně "tunelového propojení" informací uvnitř šifrované propojení mezi sítí zákazníka a Azure. Site-to-Site VPN je zabezpečené, Vyspělá technologie, která byla nasazena podniky všech velikostí desítky let. [Režimu tunelového propojení IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) se používá při použití této možnosti jako mechanismus pro šifrování.

Protože provoz v rámci tunelového připojení sítě VPN přes Internet prostřednictvím sítě site-to-site VPN, společnost Microsoft nabízí možnost jiný, ještě větší zabezpečení připojení. Azure ExpressRoute je vyhrazené sítě WAN propojení mezi Azure a místním umístěním nebo poskytovatele hostingu serveru Exchange. Jak připojení ExpressRoute se nepřenášejí prostřednictvím Internetu, tato připojení nabízí další spolehlivost, vyšší rychlost, nižší latenci a lepší zabezpečení než Typická připojení přes Internet. Navíc vzhledem k tomu, že toto je přímé připojení poskytovatele telekomunikačních zákazníka, data nejsou prochází přes Internet a proto není vystavený ho.

Osvědčené postupy pro implementaci zabezpečenou hybridní síť, která rozšiřuje místní síť do Azure jsou [dostupné](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Právní omezení

 - Tento dokument slouží pouze k informačním účelům. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÝCH, ODVOZENÝCH NEBO ZÁKONNÝCH, INFORMACE V TOMTO DOKUMENTU. Tento dokument se poskytuje "jako-je." Informace a názory vyjádřené v tomto dokumentu včetně adres URL a jiných odkazů na internetové weby, mohou změnit bez předchozího upozornění. Tento dokument, zákazníci nese riziko jeho použití.
 - Tento dokument neposkytuje žádná zákonná práva na duševní vlastnictví v libovolném produkt společnosti Microsoft nebo řešení zákazníkům.
 - Zákazníci mohou kopírovat a používat tento dokument pro interní referenční účely.
 - Některá doporučení v tomto dokumentu může vést k vyšší dat, sítě nebo výpočetní využití prostředků v Azure a můžou zvýšit náklady Azure licenci předplatného zákazníka.
 - Tato architektura má sloužit jako základ pro zákazníky, chcete-li upravit jejich konkrétním požadavkům by se neměl používat jako-je v produkčním prostředí.
 - Tento dokument je mimo jiné vyvinuto odkaz a neměl by se definovat všechny prostředky, které zákazník dokáže splnit požadavky na konkrétní dodržování předpisů a nařízení. Zákazníci by měl hledání právní podporu – od organizace na implementace schválené zákazníků.
