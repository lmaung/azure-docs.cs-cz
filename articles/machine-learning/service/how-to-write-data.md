---
title: 'Zápis: Python SDK pro přípravu dat'
titleSuffix: Azure Machine Learning service
description: Další informace o vytváření dat pomocí sady SDK služby Azure Machine Learning Data Prep. Můžete napsat data v libovolném bodě v toku dat a soubory v libovolné z našich podporovaných umístění (místního systému souborů Azure Blob Storage a Azure Data Lake Storage).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 1853ff4141a7af3260ef9575bb2457819ab2d4a9
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2019
ms.locfileid: "56735012"
---
# <a name="write-data-using-the-azure-machine-learning-data-prep-sdk"></a>Zápis dat pomocí sady SDK pro Azure Machine Learning Data Prep

V tomto článku najdete informace k zápisu dat pomocí různých metod [Azure Machine Learning Data Prep Python SDK](https://aka.ms/data-prep-sdk). Výstupní data lze zapsat v libovolném bodě toku dat a zápisy jsou přidány jako kroky pro výsledný toku dat a spouštějí se pokaždé, když je datový tok. Data se zapisují do více souborů oddíl umožňuje paralelní zápisy.

Vzhledem k tomu, že neexistují žádná omezení, kolik zapsat kroky jsou v kanálu, můžete snadno přidat další zápis kroky k získání průběžné výsledky při odstraňování problémů nebo jiných kanálů.

Vyvolá se pokaždé, když spustíte zápisu krok, úplné o přijetí změn dat v toku dat. Například toku dat s tři kroky zápis se číst a zpracovávat každý záznam v datové sadě třikrát.

## <a name="supported-data-types-and-location"></a>Podporované datové typy a umístění

Jsou podporovány následující formáty souborů
-   Soubory s oddělovači (CSV, TSV, atd.)
-   Soubory parquet

Pomocí sady SDK Azure Machine Learning Data Prep Python, můžete zapsat data do:
+ místního systému souborů
+ Azure Blob Storage
+ Azure Data Lake Storage

## <a name="spark-considerations"></a>Důležité informace o Sparku

Při spuštění toku dat ve Sparku, musíte napsat k prázdné složce. Probíhá pokus o spuštění zápis existující složce důsledkem chyby. Ujistěte se, že cílová složka je prázdná nebo použít jiné cílové umístění pro každé spuštění nebo zápisu se nezdaří.

## <a name="monitoring-write-operations"></a>Monitorování operací zápisu

Pro usnadnění práce je vygenerován sentinel soubor s názvem úspěch po dokončení zápisu. Jeho přítomnost pomáhá identifikovat, když zprostředkující zápisu byla dokončena bez nutnosti čekat celý kanál k dokončení.

## <a name="example-write-code"></a>Příklad zápisu kódu

V tomto příkladu začněte tím, že načítání dat do toku dat pomocí `auto_read_file()`. Je-li znovu použít tato data pomocí různých formátech.

```python
import azureml.dataprep as dprep
t = dprep.auto_read_file('./data/fixed_width_file.txt')
t = t.to_number('Column3')
t.head(5)
```

Příklad výstupu:
| | Sloupec1 | Column2 | Sloupec3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | Žádný | NO | NO | ENRS | NaN | NaN | NaN |   
|1| 10003.0 | 99999.0 | Žádný | NO | NO | ENSO | NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | Žádný | NO | JN | ENJA | 70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | Žádný | NO | NO |      | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | Žádný | NO | NO | ENSO | 59783.0 | 5350.0 |  500.0|

### <a name="delimited-file-example"></a>Příklad souboru s oddělovači

Následující kód používá [ `write_to_csv()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#write-to-csv-directory-path--destinationpath--separator--str--------na--str----na---error--str----error------azureml-dataprep-api-dataflow-dataflow) funkce k zápisu dat do souboru s oddělovači.

```python
# Create a new data flow using `write_to_csv` 
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'))

# Run the data flow to begin the write operation.
write_t.run_local()

written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

Příklad výstupu:
| | Sloupec1 | Column2 | Sloupec3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | CHYBA | NO | NO | ENRS | NaN    | NaN | NaN |   
|1| 10003.0 | 99999.0 | CHYBA | NO | NO | ENSO |    NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | CHYBA | NO | JN | ENJA |    70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | CHYBA | NO | NO |     | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | CHYBA | NO | NO | ENSO |    59783.0 | 5350.0 |  500.0|


V předchozím výstupu se zobrazí několik chyb v číselných sloupcích z důvodu čísla, která nebyla správně parsovat. Při zápisu do sdíleného svazku clusteru, jsou hodnoty null řetězcem "Chyba" nahrazuje výchozí.

Přidáte parametry jako součást vaší zápisu volání a specifikaci řetězec použitý k reprezentaci hodnoty null.

```python
write_t = t.write_to_csv(directory_path=dprep.LocalFileOutput('./test_out/'), 
                         error='BadData',
                         na='NA')
write_t.run_local()
written_files = dprep.read_csv('./test_out/part-*')
written_files.head(5)
```

Předchozí kód vytvoří tento výstup:
| | Sloupec1 | Column2 | Sloupec3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0| 10000.0 | 99999.0 | BadData | NO | NO | ENRS | NaN  | NaN | NaN |   
|1| 10003.0 | 99999.0 | BadData | NO | NO | ENSO |  NaN | NaN | NaN |   
|2| 10010.0 | 99999.0 | BadData | NO | JN | ENJA |  70933.0 | -8667.0 | 90.0 |
|3| 10013.0 | 99999.0 | BadData | NO | NO |   | NaN | NaN | NaN |
|4| 10014.0 | 99999.0 | BadData | NO | NO | ENSO |  59783.0 | 5350.0 |  500.0|

### <a name="parquet-file-example"></a>Příklad souboru parquet

Podobně jako `write_to_csv()`, [ `write_to_parquet()` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#write-to-parquet-file-path--typing-union--destinationpath--nonetype----none--directory-path--typing-union--destinationpath--nonetype----none--single-file--bool---false--error--str----error---row-groups--int---0-----azureml-dataprep-api-dataflow-dataflow) nového toku dat pomocí zápisu Parquet krok, který se spustí při spuštění toku dat vrátí funkce hodnotu.

```python
write_parquet_t = t.write_to_parquet(directory_path=dprep.LocalFileOutput('./test_parquet_out/'),
error='MiscreantData')
```

Spuštění toku dat. Pokud chcete spustit operaci zápisu.

```python
write_parquet_t.run_local()

written_parquet_files = dprep.read_parquet_file('./test_parquet_out/part-*')
written_parquet_files.head(5)
```

Předchozí kód vytvoří tento výstup:
|   | Sloupec1 | Column2 | Sloupec3 | Column4 | Column5 | Column6 | Column7 | Column8 | Column9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |-------- |
|0| 10000.0 | 99999.0 | MiscreantData | NO | NO | ENRS | MiscreantData | MiscreantData | MiscreantData |
|1| 10003.0 | 99999.0 | MiscreantData | NO | NO | ENSO | MiscreantData | MiscreantData | MiscreantData |   
|2| 10010.0 | 99999.0 | MiscreantData | NO| JN| ENJA|   70933.0|    -8667.0 |90.0|
|3| 10013.0 | 99999.0 | MiscreantData | NO| NO| |   MiscreantData|    MiscreantData|    MiscreantData|
|4| 10014.0 | 99999.0 | MiscreantData | NO| NO| ENSO|   59783.0|    5350.0| 500.0|

## <a name="next-steps"></a>Další postup
* Sada SDK [přehled](https://aka.ms/data-prep-sdk) vzory návrhu a příklady použití 
* Automatizované strojového učení najdete v článku [kurzu](tutorial-auto-train-models.md) příklad regresní model