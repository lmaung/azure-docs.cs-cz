---
title: Vysvětlení vstupy pro Azure Stream Analytics
description: Tento článek popisuje koncept vstupů v úloze Azure Stream Analytics, porovnání datových proudů vstup do referenčního datového vstupu.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: a6b6b548f1e5a9fa1e92b5fdbd714117758d75b4
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737072"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Vysvětlení vstupy pro Azure Stream Analytics

Připojte se k jeden nebo více vstupů dat úloh Azure Stream Analytics. Každá vstupní definuje připojení k existujícímu zdroji dat. Stream Analytics přijímá příchozí data z několika druhů zdroje událostí, včetně služby Event Hubs, služby IoT Hub a Blob storage. Vstupy jsou odkazovány podle názvu v streamování jazyka SQL, který píšete pro každou úlohu. V dotazu můžete připojit k více vstupů blend dat nebo streamovaná data s vyhledávání pro referenční data porovnat a předat výsledky do výstupů. 

Stream Analytics je prvotřídní integrace z tři druhy prostředků jako vstupy:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Tyto prostředky vstupní může existovat ve stejném předplatném Azure jako svou úlohu Stream Analytics, nebo z jiného předplatného.

Můžete použít [webu Azure portal](stream-analytics-quick-create-portal.md#configure-job-input), [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), [.Net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [rozhraní REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input), a [sady Visual Studio](stream-analytics-tools-for-visual-studio-install.md)vytvářet, upravovat a testovat vstupy úlohy Stream Analytics.

## <a name="stream-and-reference-inputs"></a>Stream a odkaz na vstupy
Jak se data odesílají do zdroje dat, má používané úlohy Stream Analytics a zpracovávat v reálném čase. Vstupy jsou rozděleny do dvou typů: data datového proudu vstupů a referenční datové výstupy.

### <a name="data-stream-input"></a>Datový proud vstup
Datový proud je bez vazby posloupnost událostí v čase. Úlohy Stream Analytics musí obsahovat alespoň jeden vstup datového streamu. Event Hubs, služby IoT Hub a Blob storage jsou podporovány jako vstupní zdroje dat datového proudu. Event Hubs slouží ke shromažďování streamů událostí z více zařízení a služeb. Tyto datové proudy může obsahovat aktivity kanálů ze sociálních médií, uložených obchodních informací nebo data ze senzorů. Centra IoT hub jsou optimalizované pro shromažďování dat z připojených zařízení ve scénářích Internet of Things (IoT).  Úložiště objektů BLOB můžete použít jako vstupní zdroj pro příjem dat hromadného jako datový proud, jako jsou například soubory protokolu.  

Další informace o streamování datových výstupů, naleznete v tématu [Stream data jako vstup do Stream Analytics](stream-analytics-define-inputs.md)

### <a name="reference-data-input"></a>Referenčního datového vstupu
Stream Analytics podporuje také označuje jako vstup *odkazují na data*. Referenční data jsou buď zcela statická nebo pomalu se změní. Používá se obvykle provádět korelaci a vyhledávání. Například můžete spojit data vstup datového streamu k datům v referenčních dat, podobně jako můžete provést připojení SQL k vyhledání statickými hodnotami. Azure Blob storage je momentálně podporovaná jenom vstupní zdroj pro referenční data. Objekty BLOB referenčních dat zdroje mají omezení 300 MB velikosti, v závislosti na složitosti dotazu a přidělovat jednotek streamování.

Další informace o referenční datové výstupy, naleznete v tématu [pomocí referenčních dat pro vyhledávání ve službě Stream Analytics](stream-analytics-use-reference-data.md)

## <a name="next-steps"></a>Další postup
> [!div class="nextstepaction"]
> [Rychlé zprovoznění: Vytvoření úlohy Stream Analytics pomocí webu Azure portal](stream-analytics-quick-create-portal.md)
