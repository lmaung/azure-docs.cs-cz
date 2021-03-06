---
title: Informace o službě Mobility pro zotavení po havárii virtuálních počítačů VMware a fyzických serverů s Azure Site Recovery | Dokumentace Microsoftu
description: Další informace o agenta služby Mobility pro zotavení po havárii virtuálních počítačů VMware a fyzických serverů do Azure pomocí služby Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: ramamill
ms.openlocfilehash: d8b009d47a7fd0057c71ff3fc120a4443fc262d7
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593654"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Informace o službě Mobility pro virtuální počítače VMware a fyzické servery

Při nastavení zotavení po havárii pro virtuální počítače VMware a fyzických serverů požíváním [Azure Site Recovery](site-recovery-overview.md), instalace služby Site Recovery Mobility na každý místní virtuální počítač VMware a fyzických serverů.  Služba Mobility zaznamenává datové zápisy na počítači a předává je na procesní server Site Recovery. Můžete nasadit službu Mobility pomocí následujících metod:

[Nabízená instalace](vmware-azure-install-mobility-service.md): Konfigurace k provedení nabízené instalace služby Mobility Site Recovery: K tomuto účelu, při nastavování zotavení po havárii můžete také nastavit účet, který procesový server Site Recovery můžete použít pro přístup k virtuální počítač nebo fyzický server za účelem instalace služby.
[Ruční instalace](vmware-physical-mobility-service-install-manual.md): Můžete nainstalovat službu Mobility ručně na každém počítači pomocí uživatelského rozhraní nebo příkazového řádku.
[Automatické nasazení](vmware-azure-mobility-install-configuration-mgr.md): Můžete automatizovat instalaci pomocí nástrojů pro nasazení softwaru jako je System Center Configuration Manager.

## <a name="azure-virtual-machine-agent"></a>Agent virtuálního počítače Azure

- **Virtuální počítače s Windows**: Oproti 9.7.0.0 verzi služby Mobility [agenta virtuálního počítače Azure](../virtual-machines/extensions/features-windows.md#azure-vm-agent) nainstalovaného pomocí Instalační program služby Mobility. Tím se zajistí, že pokud je počítač převezme služby při selhání do Azure, virtuálního počítače Azure splňuje pro všechna rozšíření virtuálních počítačů pomocí požadovaných součástí instalace agenta.
- **Virtuální počítače s Linuxem**: [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) musí ručně doinstalovat na virtuálním počítači Azure po převzetí služeb při selhání.

## <a name="installer-files"></a>Soubory Instalační služby systému

Tabulka shrnuje soubory instalačního programu pro každý virtuální počítač VMware a fyzických serverů operačního systému. Můžete zkontrolovat [podporované operační systémy](vmware-physical-azure-support-matrix.md#replicated-machines) před zahájením.


**Soubor Instalační služby systému** | **Operační systém (pouze 64bitové verze)** 
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2016; Windows Server 2012 R2; Windows Server 2012; Windows Server 2008 R2 SP1 
Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.* 
Microsoft-ASR\_UA\*SLES12-64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1, SP2 SP3 
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Ubuntu Linux 16.04 LTS server
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8

## <a name="anti-virus-on-replicated-machines"></a>Antivirový program replikované počítače

Pokud chcete replikovat počítače aktivní antivirový software spuštěný, ujistěte se, vyloučit složky instalace služby Mobility z operace antivirový program (*C:\ProgramData\ASR\agent*). Tím se zajistí, že replikace pracuje podle očekávání.

## <a name="update-mobility-service-from-azure-portal"></a>Aktualizace služby mobility z webu Azure portal

1. Před zahájením zkontrolujte, že konfigurační server, horizontální navýšení kapacity procesových serverů a všechny hlavní cílové servery, které jsou součástí vašeho nasazení jsou aktualizované před aktualizací služby Mobility na chráněné počítače.
2. Na portálu otevřete trezor > **replikované položky**.
3. Pokud je konfigurační server na nejnovější verzi, se zobrazí oznámení, který čte "je k dispozici nová aktualizace agenta replikace Site recovery. Klikněte na tlačítko nainstalovat."

     ![Okno replikované položky](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Klikněte na oznámení a v **aktualizace agenta**, vyberte počítače, na kterých chcete provést upgrade služby Mobility. Pak klikněte na **OK**.

     ![Replikované položky seznamu virtuálních počítačů](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. Úloha aktualizace služby Mobility se spustí pro všechny vybrané počítače.

## <a name="update-mobility-service-through-powershell-script-on-windows-server"></a>Aktualizace služby Mobility pomocí skriptu prostředí powershell na Windows serveru

Pomocí následujícího skriptu pro upgrade služby mobility na serveru pomocí rutiny power shell

```azurepowershell
Update-AzureRmRecoveryServicesAsrMobilityService -ReplicationProtectedItem $rpi -Account $fabric.fabricSpecificDetails.RunAsAccounts[0]
```

## <a name="update-the-account-used-for-push-installation-of-the-mobility-service"></a>Aktualizovat účet použitý pro nabízenou instalaci služby Mobility

Při nasazení Site Recovery, aby nabízená instalace služby Mobility, zadat účet, který použije procesový server Site Recovery pro přístup k počítačům a instalaci služby po povolení replikace pro počítač. Pokud chcete aktualizovat přihlašovací údaje pro tento účet, postupujte podle [tyto pokyny](vmware-azure-manage-configuration-server.md).

## <a name="uninstall-the-mobility-service"></a>Odinstalujte službu Mobility

### <a name="on-a-windows-machine"></a>Na počítači s Windows

Odinstalujte z uživatelského rozhraní nebo z příkazového řádku.

- **V uživatelském rozhraní**: V ovládacím panelu počítače vyberte **programy**. Vyberte **Microsoft Azure Site Recovery Mobility Service/hlavní cílový server** > **odinstalovat**.
- **Z příkazového řádku**: Otevřete okno příkazového řádku jako správce na počítači. Spusťte následující příkaz: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="on-a-linux-machine"></a>Na počítači s Linuxem
1. Na počítači s Linuxem, přihlaste se jako **kořenové** uživatele.
2. V terminálu přejděte do /user/local/ASR.
3. Spusťte následující příkaz:

    ```
    uninstall.sh -Y
    ```

## <a name="next-steps"></a>Další postup

[Nastavit nabízená instalace služby Mobility](vmware-azure-install-mobility-service.md).
