---
title: Co je Azure Key Vault? | Dokumenty Microsoft
description: Zjistěte, jak Azure Key Vault chrání kryptografické klíče a tajné kódy, které cloudové aplikace a služby používat.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: 48ac0c3efe74723099e87a77871aa1a78834efbd
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958528"
---
# <a name="what-is-azure-key-vault"></a>Co je Azure Key Vault?

Cloudové aplikace a služby pomocí kryptografické klíče a tajné kódy, abyste měli informace o zabezpečení. Azure Key Vault chrání tyto klíče a tajné kódy. Při použití služby Key Vault můžete šifrovat ověřovací klíče, klíče účtu úložiště, šifrovací klíče dat, soubory PFX a hesla, pomocí klíčů chráněných moduly hardwarového zabezpečení (HSM).

Key Vault vám pomůže vyřešit následující problémy:

- **Správa tajných kódů**: Umožňuje bezpečně ukládat a důsledně řídit přístup k tokeny, hesla, certifikáty, klíče rozhraní API a dalších tajných kódů.
- **Správa klíčů**: Vytvářet a řídit šifrovací klíče, které šifrují data. 
- **Správa certifikátů**: Zřízení, správu a nasazení veřejné a privátní certifikáty Secure Sockets Layer/Transport Layer Security (SSL/TLS) pro použití s Azure a vaší interní propojené prostředky. 
- **Store tajných kódů se opírá o modulech hardwarového zabezpečení**: Použijte software nebo ověřené podle standardu FIPS 140-2 úrovně 2 moduly hardwarového zabezpečení k ochraně tajných kódů a klíčů.

## <a name="basic-concepts"></a>Základní koncepty

Azure Key Vault je nástroj pro zabezpečené ukládání tajných klíčů a přístup k nim. Tajný klíč je cokoli, k čemu chcete pečlivě kontrolovat přístup, třeba klíče rozhraní API, hesla nebo certifikáty. Trezor je logická skupina tajných kódů.

Tady jsou další důležité termíny:

- **Tenant**: Tenant je organizace, která vlastní a spravuje konkrétní instanci cloudovým službám Microsoftu. Používá se nejčastěji používá k odkazování na sadu služeb Azure a Office 365 pro organizaci.

- **Vlastníka trezoru**: Vlastníka trezoru můžete vytvořit trezor klíčů a získat úplný přístup a kontrolu nad ním. Vlastník trezoru může také nastavit auditování a protokolování toho, kdo získává přístup ke klíčům a tajným klíčům. Správci můžou řídit životní cyklus klíčů. Můžou přejít na novou verzi klíče, zálohovat ho a provádět související úlohy.

- **Trezor příjemce**: Trezor příjemce může provádět akce s prostředky v trezoru klíčů při vlastníka trezoru uděluje přístup uživatelů. Dostupné akce závisí na udělených oprávněních.

- **Prostředek**: Prostředek je spravovatelná položka, která je k dispozici prostřednictvím Azure. Běžné příklady jsou virtuální počítače, účet úložiště, webové aplikace, databáze a virtuální sítě. Existuje mnoho dalších.

- **Skupina prostředků**: Skupina prostředků je kontejner, který obsahuje související prostředky pro řešení Azure. Skupina prostředků může zahrnovat všechny prostředky pro řešení nebo pouze ty prostředky, které chcete spravovat jako skupinu. Na základě toho, co je pro vaši organizaci nejvhodnější, rozhodnete, jakým způsobem se mají prostředky přidělovat do skupin prostředků.

- **Instanční objekt služby**: Instanční objekt Azure je identita zabezpečení, které uživatel vytvořil aplikace, služby a nástroje pro automatizaci použít pro přístup ke konkrétním prostředkům Azure. Ho představit jako identitu uživatele"" (uživatelské jméno a heslo nebo certifikát) s určitou rolí a přísně řízenými oprávněními. Na rozdíl od obecné identity uživatele instanční objekt potřebuje mít možnost provádět jenom určité akce. To zvyšuje zabezpečení, je-li udělit jenom úroveň minimální oprávnění, které potřebuje k provádění úloh správy.

- [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md): Azure AD je služba Active Directory pro tenanta. Každý adresář má jednu nebo víc domén. K jednomu adresáři se dá přidružit několik předplatných, ale jenom jeden tenant.

- **ID klienta Azure**: ID tenanta je jedinečný způsob, jak identifikovat instanci služby Azure AD v rámci předplatného Azure.

- **Spravované identity**: Azure Key Vault nabízí možnost bezpečného ukládání přihlašovacích údajů a dalších klíčů a tajných kódů, ale váš kód se musí ověřit ve službě Key Vault, aby je mohl načíst. Použití spravované identity je řešení tohoto problému jednodušší tím, že automaticky spravovanou identitu služby Azure ve službě Azure AD. Tuto identitu můžete použít k ověření ve službě Key Vault nebo jakékoli jiné službě, která podporuje ověřování Azure AD, aniž by váš kód obsahoval přihlašovací údaje. Další informace viz následující obrázek a [přehled o spravovaných identit pro prostředky Azure](../active-directory/managed-identities-azure-resources/overview.md).

    ![Diagram spravovaných identit pro práci s prostředky Azure](./media/key-vault-whatis/msi.png)

## <a name="authentication"></a>Authentication
Provádět všechny operace se službou Key Vault, musíte nejprve k ověření do něj. Existují tři způsoby, jak ověření do služby Key Vault:

- [Spravované identity pro prostředky Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview): Když nasazujete aplikace na virtuálním počítači v Azure, můžete přiřadit identitu, která k virtuálnímu počítači, který má přístup do služby Key Vault. Můžete také přiřadit identit, které [dalších prostředků Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Výhodou tohoto přístupu je, že aplikace nebo služba není správu oběhu první tajného klíče. Azure automaticky otočí identitu. Tento přístup vám doporučujeme jako osvědčený postup. 
- **Instanční objekt a certifikát služby**: Můžete použít instanční objekt služby a přidružený certifikát, který má přístup do služby Key Vault. Tento přístup nedoporučujeme, protože vlastník aplikace nebo vývojář musí otočit o certifikát.
- **Instanční objekt a tajný klíč**: Přestože instančního objektu a tajného kódu lze použít k ověření do služby Key Vault, to nedoporučujeme. Je těžké automaticky střídejte bootstrap tajný klíč, který se používá k ověření do služby Key Vault.


## <a name="key-vault-roles"></a>Role služby Key Vault

Následující tabulka vám pomůže lépe porozumět tomu, jak může Key Vault pomoci splnit potřeby vývojářů a správců zabezpečení.

| Role | Popis problému | Vyřešeno Azure Key Vault |
| --- | --- | --- |
| Vývojář aplikace Azure |"Chci napsat aplikaci pro Azure, která k podepisování a šifrování používá klíče. Ale tyto klíče musí být mimo aplikaci tak, aby toto řešení je vhodné pro geograficky distribuovanou aplikaci. <br/><br/>Chci, aby tyto klíče a tajné klíče byly chráněné, bez nutnosti psát vlastní kód. Také chci, aby pro mě byly tyto klíče a tajné klíče snadno použitelné v aplikacích a aby poskytovaly optimální výkon.“ |√ Klíče jsou uložené v trezoru, a když je potřeba, volají se identifikátorem URI.<br/><br/> √ Klíče jsou chráněné systémem Azure pomocí standardních algoritmů, délek klíčů a modulů hardwarového zabezpečení.<br/><br/> √ Klíče se zpracovávají v modulech HSM umístěných ve stejných datových centrech jako aplikace. Tato metoda poskytuje větší spolehlivost a nižší latenci, než kdyby byly klíče umístěné v samostatném umístění, například místně. |
| Vývojář softwaru jako služby (SaaS) |„Nechci nést odpovědnost za klientské klíče a tajné klíče zákazníků. <br/><br/>Chci, aby zákazníci sami vlastnili a spravovali své klíče, což mi umožní soustředit se na to, co umím nejlépe – poskytování základních softwarových funkcí.“ |√ Zákazníci můžou svoje klíče importovat do systému Azure a spravovat je. Když některá aplikace SaaS potřebuje provést kryptografické operace pomocí klíčů zákazníků, Key Vault tyto operace provede jménem aplikace. Aplikace klíče zákazníků nezná. |
| Ředitel pro bezpečnost |„Potřebuji vědět, že naše aplikace splňují standard FIPS 140-2 Level 2 pro moduly HMS, který zajišťuje bezpečnou správu klíčů. <br/><br/>Chci se ujistit, že moje organizace má kontrolu nad životním cyklem klíčů a může monitorovat jejich využití. <br/><br/>A přestože používáme více služeb a prostředků Azure, chci spravovat klíče z jednoho umístění v Azure.“ |√ Moduly HMS jsou ověřené podle standardu FIPS 140-2 Level 2.<br/><br/>√ Key Vault je navržený tak, aby společnost Microsoft vaše klíče neznala ani neextrahovala.<br/><br/>√ Využití klíčů se protokoluje téměř v reálném čase.<br/><br/>√ Trezor poskytuje jednotné rozhraní – bez ohledu na to, kolik trezorů v Azure máte, které oblasti podporují a které aplikace je používají. |

Trezory klíčů může vytvářet a používat každý, kdo má předplatné Azure. Přestože je Key Vault přínosný pro vývojáře a správce zabezpečení, je možné implementovat a spravovat správce v organizaci, který spravuje ostatní služby Azure. Například tento správce se můžete přihlásit pomocí předplatného Azure, vytvořte trezor pro organizace, ve kterém chcete ukládat klíče a potom mít na starost provozní úlohy, jako jsou tyto:

- Vytvoření nebo import klíče nebo tajného klíče
- Odvolání nebo odstranění klíče nebo tajného klíče
- Autorizace uživatelů nebo aplikací k přístupu k trezoru klíčů, aby potom mohli spravovat nebo používat jeho klíče a tajné klíče
- Konfigurace používání klíčů (například podepisování nebo šifrování)
- Sledování používání klíčů

Tento správce pak poskytuje vývojářům identifikátory URI pro volání ze svých aplikací. Tento správce taky poskytuje informace o protokolování používání klíčů k správce zabezpečení. 

![Přehled toho, jak funguje Azure Key Vault][1]

Vývojáři také mohou spravovat klíče přímo, pomocí rozhraní API. Další informace najdete v [příručce pro vývojáře Key Vault](key-vault-developers-guide.md).

## <a name="next-steps"></a>Další postup

Zjistěte, jak [zabezpečení trezoru](key-vault-secure-your-key-vault.md).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Key Vault je dostupný ve většině oblastí. Další informace najdete na [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).
