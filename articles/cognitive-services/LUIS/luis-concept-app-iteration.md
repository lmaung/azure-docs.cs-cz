---
title: Návrh iterativní aplikací
titleSuffix: Language Understanding - Azure Cognitive Services
description: Služba LUIS učí nejlepší v iterativní cyklus změny modelu, příklady utterance, publikování a shromažďování dat z koncového bodu dotazů.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 6f8ec10222ae58af9e39d2d88f13f355f9c9baa6
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55861132"
---
# <a name="authoring-cycle-for-your-luis-app"></a>Vytváření cyklus aplikace LUIS
Služba LUIS učí nejlepší v iterativní cyklus změny modelu, příklady utterance, publikování a shromažďování dat z koncového bodu dotazů. 

![Cyklus vytváření](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Sestavení modelu LUIS
Účel modelu je zjistit, co uživatel žádá o (záměr nebo záměr) a které části otázky zadejte podrobnosti (entity), které pomáhají určit odpověď. 

Model musí být specifické pro doménu aplikace, aby bylo možné zjistit slova a slovní spojení, že jsou relevantní, jakož i typické slovo řazení. 

Tento model zahrnuje záměr, entity. 

## <a name="add-training-examples"></a>Přidat příklady školení
Služba LUIS potřebuje příkladu projevy v příkazů. V příkladech potřebovat dostatek varianta možnosti aplikace word a slovosled být schopní určit, které záměr utterance je určená pro. Každý příklad utterance musí mít všechna požadovaná data označená jako entity. 

Dáte pokyn LUIS ignorovat projevy, které nejsou relevantní pro doménu vaší aplikace tak, že přiřadíte utterance k **žádný** záměr. Jakákoli slova nebo fráze, které nepotřebujete získaných ze utterance nemusíte být označené. Neexistuje žádný popisek slov nebo slovních spojení ignorovat. 

## <a name="train-and-publish-the-app"></a>Trénování a publikování aplikace
Jakmile budete mít různé projevy 10 až 15 v každé záměr, o požadované entity s popiskem, trénování a publikování. Od publikovat oznámení o úspěchu použijte odkaz zobrazíte vašich koncových bodů. Ujistěte se, že při vytváření vaší aplikace a publikování aplikace tak, aby byly k dispozici v [koncový bod oblastech](luis-reference-regions.md) potřebujete. 

## <a name="https-endpoint-testing"></a>Testování koncového bodu HTTPS
Umožní otestovat vaši aplikaci LUIS z koncového bodu HTTPS. Testování z koncového bodu umožňuje LUIS zvolit jakékoli projevy s nízkou spolehlivostí ke kontrole.  

## <a name="recycle"></a>Koš
Jakmile budete hotovi s cyklem vytváření, můžete začít znovu. Začněte prostudováním projevy koncový bod, LUIS označí s nízkou spolehlivostí. Zkontrolujte tyto projevy záměr a entity. Jakmile zkontrolujete projevy, zkontrolujte seznam by měl být prázdný.  

## <a name="batch-testing"></a>Dávkové testování
Testování služby batch je způsob, jak zobrazit, kolik projevy příkladu jsou hodnocené podle LUIS. Příklady by měl být novým LUIS a by měly být popsány správně s cílem a entity, které chcete, aby služba LUIS k vyhledání. Výsledky testů určit, jak dobře by na této sadě projevy provedl LUIS. 

## <a name="next-steps"></a>Další postup

Seznamte se s koncepty [spolupráci](luis-concept-collaborator.md).
