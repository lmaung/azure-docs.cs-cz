---
title: zahrnout soubor
description: zahrnout soubor
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: c4d1e26a69b2eed833cbe73c31f233c97cd72ea0
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2018
ms.locfileid: "53286225"
---
1. V novém okně prohlížeče se přihlaste k webu [Azure Portal](https://portal.azure.com/).
2. Vyberte **Vytvořit prostředek** > **Databáze** > **Azure Cosmos DB**.
   
   ![Podokno databází portálu Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. Na **vytvořit účet služby Azure Cosmos DB** zadejte základního nastavení pro nový účet Azure Cosmos DB. 
 
    Nastavení|Hodnota|Popis
    ---|---|---
    Předplatné|Vaše předplatné|Vyberte předplatné Azure, které chcete pro tento účet služby Azure Cosmos DB použít. 
    Skupina prostředků|Vytvořit nový<br><br>Potom zadejte stejný jedinečný název, jako jste zadali v ID|Vyberte, že chcete **vytvořit novou** IP adresu. Zadejte nový název skupiny prostředků pro váš účet. Pro jednoduchost použijte stejný název jako vaše ID. 
    Název účtu|Zadejte jedinečný název.|Zadejte jedinečný název, který identifikuje váš účet služby Azure Cosmos DB. Vzhledem k tomu, že se váš identifikátor URI vytvoří připojením řetězce *documents.azure.com* k ID, které zadáte, použijte jedinečné ID.<br><br>ID může obsahovat jenom malá písmena, číslice a znak spojovníku (-). Musí být dlouhý 3 až 31 znaků. 
    Rozhraní API|Core(SQL)|Rozhraní API určuje typ účtu, který se má vytvořit. Azure Cosmos DB nabízí pět rozhraní API: Core(SQL) pro databáze dokumentů, Gremlin databáze grafů, MongoDB, databáze dokumentů, tabulka Azure a Cassandra. V současné době musíte vytvořit samostatný účet pro každé rozhraní API. <br><br>Vyberte **Core(SQL)** vzhledem k tomu, že v tomto článku vytvoříte databázi dokumentů a dotazování pomocí syntaxe SQL. <br><br>[Další informace o rozhraní SQL API](../articles/cosmos-db/documentdb-introduction.md).|
    Umístění|Vyberte oblast nejbližší vašim uživatelům.|Vyberte zeměpisné umístění, ve kterém chcete účet služby Azure Cosmos DB hostovat. Použijte umístění, které je vašim uživatelům nejbližší, abyste jim zajistili nejrychlejší přístup k datům.

    Vyberte **zkontrolovat a vytvořit**. Můžete přeskočit **sítě** a **značky** oddílu. 

    ![Stránka nového účtu pro službu Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

4. Vytvoření účtu trvá několik minut. Počkejte, až portál zobrazí zprávu **Blahopřejeme! Váš účet služby Azure Cosmos DB byl vytvořen**.

    ![Podokno Oznámení portálu Azure Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

