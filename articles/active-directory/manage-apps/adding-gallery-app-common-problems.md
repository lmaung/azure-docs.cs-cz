---
title: Potíže při přidávání aplikace Galerie Azure AD | Dokumentace Microsoftu
description: Vysvětlení běžných tváří lidí problémy při přidávání aplikace Galerie Azure AD a co můžete dělat k jejich řešení
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
ms.date: 09/11/2018
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe781802309ad0945eaee23c35dda1617e47ae06
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2019
ms.locfileid: "56727345"
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Potíže při přidávání aplikace Galerie Azure AD

Tento článek vám pomůže pochopit běžné tváří lidí problémy při přidávání aplikace Galerie Azure AD a co můžete udělat, abyste je vyřešit.

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>Po klepnutí na tlačítko "Přidat" a Moje aplikace trvalo dlouhou dobu zobrazovat

Za určitých okolností může trvat 1 – 2 minutách (a někdy delší) pro aplikace se zobrazí po jeho přidání do vašeho adresáře. Normální očekávaný výkon tak není, zobrazí se přidání aplikace se po kliknutí na **oznámení** v pravém horním rohu ikonu (zvonek) [webu Azure portal](https://portal.azure.com/) a vyhledávání pro **probíhá** nebo **dokončeno** oznámení s názvem **přidává se aplikace.**

Pokud vaše aplikace se nikdy nepřidávali nebo dojde k chybě při kliknutí **přidat** tlačítko, zobrazí se vám **oznámení** v **chyba** stavu. Pokud chcete podrobnosti o této chybě Další informace o nebo sdílet s pracovníkem technické podpory, zobrazí se další informace o chybě pomocí následujících kroků v [jak zobrazit podrobnosti o oznámení na portálu](#how-to-see-the-details-of-a-portal-notification) oddílu.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>Po klepnutí na tlačítko "Přidat" a nezobrazilo Moje aplikace

V některých případech z důvodu přechodné problémy, problémy se sítí nebo chybu, přidání aplikace se nezdaří. Poznáte, to se stane, když kliknete **oznámení** ikonu (zvonek) v pravém horním rohu webu Azure portal a můžete zobrazit ikonou červené (!) vedle vašeho **přidává se aplikace** oznámení. To znamená, že došlo k chybě při vytváření aplikace.

Pokud narazíte na chybu při kliknutí **přidat** tlačítko, zobrazí se vám **oznámení** v **chyba** stavu. Pokud chcete podrobnosti o této chybě Další informace o nebo sdílet s pracovníkem technické podpory, zobrazí se další informace o chybě pomocí následujících kroků v [jak zobrazit podrobnosti o oznámení na portálu](#how-to-see-the-details-of-a-portal-notification) oddílu.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Nevím, jak nastavit Moje aplikace až po jeho přidání

Pokud potřebujete pomoc, další informace o aplikacích [seznam kurzů o integraci aplikací typu SaaS pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) článku je dobrým začátkem.

Kromě toho [knihovny dokumentů aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) vám umožní získat další informace o jednotné přihlašování s Azure AD a jak to funguje.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Jak zobrazit podrobnosti o oznámení na portálu

Můžete zobrazit podrobnosti o oznámení portálu podle následujících kroků:

1.  Vyberte **oznámení** ikonu (zvonek) v pravém horním rohu webu Azure Portal

2.  Vyberte všechna oznámení v **chyba** stavu (ty s červenou (!) vedle sebe).

    >[!NOTE]
    >Nelze klikněte na oznámení v **úspěšné** nebo **probíhá** stavu.
    >
    >

4.  Použijte informace v části **podrobnosti oznámení** pochopit podrobnosti o problému.

5.  Pokud stále potřebujete pomoc, můžete také sdílet tyto informace s pracovníkem technické podpory nebo produktovou skupinou účelem vyřešení vašeho problému.

6.  Klikněte na tlačítko **kopírování** **ikonu** napravo od **Kopírovat chybu** textového pole zkopírujte všechny podrobnosti oznámení sdílet s pracovníkem skupiny podpory nebo produktu.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Jak získat nápovědu odesláním oznámení podrobnosti pro pracovníka podpory

Je velmi důležité, jakým sdílíte **níže uvedených údajů** s pracovníkem technické podpory Pokud potřebujete pomoc, aby se vám může pomoct rychle. Můžete udělat jednoduše podle **snímek,** nebo kliknutím **ikona chyby kopírování**napravo od byl nalezen **Kopírovat chybu** textového pole.

## <a name="notification-details-explained"></a>Vysvětlení podrobnosti o oznámení

Přečtěte si následující popisy pro další podrobnosti o oznámení.

### <a name="essential-notification-items"></a>Základní oznámení položky

-   **Název** – popisný název oznámení

  * Příklad – **nastavení proxy aplikace.**

-   **Popis** – popis, k čemu došlo v důsledku operace

    -   Příklad – **zadanou vnitřní adresu url se už používá jiná aplikace**

-   **ID oznámení** – jedinečné ID oznámení

    -   Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **ID žádosti klienta** – ID konkrétní žádosti od prohlížeče

    -   Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Časové razítko UTC** – časové razítko, během které oznámení došlo k chybě, ve standardu UTC

    -   Příklad – **2017-03-23T19:50:43.7583681Z**

-   **Interní ID transakce** – interní ID můžeme použít k vyhledání Chyba v našem systému

    -   Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **Hlavní název uživatele** – uživatel, který provedl operaci

    -   Příklad: **tperkins@f128.info**

-   **ID tenanta** – jedinečné ID tenanta, který byl členem skupiny uživatele, který provedl operaci

    -   Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **ID objektu uživatele** – jedinečné ID uživatele, který provedl operaci

    -   Příklad – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Podrobný oznamovací položky

-   **Zobrazovaný název** – **(může být prázdné)** podrobnější zobrazovaný název pro chybu

    -   Příklad – **nastavení proxy aplikace.**

-   **Stav** – konkrétní stavové oznámení

    -   Příklad – **se nezdařilo**

-   **ID objektu** – **(může být prázdné)** ID objektu, proti kterému byla provedena operace

    -   Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Podrobnosti o** – podrobný popis, k čemu došlo v důsledku operace

    -   Příklad – **interní adresa url 'https://bing.com/"je neplatná, protože se už používá**

-   **Chyba při kopírování** – klikněte na tlačítko **ikonu kopírování** napravo od **Kopírovat chybu** textového pole zkopírujte všechny podrobnosti oznámení ke sdílení se skupinou pro podporu nebo produktu 
-   engineer (technik)

    -   Příklad ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

