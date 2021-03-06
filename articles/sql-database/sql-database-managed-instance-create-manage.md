---
title: Referenční dokumentace rozhraní API pro správu pro Azure SQL Database Managed Instance | Dokumentace Microsoftu
description: Další informace o vytváření a správa Azure SQL Database Managed instance.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 5d9952581049198131e30cd7d0ba0ebf6a14cc54
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098864"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Spravovaná reference k rozhraní API pro Azure SQL Database Managed instance

Můžete vytvořit a spravovat Azure SQL Database Managed instance pomocí webu Azure portal, Powershellu, rozhraní příkazového řádku Azure, rozhraní REST API a příkazů jazyka Transact-SQL. V tomto článku najdete přehled funkcí a rozhraní API, které můžete použít k vytvoření a konfigurace Managed Instance.

## <a name="azure-portal-create-a-managed-instance"></a>Azure portal: Vytvoření spravované instance

Rychlý start ukazující vytvoření Azure SQL Database Managed Instance, naleznete v tématu [rychlý start: Vytvoření spravované Instance Azure SQL Database](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-managed-instances"></a>PowerShell: Vytváření a správě spravované instance

Můžete vytvářet a spravovat spravované instance pomocí Azure Powershellu, použijte následující rutiny Powershellu. Pokud potřebujete instalaci nebo upgrade prostředí PowerShell, najdete v článku [instalace modulu Azure PowerShell](/powershell/azure/install-az-ps).

> [!TIP]
> Příklady skriptů Powershellu, najdete v části [skript rychlý start: Vytvoření spravované Instance Azure SQL pomocí prostředí PowerShell knihovny](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Rutina | Popis |
| --- | --- |
|[New-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstance)|Vytvoří spravované Instance Azure SQL Database |
|[Get-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstance)|Vrátí informace o spravované Instance Azure SQL|
|[Set-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlinstance)|Nastaví vlastnosti pro Azure SQL Database Managed Instance|
|[Remove-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstance)|Odebere instanci spravované databáze Azure SQL|
|[New-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstancedatabase)|Vytvoří databázi Azure SQL Database Managed Instance|
|[Get-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstancedatabase)|Vrátí informace o spravované Instance Azure SQL database|
|[Remove-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstancedatabase)|Odebere databázi spravované instanci databáze SQL Azure|
|[Restore-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqlinstancedatabase)|Obnoví databázi spravované instanci databáze SQL Azure|

## <a name="azure-cli-create-and-manage-managed-instances"></a>Azure CLI: Vytváření a správě spravované instance

Můžete vytvářet a spravovat spravované instance s [rozhraní příkazového řádku Azure](/cli/azure), použijte následující [Azure CLI SQL Managed Instance](/cli/azure/sql/mi) příkazy. Rozhraní příkazového řádku můžete spustit v prohlížeči pomocí [Cloud Shellu](/azure/cloud-shell/overview) nebo [nainstalovat](/cli/azure/install-azure-cli) v systémech macOS, Linux nebo Windows.

> [!TIP]
> Rychlý start Azure CLI najdete v části [práce s SQL Managed Instance pomocí rozhraní příkazového řádku Azure](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).

| Rutina | Popis |
| --- | --- |
|[Vytvoření az sql mi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Vytvoří spravovanou instanci|
|[seznam mi az sql](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Seznam dostupných spravovaných instancí|
|[Zobrazit mi az sql](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Získat podrobnosti pro Managed Instance|
|[aktualizace mi az sql](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Aktualizace spravované Instance|
|[az sql mi delete](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Odebere spravované Instance|
|[Vytvoření az sql midb](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Vytvoří spravovanou databázovou|
|[seznam midb az sql](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Seznamy, které jsou k dispozici spravované databáze|
|[obnovení midb az sql](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Obnovení spravované databáze|
|[Odstranit midb az sql](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Odebere spravované databáze|

## <a name="transact-sql-create-and-manage-instance-databases"></a>Transact-SQL: Vytvářet a spravovat instance databáze

Vytvoření a Správa instance databáze po vytvoření Managed Instance, použijte následující příkazy T-SQL. Můžete použít tyto příkazy pomocí webu Azure portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), nebo jiný program, který můžete připojit k serveru Azure SQL Database a předat příkazů jazyka Transact-SQL.

> [!TIP]
> Rychlé starty budete muset nakonfigurovat a připojte se k Managed Instance pomocí SQL Server Management Studio na Microsoft Windows, naleznete v části [rychlý start: Konfigurace virtuálního počítače Azure pro připojení k Azure SQL Database Managed Instance](sql-database-managed-instance-configure-vm.md) a [rychlý start: Konfigurace připojení typu point-to-site k Azure SQL Database Managed Instance z místní](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> Nejde vytvořit nebo odstranit Managed Instance pomocí příkazů jazyka Transact-SQL.

| Příkaz | Popis |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Vytvoří novou databázi Managed Instance. Musíte být připojení k hlavní databázi, a vytvořit novou databázi.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Upraví služby spravované Instance Azure SQL database.|

## <a name="rest-api-create-and-manage-managed-instances"></a>REST API: Vytváření a správě spravované instance

Vytvoření a správa spravovaných instancí, použijte tyto požadavky rozhraní REST API.

| Příkaz | Popis |
| --- | --- |
|[Spravované instance - vytvořit nebo aktualizovat](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Vytvoří nebo aktualizuje Managed Instance.|
|[Spravované instance - Delete](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Odstraní spravované Instance.|
|[Spravované instance - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Načte spravované Instance.|
|[Spravované instance – seznam](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Vrátí seznam spravovaných instancí v rámci předplatného.|
|[Spravované instance – seznam podle skupin prostředků](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Vrátí seznam spravovaných instancí ve skupině prostředků.|
|[Spravovaná instance – aktualizace](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Aktualizuje Managed Instance.|

## <a name="next-steps"></a>Další postup

- Další informace o migraci databáze SQL serveru do Azure najdete v tématu [migrace do služby Azure SQL Database](sql-database-single-database-migrate.md).
- Informace o podporovaných funkcích najdete v tématu [Funkce](sql-database-features.md).
