---
title: Filtry zabezpečení, které mají být odebrány výsledky pomocí služby Active Directory – Azure Search
description: Řízení přístupu u obsahu Azure Search pomocí filtrů zabezpečení a identit Azure Active Directory (AAD).
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/07/2017
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 3f55b3b099cc22fda2bebf0dcb8d3e9c1a580f02
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56099689"
---
# <a name="security-filters-for-trimming-azure-search-results-using-active-directory-identities"></a>Filtry zabezpečení pro oříznutí výsledky Azure Search pomocí identity služby Active Directory

Tento článek ukazuje, jak používat identity zabezpečení Azure Active Directory (AAD) spolu s filtry ve službě Azure Search k oříznutí výsledky hledání založené na členství ve skupině uživatelů.

Tento článek se zabývá následujícími úkony:
> [!div class="checklist"]
- Vytvořit uživatele a skupiny AAD
- Přiřadit uživatele ke skupině, kterou jste vytvořili
- Nové skupiny do mezipaměti
- Indexování dokumentů s související skupiny
- Vydejte žádost o vyhledávání pomocí filtr identifikátorů skupiny

>[!NOTE]
> Ukázka fragmentu kódu v tomto článku jsou napsané v C#. Úplný zdrojový kód najdete [na GitHubu](https://aka.ms/search-dotnet-howto). 

## <a name="prerequisites"></a>Požadavky

Musí mít indexu ve službě Azure Search [zabezpečení pole](search-security-trimming-for-azure-search.md) uložení seznamu skupiny identit máte přístup pro čtení k tomuto dokumentu. Tento případ použití předpokládá shoda mezi zabezpečitelných položku (třeba jednotlivce Fakultě aplikace) a zabezpečení pole určující, kdo má přístup k této položky (nemocnicích pracovníky).

Musíte mít oprávnění správce AAD, vyžaduje v tomto názorném postupu pro vytváření uživatelů, skupin a přidružení ve službě AAD.

Vaše aplikace musí být zaregistrovaná v AAD, také, jak je popsáno v následujícím postupu.

### <a name="register-your-application-with-aad"></a>Registrace aplikace pomocí AAD

Tento krok integruje do vaší aplikace AAD pro účely přijímat přihlášení účty uživatelů a skupin. Pokud si nejste správce AAD ve vaší organizaci, možná budete muset [vytvořit nového tenanta](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) provést následující kroky.

1. Přejděte [ **portál pro registraci aplikací**](https://apps.dev.microsoft.com) >  **Konvergované aplikace** > **přidat aplikaci**.
2. Zadejte název pro vaši aplikaci a pak klikněte na **vytvořit**. 
3. Na stránce Moje aplikace vyberte svoji nově zaregistrovaný aplikaci.
4. Na stránce pro registraci aplikace > **platformy** > **přidat platformy**, zvolte **webového rozhraní API**.
5. Stále na registrační stránku aplikace, přejděte na > **oprávnění Microsoft Graphu** > **přidat**.
6. Vyberte oprávnění, přidejte následující delegovaná oprávnění a pak klikněte na tlačítko **OK**:

   + **Directory.ReadWrite.All**
   + **Group.ReadWrite.All**
   + **User.ReadWrite.All**

Microsoft Graph poskytuje rozhraní API, který umožňuje programový přístup k AAD pomocí rozhraní REST API. Vzorový kód pro tento návod používá oprávnění k volání rozhraní Microsoft Graph API pro vytváření skupin uživatelů a přidružení. Rozhraní API se také používají identifikátory skupiny mezipaměti pro rychlejší filtrování.

## <a name="create-users-and-groups"></a>Vytvoření uživatelů a skupin

Pokud se přidání vyhledávání do vytvořených aplikací, bude pravděpodobně stávajícího uživatele a skupiny v adresáři AAD. V takovém případě můžete přeskočit následující tři kroky. 

Ale pokud nemáte stávající uživatele, můžete rozhraní Microsoft Graph API k vytvoření objektů zabezpečení. Následující fragmenty kódu ukazují, jak generovat identifikátory, které se stanou hodnoty dat pro zabezpečení pole v indexu Azure Search. V naší aplikaci nemocnicích hypotetické škole to může být identifikátory zabezpečení nemocnicích pracovníkům.

Uživatele a členství ve skupině může být velmi plynulé, zejména ve velkých organizacích. Kód, který vytváří identity uživatelů a skupin byste spouštěn dostatečně často, aby přebíral změny v členství v organizaci. Obdobně indexu Azure Search vyžaduje podobné plán aktualizace tak, aby odrážela aktuální stav povolených uživatelů a prostředků.

### <a name="step-1-create-aad-grouphttpsdocsmicrosoftcomgraphapigroup-post-groupsviewgraph-rest-10"></a>Krok 1: Vytvoření [skupiny AAD](https://docs.microsoft.com/graph/api/group-post-groups?view=graph-rest-1.0) 
```csharp
// Instantiate graph client 
GraphServiceClient graph = new GraphServiceClient(new DelegateAuthenticationProvider(...));
Group group = new Group()
{
    DisplayName = "My First Prog Group",
    SecurityEnabled = true,
    MailEnabled = false,
    MailNickname = "group1"
}; 
Group newGroup = await graph.Groups.Request().AddAsync(group);
```
   
### <a name="step-2-create-aad-userhttpsdocsmicrosoftcomgraphapiuser-post-usersviewgraph-rest-10"></a>Krok 2: Vytvoření [uživatele AAD](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0)
```csharp
User user = new User()
{
    GivenName = "First User",
    Surname = "User1",
    MailNickname = "User1",
    DisplayName = "First User",
    UserPrincipalName = "User1@FirstUser.com",
    PasswordProfile = new PasswordProfile() { Password = "********" },
    AccountEnabled = true
};
User newUser = await graph.Users.Request().AddAsync(user);
```

### <a name="step-3-associate-user-and-group"></a>Krok 3: Přidružení uživatelů a skupin
```csharp
await graph.Groups[newGroup.Id].Members.References.Request().AddAsync(newUser);
```

### <a name="step-4-cache-the-groups-identifiers"></a>Krok 4: Identifikátory skupiny do mezipaměti
Volitelně Pokud chcete snížit latenci sítě, můžete ukládat do mezipaměti přidružení skupiny uživatelů tak, aby při vydání požadavku hledání skupin se vrátí z mezipaměti, ukládání umožňujícím zpětnou transformaci na AAD. Můžete použít [rozhraní API služby Batch AAD](https://developer.microsoft.com/graph/docs/concepts/json_batching) posílat jednoho požadavku Http s více uživateli a sestavení mezipaměti.

Microsoft Graph je určený pro zpracování k velkému počtu požadavků. Pokud dojde k příliš velkého počtu požadavků, Microsoft Graphu selže požadavek se stavovým kódem HTTP 429. Další informace najdete v tématu [Microsoft Graphu omezování](https://developer.microsoft.com/graph/docs/concepts/throttling).

## <a name="index-document-with-their-permitted-groups"></a>U dokumentu indexu s jejich povolených skupin

Operace dotazů ve službě Azure Search jsou spouštěné přes indexu Azure Search. V tomto kroku operace indexování importuje prohledávatelných dat do indexu, včetně identifikátory používané jako filtry zabezpečení. 

Služba Azure Search neobsahuje ověření identity uživatelů, nebo poskytnout logiku pro vytváření obsahu, které má uživatel oprávnění k zobrazení. Případ použití pro oříznutí zabezpečení se předpokládá, že zadáte přidružení mezi citlivých dokumentů a identifikátor skupiny mají přístup k dokumentu, importovat Internet do indexu vyhledávání. 

V tomto příkladu hypotetické bude zahrnovat text žádosti PUT na index Azure Search kompozice Fakultě žadatele nebo přepisu společně s identifikátorem skupiny mají oprávnění k zobrazení tohoto obsahu. 

Akce indexu obecný příklad použitý v ukázkovém kódu v tomto návodu, může vypadat takto:

```csharp
var actions = new IndexAction<SecuredFiles>[]
              {
                  IndexAction.Upload(
                  new SecuredFiles()
                  {
                      FileId = "1",
                      Name = "secured_file_a",
                      GroupIds = new[] { groups[0] }
                  }),
              ...
             };

var batch = IndexBatch.New(actions);

_indexClient.Documents.Index(batch);  
```

## <a name="issue-a-search-request"></a>Vydat požadavek hledání

Z bezpečnostních důvodů ořezávání jsou hodnoty ve svém oboru zabezpečení v indexu statické hodnoty používané pro zahrnutí nebo vyloučení dokumenty ve výsledcích hledání. Například pokud identifikátor skupiny pro nemocnicích "A11B22C33D44 E55F66G77 H88I99JKK", všech dokumentů v indexu Azure Search s tímto identifikátorem zabezpečení zaznamenaná jsou zahrnuté (nebo vyloučit) ve výsledcích hledání pošle žadateli.

K filtrování vrácených ve výsledcích hledání na základě skupin uživatelů odesíláním žádosti dokumentů, najdete v následujících krocích.

### <a name="step-1-retrieve-users-group-identifiers"></a>Krok 1: Načíst identifikátory skupiny uživatele

Pokud uživatele skupiny již do mezipaměti neuložily, nebo vypršela platnost mezipaměti, [skupiny](https://docs.microsoft.com/graph/api/directoryobject-getmembergroups?view=graph-rest-1.0) žádosti
```csharp
private static void RefreshCacheIfRequired(string user)
{
    if (!_groupsCache.ContainsKey(user))
    {
        var groups = GetGroupIdsForUser(user).Result;
        _groupsCache[user] = groups;
    }
}

private static async Task<List<string>> GetGroupIdsForUser(string userPrincipalName)
{
    List<string> groups = new List<string>();
    var allUserGroupsRequest = graph.Users[userPrincipalName].GetMemberGroups(true).Request();

    while (allUserGroupsRequest != null) 
    {
        IDirectoryObjectGetMemberGroupsRequestBuilder allUserGroups = await allUserGroupsRequest.PostAsync();
        groups = allUserGroups.ToList();
        allUserGroupsRequest = allUserGroups.NextPageRequest;
    }
    return groups;
}
``` 

### <a name="step-2-compose-the-search-request"></a>Krok 2: Vytvořit žádost o vyhledávání

Za předpokladu, že máte členství skupin uživatele, může vydat požadavek hledání hodnotami, které příslušný filtr.

```csharp
string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", groups.Select(g => g.ToString())));
SearchParameters parameters = new SearchParameters()
             {
                 Filter = filter,
                 Select = new[] { "application essays" }
             };

DocumentSearchResult<SecuredFiles> results = _indexClient.Documents.Search<SecuredFiles>("*", parameters);
```
### <a name="step-3-handle-the-results"></a>Krok 3: Zpracování výsledků

Odpověď obsahuje filtrovaný seznam dokumentů, který se skládá z ty, které má uživatel oprávnění k zobrazení. V závislosti na tom, jak vytvořit stránku výsledků hledání můžete chtít zahrnout vizuální upozornění tak, aby odrážely sady filtrovaných výsledků.

## <a name="conclusion"></a>Závěr

V tomto návodu jste se dozvěděli techniky pro použití přihlášení AAD k filtrování dokumenty ve výsledcích hledání Azure, ořezávání výsledky dokumenty, které se neshodují s filtr zadaný v požadavku.

## <a name="see-also"></a>Další informace najdete v tématech

+ [Ovládací prvek přístupu na základě identit pomocí Azure Search filtry](search-security-trimming-for-azure-search.md)
+ [Filtry ve službě Azure Search](search-filters.md)
+ [Řízení přístupu a zabezpečení dat v provozu Azure Search](search-security-overview.md)
