---
title: Migrace na upozornění protokolu aktivit Azure výstrahy týkající se událostí správy
description: Výstrahy na události správy odebere 1. října. Připravte migraci existující výstrahy.
author: johnkemnetz
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/14/2017
ms.author: johnkem
ms.subservice: alerts
ms.openlocfilehash: 55d0269aaa330f928a9d037eec6a3445825a5ed3
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470337"
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Migrace na upozornění protokolu aktivit Azure výstrahy týkající se událostí správy


> [!WARNING]
> Výstrahy na události správy se vypne nebo po 1. října. Postupujte podle pokynů níže pochopit, pokud máte tyto výstrahy a jejich migraci, pokud tomu tak je.
>
> 

## <a name="what-is-changing"></a>Co se mění

Azure Monitor (dříve Azure Insights) nabízí možnost vytvořit výstrahu, která aktivuje z události správy a vygenerovat oznámení webhooku adresu URL nebo e-mailové adresy. Pravděpodobně jste si vytvořili jednu z těchto výstrah kterýmkoli z těchto způsobů:
* Na portálu Azure pro některé typy prostředků v části monitorování -> Výstrahy -> přidat oznámení, kde "Výstrah na" je nastaven na "Události"
* Spuštěním rutiny Add-AzureRmLogAlertRule Powershellu
* Přímo pomocí [REST API upozornění](https://docs.microsoft.com/rest/api/monitor/alertrules) s odata.type = "ManagementEventRuleCondition" a dataSource.odata.type = "RuleManagementEventDataSource"
 
Následující skript prostředí PowerShell vrátí seznam hodnot všechny výstrahy na události správy, které mají v vašeho předplatného, jakož i podmínky nastavené na jednotlivé výstrahy.

```powershell
Connect-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Pokud nemáte žádná upozornění na události správy, bude výstup výše uvedené rutiny Powershellu řadu upozornění podobný následujícímu:

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Tyto zprávy upozornění můžete ignorovat. Pokud máte výstrahy na události správy, výstup této rutiny prostředí PowerShell bude vypadat například takto:

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Každé upozornění jsou oddělené oddělovačem na přerušovanou čáru a podrobnosti zahrnují ID prostředku výstrahy a příslušné pravidlo monitoruje.

Tato funkce byla převedena do [upozornění protokolu aktivit Azure Monitor](../../azure-monitor/platform/activity-log-alerts.md). Tyto nové výstrahy umožňují nastavit podmínku pro události protokolu aktivit a při vytvoření nové události odpovídá podmínce, dostanete oznámení. Také nabízí několik vylepšení z výstrahy na události správy:
* Můžete znovu použít, vaše skupina příjemců oznámení ("akce") mezi výstrahami generovanými při použití [skupiny akcí](../../azure-monitor/platform/action-groups.md), omezil složitost změny, kdo by měl zobrazit upozornění.
* Upozornění můžete dostávat přímo na váš telefon SMS pomocí skupin akcí.
* Je možné [vytvoření upozornění protokolu aktivit se šablonami Resource Manageru](../../azure-monitor/platform/alerts-activity-log.md).
* Podmínky lze vytvořit s větší flexibilitou a složitosti a vyhovět vašim konkrétním potřebám.
* Oznámení se dodávají rychleji.
 
## <a name="how-to-migrate"></a>Jak migrovat
 
Chcete-li vytvořit novou aktivitu protokolu oznámení na, můžete buď:
* Postupujte podle [naší příručce o tom, jak vytvořit upozornění na webu Azure Portal](../../azure-monitor/platform/activity-log-alerts.md)
* Zjistěte, jak [vytvořte výstrahu pomocí šablony Resource Manageru](../../azure-monitor/platform/alerts-activity-log.md)
 
Výstrahy na události správy, které jste předtím vytvořili nebudou automaticky migrovat na upozornění protokolu aktivit. Budete muset použít předchozí skript prostředí PowerShell pro zobrazení seznamu výstrah událostí správy jste nakonfigurovali a ručně je znovu vytvořit jako upozornění protokolu aktivit. To je nutné provést před 1. října, po jejímž uplynutí výstrahy na události správy nebudou viditelné ve vašem předplatném Azure. Jiné typy upozornění v Azure, včetně upozornění metrik Azure monitoru, Application Insights výstrah a upozornění Log Analytics se tato změna nemá vliv. Pokud máte nějaké dotazy, publikuje do komentářů dole.


## <a name="next-steps"></a>Další postup

* Další informace o [protokolu aktivit](../../azure-monitor/platform/activity-logs-overview.md)
* Konfigurace [upozornění protokolu aktivit prostřednictvím webu Azure portal](../../azure-monitor/platform/activity-log-alerts.md)
* Konfigurace [upozornění protokolu aktivit prostřednictvím Resource Manageru](../../azure-monitor/platform/alerts-activity-log.md)
* Zkontrolujte [schéma webhooku v upozornění protokolu aktivit](../../azure-monitor/platform/activity-log-alerts-webhook.md)
* Další informace o [oznámení služby](../../azure-monitor/platform/service-notifications.md)
* Další informace o [skupiny akcí](../../azure-monitor/platform/action-groups.md)

