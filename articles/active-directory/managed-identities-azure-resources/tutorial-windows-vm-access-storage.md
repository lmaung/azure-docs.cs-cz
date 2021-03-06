---
title: Použití spravované identity přiřazené systémem na virtuálním počítači s Windows pro přístup k Azure Storage
description: Tento kurz vás postupně provede používáním spravované identity přiřazené systémem na virtuálním počítači s Windows pro přístup k Azure Storage.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2019
ms.author: priyamo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a5b5b2e50950967b1cb8ae1aa22587244ad67a8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203926"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-storage-via-access-key"></a>Kurz: Použití spravované identity systém přiřadil virtuálního počítače Windows pro přístup k úložišti Azure přes přístupový klíč

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

V tomto kurzu se dozvíte, jak pomocí spravované identity přiřazené systémem pro virtuální počítač s Windows načíst přístupové klíče k účtu úložiště. Přístupové klíče k úložišti můžete použít obvyklým způsobem při provádění operací úložiště, například při použití sady SDK služby Storage. Pro účely tohoto kurzu nahrajeme a stáhneme objekty blob pomocí PowerShellu služby Azure Storage. V tomto kurzu se naučíte:


> [!div class="checklist"]
> * vytvořit účet úložiště
> * Udělit virtuálnímu počítači přístup k přístupovým klíčům účtu úložiště v Resource Manageru. 
> * Získat přístupový token pomocí identity virtuálního počítače a použít ho k načtení přístupových klíčů k úložišti z Resource Manageru. 

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-a-storage-account"></a>vytvořit účet úložiště 

Teď vytvoříte účet úložiště (pokud ho ještě nemáte). Tento krok také můžete přeskočit a udělit spravované identitě přiřazené systémem virtuálního počítače přístup ke klíčům stávajícího účtu úložiště. 

1. V levém horním rohu na webu Azure Portal klikněte na tlačítko pro **vytvoření nové služby**.
2. Klikněte na **Úložiště** a potom na **Účet úložiště**. Zobrazí se nový panel Vytvořit účet úložiště.
3. Zadejte název tohoto účtu úložiště, který použijete později.  
4. V polích **Model nasazení** a **Druh účtu** nastavte Resource manager a Pro obecné účely (v uvedeném pořadí). 
5. Ověřte, že pole **Předplatné** a **Skupina prostředků** se shodují s údaji zadanými při vytvoření virtuálního počítače v předchozím kroku.
6. Klikněte na možnost **Vytvořit**.

    ![Vytvoření nového účtu úložiště](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Vytvoření kontejneru objektů blob v účtu úložiště

Později nahrajeme a stáhneme soubor do nového účtu úložiště. Soubory vyžadují úložiště objektů blob. Proto potřebujeme vytvořit kontejner objektů blob, do kterého soubor uložíme.

1. Přejděte zpět k nově vytvořenému účtu úložiště.
2. Nalevo pod položkou „Blob service“ klikněte na odkaz **Kontejnery**.
3. Když nahoře na stránce kliknete na **+ Kontejner**, vysune se panel Nový kontejner.
4. Pojmenujte kontejner, vyberte úroveň přístupu a klikněte na **OK**. Zadaný název použijeme v další části tohoto kurzu. 

    ![Vytvoření kontejneru úložiště](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-system-assigned-managed-identity-access-to-use-storage-account-access-keys"></a>Udělení přístupu pro použití přístupových klíčů k účtu úložiště spravované identitě přiřazené systémem na virtuálním počítači 

Azure Storage nativně nepodporuje ověřování Azure AD.  Spravovanou identitu přiřazenou systémem na virtuálním počítači ale můžete použít k načtení přístupových klíčů k účtu úložiště z Resource Manageru a pak klíč použít pro přístup k úložišti.  V tomto kroku udělíte spravované identitě přiřazené systémem na virtuálním počítači přístup ke klíčům k vašemu účtu úložiště.   

1. Přejděte zpět k nově vytvořenému účtu úložiště.  
2. Na panelu vlevo klikněte na odkaz **Řízení přístupu (IAM)**.  
3. Klikněte na tlačítko **+ přidat přiřazení role** nad stránky a přidat nové přiřazení role pro váš virtuální počítač
4. Na pravé straně stránky nastavte položku **Role** na Role služby Operátor klíčů účtů úložiště. 
5. V dalším rozevíracím seznamu **Přiřadit přístup k** nastavte prostředek na Virtuální počítač.  
6. Potom se ujistěte, že v rozevíracím seznamu **Předplatné** je správné předplatné, a nastavte **Skupinu prostředků** na Všechny skupiny prostředků.  
7. Nakonec **vyberte** v rozevíracím seznamu svůj virtuální počítač s Windows a klikněte na **Uložit**. 

    ![Text k alternativnímu obrázku](./media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-system-assigned-managed-identity-to-call-azure-resource-manager"></a>Získání přístupového tokenu pomocí spravované identity přiřazené systémem virtuálního počítače k volání Azure Resource Manageru 

Ve zbývající části kurzu použijeme k práci dříve vytvořený virtuální počítač. 

V této části budete muset používat rutiny prostředí PowerShell pro Azure Resource Manager.  Než budete pokračovat, [stáhněte si nejnovější verzi](https://docs.microsoft.com/powershell/azure/overview) (v případě, že ho nemáte nainstalovaný).

1. Na webu Azure Portal přejděte na **Virtuální počítače**, přejděte ke svému virtuálnímu počítači s Windows a potom nahoře na stránce **Přehled** klikněte na **Připojit**. 
2. Zadejte své **uživatelské jméno** a **heslo**, které jste přidali při vytváření virtuálního počítače s Windows. 
3. Teď, když jste vytvořili **připojení ke vzdálené ploše** virtuálního počítače, otevřete ve vzdálené relaci PowerShell.
4. Pomocí příkazu Invoke-WebRequest v PowerShellu požádejte místní spravovanou identitu o koncový bod prostředků Azure k získání přístupového tokenu pro Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > Hodnota parametru „resource“ musí přesně odpovídat hodnotě, kterou očekává Azure AD. Při použití ID prostředku Azure Resource Manageru musí být v identifikátoru URI koncové lomítko.
    
    V dalším kroku extrahujte element Content, který je uložený jako formátovaný řetězec JSON (JavaScript Object Notation) v objektu $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Potom z odpovědi extrahujte přístupový token.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Získání přístupových klíčů k účtu úložiště z Azure Resource Manageru kvůli volání úložiště  

Teď použijte PowerShell k volání Resource Manageru. Použijte přístupový token, který jste načetli v předchozí části, a načtěte přístupový klíč k úložišti. Jakmile máte přístupový klíč k úložišti, můžete volat operace nahrávání do úložiště nebo stahování z úložiště.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> V adrese URL se rozlišují velká a malá písmena. Proto zkontrolujte, jestli používáte přesně stejná velká a malá písmena, jaká jste použili při pojmenování skupiny prostředků, a zkontrolujte také velké G ve výrazu „resourceGroup“. 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Dále vytvoříme soubor s názvem test.txt. Potom použijeme přístupový klíč k úložišti k ověření pomocí rutiny `New-AzStorageContent`, nahrajeme soubor do kontejneru objektů blob a potom soubor stáhneme.

```bash
echo "This is a test text file." > test.txt
```

Nejprve je potřeba nainstalovat rutiny Azure Storage pomocí `Install-Module Az.Storage`. Potom nahrajte právě vytvořený objekt blob pomocí rutiny PowerShellu `Set-AzStorageBlobContent`:

```powershell
$ctx = New-AzStorageContext -StorageAccountName <STORAGE-ACCOUNT> -StorageAccountKey $key
Set-AzStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Odpověď:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Právě nahraný objekt blob si také můžete stáhnout pomocí rutiny PowerShellu `Get-AzStorageBlobContent`:

```powershell
Get-AzStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Odpověď:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se dozvěděli, jak vytvořit spravovanou identitu přiřazenou systémem pro přístup k Azure Storage pomocí přístupového klíče.  Další informace o přístupových klíčích Azure Storage:

> [!div class="nextstepaction"]
>[Správa přístupových klíčů k úložišti](/azure/storage/common/storage-create-storage-account)

