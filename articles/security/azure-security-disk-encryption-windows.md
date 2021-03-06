---
title: Aktivace Azure Disk Encryption pro virtuální počítače IaaS s Windows
description: Tento článek obsahuje pokyny týkající se povolení Microsoft Azure Disk Encryption pro Windows virtuální počítače IaaS.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 1/31/2019
ms.custom: seodec18
ms.openlocfilehash: a5a42f544fa2552dc7bc85b4bee67925291bef00
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992410"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms"></a>Aktivace Azure Disk Encryption pro virtuální počítače IaaS s Windows

Můžete povolit řadu scénářů šifrování disku a kroků může lišit v závislosti scénáři. Následující části se věnují scénáře podrobněji pro virtuální počítače IaaS s Windows. Než budete moct použít šifrování disku, [požadavky Azure Disk Encryption](../security/azure-security-disk-encryption-prerequisites.md) musíte provést. 

Přijmout [snímku](../virtual-machines/windows/snapshot-copy-managed-disk.md) nebo před disky jsou šifrované zálohovat. Zálohy Ujistěte se, že možnost obnovení je možné, pokud dojde k neočekávané chybě při šifrování. Virtuální počítače se spravovanými disky vyžadují zálohu, než dojde k šifrování. Po provedení zálohy můžete použít rutinu Set-AzVMDiskEncryptionExtension zadáním parametru - skipVmBackup šifrování spravované disky. Další informace o tom, jak zálohování a obnovení šifrovaných virtuálních počítačů najdete v tématu [Azure Backup](../backup/backup-azure-vms-encryption.md) článku. 

>[!WARNING]
> - Pokud jste už dřív použili [Azure Disk Encryption pomocí Azure AD app](azure-security-disk-encryption-prerequisites-aad.md) pro šifrování tento virtuální počítač, budete muset pokračovat tuto možnost použijte k šifrování virtuálního počítače. Nemůžete použít [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) na tento šifrovaný virtuální počítač jako tato akce není podporovaný scénář význam přepnutí mimo aplikaci AAD pro tento šifrovaný virtuální počítač se zatím nepodporuje. 
> - Azure Disk Encryption musí být umístěné ve stejné oblasti služby Key Vault a virtuální počítače. Vytvoření a použití služby Key Vault, která je ve stejné oblasti jako virtuální počítač k šifrování. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_RunningWinVM"></a> Povoluje šifrování na existující nebo spouštění virtuálních počítačů IaaS s Windows
V tomto scénáři můžete povolit šifrování pomocí šablony, rutin prostředí PowerShell nebo příkazů rozhraní příkazového řádku. Následující části podrobněji popisují jak povolit Azure Disk Encryption. Pokud potřebujete informace o schématu pro rozšíření virtuálního počítače, přečtěte si [rozšíření Azure Disk Encryption pro Windows](../virtual-machines/extensions/azure-disk-enc-windows.md) článku.

>[!IMPORTANT]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Příkaz Set-AzVMDiskEncryptionExtension selže ve virtuálních počítačích spravovaných disků na základě dokud provedl zálohování a tento parametr nebyl zadán. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Povoluje šifrování na existující nebo spouštění virtuálních počítačů pomocí Azure Powershellu 
Použití [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension) rutina pro povolení šifrování u spuštěného virtuálního počítače IaaS v Azure. 

-  **Šifrování spuštěného virtuálního počítače:** Níže uvedený skript inicializuje proměnných a spustí rutinu Set-AzVMDiskEncryptionExtension. Skupinu prostředků, virtuální počítač a trezor klíčů by již byly vytvořeny jako požadavky. Nahraďte hodnoty MySecureRg MySecureVM a MySecureVault.

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Šifrování pomocí KEK spuštěného virtuálního počítače:** 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Ověřte, zda že jsou zašifrovány disky:** Chcete-li zkontrolovat stav šifrování virtuálního počítače IaaS, použijte [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) rutiny. 
     ```azurepowershell-interactive
     Get-AzVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
     ```
    
- **Zakážete šifrování disku:** Pokud chcete zakázat šifrování, použijte [zakázat AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) rutiny. Když zašifrovali operačního systému a datové disky zakazuje šifrování disku na virtuálním počítači Windows nebude fungovat podle očekávání. Zakážete šifrování na všech discích místo.

     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Povoluje šifrování na existující nebo spouštění virtuálních počítačů pomocí Azure CLI
Použití [az vm encryption povolit](/cli/azure/vm/encryption#az-vm-encryption-enable) příkaz, který povoluje šifrování na spuštěný virtuální počítač IaaS v Azure.

-  **Šifrování spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Šifrování pomocí KEK spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Ověřte, zda že jsou zašifrovány disky:** Chcete-li zkontrolovat stav šifrování virtuálního počítače IaaS, použijte [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) příkazu. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Zakážete šifrování:** Pokud chcete zakázat šifrování, použijte [az vm encryption zakázat](/cli/azure/vm/encryption#az-vm-encryption-disable) příkazu. Když zašifrovali operačního systému a datové disky zakazuje šifrování disku na virtuálním počítači Windows nebude fungovat podle očekávání. Zakážete šifrování na všech discích místo.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Tento příkaz se nezdaří ve virtuálních počítačích spravovaných disků na základě, dokud byly provedeny zálohy a byl zadán tento parametr. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 

### <a name="bkmk_RunningWinVMwRM"> </a>Pomocí šablony Resource Manageru
Můžete povolit šifrování disku v existující nebo běžící virtuální počítače IaaS s Windows v Azure s použitím [šablony Resource Manageru k šifrování spuštěného virtuálního počítače Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad).


1. Na šablony Azure pro rychlý start, klikněte na tlačítko **nasadit do Azure**.

2. Vyberte předplatné, skupinu prostředků, umístění, nastavení, právní podmínky a smlouvy. Klikněte na tlačítko **nákupní** chcete povolit šifrování na existující nebo spuštěného virtuálního počítače IaaS.

V následující tabulce jsou uvedeny parametry šablony Resource Manageru pro existující nebo spouštění virtuálních počítačů:

| Parametr | Popis |
| --- | --- |
| vmName | Název virtuálního počítače ke spuštění operace šifrování. |
| keyVaultName | Název, který klíč Bitlockeru, musí být nahrán do trezoru klíčů. Můžete ho získat pomocí rutiny `(Get-AzKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` nebo příkazového řádku Azure `az keyvault list --resource-group "MySecureGroup" |ConvertFrom-JSON`|
| keyVaultResourceGroup | Název skupiny prostředků obsahující trezor klíčů|
|  KeyEncryptionKeyURL | Adresa URL šifrovací klíč klíče, který se používá k šifrování vygenerovaný klíč Bitlockeru. Tento parametr je nepovinný, pokud vyberete **nokek** v rozevíracím seznamu UseExistingKek. Pokud vyberete **kek** v rozevíracím seznamu UseExistingKek, je nutné zadat _keyEncryptionKeyURL_ hodnotu. |
| VolumeType | Typ svazku, který provádí operace šifrování na. Platné hodnoty jsou _OS_, _Data_, a _všechny_. 
| forceUpdateTag | Pokaždé, když se operace musí být vynutit spuštění se předá jedinečnou hodnotu jako identifikátor GUID. |
| resizeOSDisk | Oddíl operačního systému velikost tak, aby obsadily úplný operační systém virtuálního pevného disku před rozdělením systémový svazek. |
| location | Umístění pro všechny prostředky. |

## <a name="encrypt-virtual-machine-scale-sets"></a>Šifrování škálovací sady virtuálních počítačů

[Škálovací sady virtuálních počítačů Azure](../virtual-machine-scale-sets/overview.md) umožňují vytvářet a spravovat skupiny identických, virtuálních počítačů s vyrovnáváním zatížení. Počet instancí virtuálních počítačů se může automaticky zvyšovat nebo snižovat s ohledem na požadavky nebo definovaný plán. K šifrování škálovací sady virtuálních počítačů pomocí Azure Powershellu nebo rozhraní příkazového řádku.


### <a name="register-for-disk-encryption-preview-using-azure-powershell"></a>Zaregistrujte se ve verzi preview šifrování disku pomocí Azure Powershellu

Azure disk encryption pro škálovací sady virtuálních počítačů ve verzi preview je potřeba registrovat předplatné s [Register-AzProviderFeature](/powershell/module/az.resources/register-azproviderfeature). Stačí provést následující kroky při prvním použití funkce ve verzi preview šifrování disku:

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Může trvat až 10 minut žádost o registraci rozšíření. Vy můžete zkontrolovat stav registrace s [Get-AzProviderFeature](/powershell/module/az.Resources/Get-azProviderFeature). Když `RegistrationState` sestavy *registrované*, znovu zaregistrovat *Microsoft.Compute* zprostředkovatele s [Register-AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider):

```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Šifrování virtuálního počítače škálovací sady pomocí Azure Powershellu

Použití [Set-AzVmssDiskEncryptionExtension](/powershell/module/az.compute/set-azvmssdiskencryptionextension) rutiny, které chcete povolit šifrování na škálovací sadu virtuálních počítačů s Windows. Skupinu prostředků, virtuální počítač a trezor klíčů by již byly vytvořeny jako požadavky.

-  **Šifrování běžící škálovací sady virtuálních počítačů**:
    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:

    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $KeyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     Set-AzRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $KeyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

   >[!NOTE]
   > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Načíst stav šifrování pro škálovací sadu virtuálních počítačů:** Použití [Get-AzRmVmssVMDiskEncryption](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption) rutiny.
    
    ```azurepowershell-interactive
    get-AzVmssVMDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

- **Zakažte šifrování u škálovací sady virtuálních počítačů**: Použití [zakázat AzVmssDiskEncryption](/powershell/module/az.compute/disable-azvmssdiskencryption) rutiny. 

    ```azurepowershell-interactive
    Disable-AzVmssDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

### <a name="register-for-disk-encryption-preview-using-azure-cli"></a>Zaregistrujte se ve verzi preview šifrování disku pomocí rozhraní příkazového řádku Azure

Azure disk encryption pro škálovací sady virtuálních počítačů ve verzi preview je potřeba registrovat předplatné s [az funkce register](/cli/azure/feature#az-feature-register). Stačí provést následující kroky při prvním použití funkce ve verzi preview šifrování disku:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Může trvat až 10 minut žádost o registraci rozšíření. Vy můžete zkontrolovat stav registrace s [az funkce Zobrazit](/cli/azure/feature#az-feature-show). Když `State` sestavy *registrované*, přeregistrovat *Microsoft.Compute* zprostředkovatele s [az provider register](/cli/azure/provider#az-provider-register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```



###  <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Šifrování virtuálního počítače škálovací sady pomocí Azure CLI

Použití [povolit šifrování az vmss](/cli/azure/vmss/encryption#az-vmss-encryption-enable) chcete povolit šifrování na škálovací sadu virtuálních počítačů s Windows. Pokud nastavíte zásady upgradu pro škálovací sady na ruční, spusťte šifrování s [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). Skupinu prostředků, virtuální počítač a trezor klíčů by již byly vytvořeny jako požadavky.

-  **Šifrování běžící škálovací sady virtuálních počítačů**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Šifrování spuštěného virtuálního počítače škálovací sady s použitím KEK zabalit klíč**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
     
   >[!NOTE]
   > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Načíst stav šifrování pro škálovací sadu virtuálních počítačů:** Použití [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show)

    ```azurecli-interactive
     az vmss encryption show --resource-group "MySecureRG" --name "MySecureVmss"
    ```

- **Zakažte šifrování u škálovací sady virtuálních počítačů**: Použití [zakázat šifrování az vmss](/cli/azure/vmss/encryption#az-vmss-encryption-disable)
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MySecureRG" --name "MySecureVmss"
    ```

### <a name="azure-resource-manager-templates-for-windows-virtual-machine-scale-sets"></a>Nastaví šablony Azure Resource Manageru pro škálování virtuálního počítače Windows

Šifrování nebo dešifrování Windows virtuálního počítače škálovací sady, pomocí šablon Azure Resource Manageru a následující pokyny:

- [Povoluje šifrování na škálovací sadu virtuálních počítačů s Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)
- [Nasazení virtuálního počítače škálovací sady virtuálních počítačů Windows s jumpbox a povoluje šifrování na škálovací sady virtuálních počítačů Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)
- [Zakažte šifrování u škálovací sady virtuálních počítačů Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

     1. Klikněte na **Deploy to Azure** (Nasadit do Azure).
     2. Vyplňte požadovaná pole pak svůj souhlas s podmínkami a ujednáními.
     3. Klikněte na tlačítko **nákupní** k nasazení šablony.

## <a name="bkmk_VHDpre"> </a> Nové virtuální počítače IaaS vytvořené z šifrované zákazníka virtuální pevný disk, šifrovacích klíčů

V tomto scénáři můžete povolit šifrování pomocí rutin prostředí PowerShell nebo příkazů rozhraní příkazového řádku. 

Postupujte podle pokynů v dodatku k přípravě předem šifrované imagí, které lze použít v Azure. Po vytvoření image můžete použít kroky v další části vytvořit šifrovaný virtuální počítač Azure.

* [Příprava virtuálního pevného disku předem šifrované Windows](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Připravit předem šifrované linuxového virtuálního pevného disku](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Příkaz Set-AzVMDiskEncryptionExtension selže ve virtuálních počítačích spravovaných disků na základě dokud provedl zálohování a tento parametr nebyl zadán. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 


### <a name="bkmk_VHDprePSH"> </a> Šifrování virtuálních počítačů s předem šifrované virtuální pevné disky pomocí Azure Powershellu
Můžete povolit šifrování disku na virtuální pevný disk šifrovaný pomocí rutiny prostředí PowerShell [Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk#examples). Následující příklad obsahuje některé společné parametry. 

```azurepowershell-interactive
$VirtualMachine = New-AzVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzVM -VM $VirtualMachine -ResourceGroupName "MySecureRG"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Povoluje šifrování na nově přidaných datového disku
Je možné [přidejte nový disk k virtuálnímu počítači s Windows pomocí Powershellu](../virtual-machines/windows/attach-disk-ps.md), nebo [prostřednictvím webu Azure portal](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Povoluje šifrování na nově přidaný disk pomocí Azure Powershellu
 Při použití Powershellu k šifrování nový disk pro virtuální počítače s Windows, musí být zadaný novou verzi pořadí. Pořadí verze musí být jedinečný. Níže uvedený skript vytvoří identifikátor GUID verze pořadí. V některých případech může být disk nově přidaná data automaticky šifrovat pomocí rozšíření Azure Disk Encryption. Automatické šifrování obvykle dochází, když virtuální počítač restartuje po nový disk převede do režimu online. Důvodem je obvykle "All" nebyly zadány pro typ svazku při šifrování disku spustili dříve ve virtuálním počítači. Pokud automatické šifrování probíhá na nově přidaných datový disk, doporučujeme znovu spustit rutinu Set-AzVmDiskEncryptionExtension s novou verzí pořadí. Pokud nový datový disk se automaticky zašifrovaná a nechcete, aby šifrování, dešifrování všech jednotkách nejprve a potom znovu zašifrovat pomocí nové verze pořadí zadání typu svazku operačního systému. 
  
 

-  **Šifrování spuštěného virtuálního počítače:** Níže uvedený skript inicializuje proměnných a spustí rutinu Set-AzVMDiskEncryptionExtension. Skupinu prostředků, virtuální počítač a trezor klíčů by již byly vytvořeny jako požadavky. Nahraďte hodnoty MySecureRg MySecureVM a MySecureVault. Tento příklad používá "All" pro parametr - VolumeType, který obsahuje operační systém a datové svazky. Pokud chcete pouze k šifrování svazku operačního systému, použijte pro parametr - VolumeType "OS". 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;
    ```
- **Šifrování pomocí KEK spuštěného virtuálního počítače:** Tento příklad používá "All" pro parametr - VolumeType, který obsahuje operační systém a datové svazky. Pokud chcete pouze k šifrování svazku operačního systému, použijte pro parametr - VolumeType "OS".

     ```azurepowershell-interactive
     $sequenceVersion = [Guid]::NewGuid();
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType "All" –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Povoluje šifrování na nově přidaný disk pomocí Azure CLI
 Pomocí příkazu Azure CLI automaticky poskytne nové verze pořadí, při spuštění příkazu povolit šifrování. V příkladu "All" pro parametr typu svazku. Budete muset změnit parametr typu svazku operačního systému, pokud máte pouze šifrování disku s operačním systémem. Na rozdíl od syntaxe Powershellu rozhraní příkazového řádku, aby uživatele k zadání verze jedinečný pořadí při povolení šifrování. Rozhraní příkazového řádku automaticky generuje a používá svůj vlastní jedinečný pořadí hodnotu verze.   

-  **Šifrování spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "All"
     ```

- **Šifrování pomocí KEK spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "All"
     ```


## <a name="disable-encryption"></a>Zakázat šifrování
Můžete zakázat šifrování pomocí Azure Powershellu, rozhraní příkazového řádku Azure nebo pomocí šablony Resource Manageru. Když zašifrovali operačního systému a datové disky zakazuje šifrování disku na virtuálním počítači Windows nebude fungovat podle očekávání. Zakážete šifrování na všech discích místo.

- **Zakážete disk encryption pomocí Azure Powershellu:** Pokud chcete zakázat šifrování, použijte [zakázat AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption) rutiny. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM' -VolumeType "all"
     ```

- **Zakážete šifrování pomocí Azure CLI:** Pokud chcete zakázat šifrování, použijte [az vm encryption zakázat](/cli/azure/vm/encryption#az-vm-encryption-disable) příkazu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type "all"
     ```
- **Zakážete šifrování pomocí šablony Resource Manageru:** 

    1. Klikněte na tlačítko **nasadit do Azure** z [zakázat šifrování disku ve spuštění virtuálního počítače s Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) šablony.
    2. Vyberte předplatné, skupinu prostředků, umístění, virtuálního počítače, typ svazku, právní podmínky a smlouvy.
    3.  Klikněte na tlačítko **nákupní** můžete zakázat šifrování disků spuštěného virtuálního počítače Windows. 

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Aktivace Azure Disk Encryption pro Linux](azure-security-disk-encryption-linux.md)
