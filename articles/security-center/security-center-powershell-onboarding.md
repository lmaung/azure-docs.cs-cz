---
title: Použití Powershellu k připojení Azure Security Center a chránit vaši síť | Dokumentace Microsoftu
description: Tento dokument vás provede procesem jeho registrace Azure Security Center pomocí rutin prostředí PowerShell.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/2/2018
ms.author: rkarlin
ms.openlocfilehash: 73043680ea7b8b63a329d0a457449b635b7b80f2
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113392"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Automatizace registrace služby Azure Security Center pomocí Powershellu

Je možné zabezpečit vaše úlohy Azure prostřednictvím kódu programu, pomocí modulu Azure Security Center PowerShell.
Pomocí prostředí PowerShell vám umožňuje automatizovat úlohy a vyhněte se lidské chyby spočívající v ruční úlohy. To je obzvláště užitečné v rozsáhlých nasazeních, které se týkají desítky předplatných se stovkami a tisíci prostředky – všechny z nich musí být zabezpečená od začátku.

Registrace Azure Security Center pomocí prostředí PowerShell umožňuje programově automatické zařazení do systému a správu vašich prostředků Azure a přidání ovládacích prvků zabezpečení potřebné.

Tento článek obsahuje ukázkový skript Powershellu, který lze upravit a použít ve vašem prostředí zavedení Security Center ve vašich předplatných. 

V tomto příkladu jsme povolit Security Center na předplatné s ID: d07c0080-170c-4c24-861d-9c817742786c a použít doporučená nastavení, které poskytují implementací na úrovni Standard služby Security Center, která poskytuje vysoký stupeň ochrany, Možnosti ochrany a detekci pokročilých hrozeb:

1. Nastavte [ASC standardní úroveň ochrany](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Nastavte pracovní prostor Log Analytics, na které pošle agenta Microsoft Monitoring Agent, že data, která se shromažďuje na virtuální počítače přidružené k předplatnému – v tomto příkladu existující pracovní prostor definované uživatelem (myWorkspace).

3. Aktivovat automatické agenta Security Center zřizování, který [nasadí agenta Microsoft Monitoring Agent](security-center-enable-data-collection.md#auto-provision-mma).

5. Nastavení organizace [Ředitelka jako adresu kontaktu zabezpečení ASC výstrahy a významné události](security-center-provide-security-contact-details.md).

6. Centrum zabezpečení přiřadit [výchozí zásady zabezpečení](tutorial-security-policy.md).

## <a name="prerequisites"></a>Požadavky

Tyto kroky musí provést předtím, než spustíte rutiny Security Center:

1.  Spusťte PowerShell jako správce.
2.  Spusťte následující příkazy v prostředí PowerShell:
      
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Install-Module -Name Az.Security -Force

## <a name="onboard-security-center-using-powershell"></a>Připojení Security Center pomocí Powershellu

1.  Zaregistrujte předplatné u poskytovatele prostředků centra zabezpečení:

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  Volitelné: Nastavení úrovně pokrytí (cenová úroveň) z předplatných (Pokud není definován, cenová úroveň je nastavena na bezplatnou):

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzSecurityPricing -Name "default" -PricingTier "Standard"

3.  Konfigurovat pracovní prostor Log Analytics, do kterého budou hlásit agenty. Musíte mít pracovní prostor Log Analytics, který jste již vytvořili, že virtuální počítače předplatné budou hlásit k. Můžete definovat několik předplatných do zprávy do stejného pracovního prostoru. Pokud není definován, použije se výchozí pracovní prostor.

        Set-AzSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Automatické zřizování instalace agenta Microsoft Monitoring Agent na virtuálních počítačích Azure:
    
        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Doporučuje povolit automatické zřizování, abyste měli jistotu, že virtuální počítače Azure jsou automaticky chráněny službou Azure Security Center.
    >

5.  Volitelné: Důrazně doporučujeme, že definujete podrobností o kontaktu zabezpečení pro předplatná, připojení, který se použije jako příjemce oznámení a oznámení vygenerovaný službou Security Center:

        Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert 

6.  Přiřazení iniciativy zásad výchozí Security Center:

        Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ '[Preview]: Enable Monitoring in Azure Security Center'}
        New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

Jste teď úspěšně zprovoznění Azure Security Center pomocí Powershellu.

Nyní můžete tyto rutiny prostředí PowerShell se skripty pro automatizaci pro iteraci prostřednictvím kódu programu napříč předplatným a prostředkům. To šetří čas a snižuje pravděpodobnost lidské chyby. To může být použito [ukázkový skript](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) jako odkaz.






## <a name="see-also"></a>Další informace najdete v tématech
Další informace o použití Powershellu k automatizaci připojování ke službě Security Center, najdete v následujícím článku:

* [Az.Security](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Security/Commands.Security/help/Az.Security.md).

Další informace o službě Security Center, najdete v následujícím článku:

* [Nastavení zásad zabezpečení v Azure Security Center](tutorial-security-policy.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.
