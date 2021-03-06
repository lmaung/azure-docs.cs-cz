---
title: Zápis výrazů pro mapování atributů ve službě Azure Active Directory | Dokumentace Microsoftu
description: Další informace o použití mapování výrazů má být transformován hodnoty atributů přijatelný formát během automatického zřizování objektů aplikace SaaS ve službě Azure Active Directory.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed081b32fd8ac464f7ec66f97c6867708a6f8533
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "56991476"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Zápis výrazů pro mapování atributů ve službě Azure Active Directory
Při konfiguraci zřizování pro aplikace SaaS, je jedním z typů mapování atributů, které můžete zadat mapování výrazu. Pro ty musíte napsat skript jako výraz, který umožňuje transformovat data uživatelů na formáty, které jsou více přijatelné pro aplikace SaaS.

## <a name="syntax-overview"></a>Přehled syntaxe
Syntaxe výrazů pro mapování atributů je připomínající Visual Basic pro funkce Applications (VBA).

* Celý výraz musí být definován jako funkce, které tvoří název, za nímž následuje argumenty v závorkách: <br>
  *FunctionName (`<<argument 1>>`,`<<argument N>>`)*
* Může vnořit do jiné funkce. Příklad: <br> *FunctionOne(FunctionTwo(`<<argument1>>`))*
* Tři různé typy argumentů můžete předat do funkce:
  
  1. Atributy, které musí být uzavřeny do hranatých závorek. Příklad: [attributeName]
  2. Řetězcové konstanty, které musí být umístěn do dvojitých uvozovek. Příklad: "USA"
  3. Další funkce. Příklad: FunctionOne (`<<argument1>>`, FunctionTwo (`<<argument2>>`))
* Pro řetězcové konstanty Pokud potřebujete zpětného lomítka (\) nebo uvozovky (") v řetězci, se musejí být uvozeny symbol zpětného lomítka (\). Příklad: "Název společnosti: \\"Contoso\\""

## <a name="list-of-functions"></a>Seznam funkcí
[Připojit](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [připojení](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [NormalizeDiacritics](#normalizediacritics) [není](#not) &nbsp; &nbsp; &nbsp; &nbsp; [nahradit](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [SelectUniqueValue](#selectuniquevalue) &nbsp; &nbsp; &nbsp; &nbsp; [SingleAppRoleAssignment](#singleapproleassignment) &nbsp; &nbsp; &nbsp; &nbsp; [Rozdělení](#split) &nbsp; &nbsp; &nbsp; &nbsp; [ StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [přepínač](#switch) &nbsp; &nbsp; &nbsp; &nbsp; [ToLower](#tolower) &nbsp; &nbsp; &nbsp; &nbsp; [ToUpper](#toupper)

- - -
### <a name="append"></a>Připojit
**Funkce:**<br> Append(Source, suffix)

**Popis:**<br> Vezme řetězcovou hodnotu zdroje a připojí přípona na konec.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |Obvykle název atributu ze zdrojového objektu. |
| **Přípona** |Požaduje se |Řetězec |Řetězec, který chcete přidat do konce zdrojové hodnoty. |

- - -
### <a name="formatdatetime"></a>formatDateTime
**Funkce:**<br> FormatDateTime (zdroj, inputFormat outputFormat.)

**Popis:**<br> Přebírá řetězec data z jednoho formátu a převede jej na jiný formát.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |Obvykle název atributu ze zdrojového objektu. |
| **inputFormat** |Požaduje se |Řetězec |Očekávaný formát zdrojové hodnoty. Podporovaných formátů naleznete v tématu [ https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx ](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Požaduje se |Řetězec |Formát výstupního data. |

- - -
### <a name="join"></a>Spojit
**Funkce:**<br> Připojte se k (oddělovač, zdroj1, zdroj2,...)

**Popis:**<br> Join() je podobný Append(), s tím rozdílem, že ho můžete zkombinovat více **zdroj** hodnoty řetězce do jednoho řetězce a pro jednotlivé hodnoty oddělené bránou **oddělovač** řetězec.

Pokud jedna z hodnot zdroje je vícehodnotový atribut, pak každá hodnota v tomto atributu budou připojeny společně, oddělené hodnota oddělovače.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Oddělovač** |Požaduje se |Řetězec |Řetězec použitý k oddělení zdrojové hodnoty, když jsou zřetězeny do jednoho řetězce. Může být "" Pokud žádný oddělovač je povinný. |
| **zdroj1... zdrojN** |Povinné, proměnná počet pokusů |Řetězec |Hodnoty, který se má spojit dohromady řetězce. |

- - -
### <a name="mid"></a>Mid
**Funkce:**<br> Mid (source; start, délka)

**Popis:**<br> Vrátí podřetězec zdrojovou hodnotou. Dílčí řetězec je řetězec, který obsahuje pouze některé znaky z zdrojový řetězec.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |Obvykle název atributu. |
| **start** |Požaduje se |integer |Index v **zdroj** řetězce, kde by měla začít dílčí řetězec. První znak v řetězci budou mít index hodnotu 1, druhý znak bude mít index 2 a tak dále. |
| **Délka** |Požaduje se |integer |Délka podřetězce. Pokud délka skončí mimo **zdroj** řetězec, funkce vrátí dílčí řetězec z **start** indexu do konce **zdroj** řetězec. |

- - -
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Funkce:**<br> NormalizeDiacritics(source)

**Popis:**<br> Vyžaduje jeden argument řetězec. Vrátí řetězec, ale s znaky diakritická nahradí ekvivalentní-diakritická znaků. Obvykle slouží k převodu jména a příjmení obsahující diakritická znaky (znaky s diakritikou značky) do platné hodnoty, které můžete použít různé identifikátory uživatele, jako je například hlavních názvů uživatelů, názvy účtů SAM a e-mailové adresy.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |String | Obvykle křestní jméno nebo poslední atribut name. |

- - -
### <a name="not"></a>Not
**Funkce:**<br> Not(Source)

**Popis:**<br> Převrátí logickou hodnotu **zdroj**. Pokud **zdroj** hodnota je "*True*", vrací "*False*". V opačném případě vrátí "*True*".

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Logického řetězce |Byl očekáván **zdroj** hodnoty jsou "True" nebo "False". |

- - -
### <a name="replace"></a>Nahradit
**Funkce:**<br> Nahraďte (zdroj, oldValue, regexPattern, regexGroupName, zastaralá, replacementAttributeName, šablona)

**Popis:**<br>
Nahradí hodnoty v řetězci. V závislosti na parametry, které poskytnou funguje jinak:

* Když **oldValue** a **zastaralá** jsou k dispozici:
  
  * Nahradí všechny výskyty oldValue ve zdroji zastaralá
* Když **oldValue** a **šablony** jsou k dispozici:
  
  * Nahradí všechny výskyty **oldValue** v **šablony** s **zdroj** hodnota
* Když **regexPattern**, **regexGroupName**, **zastaralá** jsou k dispozici:
  
  * Nahradí všechny hodnoty odpovídající oldValueRegexPattern ve zdrojovém řetězci s zastaralá
* Když **regexPattern**, **regexGroupName**, **replacementPropertyName** jsou k dispozici:
  
  * Pokud **zdroj** nemá žádnou hodnotu **zdroj** je vrácena
  * Pokud **zdroj** má hodnotu, použije **regexPattern** a **regexGroupName** k extrakci nahrazení hodnoty z vlastnosti s **replacementPropertyName** . Nahrazující hodnota se vrátí jako výsledek

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |Obvykle název atributu ze zdrojového objektu. |
| **oldValue** |Nepovinné |Řetězec |Hodnota, která má být nahrazen v **zdroj** nebo **šablony**. |
| **regexPattern** |Nepovinné |Řetězec |Vzor regulárního výrazu pro hodnota, která má být nahrazen v **zdroj**. Nebo, pokud se používá replacementPropertyName, vzor, který má získat hodnoty z vlastnosti nahrazení. |
| **regexGroupName** |Nepovinné |Řetězec |Název skupiny uvnitř **regexPattern**. Pouze v případě, že replacementPropertyName se používá, se automaticky načtou hodnotu této skupiny jako zastaralá z vlastnosti nahrazení. |
| **Zastaralá** |Nepovinné |Řetězec |Nová hodnota nahradí starou s. |
| **replacementAttributeName** |Nepovinné |Řetězec |Název atributu použitého pro nahrazující hodnotou, když zdroj nemá žádnou hodnotu. |
| **Šablony** |Nepovinné |Řetězec |Když **šablony** je zadána hodnota, podíváme se **oldValue** uvnitř šablony a nahraďte ji metodou zdrojovou hodnotu. |

- - -
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Funkce:**<br> SelectUniqueValue (uniqueValueRule1, uniqueValueRule2, uniqueValueRule3,...)

**Popis:**<br> Vyžaduje minimálně dva argumenty, které jsou definovány pomocí výrazů pravidel pro vytvoření jedinečnou hodnotu. Funkce vyhodnocuje každé pravidlo a poté zkontroluje hodnotu vygenerovat jedinečný v cílové aplikaci/adresáři. První jedinečnou hodnotu najít, bude vrácena ten. Pokud všechny hodnoty již existují v cíli, položka bude získat mezi a důvod získá protokolovat v protokolech auditu. Neexistuje žádná horní mez počtu argumentů, které mohou být k dispozici.

> [!NOTE]
>1. Toto je funkce nejvyšší úrovně, nemohou být vnořeny.
>2. Tato funkce je určená jenom pro vytvoření položky. Při použití ho atributem, nastavte **použít mapování** vlastnost **pouze při vytváření objektu**.


**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **uniqueValueRule1... uniqueValueRuleN** |Minimálně 2 jsou povinné, ne horní mez |String | Seznam pravidel pro vytvoření jedinečnou hodnotu pro vyhodnocení. |


- - -
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Funkce:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Popis:**<br> Vrátí jeden appRoleAssignment ze seznamu všech appRoleAssignments přiřazená uživateli pro danou aplikaci. Tato funkce je potřeba převést objekt appRoleAssignments řetězec názvu jednu roli. Všimněte si, že osvědčeným postupem je zajistit appRoleAssignment pouze jeden je přiřazen jednomu uživateli v čase a pokud víc rolí přiřazených rolí řetězec vrácený nemusí být předvídatelné. 

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Požaduje se |Řetězec |**[appRoleAssignments]**  objektu. |

- - -
### <a name="split"></a>Rozdělit
**Funkce:**<br> Split (zdroj, oddělovač)

**Popis:**<br> Rozdělí řetězec s hodnotou mulit pole, pomocí zadané oddělovací znak.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |**Zdroj** hodnotu aktualizovat. |
| **delimiter** |Požaduje se |String |Určuje znak, který se použije k rozdělení řetězce (Příklad: ",") |

- - -
### <a name="stripspaces"></a>StripSpaces
**Funkce:**<br> StripSpaces(source)

**Popis:**<br> Odebere všechny mezery ("") znaků z řetězce zdroje.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |**Zdroj** hodnotu aktualizovat. |

- - -
### <a name="switch"></a>Přepínač
**Funkce:**<br> Switch (zdroj, výchozí hodnota, key1, value1, key2, value2,...)

**Popis:**<br> Při **zdroj** hodnotu shody **klíč**, vrátí **hodnotu** pro daný **klíč**. Pokud **zdroj** hodnota neodpovídá žádné klíče, vrátí **defaultValue**.  **Klíč** a **hodnotu** parametry musí být vždy ve dvojicích. Funkce očekává vždy sudý počet parametrů.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |**Zdroj** hodnotu aktualizovat. |
| **Výchozí hodnota** |Nepovinné |Řetězec |Výchozí hodnota má být použit při zdroj neodpovídá žádné klíče. Může být prázdný řetězec (""). |
| **key** |Požaduje se |Řetězec |**Klíč** k porovnání **zdroj** hodnotu. |
| **value** |Požaduje se |Řetězec |Nahrazující hodnotou pro **zdroj** odpovídající klíči. |

- - -
### <a name="tolower"></a>toLower
**Funkce:**<br> ToLower (zdroj, jazyková verze)

**Popis:**<br> Přijímá *zdroj* řetězcová hodnota a převede ho na malá písmena pomocí jazykové verze pravidla, které jsou určeny. Pokud není žádný *jazykovou verzi* informace zadané, pak bude použita invariantní jazyková verze.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |Obvykle název atributu ze zdrojového objektu |
| **Jazyková verze** |Nepovinné |String |Formát pro název jazykové verze podle RFC 4646 *languagecode2 – země/regioncode2*, kde *languagecode2* je kód jazyka dvoupísmenné a *země/regioncode2*dvoupísmenné subkulturu kód. Mezi příklady patří ja-JP japonština (Japonsko) a en US pro angličtinu (Spojené státy). V případech, kdy kód jazyka dvoupísmenné není k dispozici se používá třípísmenný kód odvozené ze souboru ISO 639-2.|

- - -
### <a name="toupper"></a>toUpper
**Funkce:**<br> ToUpper (zdroj, jazyková verze)

**Popis:**<br> Přijímá *zdroj* řetězcová hodnota a převede ho na velká písmena pomocí jazykové verze pravidla, které jsou určeny. Pokud není žádný *jazykovou verzi* informace zadané, pak bude použita invariantní jazyková verze.

**Parametry:**<br> 

| Název | Požadovaný / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Zdroj** |Požaduje se |Řetězec |Obvykle název atributu ze zdrojového objektu. |
| **Jazyková verze** |Nepovinné |String |Formát pro název jazykové verze podle RFC 4646 *languagecode2 – země/regioncode2*, kde *languagecode2* je kód jazyka dvoupísmenné a *země/regioncode2*dvoupísmenné subkulturu kód. Mezi příklady patří ja-JP japonština (Japonsko) a en US pro angličtinu (Spojené státy). V případech, kdy kód jazyka dvoupísmenné není k dispozici se používá třípísmenný kód odvozené ze souboru ISO 639-2.|

## <a name="examples"></a>Příklady
### <a name="strip-known-domain-name"></a>Název domény známý pruhu
Je potřeba odstranit název domény známý z e-mailu uživatele k získání uživatelského jména. <br>
Například pokud je doména "contoso.com", pak můžete použít následující výraz:

**Výraz:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Ukázkový vstup / výstup:** <br>

* **VSTUP** (pošta): "john.doe@contoso.com"
* **VÝSTUP**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Připojit konstantní příponu k uživatelské jméno
Pokud používáte Sandboxu služby Salesforce, můžete potřebovat přidat další přípony pro všechny své uživatelské jméno před jejich synchronizaci.

**Výraz:** <br>
`Append([userPrincipalName], ".test")`

**Ukázkový vstup/výstup:** <br>

* **VSTUP**: (hodnota userPrincipalName): "John.Doe@contoso.com"
* **VÝSTUP**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Vytvořte alias uživatele tím, že spojováním části křestní jméno a příjmení
Budete muset vygenerovat uživatele alias provedením první 3 písmena křestní jméno uživatele a prvních 5 písmena příjmení uživatele.

**Výraz:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Ukázkový vstup/výstup:** <br>

* **VSTUP** (givenName): "John"
* **VSTUP** (příjmení): "Doe"
* **VÝSTUP**:  "JohDoe"

### <a name="remove-diacritics-from-a-string"></a>Odebrat znaky s diakritikou v řetězci
Je třeba nahradit znaků obsahující diakritická znaménka s ekvivalentní znaků, které neobsahují slovo značky zvýraznění.

**Výraz:** <br>
NormalizeDiacritics([givenName])

**Ukázkový vstup/výstup:** <br>

* **VSTUP** (givenName): "Zoë"
* **VÝSTUP**:  "Zoe"

### <a name="split-a-string-into-a-multi-valued-array"></a>Rozdělit řetězec do pole s více hodnotami
Budete muset provést čárkami oddělený seznam řetězců a rozdělit na pole, které může být připojeno do vícehodnotový atribut jako atribut PermissionSets v Salesforce. V tomto příkladu seznamu sad oprávnění naplněné v extensionAttribute5 ve službě Azure AD.

**Výraz:** <br>
Split ([extensionAttribute5] ",")

**Ukázkový vstup/výstup:** <br>

* **VSTUP** (extensionAttribute5): "PermissionSetOne PermisionSetTwo"
* **VÝSTUP**: ["PermissionSetOne", "PermissionSetTwo"]

### <a name="output-date-as-a-string-in-a-certain-format"></a>Výstupní data jako řetězec v určitém formátu
Chcete odesílat data do aplikace SaaS v určitém formátu. <br>
Je třeba k formátování kalendářních dat pro ServiceNow.

**Výraz:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Ukázkový vstup/výstup:**

* **VSTUP** (extensionAttribute1): "20150123105347.1Z"
* **VÝSTUP**:  "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Nahraďte hodnotu podle předdefinovanou sadu možností

Budete muset definovat časové pásmo uživatele na základě kódu stavu uložené ve službě Azure AD. <br>
Pokud kód stavu neodpovídá žádné z předdefinovaných možností, použijte výchozí hodnotu "Austrálie/Sydney".

**Výraz:** <br>
`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Ukázkový vstup/výstup:**

* **VSTUP** (stav): "QLD"
* **VÝSTUP**: "Austrálie/Brisbane"

### <a name="replace-characters-using-a-regular-expression"></a>Nahradit znaky pomocí regulárních výrazů
Je nutné vyhledat znaky, které odpovídají hodnotě regulárního výrazu a jejich odebrání.

**Výraz:** <br>

Nahraďte ([mailNickname], "[-zA-Z_] *", "",)

**Ukázkový vstup/výstup:**

* **VSTUP** (mailNickname: "john_doe72"
* **VÝSTUP**: "72"

### <a name="convert-generated-userprincipalname-upn-value-to-lower-case"></a>Hodnotu generovanou userPrincipalName (UPN) převést na malá písmena
V následujícím příkladu se zřetězením polí zdroj PreferredFirstName a PreferredLastName vygeneruje hodnotu hlavního názvu uživatele a funkce ToLower pracuje vygenerovaný řetězec převádí všechny znaky na malá písmena. 

`ToLower(Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"))`

**Ukázkový vstup/výstup:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Macek"
* **VÝSTUP**: "john.smith@contoso.com"

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>Generovat jedinečnou hodnotu pro atribut userPrincipalName (UPN)
Založené na uživatele křestní jméno, křestní jméno a příjmení, je potřeba vygenerovat hodnotu pro atribut hlavního názvu uživatele a vyhledat jeho jedinečnosti v adresáři cílového AD před přiřazením hodnoty pro atribut hlavního názvu uživatele.

**Výraz:** <br>

    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com")
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )

**Ukázkový vstup/výstup:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Macek"
* **VÝSTUP**: "John.Smith@contoso.com" Pokud hodnotu hlavního názvu uživatele John.Smith@contoso.com ještě neexistuje v adresáři
* **VÝSTUP**: "J.Smith@contoso.com" Pokud hodnotu hlavního názvu uživatele John.Smith@contoso.com již existuje v adresáři
* **VÝSTUP**: "Jo.Smith@contoso.com" Pokud výše uvedené hodnoty dva hlavní název uživatele v adresáři už existuje

## <a name="related-articles"></a>Související články
* [Automatizace uživatele zřizování a jeho rušení pro aplikace SaaS](user-provisioning.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](customize-application-attributes.md)
* [Filtry oborů pro zřizování uživatelů](define-conditional-rules-for-provisioning-user-accounts.md)
* [Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM](use-scim-to-provision-users-and-groups.md)
* [Oznámení o zřizování účtů](user-provisioning.md)
* [Seznam kurzů o integraci aplikací SaaS](../saas-apps/tutorial-list.md)
