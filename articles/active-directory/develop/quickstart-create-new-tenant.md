---
title: Vytvoření tenanta Azure Active Directory | Microsoft Docs
description: Zjistěte, jak vytvořit tenanta Azure AD pro použití při registraci a vytváření aplikací.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 02/05/2019
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7db08cde0df1ba3d9bc227758ab3ad00c1fc422
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207366"
---
# <a name="quickstart-set-up-a-dev-environment"></a>Rychlý start: Vytvoření a nastavení vývojového prostředí

Microsoft Identity Platform umožňuje vývojářům vytvářet aplikace určené pro širokou škálu vlastních prostředí a identit Microsoft 365. Abyste mohli začít používat Microsoft identity platform, budete potřebovat přístup k prostředí, také nazývané tenanta služby Azure AD, který může zaregistrovat a spravovat aplikace, mají přístup k datům Microsoft 365 a nasadit vlastní omezení podmíněného přístupu a tenanta.

Tenant je reprezentace organizace. Jedná se o vyhrazenou instanci Azure AD, kterou organizace nebo vývojář aplikací obdrží při vytvoření partnerství s Microsoftem, například registrací do Azure, Microsoft Intune nebo Microsoft 365.

Každý tenant Azure AD je odlišný a oddělený od ostatních tenantů Azure AD a používá vlastní reprezentaci pracovních a školních identit, identit zákazníků (pokud se jedná o tenanta Azure AD B2C) a registrací aplikací. Registrace aplikace uvnitř vašeho tenanta může umožňovat ověřování z účtů pouze v rámci vašeho tenanta nebo ve všech tenantech.

## <a name="determining-environment-type"></a>Určení typu prostředí

Existují dva typy prostředí, které můžete vytvořit. Určení potřebného typu prostředí vychází pouze z typu uživatelů, které bude vaše aplikace ověřovat.

* Pracovní a školní účty (účty Azure AD) nebo účty Microsoft (například outlook.com a live.com)
* Účty na sociálních sítích a místní účty (Azure AD B2C)

Tento rychlý start je rozdělený na dva scénáře v závislosti na typu aplikace, kterou chcete vytvořit. Pokud potřebujete další pomoc s určením typu identity, přečtěte si [informace o platformě Microsoft Identity Platform](about-microsoft-identity-platform.md).

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>Pracovní a školní účty nebo účty Microsoft

### <a name="use-an-existing-tenant"></a>Použití existujícího tenanta

Řada vývojářů už má tenanty vytvořené prostřednictvím služeb nebo předplatných vázaných na tenanty Azure AD, jako jsou předplatná Microsoft 365 nebo Azure.

1. Pokud chcete zkontrolovat, jestli máte tenanta, přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí účtu, který chcete použít ke správě vaší aplikace.
1. Zkontrolujte pravý horní roh. Pokud tenanta máte, budete k němu automaticky přihlášeni a název tenanta se zobrazí přímo pod názvem vašeho účtu.
   * Najeďte myší na název vašeho účtu v pravém horním rohu webu Azure Portal, zobrazí se vaše jméno, e-mail, ID adresáře nebo tenanta (GUID) a vaše doména.
   * Pokud je k vašemu účtu přidruženo více tenantů, můžete výběrem názvu vašeho účtu otevřít nabídku, ve které můžete mezi jednotlivými tenanty přepínat. Každý tenant má vlastní ID tenanta.

> [!TIP]
> Pokud potřebujete zjistit ID tenanta, můžete to provést následovně:
* Najeďte myší na název vašeho účtu, aby se zobrazilo ID adresáře nebo tenanta.
* Nebo na webu Azure Portal vyberte **Azure Active Directory > Vlastnosti > ID adresáře**.

Pokud existujícího tenanta přidruženého k účtu nemáte, zobrazí se pod názvem vašeho účtu GUID a než dokončíte postup v následující části, nebudete moct provádět akce, jako jsou třeba registrace aplikací.

### <a name="create-a-new-azure-ad-tenant"></a>Vytvoření nového tenanta Azure AD

Pokud ještě tenanta Azure AD nemáte nebo chcete pro účely vývoje vytvořit nového, postupujte podle [prostředí pro vytváření adresářů](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Při vytváření nového tenanta budete muset zadat následující informace:

- **Název organizace**
- **Počáteční doména** – bude to část domény *.onmicrosoft.com. Doménu můžete dále přizpůsobit později.
- **Země nebo oblast**

> [!NOTE]
> Když pojmenováváte vašeho tenanta, použijte alfanumerické znaky. Speciální znaky nejsou povoleny. Název nesmí být delší než 256 znaků.

## <a name="social-and-local-accounts"></a>Účty na sociálních sítích a místní účty

Pokud chcete začít vytvářet aplikace s přihlašováním účtů na sociálních sítích a místních účtů, budete muset vytvořit tenanta Azure AD B2C. Pokud chcete začít, postupujte podle pokynů k [vytvoření tenanta Azure AD B2C](../../active-directory-b2c/tutorial-create-tenant.md).

## <a name="next-steps"></a>Další postup

* Vyzkoušejte rychlý start zaměřený na psaní kódu a začněte s ověřováním uživatelů. 
* Podrobnější vzorové kódy najdete v dokumentaci v části **Kurzy**.
* Chcete svou aplikaci nasadit do cloudu? Přečtěte si o [nasazování kontejnerů do Azure](https://docs.microsoft.com/azure/index#pivot=products&panel=containers). 
