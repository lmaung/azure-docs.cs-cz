---
title: Přiřazení role adresáře pro uživatele – Azure Active Directory | Dokumentace Microsoftu
description: Pokyny ohledně toho, jak přiřadit role správce a bez oprávnění správce pro uživatele se službou Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd26fa53c91c53893c7f326afda5158fa430be1e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188133"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>Přiřazení rolí správce a bez oprávnění správce uživatelům v Azure Active Directory
Pokud uživatel ve vaší organizaci potřebuje oprávnění ke správě prostředků Azure Active Directory (Azure AD), musíte přiřadit uživateli příslušné role ve službě Azure AD podle akce, které uživatel potřebuje oprávnění k provedení.

Další informace o dostupných rolí najdete v tématu [přiřazení rolí správce v Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md). Další informace o přidávání uživatelů najdete v tématu [přidání nových uživatelů do služby Azure Active Directory](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Přiřazení rolí
Běžný způsob, jak přiřadit role Azure AD pro uživatele je na **role adresáře** stránky pro uživatele.

Můžete také přiřadit role pomocí Privileged Identity Management (PIM). Podrobné informace o tom, jak používat PIM, naleznete v tématu [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

### <a name="to-assign-a-role-to-a-user"></a>Přiřazení role uživateli
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/) pomocí účtu globálního správce daného adresáře.

2. Vyberte **Azure Active Directory**vyberte **uživatelé**a poté vyhledejte a vyberte uživatele, získávání přiřazení role. Například _Alain Charon_.

3. Na **Alain Charon – profil** stránce **role adresáře**.

    **Alain Charon - role adresáře** se zobrazí stránka.

4. Vyberte **přidat roli**, vyberte roli, kterou chcete přiřadit k Alain (například _správce aplikace_) a klikněte na tlačítko **vyberte**.

    ![Stránka role adresáře, zobrazující vybranou roli](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    Role správce aplikace je přiřazená Alain Charon a zobrazí se na **Alain Charon - role adresáře** stránky.

## <a name="remove-a-role-assignment"></a>Odebrání přiřazení role
Pokud je potřeba odebrat přiřazení role od uživatele, můžete také provést z **Alain Charon - role adresáře** stránky.

### <a name="to-remove-a-role-assignment-from-a-user"></a>Odebrání uživateli přiřazení role

1. Vyberte **Azure Active Directory**vyberte **uživatelé**a poté vyhledejte a vyberte uživatele, získávání odebrat přiřazení role. Například _Alain Charon_.

2. Vyberte **role adresáře**vyberte **správce aplikace**a pak vyberte **odebrat roli**.

    ![Stránka role adresáře, zobrazující vybranou roli a možnost odebrat](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    Role správce aplikace se odebere z Alain Charon a již nezobrazuje v **Alain Charon - role adresáře** stránky.

## <a name="next-steps"></a>Další postup
- [Přidání nebo odstranění uživatelů](add-users-azure-active-directory.md)

- [Přidání nebo změně informací profilu](active-directory-users-profile-azure-portal.md)

- [Přidání uživatelů typu host z jiného adresáře](../b2b/what-is-b2b.md)

Nebo můžete provádět další úkoly při správě uživatelů, jako je například přiřazení delegáty, pomocí zásad, sdílení a uživatelské účty. Další informace o dalších dostupných akcí najdete v tématu [dokumentace ke službě Azure Active Directory uživatele management](../users-groups-roles/index.yml).


