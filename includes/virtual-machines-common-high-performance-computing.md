---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-machines-linux, virtual-machines-windows
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 01/15/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a1a6b31c1500c51dbbc32683d9e0e911b60dcae4
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246790"
---
Organizace mají rozsáhlých výpočetních potřeb. Tyto úlohy Big Compute zahrnují technického návrhu a analýzy, výpočty finančních rizik, vykreslování obrázků, komplexního modelování, simulace typu Monte Carlo a další. 

Použití cloudu Azure pro efektivní spuštění úlohy náročné na výpočetní prostředky se systémy Linux a Windows z paralelní dávkové úlohy pro tradiční simulace prostředí HPC. Spustit vaše prostředí HPC a úlohy služby batch na infrastrukturu Azure, s libovolným výpočetní služby, vedoucí mřížky, řešení na Marketplace i dodavatele aplikací (SaaS). Azure nabízí flexibilní řešení pro distribuci práce a škálovat na tisíce virtuálních počítačů nebo jader a potom vertikálně snížit kapacitu, když budete potřebovat méně prostředků. 

## <a name="solution-options"></a>Možnosti řešení

* **Sestavili řešení**
    * Nastavte cluster prostředí ve službě Azure virtual machines nebo [škálovací sady virtuálních počítačů](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Zvedněte a shift místní cluster nebo nasadí nový cluster v Azure další zvýšení kapacity. 
    * Použití šablon Azure Resource Manageru k nasazení řešení [Správci úloh](#workload-managers), infrastruktury, a [aplikací](#hpc-applications). 
    * Zvolte [velikosti prostředí HPC a virtuálního počítače s GPU](#hpc-and-gpu-vm-sizes) , které zahrnují specializovaný hardware a připojení k síti pro úlohy MPI nebo GPU. 
    * Přidat [vysoce výkonné úložiště](#hpc-storage) pro můžu intenzivních vstupně-výstupních operací.
* **Hybridní řešení**
    * Rozšířit vaše místní řešení pro přesměrování zpracování úloh ve špičce ("rozšíření") do infrastruktury Azure
    * Cloudové výpočetní prostředky na vyžádání pomocí stávajících [Správce úloh](#workload-managers).
    * Využijte výhod [velikosti prostředí HPC a virtuálního počítače s GPU](#hpc-and-gpu-vm-sizes) pro úlohy MPI nebo GPU.
* **Velký výpočetní řešení jako služba**
    * Vývoj vlastních řešení Big Compute a pracovní postupy pomocí [Azure CycleCloud](#azure-cyclecloud), [Azure Batch](#azure-batch)a související [služeb Azure](#related-azure-services).
    * Spouštění Azure umožnil inženýrství a simulace řešení od dodavatelů včetně [Altair](http://www.altair.com/), [měřítko](https://www.rescale.com/azure/), a [Cycle Computing](https://cyclecomputing.com/) (nyní [spojit s Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
    * Použití [Superpočítač cray vám umožní](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) jako služby hostované v Azure.
* **Řešení na Marketplace**
    * Použijte škálovací sady [aplikace HPC](#hpc-applications) a [řešení](#marketplace-solutions) nabízí [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 
    
Další informace o podpůrné technologie a odkazy na informace v následujících částech.

## <a name="marketplace-solutions"></a>Řešení na Marketplace

Přejděte [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) pro Image Linuxu a virtuálních počítačů Windows a řešení navržených pro prostředí HPC. Příklady obsahují:

* [RogueWave založené na CentOS HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview)
* [SUSE Linux Enterprise Server pro prostředí HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [TIBCO Grid Server Engine](https://azuremarketplace.microsoft.com/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Azure pro datové vědy pro Windows a Linux virtuálního počítače](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [D3View](https://azuremarketplace.microsoft.com/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)

## <a name="hpc-applications"></a>Aplikace HPC

Spouštějte vlastní nebo obchodní aplikace HPC v Azure. Několik příkladů v této části jsou benchmarked výpočetních jader a efektivní škálování s další virtuální počítače. Přejděte [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace) připravených k nasazení řešení.

> [!NOTE]
> Obraťte se na dodavatele komerčních aplikací pro licencování a dalších omezení při spouštění v cloudu. Ne všichni dodavatelé nabízejí licencování formou průběžných plateb. Může být nutné licenční server v cloudu pro vaše řešení nebo připojení k serveru na místní licence.

### <a name="engineering-applications"></a>Vytváření aplikací

* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB distribuované výpočetní Server](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM+](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

### <a name="graphics-and-rendering"></a>Grafika a vykreslování

* [Vykreslovací aplikace](../articles/batch/batch-rendering-service.md) v Azure Batch, jako jsou Autodesk Maya, 3ds Max a Arnold a Chaos Group V-Ray blenderu

### <a name="ai-and-deep-learning"></a>AI a hloubkového učení

* [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Hloubkové učení virtuálního počítače](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Batch loděnice receptů obsáhlého learningu](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)

## <a name="hpc-and-gpu-vm-sizes"></a>Prostředí HPC a virtuálního počítače s GPU

Azure nabízí širokou škálu velikostí pro [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuálních počítačů, včetně velikosti pro úlohy náročné na výpočetní prostředky. H16r a H16mr virtuálních počítačů může například připojení k síti RDMA back-end vysokou propustnost. Tato síť v cloudu můžete zvýšit výkon vysoce provázané paralelní aplikace spuštěné v [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) nebo Intel MPI. 

Funkce virtuálních počítačů řady N-series je určená pro aplikace náročné na výpočetní nebo náročné na grafiku včetně learning umělé inteligence (AI) a vizualizace grafických procesorů NVIDIA. 

Další informace:

* Vysokovýkonné výpočetní velikosti [Linux](../articles/virtual-machines/linux/sizes-hpc.md) a [Windows](../articles/virtual-machines/windows/sizes-hpc.md) virtuálních počítačů 
* S podporou grafického procesoru velikosti [Linux](../articles/virtual-machines/linux/sizes-gpu.md) a [Windows](../articles/virtual-machines/windows/sizes-gpu.md) virtuálních počítačů 

## <a name="azure-cyclecloud"></a>Azure CycleCloud

Při vytváření a optimalizaci HPC clusterů na virtuálních počítačích Azure s efektivně spravovat běžné úlohy s lehkostí a elegancí [Azure CycleCloud](https://docs.microsoft.com/azure/cyclecloud/overview).

Naučte se:

* [Instalace a nastavení CycleCloud pomocí šablony Resource Manageru](https://docs.microsoft.com/azure/cyclecloud/quickstart-install-cyclecloud)

* [Nastavit CycleCloud ručně](https://docs.microsoft.com/azure/cyclecloud/installation)

## <a name="azure-batch"></a>Azure Batch

[Batch](../articles/batch/batch-technical-overview.md) je platformu, služby pro spouštění rozsáhlých paralelních a vysoce výkonné výpočetní (prostředí HPC) aplikace účinně v cloudu. Azure Batch plánuje výpočetně náročné práce ke spuštění v rámci spravovaného fondu virtuálních počítačů a dokáže automaticky škálovat výpočetní prostředky pro splnění požadavků vašich úloh. 

Poskytovatelé SaaS nebo vývojáři můžou pomocí sady Batch SDK a nástroje pro integraci aplikací HPC nebo úloh kontejneru v Azure, připravit data pro Azure a sestavit kanály spouštění úloh. 

Naučte se:

* [Začněte s vývojem pomocí služby Batch](../articles/batch/quick-run-dotnet.md)
* [Použití ukázek kódu služby Azure Batch](https://github.com/Azure/azure-batch-samples)
* [Použití virtuálních počítačů s nízkou prioritou pomocí služby Batch](../articles/batch/batch-low-pri-vms.md)
* [Spuštění kontejnerizované úlohy HPC s loděnice služby Batch](https://github.com/Azure/batch-shipyard)
* [Spuštění paralelní úlohy v jazyce R na Batch](https://github.com/Azure/doAzureParallel)
* [Spuštění úlohy Spark na vyžádání na Batch](https://github.com/Azure/aztk)

## <a name="workload-managers"></a>Správce úloh

Následují příklady clusteru a úlohy správce, které můžou běžet v infrastruktuře Azure. Vytvoření samostatné clustery ve virtuálních počítačích Azure nebo rozšíření do virtuálních počítačů Azure z místního clusteru. 
* [Výpočetní Alces let](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Jasně Správce clusteru](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM Spectrum Symphony a Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [Microsoft HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) 

## <a name="hpc-storage"></a>Úložiště HPC

Úlohy ve velkém měřítku Batch a prostředí HPC mají vztahujících se na úložiště dat a přístupu, které přesahují možnosti tradičních cloudové systémy souborů. 

Další informace:

* [Avere vFXT pro Azure](../articles/avere-vfxt/avere-vfxt-overview.md) 
* [Paralelní virtuálním souborovém systému v Azure](https://azure.microsoft.com/resources/parallel-virtual-file-systems-on-microsoft-azure/) včetně [Lustre](http://lustre.org/) a [BeeGFS](http://www.beegfs.com/content/).

## <a name="related-azure-services"></a>Související služby Azure

Virtuální počítače Azure, škálovací sady virtuálních počítačů, Batch a související výpočetních služeb jsou základem pro většinu řešení Azure HPC. Řešení však můžete využít výhod mnoha souvisejících služeb Azure. Tady je částečný seznam:

### <a name="storage"></a>Storage

* [Objekt BLOB, table a queue storage](../articles/storage/storage-introduction.md)
* [Úložiště souborů](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Data a analýza

* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md)
* [Data Factory](../articles/data-factory/introduction.md)
* [Data Lake Storage Gen1](../articles/data-lake-store/data-lake-store-overview.md)
* [Databricks](../articles/azure-databricks/what-is-azure-databricks.md)
* [SQL Database](../articles/sql-database/sql-database-technical-overview.md)

### <a name="ai-and-machine-learning"></a>AI a strojové učení

* [Služba Machine Learning](../articles/machine-learning/service/overview-what-is-azure-ml.md)
* [Genomics](../articles/genomics/overview-what-is-genomics.md)

### <a name="networking"></a>Sítě

* [Virtual Network](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Containers

* [Azure Kubernetes Service (AKS)](../articles/aks/intro-kubernetes.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)
* [Container Instances](../articles/container-instances/container-instances-overview.md)

## <a name="customer-stories"></a>Příběhy zákazníků

Příklady zákazníků, které se mají vyřešit obchodní problémy s využitím řešení Azure HPC:

* [SPOLEČNOSTI ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA Global P&C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [D3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [EFS](https://customers.microsoft.com/story/efs-professionalservices-azure)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://customers.microsoft.com/story/metlife-insurance-azure-cloud-services-windows-server-analytics-platform-server-en)
* [Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ Securities International](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [NeuroInitiative](https://customers.microsoft.com/story/neuroinitiative-health-provider-azure)
* [Schlumberger](https://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)

## <a name="next-steps"></a>Další postup

* Další informace o řešení Azure pro [vysokovýkonného výpočetního prostředí](https://azure.microsoft.com/solutions/high-performance-computing/), [vykreslování](https://azure.microsoft.com/solutions/big-compute/rendering/), [bankovnictví a kapitálové trhy](https://finance.azure.com/), a [genomics](https://enterprise.microsoft.com/industries/health/genomics/).

* Další informace o [architekturu řešení pro Big Compute](https://azure.microsoft.com/solutions/architecture/?solution=high-performance-computing) v Azure.
