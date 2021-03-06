---
title: Řešení potíží s neočekávaná restartování virtuálních počítačů s připojenými virtuálními pevnými disky na virtuálních počítačích Azure | Dokumentace Microsoftu
description: Řešení potíží se neočekávaná restartování virtuálních počítačů.
keywords: SSH připojení se odmítlo, ssh chyba, azure ssh, připojení SSH se nezdařilo
services: virtual-machines
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 6273087e28be8b784168d5808918d04d0e4cf303
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748149"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Řešení potíží s neočekávaná restartování virtuálních počítačů s připojenými virtuálními pevnými disky

Pokud virtuální počítač Azure (VM) má velký počet připojenými virtuálními pevnými disky, které jsou ve stejném účtu úložiště, nesmí překročit cíle škálovatelnosti pro účet úložiště jednotlivých způsobí virtuální počítač neočekávaně restartuje. Zkontrolujte minutové metriky pro účet úložiště (**TotalRequests**/**TotalIngress**/**TotalEgress**), které překračují špiček cíle škálovatelnosti účtu úložiště. Zobrazit [metrika ukazuje zvýšení u PercentThrottlingError](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) pro pomoc při určování, zda má došlo k omezování na vašem účtu úložiště.

Obecně platí, každé jednotlivé vstupní nebo výstupní operace na virtuální pevný disk z virtuálního počítače se přeloží na **získat stránku** nebo **umístit stránku** operace na základní objekt blob stránky. Proto můžete použít odhadované vstupně-výstupních operací pro vaše prostředí vyladit kolik virtuálních pevných disků můžete mít jeden účet úložiště na základě konkrétní chování vaší aplikace. Společnost Microsoft doporučuje s 40 nebo menší počet disků na jeden účet úložiště. Zobrazit [Azure Storage škálovatelnost a cíle výkonnosti](../../storage/common/storage-scalability-targets.md) podrobnosti o cíle škálovatelnosti pro účty úložiště, zejména celkovou frekvenci požadavků a celková šířka pásma pro typ účtu úložiště používáte.

Pokud jsou překročení cíle škálovatelnosti účtu úložiště, umístěte virtuální pevné disky ve více účtech úložiště ke snížení aktivity v jednotlivé účty.
