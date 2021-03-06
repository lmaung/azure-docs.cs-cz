---
title: Konverzace Learner výchozí konfigurace – Microsoft Cognitive Services | Dokumentace Microsoftu
titleSuffix: Azure
description: Další informace o konverzaci Learner výchozí konfiguraci.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8050008bbae4a23f09b5fa94874a6315e798b448
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211283"
---
# <a name="default-values-and-boundaries"></a>Výchozí hodnoty a hranice

Tento dokument popisuje výchozí konfigurace Learner konverzace a hranice klíčových služeb.

## <a name="default-configuration"></a>Výchozí konfigurace

Parametr | Výchozí hodnota
--- | --- 
Výchozí časový limit relace | 30 minut

## <a name="boundaries"></a>Hranice

Parametr | Omezení
--- | --- 
Vytváření rozhraní API, maximální HTTP volání za měsíc | 5 MIN
Vytváření rozhraní API, maximální HTTP volání za sekundu | 25
Relace rozhraní API, maximální HTTP volání za měsíc | 500 000
Rozhraní API relace, maximální HTTP volání za sekundu | 10
Maximální počet vlastních entit (bez-prostřednictvím kódu programu) na modelu | Zobrazit [LUIS hranice doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries); v praxi, skutečný počet může být o něco menší
Maximální počet předem připravených entit na modelu | Zobrazit [doc hranice LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries)
Maximální počet entit (v celkem) na modelu | 100
Maximální počet akcí za model | 32
Maximální počet, dialogová okna trénování za model | 1000
Maximální počet uživatelů se změní na dialogové okno trénování | 100
Maximální počet, dialogová okna protokolu na modelu | Žádné předem nastavený limit, ale protokol dialogová okna se uchovávají pouze na pevné období, než bude zahozen.  Navíc zobrazí rozhraní Learner konverzace 100 dialogová okna protokolu najednou. 
Maximální počet modelů na uživatele | Žádné předem nastavený limit
Maximální počet akcí sekvenční bez čekání | 5 (*)

(*) Po 5 sekvenční akce bez čekání se maskují všechny akce bez čekání, a potom vybere Learner konverzace mezi akce k dispozici čekání.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Začínáme s Learner konverzace](./quickstart.md)
