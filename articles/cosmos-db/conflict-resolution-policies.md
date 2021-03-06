---
title: Řešení konfliktu a zásady řešení s několika zápisu oblastí ve službě Azure Cosmos DB
description: Tento článek popisuje kategorie konflikt a zásady řešení konfliktu ve službě Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 6b168efc2314e21f381d55f6ac79cda8b426e66d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110298"
---
# <a name="conflict-types-and-resolution-policies"></a>Zásady řešení a typy konfliktů

Konflikty a řešení konfliktů, že zásady se dají použít, pokud váš účet Azure Cosmos DB je nakonfigurovaný s několika zápisu oblastech.

Účty služby Azure Cosmos DB s více oblastí zápisu nakonfigurovanou můžete aktualizace konflikt nastane, když uživatelé vytvářející obsah současně aktualizace jedné položce v několika oblastech. Aktualizace je v konfliktu se zařazují do následující tři typy:

* **Vložit konflikty**: Tyto konflikty mohou nastat při aplikaci současně vloží dva nebo více položek se stejný jedinečný index ze dvou nebo více oblastech. Jako příklad může dojít k tomuto konfliktu s vlastností ID. Všechny operace zápisu se může zpočátku povést v jejich příslušných místních oblastech. Ale podle zásada řešení konfliktů, kterou zvolíte, je jenom jedna položka s ID původního nakonec potvrdit.

* **Nahraďte je v konfliktu**: Tyto konflikty situace může nastat, pokud aplikace aktualizuje jednu položku současně ze dvou nebo více oblastech.

* **Odstranit konflikty**: Tyto konflikty mohou nastat při aplikaci současně odstraní položku z jedné oblasti a aktualizuje ji z jiné oblasti.

## <a name="conflict-resolution-policies"></a>Zásady řešení konfliktů

Azure Cosmos DB nabízí flexibilní založená na zásadách mechanismus k vyřešení konfliktů při aktualizacích. Můžete vybrat mezi dvěma zásadami řešení konfliktů v kontejneru Azure Cosmos DB:

- **Posledního zápisu Wins (LWW)**: Tato zásada řešení používá ve výchozím nastavení vlastnosti definované v systému časové razítko. Je založen na protokolu synchronizaci času hodin. Pokud používáte SQL API služby Azure Cosmos DB, můžete zadat jiné vlastní číselné vlastnosti pro řešení konfliktů. Vlastní číselné vlastnosti se také označuje jako cesta k řešení konfliktů. 

  Pokud dva nebo více položek konflikt na vložení nebo nahrazení operace, stane položek s nejvyšší hodnotou pro cestu k řešení konfliktů vítěze. Systém určí vítěze máte více položek na stejnou číselnou hodnotu pro cestu k řešení konfliktů. Všechny oblasti zaručeno směřovat do jednoho vítěze a end nahoru se stejnou verzí položky potvrzeny. Při odstranění je v konfliktu se podílejí, vždy odstraněné verze služby wins vložení nebo nahrazení je v konfliktu. Tento výsledek nastane bez ohledu na hodnotu cesty řešení konfliktů.

  > [!NOTE]
  > Poslední zápis Wins je výchozí zásada řešení konfliktů. Je dostupná pro účty SQL, tabulky Azure Cosmos DB, MongoDB, Cassandra a Gremlin API nevztahují.

  Další informace najdete v tématu [příklady, které používají LWW konfliktu zásad řešení](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Vlastní**: Tato zásada řešení je určena pro sémantiku definovaného aplikací pro odsouhlasení je v konfliktu. Když tyto zásady nastavíte na váš kontejner Azure Cosmos DB, musíte také zaregistrovat sloučení uložené procedury. Tento postup je automaticky vyvolána při zjištění konfliktu v rámci transakce databáze na serveru. Poskytuje tento systém přesně jednou zaručit k provádění procedury sloučení jako součást závazku protokolu.  

  Existují dva body na paměti, pokud konfigurace vašeho kontejneru s možností vlastní řešení. Pokud se nezdaří jejich registrace sloučení proceduru ke kontejneru nebo sloučení postupu dojde k výjimce za běhu, konflikty se zapisují do kanálu konflikty. Aplikace pak je potřeba ručně vyřešit konflikty v konflikty informačního kanálu. Další informace najdete v tématu [příklady, jak používat zásady vlastní řešení a jak používat konfliktům kanál](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Vlastní konflikt zásada řešení je k dispozici pouze pro účty SQL API.

## <a name="next-steps"></a>Další postup

Zjistěte, jak nakonfigurovat zásady řešení konfliktů. Viz následující články:

* [Jak nakonfigurovat více hlavních databází ve svých aplikacích](how-to-multi-master.md)
* [Použití LWW zásada řešení konfliktů](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Použít vlastní konflikt zásada řešení](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Použít konflikty informačního kanálu](how-to-manage-conflicts.md#read-from-conflict-feed)
