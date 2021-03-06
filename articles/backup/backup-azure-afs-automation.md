---
title: Nasazení a správa záloh sdílených složek Azure pomocí Powershellu
description: Nasazení a správa záloh sdílených složek Azure v Azure pomocí Powershellu
services: backup
author: pvrk
manager: shivamg
keywords: Prostředí PowerShell; Zálohování souborů Azure; Obnovení se soubory Azure;
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: 912336d697e8f7b5d9c71080ec9a052ca562da4b
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/28/2019
ms.locfileid: "55101126"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>Použití Powershellu k zálohování a obnovení sdílených složek Azure

Tento článek popisuje, jak pomocí rutin Azure Powershellu k zálohování a obnovení sdílené složky Azure pomocí trezoru služby Recovery Services. Trezor služby Recovery Services je prostředek Azure Resource Manageru, který se používá k ochraně dat a assetů v Azure Backup a Azure Site Recovery.

## <a name="concepts"></a>Koncepty

Pokud nejste obeznámeni s Azure Backup, získáte přehled o službě, přečtěte si téma [co je Azure Backup?](backup-introduction-to-azure-backup.md). Než začnete, naleznete v tématu Možnosti ve verzi preview, které slouží k zálohování Azure sdílených složek v [zálohování Azure sdílených složek](backup-azure-files.md).

Jak efektivně pomocí prostředí PowerShell, je nezbytné pro zjištění hierarchie objektů a kde začít z.

![Hierarchie objektů Recovery Services](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Chcete-li zobrazit **AzureRm.RecoveryServices.Backup** Reference k rutinám Powershellu najdete v článku [Azure Backup – rutiny služby Recovery Services](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) v knihovně Azure.

## <a name="setup-and-registration"></a>Instalace a registrace

> [!NOTE]
> Jak je uvedeno v [instalace modulu Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.13.0), podporu pro nové funkce v AzureRM modulu končí v listopadu 2018. Podpora se poskytuje pro zálohování sdílených složek Azure pomocí nového modulu prostředí PowerShell Az, která je teď obecně dostupná.

Použijte následující postup začít.

1. [Stáhněte si nejnovější verzi prostředí PowerShell Az](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0). Minimální požadovaná verze je 1.0.0.

2. Najít **Azure Backup Powershellu** rutiny, které jsou dostupné tak, že zadáte následující příkaz.

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    Zobrazí aliasů a rutin pro trezor služby Recovery Services, Azure Backup a Azure Site Recovery. Na následujícím obrázku je příklad, co vidíte. Není úplný seznam rutin.

    ![Seznam rutin služby Recovery Services](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Přihlaste se ke svému účtu Azure pomocí **připojit AzAccount**. Tato rutina zobrazí webová stránka, která vás vyzve k zadání přihlašovacích údajů k účtu:

    * Alternativně můžete zahrnout přihlašovacích údajů k účtu jako parametr v **připojit AzAccount** rutiny s využitím **– přihlašovací údaje** parametru.
    * Pokud jste partner CSP, který spolupracuje jménem klienta, určení zákazníka jako tenant s použitím názvu primární doména tenanta nebo ID Tenanta. Příkladem je **Connect AzAccount-Tenanta** fabrikam.com.

4. Přidružte předplatné, které chcete používat s účtem, protože účet může mít několik předplatných.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Pokud používáte Azure Backup poprvé, použijte **Register-AzResourceProvider** rutiny zaregistrujte zprostředkovatele služby Azure Recovery Services s vaším předplatným.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Ověří, zda zprostředkovatele úspěšně registrován pomocí následujícího příkazu.
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    Ve výstupu příkazu **RegistrationState** změny **registrované**. Pokud nevidíte tuto změnu, spusťte **Register-AzResourceProvider** rutinu znovu.

Tyto úlohy je možné automatizovat pomocí prostředí PowerShell:

* Vytvořte trezor služby Recovery Services.
* Konfigurace zálohování sdílených složek Azure.
* Aktivujte úlohu zálohování.
* Monitorování úlohy zálohování.
* Obnovení sdílené složky Azure.
* Obnovení jednotlivých souborů Azure ze sdílené složky Azure.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

Postupujte podle těchto kroků a vytvořte trezor služby Recovery Services.

1. Trezor služby Recovery Services je prostředek Resource Manageru, proto je nutné umístit v rámci skupiny prostředků. Můžete použít existující skupinu prostředků, nebo můžete vytvořit skupinu prostředků pomocí **New-AzResourceGroup** rutiny. Když vytvoříte skupinu prostředků, zadejte název a umístění pro skupinu prostředků.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Použití **New-AzRecoveryServicesVault** rutina pro vytvoření trezoru služby Recovery Services. Určení stejného umístění trezoru, protože byl použit pro skupinu prostředků.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Zadejte typ redundance úložiště používat. Můžete použít [místně redundantní úložiště](../storage/common/storage-redundancy-lrs.md) nebo [geograficky redundantní úložiště](../storage/common/storage-redundancy-grs.md). Následující příklad ukazuje **- BackupStorageRedundancy** možnost **testvault** nastavena na **GeoRedundant**.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Zobrazit tyto trezory v rámci předplatného

Chcete-li zobrazit všechny trezorů v předplatném, použijte **Get-AzRecoveryServicesVault**.

```powershell
Get-AzRecoveryServicesVault
```

Výstup se podobá následujícímu příkladu. Všimněte si, že přidružený **ResourceGroupName** a **umístění** jsou k dispozici.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

Řada rutin služby Azure Backup vyžaduje jako vstup objekt trezoru služby Recovery Services.

Použití **Set-AzRecoveryServicesVaultContext** nastavit kontext trezoru. Po nastavení kontext trezoru použije pro všechny další rutiny. Následující příklad nastaví kontext trezoru pro **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> Plánujeme přestat používat nastavení kontextu trezoru podle pokynů v prostředí Azure PowerShell. Namísto toho doporučujeme, aby uživatelé předat ID trezoru, jak je uvedeno v následující pokyny.

Můžete také uložit nebo načíst ID trezoru, ke které chcete provést operaci prostředí PowerShell a předat ho na příslušný příkaz.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Konfigurace zálohování pro sdílené složky Azure

### <a name="create-a-protection-policy"></a>Vytvořit zásady ochrany.

Zásady zálohování ochrany je přidružená aspoň jednu zásadu uchovávání informací. Zásady uchovávání informací Určuje, jak dlouho bod obnovení je zachována před odstraněním. Použití **Get-AzRecoveryServicesBackupRetentionPolicyObject** zobrazení výchozí zásady uchovávání informací. 

Podobně můžete použít **Get-AzRecoveryServicesBackupSchedulePolicyObject** získat výchozí plán zásady. **New-AzRecoveryServicesBackupProtectionPolicy** rutina vytvoří objekt prostředí PowerShell, který obsahuje informace o zásadách zálohování. Objekty zásad plán a uchovávání se používají jako vstupy **New-AzRecoveryServicesBackupProtectionPolicy** rutiny. 

Následující příklad ukládá v proměnné plán zásady a zásady uchovávání informací. V příkladu se používá k definici parametrů těchto proměnných při **NewPolicy** vytvořili zásady ochrany.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Výstup se podobá následujícímu příkladu.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

**NewAFSPolicy** trvá denní zálohování a uchovávají po dobu 30 dnů.

### <a name="enable-protection"></a>Povolení ochrany

Po definování zásady ochrany, můžete povolit ochranu pro sdílenou složku Azure s touto zásadou.

Nejdřív načíst objekt příslušné zásady s **Get-AzRecoveryServicesBackupProtectionPolicy** rutiny. Načte konkrétní zásady, nebo pro zobrazení zásad přidružený k typu úlohy, použijte tuto rutinu.

Následující příklad získá zásady pro typ úlohy **AzureFiles**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Výstup se podobá následujícímu příkladu.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> Časové pásmo **BackupTime** pole v prostředí PowerShell je koordinovaný světový čas (UTC). Když čas zálohování se zobrazí na webu Azure Portal, čas se upraví na místním časovém pásmu.
>
>

Tyto zásady načte zásady zálohování s názvem **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Použití **povolit AzRecoveryServicesBackupProtection** k povolení ochrany položka s daným zásadám. Poté, co je zásada přidružená k trezoru, pracovního postupu zálohování se aktivuje v době definovaný v plánu zásady.

Následující příklad povolí ochranu pro sdílené složky Azure **testAzureFileShare** pod účtem úložiště **testStorageAcct** zásadám **dailyafs**.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Příkaz počká, až úloha Konfigurace ochrany je dokončená a nabízí podobný výstup, jak je znázorněno.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>Spustit zálohu na vyžádání

Použití **zálohování AzRecoveryServicesBackupItem** k aktivaci úlohy zálohování pro chráněný sdílenou složku Azure. Načíst úložiště souborů a sdílené složky v rámci ho pomocí následujících příkazů a spustit zálohu na vyžádání.

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Příkaz vrátí úlohu s ID, které má sledovat, jak je znázorněno v následujícím příkladu.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Snímky sdílené složky Azure se používají při zálohy jsou prováděny, takže obvykle dokončení úlohy podle času, příkaz vrátí tento výstup.

### <a name="modify-the-protection-policy"></a>Upravit zásady ochrany

Chcete-li změnit zásady, které je chráněné sdílené složky Azure, použijte **povolit AzRecoveryServicesBackupProtection** relevantní zálohovaná položka a nové zásady ochrany.

Následující příklad změny **testAzureFS** zásady ochrany z **dailyafs** k **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-azure-files"></a>Obnovení sdílené složky Azure a služba soubory Azure

Celou sdílenou můžete obnovit do původního umístění nebo alternativního umístění. Podobně jednotlivé soubory ze sdílené složky můžete obnovit, příliš.

### <a name="fetch-recovery-points"></a>Načíst body obnovení

Použití **Get-AzRecoveryServicesBackupRecoveryPoint** rutiny pro zobrazení seznamu všech bodů obnovení zálohované položky. V následujícím skriptu, proměnná **$rp** je pole bodů obnovení pro vybrané záložní položky z posledních sedmi dnů. Pole má řazení proběhnout v obráceném pořadí času s nejnovějším dostupným bodem obnovení v indexu **0**. Použijte standardní indexování pole prostředí PowerShell a vyberte bod obnovení. V tomto příkladu **$rp [0]** Vybere poslední bod obnovení.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

Výstup se podobá následujícímu příkladu.

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```

Po výběru příslušné obnovení bodu obnovení sdílené složky nebo souboru do alternativního umístění nebo do původního umístění jak je popsáno zde.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Obnovení do alternativního umístění sdílené složky Azure

#### <a name="restore-an-azure-file-share"></a>Obnovení sdílené složky Azure

Určete alternativní umístění tím, že poskytuje následující informace:

* **TargetStorageAccountName**: Účet úložiště, ke kterému je obnovit zálohovaná obsah. Cílový účet úložiště musí být ve stejném umístění jako trezor.
* **TargetFileShareName**: Sdílené složky v rámci cílového úložiště účtu zálohovanou obsah je obnovit.
* **TargetFolder**: Složka, v rámci sdílené složky, do kterého se data obnovit. Pokud se zálohovaná obsahu je možné obnovit do kořenové složky, poskytují cílové složky hodnoty jako prázdný řetězec.
* **ResolveConflict**: Instrukce, pokud dojde ke konfliktu s obnovená data. Přijímá **přepsat** nebo **přeskočit**.

Poskytuje tyto parametry pro příkaz restore k obnovení zálohovaných sdílené složky do alternativního umístění.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

Příkaz vrátí úlohu s ID, které má sledovat, jak je znázorněno v následujícím příkladu.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

#### <a name="restore-an-azure-file"></a>Obnovení Azure file

Obnovení jednotlivých souborů místo celou sdílenou jednoznačné identifikaci jednotlivých souborů tím, že poskytuje následující parametry:

* **TargetStorageAccountName**: Účet úložiště, ke kterému je obnovit zálohovaná obsah. Cílový účet úložiště musí být ve stejném umístění jako trezor.
* **TargetFileShareName**: Sdílené složky v rámci cílového úložiště účtu zálohovanou obsah je obnovit.
* **TargetFolder**: Složka, v rámci sdílené složky, do kterého se data obnovit. Pokud se zálohovaná obsahu je možné obnovit do kořenové složky, poskytují cílové složky hodnoty jako prázdný řetězec.
* **SourceFilePath**: Absolutní cesta souboru pro obnovení ve sdílené složce, jako řetězec. Tato cesta je stejnou cestu používané **Get-AzStorageFile** rutiny Powershellu.
* **SourceFileType**: Určuje, zda je vybrána adresář nebo soubor. Přijímá **Directory** nebo **souboru**.
* **ResolveConflict**: Instrukce, pokud dojde ke konfliktu s obnovená data. Přijímá **přepsat** nebo **přeskočit**.

Další parametry se vztahují pouze k jednotlivých souborů, které je potřeba obnovit.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Tento příkaz také vrátí úlohu s ID, které má sledovat, jak bylo dříve uvedeno.

### <a name="restore-azure-file-shares-to-the-original-location"></a>Obnovit do původního umístění sdílené složky Azure

Při obnovení do původního umístění není potřeba zadat všechny související cílové a cílové parametry. Pouze **ResolveConflict** musí být zadaná.

#### <a name="overwrite-an-azure-file-share"></a>Přepsat sdílené složky Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Přepsat soubor v Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Přehled zálohování a obnovení úloh

Operace zálohování a obnovení na vyžádání vrací úlohu spolu s ID, jak je znázorněno v předchozí části ["Spustit zálohu na vyžádání."](#trigger-an-on-demand-backup) Použití **Get-AzRecoveryServicesBackupJobDetails** rutiny sledovat průběh úlohy a načíst další podrobnosti.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```
