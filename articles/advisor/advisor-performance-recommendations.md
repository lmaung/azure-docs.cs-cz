---
title: Zvýšení výkonu aplikace na platformě Azure s využitím Azure Advisoru | Dokumentace Microsoftu
description: Použijte Advisor za účelem optimalizace výkonu všech vašich nasazení Azure.
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.author: kasparks
ms.openlocfilehash: f3c6e39203fb0d864ecf952e0468959d66931e1f
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/31/2019
ms.locfileid: "55491575"
---
# <a name="improve-performance-of-azure-applications-with-azure-advisor"></a>Zvýšení výkonu aplikace na platformě Azure s využitím Azure Advisoru

Doporučení Azure Advisoru výkonu zvýšit rychlost a rychlost odezvy důležitých aplikací. Doporučení k výkonu služby Advisor můžete získat **výkonu** karty řídicí panel služby Advisor.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>DNS doba TTL na svůj profil Traffic Manageru k převzetí služeb při selhání v dobrém stavu koncových bodů rychleji

[Čas nastavení Live (TTL)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) ve vašem profilu Traffic Manageru umožňuje určit, jak rychle přepnout koncové body, pokud daný koncový bod reagovat na dotazy. Snížení hodnoty TTL znamená, že klienti se budou směrovat do funkční koncových bodů rychlejší.

Azure Advisor určí profily Traffic Manageru s delší TTL nakonfigurovaný a doporučuje konfigurace hodnoty TTL na 20 sekund nebo 60 sekund, podle toho, jestli profil, který je nakonfigurovaný pro [rychlé převzetí služeb při selhání](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>Zvyšte výkon databáze pomocí SQL DB Advisoru.

Advisor vám poskytuje konzistentní vzhledem k aplikacím, konsolidované zobrazení doporučení pro všechny vaše prostředky Azure. Integruje se službou SQL Database Advisor a přináší vám doporučení pro vylepšení výkonu vaší databáze SQL Azure. SQL Database Advisor vyhodnocuje výkon vašich databází SQL Azure díky analýze historii využití. Potom nabízí doporučení, která jsou nejvhodnější pro spuštění typické zatížení databáze.

> [!NOTE]
> Pokud chcete získat doporučení, musí mít databázi o týden využití a v daném týdnu musí být některé aktivity konzistentní vzhledem k aplikacím. SQL Database Advisor můžete optimalizovat snadněji pro vzory dotazů konzistentní než pro náhodné nárůstům aktivity.

Další informace o službě SQL Database Advisor najdete v tématu [služby SQL Database Advisor](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-app-service-performance-and-reliability"></a>Zlepšení výkonu služby App Service a spolehlivosti

Azure Advisor se integruje se doporučené postupy pro zlepšení prostředí služby App Services a zajištění příslušné možnosti. Příklady doporučení App Services:
* Zjišťování instancí, ve kterém vyčerpání paměti nebo prostředků procesoru podle moduly runtime aplikace s možnostmi omezení rizik.
* Zjišťování instancí, kde tyto zdroje, jako jsou webové aplikace a databáze můžete zvýšit výkon a nižší náklady.

Další informace o doporučeních App Services najdete v tématu [osvědčené postupy pro službu Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Chcete-li zabránit omezování disku vstupně-výstupní operace použít Managed Disks

Advisor bude identifikovat virtuální počítače, které patří k účtu úložiště, která dosahuje svého cíle škálovatelnosti. Tato podmínka díky ty virtuální počítače náchylný k omezování vstupně-výstupních operací. Advisor vám doporučí, aby se zabránilo snížení výkonu použít Managed Disks.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>Zvýšit výkon a spolehlivost disků virtuálního počítače pomocí služby Premium Storage

Advisor identifikuje virtuální počítače se standardní disky, které mají k velkému počtu transakcí na vašem účtu úložiště a doporučuje upgrade na disky premium. 

Azure Premium Storage poskytuje podporu vysoce výkonných disků s nízkou latencí pro virtuální počítače, na kterých běží I intenzivních vstupně-výstupních operací. Disky virtuálních počítačů, které používají účty služby premium storage ukládat data na jednotky SSD (Solid-State Drive). Pro zajištění nejlepšího výkonu pro vaši aplikaci doporučujeme vám, že můžete migrovat všechny disky virtuálního počítače, které vyžadují vysokou vstupně-výstupních operací na premium storage.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Odeberte na tabulku SQL data warehouse ke zvýšení výkonu dotazů Nerovnoměrná distribuce dat.

Nerovnoměrná distribuce dat může způsobit nepotřebná data pohybu nebo prostředků problémová místa při spuštění vaší úlohy. Poradce zjistí data distribuce zkosení delší než 15 % a doporučujeme znovu distribuovat data a návštěvě klíče výběr distribuce tabulky. Další informace o identifikaci a odebírání zkosení najdete v tématu [řešení potíží s zkosení](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Vytvořit nebo aktualizovat statistiky zastaralé tabulky na tabulku SQL data warehouse ke zvýšení výkonu dotazů

Advisor identifikuje tabulky, které nemají aktuální [Statistika tabulky](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) a doporučuje se vytváří nebo aktualizuje statistika tabulky. Dotaz, že Optimalizátor používá aktuální statik k odhadu kardinality nebo počet řádků ve výsledku dotazu, který umožňuje optimalizátoru dotazů, chcete-li vytvořit plán dotazu vysoce kvalitní nejrychlejší výkon služby SQL data warehouse.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Vertikálně navýšit kapacitu k optimalizaci využití mezipaměti v tabulkách SQL Data Warehouse ke zvýšení výkonu dotazů

Azure Advisor zjišťuje, pokud má vaše služba SQL Data Warehouse vysokou mezipaměti použít procento a nízké procento úspěšnosti. Tento stav indikuje vyřazení vysokou mezipaměti, což může ovlivnit výkon služby SQL Data Warehouse. Advisor navrhuje, vertikálně navýšit kapacitu služby SQL Data Warehouse k zajištění, že přidělíte dostatek kapacity mezipaměti pro vaše úlohy.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>Převést tabulek SQL Data Warehouse na replikované tabulky pro zvýšení výkonu dotazů

Advisor určí tabulek, které nejsou replikované tabulky, ale je výhodná převodu a naznačuje, že převod těchto tabulek. Doporučení jsou založená na replikovanou tabulku velikost, počet sloupců, typ distribuce tabulky a počet oddílů v tabulce SQL Data Warehouse. Další heuristické metody mohou být poskytovány v doporučení pro kontext. Další informace o tom, jak toto doporučení závisí, naleznete v tématu [doporučení pro SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables). 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Migrace účtu úložiště do Azure Resource Manageru a mějte všechny nejnovější funkce Azure

Migrace vašeho účtu úložiště modelu nasazení do Azure Resource Manageru (Resource Manager) výhod nasazení šablon, další možnosti zabezpečení a možnosti upgradu na účet GPv2 pro využití nejnovějších funkcích Azure Storage. Advisor bude identifikovat všechny samostatné účty, které používají model nasazení Classic a doporučuje migrace do modelu nasazení Resource Manager.

> [!NOTE]
> Klasických upozornění ve službě Azure Monitor jsou naplánovány k vyřazení z provozu v červnu 2019. Doporučujeme vám upgradovat svůj účet klasického úložiště. pro použití Resource Manageru pro zachování výstrah funkce s novou platformu. Další informace najdete v tématu [klasického upozornění vyřazení](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Jak získat přístup k doporučení k výkonu v Advisoru

1. Přihlaste se k [webu Azure portal](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).

2.  Na řídicím panelu služby Advisor, klikněte na tlačítko **výkonu** kartu.

## <a name="next-steps"></a>Další postup

Další informace o doporučení Advisoru, naleznete v tématu:

* [Úvod do služby Advisor](advisor-overview.md)
* [Začínáme se službou Advisor](advisor-get-started.md)
* [Náklady na doporučení advisoru](advisor-performance-recommendations.md)
* [Poradce doporučení k vysoké dostupnosti](advisor-high-availability-recommendations.md)
* [Bezpečnostní doporučení advisoru](advisor-security-recommendations.md)
