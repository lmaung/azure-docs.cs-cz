---
title: Sestavy aktivit auditu na portálu Azure Active Directory | Dokumentace Microsoftu
description: Seznámení se sestavami aktivit auditu na portálu Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7f221b815b6800f635c07525fdbd332ac508786
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171532"
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Sestavy aktivit auditu na portálu Azure Active Directory 

Pomocí sestav Azure Active Directory (Azure AD) můžete získat informace, které potřebujete ke zjištění stavu vašeho prostředí.

Architektura generování sestav se skládá z následujících součástí:

- **Aktivita** 
    - **Přihlášení** – [sestavy přihlášení](concept-sign-ins.md) poskytuje informace o využití spravovaných aplikací a uživatel aktivit přihlašování.
    - **Protokoly auditu** – Zajišťuje sledovatelnost prostřednictvím protokolů všech změn provedených různými funkcemi v rámci Azure AD. Mezi příklady protokolů auditu patří změny jakýchkoli prostředků v rámci Azure AD, jako jsou přidávání nebo odebírání uživatelů, aplikace, skupiny, role a zásady.
- **Zabezpečení** 
    - **Riziková přihlášení** – [rizikových přihlášení](concept-risky-sign-ins.md) je indikátorem pokusu přihlásit, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu. 
    - **Uživatelé označení příznakem rizika** – [rizikový uživatel](concept-user-at-risk.md) je indikátorem uživatelského účtu, který mohl být ohrožený.

Tento článek obsahuje přehled sestavy auditu.
 
## <a name="who-can-access-the-data"></a>Kdo má přístup k datům?

* Uživatelé v **zabezpečení uživatelské**, **Čtenář zabezpečení** nebo **globálního správce** role
* Kromě toho všichni uživatelé (bez oprávnění správce) mohou zobrazit své vlastní aktivity auditu

## <a name="audit-logs"></a>Protokoly auditu

Auditování Azure AD protokoly obsahují záznamy aktivit systému kvůli dodržování předpisů. Pro přístup k sestavě auditu, vyberte **protokoly auditu** v **aktivity** část **Azure Active Directory**. Všimněte si, že protokoly auditu mohou latenci až hodinu, tak může trvat dlouho to pro data aktivit auditu se zobrazí na portálu po dokončení úlohy.

![Protokoly auditu](./media/concept-audit-logs/61.png "Protokoly auditu")

Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:

- datum a čas výskytu
- iniciátor/aktér aktivity (*kdo*) 
- aktivita (*co*) 
- cíl

![Protokoly auditu](./media/concept-audit-logs/18.png "Protokoly auditu")

Zobrazení seznamu můžete upravit kliknutím na **Sloupce** na panelu nástrojů.

![Protokoly auditu](./media/concept-audit-logs/19.png "Protokoly auditu")

To umožňuje zobrazit další pole, nebo odebrat pole, která jsou už zobrazená.

![Protokoly auditu](./media/concept-audit-logs/21.png "Protokoly auditu")

Vyberte položku v zobrazení seznamu zobrazíte podrobnější informace.

![Protokoly auditu](./media/concept-audit-logs/22.png "Protokoly auditu")


## <a name="filtering-audit-logs"></a>Filtrování protokolů auditu

Můžete filtrovat data auditu pro následující pole:

- Rozsah dat
- Spustil(a) (činitel)
- Kategorie
- Typ prostředku aktivity
- Aktivita

![Protokoly auditu](./media/concept-audit-logs/23.png "Protokoly auditu")

Filtr pro **rozsah dat** umožňuje definovat časový rámec pro vracená data.  
Možné hodnoty:

- 1 měsíc
- 7 dní
- 24 hodin
- Vlastní

Když vyberete vlastní časový rámec, můžete nakonfigurovat počáteční a koncový čas.

**Iniciovaných** filtr umožňuje definovat jméno prvek "actor" nebo univerzální hlavní název (UPN).

Filtr **Kategorie** umožňuje vybrat jeden z následujících filtrů:

- Vše
- Základní kategorie
- Základní adresář
- Samoobslužná správa hesel
- Samoobslužná správa skupin
- Zřizování účtů – automatická změna hesel
- Pozvaní uživatelé
- Služba MIM
- Identity Protection
- B2C

Filtr **Typ prostředku aktivity** umožňuje vybrat jeden z následujících filtrů:

- Vše 
- Skupina
- Adresář
- Uživatel
- Aplikace
- Zásada
- Zařízení
- Ostatní

Když jako **Typ prostředku aktivity** vyberete **Skupina**, zobrazí se další kategorie filtru, která umožňuje zadat také **Zdroj**:

- Azure AD
- O365


**Aktivity** filtr podle kategorií a aktivit prostředků typ výběru provedete. Můžete vybrat konkrétní aktivitu, kterou chcete zobrazit, nebo zvolit všechny. 

Seznam všech aktivit auditu můžete získat pomocí Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, kde $tenantdomain = název domény. Také se můžete podívat na článek o [událostech sestavy auditování](reference-audit-activities.md).

## <a name="audit-logs-shortcuts"></a>Zástupci pro protokoly auditu

Kromě **Azure Active Directory** poskytuje web Azure Portal dva další vstupní body k datům auditu:

- Uživatelé a skupiny
- Podnikové aplikace

### <a name="users-and-groups-audit-logs"></a>Protokoly auditu uživatelů a skupin

S použitím sestav auditu orientovaných na uživatele a skupiny můžete najít odpovědi na otázky tohoto typu:

- Jaké typy aktualizací uživatelé použili?

- Kolik uživatelů bylo změněno?

- Kolik hesel bylo změněno?

- Co provedl správce v adresáři?

- Které skupiny byly přidány?

- Došlo u některých skupin ke změnám členství?

- Došlo ke změnám vlastníků skupiny?

- Jaké licence byly přiřazeny skupině nebo uživateli?

Pokud chcete jenom zkontrolovat data auditování týkající se uživatelů a skupin, najdete filtrované zobrazení v sekci **Protokoly auditu** v oddílu **Aktivity** v části **Uživatelé a skupiny**. Tento vstupní bod má jako **Typ prostředku aktivity** předem vybranou možnost **Uživatelé a skupiny**.

![Protokoly auditu](./media/concept-audit-logs/93.png "Protokoly auditu")

### <a name="enterprise-applications-audit-logs"></a>Protokoly auditu podnikových aplikací

S použitím sestav auditu orientovaných na aplikace můžete najít odpovědi na otázky tohoto typu:

* Které aplikace byly přidány nebo aktualizovány?
* Které aplikace byly odebrány?
* Změnil se instanční objekt pro aplikaci?
* Změnily se názvy aplikací?
* Kdo udělil souhlas pro aplikaci?

Pokud chcete zkontrolovat data auditování týkající se aplikací, najdete filtrované zobrazení v sekci **protokoly auditu** v **aktivity** část **podnikové aplikace** okno. Tento vstupní bod má **podnikové aplikace** Zkontrolujte předem vybrané jako **typ prostředku aktivity**.

![Protokoly auditu](./media/concept-audit-logs/134.png "Protokoly auditu")

Můžete filtrovat toto zobrazení dolů na **skupiny** nebo **uživatelé**.

![Protokoly auditu](./media/concept-audit-logs/25.png "Protokoly auditu")

## <a name="office-365-activity-logs"></a>Protokoly aktivit Office 365

Můžete zobrazit protokoly aktivit Office 365 z [centrum pro správu Office 365](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). I když aktivita Office 365 a Azure AD aktivity protokoly sdílejí velké množství prostředků adresáře pouze Office 365 centru pro správu poskytuje úplný přehled protokolů aktivit Office 365. 

Programově pomocí protokolů aktivit Office 365 se dá dostat taky [rozhraní API pro správu Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Další postup

- [Referenční informace aktivit auditu Azure AD](reference-audit-activities.md)
- [Odkaz na uchování sestavy Azure AD](reference-reports-data-retention.md)
- [Odkazovat na latenci protokolu Azure AD](reference-reports-latencies.md)
