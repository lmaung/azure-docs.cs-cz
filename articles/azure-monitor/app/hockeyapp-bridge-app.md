---
title: Zkoumání dat HockeyApp ve službě Azure Application Insights | Dokumentace Microsoftu
description: Analýza využití a výkonu vaší aplikace Azure pomocí Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/30/2017
ms.author: mbullwin
ms.openlocfilehash: 4115ec5add9ac523852b4c60c4f9d750bc430a37
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121443"
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Zkoumat data HockeyApp v Application Insights

> [!NOTE]
> HockeyApp už nejsou k dispozici pro nové aplikace. Stávající nasazení HockeyApp budou nadále fungovat. Visual Studio App Center je teď doporučená služba od Microsoftu pro monitorování nových mobilních aplikací. [Další informace o nastavení aplikace pomocí App Center a Application Insights](../../azure-monitor/learn/mobile-center-quickstart.md).

[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) je služba pro sledování živého desktopových a mobilních aplikací. Z HockeyApp můžete odeslat vlastní a sledovat telemetrii ke sledování využití a pomoct s diagnostikou (navíc k získávání dat o chybách). Tento datový proud telemetrických dat může být dotázán pomocí výkonný [Analytics](../../azure-monitor/app/analytics.md) funkce [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Kromě toho můžete [exportovat vlastní a sledovat telemetrii](export-telemetry.md). Pokud chcete povolit tyto funkce, nastavení mostu, který předává HockeyApp vlastních dat do Application Insights.

## <a name="the-hockeyapp-bridge-app"></a>Aplikace HockeyApp Bridge
Aplikace HockeyApp Bridge je základní funkce, která umožňuje přístup k vlastní HockeyApp a telemetrická data trasování v Application Insights prostřednictvím funkce analýzu a průběžný Export. Vlastní a trasování událostí shromážděných za HockeyApp po vytvoření aplikace HockeyApp Bridge bude přístupné z těchto funkcí. Podívejme se na tom, jak nastavit jednu z těchto aplikací mostu.

V Hockeyappu, otevřete nastavení účtu [tokeny API](https://rink.hockeyapp.net/manage/auth_tokens). Vytvořit nový token nebo znovu použít nějaký existující. Minimální práva potřebná jsou "jen pro čtení". Pořiďte si rozhraní API tokenů.

![Získání tokenu pro rozhraní API HockeyApp](./media/hockeyapp-bridge-app/01.png)

Otevřít na portálu Microsoft Azure a [vytvořte prostředek Application Insights](../../azure-monitor/app/create-new-resource.md ). Typ aplikace nastavte na "Aplikace HockeyApp bridge":

![Nový prostředek Application Insights](./media/hockeyapp-bridge-app/02.png)

Není nutné nastavit název – to se automaticky nastaví z názvu HockeyApp.

Zobrazí pole most HockeyApp. 

![Zadejte pole bridge](./media/hockeyapp-bridge-app/03.png)

Zadejte token HockeyApp, kterou jste si předtím poznamenali. Tato akce vyplní "Aplikace HockeyApp" rozevírací nabídky s vašimi aplikacemi HockeyApp. Vyberte ten, který chcete použít a dokončete zbývající pole. 

Otevřete nový prostředek. 

Všimněte si, že data trvá určitou dobu spuštění toku.

![Čekání na data prostředku Application Insights](./media/hockeyapp-bridge-app/04.png)

A to je vše! Vlastní a trasování dat shromážděných ve vaší aplikaci instrumentované HockeyApp od té chvíle se teď také k dispozici, analýzu a průběžný Export funkcí Application Insights.

Pojďme krátce se podívat na každou z těchto funkcí, které jsou teď k dispozici.

## <a name="analytics"></a>Analýzy
Analýza je výkonný nástroj pro dotazování ad hoc vašich dat díky tomu můžete diagnostikovat a analýze telemetrických dat a rychle vyhledat hlavní příčiny a vzory.

![Analýzy](./media/hockeyapp-bridge-app/05.png)

* [Další informace o analýze](../../azure-monitor/log-query/get-started-portal.md)

## <a name="continuous-export"></a>Průběžný export
Průběžný Export umožňuje exportovat data do kontejneru úložiště objektů Blob v Azure. To je velmi užitečné, pokud je potřeba zachovejte si svá data po dobu delší než doba uchování v současné době nabízena službou Application Insights. Můžete ponechat data v úložišti objektů blob, zpracovat je do databáze SQL nebo váš upřednostňovaný řešení datového skladu.

[Další informace o průběžný Export](export-telemetry.md)

## <a name="next-steps"></a>Další postup
* [Použití analýzy k vašim datům](../../azure-monitor/log-query/get-started-portal.md)

