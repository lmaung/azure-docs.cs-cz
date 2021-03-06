---
title: Potíže při přihlašování k aplikaci Galerie Azure AD konfigurované pro heslo jednotného přihlašování | Dokumentace Microsoftu
description: Tento článek popisuje problémových oblastí, které poskytují pokyny k odstraňování problémů souvisejících s přihlašováním k aplikacím Galerie Azure AD nakonfigurované pro heslo jednotného přihlašování
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad844f208045c22afb2a22f7b167c6ecd1632d81
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173836"
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Potíže při přihlašování k aplikaci Galerie Azure AD konfigurované pro heslo jednotného přihlašování

Přístupový Panel je webový portál, který uživatel, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) umožňuje zobrazovat a spouštět cloudové aplikace, které jim správce Azure AD udělil přístup k. Uživatel, který má edice Azure AD můžete použít rovněž samoobslužné skupiny a možnosti správy aplikací přes přístupový Panel. Přístupový Panel je nezávislý na webu Azure Portal a nevyžaduje, aby uživatelé měli předplatné Azure.

Chcete-li použít založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalovaná rozšíření přístupového panelu v prohlížeči uživatele. Toto rozšíření se automaticky stáhne, když uživatel vybere, který je nakonfigurovaný pro jednotné přihlašování pomocí hesla aplikace.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Splňuje požadavky na prohlížeč pro na přístupovém panelu

Přístupový Panel vyžaduje prohlížeč, který podporuje JavaScript a má povolený šablon stylů CSS. Chcete-li použít založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalovaná rozšíření přístupového panelu v prohlížeči uživatele. Toto rozšíření se automaticky stáhne, když uživatel vybere, který je nakonfigurovaný pro jednotné přihlašování pomocí hesla aplikace.

Pro jednotné přihlašování založené na heslech může být koncového uživatele prohlížeče:

-   Aplikace Internet Explorer 8, 9, 10, 11 – ve Windows 7 nebo novější

-   Chrome – Ve Windows 7 nebo novější a v systému MacOS X nebo novější

-   Firefox 26.0 nebo později – na Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější

>[!NOTE]
>Rozšíření založené na heslech jednotného přihlašování k dispozici pro Microsoft Edge ve Windows 10 při rozšíření prohlížeče se nepodporuje pro Microsoft Edge.
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a>Postup instalace rozšíření prohlížeče Panel přístupu

K instalaci rozšíření prohlížeče panelu přístup, postupujte podle následujících kroků:

1.  Otevřít [přístupového panelu](https://myapps.microsoft.com) v jednom z podporovaných prohlížečů a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **heslem jednotného přihlašování aplikace** na přístupovém panelu.

3.  V příkazovém řádku s výzvou k instalaci softwaru vyberte **nainstalovat**.

4.  Závislosti na vašem prohlížeči budete přesměrováni na odkaz ke stažení. **Přidat** rozšíření do prohlížeče.

5.  Pokud vás prohlížeč vyzve, vyberte buď **povolit** nebo **povolit** rozšíření.

6.  Po instalaci **restartovat** relace prohlížeče.

7.  Přihlaste se na přístupovém panelu a zobrazit, pokud můžete **spuštění** vaše heslo jednotného přihlašování aplikace

Rozšíření pro Chrome a Firefox lze také stáhnout z přímé odkazy níže:

-   [Rozšíření Chrome přístupového panelu](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření Firefox přístupového panelu](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Nastavení zásad skupiny pro aplikaci Internet Explorer

Můžete nastavit zásady skupiny, který umožňuje vzdálenou instalaci rozšíření přístupový Panel pro aplikaci Internet Explorer v počítačích uživatelů.

Požadované součásti patří:

-   Nastavíte [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a vaši uživatelé počítačů se připojili k vaší doméně.

-   Musíte mít oprávnění "Upravit nastavení" k úpravě objektu zásad skupiny (GPO). Ve výchozím nastavení mají toto oprávnění členy z těchto skupin zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners. [Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Postupujte podle kurzu [jak pro aplikaci Internet Explorer pomocí zásad skupiny nasadit rozšíření přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) pro podrobné pokyny o tom, jak nakonfigurovat zásady skupiny a nasadit je uživatelům.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Řešení potíží s přístupového panelu v aplikaci Internet Explorer

Postupujte podle [řešení potíží s rozšířením Panel přístupu pro aplikaci Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) Příručka pro přístup Diagnostika a podrobné pokyny ke konfiguraci rozšíření pro aplikaci Internet Explorer.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Postup konfigurace hesel jednotného přihlašování pro aplikaci mimo Galerii

Konfigurace aplikace z Galerie Azure AD, které je potřeba:

-   [Přidat aplikaci mimo Galerii](#add-a-non-gallery-application)

-   [Konfigurace aplikace pro heslo jednotného přihlašování](#configure-the-application-for-password-single-sign-on)

-   [Přiřazení uživatelů k aplikaci](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a>Přidat aplikaci mimo Galerii

Přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:

1.  Otevřít [webu Azure portal](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřít **rozšíření Azure Active Directory** kliknutím **všechny služby** v horní části hlavní navigační nabídce vlevo.

3.  Zadejte **"Azure Active Directory**" do vyhledávacího pole filtrovat a vybrat **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace** levé navigační nabídce Azure Active Directory.

5.  Klikněte na tlačítko **přidat** tlačítko v pravém horním rohu **podnikové aplikace** podokně.

6.  Klikněte na tlačítko **aplikace mimo galerii.**

7.  Zadejte název vaší aplikace v **název** textového pole. Vyberte **přidat.**

Po krátké době budete moci zobrazit podokno konfiguračních vaší aplikace.

### <a name="configure-the-application-for-password-single-sign-on"></a>Konfigurace aplikace pro heslo jednotného přihlašování

Pokud chcete nakonfigurovat jednotné přihlašování pro aplikaci, použijte následující postup:

1.  Otevřít [ **webu Azure portal** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **Spolusprávce.**

2.  Otevřít **rozšíření Azure Active Directory** kliknutím **všechny služby** v horní části hlavní navigační nabídce vlevo.

3.  Zadejte **"Azure Active Directory**" do vyhledávacího pole filtrovat a vybrat **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace** levé navigační nabídce Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** zobrazíte seznam všech aplikací.

   * Pokud nevidíte aplikaci, kterou má zobrazit tady, použijte **filtr** ovládacího prvku v horní části **seznam všech aplikací** a nastavit **zobrazit** umožňuje **všechny Aplikace.**

6.  Vyberte aplikaci, kterou chcete nakonfigurovat jednotné přihlašování

7.  Po načtení aplikace, klikněte na tlačítko **jednotného přihlašování** levé navigační nabídce aplikace.

8.  Vyberte režim **přihlašování na základě heslo.**

9.  Zadejte **přihlašovací adresa URL**. Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci. Zkontrolujte, že přihlášení pole jsou viditelné na adrese URL.

10. Přiřazení uživatelů k aplikaci.

11. Kromě toho můžete také zadat přihlašovací údaje jménem uživatele výběr řádků uživatele a kliknutím na **aktualizaci přihlašovacích údajů** a zadáním uživatelského jména a hesla jménem uživatele. V opačném případě uživatelé vyzváni k zadání samotné přihlašovací údaje při spuštění.

### <a name="assign-users-to-the-application"></a>Přiřazení uživatelů k aplikaci

Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:

1.  Otevřít [ **webu Azure portal** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřít **rozšíření Azure Active Directory** kliknutím **všechny služby** v horní části hlavní navigační nabídce vlevo.

3.  Zadejte **"Azure Active Directory**" do vyhledávacího pole filtrovat a vybrat **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace** levé navigační nabídce Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** zobrazíte seznam všech aplikací.

   * Pokud nevidíte aplikaci, kterou má zobrazit tady, použijte **filtr** ovládacího prvku v horní části **seznam všech aplikací** a nastavit **zobrazit** umožňuje **všechny Aplikace.**

6.  Vyberte aplikaci, kterou chcete přiřadit uživatele ze seznamu.

7.  Po načtení aplikace, klikněte na tlačítko **uživatelů a skupin** levé navigační nabídce aplikace.

8.  Klikněte na tlačítko **přidat** tlačítko nahoře **uživatelů a skupin** seznamu a otevře **přidat přiřazení** podokně.

9.  Klikněte na tlačítko **uživatelů a skupin** selektor z **přidat přiřazení** podokně.

10. Zadejte **celý název** nebo **e-mailová adresa** uživatele zájem o přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Najeďte myší **uživatele** v seznamu zobrazíte **zaškrtávací políčko**. Klikněte na zaškrtávací políčko vedle profilové fotky uživatele nebo logo, které chcete přidat uživatele **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** vyhledávací pole a klikněte na zaškrtávací políčko a přidáním tohoto uživatele do **vybrané** seznamu.

13. Po dokončení výběru uživatelů, klikněte na tlačítko **vyberte** tlačítko pro přidání do seznamu uživatelů a skupin pro přiřazení k aplikaci.

14. **Volitelné:** klikněte na tlačítko **vybrat roli** oblasti pro výběr **přidat přiřazení** podokně vyberte roli, kterou chcete přiřadit uživatelům, které jste vybrali.

15. Klikněte na tlačítko **přiřadit** tlačítko přiřadit aplikaci do vybraného uživatele.

Po krátké době uživatele, které jste vybrali dát spustit tyto aplikace na přístupovém panelu.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Pokud tyto kroky pro řešení potíží není vyřešit problém

vytvořit lístek podpory s použitím následujících informací, pokud je k dispozici:

-   Chyba ID korelace

-   Hlavní název uživatele (uživatel e-mailová adresa)

-   ID Tenanta

-   Typ prohlížeče

-   Vyvolá se časové pásmo a čas nebo časový rámec při chybě

-   Trasování Fiddler

## <a name="next-steps"></a>Další postup
[Poskytovat jednotné přihlašování do aplikací pomocí Proxy aplikace](application-proxy-configure-single-sign-on-with-kcd.md)

