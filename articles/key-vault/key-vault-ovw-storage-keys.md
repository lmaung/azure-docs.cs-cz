---
ms.assetid: ''
title: Služba Azure Key Vault spravovat účet úložiště – rozhraní příkazového řádku
description: Klíče účtu úložiště poskytují bezproblémovou integraci Azure Key Vault a klíče přístupu na základě do účtu úložiště Azure.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: prashanthyv
ms.author: pryerram
manager: barbkess
ms.date: 10/03/2018
ms.openlocfilehash: 9b1a4e23ed0da0637b44ac52dd4d1baeb22cd6ce
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118050"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Služba Azure Key Vault spravovat účet úložiště – rozhraní příkazového řádku

> [!NOTE]
> [Integrace služby Azure storage s Azure Active Directory (Azure AD) je teď ve verzi preview](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Doporučujeme použít Azure AD pro ověřování a autorizaci, které poskytuje přístup na základě tokenu OAuth2 do úložiště Azure, stejně jako Azure Key Vault. To vám umožní:
> - Ověření klientské aplikace pomocí identitou aplikace nebo uživatele místo přihlašovacích údajů účtu úložiště. 
> - Použití [identita spravované služby Azure AD](/azure/active-directory/managed-identities-azure-resources/) při spouštění v Azure. Spravovat identity odebrat potřebné pro ověřování klientů všechno dohromady a ukládání přihlašovacích údajů v nebo s vaší aplikací.
> - Používejte na základě řízení přístupu Role (RBAC) pro správu autorizace, která je také podporována službou Key Vault.

- Azure Key Vault se spravuje klíče z Azure Storage účtu (ASA).
    - Azure Key Vault interně, můžete zobrazit seznam klíčů (sync) pomocí účtu služby Azure Storage.    
    - Služba Azure Key Vault obnoví (obmění) klíče pravidelně.
    - Hodnoty klíče se nikdy vrátí odpověď na volajícího.
    - Služba Azure Key Vault slouží ke správě klíčů účtů úložiště a klasické účty úložiště.
    
> [!IMPORTANT]
> Tenanta služby Azure AD poskytuje každý registrované aplikaci  **[instanční objekt služby](/azure/active-directory/develop/developer-glossary#service-principal-object)**, který slouží jako identitu aplikace. ID aplikace instančního objektu se používá při zadání autorizaci pro přístup k další prostředky Azure prostřednictvím řízení přístupu na základě role (RBAC). Protože Key Vault je aplikace Microsoft, je předem registrovánu ve všech tenantů Azure AD v rámci stejné ID aplikace v rámci každé cloudu Azure:
> - ID aplikace používat klienty Azure AD v cloudu Azure government `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - ID aplikace používat klienty Azure AD ve veřejném cloudu Azure a všechny ostatní `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

<a name="prerequisites"></a>Požadavky
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) instalace Azure CLI   
2. [Vytvoření účtu úložiště](https://azure.microsoft.com/services/storage/)
    - Postupujte podle kroků v tomto [dokumentu](https://docs.microsoft.com/azure/storage/) k vytvoření účtu úložiště  
    - **Pokyny pro pojmenování:** Názvy účtů úložiště musí mít délku 3 až 24 znaků a můžou obsahovat jenom číslice a malá písmena.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Krok pokyny o tom, jak používat ke správě klíčů účtu úložiště služby Key Vault
--------------------------------------------------------------------------------
Obecně jsou seznam kroků, kterými se nepoužijí
- Nejprve získáme (existující) účet úložiště
- My pak načíst (existující) služby key vault
- Potom přidáme účet služby KeyVault spravované úložiště do trezoru, nastavení Key1 jako aktivní klíč a opětovné vygenerování období trvajícího 180 dní
- Nakonec nastavíme kontext úložiště pro zadaný účet úložiště, s Key1

V následujících pokynů, jsme služby Key Vault přiřazování jako službu, která mají oprávnění operátora na vašem účtu úložiště

> [!NOTE]
> Prosím Všimněte si, že když jste nastavili Azure Key Vault spravovat účet úložiště klíčů, měli **ne** už může změnit pouze pomocí služby Key Vault. Spravované úložiště klíčů účtu znamená, že Key Vault spravovat obměně klíče účtu úložiště

1. Po vytvoření účtu úložiště, spuštěním následujícího příkazu Získejte ID prostředku účtu úložiště, kterou chcete spravovat

    ```
    az storage account show -n storageaccountname 
    ```
    Zkopírujte ID pole je mimo výsledek výše uvedeného příkazu
    
2. Získání ID objektu z Azure Key Vault služby hlavní spuštěním následující příkaz

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
    Po úspěšném dokončení tohoto příkazu najdete ID objektu ve výsledku:
    ```console
        {
            ...
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
            ...
        }
    ```
    
3. Přiřadíte roli operátora klíč úložiště pro Azure Key Vault Identity.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Vytvoření trezoru klíčů spravovaného účtu úložiště.     <br /><br />
   Níže jsme nastavujete opětovné generování uplynutí 90 dnů. Po 90 dnech se služby Key Vault znovu vygenerovat "key1" a Prohodit aktivní klíč z "key2" k "key1". Ho označí Key1 jako aktivní klíč teď. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```
    V případě uživatel nevytvořili účet úložiště a nemá oprávnění k účtu úložiště, následujícím způsobem nastavit oprávnění pro svůj účet a ujistěte se, že můžete spravovat všechna úložiště oprávnění ve službě Key Vault.
    

<a name="step-by-step-instructions-on-how-to-use-key-vault-to-create-and-generate-sas-tokens"></a>Krok pokyny o tom, jak vytvořit a generovat tokeny SAS pomocí služby Key Vault
--------------------------------------------------------------------------------
Můžete také požádat služby Key Vault ke generování tokenů SAS (sdíleným přístupovým podpisům). Sdílený přístupový podpis poskytuje Delegovaný přístup k prostředkům ve vašem účtu úložiště. Pomocí SAS můžete udělit klientům přístup k prostředkům ve vašem účtu úložiště, aniž byste sdíleli své klíče účtu. Toto je zásadní aspekt používání sdílených přístupových podpisů v aplikacích – SAS představuje bezpečný způsob sdílení prostředků úložiště, aniž byste ohrozili své klíče účtu.

Po dokončení kroků uvedených výše můžete můžete spusťte následující příkazy Key Vault ke generování tokenů SAS pro vás požádat. 

Seznam věcí, které by se dají naplnit v níže uvedené kroky jsou
- Nastaví účet definice SAS s názvem "<YourSASDefinitionName>"v účtu úložiště služby KeyVault spravované"<YourStorageAccountName>"ve vašem trezoru"<VaultName>". 
- Vytvoří token SAS účtu služby Blob, soubor, tabulka a fronta, pro typy prostředků, služby kontejneru a objektu, se všechna oprávnění, přes protokol https a se zadaným počátečním a koncovým datem.
- Nastaví KeyVault cloudově spravovaného úložiště definice SAS v trezoru se šablona identifikátoru uri jako token SAS vytvořili výše, SAS typu "účet" a platný N dní
- Načte skutečnou přístupový token z tajného kódu trezoru klíčů, které odpovídají jeho definice SAS

1. V tomto kroku vytvoříme definice SAS. Po vytvoření této definice SAS, můžete požádat o službě Key Vault ke generování více tokenů SAS za vás. Tato operace vyžaduje oprávnění úložiště/setsas.

```
$sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
```
Zobrazí se další nápovědu výše uvedené operace [zde](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas)

Pokud tuto operace proběhne úspěšně, byste měli vidět výstup podobný jak je znázorněno níže. Kopírování, která

```console
   "se=2020-01-01&sp=***"
```

2. V tomto kroku použijeme výstup ($sasToken) generovaný nad k vytvoření definice SAS. Další dokumentaci najdete [zde](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters)   

```
az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
```
                        

 > [!NOTE] 
 > V případě, že uživatel nemá oprávnění k účtu úložiště dostaneme nejprve Id objektu uživatele

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    
    ```
    
## <a name="fetch-sas-tokens-in-code"></a>Načíst tokeny SAS v kódu

V této části probereme, jak lze provést operace v účtu úložiště načítání [tokeny SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) ze služby Key Vault

V níže uvedený oddíl, jsme ukazují, jak načíst tokeny SAS, jakmile se vytvoří definice SAS, jak je znázorněno výše.

> [!NOTE] 
  Existují 3 způsoby ověření do služby Key Vault, jak si můžete přečíst [základní koncepty](key-vault-whatis.md#basic-concepts)
> - Pomocí Identity spravované služby (doporučeno)
> - Pomocí instančního objektu a certifikátů 
> - Pomocí instančního objektu a hesla (nedoporučuje se)

```cs
// Once you have a security token from one of the above methods, then create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault. SecretUri is of the format https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Pokud váš token SAS vyprší, pak byste znovu načíst změny SAS token ze služby Key Vault a aktualizace kódu

```cs
// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

### <a name="relevant-azure-cli-commands"></a>Důležité příkazy rozhraní příkazového řádku Azure

[Příkazy Azure CLI úložiště](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

## <a name="see-also"></a>Další informace najdete v tématech

- [Informace o klíčích, tajných kódech a certifikátech](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog týmu služby Key Vault](https://blogs.technet.microsoft.com/kv/)
