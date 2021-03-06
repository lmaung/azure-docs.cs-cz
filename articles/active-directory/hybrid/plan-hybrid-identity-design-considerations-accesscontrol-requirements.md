---
title: Hybridní identita návrhu požadavky na řízení přístupu Azure | Dokumentace Microsoftu
description: Obsahuje identitu a identifikuje požadavky na přístup k prostředkům pro uživatele v hybridním prostředí.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84b786a1701892823554a83fa2015ac88d6eff4d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56197381"
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Určete požadavky na řízení přístupu pro vaše řešení hybridní identity
Organizace je návrh jejich řešení hybridní identity, může také použít tuto příležitost zkontrolovat požadavky na přístup pro prostředky, které jsou plánování a zpřístupnit ji pro uživatele. Přístup k datům napříč všechny čtyři pilíře identity, které jsou:

* Správa
* Authentication
* Autorizace
* Auditování

Následující části budou zahrnovat ověřování a autorizace podrobněji ukáží, Správa a auditování jsou součástí životního cyklu hybridní identity. Čtení [určit úlohy správy hybridních identit](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) pro další informace o těchto funkcích.

> [!NOTE]
> Čtení [čtyři pilíře z Identity – Správa identit ve věku hybridní IT](https://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) Další informace o každé z těchto pilíře.
> 
> 

## <a name="authentication-and-authorization"></a>Ověřování a autorizace
Existují různé scénáře pro ověřování a autorizaci, těchto scénářů bude mít konkrétní požadavky, které musí být splněny, řešení hybridní identity, které společnost přechází na přijmout. Scénáře zahrnující komunikaci B2b (B2B) můžete přidat další výzvy pro správce IT, protože je potřeba zajistit, že metodu ověřování a autorizace používá organizace může komunikovat s jejich obchodní partneři. Během procesu návrhu pro požadavky na ověřování a autorizace Ujistěte se, že jsou odpovědi na následující otázky:

* Bude vaše organizace ověřování a autorizace pouze uživatelů nachází v jejich systému pro správu identity?
  * Existují nějaké plány pro scénáře B2B?
  * Pokud ano, už víte, které protokoly (SAML, OAuth, Kerberos nebo certifikáty) se použije pro připojení obou firem?
* Podporuje řešení hybridní identity, která se má přijmout podporu těchto protokolů?

Dalším důležitým bodem ke zvážení je, kde budou umístěné ověřování úložiště, který se použije uživatele a partnery a modelu správy, který se má použít. Vezměte v úvahu následující dvě jádra:

* Centralizované: v tomto modelu, přihlašovací údaje, zásady a správu uživatele může být centralizovaný místně nebo v cloudu.
* Hybridní: v tomto modelu, přihlašovací údaje uživatele, zásad a správu bude centralizovaný místně a replikované v cloudu.

Který model přijímají vaší organizaci se bude lišit podle svých obchodních požadavků, chcete odpovědět na následující otázky k identifikaci, kde se bude nacházet systému pro správu identit a správy režim:

* Má vaše organizace aktuálně správy identit v místním?
  * Pokud ano, plánují zajistit jeho?
  * Existují nějaké požadavky na dodržování předpisů nebo legislativě na, že vaše organizace musí dodržovat tento určují kde by měl být uložený systému pro správu identit?
* Vaše organizace používá jednotné přihlašování pro aplikace umístěné v místním nebo v cloudu?
  * Pokud ano, přijetí modelu hybridní identity vliv tohoto procesu?

## <a name="access-control"></a>Řízení přístupu
Ověřování a autorizace jsou základní prvky pro povolení přístupu k firemním datům prostřednictvím ověření uživatele, je také důležité určit úroveň přístupu, který bude mít tyto uživatele a úroveň přístupu správce bude mít nad prostředky, které jestli se správou. Hybridní řešení identit musí být schopný poskytnout granulární přístup k prostředkům, delegování a řízení přístupu na základě rolí. Ujistěte se, že následující dotaz zodpovězen týkající se řízení přístupu:

* Má vaše společnost více než jeden uživatel se zvýšenými oprávněními ke správě vašeho systému identit?
  * Pokud ano, každý uživatel potřebuje stejnou úroveň přístupu?
* Třeba pro delegování přístupu uživatelům ke správě konkrétní prostředky společnosti?
  * Pokud ano, jak často to se stane?
* Integrace řízení přístupu určené mezi místním prostředím a cloudem třeba vaše společnost prostředky?
* By k omezení přístupu k prostředkům podle některé podmínky musí vaše společnost?
* Má vaše společnost jakékoli aplikaci, která vyžaduje vlastní řízení přístupu k některým prostředkům?
  * Pokud ano, ve kterém jsou tyto aplikace umístěné (místní nebo v cloudu)?
  * Pokud ano, ve kterém jsou tyto cílové prostředky umístěné (místní nebo v cloudu)?

> [!NOTE]
> Ujistěte se, že každá odpověď a pochopení odůvodnění odpověď. [Definování strategie ochrany dat](plan-hybrid-identity-design-considerations-data-protection-strategy.md) možností a výhod i nevýhod každé z.  Díky zodpovězení těchto otázek bude vyberete, která možnost nejlépe vyhovuje vašim obchodním potřebám.
> 
> 

## <a name="next-steps"></a>Další postup
[Určení požadavků na reakce na incidenty](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](plan-hybrid-identity-design-considerations-overview.md)

