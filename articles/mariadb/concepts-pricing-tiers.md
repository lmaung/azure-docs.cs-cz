---
title: Cenové úrovně pro službu Azure Database pro MariaDB
description: Tento článek popisuje cenové úrovně pro službu Azure Database pro MariaDB.
author: jan-eng
ms.author: janeng
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: bb6e27f92f60712cce71ba6fca53b40af00ee714
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354446"
---
# <a name="azure-database-for-mariadb-pricing-tiers"></a>Azure Database pro MariaDB cenové úrovně

Vytvoření Azure Database pro MariaDB server v jednom ze tří různých cenových úrovní: Basic, pro obecné účely a paměťově optimalizovaná. Cenové úrovně se liší podle množství výpočetních prostředků ve virtuálních jader, které je možné zřídit paměti na vCore a technologie úložiště používají k ukládání dat. Všechny prostředky jsou zřízené na úrovni serveru MariaDB. Server může mít jeden nebo více databází.

|    | **Basic** | **Obecné účely** | **Optimalizované z hlediska paměti** |
|:---|:----------|:--------------------|:---------------------|
| Generace výpočetních funkcí | Gen 5 |Gen 5 | Gen 5 |
| Virtuální jádra | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| Paměť na vCore | 2 GB | 5 GB | 10 GB |
| Velikost úložiště | 5 GB až 1 TB | 5 GB až 4 TB | 5 GB až 4 TB |
| Typ úložiště | Azure Storage úrovně Standard | Azure Premium Storage | Azure Premium Storage |
| Období uchovávání záloh databáze | 7 až 35 dnů | 7 až 35 dnů | 7 až 35 dnů |

Zvolte cenovou úroveň, použijte jako výchozí bod v následující tabulce.

| Cenová úroveň | Cílová zátěž |
|:-------------|:-----------------|
| Basic | Úlohy vyžadující nízký výpočetní a I/O výkon. Mezi příklady patří servery používané pro vývoj a testování nebo rozsáhlé a zřídka používané aplikace. |
| Obecné použití | Většinu obchodních úloh, které vyžadují vyvážené výpočetní a paměťové prostředky se škálovatelnou I/O propustností. Mezi příklady patří pro servery hostující webové a mobilní aplikace a jiné podnikové aplikace.|
| Paměťově optimalizované | Vysoce výkonné databázové úlohy, které vyžadují výkon v paměti kvůli rychlejšímu zpracování transakcí a lepší souběžnosti. Mezi příklady patří servery pro zpracování dat v reálném čase a vysoce výkonné transakční a analytické aplikace.|

Po vytvoření serveru, počet virtuálních jader a cenovou úroveň (s výjimkou do a z Basic) lze změnit navýšit nebo snížit kapacitu během několika sekund. Také můžete samostatně upravit velikost úložiště nahoru a období uchovávání záloh navýšit nebo snížit kapacitu bez výpadků aplikace. Typ úložiště pro zálohování nelze změnit po vytvoření serveru. Další informace najdete v tématu [škálovat prostředky](#scale-resources) oddílu.

## <a name="compute-generations-and-vcores"></a>COMPUTE generace a virtuální jádra

Výpočetní prostředky jsou k dispozici jako virtuální jádra, která představuje logický procesor základního hardwaru. Generace 5 logické procesory jsou založené na Intel E5-2673 v4 (Broadwell) 2.3 GHz procesorech.

## <a name="storage"></a>Storage

Úložiště, které zřizujete je objem úložné kapacity k dispozici ke službě Azure Database pro MariaDB server. Úložiště se používá pro soubory databáze, dočasných souborů, protokoly transakcí a MariaDB server protokoly. Celkový objem úložiště, který zřídíte také definuje vstupně-výstupní kapacity k dispozici pro váš server.

|    | **Basic** | **Obecné účely** | **Optimalizované z hlediska paměti** |
|:---|:----------|:--------------------|:---------------------|
| Typ úložiště | Azure Storage úrovně Standard | Azure Premium Storage | Azure Premium Storage |
| Velikost úložiště | 5 GB až 1 TB | 5 GB až 4 TB | 5 GB až 4 TB |
| Zvýšení velikosti úložiště | 1 GB | 1 GB | 1 GB |
| IOPS | Proměnná |3 IOPS/GB<br/>Minimum 100 vstupně-výstupních operací<br/>Maximální počet 6000 vstupně-výstupních operací | 3 IOPS/GB<br/>Minimum 100 vstupně-výstupních operací<br/>Maximální počet 6000 vstupně-výstupních operací |

Můžete přidat další kapacitu, během a po vytvoření serveru. Úroveň Basic neposkytuje záruka vstupně-výstupních operací. V obecné účely a optimalizovaný pro paměť cenové úrovně se škálují vstupně-výstupních operací s velikost zřízeného úložiště poměr 3:1.

Můžete monitorovat spotřebu vstupně-výstupních operací na webu Azure Portal nebo pomocí příkazů rozhraní příkazového řádku Azure. Jsou důležité metriky pro monitorování [limitu úložiště, procento úložiště, využité úložiště a vstupně-výstupních operací procent](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Dosažení limitu úložiště

Server se označí jako jen pro čtení, když velikost volného úložiště klesne pod 5 GB nebo 5 % zřízeného úložiště, podle toho, která hodnota je nižší. Například, pokud jste zřídili 100 GB úložiště a skutečné využití prochází přes 95 GB, na serveru je označen jen pro čtení. Případně pokud jste zřídili 5 GB úložiště, server se označí jako jen pro čtení, když velikost volného úložiště klesne pod 250 MB.  

Zatímco se služba pokouší nastavit server jen pro čtení, všechny požadavky transakcí zápisu se zablokují a stávající aktivní transakce se budou provádět dál. Když je server nastavený jen pro čtení, všechny další operace zápisu a potvrzení transakcí selžou. Dotazy na čtení budou fungovat dál bez přerušení. Jakmile navýšíte velikost zřízeného úložiště, bude server připravený znovu přijímat transakce zápisu.

Doporučujeme nastavit upozornění pro upozornění, úložiště serveru se blíží prahové hodnoty, tomu se můžete vyhnout, převedení do stavu jen pro čtení. 

Další informace najdete v dokumentaci na [jak nastavit výstrahu](howto-alert-metric.md).

## <a name="backup"></a>Backup

Služby trvá automatické zálohy vašeho serveru. Období minimální doby uchování pro zálohy je sedm dní. Můžete nastavit dobu uchování o délce až po dobu 35 dní. Uchovávání je možné upravit kdykoli během životního cyklu serveru. Můžete vybrat mezi místně redundantní a geograficky redundantní zálohy. Geograficky redundantní zálohy jsou také uloženy v [geograficky spárované oblasti](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) oblasti, kde se vytvoří váš server. Tuto redundanci poskytuje úroveň ochrany v případě havárie. Můžete také získat lepší schopnost obnovení vašeho serveru do libovolné jiné oblasti Azure ve kterém je služba dostupná s geograficky redundantní zálohy. Není možné změnit mezi těmito dvěma možnostmi úložiště záloh po vytvoření serveru.

## <a name="scale-resources"></a>Škálování prostředků

Po vytvoření serveru nezávisle na sobě můžete změnit virtuálních jader, cenová úroveň (s výjimkou do a z Basic), jaká část úložiště a období uchovávání záloh. Typ úložiště pro zálohování nelze změnit po vytvoření serveru. Počet virtuálních jader je možné škálovat směrem nahoru nebo dolů. Období uchování zálohy je možné škálovat směrem nahoru nebo dolů na 7 pro po dobu 35 dní. Velikost úložiště může být pouze zvýšena. Škálování prostředků můžete udělat buď na portálu nebo rozhraní příkazového řádku Azure. 

<!--For an example of scaling by using Azure CLI, see [Monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->

Pokud změníte počet virtuálních jader nebo cenovou úroveň, kopii původního serveru se vytvoří s nové přidělení výpočetní prostředky. Po vytvoření a spuštění je nový server, připojení se přepnout na nový server. Během okamžiku, kdy systém přepne na nový server je možné navázat nová připojení a jsou všechny nepotvrzené transakce vráceny zpět. Toto okno se liší, ale ve většině případů je méně než minutu.

Škálování úložiště a změna období uchování zálohy jsou true online operace. Neexistuje žádný výpadek a vaší aplikace neprojeví. Vstupně-výstupních operací škálování s velikost zřízeného úložiště, můžete zvýšit IOPS k dispozici pro váš server vertikálním navýšení kapacity úložiště.

## <a name="pricing"></a>Ceny

Nejnovější informace o cenách najdete v článku Služba [stránce s cenami](https://azure.microsoft.com/pricing/details/mariadb/). Zobrazit náklady pro konfigurace, kterou chcete [webu Azure portal](https://portal.azure.com/#create/Microsoft.MariaDBServer) ukazuje měsíční náklady na **cenová úroveň** kartu podle možností, které zvolíte. Pokud nemáte předplatné Azure, můžete získat Odhadovaná cena cenovou kalkulačku Azure. Na [cenovou kalkulačku Azure](https://azure.microsoft.com/pricing/calculator/) webu, vyberte **přidat položky**, rozbalte **databází** kategorie a zvolte **– Azure Database pro MariaDB**přizpůsobit možnosti.

## <a name="next-steps"></a>Další postup
- Další informace o [služby omezení](concepts-limits.md).
- Zjistěte, jak [vytvoření MariaDB serveru na webu Azure Portal](quickstart-create-mariadb-server-database-using-azure-portal.md).

<!--
- Learn how to [monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->
