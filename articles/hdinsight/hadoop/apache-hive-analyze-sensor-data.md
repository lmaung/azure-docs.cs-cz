---
title: Analýza dat senzoru pomocí Apache Hivu a Apache Hadoop – Azure HDInsight
description: Zjistěte, jak analyzovat data ze senzorů pomocí dotazu konzoly Apache Hive s HDInsight (Hadoop) a pak data vizualizovat v Microsoft Excel pomocí PowerView.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 04/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: b9c8f1af612c220534e45c5c66651f0ad8600826
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628182"
---
# <a name="analyze-sensor-data-using-the-apache-hive-query-console-on-apache-hadoop-in-hdinsight"></a>Analýza dat senzoru pomocí Apache Hive konzoly pro dotazy na Apache Hadoop v HDInsight

Zjistěte, jak analyzovat data ze senzorů pomocí dotazu konzoly Apache Hive s HDInsight (Apache Hadoop) a pak data vizualizovat v Microsoft Excelu pomocí Power View.

> [!IMPORTANT]  
> Kroky v tomto dokumentu fungovat jenom s clustery HDInsight se systémem Windows. HDInsight je dostupná jenom ve Windows pro verze nižší než HDInsight 3.4. HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).


V této ukázce použijete Hive ke zpracování historických dat a identifikovat problémy se zahřívání a klimatizaci systémy. Konkrétně identifikovat systémy nejsou schopni spolehlivě udržovat nastavenou teplotu provedením následujících úloh:

* Vytváření tabulek HIVE k dotazování dat. uloženy v souborech s hodnotami oddělenými čárkami (CSV).
* Vytváření dotazů HIVU analyzovat data.
* Aby se načetla data analyzovat, aplikaci Microsoft Excel pro připojení k HDInsight.
* K vizualizaci dat pomocí Power View.

![Diagram architektury řešení](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Požadavky

* Cluster HDInsight (Hadoop): Zobrazit [vytvořit Apache Hadoop clusterů v HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) informace o vytváření clusteru.
* Microsoft Excel 2013

  > [!NOTE]  
  > Aplikace Microsoft Excel se používá pro vizualizace dat s využitím [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Ovladače ODBC Microsoft Hivu](https://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Ke spuštění ukázky

1. Z webového prohlížeče přejděte na následující adresu URL: 

         https://<clustername>.azurehdinsight.net

    Parametr `<clustername>` nahraďte názvem vašeho clusteru HDInsight.

    Po zobrazení výzvy ověřování pomocí uživatelského jména správce a heslo, které jste použili při vytváření tohoto clusteru.

2. Z webové stránky, které se otevře, klikněte na tlačítko **Galerie spuštěno získání** kartu a potom v části **řešení s ukázkovými daty** kategorii, klikněte na tlačítko **analýza dat snímačů** vzorku.

    ![Získat image z Galerie Začínáme](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Postupujte podle pokynů k dispozici na webové stránce a dokončete ukázku.
