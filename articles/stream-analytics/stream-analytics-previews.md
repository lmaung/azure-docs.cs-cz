---
title: Funkce Azure Stream Analytics ve verzi preview
description: Tento článek obsahuje seznam funkcí Azure Stream Analytics, které jsou aktuálně ve verzi preview.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/05/2019
ms.openlocfilehash: 6cd1d917be67d21e3e6dfe54ed5dec77b92509e9
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821451"
---
# <a name="azure-stream-analytics-preview-features"></a>Funkce Azure Stream Analytics ve verzi preview

Tento článek shrnuje všechny funkce aktuálně ve verzi preview pro Azure Stream Analytics. Pomocí funkce ve verzi preview v produkčním prostředí se nedoporučuje.

## <a name="public-previews"></a>Verze Public Preview

Následující funkce jsou ve verzi public preview. Můžete využít výhod těchto funkcích ještě dnes, ale nemusíte používat v produkčním prostředí.

### <a name="anomaly-detection"></a>Detekce anomálií

Azure Stream Analytics zavádí nové modely strojového učení s podporou *zásobníku* a *poklesy* detekce kromě detekce obousměrný, pomalé kladné a pomalé negativních trendů. Další informace najdete v článku [detekce anomálií ve službě Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="sql-database-reference-data"></a>SQL Database referenčních dat

Azure Stream Analytics podporuje Azure SQL Database jako zdroj vstupu pro referenční data. SQL Database můžete použít jako referenční data pro vaši úlohu Stream Analytics na portálu Azure portal a v sadě Visual Studio pomocí nástroje Stream Analytics. Další informace najdete, [pomocí referenčních dat z databáze serveru SQL pro úlohy Azure Stream Analytics](sql-reference-data.md).

### <a name="integration-with-azure-machine-learning"></a>Integrace s Azure Machine Learning

Je možné škálovat úlohy Stream Analytics s funkcemi Machine Learning (ML). Další informace o použití funkce ML v úloze Stream Analytics, najdete v tématu [škálovat úlohy Stream Analytics s funkcemi Azure Machine Learning](stream-analytics-scale-with-machine-learning-functions.md). Podívejte se na skutečném scénáři s [provádět analýzu subjektivního hodnocení s využitím Azure Stream Analytics a Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="javascript-user-defined-aggregate"></a>Uživatelem definovaná agregace jazyka JavaScript

Azure Stream Analytics podporuje uživatelem definované agregace (UDA) napsané v jazyce JavaScript, které vám umožní implementovat komplexní stavové obchodní logiku. Další informace o použití UDA z [jazyka JavaScript v Azure Stream Analytics uživatelem definovaných agregacích](stream-analytics-javascript-user-defined-aggregates.md) dokumentaci. 

### <a name="live-data-testing-in-visual-studio"></a>Živé testování data v sadě Visual Studio

Visual Studio tools pro Azure Stream Analytics rozšířit místní testování funkce, která využijete k otestování dotazů vůči datové proudy živou událost z cloudových zdrojů, jako je například Event Hubu nebo služby IoT hub. Zjistěte, jak [živá data místně pomocí nástroje Azure Stream Analytics pro Visual Studio Test](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>.NET uživatelsky definovaných funkcí na hraničních zařízeních IoT

Pomocí .NET standard uživatelem definované funkce můžete spustit .NET Standard kódu jako součást vašeho streamovacího kanálu. Můžete vytvořit jednoduché třídy jazyka C# nebo import projektů a knihovny. Úplné pro vytváření a ladění je podporováno v sadě Visual Studio. Další informace najdete v článku [vývoj .NET Standard uživatelsky definovaných funkcí pro úlohy Azure Stream Analytics Edge](stream-analytics-edge-csharp-udf-methods.md).

## <a name="private-previews"></a>Privátní verze Preview

Následující funkce jsou ve verzi private preview.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge"></a>C# vlastní deserializátor pro Azure Stream Analytics na hraničních zařízeních IoT

Vývojáři teď můžete implementovat vlastní deserializers v jazyce C# k deserializaci událostí přijatých službou Azure Stream Analytics. Příklady formátů, které lze deserializovat: Parquet, Protobuf, XML nebo libovolný binární formát.

### <a name="managed-identities-for-azure-resource-authentication-to-azure-data-lake-storage"></a>Spravovaných identit pro ověřování prostředků Azure do služby Azure Data Lake Storage

Můžete teď provozu v reálném čase kanály pomocí spravované identity pro prostředky Azure na základě ověření při zápisu do Azure Data Lake Storage Gen1 umožňuje programově vytvářet úlohy. Další informace najdete v článku [použití spravovaných identit pro prostředky Azure, které ověřování Azure Stream Analytics úloh do Azure Data Lake Storage Gen1 výstupu](stream-analytics-managed-identities-adls.md).

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code pro Azure Stream Analytics

Úlohy Azure Stream Analytics se můžou vytvořit ve Visual Studio Code. Pro přístup k nástrojů funkce ve verzi private preview, obraťte se na *ASAToolsfeedback@microsoft.com*.

## <a name="next-steps"></a>Další postup

* [Osm nových funkcí ve službě Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/)

* [Čtyři nové funkce, které jsou teď dostupné ve službě Azure Stream Analytics](https://azure.microsoft.com/blog/4-new-features-now-available-in-azure-stream-analytics/)
