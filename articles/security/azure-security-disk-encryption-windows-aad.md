---
title: Azure Disk Encryption pomocí virtuálních počítačů IaaS v Azure AD aplikace Windows (předchozí verze)
description: Tento článek obsahuje pokyny týkající se povolení Microsoft Azure Disk Encryption pro Windows virtuální počítače IaaS.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 12/17/2018
ms.custom: seodec18
ms.openlocfilehash: 1dbdf1ba39acb526e2552e480e3081efc9f30f32
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990250"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms-previous-release"></a>Povolit Disk Encryption pro Windows virtuálních počítačů Azure IaaS (předchozí verze)

**Novou verzi sady Azure Disk Encryption eliminuje požadavek na poskytnutí parametrem aplikace Azure AD povolit šifrování disku virtuálního počítače. Nové verze se už nevyžadují zadejte přihlašovací údaje Azure AD během kroku povolení šifrování. Všechny nové virtuální počítače musí být zašifrován bez parametrů aplikace Azure AD pomocí nové verze. Chcete-li zobrazit pokyny, jak povolit šifrování disku virtuálního počítače pomocí nové verze, najdete v článku [Azure Disk Encryption pro Windows VMS](azure-security-disk-encryption-windows.md). Virtuální počítače, které již byly šifrované pomocí aplikace Azure AD, parametry jsou stále podporovány a má pokračovat udržovat syntaxí AAD.**


Můžete povolit řadu scénářů šifrování disku a kroků může lišit v závislosti scénáři. Následující části se věnují scénáře podrobněji pro virtuální počítače IaaS s Windows. Než budete moct použít šifrování disku, [požadavky Azure Disk Encryption](../security/azure-security-disk-encryption-prerequisites-aad.md) musíte provést. 

Přijmout [snímku](../virtual-machines/windows/snapshot-copy-managed-disk.md) nebo před disky jsou šifrované zálohovat. Zálohy Ujistěte se, že možnost obnovení je možné, pokud dojde k neočekávané chybě při šifrování. Virtuální počítače se spravovanými disky vyžadují zálohu, než dojde k šifrování. Po provedení zálohy můžete použít rutinu Set-AzVMDiskEncryptionExtension zadáním parametru - skipVmBackup šifrování spravované disky. Další informace o tom, jak zálohování a obnovení šifrovaných virtuálních počítačů najdete v tématu [Azure Backup](../backup/backup-azure-vms-encryption.md) článku. 

>[!WARNING]
> - Pokud jste už dřív použili [Azure Disk Encryption pomocí Azure AD app](azure-security-disk-encryption-prerequisites-aad.md) pro šifrování tento virtuální počítač, budete muset pokračovat tuto možnost použijte k šifrování virtuálního počítače. Nemůžete použít [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) na tento šifrovaný virtuální počítač jako tato akce není podporovaný scénář význam přepnutí mimo aplikaci AAD pro tento šifrovaný virtuální počítač se zatím nepodporuje.
> - Pokud chcete mít jistotu, že šifrování tajných kódů není překračují hranice regionální, Azure Disk Encryption musí být umístěné ve stejné oblasti služby Key Vault a virtuální počítače. Vytvoření a použití služby Key Vault, která je ve stejné oblasti jako virtuální počítač k šifrování. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-encryption-on-new-iaas-vms-created-from-the-marketplace"></a>Povoluje šifrování na nové virtuální počítače IaaS vytvořené z Marketplace
Můžete povolit šifrování disku na nový virtuální počítač IaaS Windows z Tržiště v Azure pomocí šablony Resource Manageru. Šablona vytvoří nový šifrované Windows virtuální počítač pomocí image z galerie systému Windows Server 2012.

1. Na [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), klikněte na tlačítko **nasadit do Azure**.

2. Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, parametry, právní podmínky a smlouvy. Klikněte na tlačítko **nákupní** k nasazení nového virtuálního počítače IaaS, kde je povolené šifrování.

3. Po nasazení šablony, ověřte stav šifrování virtuálního počítače pomocí upřednostňované metody:
     - Ověření pomocí Azure CLI pomocí [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) příkazu. 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
         ```

     - Ověření pomocí Azure Powershellu pomocí [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) rutiny. 

         ```azurepowershell-interactive
         Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
         ```

     -  Vyberte virtuální počítač a potom klikněte na **disky** pod **nastavení** záhlaví a ověřit stav šifrování na portálu. V grafu v části **šifrování**, uvidíte, jestli je povolené. 
           ![Azure portal – povolené šifrování disku](./media/azure-security-disk-encryption/disk-encryption-fig2.png) v následující tabulce jsou uvedeny parametry šablony Resource Manageru pro nové virtuální počítače z Tržiště scénáře pomocí ID klienta Azure AD:

| Parametr | Popis | 
| --- | --- |
| adminUserName | Uživatelské jméno správce pro virtuální počítač. |
| adminPassword | Heslo správce pro virtuální počítač. |
| newStorageAccountName | Název účtu úložiště pro uložení operačního systému a dat virtuálních pevných disků. |
| vmSize | Velikost virtuálního počítače. V současné době jsou podporovány pouze standardní A, D a G series. |
| virtualNetworkName | Název virtuální sítě, síťové karty virtuálního počítače by měly patřit do. |
| subnetName | Název podsítě ve virtuální síti, která síťové karty virtuálního počítače by měly patřit do. |
| AADClientID | ID klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů. |
| AADClientSecret | Tajný kód klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů. |
| KeyVaultURL | Adresa URL, která klíč Bitlockeru, musí být nahrán do trezoru klíčů. Můžete ho získat pomocí rutiny `(Get-AzKeyVault -VaultName,-ResourceGroupName).VaultURI` nebo rozhraní příkazového řádku Azure `az keyvault show --name "MySecureVault" --query properties.vaultUri` |
| KeyEncryptionKeyURL | Adresa URL šifrovací klíč klíče, který se používá k šifrování vygenerovaný klíč nástroje BitLocker (volitelné). </br> </br>KeyEncryptionKeyURL je volitelný parametr. Můžete přinést vlastní KEK další ochranná datového šifrovacího klíče (tajný klíč přístupového hesla) v trezoru klíčů. |
| keyVaultResourceGroup | Skupina prostředků trezoru klíčů. |
| vmName | Název virtuálního počítače, který má být proveden na operace šifrování. |


## <a name="bkmk_RunningWinVM"></a> Povoluje šifrování na existující nebo spouštění virtuálních počítačů IaaS s Windows
V tomto scénáři můžete povolit šifrování pomocí šablony, rutin prostředí PowerShell nebo příkazů rozhraní příkazového řádku. Následující části podrobněji popisují jak povolit Azure Disk Encryption. 

>[!IMPORTANT]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Příkaz Set-AzVMDiskEncryptionExtension selže ve virtuálních počítačích spravovaných disků na základě dokud provedl zálohování a tento parametr nebyl zadán. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Povoluje šifrování na existující nebo spouštění virtuálních počítačů pomocí Azure Powershellu 
Použití [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) rutina pro povolení šifrování u spuštěného virtuálního počítače IaaS v Azure. Informace o povolení šifrování v Azure Disk Encryption pomocí rutin prostředí PowerShell najdete v tématu v příspěvcích na blogu [prozkoumání Azure Disk Encryption pomocí Azure Powershellu – část 1](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) a [prozkoumání Azure Disk Encryption pomocí Azure Powershellu – část 2](https://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

-  **Šifrování spuštěného virtuálního počítače pomocí tajného klíče klienta:** Níže uvedený skript inicializuje proměnných a spustí rutinu Set-AzVMDiskEncryptionExtension. Skupina prostředků, virtuální počítač, trezor klíčů, aplikace AAD a tajný kód klienta by měl již byly vytvořeny jako požadavky. Nahraďte hodnoty MySecureRg MySecureVM, MySecureVault, My-AAD-client-ID a My-AAD-client-secret.
     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Šifrování spuštěného virtuálního počítače pomocí KEK zabalit tajný kód klienta:** Azure Disk Encryption umožňuje určit existujícího klíče v trezoru klíčů při zabalení šifrování tajných kódů disku, které byly generovány při povolení šifrování. Pokud je zadaný šifrovací klíč klíče, Azure Disk Encryption používá tento klíč k šifrování tajných kódů zabalení před zápisem do služby Key Vault. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = ‘MyExtraSecureVM’;
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Ověřte, zda že jsou zašifrovány disky:** Chcete-li zkontrolovat stav šifrování virtuálního počítače IaaS, použijte [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) rutiny. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
     ```
    
- **Zakážete šifrování disku:** Pokud chcete zakázat šifrování, použijte [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) rutiny. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Povoluje šifrování na existující nebo spouštění virtuálních počítačů pomocí Azure CLI
Použití [az vm encryption povolit](/cli/azure/vm/encryption#az-vm-encryption-enable) příkaz, který povoluje šifrování na spuštěný virtuální počítač IaaS v Azure.

-  **Šifrování spuštěného virtuálního počítače pomocí tajného klíče klienta:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Šifrování spuštěného virtuálního počítače pomocí KEK zabalit tajný kód klienta:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Ověřte, zda že jsou zašifrovány disky:** Chcete-li zkontrolovat stav šifrování virtuálního počítače IaaS, použijte [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) příkazu. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Zakážete šifrování:** Pokud chcete zakázat šifrování, použijte [az vm encryption zakázat](/cli/azure/vm/encryption#az-vm-encryption-disable) příkazu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Tento příkaz se nezdaří ve virtuálních počítačích spravovaných disků na základě, dokud byly provedeny zálohy a byl zadán tento parametr. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 

### <a name="bkmk_RunningWinVMwRM"> </a>Pomocí šablony Resource Manageru
Můžete povolit šifrování disku v existující nebo běžící virtuální počítače IaaS s Windows v Azure s použitím [šablony Resource Manageru k šifrování spuštěného virtuálního počítače Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).


1. Na šablony Azure pro rychlý start, klikněte na tlačítko **nasadit do Azure**.

2. Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, parametry, právní podmínky a smlouvy. Klikněte na tlačítko **nákupní** chcete povolit šifrování na existující nebo spuštěného virtuálního počítače IaaS.

V následující tabulce jsou uvedeny parametry šablony Resource Manageru pro existující nebo spouštění virtuálních počítačů, které používají ID klienta Azure AD:

| Parametr | Popis |
| --- | --- |
| AADClientID | ID klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do služby key vault. |
| AADClientSecret | Tajný kód klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do služby key vault. |
| keyVaultName | Název, který klíč Bitlockeru, musí být nahrán do trezoru klíčů. Můžete ho získat pomocí rutiny `(Get-AzKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` nebo příkazového řádku Azure `az keyvault list --resource-group "MySecureGroup" |Convertfrom-JSON`|
|  KeyEncryptionKeyURL | Adresa URL šifrovací klíč klíče, který se používá k šifrování vygenerovaný klíč Bitlockeru. Tento parametr je nepovinný, pokud vyberete **nokek** v rozevíracím seznamu UseExistingKek. Pokud vyberete **kek** v rozevíracím seznamu UseExistingKek, je nutné zadat _keyEncryptionKeyURL_ hodnotu. |
| VolumeType | Typ svazku, který provádí operace šifrování na. Platné hodnoty jsou _OS_, _Data_, a _všechny_. |
| SequenceVersion | Pořadí verze Bitlockeru operace. Toto číslo verze postupně zvyšuje vždy, když je provedena operace šifrování disku ve stejném virtuálním počítači. |
| vmName | Název virtuálního počítače, který má být proveden na operace šifrování. |


## <a name="bkmk_VHDpre"> </a>Nové virtuální počítače IaaS vytvořené z šifrované zákazníka virtuální pevný disk, šifrovacích klíčů
V tomto scénáři můžete povolit šifrování pomocí šablony Resource Manageru, rutin prostředí PowerShell nebo příkazů rozhraní příkazového řádku. Následující části popisují podrobněji šablonu Resource Manageru a příkazech rozhraní příkazového řádku. 

Postupujte podle pokynů v dodatku k přípravě předem šifrované imagí, které lze použít v Azure. Po vytvoření image můžete použít kroky v další části vytvořit šifrovaný virtuální počítač Azure.

* [Příprava virtuálního pevného disku předem šifrované Windows](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Připravit předem šifrované linuxového virtuálního pevného disku](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Příkaz Set-AzVMDiskEncryptionExtension selže ve virtuálních počítačích spravovaných disků na základě dokud provedl zálohování a tento parametr nebyl zadán. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 



### <a name="bkmk_VHDprePSH"> </a> Šifrování virtuálních počítačů s předem šifrované virtuální pevné disky pomocí Azure Powershellu
Můžete povolit šifrování disku na virtuální pevný disk šifrovaný pomocí rutiny prostředí PowerShell [Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples). Následující příklad obsahuje některé společné parametry. 

```powershell
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MySecureRG"
```


### <a name="bkmk_VHDpreRM"> </a>Šifrování virtuálních počítačů IaaS s předem šifrované virtuálních pevných disků pomocí šablony Resource Manageru 
Můžete povolit šifrování disku na virtuální pevný disk šifrovaný pomocí [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Na šablony Azure pro rychlý start, klikněte na tlačítko **nasadit do Azure**.

2. Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, parametry, právní podmínky a smlouvy. Klikněte na tlačítko **vytvořit** chcete povolit šifrování na nový virtuální počítač IaaS.

Následující tabulka uvádí parametry šablony Resource Manageru pro šifrované virtuální pevný disk:

| Parametr | Popis |
| --- | --- |
| newStorageAccountName | Název účtu úložiště pro uložení šifrovaného virtuálního pevného disku operačního systému. Tento účet úložiště by měl již byly vytvořeny ve stejné skupině prostředků a stejném umístění jako virtuální počítač. |
| osVhdUri | Identifikátor URI virtuálního pevného disku operačního systému z účtu úložiště. |
| osType | Typ produktu operačního systému (Windows/Linux). |
| virtualNetworkName | Název virtuální sítě, síťové karty virtuálního počítače by měly patřit do. Název by měl již byly vytvořeny ve stejné skupině prostředků a stejném umístění jako virtuální počítač. |
| subnetName | Název podsítě ve virtuální síti, která síťové karty virtuálního počítače by měly patřit do. |
| vmSize | Velikost virtuálního počítače. V současné době jsou podporovány pouze standardní A, D a G series. |
| keyVaultResourceID | ID prostředku, který identifikuje prostředek trezoru klíčů v Azure Resource Manageru. Můžete ho získat pomocí rutiny prostředí PowerShell `(Get-AzKeyVault -VaultName &lt;MyKeyVaultName&gt; -ResourceGroupName &lt;MyResourceGroupName&gt;).ResourceId` nebo pomocí příkazového řádku Azure `az keyvault show --name "MySecureVault" --query id`|
| keyVaultSecretUrl | Adresa URL klíče šifrování disku, který je nastavený v trezoru klíčů. |
| keyVaultKekUrl | Adresa URL šifrovací klíč klíče pro šifrování klíč vygenerovaný šifrování disku. |
| vmName | Název virtuálního počítače IaaS. |

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Povoluje šifrování na nově přidaných datového disku
Je možné [přidejte nový disk k virtuálnímu počítači s Windows pomocí Powershellu](../virtual-machines/windows/attach-disk-ps.md), nebo [prostřednictvím webu Azure portal](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Povoluje šifrování na nově přidaný disk pomocí Azure Powershellu
 Při použití Powershellu k šifrování nový disk pro virtuální počítače s Windows, musí být zadaný novou verzi pořadí. Pořadí verze musí být jedinečný. Níže uvedený skript vytvoří identifikátor GUID verze pořadí. V některých případech může být disk nově přidaná data automaticky šifrovat pomocí rozšíření Azure Disk Encryption. Automatické šifrování obvykle dochází, když virtuální počítač restartuje po nový disk převede do režimu online. Důvodem je obvykle "All" nebyly zadány pro typ svazku při šifrování disku spustili dříve ve virtuálním počítači. Pokud automatické šifrování probíhá na nově přidaných datový disk, doporučujeme znovu spustit rutinu Set-AzVmDiskEncryptionExtension s novou verzí pořadí. Pokud nový datový disk se automaticky zašifrovaná a nechcete, aby šifrování, dešifrování všech jednotkách nejprve a potom znovu zašifrovat pomocí nové verze pořadí zadání typu svazku operačního systému. 
 

-  **Šifrování spuštěného virtuálního počítače pomocí tajného klíče klienta:** Níže uvedený skript inicializuje proměnných a spustí rutinu Set-AzVMDiskEncryptionExtension. Skupina prostředků, virtuální počítač, trezor klíčů, aplikace AAD a tajný kód klienta by měl již byly vytvořeny jako požadavky. Nahraďte hodnoty MySecureRg MySecureVM, MySecureVault, My-AAD-client-ID a My-AAD-client-secret. Tento příklad používá "All" pro parametr - VolumeType, který obsahuje operační systém a datové svazky. Pokud chcete pouze k šifrování svazku operačního systému, použijte pro parametr - VolumeType "OS". 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;
    ```
- **Šifrování spuštěného virtuálního počítače pomocí KEK zabalit tajný kód klienta:** Azure Disk Encryption umožňuje určit existujícího klíče v trezoru klíčů při zabalení šifrování tajných kódů disku, které byly generovány při povolení šifrování. Pokud je zadaný šifrovací klíč klíče, Azure Disk Encryption používá tento klíč k šifrování tajných kódů zabalení před zápisem do služby Key Vault. Tento příklad používá "All" pro parametr - VolumeType, který obsahuje operační systém a datové svazky. Pokud chcete pouze k šifrování svazku operačního systému, použijte pro parametr - VolumeType "OS". 

     ```azurepowershell-interactive
     $sequenceVersion = [Guid]::NewGuid();
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'all' –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Povoluje šifrování na nově přidaný disk pomocí Azure CLI
  Pomocí příkazu Azure CLI automaticky poskytne nové verze pořadí, při spuštění příkazu povolit šifrování. Přijatelné hodnoty pro parametr svazku yype jsou všechny, operačním systémem a daty. Budete muset změňte parametr typ svazku operačního systému nebo Data, pokud který šifrujete pouze jeden typ disku virtuálního počítače. V příkladech se používá "All" pro parametr typu svazku. 

-  **Šifrování spuštěného virtuálního počítače pomocí tajného klíče klienta:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **Šifrování spuštěného virtuálního počítače pomocí KEK zabalit tajný kód klienta:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "all"
     ```


## <a name="enable-encryption-using-azure-ad-client-certificate-based-authentication"></a>Povolte šifrování pomocí ověřování založeného na certifikátu klienta služby Azure AD.
Ověření klientského certifikátu můžete použít s nebo bez certifikátu KEK. Tyto skripty vyžadují, aby [požadavky Azure Disk Encryption](azure-security-disk-encryption-prerequisites-aad.md) se dokončí. Než začnete používat skripty prostředí PowerShell, byste už měli mít certifikát odeslán do trezoru klíčů a nasazené do virtuálního počítače. Pokud používáte KEK příliš, se klíč KEK by měl existovat. Další informace najdete v tématu [ověřování pomocí certifikátu pro službu Azure AD](azure-security-disk-encryption-prerequisites-aad.md#bkmk_Cert) požadavky článku.


### <a name="enable-encryption-using-certificate-based-authentication-with-azure-powershell"></a>Povolit šifrování pomocí ověřování prostřednictvím certifikátu pomocí Azure Powershellu

```powershell
## Fill in 'MySecureRg', 'My-AAD-client-ID', 'MySecureVault, and ‘MySecureVM’.

$rgName = 'MySecureRg';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = ‘MySecureVM’;
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;

# Fill in the certificate path and the password so the thumbprint can be set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
```
  
### <a name="enable-encryption-using-certificate-based-authentication-and-a-kek-with-azure-powershell"></a>Povolit šifrování pomocí ověřování prostřednictvím certifikátu a KEK pomocí Azure Powershellu

```powershell
# Fill in 'MySecureRg', 'My-AAD-client-ID', 'MySecureVault,, 'MySecureVM’, and "KEKName.

$rgName = 'MySecureRg';
$AADClientID ='My-AAD-client-ID';
$KeyVaultName = 'MySecureVault';
$VMName = 'MySecureVM';
$keyEncryptionKeyName ='KEKName';
$diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
$KeyVaultResourceId = $KeyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

## Fill in the certificate path and the password so the thumbprint can be read and set as a variable. 

$certPath = '$CertPath = "C:\certificates\mycert.pfx';
$CertPassword ='Password'
$Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
$aadClientCertThumbprint = $cert.Thumbprint;

# Enable disk encryption using the client certificate thumbprint and a KEK

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

## <a name="disable-encryption"></a>Zakázat šifrování
Můžete zakázat šifrování pomocí Azure Powershellu, rozhraní příkazového řádku Azure nebo pomocí šablony Resource Manageru. 

- **Zakážete disk encryption pomocí Azure Powershellu:** Pokud chcete zakázat šifrování, použijte [Disable-AzureRmVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) rutiny. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

- **Zakážete šifrování pomocí Azure CLI:** Pokud chcete zakázat šifrování, použijte [az vm encryption zakázat](/cli/azure/vm/encryption#az-vm-encryption-disable) příkazu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Zakážete šifrování pomocí šablony Resource Manageru:** 

    1. Klikněte na tlačítko **nasadit do Azure** z [zakázat šifrování disku ve spuštění virtuálního počítače s Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm) šablony.
    2. Vyberte předplatné, skupinu prostředků, umístění, virtuálních počítačů, právní podmínky a smlouvy.
    3.  Klikněte na tlačítko **nákupní** můžete zakázat šifrování disků spuštěného virtuálního počítače Windows. 

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Aktivace Azure Disk Encryption pro Linux](azure-security-disk-encryption-linux-aad.md)
