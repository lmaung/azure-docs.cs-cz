---
title: Zpracování událostí v reálném čase pomocí Azure Stream Analytics zpracování událostí
description: Tento článek popisuje referenční architektury, abyste dosáhli zpracování událostí v reálném čase a analytics pomocí Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 7f9748a4e4f1c86362781aa80d8958237c97106a
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555434"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Referenční architektura: S Microsoft Azure Stream Analytics pro zpracování událostí v reálném čase
Referenční architektura pro zpracování ve službě Azure Stream Analytics událostí v reálném čase je určená k zajištění obecný plán pro nasazení v reálném čase platforma jako služba (PaaS) řešení zpracování datových proudů s Microsoft Azure.

## <a name="summary"></a>Souhrn
Tradičně mají byla analytická řešení založená na možnosti, jako jsou ETL (extrakce, transformace, načtení) a datové sklady, kde jsou data uložena před analýzy. Měnícím se požadavkům, včetně další rychle příchozích dat jsou doručením (push) tento existující model do limitu. Umožňuje analyzovat data v rámci přesun datové proudy před úložiště je jedním z řešení, a i když není novou funkci, přístup nebyl široce přijat ve všech odvětví vertikálně. 

Microsoft Azure poskytuje rozsáhlou katalogu analytických technologií, které podporují podporující celou řadu různých řešení scénáře a požadavky. Výběr služby Azure k nasazení pro-ucelené řešení může být náročné zadaný široké nabídky. Tento dokument je určen k popisují funkce a vzájemná spolupráce grafického subsystému různých služeb Azure, které podporují řešení aplikace Streamovat události. Také vysvětluje některé scénáře, ve kterých zákazníci využívat tento typ přístupu přináší.

## <a name="contents"></a>Obsah
* Shrnutí
* Úvod do analýzy v reálném čase
* Návrh hodnoty z dat v reálném čase v Azure
* Časté scénáře pro analýzu v reálném čase
* Architektura a komponenty
  * Zdroje dat
  * Integrace dat vrstvy
  * Analýzy v reálném čase vrstvy
  * Datová vrstva úložiště
  * Prezentace / spotřeby vrstvy
* Závěr

**Autor:** Charles Feddersen, architekt řešení, datové centrum Insights vzdělávání profesionálů v oblasti, Microsoft Corporation

**Publikováno:** Leden 2015

**Revize:** 1.0

**Stažení:** [S Microsoft Azure Stream Analytics pro zpracování událostí v reálném čase](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Podpora
Potřebujete další pomoc, zkuste [fóru Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další postup
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

