---
title: Uspořádání vašich prostředků s využitím skupin pro správu Azure – zásady správného řízení Azure
description: Další informace o skupinách pro správu, fungování jejich oprávnění a způsobu jejich využití
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: overview
ms.openlocfilehash: 9d606a46bd08ce3e999806bed2357968e5ffd914
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/18/2019
ms.locfileid: "56339283"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Uspořádání vašich prostředků s využitím skupin pro správu Azure

Pokud má vaše organizace mnoho předplatných, možná budete potřebovat způsob, jak efektivně spravovat přístup, zásady a dodržování předpisů pro tato předplatná. Skupiny pro správu Azure představují úroveň rozsahu nad předplatnými. Předplatná uspořádáte do kontejnerů označovaných jako skupiny pro správu a na tyto skupiny pro správu použijete své zásady správného řízení. Všechna předplatná v rámci skupiny pro správu automaticky dědí podmínky, které se na příslušnou skupinu pro správu vztahují. Skupiny pro správu poskytují správu na podnikové úrovni ve velkém měřítku bez ohledu na to, jaké typy předplatného případně máte.

Zásady můžete například použít pro skupinu pro správu, která omezuje oblasti dostupné pro vytváření virtuálních počítačů (VM). Tyto zásady se použijí pro všechny skupiny pro správu, předplatná a prostředky v rámci této skupiny a umožní vytváření virtuálních počítačů jenom v příslušné oblasti.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarchie skupin pro správu a předplatných

Můžete vytvořit flexibilní strukturu skupin pro správu a předplatných a uspořádat tak prostředky do hierarchie umožňující využít jednotné zásady a správu přístupu. Následující diagram ukazuje příklad vytvoření hierarchie pro zásady správného řízení s využitím skupin pro správu.

![strom](./media/tree.png)

Vytvořte hierarchii, abyste mohli použít zásadu, jako je například omezení umístění virtuálních počítačů ve skupině „Skupina pro správu týmu infrastruktury“ na oblast USA – západ. Tato zásada se v této skupině pro správu bude dědit do obou předplatných EA a bude se vztahovat na všechny virtuální počítače v rámci těchto předplatných. Tuto zásadu zabezpečení nemůže změnit vlastník prostředku ani předplatného. Výsledkem je vylepšení zásad správného řízení.

Dalším scénářem, kde by se skupiny pro správu použily, je poskytnutí uživatelského přístupu k několika předplatným. Přesunutím několika předplatných do skupiny pro správu můžete v této skupině vytvořit jedno přiřazení [řízení přístupu na základě role](../../role-based-access-control/overview.md), které zdědí tento přístup pro všechna předplatná.
Jedno přiřazení v rámci skupiny pro správu tak může uživatelům umožnit přístup ke všemu, co potřebují, a není potřeba vytvářet skript řízení přístupu na základě role pro různá předplatná.

### <a name="important-facts-about-management-groups"></a>Důležité informace o skupinách pro správu

- Jeden adresář podporuje až 10 000 skupin pro správu.
- Strom skupin pro správu může mít až šest úrovní vnoření.
  - Toto omezení nezahrnuje kořenovou úroveň ani úroveň předplatného.
- Každá skupina pro správu a předplatné může mít jenom jeden nadřazený prvek.
- Každá skupina pro správu může mít několik podřízených položek.
- Všechny skupiny pro správu a předplatná jsou v rámci adresáře v jedné hierarchii. Výjimky ve verzi Preview jsou popsané v tématu věnovaném [důležitým informacím o kořenových skupinách pro správu](#important-facts-about-the-root-management-group).

## <a name="root-management-group-for-each-directory"></a>Kořenová skupina pro správu pro jednotlivé adresáře

Každý adresář obdrží jednu skupinu pro správu nejvyšší úrovně, která se označuje jako kořenová skupina pro správu.
Tato kořenová skupina pro správu je integrovaná do hierarchie tak, aby pod ní spadaly všechny skupiny pro správu a všechna předplatná. Tato kořenová skupina pro správu umožňuje používání globálních zásad a přiřazení RBAC na úrovni adresáře. [Správce adresáře musí sám sobě zvýšit oprávnění](../../role-based-access-control/elevate-access-global-admin.md), aby v počátečním nastavení byl vlastníkem této kořenové skupiny. Jakmile je správce vlastníkem skupiny, může v rámci správy hierarchie přiřadit libovolnou roli RBAC ostatním skupinám nebo uživatelům adresáře.

### <a name="important-facts-about-the-root-management-group"></a>Důležité informace o kořenových skupinách pro správu

- Název a ID kořenové skupiny pro správu jsou ve výchozím nastavení dané. Zobrazovaný název je možné kdykoli aktualizovat, aby se na webu Azure Portal zobrazoval jinak.
  - Název bude „Kořenová skupina tenanta“.
  - ID bude ID služby Azure Active Directory.
- Kořenová skupina pro správu se na rozdíl od ostatních skupin pro správu nedá přesunout ani odstranit.  
- Všechna předplatná a skupiny pro správu v rámci adresáře spadají do jedné kořenové skupina pro správu.
  - Všechny prostředky v adresáři spadají do kořenové skupiny pro správu, která umožňuje globální správu.
  - Nová předplatná při vytvoření ve výchozím nastavení automaticky spadají do kořenové skupiny pro správu.
- Kořenovou složku pro správu sice vidí všichni zákazníci Azure, ale ne všichni mají přístup ke správě této skupiny.
  - Každý, kdo má přístup k předplatnému, vidí kontext tohoto předplatného v rámci hierarchie.  
  - Ke kořenová skupina pro správu nikdo nemá výchozí přístup. Globální správci adresáře jsou jedinými uživateli, kteří si sami sobě mohou zvýšit oprávnění a získat tak přístup.  Jakmile správci adresáře mají přístup, mohou ostatním uživatelům přidělovat libovolné role RBAC pro správu.  

> [!IMPORTANT]
> Všechna přiřazení uživatelského přístupu nebo zásad v kořenové skupině pro správu **se použijí pro všechny prostředky v rámci příslušného adresáře**.
> Vzhledem k tomu by uživatelé měli vyhodnotit, jestli je potřeba mít prostředky definované v tomto rozsahu.
> Přiřazení uživatelského přístupu nebo zásad by v tomto rozsahu měla být jenom „povinná“.  

## <a name="initial-setup-of-management-groups"></a>Počáteční nastavení skupin pro správu

Když libovolný uživatel začne využívat skupiny pro správu, musí proběhnout úvodní proces nastavení. Prvním krokem je, že se v adresáři vytvoří kořenová skupina pro správu. Po vytvoření této skupiny se všechna existující předplatná, která v příslušném adresáři existují, nastaví jako podřízené prvky kořenové skupiny pro správu. Cílem tohoto procesu je zajistit, aby v rámci adresáře existovala jenom jedna hierarchie skupin pro správu. Jedna hierarchie v adresáři umožňuje zákazníkům využít globální přístup a zásady, které ostatní zákazníci v tomto adresáři nemohou obejít. Všechno, co se přiřadí na kořenové úrovni, se použije napříč všemi skupinami pro správu, skupinami prostředků, předplatnými a prostředky v rámci celého adresáře, a to díky tomu, že v adresáři je jediná hierarchie.

## <a name="trouble-seeing-all-subscriptions"></a>Potíže se zobrazením všech předplatných

U několika adresářů, které začaly využívat skupiny pro správu v rané fázi verze Preview před 25. červnem 2018, může docházet k chybě, kdy se do hierarchie nedostanou všechna předplatná.  Procesy pro zařazení předplatných do hierarchie se implementovaly až po přiřazení zásad nebo role pro kořenovou skupinu pro správu v příslušném adresáři.

### <a name="how-to-resolve-the-issue"></a>Jak tyto potíže vyřešit

Tento problém můžete vyřešit dvěma způsoby.

1. Odebrání všech přiřazení rolí a zásad z kořenové skupiny pro správu
    1. Odebrání všech přiřazení zásad a rolí z kořenové skupiny pro správu způsobí, že tato služba obnoví všechna předplatná do hierarchie při příštím nočním cyklu.  Smyslem tohoto procesu je zajistit, že se žádnému z klientských předplatných neposkytne náhodný přístup nebo přiřazení zásad.
    1. Nejlepší způsob, jak to provést bez dopadu na vaše služby, je použít přiřazení zásad nebo rolí o jednu úroveň pod kořenovou skupinu pro správu. Potom můžete všechna přiřazení z kořenového oboru odebrat.
1. Přímé volání rozhraní API pro zahájení procesu obnovení
    1. Libovolný uživatel adresáře může volat rozhraní API *TenantBackfillStatusRequest* nebo *StartTenantBackfillRequest*. Rozhraní API StartTenantBackfillRequest po zavolání zahájí proces nastavení přesunu všech předplatných do hierarchie. Tento proces také začne vynucovat, aby se všechna nová předplatná stala podřízeným elementem kořenové skupiny pro správu. Tento postup se dá udělat beze změny přiřazení na kořenové úrovni. Voláním rozhraní API říkáte, že je v pořádku, když se přiřazení zásad nebo přístupu na kořenové úrovni použije pro všechna předplatná.

Pokud máte k tomuto procesu obnovení nějaké dotazy, obraťte se na managementgroups@microsoft.com.  
  
## <a name="management-group-access"></a>Přístup ke skupinám pro správu

Skupiny pro správu Azure podporují [řízení přístupu na základě role Azure (RBAC)](../../role-based-access-control/overview.md) pro všechny přístupy k prostředkům a definice rolí.
Tato oprávnění se dědí do podřízených prostředků, které v hierarchii existují. Libovolná integrovaná role RBAC se dá přiřadit skupině pro správu, ze které ji v rámci hierarchie zdědí prostředky.
Skupině pro správu se dá například přiřadit role RBAC Přispěvatel virtuálních počítačů. Tato role nemá ve skupině pro správu žádnou akci, ale zdědí se do všech virtuálních počítačů v rámci této skupiny.

Následující diagram ukazuje role a podporované akce pro skupiny pro správu.

| Název role RBAC             | Vytvořit | Přejmenovat | Přesunout | Odstranění | Přiřazení přístupu | Přiřazení zásad | Čtení  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Vlastník                       | ×      | ×      | ×    | ×      | ×             | ×             | ×     |
|Přispěvatel                 | ×      | ×      | ×    | ×      |               |               | ×     |
|Přispěvatel MG*             | ×      | ×      | ×    | ×      |               |               | ×     |
|Čtenář                      |        |        |      |        |               |               | ×     |
|Čtenář MG*                  |        |        |      |        |               |               | ×     |
|Přispěvatel zásad prostředků |        |        |      |        |               | ×             |       |
|Správce přístupu uživatelů   |        |        |      |        | ×             |               |       |

*: Role Přispěvatel MG a Čtenář MG umožňují uživateli provádět tyto akce jenom v rozsahu příslušné skupiny pro správu.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Vlastní definice a přiřazení role RBAC

Vlastní role RBAC se u skupin pro správu momentálně nepodporují. Aktuální stav si můžete prohlédnout na [fóru pro názory na skupiny pro správu](https://aka.ms/mgfeedback).

## <a name="audit-management-groups-using-activity-logs"></a>Audit skupin pro správu s využitím protokolů aktivit

Pokud chcete přes toto rozhraní API sledovat skupiny pro správu, použijte [rozhraní API protokolů aktivit tenanta](/rest/api/monitor/tenantactivitylogs). Ke sledování aktivit skupin pro správu aktuálně není možné použít PowerShell, rozhraní příkazového řádku ani Azure Portal.

1. Jako správce tenanta Azure AD [zvyšte úroveň přístupu](../../role-based-access-control/elevate-access-global-admin.md) uživateli provádějícímu audit a pak mu přiřaďte roli Čtenář v oboru `/providers/microsoft.insights/eventtypes/management`.
1. Jako uživatel provádějící audit zavoláním [rozhraní API protokolů aktivit tenanta](/rest/api/monitor/tenantactivitylogs) zobrazte aktivity skupiny pro správu. Doporučujeme vyfiltrovat poskytovatele prostředků **Microsoft.Management**, aby se zobrazily všechny aktivity skupiny pro správu.  Příklad:

```http
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Management'"
```

> [!NOTE]
> Pokud chcete toto rozhraní API pohodlně volat z příkazového řádku, vyzkoušejte [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Další kroky

Další informace o řešeních pro správu najdete v následujících tématech:

- [Vytváření skupin pro správu pro organizaci prostředků Azure](create.md)
- [Jak měnit, odstraňovat nebo spravovat skupiny pro správu](manage.md)
- [Kontrola skupin pro správu v modulu Prostředky Azure PowerShellu](https://aka.ms/mgPSdocs)
- [Kontrola skupin pro správu v rozhraní REST API](https://aka.ms/mgAPIdocs)
- [Kontrola skupin pro správu v Azure CLI](https://aka.ms/mgclidoc)
