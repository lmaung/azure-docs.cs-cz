---
title: Rozšířené zabezpečení dat – Azure SQL Database | Dokumentace Microsoftu
description: Další informace o funkcích pro zjišťování a klasifikace citlivá data, Správa ohrožení zabezpečení vaší databáze a detekuje neobvyklé aktivity, které může znamenat hrozbu pro vaše databáze Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: 388a1a2ff70f4d00e66511811bffdb76806ed713
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735680"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Pokročilé data zabezpečení pro službu Azure SQL Database

Pokročilé datové zabezpečení je jednotný balíček pro pokročilé funkce zabezpečení SQL. Zahrnuje funkce pro zjišťování a klasifikace citlivá data, zpřístupnění a Zklidňující potenciální ohrožení zabezpečení databáze a detekuje neobvyklé aktivity, které může znamenat hrozbu pro vaše databáze. Poskytuje centrální místo pro povolování a správu těchto možností.

## <a name="overview"></a>Přehled

Zabezpečení dat pokročilé (reklamy) poskytuje sadu pokročilé funkce zabezpečení SQL, včetně dat zjišťování a klasifikace, posouzení ohrožení zabezpečení a detekce hrozeb.

- [Zjišťování a klasifikace dat](sql-database-data-discovery-and-classification.md) (aktuálně ve verzi preview) poskytuje do Azure SQL Database integrované možnosti pro zjišťování, klasifikaci, označování popisky a ochranu citlivých dat ve vašich databázích. Může sloužit k poskytování přehledu o stavu klasifikace databáze a ke sledování přístupu k citlivým datům v databázi i mimo ni.
- [Posouzení ohrožení zabezpečení](sql-vulnerability-assessment.md) je snadno konfigurovat službu, můžete zjistit, sledovat a umožňují náprava potenciálních ohrožení zabezpečení databáze. Poskytuje přehled o stavu zabezpečení a zahrnuje praktické kroky k vyřešení problémů se zabezpečením a zlepšení ochrany databáze.
- [Detekce hrozeb](sql-database-threat-detection-overview.md) detekuje neobvyklé aktivity a potenciálně nebezpečné pokusy o přístup nebo zneužití vaší databáze. Nepřetržitě monitoruje podezřelé aktivity v databázi a okamžitě poskytuje výstrahy zabezpečení týkající se potenciálních ohrožení zabezpečení, útoků prostřednictvím injektáže SQL a neobvyklých vzorů přístupu k databázi. Výstrahy detekce hrozeb zadejte podrobnosti o podezřelé aktivitě a doporučení akce k prošetření a zmírnění hrozby.

REKLAMY SQL povolte, jakmile povolíte všechny z nich zahrnuty funkce. Jedním kliknutím můžete povolit služby Active Directory pro všechny databáze na serveru služby SQL Database nebo spravované instance. Povolení nebo správu nastavení služby Active Directory vyžaduje, který patří do [správce zabezpečení SQL](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) role, role správce databáze SQL nebo role správce systému SQL server. 

Ceny služby Active Directory v souladu s Azure Security Center úrovně standard, ve kterém každý chráněný server SQL Database nebo spravované instance se počítá jako jeden uzel. Nově chráněných prostředků nárok na bezplatnou zkušební verzi služby Security Center úrovně standard. Další informace najdete v tématu [stránce s cenami za Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Začínáme s reklamy

Následující kroky vám pomůžou začít s reklamy.

## <a name="1-enable-ads"></a>1. Povolení služby Active Directory

Povolit REKLAMU tak, že přejdete do **rozšířené zabezpečení dat** pod **zabezpečení** záhlaví pro váš server SQL Database nebo spravovaných instancí. Pro všechny databáze na serveru databáze nebo spravované instance, klikněte na tlačítko Povolit služby Active Directory **povolit pokročilé zabezpečení dat na serveru**.

![Povolení služby Active Directory](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> Náklady na služby Active Directory je v souladu s Azure Security Center úrovně standard ceny podle počtu uzlů, kde je uzel celý server SQL Database nebo spravované instance. Proto platíte jen jednou pro ochranu všech databází na serveru databáze nebo spravované instance pomocí služby Active Directory. Můžete vyzkoušet reklamy zpočátku s bezplatnou zkušební verzi.

## <a name="2-configure-vulnerability-assessment"></a>2. Konfigurovat posouzení ohrožení zabezpečení

Pokud chcete začít používat sken posouzení ohrožení zabezpečení, musíte nakonfigurovat účet úložiště, kde jsou uloženy výsledky kontroly. Uděláte to tak, klikněte na kartu posouzení ohrožení zabezpečení.

![Konfigurovat posouzení ohrožení zabezpečení](./media/sql-advanced-protection/configure_va.png) 

Vyberte nebo vytvořte účet úložiště pro ukládání výsledků skenování. Můžete také zapnout pravidelné opakované kontroly konfigurace posouzení ohrožení zabezpečení pro spuštění automatického vyhledávání, jednou za týden. Souhrn výsledků kontroly se pošle e-mailové adresy, které zadáte.

![Nastavení posouzení ohrožení zabezpečení](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Spustit klasifikaci dat, sledování chyb zabezpečení a vyšetřování upozornění na hrozby

Klikněte na tlačítko **zjišťování a klasifikace dat** karty zobrazíte doporučené citlivé sloupce ke klasifikaci a klasifikaci dat pomocí popisků trvalé citlivosti. Klikněte na tlačítko **sken posouzení ohrožení zabezpečení** kartu k zobrazení a správa prověřování ohrožení zabezpečení a sestav a sledování vašich zásadní roli zabezpečení. Pokud byly přijaty výstrahy zabezpečení, klikněte na tlačítko **detekce hrozeb** kartu k zobrazení podrobností výstrah a zobrazíte souhrnnou zprávu na všechny výstrahy ve vašem předplatném Azure prostřednictvím stránky s upozorněními zabezpečení Azure Security Center.

## <a name="4-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>4. Spravovat nastavení služby Active Directory na serveru služby SQL Database nebo spravované instance

Pokud chcete zobrazit a spravovat nastavení služby Active Directory, přejděte na **rozšířené zabezpečení dat** pod **zabezpečení** záhlaví pro váš server SQL Database nebo spravované instance. Na této stránce můžete povolit nebo zakázat reklamy a upravovat hrozeb nastavení detekce pro celý server SQL Database nebo spravované instance.

![Nastavení serveru](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-ads-settings-for-a-sql-database"></a>5. Správa nastavení služby Active Directory pro SQL database

Chcete-li přepsat nastavení služby Active Directory pro určitou databázi, zkontrolujte **povolit pokročilé zabezpečení dat na úrovni databáze** zaškrtávací políčko. Tuto možnost použijte jenom v případě, že nemáte konkrétní požadavek pro příjem samostatné hrozeb výstrahy detekce pro jednotlivé databáze, místo nebo kromě výstrahy přijaté pro všechny databáze na serveru databáze nebo spravované instance. 

Jakmile je políčko zaškrtnuté, klikněte na tlačítko **nastavení detekce hrozeb pro tuto databázi** a potom nakonfigurovat relevantní nastavení pro tuto databázi.

![Nastavení detekce hrozeb a databáze](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Pokročilé datové nastavení zabezpečení pro databázový server nebo spravované instance můžete také dosažitelná z podokna databáze služby Active Directory. Klikněte na tlačítko **nastavení** v hlavním podokně reklamy a pak klikněte na tlačítko **nastavení zobrazit pokročilé zabezpečení dat serveru**. 

![Nastavení databáze](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Další postup 

- Další informace o [zjišťování a klasifikace dat](sql-database-data-discovery-and-classification.md) 
- Další informace o [sken posouzení ohrožení zabezpečení](sql-vulnerability-assessment.md) 
- Další informace o [detekce hrozeb](sql-database-threat-detection.md)
- Další informace o [Azure security center](https://docs.microsoft.com/azure/security-center/security-center-intro)
