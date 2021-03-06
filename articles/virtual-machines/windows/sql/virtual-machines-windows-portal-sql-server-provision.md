---
title: Zřizování Příručka pro virtuální počítače s Windows SQL serverem na webu Azure Portal | Dokumentace Microsoftu
description: Tato příručka popisuje možnosti pro vytváření virtuálních počítačů s Windows a SQL serverem 2017 na webu Azure Portal.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 05/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: fd01fdd3f7f8803dc7221bd0bd6c993120a83d44
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2019
ms.locfileid: "54330887"
---
# <a name="how-to-provision-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Přidělení virtuálního počítače s Windows SQL serverem na webu Azure Portal

Tato příručka obsahuje podrobné informace o různých možnostech, které jsou k dispozici při vytváření virtuálního počítače s Windows serverem SQL na webu Azure Portal. V tomto článku najdete další možnosti konfigurace, než [rychlý start virtuálního počítače s SQL serverem](quickstart-sql-vm-create-portal.md), které jde další až jeden možný zřizování úloh. 

Tento průvodce vám vytvořit vlastní virtuální počítač s SQL serverem. Případně ho můžete použít jako referenci naleznete možnosti dostupné na webu Azure Portal.

> [!TIP]
> Pokud máte dotazy k virtuálním počítačům s SQL Serverem, přečtěte si [Nejčastější dotazy](virtual-machines-windows-sql-server-iaas-faq.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a id="select"></a> Image z Galerie virtuálních počítačů SQL serveru

Při vytváření virtuálního počítače s SQL serverem, vyberte jednu z několika předem nakonfigurované Image z Galerie virtuálních počítačů. Následující kroky ukazují, jak vybrat některou k imagí SQL serveru 2017.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí svého účtu.

1. Na webu Azure Portal klikněte na **Vytvořit prostředek**. Na Portálu se otevře okno **Nový**.

1. V okně **Nový** klikněte na **Compute** a pak klikněte na **Zobrazit všechno**.

   ![Okno Nová služba Compute](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

1. Do pole hledání zadejte **SQL Server 2017** a stiskněte ENTER.

1. Pak klikněte na ikonu **Filtr**.

1. V oknech Filtr zaškrtněte podkategorii **Založené na Windows** a jako vydavatele zaškrtněte **Microsoft**. Pak kliknutím na **Hotovo** vyfiltrujte z výsledků image Windows s SQL Serverem publikované Microsoftem.

   ![Okno Azure Virtual Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

1. Projděte si dostupné image SQL Serveru. U každé image je označena příslušná verze SQL Serveru a operační systém.

1. Vyberte image s názvem **bezplatná licence SQL serveru: SQL Server 2017 Developer ve Windows serveru 2016**.

   > [!TIP]
   > U verze Developer edition je použít v tomto návodu, protože je plně funkční, bezplatnou edici systému SQL Server pro vývoj testování. Platíte jenom náklady na provozování virtuálního počítače. Ale budete moci vybrat kteroukoli z Image na použití v tomto názorném postupu. Popis dostupných imagí najdete v tématu [SQL Server Windows Virtual Machines – přehled](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

   > [!TIP]
   > Náklady na licencování pro SQL Server jsou začleněny do ceny za sekundu virtuální počítač vytvořit a liší se podle edice a počet jader. SQL Server Developer edition je však zdarma pro vývoj a testování (ne produkci) a SQL Express je zdarma pro nenáročné úlohy (méně než 1 GB paměti, méně než 10 GB úložiště). Můžete také přinést si – vlastní licence (BYOL) a platíte jenom za virtuální počítač. Tyto názvy bitových kopií mají předponu {BYOL}. 
   >
   > Další informace o těchto možnostech najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

1. V části **Vybrat model nasazení** ověřte, že je vybraný **Resource Manager**. U nových virtuálních počítačů se doporučuje používat model nasazení Resource Manageru. 

1. Klikněte na možnost **Vytvořit**.

    ![Vytvoření virtuálního počítače s SQL Serverem pomocí Resource Manageru](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a id="configure"></a> Možnosti konfigurace
Pro konfiguraci virtuálního počítače s SQL Serverem se používá pět oken.

| Krok | Popis |
| --- | --- |
| **Základy** |[Konfigurace základního nastavení](#1-configure-basic-settings) |
| **Velikost** |[Volba velikosti virtuálního počítače](#2-choose-virtual-machine-size) |
| **Nastavení** |[Konfigurace volitelných funkcí](#3-configure-optional-features) |
| **Nastavení SQL Serveru** |[Konfigurace nastavení SQL Serveru](#4-configure-sql-server-settings) |
| **Souhrn** |[Kontrola souhrnných informací](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Konfigurace základního nastavení

V okně **Základy** zadejte následující informace:

* Zadejte jedinečný **název** virtuálního počítače.

* Vyberte **SSD** jako typ disku virtuálního počítače pro zajištění optimálního výkonu.

* Zadejte **uživatelské jméno** pro účet místního správce ve virtuálním počítači. Tento účet je také přidán do pevné role serveru na serveru **sysadmin** SQL Serveru.

* Zadejte silné **heslo**.

* Pokud máte více předplatných, ověřte, že je předplatné správné pro nový virtuální počítač.

* Do pole **Skupina prostředků** zadejte název pro novou skupinu prostředků. Pokud chcete použít existující skupinu prostředků, klikněte na **Použít existující**. Skupina prostředků je kolekce souvisejících prostředků v Azure (virtuální počítače, účty úložiště, virtuální sítě atd.).

  > [!NOTE]
  > Použití nové skupinu prostředků je užitečné, pokud testujete nasazení SQL Serveru v Azure nebo se snažíte o něm dozvědět více. Až s testováním skončíte, odstraňte skupinu prostředků. Automaticky se tím odstraní virtuální počítač se všemi prostředky spojenými s danou skupinu prostředků. Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../../../azure-resource-manager/resource-group-overview.md).

* Vyberte **umístění** pro oblast Azure pro toto nasazení hostovat.

* Kliknutím na **OK** uložte nastavení.

    ![Okno Základy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Volba velikosti virtuálního počítače

V kroku **Velikost** zvolte velikost virtuálního počítače v okně **Zvolit velikost**. V okně se po jeho otevření zobrazí doporučené velikosti počítačů na základě image, kterou jste vybrali.

> [!IMPORTANT]
> Odhadované měsíční náklady zobrazené v okně **Zvolit velikost** nezahrnují náklady na licencování SQL Serveru. Tento odhad jsou náklady pouze na virtuální počítač. Pro edice Express a Developer systému SQL Server je tento odhad o celkové odhadované náklady. Pro ostatní edice se podívejte na [stránku s cenami pro virtuální počítače s Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) a vyberte cílovou edici vašeho SQL Serveru. Podívejte se také na [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

![Možnosti velikosti virtuálního počítače s SQL Serverem](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Doporučené velikosti a konfiguraci počítačů pro produkční úlohy najdete v tématu [Osvědčené postupy z hlediska výkonu pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md).

> [!NOTE]
> Další informace o velikostech virtuálních počítačů najdete v tématu [Velikosti virtuálních počítačů](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zvolte velikost počítače a potom klikněte na **Vybrat**.

## <a name="3-configure-optional-features"></a>3. Konfigurace volitelných funkcí

V okně **Nastavení** nakonfigurujte úložiště, sítě a monitorování Azure pro virtuální počítač.

* V části **Úložiště** vyberte **Ano** u možnosti **Použít Managed Disks**.

   > [!NOTE]
   > Microsoft pro SQL Server doporučuje službu Managed Disks. Služba Managed Disks se stará o úložiště na pozadí. Navíc, pokud jsou virtuální počítače se službou Managed Disks ve stejné skupině dostupnosti, Azure distribuuje prostředky úložiště pro zajištění odpovídající redundance. Další informace najdete v článku [Azure Přehled služby Managed Disks] [.. / spravovaných overview.md disků). Podrobnosti o spravovaných discích ve skupině dostupnosti najdete v tématu [Použití spravovaných disků pro virtuální počítače ve skupině dostupnosti](../manage-availability.md).

* V části **sítě**, vyberte všechny vstupní porty, které v **vyberte veřejné příchozí porty** seznamu. Například, pokud chcete vzdálené plochy k virtuálnímu počítači, vyberte **protokolu RDP (3389)** portu.

   ![Příchozí porty](./media/quickstart-sql-vm-create-portal/inbound-ports.png)

   > [!NOTE]
   > Pokud chcete získat vzdálený přístup k SQL Serveru, můžete vybrat port **MS SQL (1433)**. Ale to není nezbytné, protože **nastavení systému SQL Server** krok obsahuje také tuto možnost. Pokud v tomto kroku vyberete port 1433, otevře se bez ohledu na vaše výběry v kroku **Nastavení SQL Serveru**.

   Můžete udělat jiné změny nastavení sítě, nebo ponechte výchozí hodnoty.

* Azure umožňuje **monitorování** ve výchozím nastavení se stejným účtem, jaký je nastavený pro virtuální počítač. Tato nastavení tady můžete změnit.

* V části **dostupnosti**, můžete ponechat výchozí hodnotu **žádný** v tomto návodu. Pokud budete chtít nastavit Skupiny dostupnosti AlwaysOn SQL, nakonfigurujte dostupnost tak, aby se předešlo opětovnému vytvoření virtuálního počítače.  Další informace najdete v tématu [Správa dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Po dokončení konfigurace těchto nastavení klikněte na **OK**.

## <a name="4-configure-sql-server-settings"></a>4. Konfigurace nastavení SQL Serveru
V okně **Nastavení SQL Serveru** nakonfigurujte konkrétní nastavení a optimalizace pro SQL Server. Pro SQL Server můžete například nakonfigurovat následující nastavení.

| Nastavení |
| --- |
| [Připojení](#connectivity) |
| [Ověřování](#authentication) |
| [Konfigurace úložiště](#storage-configuration) |
| [Automatizované opravy](#automated-patching) |
| [Automatizované zálohování](#automated-backup) |
| [Integrace se službou Azure Key Vault](#azure-key-vault-integration) |
| [SQL Server Machine Learning Services](#sql-server-machine-learning-services) |

### <a name="connectivity"></a>Připojení

V části **Připojení SQL** zadejte typ přístupu, který chcete mít k instanci SQL Serveru na tomto virtuálním počítači. Pro účely tohoto názorného postupu, vyberte **veřejné (internet)** umožňující připojení k SQL serveru z počítačů nebo služeb na Internetu. Když je vybraná tato možnost, Azure automaticky nakonfiguruje bránu firewall a skupinu zabezpečení sítě tak, aby umožňovaly přenosy na portu 1433.

![Možnosti připojení SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

> [!TIP]
> Ve výchozím nastavení SQL Server naslouchá na dobře známém portu **1433**. Pokud chcete zvýšit zabezpečení, změňte port v předchozím dialogovém okně tak, aby SQL Server naslouchal na jiném než výchozím portu, například 1401. Pokud změníte port, musíte se připojit pomocí tohoto portu ze všech nástrojů klienta, jako je SSMS.

Aby bylo možné se k SQL Serveru připojovat prostřednictvím internetu, musíte také povolit ověřování SQL Serveru, které je popsané v následující části.

Pokud raději nechcete povolovat připojení k databázovému stroji prostřednictvím internetu, zvolte jednu z následujících možností:

* **Místní (jen uvnitř virtuálního počítače):** Umožňuje připojení k SQL Serveru pouze v rámci virtuálního počítače.
* **Privátní (uvnitř virtuální sítě):** Umožňuje připojení k SQL Serveru z počítačů nebo služeb ve stejné virtuální síti.

Obecně se doporučuje zvýšit zabezpečení výběrem nejvíce omezujícího připojení, které váš scénář umožňuje. Všechny možnosti je ale možné zabezpečit prostřednictvím pravidel skupin zabezpečení sítě a ověřování SQL Serveru a Windows. Skupinu zabezpečení sítě můžete upravit po vytvoření virtuálního počítače. Další informace najdete v tématu [Informace o zabezpečení pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-security.md).

### <a name="authentication"></a>Authentication

Pokud budete chtít vyžadovat ověřování SQL Serveru, klikněte v části **Ověřování SQL** na **Povolit**.

![Ověřování SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Pokud budete chtít přístup k SQL serveru prostřednictvím Internetu (nastavením veřejného připojení), které musíte tu povolit ověřování SQL. Veřejný přístup k SQL Serveru vyžaduje použití ověřování SQL Serveru.

Pokud povolíte ověřování SQL Serveru, zadejte **přihlašovací jméno** a **heslo**. Toto uživatelské jméno se nakonfiguruje jako přihlašovací jméno ověřování SQL Serveru a člen pevné role serveru **sysadmin**. Další informace o režimech ověřování najdete v tématu [Volba režimu ověřování](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

Pokud ověřování SQL Serveru nepovolíte, můžete pro připojení k instanci SQL Serveru používat účet místního správce ve virtuálním počítači.

### <a name="storage-configuration"></a>Konfigurace úložiště

Klikněte na **Konfigurace úložiště** a zadejte požadavky na úložiště.

![Konfigurace úložiště SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Pokud jste virtuální počítač ručně nakonfigurovali tak, aby používal Storage úrovně Standard, tato možnost není dostupná. Automatická optimalizace úložiště je k dispozici pouze pro Premium Storage.

> [!TIP]
> Počet zastavení a horní omezení každého posuvníku závisí na velikosti vybraného virtuálního počítače. Větší a výkonnější virtuální počítač umožňuje větší vertikální navýšení kapacity.

Požadavky můžete zadat jako vstupně-výstupní operace za sekundu (IOPs), propustnost v MB/s a celkovou velikost úložiště. Tyto hodnoty nakonfigurujte pomocí posuvníků. Tato nastavení úložiště můžete podle náročnosti zpracovávaných úloh změnit. Portál na základě těchto požadavků automaticky vypočítá počet disků, které se mají připojit a nakonfigurovat.

V části **Optimalizace úložiště** vyberte jednu z následujících možností:

* **Obecné:** Výchozí nastavení a podporuje většinu úloh.
* **Transakční:** Toto zpracování optimalizuje úložiště pro standardní úlohy databází OLTP.
* **Datové sklady:** Optimalizuje úložiště pro úlohy analýz a generování sestav.

### <a name="automated-patching"></a>Automatizované opravy

**Automatizované opravy** jsou ve výchozím nastavení povolené. Automatizované opravy umožňují na platformě Azure automaticky opravovat SQL Server a operační systém. Zadejte den v týdnu, čas a dobu trvání intervalu údržby. V té době pak Azure nainstaluje potřebné opravy. V rámci plánování intervalu údržby se pro čas používá národní prostředí virtuálních počítačů. Pokud nechcete, aby se v rámci Azure automaticky opravoval SQL Server a operační systém, klikněte na **Zakázat**.  

![Automatizované opravy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Další informace najdete v tématu [Automatizované opravy pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatizované zálohování

Automatické zálohování databází můžete pro všechny databáze povolit v části **Automatizované zálohování**. Automatizované zálohování je ve výchozím nastavení zakázané.

Když povolíte automatizované zálohování SQL, můžete nakonfigurovat následující nastavení:

* Doba uchování dat (dny) pro zálohování
* Účet úložiště, který se má používat pro zálohování
* Možnost šifrování a heslo pro zálohování
* Zálohování systémových databází
* Konfigurování plánu zálohování

Pokud chcete zálohy šifrovat, klikněte na **Povolit**. Pak zadejte **heslo**. Azure vytvoří certifikát pro šifrování záloh a používá zadané heslo k ochraně tohoto certifikátu.

![Automatizované zálohování SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Další informace najdete v tématu [Automatizované zálohování pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Integrace se službou Azure Key Vault

Pokud budete chtít ukládat tajné klíče zabezpečení v Azure pro šifrování, klikněte na **Integrace se službou Azure Key Vault** a klikněte na **Povolit**.

![Integrace se službou Azure Key Vault pro SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

V následující tabulce jsou uvedeny parametry, které jsou nezbytné pro konfiguraci Integrace se službou Azure Key Vault.

| PARAMETR | POPIS | PŘÍKLAD |
| --- | --- | --- |
| **Adresa URL služby Key Vault** |Umístění služby Key Vault |https://contosokeyvault.vault.azure.net/ |
| **Název objektu zabezpečení** |Hlavní název služby Azure Active Directory. Tento název se také označuje jako ID klienta. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Tajný kód objektu zabezpečení** |Tajný klíč objektu zabezpečení služby Azure Active Directory. Tento tajný klíč se také označuje jako Tajný klíč klienta. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Název přihlašovacího údaje** |**Název přihlašovacího údaje**: Integrace se službou AZURE vytvoří přihlašovací údaje v rámci SQL serveru, díky čemuž mají přístup k trezoru klíčů virtuální počítače. Zvolte název pro tyto přihlašovací údaje. |moje_přihlaš1 |

Další informace najdete v tématu [Konfigurace Integrace se službou Azure Key Vault pro virtuální počítače Azure](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Services

Máte možnost povolit službu [SQL Server Machine Learning Services](https://msdn.microsoft.com/library/mt604845.aspx). Tato možnost umožňuje používat pokročilé analýzy s SQL serverem 2017. V okně **Nastavení SQL Serveru** klikněte na **Povolit**.

![Povolení služby SQL Server Machine Learning Services](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

Po dokončení konfigurace nastavení SQL Serveru klikněte na **OK**.

## <a name="5-review-the-summary"></a>5. Kontrola souhrnných informací

V okně **Souhrn** zkontrolujte souhrn a pak kliknutím na **Koupit** vytvořte SQL Server, skupinu prostředků a prostředky zadané pro tento virtuální počítač.

Nasazení můžete monitorovat z webu Azure Portal. Tlačítko **Oznámení** v horní části obrazovky zobrazuje základní stav nasazení.

> [!NOTE]
> Abyste si udělali představu o tom, jak dlouho nasazování trvá, nasadil jsem virtuální počítač s SQL Serverem pro oblast Východní USA s výchozím nastavením. Dokončení tohoto testovacího nasazení trvalo přibližně 12 minut. Na základě vaší oblasti a vybraného nastavení ale můžete zaznamenat kratší nebo delší čas nasazení.

## <a id="remotedesktop"></a>Otevření virtuálního počítače pomocí Vzdálené plochy

Podle následujícího postupu se připojte k virtuálnímu počítači s SQL Serverem pomocí Vzdálené plochy:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Po připojení k virtuálnímu počítači s SQL Serverem můžete spustit SQL Server Management Studio a připojit se pomocí ověřování systému Windows se svými přihlašovacími údaji místního správce. Pokud jste povolili ověřování SQL Serveru, můžete se také připojit pomocí ověřování SQL Serveru a použít k tomu přihlašovací jméno a heslo SQL Serveru, které jste nakonfigurovali během zřizování.

Přístup k počítači vám umožňuje podle potřeb přímo měnit nastavení počítače a SQL Serveru. Můžete například nakonfigurovat nastavení brány firewall nebo změnit nastavení konfigurace SQL Serveru.

## <a id="connect"></a>Vzdálené připojení k SQL Serveru

V tomto návodu jste vybrali **veřejné** přístup pro virtuální počítač a **ověřování systému SQL Server**. Tato nastavení automaticky nakonfigurovala virtuální počítač tak, aby povoloval připojení k SQL Serveru z libovolného klienta přes internet (za předpokladu, že má správné přihlašovací údaje SQL Serveru).

> [!NOTE]
> Pokud jste nevybrali veřejný přístup během zřizování, můžete prostřednictvím portálu změnit nastavení připojení SQL po zřízení. Další informace najdete v tématu popisujícím [změnu nastavení připojení SQL](virtual-machines-windows-sql-connect.md#change).

Následující části vysvětlují, jak se připojit přes internet k vaší instanci virtuálního počítače s SQL serverem.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Další kroky

Další informace o používání SQL Serveru v Azure najdete v tématu [SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) a [Nejčastější dotazy](virtual-machines-windows-sql-server-iaas-faq.md).