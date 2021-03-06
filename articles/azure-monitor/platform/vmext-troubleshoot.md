---
title: Řešení potíží s rozšíření virtuálního počítače Azure Log Analytics | Dokumentace Microsoftu
description: Popisují příznaky, příčiny a řešení většiny běžných potíží s rozšířením Log Analytics VM pro Windows a virtuální počítače Azure s Linuxem.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2018
ms.author: magoedte
ms.openlocfilehash: f2f50144e188a297468ac8ea8fc8891d2e4ec145
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106466"
---
# <a name="troubleshooting-the-log-analytics-vm-extension"></a>Řešení potíží s rozšíření virtuálního počítače Log Analytics
Tento článek poskytuje nápovědu řešení potíží s chybami setkat s rozšířením Log Analytics VM pro virtuální počítače Windows a Linuxu spuštěné na Microsoft Azure a navrhne řešení k jejich řešení.

Pokud chcete ověřit stav rozšíření, proveďte následující kroky na webu Azure Portal.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na webu Azure Portal klikněte na **Všechny služby**. V seznamu prostředků zadejte **virtuálních počítačů**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **virtuálních počítačů**.
3. V seznamu virtuálních počítačů vyhledejte a vyberte ji.
3. Na virtuálním počítači, klikněte na tlačítko **rozšíření**.
4. Ze seznamu zkontrolujte, jestli je povolené rozšíření Log Analytics, nebo ne.  Pro Linux, je agent uveden jako **OMSAgentforLinux** a pro Windows, je agent uveden jako **MicrosoftMonitoringAgent**.

   ![Zobrazení rozšíření virtuálního počítače](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Klikněte na rozšíření zobrazíte podrobnosti. 

   ![Podrobnosti o rozšíření virtuálního počítače](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Řešení potíží s rozšíření virtuálního počítače Windows Azure

Pokud *agenta Microsoft Monitoring Agent* není instalace rozšíření virtuálního počítače nebo vytváření sestav, můžete provést následující kroky k odstranění tohoto problému.

1. Zkontrolujte, jestli je nainstalovaný agent virtuálního počítače Azure a funguje správně pomocí kroků v [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Můžete také zkontrolovat soubor protokolu agenta virtuálního počítače `C:\WindowsAzure\logs\WaAppAgent.log`
   * Pokud v protokolu neexistuje, není nainstalován agent virtuálního počítače.
   * [Nainstalujte agenta virtuálního počítače Azure](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Potvrďte, že je spuštěn úkol prezenčního signálu rozšíření Microsoft Monitoring Agent pomocí následujících kroků:
   * Přihlaste se k virtuálnímu počítači
   * Otevřít okno služby Plánovač úloh a najděte `update_azureoperationalinsight_agent_heartbeat` úkolu
   * Potvrzení úlohy je povolená a běží každou minutu
   * Vrátit se změnami souboru protokolu prezenčního signálu `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Projděte si soubory protokolů rozšíření Microsoft Monitoring Agent virtuálního počítače v `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Ujistěte se, že virtuální počítač může spouštět skripty prostředí PowerShell
5. Ujistěte se, že oprávnění C:\Windows\temp nebyly změněny.
6. Zobrazení stavu agenta Microsoft Monitoring Agent pomocí následujícího příkazu v okně PowerShell se zvýšenými oprávněními na virtuálním počítači `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Projděte si soubory protokolů instalace agenta Microsoft Monitoring Agent v `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Další informace najdete v tématu [řešení potíží s rozšířeními Windows](../../virtual-machines/extensions/oms-windows.md).

## <a name="troubleshooting-linux-vm-extension"></a>Řešení potíží s rozšíření virtuálního počítače s Linuxem
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Pokud *agenta Log Analytics pro Linux* není instalace rozšíření virtuálního počítače nebo vytváření sestav, můžete provést následující kroky k odstranění tohoto problému.

1. Pokud je stav rozšíření *neznámý* zkontrolujte, jestli je nainstalovaný agent virtuálního počítače Azure a správně funguje, zobrazením souboru protokolu agenta virtuálního počítače `/var/log/waagent.log`
   * Pokud v protokolu neexistuje, není nainstalován agent virtuálního počítače.
   * [Nainstalujte agenta virtuálního počítače Azure na virtuálních počítačích s Linuxem](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Ostatní stavy není v pořádku, najdete v tématu agenta Log Analytics pro rozšíření virtuálního počítače s Linuxem soubory protokolů `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` a `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Pokud se stav extension byl v pořádku, ale není v průběhu nahrávání dat zkontrolujte agenta pro Linux soubory protokolu v Log Analytics `/var/opt/microsoft/omsagent/log/omsagent.log`

Další informace najdete v tématu [řešení potíží s rozšířeními Linux](../../virtual-machines/extensions/oms-linux.md).

## <a name="next-steps"></a>Další postup

Další pokyny k odstraňování souvisejících agenta Log Analytics pro Linux hostované na počítače mimo Azure, najdete v části [řešení potíží s agentem Azure Log Analytics Linux](agent-linux-troubleshoot.md).  
