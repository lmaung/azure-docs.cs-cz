---
title: Ukázkové skripty Azure PowerShellu pro službu SQL Database | Microsoft Docs
description: Ukázkové skripty Azure PowerShellu, které vám pomůžou vytvářet a spravovat servery služby Azure SQL Database, elastické fondy, databáze a brány firewall.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/01/2019
ms.openlocfilehash: 07e530a30898e57916b91632c4bf49d43d69471a
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564846"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Ukázky v Azure PowerShellu pro službu Azure SQL Database

Azure SQL Database umožňuje konfigurovat databáze, instance a fondy pomocí Azure Powershellu.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte v tomto kurzu použít modul Azure PowerShell verze 5.7.0 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-az-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Connect-AzureRmAccount` pro vytvoření připojení k Azure.

## <a name="single-database-and-elastic-pools"></a>Jediné databáze a elastické fondy

Následující tabulka obsahuje odkazy na ukázkové skripty Azure PowerShellu pro službu Azure SQL Database.

| |  |
|---|---|
|**Vytvoření a konfigurace izolovaných databází a elastických fondů**||
| [Vytvoření izolované databáze a konfigurace pravidla brány firewall serveru databáze](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu vytvoří izolovanou databázi SQL Azure a nakonfiguruje pravidlo brány firewall na úrovni serveru. |
| [Vytváření elastických fondů a přesun databází ve fondu](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu vytvoří elastické fondy Azure SQL Database, přesune databáze ve fondu a změní výpočetní velikosti.|
|**Konfigurace geografické replikace a převzetí služeb při selhání**||
| [Konfigurace a převzetí služeb při selhání izolované databáze s využitím aktivní geografické replikace](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript PowerShellu nakonfiguruje aktivní geografickou replikaci pro izolovanou databázi SQL Azure a převezme její služby při selhání do sekundární repliky. |
| [Konfigurace a převzetí služeb při selhání databáze ve fondu s využitím aktivní geografické replikace](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript PowerShellu nakonfiguruje aktivní geografickou replikaci pro databázi SQL Azure v elastickém fondu SQL a převezme její služby při selhání do sekundární repliky. |
| [Konfigurace a převzetí služeb při selhání skupiny převzetí služeb při selhání pro izolovanou databázi](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu nakonfiguruje skupinu převzetí služeb při selhání pro instanci serveru služby Azure SQL Database, do této skupiny převzetí služeb při selhání přidá databázi a převezme její služby při selhání na sekundární server. |
|**Škálování izolované databáze a elastického fondu**||
| [Škálování izolované databáze](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu monitoruje metriky výkonu databáze SQL Azure, škáluje ji na vyšší výpočetní velikost a vytvoří pravidlo upozornění na jednu z metrik výkonu. |
| [Škálování elastického fondu](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu monitoruje metriky výkonu elastického fondu databáze SQL Azure, škáluje ji na vyšší výpočetní velikost a vytvoří pravidlo upozornění na jednu z metrik výkonu.  |
| **Auditování a detekce hrozeb** |
| [Konfigurace auditování a detekce hrozeb](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript PowerShellu nakonfiguruje zásady auditování a detekce hrozeb pro databázi SQL Azure. |
| **Obnovení, kopírování a import databáze**||
| [Obnovení databáze](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript PowerShellu obnoví databázi SQL Azure z geograficky redundantní zálohy a odstraněnou databázi SQL Azure z poslední zálohy. |
| [Kopírování databáze na nový server](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript PowerShellu vytvoří kopii stávající databáze SQL Azure na novém serveru SQL Azure. |
| [Import databáze ze souboru bacpac](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript PowerShellu importuje databázi na server SQL Azure ze souboru bacpac. |
| **Synchronizace dat mezi databázemi**||
| [Synchronizace dat mezi databázemi SQL](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu nakonfiguruje Synchronizaci dat pro synchronizaci mezi několika databázemi SQL Azure. |
| [Synchronizace dat mezi službou SQL Database a místním SQL Serverem](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu nakonfiguruje Synchronizaci dat pro synchronizaci mezi databází SQL Azure a místní databází SQL Serveru. |
| [Aktualizace schématu synchronizace pro Synchronizaci dat SQL](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript PowerShellu přidá položky do schématu synchronizace pro Synchronizaci dat nebo je z něj odebere. |
|||

Další informace o [jediné databáze rozhraní API Azure PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases).

## <a name="managed-instance"></a>MI

Následující tabulka obsahuje odkazy na ukázkové skripty Azure Powershellu pro Azure SQL Database – Managed Instance.

| |  |
|---|---|
|**Vytvořte a nakonfigurujte spravované instance**||
| [Vytvoření a správa spravované instance](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) | Teto skript PowerShellu ukazuje, jak vytvořit a spravovat spravovanou instanci pomocí Azure PowerShellu. |
| [Vytvoření a správa Managed Instance pomocí šablony Azure Resource Manageru](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell ukazuje, jak vytvářet a spravovat Managed Instance pomocí šablony Azure Resource Manageru a Azure Powershellu.|
| **Nakonfigurovat transparentní šifrování dat (TDE)**||
| [Správa transparentního šifrování dat v Managed Instance pomocí vlastního klíče ze služby Azure Key Vault](scripts/transparent-data-encryption-byok-sql-managed-instance-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript Powershellu nakonfiguruje transparentní šifrování dat (TDE) ve scénáři přineste si vlastní klíč pro spravované Instance Azure SQL, použití klíče ze služby Azure Key Vault|
|||

Další informace o [spravované Instance Azure rozhraní API prostředí PowerShell](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances).
