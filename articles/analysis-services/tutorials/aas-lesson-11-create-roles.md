---
title: 'Kurz služby Azure Analysis Services lekce 11: Vytvoření rolí | Dokumentace Microsoftu'
description: Popisuje, jak vytvořit role v projektu Kurz služby Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5b89051cab7e89f79a2b62a392173e6dc234e48d
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189741"
---
# <a name="create-roles"></a>Vytvoření rolí

V této lekci vytvoříte role. Role poskytují objekt databáze modelu a zabezpečení dat tím, že omezují přístup pouze na uživatele, kteří jsou členy dané role. Každá role je definována s jediným oprávněním: Žádné, čtení, čtení a procesu, proces nebo správce. Role je možné definovat při vytváření modelu pomocí Správce rolí. Po nasazení modelu můžete role spravovat pomocí aplikace SQL Server Management Studio (SSMS). Další informace najdete v tématu [Role](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).
  
> [!NOTE]  
> Vytvoření rolí není nezbytné pro dokončení tohoto kurzu. Ve výchozím nastavení má účet, ke kterému jste právě přihlášeni, oprávnění správce modelu. Aby však mohli data procházet pomocí klienta sestav ostatní uživatelé ve vaší organizaci, musíte vytvořit alespoň jednu roli s oprávněním Čtení a přidat tyto uživatele jako členy.  
  
Vytvoříte tři role:  
  
-   **Manažer prodeje** – Tato role může zahrnovat uživatele ve vaší organizaci, u kterých chcete, aby měli oprávnění Čtení ke všem objektům a datům modelu.  
  
-   **Analytik prodeje v USA** – Tato role může zahrnovat uživatele ve vaší organizaci, u kterých chcete, aby mohli procházet pouze data související s prodejem v USA. Pro tuto roli pomocí vzorce DAX nadefinujete *Filtr řádků*, který členům umožní procházet pouze data pro USA.  
  
-   **Správce** – Tato role může zahrnovat uživatele, u kterých chcete, aby měli oprávnění Správce, což jim poskytne neomezený přístup a oprávnění k provádění úloh správy s databází modelu.  
  
Protože jsou účty uživatelů a skupin systému Windows ve vaší organizaci jedinečné, můžete mezi členy přidat účty z konkrétní organizace. Pro účely tohoto kurzu však můžete členy také ponechat prázdné. Testovat účinky jednotlivých rolí později v lekci 12: Analyzujte v aplikaci Excel.  
  
Odhadovaný čas dokončení této lekce: **15 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabulkového modelování, který by se měl dokončit v daném pořadí. Před provedením úkolů v této lekci, měli mít dokončenou předchozí lekci: [Lekce 10: Vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Vytvoření rolí  
  
#### <a name="to-create-a-sales-manager-user-role"></a>Vytvoření role uživatele Manažer prodeje  
  
1.  V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Role** > **Role**.  
  
2.  Ve Správci rolí klikněte na **Nový**.  
  
3.  Klikněte na novou roli a potom ji ve sloupci **Název** přejmenujte na **Manažer prodeje**.  
  
4.  Ve sloupci **Oprávnění** klikněte na rozevírací seznam a vyberte oprávnění **Čtení**. 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  Volitelné: Klikněte na kartu **Členové** a pak na **Přidat**. V dialogovém okně **Vybrat uživatele nebo skupiny** zadejte uživatele nebo skupiny systému Windows z vaší organizace, které chcete zahrnout do role.  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a>Vytvoření role uživatele Analytik prodej v USA  
  
1.  Ve Správci rolí klikněte na **Nový**.    
  
2.  Přejmenujte roli na **Analytik prodeje v USA**.  
  
3.  Přidělte této roli oprávnění **Čtení**.  
  
4.  Klikněte na kartu Filtry řádků a pak jenom pro tabulku **DimGeography** zadejte do sloupce Filtr DAX následující vzorec:  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Vzorec filtru řádků se musí přeložit na logickou hodnotu (TRUE/FALSE). Pomocí tohoto vzorce určujete, že pro uživatele budou viditelné pouze řádky, na kterých má kód země nebo oblasti hodnotu USA.  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  Volitelné: Klikněte na kartu **Členové** a pak na **Přidat**. V dialogovém okně **Vybrat uživatele nebo skupiny** zadejte uživatele nebo skupiny systému Windows z vaší organizace, které chcete zahrnout do role.  
  
#### <a name="to-create-an-administrator-user-role"></a>Vytvoření role uživatele Správce  
  
1.  Klikněte na možnost **Nové**.  
  
2.  Přejmenujte roli na **Správce**.  
  
3.  Přidělte této roli oprávnění **Správce**.  
  
4.  Volitelné: Klikněte na kartu **Členové** a pak na **Přidat**. V dialogovém okně **Vybrat uživatele nebo skupiny** zadejte uživatele nebo skupiny systému Windows z vaší organizace, které chcete zahrnout do role. 
  
  
## <a name="whats-next"></a>Co dále?
[Lekce 12: Analyzovat v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).

  
  
