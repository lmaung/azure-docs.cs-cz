---
title: Nasazení a správa topologií Apache Storm v Azure HDInsight
description: Zjistěte, jak nasadit, monitorovat a správa topologií Apache Storm pomocí řídicího panelu Storm v HDInsight se systémem Windows. Použití nástrojů Hadoopu pro Visual Studio.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 03/01/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 322c7164c0ecda550bf1bfe6a55075759bf95735
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630512"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Nasazení a správa topologií Apache Storm v HDInsight se systémem Windows

[Apache Storm](https://storm.apache.org/) řídicí panel umožňuje snadno nasadit a spustit topologií Apache Storm pro HDInsight, vaše clusteru pomocí webového prohlížeče. Řídicí panel můžete také monitorovat a spravovat spuštěné topologie. Pokud používáte sadu Visual Studio, nástroje HDInsight pro Visual Studio poskytuje podobné funkce v sadě Visual Studio.

Řídicí panel Storm a funkce Storm v HDInsight Tools využívají Storm REST API, které je možné vytvořit vlastní, monitorování a řešení pro správu.

> [!IMPORTANT]  
> Kroky v tomto dokumentu vyžadují Storm na clusteru HDInsight, používající jako operační systém Windows. HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Informace o nasazení a správa topologií Storm pomocí clusteru služby HDInsight, který používá Linux, najdete v tématu [nasazení a správa topologií Apache Storm v HDInsight se systémem Linux](apache-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Požadavky

* **Apache Storm v HDInsight** – viz [Začínáme s Apache Storm v HDInsight](apache-storm-tutorial-get-started-linux.md) pokyny týkající se vytvoření clusteru.

* Pro **řídicí panel Storm**: Moderní webový prohlížeč, který podporuje HTML5.

* Pro **sady Visual Studio** – sada Azure SDK 2.5.1 nebo novější a HDInsight Tools for Visual Studio. Zobrazit [začněte používat nástroje HDInsight pro Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md) k instalaci a konfiguraci nástroje HDInsight pro Visual Studio.

    Jeden z následujících verzí sady Visual Studio:

  * Visual Studio 2012 s aktualizací Update 4

  * Visual Studio 2013 with Update 4 nebo Visual Studio 2013 Community

  * Visual Studio 2015 (libovolná edice)

  * Visual Studio 2017 (libovolná edice)

## <a name="storm-dashboard"></a>Řídicí panel Storm

Řídicí panel Storm je k dispozici ve vašem clusteru Storm webová stránka. Adresa URL je **https://&lt;Název_clusteru >.azurehdinsight.net/**, kde **clustername** je název vašeho cluster Storm v HDInsight.

V horní části řídicího panelu Storm, vyberte **odeslat topologii**. Postupujte podle pokynů na stránce ke spuštění ukázky topologie nebo odeslat a spustit topologii, kterou jste vytvořili.

![stránku odeslat topologii][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm UI

Řídicí panel Storm, vyberte **uživatelské rozhraní Storm** odkaz. Zobrazí se informace o clusteru, kromě všechny spuštěné topologie.

![uživatelské rozhraní storm][storm-dashboard-ui]

> [!NOTE]  
> V některých verzích aplikace Internet Explorer je zjistit, že uživatelské rozhraní Storm neaktualizuje po jste ji nejprve nenavštívil. Například se nemusí zobrazit nové topologie jste odeslali, nebo měl dříve deaktivuje, se může zobrazit topologie jako aktivní. Microsoft si tohoto problému vědomi a pracuje na řešení.

#### <a name="main-page"></a>Hlavní stránka

Hlavní stránka uživatelského rozhraní Storm poskytuje následující informace:

* **Souhrn clusteru**: Základní informace o clusteru Storm.

* **Souhrn topologie**: Seznam spuštěných topologií. Pomocí odkazů v této části můžete zobrazit další informace o konkrétních topologií.

* **Dohledové Souhrn**: Informace o vedoucí Storm.

* **Konfigurace nimbus**: Nimbus konfiguraci clusteru.

#### <a name="topology-summary"></a>Souhrn topologie

Vyberte odkaz z **souhrn topologie** části zobrazí následující informace o topologii:

* **Souhrn topologie**: Základní informace o topologii.

* **Akce topologie**: Akce správy, které můžete provést pro topologie.

  * **Aktivovat**: Obnoví zpracování deaktivované topologie.

  * **Deaktivovat**: Pozastaví spuštěné topologie.

  * **Obnovit rovnováhu**: Upraví paralelismus topologii. Po změně počtu uzlů v clusteru musíte znovu vyvážit spuštěné topologie. To umožňuje topologii upravovat paralelismus za účelem kompenzace zvýšení nebo snížení počtu uzlů v clusteru.

      Další informace najdete v tématu [pochopení paralelismu topologie Apache Storm](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **Ukončit**: Ukončí topologii Storm po zadaný časový limit.

* **Statistiky topologie**: Statistika týkající se topologie. Pomocí odkazů ve **okno** sloupce nastavit časový rámec pro zbývající položky na stránce.

* **Spouts**: Spoutů používat topologii. Chcete-li zobrazit další informace o konkrétních funkcích spouts pomocí odkazů v této části.

* **Bolts**: Bolty používat topologii. Pomocí odkazů v této části můžete zobrazit další informace o konkrétních funkcích bolts.

* **Topologie konfigurace**: Konfigurace vybraného topologie.

#### <a name="spout-and-bolt-summary"></a>Spout a Bolt souhrn

Výběr spout z **Spouts** nebo **Bolts** části zobrazí následující informace o vybrané položce:

* **Souhrn komponenty**: Základní informace o funkcích spout nebo bolt.

* **Funkcí spout/Bolt statistiky**: Statistické údaje o funkcích spout nebo bolt. Pomocí odkazů ve **okno** sloupce nastavit časový rámec pro zbývající položky na stránce.

* **Statistiky vstupu** (pouze funkce bolt): Informace o vstupní datové proudy využívaná funkcí bolt.

* **Statististiky výstupu**: Informace o datových proudů, protože ho vygeneroval tento funkcích spout nebo bolt.

* **Prováděcí moduly**: Informace o instancích sady funkcích spout nebo bolt. Vyberte **Port** vytvořený záznam pro konkrétní prováděcí modul zobrazení protokolu diagnostických informací pro tuto instanci.

* **Chyby**: Veškeré informace o chybě pro tento funkcích spout nebo bolt.

## <a name="hdinsight-tools-for-visual-studio"></a>Nástroje HDInsight pro Visual Studio

[Nástroje HDInsight](https://azure.microsoft.com/resources/videos/hdinsight-tools-for-visual-studio/) slouží k odeslání C# nebo hybridní topologie pro váš cluster Storm. Následující kroky používají ukázková aplikace. Informace o vytváření vlastních topologií pomocí nástrojů HDInsight najdete v tématu [vývoj topologií C# pomocí nástrojů HDInsight pro Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

Nasadit ukázku Storm v clusteru HDInsight, pak zobrazení a Správa topologie pomocí následujících kroků.

1. Pokud jste ještě nenainstalovali nejnovější verzi nástrojů HDInsight pro Visual Studio, přečtěte si téma [začněte používat nástroje HDInsight pro Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Otevřít Visual Studio, vyberte **souboru** > **nový** > **projektu**.

3. V **nový projekt** dialogového okna rozbalte **nainstalováno** > **šablony**a pak vyberte **HDInsight**. V seznamu šablon vyberte **Storm ukázka**. V dolní části dialogového okna zadejte název aplikace.

    ![image](./media/apache-storm-deploy-monitor-topology/sample.png)

4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a vyberte **odeslat do Storm v HDInsight**.

   > [!NOTE]  
   > Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure. Pokud máte více než jedno předplatné, přihlaste se na ten, který obsahuje váš cluster Storm v HDInsight.

5. Vyberte váš cluster Storm v HDInsight z **Storm Cluster** rozevíracího seznamu a pak vyberte **odeslat**. Můžete sledovat, jestli je pomocí úspěšného odeslání **výstup** okna.

6. Při úspěšném odeslání topologie **topologií Storm** pro cluster by se měla objevit. Vyberte ze seznamu k zobrazení informací o topologii spuštěné topologie.

    ![monitorování sady Visual studio](./media/apache-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]  
   > Můžete také zobrazit **topologií Storm** z **Průzkumníka serveru** tak, že rozbalíte **Azure** > **HDInsight**a pak pravým tlačítkem cluster Storm v HDInsight a výběrem **zobrazit topologie Storm**.

    Vyberte tvar spoutů a boltů zobrazíte informace o těchto součástí. Otevře se nové okno pro každé vybrané položky.

   > [!NOTE]  
   > Název topologie je název třídy topologie (v tomto případě `HelloWord`,) s časovým razítkem připojí.

7. Z **souhrn topologie** zobrazit, vyberte možnost **Kill** zastavení topologie.

   > [!NOTE]  
   > Topologie Storm pokračovat, spuštění, dokud se zastaví nebo odstranění clusteru.


## <a name="rest-api"></a>REST API

Uživatelské rozhraní Storm je postavený na rozhraní REST API, takže můžete provádět podobné správy a monitorování funkce pomocí rozhraní REST API. Rozhraní REST API můžete použít k vytvoření vlastních nástrojů pro správu a monitorování topologií Storm.

Další informace najdete v tématu [Apache Storm uživatelského rozhraní REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). Tyto informace je specifická pro Apache Storm v HDInsight pomocí rozhraní REST API.

### <a name="base-uri"></a>Základní identifikátor URI

Základní identifikátor URI pro rozhraní REST API v clusterech HDInsight je **https://&lt;Název_clusteru >.azurehdinsight.net/stormui/api/v1/**, kde **clustername** je název Storm v HDInsight cluster.

### <a name="authentication"></a>Authentication

Požadavky rozhraní REST API musí používat **základní ověřování**, takže se pomocí jména správce clusteru HDInsight a heslo.

> [!NOTE]  
> Protože odesílají základní ověřování pomocí prostého textu, měli byste **vždy** zabezpečená komunikace s využitím clusteru pomocí protokolu HTTPS.

### <a name="return-values"></a>Vrácené hodnoty

Použitelné z v rámci clusteru nebo virtuálním počítačům ve stejné virtuální síti Azure jako cluster může být pouze informace vrácené z rozhraní REST API. Například plně kvalifikovaný název domény (FQDN) vrátil pro [Apache ZooKeeper](https://zookeeper.apache.org/) servery nejsou přístupné z Internetu.

## <a name="next-steps"></a>Další kroky

Teď, když jste zjistili, jak nasazení a monitorování topologií pomocí řídicího panelu Storm, zjistěte, jak:

* [Vývoj topologií C# pomocí nástrojů HDInsight pro Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)

* [Vývoj topologií založených na Javě pomocí nástroje Apache Maven](apache-storm-develop-java-topology.md)

Seznam Další příklad topologie najdete v tématu [příklad topologií pro Apache Storm v HDInsight](apache-storm-example-topology.md).

[hdinsight-dashboard]: ./media/apache-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/apache-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/apache-storm-deploy-monitor-topology/storm-ui-summary.png
