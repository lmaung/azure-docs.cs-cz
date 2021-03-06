---
title: Používání služby Azure AD Connect Health se službou AD FS | Dokumentace Microsoftu
description: Toto je stránka o službě Azure AD Connect Health, která popisuje postup monitorování místní infrastruktury služby AD FS.
services: active-directory
documentationcenter: ''
ms.reviewer: zhiweiwangmsft
author: billmath
manager: daveba
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70fb463b1ac8664838404a7dfcd0380da8f3358d
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889490"
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Sledování služby AD FS pomocí služby Azure AD Connect Health
Následující dokumentace se věnuje sledování infrastruktury služby AD FS ve službě Azure AD Connect Health. Informace o sledování služby Azure AD Connect (Sync) pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health pro synchronizaci](how-to-connect-health-sync.md). Informace o sledování služby Active Directory Domain Services pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD DS](how-to-connect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Upozornění služby AD FS
Část pojednávající o upozorněních služby Azure AD Connect Health uvádí seznam aktivních upozornění. Každé upozornění obsahuje důležité informace, postup řešení a odkazy na související dokumentaci.

Dvojitým kliknutím na aktivní nebo vyřešené upozornění můžete otevřít nové okno s doplňujícími informacemi, kroky, které můžete k vyřešení upozornění použít, a odkazy na relevantní dokumentaci. Můžete si zobrazit i historické údaje o dříve vyřešených upozorněních.

![Portál služby Azure AD Connect Health](./media/how-to-connect-health-adfs/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Funkce analýzy využití služby AD FS
Funkce analýzy využití služby Azure AD Connect Health analyzuje ověřovací provoz na federačních serverech. Dvojitým kliknutím na políčko funkce analýzy využití můžete otevřít okno analýzy využití, ve kterém je zobrazeno několik metrik a seskupení.

> [!NOTE]
> Pokud chcete použít funkci analýzy využití ve službě AD FS, povolte auditování AD FS. Další informace najdete v článku o [povolení auditování služby AD FS](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Portál služby Azure AD Connect Health](./media/how-to-connect-health-adfs/report1.png)

Pokud chcete vybrat další metriky, určit časový rozsah nebo změnit seskupení, klikněte pravým tlačítkem na graf analýzy využití a vyberte Upravit graf. Potom můžete zadat časový rozsah, vybrat jiné metriky a změnit seskupení. Distribuci ověřovacího provozu můžete zobrazit podle různých „metrik“ a jednotlivé metriky můžete seskupit pomocí příslušných parametrů možnosti „Seskupit podle“, které jsou uvedené v následující části:

**Metrika: Celkový počet požadavků** – celkový počet požadavků zpracovaných servery AD FS.

|Seskupit podle | Co seskupení znamená a proč je užitečné? |
| --- | --- |
| Vše | Zobrazí celkový počet požadavků zpracovaných všemi servery AD FS.|
| Aplikace | Seskupí celkový počet požadavků podle cílové přijímající strany. Toto seskupení vás seznámí s procentem celkového provozu, které jednotlivé aplikace přijímají. |
|  Server |Seskupí celkový počet požadavků podle serveru, který požadavek zpracoval. Toto seskupení vás seznámí s distribucí zatížení celkového provozu.
| Připojení k pracovišti |Seskupí celkový počet požadavků podle toho, jestli přicházejí ze zařízení, která jsou připojená k pracovišti (známá). Toto seskupení vás seznámí s tím, jestli se k vašim prostředkům přistupuje pomocí zařízení, které infrastruktura identity nezná. |
|  Metoda ověřování | Seskupí celkový počet požadavků podle metody ověřování, která se k ověřování používá. Toto seskupení vás seznámí s běžnou metodu ověřování, která se k ověřování používá. Níže jsou uvedené možné metody ověřování. <ol> <li>integrované ověřování systému Windows (Windows)</li> <li>ověřování pomocí formulářů (formuláře)</li> <li>jednotné přihlašování (SSO)</li> <li>ověření certifikátem X509 (certifikát)</li> <br>Pokud federační servery požadavek přijmou pomocí souboru cookie jednotného přihlašování, příslušný požadavek se počítá jako jednotné přihlašování (SSO). V takových případech (pokud je soubor cookie platný) se od uživatele nevyžadují přihlašovací údaje a uživatel získá bezproblémový přístup k aplikaci. Toto chování je běžné v případě, kdy máte několik přijímajících stran, které jsou chráněné federačními servery. |
| Umístění v síti | Seskupí celkový počet požadavků podle umístění uživatele v síti. Může to být intranet nebo extranet. Toto seskupení vás seznámí s procentuálním podílem provozu z intranetu vzhledem k provozu z extranetu. |


**Metrika: Celkový počet neúspěšných požadavků** – celkový počet neúspěšných požadavků zpracovaných službou FS. (Tato metrika je dostupná pouze ve službě AD FS pro Windows Server 2012 R2.)

|Seskupit podle | Co seskupení znamená a proč je užitečné? |
| --- | --- |
| Typ chyby | Zobrazí počet chyb podle předdefinovaných typů chyb. Toto seskupení vás seznámí s běžnými typy chyb. <ul><li>Nesprávné uživatelské jméno nebo heslo: Chyby vzniklé v důsledku nesprávné uživatelské jméno nebo heslo.</li> <li>"Uzamčení extranetu": Chyby způsobené požadavky přijatými od uživatele, který byl uzamčený přístup do extranetu </li><li> "Prošlé heslo": Chyby způsobené uživateli, kteří se přihlašují pomocí vypršet platnost jeho hesla.</li><li>"Zakázaný účet": Chyby způsobené uživateli, protokolování pomocí deaktivovaného účtu.</li><li>"Ověřování zařízení": Chyby způsobené uživateli, kteří neprovádějí ověřování pomocí ověření zařízení.</li><li>"Ověřování certifikátu uživatele": Chyby způsobené uživateli, kterým nefunguje ověřování kvůli neplatnému certifikátu.</li><li>"MFA": Chyby způsobené uživateli, kteří neprovádějí ověřování pomocí služby Multi-Factor Authentication.</li><li>"Jiné přihlašovací údaje": "Autorizace vystavení": Chyby způsobené selháním autorizace.</li><li>"Delegování vystavení": Chyby způsobené chybami delegace vystavení.</li><li>"Přijetí tokenu": Chyby způsobené Služba ADFS odmítla token od zprostředkovatele Identity třetí strany.</li><li>"Protokol": Chyba způsobená chybami protokolu.</li><li>"Neznámý": Zachytit vše. Jakékoli jiné chyby, které se nehodí do definovaných kategorií.</li> |
| Server | Seskupí chyby podle serveru. Toto seskupení vás seznámí s distribucí chyb mezi servery. Nerovnoměrná distribuce může naznačovat vadný stav serveru. |
| Umístění v síti | Seskupí chyby podle umístění požadavků v síti (intranet vs. extranet). Toto seskupení vás seznámí s typy neúspěšných požadavků. |
|  Aplikace | Seskupí chyby podle cílové aplikace (přijímající strany). Toto seskupení vás seznámí s tím, která cílová aplikace zaznamenává největší počet chyb. |

**Metrika: Počet uživatelů** – průměrný počet jedinečných uživatelů aktivně ověřujících pomocí AD FS

|Seskupit podle | Co seskupení znamená a proč je užitečné? |
| --- | --- |
|Vše |Tato metrika poskytuje průměrný počet uživatelů, kteří používají službu FS ve vybraném časovém intervalu. Uživatelé nejsou seskupení. <br>Průměr závisí na vybraném časovém intervalu. |
| Aplikace |Seskupí průměrný počet uživatelů podle cílové aplikace (přijímající strany). Toto seskupení vás seznámí s počtem uživatelů používajících jednotlivé aplikace. |

## <a name="performance-monitoring-for-ad-fs"></a>Sledování výkonu služby AD FS
Sledování výkonu služby Azure AD Connect Health poskytuje sledovací informace o metrikách. Po zaškrtnutí políčka sledování se otevře nové okno s podrobnými informacemi o metrikách.

![Portál služby Azure AD Connect Health](./media/how-to-connect-health-adfs/perf1.png)

Výběrem možnosti Filtrovat (v horní části okna) můžete filtrovat podle serveru a prohlédnout si metriky na jednotlivých serverech. Pokud chcete změnit metriky, klikněte pravým tlačítkem na graf sledování pod oknem sledování a vyberte Upravit graf (nebo vyberte tlačítko Upravit graf). V nově otevřeném okně můžete vybrat další metriky pomocí rozevíracího seznamu a také zadat časový rozsah pro zobrazení dat výkonu.

## <a name="top-50-users-with-failed-usernamepassword-logins"></a>Nejčastějších 50 uživatelů s neúspěšným přihlášením kvůli uživatelskému jména nebo heslu
Jednou z běžných příčin neúspěšného požadavku na ověření na serveru AD FS je požadavek provedený s neplatnými přihlašovacími údaji, tedy s nesprávným uživatelským jménem nebo heslem. Do této situace se uživatelé zpravidla dostávají v důsledku používání složitých hesel, zapomenutí hesel nebo překlepů.

Ale existují další důvody, které může mít za následek neočekávaný počet požadavků zpracovávaných servery AD FS, jako například: Aplikace, která uloží uživatelské údaje a přihlašovací údaje vypršení platnosti nebo pokus o přihlášení k účtu pomocí řady známých hesel uživatel se zlými úmysly. Tyto dva příklady jsou legitimními důvody, které by mohly vést k prudkému nárůstu množství požadavků.

Azure AD Connect Health pro ADFS poskytuje sestavy s nejčastějšími 50 uživateli, kteří se neúspěšně přihlašovali pomocí neplatného uživatelského jména nebo hesla. Tuto sestavu můžete vytvořit zpracováním událostí auditu, které generují všechny servery AD FS ve farmách.

![Portál služby Azure AD Connect Health](./media/how-to-connect-health-adfs/report1a.png)

V rámci této sestavy máte snadný přístup k následujícím informacím:

* Celkový počet neúspěšných požadavků s nesprávným uživatelským jménem nebo heslem za posledních 30 dní.
* Průměrný počet uživatelů, kteří mají problém s přihlašováním kvůli chybnému uživatelskému jménu nebo heslu, za jeden den.

Kliknutím na tuto část přejdete do hlavního okna sestavy, které vám nabídne další podrobnosti. Toto okno obsahuje graf informace o trendech, které vám usnadní vytváření směrného plánu pro požadavky s nesprávným uživatelským jménem nebo heslem. Kromě toho obsahuje seznam 50 uživatelů s nejvyšším počtem neúspěšných pokusů za poslední týden. Všimněte si, že těchto 50 uživatelů z minulého týdne může pomoct s identifikací špiček zadání špatného hesla.  

Graf obsahuje následující informace:

* Celkový počet neúspěšných přihlášení z důvodu chybného uživatelského jména nebo hesla na denní bázi.
* Celkový počet jedinečných uživatelů s neúspěšným přihlášení na denní bázi.
* IP adresa klienta posledního požadavku

![Portál služby Azure AD Connect Health](./media/how-to-connect-health-adfs/report3a.png)

Sestava obsahuje následující informace:

| Položky sestavy | Popis |
| --- | --- |
| ID uživatele |Zobrazuje použité ID uživatele. Tato hodnota odpovídá hodnotě zadané uživatelem, což je v některých případech nesprávné ID uživatele, které bylo použito. |
| Neúspěšné pokusy |Zobrazuje celkový počet neúspěšných pokusů s konkrétním ID uživatele. Tabulka je řazená podle největšího počtu neúspěšných pokusů v sestupném pořadí. |
| Poslední chyba |Zobrazuje časové razítko výskytu poslední chyby. |
| IP adresa poslední chyby |Zobrazuje IP adresu klienta z posledního neúspěšného požadavku. Pokud se v této hodnotě zobrazí více než jedna IP adresa, může zahrnovat IP adresu klienta přesměrování i IP adresu požadavku z posledního pokusu uživatele.  |

> [!NOTE]
> Sestava se každých 12 hodin automaticky aktualizuje novými informacemi, které se za tu dobu shromáždily. V důsledku tohoto postupu nemusí být v sestavě zahrnuté pokusy o přihlášení za posledních 12 hodin.

## <a name="related-links"></a>Související odkazy
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Instalace agenta služby Azure AD Connect Health](how-to-connect-health-agent-install.md)
* [Sestavě rizikových IP adres ](how-to-connect-health-adfs-risky-ip.md)

