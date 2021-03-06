---
title: Používání webové služby v aplikaci Excel
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio usnadňuje volání webové služby přímo z aplikace Excel bez nutnosti psát jakýkoli kód.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: 37a7610cf2458906729432b1cf53043f1badd64b
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820837"
---
# <a name="consuming-an-azure-machine-learning-studio-web-service-from-excel"></a>Využívání webové služby Azure Machine Learning Studio z Excelu

 Azure Machine Learning Studio usnadňuje volání webové služby přímo z aplikace Excel bez nutnosti psát jakýkoli kód.

Pokud používáte aplikaci Excel 2013 (nebo novější) nebo Excelu Online, pak doporučujeme použít v aplikaci Excel [doplněk aplikace Excel](excel-add-in-for-web-services.md).



## <a name="steps"></a>Kroky
Publikování webové služby. [Tutoriál 3: Nasazení modelu úvěrové riziko](tutorial-part3-credit-risk-deploy.md) vysvětluje, jak na to. Aktuálně funkce sešitu aplikace Excel je podporována pouze pro služby požadavků/odpovědí, které mají jeden výstup (tedy hodnoticí přípony). 

Jakmile budete mít webovou službu, klikněte na **webových služeb** části nalevo od sady studio a pak vyberte webovou službu používat z aplikace Excel.

**Klasické webové služby**

1. Na **řídicí panel** kartě je řádek pro webovou službu **žádostí a odpovědí** služby. Pokud tato služba má jeden výstup, měli byste vidět **stáhněte si Excelový sešit** odkaz v daném řádku.
   
    ![](./media/consuming-from-excel/excellink.png)
2. Klikněte na **stáhněte si Excelový sešit**.

**Nové webové služby**

1. Na portálu webové služby Azure Machine Learning, vyberte **spotřebovat**.
2. Na stránce využívání v **webové služby možnosti spotřeby** klikněte na ikonu aplikace Excel.

**V práci se sešitem**

1. Otevřete sešit.
2. Zobrazí se upozornění zabezpečení. Klikněte na **povolit úpravy** tlačítko.
   
    ![](./media/consuming-from-excel/enableeditting.png)
3. Zobrazí se upozornění zabezpečení. Klikněte na **povolit obsah** možnost spouštět makra tabulky.
   
    ![](./media/consuming-from-excel/enablecontent.png)
4. Jakmile jsou povolená makra, je vygenerována tabulku. Sloupce v modré jsou vyžaduje jako vstup do webové služby RRS nebo **parametry**. Všimněte si výstup služby RRS **PŘEDPOVĚDĚT hodnoty** zeleně. Když jsou vyplněny všechny sloupce daného řádku, sešit automaticky volá rozhraní API pro vyhodnocení a zobrazí skóre výsledky.
   
    ![](./media/consuming-from-excel/sampletable.png)
5. Ke stanovení skóre pro více než jeden řádek, jsou vytvářeny výplně druhý řádek s daty a předpovězeným hodnotám. I vložením několika řádků najednou.

Vám pomůže některou z funkcí aplikace Excel (grafů, doplňkem power map, podmíněné formátování, atd.) s předpovězeným hodnotám pomáhají vizualizovat data.    

## <a name="sharing-your-workbook"></a>Sdílení vašeho sešitu
Makra pro práci klíč rozhraní API musí být součástí tabulky. To znamená, že by měly sdílet sešit pouze u entity nebo jednotlivce, které důvěřujete.

## <a name="automatic-updates"></a>Automatické aktualizace
Volání rozhraní RRS se provádí v těchto dvou případů:

1. První řádek má obsah ve všech jeho **parametry**
2. Pokaždé, když některý **parametry** změny v řádku, které měly všechny jeho **parametry** zadali.