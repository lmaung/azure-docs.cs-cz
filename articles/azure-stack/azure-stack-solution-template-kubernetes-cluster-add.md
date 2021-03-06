---
title: Přidat Kubernetes pro Azure Stack Marketplace | Dokumentace Microsoftu
description: Zjistěte, jak přidat Kubernetes pro Azure Stack Marketplace.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 7b73a0efbe20b43b3ba8e94fce3aabf12ea918e8
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56986052"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Přidat Kubernetes na webu Marketplace služby Azure Stack

*Platí pro: Azure Stack integrované systémy a Azure Stack Development Kit*

> [!note]  
> Kubernetes ve službě Azure Stack je ve verzi preview. Azure Stack odpojené scénář není aktuálně podporován ve verzi preview.

Uživatelům můžete nabídnout Kubernetes jako položky Marketplace. Uživatelé můžou potom nasaďte Kubernetes v rámci jediné koordinované operace.

Následující článek se zabývá pomocí šablony Azure Resource Manageru k nasazení a zřizování prostředkům pro samostatného clusteru Kubernetes. Než začnete, zkontrolujte Azure Stack a nastavení globální tenanta Azure. Shromážděte požadované informace o Azure Stack. Přidáte prostředky potřebné pro vašeho tenanta a na Azure Marketplace zásobníku. Clusteru závisí na serveru se systémem Ubuntu, vlastní skript a položky Marketplace clusteru Kubernetes ve na webu marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Vytvořit plán, nabídky a předplatné

Vytvořte plán, nabídky a předplatné pro položky Kubernetes Marketplace. Můžete také použít existující plán a nabídnout.

1. Přihlaste se k [portál pro správu.](https://adminportal.local.azurestack.external)

1. Vytvoření plánu jako základní plán. Pokyny najdete v tématu [vytvoření plánu ve službě Azure Stack](azure-stack-create-plan.md).

1. Vytvořte nabídku. Pokyny najdete v tématu [vytvoření nabídky ve službě Azure Stack](azure-stack-create-offer.md).

1. Vyberte **nabízí**a najít vámi vytvořená nabídka.

1. Vyberte **přehled** v okně nabídky.

1. Vyberte **změnit stav**. Vyberte **veřejné**.

1. Vyberte **+ vytvořit prostředek** > **nabídky a plány** > **předplatné** vytvořit odběr.

    a. Zadejte **zobrazovaný název**.

    b. Zadejte **uživatele**. Použijte účet Azure AD přidružené k tenantovi.

    c. **Popis zprostředkovatele**

    d. Nastavte **adresář tenanta** do tenanta služby Azure AD pro Azure Stack. 

    e. Vyberte **nabízejí**. Vyberte název nabídky, která jste vytvořili. Poznamenejte si ID předplatného.

## <a name="create-a-service-principal-and-credentials-in-ad-fs"></a>Vytvoření instančního objektu a přihlašovacích údajů ve službě AD FS

Pokud používáte Active Directory Federated Services (AD FS) pro vaši službu identity management, je potřeba vytvořit instanční objekt pro uživatele nasazení clusteru Kubernetes.

1. Vytvořte a exportujte certifikát podepsaný svým držitelem použitý k vytvoření instančního objektu služby. 

    - Budete potřebovat následující údaje:

       | Hodnota | Popis |
       | ---   | ---         |
       | Heslo | Zadejte nové heslo pro certifikát. |
       | Cesta k místní certifikátu | Zadejte název a cesta k souboru certifikátu. Příklad: `c:\certfilename.pfx` |
       | Název certifikátu | Zadejte název certifikátu. |
       | Umístění úložiště certifikátů |  Například `Cert:\LocalMachine\My`. |

    - Otevřete prostředí PowerShell s řádku se zvýšenými oprávněními. Spusťte následující skript s parametry, aktualizovat, aby vaše hodnoty:

        ```PowerShell  
        # Creates a new self signed certificate 
        $passwordString = "<password>"
        $certlocation = "<local certificate path>.pfx"
        $certificateName = "CN=<certificate name>"
        $certStoreLocation="<certificate store location>"
        
        $params = @{
        CertStoreLocation = $certStoreLocation
        DnsName = $certificateName
        FriendlyName = $certificateName
        KeyLength = 2048
        KeyUsageProperty = 'All'
        KeyExportPolicy = 'Exportable'
        Provider = 'Microsoft Enhanced Cryptographic Provider v1.0'
        HashAlgorithm = 'SHA256'
        }
        
        $cert = New-SelfSignedCertificate @params -ErrorAction Stop
        Write-Verbose "Generated new certificate '$($cert.Subject)' ($($cert.Thumbprint))." -Verbose
        
        #Exports certificate with password in a .pfx format
        $pwd = ConvertTo-SecureString -String $passwordString -Force -AsPlainText
        Export-PfxCertificate -cert $cert -FilePath $certlocation -Password $pwd
        ```

2.  Poznamenejte si nové ID certifikátu, zobrazí v relaci Powershellu, `1C2ED76081405F14747DC3B5F76BB1D83227D824`. ID se použije při vytváření instančního objektu.

    ```PowerShell  
    VERBOSE: Generated new certificate 'CN=<certificate name>' (1C2ED76081405F14747DC3B5F76BB1D83227D824).
    ```

3. Vytvoření instančního objektu pomocí certifikátu.

    - Budete potřebovat následující údaje:

       | Hodnota | Popis                     |
       | ---   | ---                             |
       | ERCS IP | V ASDK, privilegovaných koncový bod je obvykle `AzS-ERCS01`. |
       | Název aplikace | Zadejte jednoduchý název instančního objektu aplikace služby. |
       | Umístění úložiště certifikátů | Cesta v počítači, kam jste uložili certifikát. Je toto označeno umístění úložiště a ID certifikátu vygenerovaný v prvním kroku. Příklad: `Cert:\LocalMachine\My\1C2ED76081405F14747DC3B5F76BB1D83227D824` |

       Po zobrazení výzvy použijte následující pověření pro připojení ke koncovému bodu oprávnění. 
        - Uživatelské jméno: Zadejte účet CloudAdmin ve formátu <Azure Stack domain>\cloudadmin. (Pro ASDK, uživatelské jméno je azurestack\cloudadmin.)
        - Heslo: Zadejte stejné heslo, které jste zadali během instalace pro účet správce domény AzureStackAdmin.

    - Spusťte následující skript s parametry, aktualizovat, aby vaše hodnoty:

        ```PowerShell  
        #Create service principal using the certificate
        $privilegedendpoint="<ERCS IP>"
        $applicationName="<application name>"
        $certStoreLocation="<certificate location>"
        
        # Get certificate information
        $cert = Get-Item $certStoreLocation
        
        # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
        $creds = Get-Credential

        # Creating a PSSession to the ERCS PrivilegedEndpoint
        $session = New-PSSession -ComputerName $privilegedendpoint -ConfigurationName PrivilegedEndpoint -Credential $creds

        # Get Service principal Information
        $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name "$using:applicationName" -ClientCertificates $using:cert}

        # Get Stamp information
        $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }

        # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
        $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

        # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
        $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

        # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
        $TenantID = $AzureStackInfo.AADTenantID

        # Register an AzureRM environment that targets your Azure Stack instance
        Add-AzureRMEnvironment `
        -Name "AzureStackUser" `
        -ArmEndpoint $ArmEndpoint

        # Set the GraphEndpointResourceId value
        Set-AzureRmEnvironment `
        -Name "AzureStackUser" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true
        Add-AzureRmAccount -EnvironmentName "azurestackuser" `
        -ServicePrincipal `
        -CertificateThumbprint $ServicePrincipal.Thumbprint `
        -ApplicationId $ServicePrincipal.ClientId `
        -TenantId $TenantID

        # Output the SPN details
        $ServicePrincipal
        ```

    - Podrobnosti instančního objektu služby vypadat jako následující fragment kódu

        ```Text  
        ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
        ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
        Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
        ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
        PSComputerName        : azs-ercs01
        RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
        ```

## <a name="add-an-ubuntu-server-image"></a>Přidat Ubuntu server image

Přidejte následující image Ubuntu Server na webu Marketplace:

1. Přihlaste se k [portál pro správu](https://adminportal.local.azurestack.external).

1. Vyberte **všechny služby**a potom v části **správu** vyberte **Marketplace správu**.

1. Vyberte **+ přidat z Azure**.

1. Zadejte `Ubuntu Server`.

1. Vyberte nejnovější verzi serveru. Zkontrolujte na plnou verzi a ujistěte se, že máte nejnovější verzi:
    - **Publisher**: Canonical
    - **Nabízejí**: UbuntuServer
    - **Verze**: 16.04.201806120 (nebo nejnovější verze)
    - **SKLADOVÁ POLOŽKA**: 16.04-LTS

1. Vyberte **stáhnout.**

## <a name="add-a-custom-script-for-linux"></a>Přidání vlastních skriptů pro Linux

Přidáte rozhraní Kubernetes na Marketplace:

1. Otevřít [portál pro správu](https://adminportal.local.azurestack.external).

1. Vyberte **všechny služby** a potom v části **správu** vyberte **Marketplace správu**.

1. Vyberte **+ přidat z Azure**.

1. Zadejte `Custom Script for Linux`.

1. Vyberte skript, který se následující profil:
    - **Nabízejí**: Vlastní skript pro Linux 2.0
    - **Verze**: 2.0.6 (nebo nejnovější verze)
    - **Publisher**: Microsoft Corp

    > [!Note]  
    > Může být uvedeno více než jednu verzi vlastních skriptů pro Linux. Je potřeba přidat poslední verzi položky.

1. Vyberte **stáhnout.**


## <a name="add-kubernetes-to-the-marketplace"></a>Přidat Kubernetes na webu Marketplace

1. Otevřít [portál pro správu](https://adminportal.local.azurestack.external).

1. Vyberte **všechny služby** a potom v části **správu** vyberte **Marketplace správu**.

1. Vyberte **+ přidat z Azure**.

1. Zadejte `Kubernetes`.

1. Vyberte `Kubernetes Cluster`.

1. Vyberte **stáhnout.**

    > [!note]  
    > Může trvat 5 minut, další položky marketplace se zobrazí na webu Marketplace.

    ![Kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Aktualizace nebo odebrání rozhraní Kubernetes 

Při aktualizaci položky Kubernetes, odeberete předchozí položky na webu Marketplace. Postupujte podle pokynů v tomto článku se přidat že kubernetes aktualizovat na webu Marketplace.

Odebrat položku Kubernetes:

1. Připojení ke službě Azure Stack pomocí prostředí PowerShell jako operátor. Pokyny naleznete v tématu [připojit ke službě Azure Stack pomocí prostředí PowerShell jako operátor](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Aktuální položka clusteru Kubernetes najdete v galerii.

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Poznamenejte si název aktuální položky, jako například `Microsoft.AzureStackKubernetesCluster.0.3.0`

4. Použijte následující rutiny Powershellu pro odebrání položky:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Další postup

[Nasazení Kubernetes pro Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

[Přehled nabízených služeb ve službě Azure Stack](azure-stack-offer-services-overview.md)
