---
title: 'Kurz: Integrace Azure Active Directory s QuickHelp | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a QuickHelp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 013504fefd927d2e970c5b07a0e9c61afc715d7c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56186909"
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Kurz: Integrace Azure Active Directory s QuickHelp

V tomto kurzu se dozvíte, jak integrovat QuickHelp s Azure Active Directory (Azure AD).

QuickHelp integraci se službou Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k QuickHelp
- Můžete povolit uživatelům, aby automaticky získat přihlášení k QuickHelp (Single Sign-On) s jejich účty Azure AD
- Můžete spravovat své účty na jediném místě – na webu Azure portal

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s QuickHelp, potřebujete následující položky:

- Předplatné Azure AD
- QuickHelp jednotného přihlašování povolená předplatného

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete získat měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání QuickHelp z Galerie
1. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-quickhelp-from-the-gallery"></a>Přidání QuickHelp z Galerie
Konfigurace integrace QuickHelp do služby Azure AD, budete muset přidat QuickHelp z Galerie na váš seznam spravovaných aplikací SaaS.

**Chcete-li přidat QuickHelp z galerie, postupujte následovně:**

1. V **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

1. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
1. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Aplikace][3]

1. Do vyhledávacího pole zadejte **QuickHelp**.

    ![Vytváří se testovací uživatele služby Azure AD](./media/quickhelp-tutorial/tutorial_quickhelp_search.png)

1. Na panelu výsledků vyberte **QuickHelp**a potom klikněte na tlačítko **přidat** tlačítko pro přidání aplikace.

    ![Vytváří se testovací uživatele služby Azure AD](./media/quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části Konfigurace a testování Azure AD jednotné přihlašování pomocí QuickHelp podle testovacího uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v QuickHelp je pro uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a související uživatelské v QuickHelp potřeba navázat.

V QuickHelp, přiřaďte hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** a tím vytvoří vztah odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s QuickHelp, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
1. **[Vytváří se testovací uživatele služby Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
1. **[Vytvoření zkušebního uživatele QuickHelp](#creating-a-quickhelp-test-user)**  – Pokud chcete mít protějšek Britta Simon QuickHelp, který je propojený s Azure AD reprezentace uživatele.
1. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
1. **[Testování Single Sign-On](#testing-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování v aplikaci QuickHelp.

**Ke konfiguraci Azure AD jednotné přihlašování s QuickHelp, proveďte následující kroky:**

1. Na webu Azure Portal na **QuickHelp** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování][4]

1. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.
 
    ![Konfigurace jednotného přihlašování](./media/quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

1. Na **QuickHelp domény a adresy URL** části, proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/quickhelp-tutorial/tutorial_quickhelp_url.png)

    a. V **přihlašovací adresa URL** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://quickhelp.com/<ROUTEURL>`

    b. V **identifikátor** textového pole zadejte adresu URL: `https://auth.quickhelp.com`

    > [!NOTE] 
    > Hodnota přihlašovací adresa URL není skutečný. Aktualizujte příslušnou hodnotu skutečné přihlašovací adresa URL. Obraťte se na správce ve vaší organizaci QuickHelp nebo správce Debata klienta úspěch má být získána hodnota.
 
1. Na **podpisový certifikát SAML** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor metadat ve vašem počítači.

    ![Konfigurace jednotného přihlašování](./media/quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

1. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurace jednotného přihlašování](./media/quickhelp-tutorial/tutorial_general_400.png) 

1. Přihlašování k webu společnosti QuickHelp jako správce.

1. V nabídce v horní části klikněte na tlačítko **správce**.
   
    ![Konfigurace jednotného přihlašování][21]

1. V **QuickHelp správce** nabídky, klikněte na tlačítko **nastavení**.
   
    ![Konfigurace jednotného přihlašování][22]

1. Klikněte na tlačítko **nastavení ověřování**.

1. Na **nastavení ověřování** stránce, proveďte následující kroky
   
    ![Konfigurace jednotného přihlašování][23]
   
    a. Jako **typ jednotného přihlašování**vyberte **WSFederation**.
   
    b. Nahrát soubor metadat stažené Azure, klikněte na tlačítko **Procházet**, přejděte k souboru, pak klikněte na tlačítko Ukončit **nahrát metadat**.
   
    c. V **e-mailu** textové pole, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
   
    d. V **křestní jméno** textového pole `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
   
    e. V **příjmení** textového pole `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
   
    f. V **panel akcí**, klikněte na tlačítko **Uložit**.

### <a name="creating-an-azure-ad-test-user"></a>Vytváří se testovací uživatele služby Azure AD
Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

![Vytvoření uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. V **webu Azure portal**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváří se testovací uživatele služby Azure AD](./media/quickhelp-tutorial/create_aaduser_01.png) 

1. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváří se testovací uživatele služby Azure AD](./media/quickhelp-tutorial/create_aaduser_02.png) 

1. Chcete-li otevřít **uživatele** dialogového okna, klikněte na tlačítko **přidat** horní části dialogového okna.
 
    ![Vytváří se testovací uživatele služby Azure AD](./media/quickhelp-tutorial/create_aaduser_03.png) 

1. Na **uživatele** dialogového okna stránky, proveďte následující kroky:
 
    ![Vytváří se testovací uživatele služby Azure AD](./media/quickhelp-tutorial/create_aaduser_04.png) 

    a. V **název** textové pole, typ **BrittaSimon**.

    b. V **uživatelské jméno** textové pole, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit heslo** a zapište si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-quickhelp-test-user"></a>Vytvoření zkušebního uživatele QuickHelp

Cílem této části je vytvořte uživatele Britta Simon v QuickHelp.
Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co je uživateli protějšky v QuickHelp uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a související uživatelské v QuickHelp potřeba navázat.

QuickHelp podporuje zřizování just-in-time. To znamená, pokud je to nezbytné, uživatelský účet se automaticky vytvoří v QuickHelp a účet je propojený s účtem služby Azure AD.

Neexistuje žádná položka akce pro vás v této části.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části je povolit Britta Simon k udělení přístupu k QuickHelp použití Azure jednotného přihlašování.

![Přiřadit uživatele][200] 

**Přiřadit QuickHelp Britta Simon, proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

1. V seznamu aplikací vyberte **QuickHelp**.

    ![Konfigurace jednotného přihlašování](./media/quickhelp-tutorial/tutorial_quickhelp_app.png) 

1. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

1. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Přiřadit uživatele][203]

1. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

1. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

1. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.
    
### <a name="testing-single-sign-on"></a>Testování jednotného přihlašování

Cílem této části je test vaší konfigurace Azure AD jednotné přihlašování pomocí přístupového panelu.  

Po kliknutí na dlaždici QuickHelp na přístupovém panelu, vám by měl získat automaticky přihlášení k aplikaci QuickHelp.


## <a name="additional-resources"></a>Další materiály

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/quickhelp-tutorial/tutorial_quickhelp_07.png
