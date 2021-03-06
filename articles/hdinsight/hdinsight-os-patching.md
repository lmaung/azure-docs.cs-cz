---
title: Konfigurovat plán pro clustery založené na Linuxu HDInsight – Azure oprav operačního systému
description: Zjistěte, jak nakonfigurovat plán pro clustery HDInsight založené na Linuxu oprav operačního systému.
services: hdinsight
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: ef57608d092c05b30be63a54bb41ba87558eabc3
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694614"
---
# <a name="os-patching-for-hdinsight"></a>Opravy operačního systému pro HDInsight 

> [!IMPORTANT]
> K dispozici pro vytvoření nového clusteru HDInsight 3 měsících publikování Image Ubuntu. Od ledna 2019 spuštěné clustery jsou **není** automaticky opravit. Zákazníkům musíte použít akce skriptů nebo jiných mechanismů o opravu spuštěný cluster. Nově vytvořený clustery bude mít vždy nejnovější dostupné aktualizace, včetně nejnovější opravy zabezpečení.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Jak nakonfigurovat plán pro clustery HDInsight založené na Linuxu oprav operačního systému
Virtuální počítače v clusteru služby HDInsight je potřeba restartovat příležitostně tak, že je možné nainstalovat důležité opravy. 

Pomocí skriptových akcí popsaných v tomto článku, můžete upravit následujícím způsobem plán oprav operačního systému:
1. Povolení nebo zakázání automatického restartování počítače
2. Nastavení četnosti restartuje (ve dnech mezi jednotlivými restartováními)
3. Nastavte dne v týdnu, kdy dojde k restartování

> [!NOTE]  
> Tuto akci se skripty budou fungovat jenom s clustery HDInsight založené na Linuxu vytvořená po 1. srpna 2016. Opravy bude platit pouze v případě, že virtuální počítače se restartují. 

## <a name="how-to-use-the-script"></a>Jak používat skript 

Při použití tento skript vyžaduje následující informace:
1. Umístění skriptu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight používá tento identifikátor URI k vyhledání a spuštění skriptu na všech virtuálních počítačů v clusteru.
  
2. Typy uzlů clusteru, které skript platí pro: hlavního uzlu, workernode, zookeeper. Tento skript se musí použít na všechny typy uzlů v clusteru. Pokud není použité u typu uzlu, virtuálních počítačů pro daný typ uzlu bude nadále používat předchozí plán oprav.


3.  Parametr: Tento skript je možné zadat tři číselné parametry:

    | Parametr | Definice |
    | --- | --- |
    | Povolit nebo zakázat automatické restartování počítače |0 nebo 1. Hodnota 0 zakáže automatického restartování počítače během 1 povolí automatické restartování počítače. |
    | Frekvence |7 až 90 (včetně). Počet dní čekání před restartem systému virtuálních počítačů pro opravy, které vyžadují restartování. |
    | Den v týdnu |1 až 7 (včetně). Hodnota 1 značí restartování počítače se budou objevovat v pondělí a 7 znamená Sunday.For příklad, pomocí parametrů 1 60 2 výsledky v automaticky restartuje každých 60 dní (maximální) úterý. |
    | Trvalost |Při použití akce skriptu do existujícího clusteru, můžete skript označit jako trvalý. Trvalých skriptů se použijí, když se přidají nové workernodes ke clusteru prostřednictvím operace škálování. |

> [!NOTE]  
> Tento skript je třeba označit jako trvalý, při použití do existujícího clusteru. V opačném případě žádné nové uzly, které jsou vytvořené prostřednictvím operací škálování bude používat výchozí plán oprav.  Pokud použijete skript jako součást procesu vytváření clusteru, se ukládají automaticky.

## <a name="next-steps"></a>Další postup

Konkrétní kroky pomocí akce skriptu, najdete v následujících částech [HDInsight založených na Linuxu přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md):

* [Použití akce skriptu při vytváření clusteru](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Použít akci skriptu spuštěného clusteru](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
