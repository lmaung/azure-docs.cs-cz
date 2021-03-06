---
title: Automatizované zálohování pro SQL Server 2014 Azure Virtual Machines | Dokumentace Microsoftu
description: Vysvětluje funkci automatizované zálohování pro SQL Server 2014, virtuální počítače spuštěné v Azure. Tento článek je určený speciálně pro virtuální počítače pomocí Resource Manageru.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 99439c2b6bd4fdd271dda7a49850c5b6f44330b3
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984711"
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatizovaná záloha pro SQL Server 2014 Virtual Machines (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Automatizované zálohování automaticky nakonfiguruje [spravovaného zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající i nové databáze na Virtuálním počítači Azure s SQL Server 2014 Standard nebo Enterprise. To umožňuje nakonfigurovat pravidelné zálohy, které využívají úložiště objektů blob v Azure odolné. Automatizované zálohování závisí [rozšíření agenta SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Požadavky
Chcete-li pomocí automatizovaného zálohování, zvažte následující požadavky:

**Operační systém**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**Verze a edice systému SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Automatizované zálohování funguje s SQL serverem 2014. Pokud používáte SQL Server 2016 a 2017, můžete k zálohování databází v2 automatizovaného zálohování. Další informace najdete v tématu [v2 automatizované zálohování pro SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

**Konfigurace databáze**:

- Cílová databáze musí použít model úplného obnovení. Další informace o dopadu úplném modelu obnovení na zálohování, naleznete v tématu [zálohování v části the úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).
- Cílová databáze musí být na výchozí instanci SQL serveru. Rozšíření SQL Server IaaS nepodporuje pojmenované instance.

> [!NOTE]
> Automatizované zálohování spoléhá na rozšíření agenta SQL Server IaaS. Aktuální Image SQL z Galerie virtuálních počítačů přidat toto rozšíření ve výchozím nastavení. Další informace najdete v tématu [rozšíření agenta SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení

Následující tabulka popisuje možnosti, které je možné nakonfigurovat pro automatizované zálohování. Skutečný konfigurační kroky se liší v závislosti na tom, zda používáte portál Azure portal nebo příkazů Windows Powershellu Azure.

| Nastavení | Rozsah (výchozí) | Popis |
| --- | --- | --- |
| **Automatizované zálohování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže automatické zálohování pro virtuální počítač Azure s SQL Server 2014 Standard nebo Enterprise. |
| **Doba uchování** | 1 – 30 dnů (30 dnů) | Počet dní uchování zálohy. |
| **Účet úložiště** | Účet služby Azure Storage | Účet úložiště Azure k ukládání souborů automatizovaného zálohování ve službě blob storage. Kontejner se vytvoří v tomto umístění pro uložení všechny záložní soubory. Zásady vytváření názvů záložní soubor obsahuje data, času a název počítače. |
| **Šifrování** | Povolí nebo zakáže (zakázáno) | Povolí nebo zakáže šifrování. Když je povoleno šifrování, certifikátů používaných pro obnovení zálohy jsou umístěny v zadaný účet úložiště ve stejném `automaticbackup` kontejner pomocí stejné zásady vytváření názvů. Pokud se změní heslo, se toto heslo se vygeneruje nový certifikát, ale starý certifikát zůstane k obnovení předchozího záloh. |
| **Heslo** | Heslo text | Heslo šifrovacích klíčů. Toto je jenom nutné, pokud je povolené šifrování. Aby bylo možné obnovit šifrované zálohování, musíte mít správné heslo a související certifikát, který byl použit v době, kdy bylo provedeno zálohování. |

## <a name="configure-in-the-portal"></a>Konfigurace na portálu

Na webu Azure portal můžete použít ke konfiguraci automatizovaného zálohování během zřizování nebo pro stávající SQL Server 2014 virtuální počítače.

## <a name="configure-new-vms"></a>Konfigurace nových virtuálních počítačů

Konfigurace automatizovaného zálohování, když vytvoříte nový SQL Server 2014 virtuálního počítače v modelu nasazení Resource Manageru pomocí webu Azure portal.

V **nastavení systému SQL Server** vyberte **automatizované zálohování**. Ukazuje následující snímek obrazovky Azure portal **automatizované zálohování SQL** nastavení.

![Konfigurace automatizovaného zálohování SQL na webu Azure portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

## <a name="configure-existing-vms"></a>Konfigurace existujících virtuálních počítačů

Existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server. Vyberte **konfigurace systému SQL Server** části virtuální počítač **nastavení**.

![Automatizované zálohování SQL pro stávající virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

V **konfigurace systému SQL Server** podokně klikněte na tlačítko **upravit** tlačítko v části pro automatizovaná zálohování.

![Konfigurace automatizovaného zálohování SQL pro stávající virtuální počítače](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Až budete hotovi, klikněte na tlačítko **OK** tlačítko v dolní části **konfigurace systému SQL Server** nastavení uložte provedené změny.

Pokud povolíte automatizované zálohování pro první, Azure nakonfiguruje agenta SQL Server IaaS na pozadí. Během této doby nemusí zobrazit na webu Azure portal, automatické zálohování je nakonfigurované. Počkejte několik minut, než agent nainstalován, nakonfigurován. Následně se na webu Azure portal bude odrážet nové nastavení.

> [!NOTE]
> Můžete také nakonfigurovat automatizované zálohování pomocí šablony. Další informace najdete v tématu [šablona Azure quickstart pro automatizované zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configure-with-powershell"></a>Konfigurovat pomocí Powershellu

Konfigurace automatizovaného zálohování můžete použít PowerShell. Než začnete, musíte mít:

- [Stáhněte a nainstalujte nejnovější Azure PowerShell](https://aka.ms/webpi-azps).
- Otevřete prostředí Windows PowerShell a přidružíte ho k účtu se **připojit AzAccount** příkazu.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

### <a name="install-the-sql-iaas-extension"></a>Instalace rozšíření SQL IaaS
Zřízení virtuálního počítače s SQL serverem na webu Azure Portal by již nainstalována rozšíření SQL Server IaaS. Můžete určit, pokud je nainstalován pro váš virtuální počítač pomocí volání **rutiny Get-AzVM** příkazu a podíváte **rozšíření** vlastnost.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Pokud je nainstalovaná rozšíření agenta SQL Server IaaS, měli byste vidět, že je uveden jako "SqlIaaSAgent" nebo "SQLIaaSExtension". **Stav zřizování** pro rozšíření by měl také zobrazí "ÚSPĚCH".

Pokud není nainstalovaná nebo se nepovedlo zřídit, můžete ho nainstalovat pomocí následujícího příkazu. Kromě virtuální počítač a skupinou prostředků, musíte zadat také oblast (**$region**), který váš virtuální počítač se nachází v.

```powershell
$region = "EASTUS2"
Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

> [!IMPORTANT]
> Pokud rozšíření ještě není nainstalovaná, instalaci rozšíření restartuje službu systému SQL Server.

### <a id="verifysettings"></a> Ověřit aktuální nastavení

Pokud jste povolili automatizované zálohování během zřizování, můžete pomocí prostředí PowerShell zkontrolovat konfiguraci vašeho aktuální ho. Spustit **Get-AzVMSqlServerExtension** příkaz a prověřte **AutoBackupSettings** vlastnost:

```powershell
(Get-AzVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

By měl získat výstup podobný následujícímu:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Pokud výstup ukazuje, že **povolit** je nastavena na **False**, budete muset povolit automatické zálohování. Dobrou zprávou je, že můžete povolit a nakonfigurovat automatizované zálohování stejným způsobem. V části Další informace.

> [!NOTE] 
> Pokud zaškrtnete okamžitě po provedení změny nastavení, je možné, že se dostanete zpět původní hodnoty konfigurace. Počkejte pár minut a zkontrolujte nastavení znovu a ujistěte se, že vaše změny byly použity.

### <a name="configure-automated-backup"></a>Konfigurace automatizovaného zálohování
Prostředí PowerShell můžete povolit automatické zálohování i či kdykoli upravit jeho konfiguraci a chování.

Nejprve vyberte nebo vytvořte účet úložiště pro zálohování souborů. Následující skript vybere účtu úložiště nebo ji vytvoří, pokud neexistuje.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }
```

> [!NOTE]
> Automatizované zálohování nepodporuje ukládání záloh ve službě storage úrovně premium, ale může trvat zálohování disků virtuálních počítačů, které používají službu Premium Storage.

Potom použijte **New-AzVMSqlServerAutoBackupConfig** příkaz pro povolení a konfigurace nastavení automatizovaného zálohování pro ukládání záloh v účtu úložiště Azure. V tomto příkladu zálohy se uchovávají po dobu 10 dnů. Druhý příkaz **Set-AzVMSqlServerExtension**, aktualizuje zadaný virtuální počítač Azure s těmito nastaveními.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

To může trvat několik minut, instalaci a konfiguraci agenta SQL Server IaaS.

> [!NOTE]
> Existují další nastavení pro **New-AzVMSqlServerAutoBackupConfig** , která platí jenom pro SQL Server 2016 a automatizovaného zálohování v2. SQL Server 2014 nepodporuje následující nastavení: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, a **LogBackupFrequencyInMinutes**. Při pokusu o konfiguraci těchto nastavení na virtuálním počítači s SQL Server 2014, se nezobrazí žádná chyba, ale není položka konfigurace použije. Pokud chcete použít tato nastavení na virtuálním počítači s SQL serverem 2016, přečtěte si téma [v2 automatizované zálohování pro SQL Server 2016 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup-v2.md).

Pokud chcete povolit šifrování, upravit předchozí skript k předání **EnableEncryption** parametr spolu s heslem (zabezpečený řetězec) **CertificatePassword** parametru. Následující skript povolí nastavení automatizovaného zálohování v předchozím příkladu a přidá šifrování.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Potvrďte nastavení se použijí, [ověřit konfiguraci automatizovaného zálohování](#verifysettings).

### <a name="disable-automated-backup"></a>Zakázat automatické zálohování

Chcete-li zakázat automatické zálohování, spusťte stejný skript bez **-povolit** parametr **AzVMSqlServerAutoBackupConfig nový** příkaz. Chybí **-povolit** parametr signály příkazu zakažte funkci. Stejně jako u instalace to může trvat několik minut zakázat automatizovaného zálohování.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Ukázkový skript

Následující skript představuje sadu proměnných, které si můžete přizpůsobit pro povolení a konfigurace automatizovaného zálohování pro virtuální počítač. Ve vašem případě potřebujete vlastní skript na základě vašich požadavků. Například je třeba provést změny, pokud chcete zakázat zálohování systémových databází nebo povolit šifrování.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension

Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>Monitorování

K monitorování automatizované zálohování SQL serveru 2014, máte dvě hlavní možnosti. Protože automatizovaného zálohování použije funkci spravované zálohování SQL serveru, se stejné techniky monitorování platit pro oboje.

Nejprve můžete dotazovat stav voláním [msdb.smart_admin.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Nebo dotazu [msdb.smart_admin.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) funkce vracející tabulku.

> [!NOTE]
> Schéma pro spravovanou zálohu v SQL serveru 2014 je **msdb.smart_admin**. V SQL serveru 2016 to změnit na **msdb.managed_backup**, a toto novější schéma používají, referenční témata. Pro SQL Server 2014, je nutné nadále používat, ale **smart_admin** schéma pro všechny objekty spravovaného zálohování systému.

Další možností je využít integrované funkce databázového e-mailu pro oznámení.

1. Volání [msdb.smart_admin.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) uložené procedury k e-mailovou adresu k přiřazení **SSMBackup2WANotificationEmailIds** parametru. 
1. Povolit [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) odesílat e-maily z virtuálního počítače Azure.
1. Umožňuje nakonfigurovat databázového e-mailu SMTP serveru a uživatelské jméno. Databázového e-mailu můžete nakonfigurovat v SQL Server Management Studio nebo pomocí příkazů jazyka Transact-SQL. Další informace najdete v tématu [databázového e-mailu](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Konfigurace agenta systému SQL Server k použití databázového e-mailu](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. Ověřte, jestli je SMTP port povoleno přes místní bránu firewall virtuálního počítače a skupiny zabezpečení sítě pro virtuální počítač.

## <a name="next-steps"></a>Další postup

Automatizované zálohování nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure. Proto je důležité [najdete v dokumentaci spravovaného zálohování systému SQL Server 2014](https://msdn.microsoft.com/library/dn449497(v=sql.120).aspx).

Můžete najít další zálohování a obnovení doprovodné materiály pro SQL Server na virtuálních počítačích Azure v následujícím článku: [Zálohování a obnovení pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-backup-recovery.md).

Informace o dalších úlohách dostupných automation najdete v tématu [rozšíření agenta SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Další informace o spuštění SQL serveru na virtuálních počítačích Azure najdete v tématu [systému SQL Server na Azure Virtual Machines – přehled](virtual-machines-windows-sql-server-iaas-overview.md).
