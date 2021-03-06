---
title: Odebrání aplikace zaregistrované na platformě Microsoft Identity Platform |Azure
description: Zjistěte, jak odebrat aplikaci zaregistrovanou na platformě Microsoft Identity Platform.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/25/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: d71489194422a785ee98ec70a07c765c3d686808
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182608"
---
# <a name="quickstart-remove-an-application-registered-with-the-microsoft-identity-platform-preview"></a>Rychlý start: Odebrat aplikaci registrovanou pomocí Microsoft identity platform (Preview)

Podnikoví vývojáři a poskytovatelé softwaru jako služby (SaaS), kteří mají zaregistrované aplikace na platformě Microsoft Identity Platform, můžou potřebovat odebrat registraci aplikace.

V tomto rychlém startu se naučíte:

* Odebrání aplikace vytvořené vámi nebo vaší organizací
* Odebrání aplikace vytvořené jinou organizací

## <a name="prerequisites"></a>Požadavky

Než začnete, musíte splnit následující požadavky:

* Máte tenanta, ke kterému jsou zaregistrované aplikace. Informace o přidání a registraci aplikací najdete v tématu [Registrace aplikace na platformě Microsoft Identity Platform](quickstart-register-app.md).
* Na webu Azure Portal jste se přihlásili k prostředí pro registrace aplikací ve verzi Preview. Kroky v tomto rychlém startu odpovídají novému uživatelskému rozhraní a fungují pouze v případě, že jste se přihlásili k prostředí ve verzi Preview.

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Odebrání aplikace vytvořené vámi nebo vaší organizací

Aplikace, které jste zaregistrovali vy nebo vaše organizace, jsou reprezentované objektem aplikace a instančním objektem ve vašem tenantovi. Další informace najdete v tématu [Objekty aplikací a instanční objekty](active-directory-application-objects.md).

### <a name="to-remove-an-application"></a>Odebrání aplikace

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účtu Microsoft.
2. Pokud váš účet umožňuje přístup k více tenantům, vyberte svůj účet v pravém horním rohu a nastavte relaci portálu na požadovaného tenanta Azure AD.
3. V levém navigačním podokně vyberte službu **Azure Active Directory** a pak **Registrace aplikací (Preview)**. Vyhledejte a vyberte aplikaci, kterou chcete nakonfigurovat. Jakmile vyberete aplikaci, zobrazí se stránka **Přehled** dané aplikace.
4. Na stránce **Přehled** vyberte **Odstranit**.
5. Vybráním **Ano** potvrďte, že chcete aplikaci odstranit.

  > [!NOTE]
  > Abyste mohli odstranit aplikaci, musíte být uvedeni jako vlastník aplikace nebo mít oprávnění správce.

## <a name="remove-an-application-authored-by-another-organization"></a>Odebrání aplikace vytvořené jinou organizací

Pokud se díváte na **Registrace aplikací** v kontextu tenanta, na kartě **Všechny aplikace** se zobrazí podmnožina aplikací, které jsou z jiného tenanta a do vašeho tenanta se zaregistrovaly po vyjádření souhlasu. Konkrétněji je reprezentuje pouze instanční objekt ve vašem tenantovi bez odpovídajícího objektu aplikace. Další informace o rozdílech mezi objekty aplikací a instančními objekty najdete v tématu [Objekty aplikací a instanční objekty v Azure AD](active-directory-application-objects.md).

Pokud chcete odebrat přístup aplikace k vašemu adresáři (po udělení souhlasu), musí správce společnosti odebrat instanční objekt aplikace. Tento správce musí mít přístup globálního správce a instanční objekt může odebrat prostřednictvím webu Azure Portal nebo použít [rutiny Azure AD PowerShellu](https://go.microsoft.com/fwlink/?LinkId=294151) k odebrání přístupu.

## <a name="next-steps"></a>Další postup

Další informace najdete v těchto rychlých startech souvisejících se správou aplikací:

* [Registrace aplikace na platformě Microsoft Identity Platform](quickstart-register-app.md)
* [Konfigurace klientské aplikace pro přístup k webovým rozhraním API](quickstart-configure-app-access-web-apis.md)
* [Konfigurace aplikace pro zveřejnění webových rozhraní API](quickstart-configure-app-expose-web-apis.md)
* [Úprava účtů podporovaných aplikací](quickstart-modify-supported-accounts.md)
