---
title: Zabezpečení dat ve službě Azure Security Center | Dokumentace Microsoftu
description: Tento dokument popisuje způsob správy a ochrany dat ve službě Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2018
ms.author: rkarlin
ms.openlocfilehash: af3cc229482021fe6d5e5c988bc98afe6f7f97ce
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114157"
---
# <a name="azure-security-center-data-security"></a>Zabezpečení dat ve službě Azure Security Center
Služba Azure Security Center pomáhá zákazníkům předcházet hrozbám, detekovat je a reagovat na ně tím, že shromažďuje a zpracovává data související se zabezpečením, včetně informací o konfiguraci, metadat, protokolů událostí, souborů se stavem systému a dalších dat. Společnost Microsoft dodržuje přísné pokyny pro dodržování předpisů a zabezpečení – od psaní kódu po provoz služeb.

Tento článek popisuje způsob správy a ochrany dat ve službě Azure Security Center.

## <a name="data-sources"></a>Zdroje dat
Azure Security Center analyzuje data z následujících zdrojů a poskytuje přehled o stavu vašeho zabezpečení, zjišťuje ohrožení zabezpečení a doporučuje způsoby zmírnění rizik a detekuje aktivní hrozby:

- Služby Azure: Na základě informací o konfiguraci služeb Azure, které máte nasazené, tím, že komunikuje s poskytovatelem prostředků pro danou službu.
- Síťový provoz: Využívá Vzorkovaná metadata síťového provozu z infrastruktury společnosti Microsoft, například zdrojovou a cílovou adresu IP/port, velikost paketu nebo síťový protokol.
- Partnerská řešení: Využívá výstrahy zabezpečení z integrovaných partnerských řešení, jako jsou brány firewall a antimalwarová řešení.
- Virtuální počítače a servery: Využívá informace o konfiguraci a informace o událostech zabezpečení, jako jsou protokoly událostí a auditů Windows, protokoly služby IIS, zprávy syslog a soubory se stavem systému z vašich virtuálních počítačů. Při vytvoření výstrahy Azure Security Center může navíc vygenerovat snímek příslušného disku virtuálního počítače a z tohoto disku extrahovat artefakty související s příslušnou výstrahou (jako je třeba soubor registru) pro účely forenzní analýzy.


## <a name="data-protection"></a>Ochrana dat
**Oddělení dat**: Data se ukládají logicky oddělená pro jednotlivé komponenty v rámci služby. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu celého životního cyklu dat a je vyžadováno na každé úrovni služby.

**Přístup k datům**: Aby bylo možné poskytovat doporučení týkající se zabezpečení a prošetřovat potenciální ohrožení zabezpečení, mají pracovníci společnosti Microsoft přístup k informacím shromažďovaným nebo analyzovaným službami Azure, včetně souborů se stavem systému, události vytváření procesů, snímků disku virtuálního počítače a artefakty, zahrnující mohou neúmyslně zákaznických dat ani osobní data z vašich virtuálních počítačů. Dodržujeme [Podmínky online služeb společnosti Microsoft a Prohlášení o zásadách ochrany osobních údajů](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), ve kterých je uvedeno, že společnost Microsoft nebude informace o zákaznících používat ani z nich odvozovat další informace pro reklamní nebo podobné obchodní účely. Informace o zákaznících podle potřeby používáme pouze k poskytování služeb Azure a k účelům slučitelným s poskytováním těchto služeb. Všechna práva na informace o zákaznících zůstávají ve vašem vlastnictví.

**Za využívání dat**: Společnost Microsoft používá vzory a analýzy hrozeb napříč několika klienty k vylepšení našich schopnosti prevence a detekce; uděláme v souladu se závazky ochrany osobních údajů je popsáno v našem [prohlášení o zásadách](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

## <a name="data-location"></a>Umístění dat

**Vaše pracovní prostory**: Pracovní prostor je určen pro následující Geografie a data shromážděná z vašich virtuálních počítačů Azure, včetně výpisy stavu systému a některé typy dat výstrah, se ukládají v nejbližším pracovním prostoru.

| Geografie virtuálního počítače                        | Geografie pracovního prostoru |
|-------------------------------|---------------|
| Spojené státy, Brazílie, Kanada | Spojené státy |
| Evropa, Spojené království        | Evropa        |
| Asie a Tichomoří, Japonsko, Indie    | Asie a Tichomoří  |
| Austrálie                     | Austrálie     |


Snímky disků virtuálních počítačů se ukládají ve stejném účtu úložiště jako disk virtuálního počítače.

Pro virtuální počítače a servery spuštěné v jiných prostředích, například místně, můžete zadat pracovní prostor a oblast, kde se shromážděná data ukládají.

**Azure Security Center Storage**: Informace o výstrahách zabezpečení, včetně partnerských výstrah, se ukládají místně v závislosti na umístění souvisejícího prostředku Azure, kdežto informace o stavu zabezpečení a doporučení se ukládají centrálně ve Spojených státech nebo Evropa závislosti na umístění zákazníka.
Azure Security Center shromažďuje dočasné kopie souborů se stavem systému a analyzuje je za účelem detekce stop pokusů o napadení zabezpečení, neúspěšných i úspěšných. Azure Security Center provádí tuto analýzu v rámci stejné geografie jako pracovní prostor a po dokončení analýzy tyto dočasné kopie odstraní.

Artefakty počítačů se ukládají centrálně ve stejné oblasti jako virtuální počítač.


## <a name="managing-data-collection-from-virtual-machines"></a>Správa shromažďování dat z virtuálních počítačů

Když povolíte službu Security Center v Azure, u každého vašeho předplatného Azure se zapne funkce shromažďování dat. Shromažďování dat pro předplatná můžete zapnout také v části Zásady zabezpečení služby Azure Security Center. Když je funkce shromažďování dat zapnutá, služba Azure Security Center zřídí ve všech stávajících i nově vytvořených podporovaných virtuálních počítačích agenta Microsoft Monitoring Agent.
Microsoft Monitoring Agent prohledává různé konfigurace týkající se zabezpečení a zapisuje události do složek [Trasování událostí pro Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Operační systém bude kromě toho během chodu počítače shromažďovat události protokolu událostí. Mezi příklady těchto údajů patří: typ a verze operačního systému, protokoly operačního systému (protokoly událostí systému Windows), spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID klienta. Microsoft Monitoring Agent načte položky protokolu událostí a trasování ETW a zkopíruje je do vašich pracovní prostorů za účelem analýzy. Microsoft Monitoring Agent do vašich pracovních prostorů zkopíruje také soubory se stavem systému, povolí události vytváření procesů a povolí auditování příkazového řádku.

Pokud používáte Azure Security Center úrovně Free, můžete pomocí zásad zabezpečení také zakázat shromažďování dat z virtuálních počítačů. Pro předplatná na úrovni Standard se shromažďování dat požaduje. Shromažďování artefaktů a snímků disku virtuálního počítače bude nadále povolené i v případě, že shromažďování dat je zakázané.

## <a name="data-consumption"></a>Využití dat

Zákazníci můžou využívat data související se službou Security Center z různých datových proudů, jak je znázorněno níže:

* **Aktivity Azure:** všechny výstrahy zabezpečení, schválené žádosti o přístup [právě včas](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) ke službě Security Center a všechny výstrahy vygenerované [adaptivními ovládacími prvky aplikace](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application).
* **Log Analytics:** všechny výstrahy zabezpečení.


> [!NOTE]
> Doporučení zabezpečení je možné využívat také prostřednictvím rozhraní REST API. Další informace najdete v [referenčních materiálech k rozhraní REST API poskytovatele prostředků zabezpečení](https://msdn.microsoft.com/library/mt704034(Azure.100).aspx).

## <a name="see-also"></a>Další informace najdete v tématech
V tomto dokumentu jste se dozvěděli informace o způsobu správy a ochrany ve službě Azure Security Center. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md) – Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby
* [Blog o zabezpečení Azure](https://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů
