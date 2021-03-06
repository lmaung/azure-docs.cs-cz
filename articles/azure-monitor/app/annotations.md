---
title: Poznámky pro službu Application Insights k verzi | Dokumentace Microsoftu
description: Přidat nasazení nebo vytvořit značky do grafy Průzkumníka metrik ve službě Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: mbullwin
ms.openlocfilehash: 652591fc4539e6f19c0606c1502609a823327f2b
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55811014"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Poznámky na grafy metrik ve službě Application Insights

Poznámky na [Průzkumníka metrik](../../azure-monitor/app/metrics-explorer.md) grafy zobrazují, kam jste nasadili nového sestavení nebo jiné významné události. Využívají ji snadno zjistit, zda změny měla vliv na výkon vaší aplikace. Můžete být automaticky vytvoří podle [Azure DevOps služby sestavovací systém](https://docs.microsoft.com/azure/devops/pipelines/tasks/). Můžete také vytvořit poznámky označit, že všechny události, kterou chcete, můžete vytvořit z prostředí PowerShell.

> [!NOTE]
> Tento článek odráží zastaralá **prostředí klasické metriky**. Poznámky jsou aktuálně dostupné v klasickém prostředí a  **[sešity](../../azure-monitor/app/usage-workbooks.md)**. Další informace o aktuálním prostředí metriky, můžete konzultovat [v tomto článku](../../azure-monitor/platform/metrics-charts.md).

![Příklad poznámky s viditelné korelace s doba odezvy serveru](./media/annotations/00.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Poznámky k sestavení služeb Azure DevOps verzi

Poznámky k verzi jsou funkce cloudovou službu Azure kanály DevOps služeb Azure. 

### <a name="install-the-annotations-extension-one-time"></a>Instalace rozšíření poznámky (jednou)
Aby bylo možné vytvořit anotace k vydání verze, bude nutné nainstalovat některou z mnoha služeb Azure DevOps rozšíření jsou dostupná v aplikaci Visual Studio Marketplace.

1. Přihlaste se k vaší [Azure DevOps služby](https://visualstudio.microsoft.com/vso/) projektu.
2. Visual Studio Marketplace [získat rozšíření poznámek](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)a přidejte ji do vaší organizace služby Azure DevOps.

![V horní pravé části stránky webové služby Azure DevOps, open Marketplace. Vyberte služby Azure DevOps a v části Azure kanály, zvolte možnost zobrazit další.](./media/annotations/10.png)

Stačí provést jednou pro organizaci služeb Azure DevOps. Poznámky k verzi lze nyní nastavit pro libovolný projekt ve vaší organizaci. 

### <a name="configure-release-annotations"></a>Konfigurace poznámek

Je nutné získat samostatný klíč rozhraní API pro každé šablony vydané verze služby Azure DevOps.

1. Přihlaste se k [Microsoft Azure Portal](https://portal.azure.com) a otevřete prostředek Application Insights, který monitoruje vaše aplikace. (Nebo [založte si ho teď](../../azure-monitor/app/app-insights-overview.md), pokud jste tak ještě neučinili.)
2. Otevřít **přístup přes rozhraní API**, **Id aplikace Insights**.
   
    ![Na stránce portal.azure.com otevřete prostředek Application Insights a vyberte nastavení. Otevřete přístup přes rozhraní API. Zkopírujte ID aplikace](./media/annotations/20.png)

4. V samostatném okně prohlížeče otevřete (nebo vytvořte) šablony vydané verze, která spravuje vaše nasazení služby Azure DevOps. 
   
    Přidat úlohy a vyberte úlohu poznámky Application Insights verze v nabídce.
   
    Vložit **Id aplikace** , který jste zkopírovali z okna přístup přes rozhraní API.
   
    ![Ve službě Azure DevOps Services otevřete verzi, vyberte kanál pro vydávání verzí a klikněte na položku upravit. Klikněte na tlačítko Přidat úkol a vyberte Application Insights verze poznámky. Vložte ID aplikace Insights.](./media/annotations/30.png)
4. Nastavte **APIKey** pole proměnné `$(ApiKey)`.

5. Zpět v okně Azure vytvořte nový klíč rozhraní API a pořiďte si ho.
   
    ![V okně přístup přes rozhraní API v okně Azure klikněte na vytvořit klíč rozhraní API. Zadejte komentář, zkontrolujte poznámky zápisu a klikněte na tlačítko vygenerovat klíč. Zkopírujte nový klíč.](./media/annotations/40.png)

6. Otevřete kartu Konfigurace šablony vydané verze.
   
    Vytvořit definici proměnné pro `ApiKey`.
   
    Vložte svůj klíč rozhraní API pro definici proměnné ApiKey.
   
    ![V okně Azure DevOps služby vyberte na kartě Konfigurace a klikněte na tlačítko Přidat proměnnou. Nastavte název ApiKey a do hodnoty, vložte klíč, který jste právě vygenerovali a klikněte na ikonu zámku.](./media/annotations/50.png)
7. Nakonec **Uložit** kanál pro vydávání verzí.


## <a name="view-annotations"></a>Zobrazit poznámky
Nyní pokaždé, když použijete šablonu vydané verze pro nasazení nové verze, Poznámka se odešlou do služby Application Insights. Poznámky se zobrazí v grafech v Průzkumníku metrik.

Klikněte na všechny značky poznámek otevřete podrobnosti o verzi, včetně žadatel, zdrojová větev ovládacího prvku, vydaných verzí kanálu, prostředí a další.

![Klikněte na možnost všechny verze poznámky značky.](./media/annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Vytvoření vlastní poznámky z prostředí PowerShell
Můžete také vytvořit poznámky z nějaký proces, který rádi používáte (bez použití služby Azure DevOps). 


1. Vytvořit místní kopii [Powershellový skript z Githubu](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Získání ID aplikace a vytvořte klíč rozhraní API v okně přístup přes rozhraní API.

3. Volání skriptu tímto způsobem:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Je snadné a upravte skript, například k vytváření poznámek za poslední.

## <a name="next-steps"></a>Další postup

* [Vytváření pracovních položek](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [Automatizace pomocí Powershellu](../../azure-monitor/app/powershell.md)
