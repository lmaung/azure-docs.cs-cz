---
title: 'Kurz: Integrace Azure Active Directory s PureCloud podle Genesys | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PureCloud podle Genesys.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: c9b2770f861098993623d69f6b9f6a1577c9cf27
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56890739"
---
# <a name="tutorial-azure-active-directory-integration-with-purecloud-by-genesys"></a>Kurz: Integrace Azure Active Directory s PureCloud podle Genesys

V tomto kurzu se dozvíte, jak integrovat PureCloud podle Genesys s Azure Active Directory (Azure AD).
Integrace PureCloud podle Genesys s Azure AD poskytuje následující výhody:

* Můžete řídit ve službě Azure AD, který má přístup k PureCloud podle Genesys.
* Můžete povolit uživatelům být automaticky přihlášeni k PureCloud podle Genesys (Single Sign-On) pomocí jejich účtů služby Azure AD.
* Můžete spravovat své účty na jediném místě – na webu Azure portal.

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s PureCloud podle Genesys, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat měsíční zkušební verze [zde](https://azure.microsoft.com/pricing/free-trial/)
* PureCloud podle Genesys jednotného přihlašování povolená předplatného

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu konfigurace a testování v testovacím prostředí Azure AD jednotného přihlašování.

* Podporuje PureCloud podle Genesys **SP a zprostředkovatele identity** jednotné přihlašování zahájené pomocí

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>Přidání PureCloud podle Genesys z Galerie

Konfigurace integrace PureCloud podle Genesys do služby Azure AD, budete muset přidat PureCloud podle Genesys z Galerie na váš seznam spravovaných aplikací SaaS.

**Chcete-li přidat PureCloud podle Genesys z galerie, postupujte následovně:**

1. V **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu.

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte do **podnikové aplikace** a pak vyberte **všechny aplikace** možnost.

    ![V okně podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Tlačítko nové aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **PureCloud podle Genesys**vyberte **PureCloud podle Genesys** z panelu výsledků klikněte **přidat** tlačítko pro přidání aplikace.

     ![PureCloud podle Genesys v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování služby Azure AD jednotného přihlašování

V této části, konfiguraci a testování Azure AD jednotné přihlašování s PureCloud podle Genesys podle testovacího uživatele volá **Britta Simon**.
Pro jednotné přihlašování pro práci je potřeba navázat vztah odkazu mezi uživatele služby Azure AD a související uživatelské v PureCloud podle Genesys.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PureCloud podle Genesys, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurovat Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
2. **[Konfigurace PureCloud podle Genesys Single Sign-On](#configure-purecloud-by-genesys-single-sign-on)**  – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
3. **[Vytvořit testovacího uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
4. **[Přiřadit uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
5. **[Vytvoření PureCloud Genesys testovací uživatel](#create-purecloud-by-genesys-test-user)**  – Pokud chcete mít protějšek Britta Simon PureCloud podle Genesys, který je propojený s Azure AD reprezentace uživatele.
6. **[Otestovat jednotné přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure portal.

Ke konfiguraci Azure AD jednotné přihlašování s PureCloud podle Genesys, proveďte následující kroky:

1. V [webu Azure portal](https://portal.azure.com/)na **PureCloud podle Genesys** integrace stránce aplikace vyberte **jednotného přihlašování**.

    ![Nakonfigurovat jednotné přihlašování – odkaz](common/select-sso.png)

2. Na **vybrat jedinou metodu přihlašování** dialogového okna, vyberte **SAML/WS-Fed** chcete povolit jednotné přihlašování.

    ![Jednotné přihlašování režim výběru](common/select-saml-option.png)

3. Na **nastavte si jednotné přihlašování pomocí SAML** klikněte na **upravit** ikony otevřete **základní konfiguraci SAML** dialogového okna.

    ![Upravit konfiguraci základní SAML](common/edit-urls.png)

4. Na **základní konfiguraci SAML** části, pokud chcete nakonfigurovat aplikace v **IDP** iniciované režimu, proveďte následující kroky:

    ![PureCloud Genesys domény a adresy URL jednotné přihlašování – informace](common/idp-intiated.png)

    a. V **identifikátor** textové pole, zadejte adresu URL podle vaší oblasti:
    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. V **adresy URL odpovědi** textové pole, zadejte adresu URL podle vaší oblasti:
    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml` |

5. Klikněte na tlačítko **nastavit další adresy URL** a provést následující krok, pokud chcete nakonfigurovat aplikace v **SP** iniciované režimu:

    ![PureCloud Genesys domény a adresy URL jednotné přihlašování – informace](common/metadata-upload-additional-signon.png)

    V **přihlašovací adresa URL** textové pole, zadejte adresu URL podle vaší oblasti:
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

6. PureCloud Genesys aplikace očekává, že kontrolní výrazy SAML v určitém formátu, který je potřeba přidat vlastní atribut mapování konfigurace atributy tokenu SAML. Na následujícím snímku obrazovky se zobrazí v seznamu atributů výchozí. Klikněte na tlačítko **upravit** ikony otevřete **atributy uživatele** dialogového okna.

    ![image](common/edit-attribute.png)

7. Kromě výše PureCloud Genesys aplikace očekává, že několik dalších atributů musí být předány zpět odpověď SAML. V **deklarace identity uživatelů** části na **atributy uživatele** dialogového okna, proveďte následující kroky pro přidání atributu tokenu SAML, jak je znázorněno v následující tabulka:

    | Název | Zdrojový atribut|
    | ---------------| --------------- |
    | Email | user.userprinicipalname |
    | Název organizace | `Your organization name` |

    a. Klikněte na tlačítko **přidat novou deklaraci** otevřít **spravovat deklarace identity uživatelů** dialogového okna.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. V **název** textového pole zadejte název atributu, který je zobrazený pro tento řádek.

    c. Nechte **Namespace** prázdné.

    d. Vyberte zdroj jako **atribut**.

    e. Z **zdrojový atribut** seznamu, zadejte hodnotu atributu zobrazený pro tento řádek.

    f. Klikněte na tlačítko **Ok**

    g. Klikněte na **Uložit**.

8. Na **nastavte si jednotné přihlašování pomocí SAML** stránku, **podpisový certifikát SAML** klikněte na tlačítko **Stáhnout** ke stažení **certifikát (Base64)** z se zadanými možnostmi podle vašich požadavků a uložit je ve vašem počítači.

    ![Odkaz ke stažení certifikátu](common/certificatebase64.png)

9. Na **PureCloud zřízený Genesys** tématu, zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Zkopírování adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Adresa URL – odhlášení

### <a name="configure-purecloud-by-genesys-single-sign-on"></a>Konfigurace PureCloud podle Genesys jednotné přihlašování

1. V jiné okno webového prohlížeče, přihlaste se k PureCloud podle Genesys jako správce.

2. Klikněte na **správce** nahoře a přejděte do **Single Sign-on** pod **integrace**.

    ![Konfigurace jednotného přihlašování](./media/purecloud-by-genesys-tutorial/configure01.png)

3. Přepnout na **služby AD FS a Azure AD(Premium)** kartu a proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/purecloud-by-genesys-tutorial/configure02.png)

    a. Klikněte na tlačítko **Procházet** nahrát base-64 kódování certifikátu, který jste si stáhli z portálu Azure portal do **certifikát služby AD FS**.

    b. V **URI vystavitele služby AD FS** textového pole vložte hodnotu **Azure AD identifikátor** který jste zkopírovali z portálu Azure portal.

    c. V **cílový identifikátor URI** textového pole vložte hodnotu **přihlašovací adresa URL** který jste zkopírovali z portálu Azure portal.

    d. Pro **identifikátor předávající strany** hodnotu, budete muset přejít na portál Azure Portal, na **PureCloud podle Genesys** stránky integrace aplikace, klikněte na **vlastnosti** karty a kopii **ID aplikace** hodnotu. Vložte ji **identifikátor předávající strany** textového pole. 

    ![Konfigurace jednotného přihlašování](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. Klikněte na **Uložit**.   

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovacího uživatele Azure AD 

Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

1. Na webu Azure Portal, v levém podokně vyberte **Azure Active Directory**vyberte **uživatelé**a pak vyberte **všichni uživatelé**.

    !["Uživatele a skupiny" a "Všechny uživatele" odkazy](common/users.png)

2. Vyberte **nového uživatele** v horní části obrazovky.

    ![Tlačítko Nový uživatel](common/new-user.png)

3. Ve vlastnosti uživatele proveďte následující kroky.

    ![Dialogové okno uživatele](common/user-properties.png)

    a. V **název** zadat **BrittaSimon**.
  
    b. V **uživatelské jméno** typ pole **brittasimon@yourcompanydomain.extension**  
    Například BrittaSimon@contoso.com.

    c. Vyberte **zobrazit heslo** zaškrtněte políčko a zapište si hodnotu, která se zobrazí v poli heslo.

    d. Klikněte na možnost **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit uživatele Azure AD

V této části je povolit Britta Simon používat jednotné přihlašování Azure díky udělení přístupu k PureCloud podle Genesys.

1. Na webu Azure Portal, vyberte **podnikové aplikace**vyberte **všechny aplikace**a pak vyberte **PureCloud podle Genesys**.

    ![Okno aplikace organizace](common/enterprise-applications.png)

2. V seznamu aplikací vyberte **PureCloud podle Genesys**.

    ![PureCloud podle Genesys propojení v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **uživatelů a skupin**.

    ![Odkaz "Uživatele a skupiny"](common/users-groups-blade.png)

4. Klikněte na tlačítko **přidat uživatele** tlačítko a pak vyberte **uživatelů a skupin** v **přidat přiřazení** dialogového okna.

    ![Podokno Přidat přiřazení](common/add-assign-user.png)

5. V **uživatelů a skupin** dialogové okno Vybrat **Britta Simon** v seznamu uživatelů, klikněte **vyberte** tlačítko v dolní části obrazovky.

6. Pokud očekáváte libovolnou hodnotu role v kontrolní výraz SAML a potom v **vybrat roli** dialogové okno vybrat vhodnou roli pro uživatele ze seznamu, klikněte **vyberte** tlačítko v dolní části obrazovky.

7. V **přidat přiřazení** dialogové okno kliknutím **přiřadit** tlačítko.

### <a name="create-purecloud-by-genesys-test-user"></a>Vytvoření PureCloud podle Genesys testovacího uživatele

Umožňuje uživatelům Azure AD se přihlaste k PureCloud podle Genesys, musí být poskytnuty do PureCloud podle Genesys. V PureCloud podle Genesys zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, postupujte následovně:**

1. Přihlaste se k PureCloud podle Genesys jako správce.

2. Klikněte na **správce** nahoře a přejděte do **lidé** pod **lidí a oprávnění**.

    ![Konfigurace jednotného přihlašování](./media/purecloud-by-genesys-tutorial/configure03.png)

3. Na stránce uživatelé klikněte na **přidat osobu**.

    ![Konfigurace jednotného přihlašování](./media/purecloud-by-genesys-tutorial/configure04.png)

4. Na **přidat uživatelé v organizaci** rozbalovací, proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/purecloud-by-genesys-tutorial/configure05.png)

    a. V **jméno a příjmení** textové pole, zadejte jméno uživatele, jako je **Brittasimon**.

    b. V **e-mailu** textové pole, zadejte e-mailu uživatele, jako je **brittasimon@contoso.com**.
    
    c. Klikněte na možnost **Vytvořit**.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování 

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po klepnutí PureCloud podle Genesys dlaždici na přístupovém panelu, můžete by měl být automaticky podepsány k PureCloud Genesys, u kterého nastavíte jednotné přihlašování. Další informace o přístupovém panelu, naleznete v tématu [Úvod k přístupovému panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další prostředky

- [ Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

