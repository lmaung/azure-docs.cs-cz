---
title: Monitorování stavu SQL Database pomocí Azure Resource Health | Dokumentace Microsoftu
description: Pomocí Azure Resource Health umožňuje monitorovat stav SQL Database, pomáhá diagnostikovat a získáním podpory v případě, že problém Azure ovlivňuje vaše prostředky SQL.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: c3b9fecd3ad404385732e55a9cf3aa65a6e388b8
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889371"
---
# <a name="use-resource-health-to-troubleshoot-connectivity-for-azure-sql-database"></a>Řešení potíží s připojením pro službu Azure SQL Database pomocí Resource Health

## <a name="overview"></a>Přehled

[Služba Resource Health](../service-health/resource-health-overview.md#getting-started) pro SQL Database pomáhá při diagnostice a získání podpory v případě, že problém Azure ovlivňuje vaše prostředky SQL. Informuje o aktuálním a dřívějším stavu prostředků a pomáhá zmírnit problémy. Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.

![Přehled](./media/sql-database-resource-health/sql-resource-health-overview.jpg)

## <a name="health-checks"></a>Kontroly stavu

Služba Resource Health Určuje stav prostředek SQL tím, že kontroluje úspěšné a neúspěšné přihlášení k prostředku. Stav prostředků pro prostředek databáze SQL v současné době zkontroluje pouze neúspěšná přihlášení z důvodu chyby systému a není chyba uživatele. Stav Resource Health je aktualizován každé 1-2 minuty.

## <a name="health-states"></a>Stavy

### <a name="available"></a>K dispozici.

Stav **dostupné** znamená, že Resource Health nezjistila selhání přihlášení z důvodu chyby systému pro váš prostředek SQL.

![K dispozici.](./media/sql-database-resource-health/sql-resource-health-available.jpg)

### <a name="degraded"></a>Sníženo

Stav **Degradovaný** znamená, že služba Resource Health zjistila většinu úspěšných přihlášení, ale také několik selhání. Toto jsou pravděpodobně přechodné přihlášení chyby. Pokud chcete snížit dopad problémům způsobeným přihlášení přechodné chyby, implementujte [Logika opakování](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) ve vašem kódu.

![Sníženo](./media/sql-database-resource-health/sql-resource-health-degraded.jpg)

### <a name="unavailable"></a>Není dostupný

Stav **není k dispozici** znamená, že Resource Health zjistila pokusů o přihlášení konzistentní vzhledem k aplikacím na prostředek SQL. Pokud váš prostředek zůstane v tomto stavu delší dobu, kontaktujte prosím podporu.

![Není dostupný](./media/sql-database-resource-health/sql-resource-health-unavailable.jpg)

### <a name="unknown"></a>Neznámé

Stav **neznámý** označuje, že Resource Health neobdržel informace o tomto zdroji pro více než 10 minut. Přestože tento stav není úplným a rozhodujícím údaj o stavu prostředku, je důležitý datový bod v procesu odstraňování potíží. Pokud prostředek běží podle očekávání, stav prostředku se změní na dostupný za pár minut. Pokud dojde k problémům s prostředkem, neznámý stav může naznačovat, že událost platformy ovlivňuje prostředku.

![Neznámé](./media/sql-database-resource-health/sql-resource-health-unknown.jpg)

## <a name="historical-information"></a>Historické informace

Dostanete až 14 dní historie stavu v části historie stavu Resource Health. V části, bude obsahovat také z důvodu výpadek (Pokud je k dispozici) pro výpadky hlášených Resource Health. Azure v současné době ukazuje výpadek pro prostředek databáze SQL ve dvouminutových intervalech. Skutečná délka výpadku je pravděpodobně méně než minuty – průměrná je 8s.

### <a name="downtime-reasons"></a>Z důvodů výpadek

Když vaši službu SQL Database dojde k výpadku, provedení analýzy určit důvod. Pokud je k dispozici, v části historie stavu Resource Health hlásí z důvodu výpadek. Důvody výpadků se obvykle publikují 30 minut po události.

#### <a name="planned-maintenance"></a>Plánovaná údržba

Infrastruktura Azure pravidelně provede plánovanou údržbu – upgrade hardwarové nebo softwarové komponenty v datovém centru. Zatímco databáze při údržbě, SQL může ukončit některé existující připojení a odmítnout nové značky. Neúspěšná přihlášení během plánované údržby jsou nejčastěji přechodné a [Logika opakování](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) pomáhá snižovat dopad. Pokud budete nadále dochází k chybám přihlášení, kontaktujte prosím podporu.

#### <a name="reconfiguration"></a>Rekonfigurace

Rekonfigurací se považují za přechodné podmínky. proto se očekává, že čas od času. Tyto události mohou být spouštěny zavádění vyrovnávání nebo softwaru a hardwaru. Všechny klientské produkční aplikace, která se připojuje k databázi cloudu by měly implementovat robustní připojení [Logika opakování](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors), protože by zmírnit tyto situace a by měly obecně průhledného chyby pro koncového uživatele.

## <a name="next-steps"></a>Další postup

- Další informace o [Logika opakování pro přechodné chyby](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors)
- [Oprava, diagnostika a prevence chyb připojení SQL](./sql-database-connectivity-issues.md)
- Další informace o [konfigurace výstrah Resource Health](../service-health/resource-health-alert-arm-template-guide.md)
- Získejte přehled o [Resource Health](../service-health/resource-health-overview.md)
- [Služba Resource Health – nejčastější dotazy](../service-health/resource-health-faq.md)
