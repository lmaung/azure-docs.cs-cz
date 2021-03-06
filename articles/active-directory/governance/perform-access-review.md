---
title: Zkontrolovat přístup skupinám nebo aplikacím v kontrol přístupu Azure AD | Dokumentace Microsoftu
description: Zjistěte, jak kontrolovat přístup členů skupiny nebo přístupu k aplikacím v Azure Active Directory kontroly přístupu.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 097d230e919e6d4b56e6c677364610bda6630f75
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728382"
---
# <a name="review-access-to-groups-or-applications-in-azure-ad-access-reviews"></a>Zkontrolovat přístup skupinám nebo aplikacím v kontrol přístupu Azure AD

Azure Active Directory (Azure AD) zjednodušuje, jak podniky spravovat přístup ke skupinám a aplikacím ve službě Azure AD a druhou Microsoft Online Services s funkcí s názvem kontrol přístupu Azure AD.

Tento článek popisuje, jak kontrolora určené provádí kontroly přístupu pro členy skupiny nebo uživatelé s přístupem k aplikaci.

## <a name="open-the-access-review"></a>Otevřete kontroly přístupu.

Prvním krokem k provádění kontroly přístupu je k vyhledání a otevření kontroly přístupu.

1. Vyhledejte e-mailu od Microsoftu, která vás vyzve k kontrolovat přístup. Tady je příklad e-mailu ke kontrole přístupu pro skupinu.

    ![Kontrola přístupu k e-mailu](./media/perform-access-review/access-review-email.png)

1. Klikněte na tlačítko **spuštění kontroly** odkaz k otevření kontroly přístupu.

Pokud nemáte k dispozici v e-mailu, zjistíte, že se čeká na přístup kontroly pomocí následujících kroků.

1. Přihlaste se k portálu MyApps na [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![Portálu MyApps](./media/perform-access-review/myapps-access-panel.png)

1. V pravém horním rohu stránky klikněte na symbol uživatele, která zobrazuje název a výchozí organizace. Pokud je uveden více než jedné organizace, vyberte organizace, která požadované kontroly přístupu.

1. Na pravé straně stránky klikněte na tlačítko **kontrol přístupu** dlaždici zobrazíte seznam kontrol přístupu.

    Pokud není zobrazena na dlaždici, neexistují žádné kontroly přístupu k provedení pro danou organizaci a v tuto chvíli není potřeba žádná akce.

    ![Seznam kontrol přístupu](./media/perform-access-review/access-reviews-list.png)

1. Klikněte na tlačítko **začít kontrolu** odkaz pro kontroly přístupu, kterou chcete provést.

## <a name="perform-the-access-review"></a>Provádění kontroly přístupu

Po spuštění kontroly přístupu, zobrazí jména uživatelů, kteří potřebují ke kontrole.

Pokud požadavek má-li zkontrolovat svůj vlastní přístup, bude stránka vypadat jiný. Další informace najdete v tématu [zkontrolujte přístup skupinám nebo aplikacím](review-your-access.md).

![Provádění kontroly přístupu](./media/perform-access-review/perform-access-review.png)

Existují dva způsoby, jak můžete schválit nebo odepřít přístup:

- Můžete schválit nebo zamítnout každý požadavek jednotlivě, nebo
- Můžete přijmout doporučení systému, což je nejjednodušší a nejrychlejší způsob.

### <a name="approve-or-deny-access-for-each-request"></a>Schvalte nebo zamítněte přístup pro každý požadavek

1. Projděte si seznam uživatelů, můžete rozhodnout, jestli Schvalte nebo zamítněte jejich přístup.

1. Schválit nebo zamítnout každý požadavek, klikněte na něj a otevřete okno k určení akce má být provedena.

1. Klikněte na tlačítko **schválit** nebo **Odepřít**. Pokud si nejste jistí, můžete kliknout na **Nevím**. To způsobí zůstane zachována jeho přístupu uživatele, ale výběr se projeví v protokolech auditování.

    ![Provádění kontroly přístupu](./media/perform-access-review/approve-deny.png)

    Správce kontroly přístupu může vyžadovat, abyste zadali důvod, proč schvalujete přístup nebo členství ve skupině.

1. Po zadání akce má být provedena, klikněte na tlačítko **Uložit**.

    Pokud chcete změnit vaše odpověď, vyberte řádek a aktualizujte odpovědi. Můžete například dříve odepření uživatel schválit nebo zamítnout dříve schválené uživatele. Vaše odpověď můžete změnit kdykoli až do ukončení kontroly přístupu.

    Pokud existuje více revidujících, se zaznamená poslední odeslané odpovědi. Vezměte v úvahu příklad určuje, kde správce dvě revidující – Alice a Bob. Alice kontroly přístupu. Otevře se nejdřív a schvalovat přístup. Před přezkum ukončí, Bob otevře kontrolu přístupu a odepře přístup. Poslední odepřít, že odpověď je, co je zaznamenán.

    > [!NOTE]
    > Pokud je uživateli odepřen přístup, se neodeberou okamžitě. Odstraní se, když kontrola skončila, nebo při zastavení revize správce.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Schvalte nebo zamítněte přístup na základě doporučení služby

Chcete-li kontroly přístupu pro vás snadnější a rychlejší, poskytujeme také doporučení, která může přijmout jediným kliknutím. Doporučení jsou generovány podle aktivit přihlašování uživatele.

1. Na modrém panelu v dolní části stránky klikněte na tlačítko **přijmout doporučení**.

    ![Přijmout doporučení](./media/perform-access-review/accept-recommendations.png)

    Zobrazí souhrn doporučené akce.

    ![Přijmout doporučení souhrnu](./media/perform-access-review/accept-recommendations-summary.png)

1. Klikněte na tlačítko **Ok** tak, aby přijímal doporučení.

## <a name="next-steps"></a>Další postup

- [Dokončení kontroly přístupu skupiny nebo aplikace](complete-access-review.md)
