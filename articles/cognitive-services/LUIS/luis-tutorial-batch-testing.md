---
title: Testovací služby batch
titleSuffix: Azure Cognitive Services
description: Tento kurz ukazuje, jak pomocí služby batch testu najít utterance předpovědi problémy ve vaší aplikaci a opravte je.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 97dd880f7342c8d7f4854bb06abf2906bbd4c50a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55872964"
---
# <a name="tutorial-batch-test-data-sets"></a>Kurz: Batch testovací datové sady

Tento kurz ukazuje, jak pomocí služby batch testu najít utterance předpovědi problémy ve vaší aplikaci a opravte je.  

Testování služby batch vám pomůže ověřit aktivní, Trénink modelu stavu se známou sadou s popiskem projevy a entit. V souboru ve formátu JSON služby batch přidejte projevy a nastavte popisky entity, které potřebujete předpovědět uvnitř utterance. 

Požadavky pro testování služby batch:

* Maximálně 1000 projevy na test. 
* Žádné duplicity. 
* Povolené typy entit: pouze entity se naučili obrobeny použití jednoduchých, hierarchické (pouze nadřazené) a složené. Testování služby batch je užitečná pouze pro obrobeny zjistili záměry a entity.

Při používání aplikace než v tomto kurzu, proveďte *není* pomocí příkladu projevy již byla přidána do záměru. 

**V tomto kurzu se naučíte:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importovat ukázková aplikace
> * Vytvořte dávkový soubor testu 
> * Spuštění testu služby batch
> * Kontrola výsledků testu
> * Opravit chyby 
> * Opětovné testování služby batch

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importovat ukázková aplikace

Pokračujte s aplikací **HumanResources**, kterou jste vytvořili v posledním kurzu. 

Použijte k tomu následující postup:

1.  Stáhněte si [soubor JSON aplikace](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-review-HumanResources.json) a uložte si ho.

2. Naimportujte soubor JSON do nové aplikace.

3. V části **Manage** (Správa) na kartě **Versions** (Verze) naklonujte verzi a pojmenujte ji `batchtest`. Klonování představuje skvělý způsob, jak si můžete vyzkoušet různé funkce služby LUIS, aniž by to mělo vliv na původní verzi. Název verze je součástí cesty URL, a proto smí obsahovat jenom znaky, které jsou platné v adresách URL. 

4. Trénujte aplikaci.

## <a name="batch-file"></a>Dávkový soubor

1. Vytvoření `HumanResources-jobs-batch.json` v textovém editoru nebo [Stáhnout](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-jobs-batch.json) ho. 

2. V souboru ve formátu JSON batch přidání projevů s **záměr** chcete předpokládané v testu. 

   [!code-json[Add the intents to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-jobs-batch.json "Add the intents to the batch test file")]

## <a name="run-the-batch"></a>Spusťte dávku

1. Vyberte **Test** v horním navigačním panelu. 

2. Vyberte **Batch testování panel** na pravé straně panelu. 

    [![Aplikace LUIS snímek obrazovky s panelem test Batch zvýrazněnou](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Vyberte **datové sady importu**.

    [![Aplikace LUIS snímek obrazovky s datovou sadou Import zvýrazněnou](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. Zvolit umístění souboru `HumanResources-jobs-batch.json` souboru.

5. Zadejte název datové sady `intents only` a vyberte **provádí**.

    ![Výběr souboru](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. Vyberte tlačítko **Spustit**. 

7. Vyberte **zobrazit výsledky**.

8. Zkontrolujte výsledky v grafu a legend.

    [![Snímek obrazovky LUIS aplikace s výsledky testů služby batch](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Zkontrolujte výsledky služby batch

Batch graf zobrazuje čtyři kvadrantech výsledky. Napravo od grafu je filtr. Ve výchozím nastavení je nastaven filtr na první záměr v seznamu. Filtr obsahuje všechny záměry a pouze jednoduché, hierarchické (pouze nadřazené) a složené entity. Když vyberete [části grafu](luis-concept-batch-test.md#batch-test-results) nebo bodu v rámci tohoto grafu, přidružené utterance(s) zobrazení pod grafem. 

Když najede myší na graf, kolečko myši můžete zvětšit nebo zmenšit zobrazení v grafu. To je užitečné, když existuje mnoho bodů v grafu úzce společně v clusteru. 

Graf je v čtyři kvadrantech dva oddíly zobrazí červeně. **Toto jsou oddíly pro zaměření na**. 

### <a name="getjobinformation-test-results"></a>Výsledky testu GetJobInformation

**GetJobInformation** zobrazit výsledky testů zobrazené ve filtru, že byla úspěšná 2 čtyři předpovědí. Vyberte název **falešně pozitivní** nad horním pravém kvadrantu zobrazíte projevy pod grafem. 

![Služba LUIS projevy testovací služby batch](./media/luis-tutorial-batch-testing/hr-applyforjobs-false-positive-results.png)

Proč jsou dvě projevy předpovědět jako **ApplyForJob**, namísto správné záměr **GetJobInformation**? Co se týče možnosti aplikace word a uspořádání word jsou velmi úzce souvisí dvou příkazů. Kromě toho jsou skoro tři třikrát tolik příklady **ApplyForJob** než **GetJobInformation**. Tato nerovnosti příklad projevy váží **ApplyForJob** upřednostnit na záměr. 

Všimněte si, že oba záměry mají stejný počet chyb. Nesprávný predikce v jedné záměr má vliv na ostatní záměr také. Oba obsahuje chyby, protože projevy byly správně předpovědět pro jeden záměr a také nesprávně ne pro jiné záměr předpovědět. 

![Služba LUIS batch test filtrování chyb](./media/luis-tutorial-batch-testing/hr-intent-error-count.png)

K časovému okamžiku projevy odpovídající nahoře **falešně pozitivní** jsou části `Can I apply for any database jobs with this resume?` a `Can I apply for any database jobs with this resume?`. Pro první utterance slovo `resume` pouze se použil **ApplyForJob**. Pro druhý utterance slovo `apply` pouze se použil **ApplyForJob** záměr.

## <a name="fix-the-app"></a>Oprava aplikace

Cílem této části je, aby všechny projevy správně předpovědět pro **GetJobInformation** po opravě aplikace. 

Přidání projevů soubor tyto služby batch na správné záměr je zdánlivě rychlé opravy. To je, co nechcete dělat ale. Chcete, aby služba LUIS správně předpovědět těchto projevů bez nutnosti přidávat jako příklady. 

Může vás také zajímat o odebrání projevy z **ApplyForJob** dokud množství utterance je stejný jako **GetJobInformation**. Který lze pravděpodobně vyřešit výsledky testů, ale bude bránit LUIS z přesně předpovídat tohoto záměru příště. 

První opravu, je přidat další projevy do **GetJobInformation**. Druhý oprava je omezení hmotnosti slova, jako je `resume` a `apply` směrem k **ApplyForJob** záměr. 

### <a name="add-more-utterances"></a>Přidání další projevů

1. Zavřít panel dávky testů tak, že vyberete **testování** tlačítko v horním navigačním panelu. 

2. Vyberte **GetJobInformation** ze seznamu záměry. 

3. Přidat další projevy, které jsou různé délky, volba slov a uspořádání slova, nezapomeňte zahrnout podmínky `resume`, `c.v.`, a `apply`:

    |Příklad projevy pro **GetJobInformation** záměr|
    |--|
    |Vyžaduje novou úlohu v datovém skladu pro stocker můžu použít s životopis?|
    |Kde se dnes střešní úlohy?|
    |Jsem slyšela, že došlo lékařské kódovací úlohu, která vyžaduje životopis.|
    |Chci dostávat úlohu pomáhá zápis jejich c.v.s. děti škole |
    |Tady je Moje pokračování hledání nového příspěvku na komunitní škole pomocí počítače se.|
    |Jaké pozice jsou k dispozici v podřízené domény a domovské péče?|
    |Je k dispozici učně helpdesku na novinových?|
    |Moje C.v. ukazuje, že jsem vhodný pro analýzu zajišťování rozpočtů a ke ztrátě peníze. Je cokoli, co je pro tento typ práce?|
    |Kde jsou země procházení úlohy nyní?|
    |Jsem pracovala 8 let jako ovladač EMS. Žádné nové úlohy?|
    |Nové úlohy zpracování potravin vyžadují aplikace?|
    |Kolik nové yard pracovní úlohy jsou k dispozici?|
    |Existuje nový příspěvek HR pro práci s vývojáři a vyjednávání?|
    |Mám k dispozici nadřízených serverů knihovny a archivní řízení. Všechny nové pozice?|
    |Existují jakékoli babysitting úlohy pro 13 olds rok ve městě dnes?|

    Neoznačuje popiskem **úlohy** entity v projevy. V této části kurzu se zaměřuje na pouze záměru předpovědi.

4. Trénování aplikace tak, že vyberete **Train** v pravém horním navigačním panelu.

## <a name="verify-the-new-model"></a>Ověřte nový model

Pokud chcete ověřit, že jsou správně předpovědět projevy v testu služby batch, znovu spusťte test služby batch.

1. Vyberte **Test** v horním navigačním panelu. Pokud výsledky batch jsou stále otevřen, vyberte **zpět do seznamu**.  

2. Vyberte tři tečky (***...*** ) tlačítko vpravo od názvu služby batch a vyberte **spouštět datovou sadu**. Počkejte, dokud se provádí test služby batch. Všimněte si, že **zobrazit výsledky** je nyní zelené tlačítko. To znamená, že celý batch proběhla úspěšně.

3. Vyberte **zobrazit výsledky**. Příkazů by měly mít zelené ikony nalevo od názvu záměru. 

    ![Snímek obrazovky služby LUIS se zvýrazněným tlačítkem výsledky služby batch](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png)

## <a name="create-batch-file-with-entities"></a>Vytvořte dávkový soubor s entitami 

Aby bylo možné ověřit entity v rámci služby batch testu, entity muset označeny v dávkovém souboru JSON. Pouze entity zjištěné počítače se používají: jednoduchý, hierarchické (pouze nadřazené) a složené entity. Nepřidávejte mimo počítač zjistili entity, protože se nenachází vždy prostřednictvím regulární výrazy nebo explicitní text odpovídá.

Změnu entity pro celkový počet slov ([token](luis-glossary.md#token)) počet může mít vliv na kvalitu předpovědi. Zajistěte, aby trénovacích dat zadané na záměr s s popiskem projevy zahrnuje celou řadu délky entity. 

Při prvním psaní a testování dávkové soubory, je nejlepší začít s několika projevy a entity, které znáte pracovat, a také pár, který jste domníváte se může správně předpovědět. Díky tomu umožní věnovat v problémových oblastí rychle. Po otestování **GetJobInformation** a **ApplyForJob** záměry pomocí několika různých úloh názvy, které nebyly předpovědět, zobrazíte, pokud dojde k problému předpovědi byl vyvinut tento dávkový soubor testu s určitými hodnotami pro **úlohy** entity. 

Hodnota **úlohy** entita, součástí projevy testu je obvykle jedno nebo dvě slova s pár příkladů se další slova. Pokud _vlastní_ lidských zdrojů, aplikace obvykle obsahuje názvy úlohy řada výrazů, příklad projevy označené **úlohy** entity v této aplikaci nebude fungovat správně.

1. Vytvoření `HumanResources-entities-batch.json` v textovém editoru, jako [VSCode](https://code.visualstudio.com/) nebo [Stáhnout](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-entities-batch.json) ho.


2. V souboru ve formátu JSON služby batch, přidejte pole objektů, které zahrnují projevy s **záměr** chcete předpokládané v testu, stejně jako umístění všech entit v utterance. Entita je založená na tokenech, ujistěte se, že ke spouštění a zastavování Každá entita na znak. Začínat nebo končit utterance v prostoru. To způsobí chybu při importu souboru služby batch.  

   [!code-json[Add the intents and entities to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-entities-batch.json "Add the intents and entities to the batch test file")]


## <a name="run-the-batch-with-entities"></a>Spusťte dávku s entitami

1. Vyberte **Test** v horním navigačním panelu. 

2. Vyberte **Batch testování panel** na pravé straně panelu. 

3. Vyberte **datové sady importu**.

4. Vyberte umístění systému souboru `HumanResources-entities-batch.json` souboru.

5. Zadejte název datové sady `entities` a vyberte **provádí**.

6. Vyberte tlačítko **Spustit**. Počkejte, dokud se provádí test.

7. Vyberte **zobrazit výsledky**.

## <a name="review-entity-batch-results"></a>Zkontrolujte výsledky entit služby batch

Grafu se otevře s všechny záměry správně předpovědět. Přejděte dolů na pravé straně filtru najít entitu predikcí ohlásí chybu. 

1. Vyberte **úlohy** entity ve filtru.

    ![Predikce entity ohlásí chybu ve filtru](./media/luis-tutorial-batch-testing/hr-entities-filter-errors.png)

    Graf se změny zobrazily predikcí entity. 

2. Vyberte **falešně negativní** v nižší, zůstane kvadrantu grafu. Pak pomocí klávesnice kombinaci CTRL + E přepněte do zobrazení tokenu. 

    [![Token zobrazení entity predikcí](./media/luis-tutorial-batch-testing/token-view-entities.png)](./media/luis-tutorial-batch-testing/token-view-entities.png#lightbox)
    
    Recenzování projevy pod grafem odhalí konzistentní chyba, pokud název úlohy obsahuje `SQL`. Kontrola příklad projevy a seznam úloh frázi, SQL je jen jednou a pouze jako součást názvu větší úlohy `sql/oracle database administrator`.

## <a name="fix-the-app-based-on-entity-batch-results"></a>Oprava aplikace na základě výsledků entity batch

Oprava aplikace vyžaduje LUIS správně určit variant úloh SQL. Pro opravu několika způsoby. 

* Explicitně přidáte další příklad projevy, které používají SQL a označovat pomocí popisků těchto slov jako entita úlohy. 
* Explicitně přidáte do seznamu slovní spojení více úloh SQL

Tyto úlohy jsou ponechána musíte udělat.

Přidávání [vzor](luis-concept-patterns.md) před entity je správně předpovědět, nebude tento problém vyřešit. Je to proto, že vzor nebudou odpovídat, dokud se zjistí všechny entity ve vzoru. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Další postup

Tento kurz používá testovací služby batch k najít problémy s aktuální model. Model bylo opraveno a podrobeny novému testování s dávkový soubor, pokud chcete ověřit že správné provedení změny.

> [!div class="nextstepaction"]
> [Další informace o vzorech](luis-tutorial-pattern.md)

