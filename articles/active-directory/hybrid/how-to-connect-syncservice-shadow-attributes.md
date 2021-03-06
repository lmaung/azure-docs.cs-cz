---
title: Azure AD Connect sync služby stínové atributy | Dokumentace Microsoftu
description: Popisuje, jak fungují stínové atributů ve službě Azure AD Connect sync.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 564a0948d09e4726800d19858b4c23924fc56973
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2019
ms.locfileid: "56187334"
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect sync služby stínové atributy
Většina atributy jsou reprezentovány stejným způsobem jako ve službě Azure AD se nacházejí v místní Active Directory. Ale některé atributy mají některé zvláštní zpracování a hodnota atributu ve službě Azure AD může být jiný než Azure AD Connect synchronizuje.

## <a name="introducing-shadow-attributes"></a>Představujeme stínové atributy
Některé atributy mají dvě vyjádření ve službě Azure AD. Uloží hodnotu místní a počítané hodnoty. Tyto atributy navíc se nazývají stínové atributy. Jsou dva nejběžnější atributy, kde uvidíte toto chování **userPrincipalName** a **proxyAddress**. Změna hodnoty atributu se stane, když jsou hodnoty v těchto atributů reprezentující neověřených domén. Ale synchronizační modul ve službě Connect přečte hodnotu v atributu stínu tak z hlediska jeho, atribut byl potvrzen pomocí Azure AD.

Nejde zobrazit atributy stínové pomocí Azure portal nebo Powershellu. Ale Principy koncept pomáhá při řešení některých scénářích, kde atribut má jiné hodnoty v místním prostředí i v cloudu.

Abyste lépe pochopili chování, podívejte se na tomto příkladu ze společnosti Fabrikam:  
![Domény](./media/how-to-connect-syncservice-shadow-attributes/domains.png)  
Mají více přípon UPN ve svojí místní službě Active Directory, ale jeden pouze ověření.

### <a name="userprincipalname"></a>userPrincipalName (Hlavní název uživatele)
Uživatel má následující hodnoty atributu v neověřenou doménu:

| Atribut | Hodnota |
| --- | --- |
| místní atribut userPrincipalName | lee.sperry@fabrikam.com |
| Azure AD shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

Atribut userPrincipalName je hodnota, které se zobrazí při použití prostředí PowerShell.

Protože skutečné místní hodnota atributu je uložená ve službě Azure AD, když ověření domény fabrikam.com, Azure AD aktualizuje atribut userPrincipalName s použitím hodnoty z shadowUserPrincipalName. Není nutné synchronizovat všechny změny z Azure AD Connect pro tyto hodnoty aktualizovat.

### <a name="proxyaddresses"></a>proxyAddresses
Stejný postup pro pouze včetně ověřených domén také vyvolá proxyAddresses, ale některé další logiku. Vyhledat ověřených domén dochází jenom pro poštovní schránky uživatele. Poštovní uživatel nebo kontakt představují uživatele v jiné organizaci Exchange a všechny hodnoty v poli proxyAddresses. můžete přidat k těmto objektům.

Uživatel poštovní schránky, ať už místní nebo v Exchangi Online se zobrazí pouze hodnoty ověřených domén. Může vypadat například takto:

| Atribut | Hodnota |
| --- | --- |
| místní proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

V tomto případě **smtp:abbie.spencer@fabrikam.com** byl odebrat, protože nebyla ověřena tuto doménu. Výměna také přidá, ale **SIP:abbie.spencer@fabrikamonline.com**. Společnost Fabrikam nebyl použit, Lync nebo Skype místní, ale Azure AD a Exchange Online připravte se na to.

Tuto logiku pro proxyAddresses se označuje jako **ProxyCalc**. ProxyCalc je vyvolána při každé změně na uživatele při:

- Uživateli se přiřadila plán služeb, které zahrnuje Exchange Online i v případě, že se uživatel licenci pro Exchange. Například, pokud uživatel je přiřazena skladové položky Office E3, ale pouze byl přiřazen Sharepointu Online. To platí i v případě, že vaší poštovní schránce je stále místně.
- MsExchRecipientTypeDetails atribut má hodnotu.
- Provedete změnu proxyAddresses nebo userPrincipalName.

ProxyCalc může trvat nějakou dobu zpracování změn na uživatele a není synchronní s procesem exportu služby Azure AD Connect.

> [!NOTE]
> ProxyCalc logic obsahuje některé další chování pro pokročilé scénáře, které nejsou uvedené v tomto tématu. Toto téma poskytuje pochopit chování a není dokumentu všechny interní logiku.

### <a name="quarantined-attribute-values"></a>Hodnoty atributů v karanténě
Stín atributy se používají také po duplicitními hodnotami atributů. Další informace najdete v tématu [odolnost duplicitních atributů](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Další informace najdete v tématech
* [Synchronizace služby Azure AD Connect](how-to-connect-sync-whatis.md)
* [Integrace místních identit s Azure Active Directory](whatis-hybrid-identity.md).
