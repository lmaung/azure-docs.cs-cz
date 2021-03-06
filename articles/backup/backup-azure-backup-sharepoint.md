---
title: Aplikace DPM nebo Azure Backup server ochrana Sharepointové farmy do Azure
description: Tento článek obsahuje přehled ochrany aplikace DPM nebo Azure Backup serveru farmy služby SharePoint do Azure
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: kasinh
ms.openlocfilehash: 35f9b76e27a0977a25f6d060f7362bc417e0568e
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813853"
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a>Zálohování sharepointové farmy do Azure
Zálohujete Sharepointové farmy do Microsoft Azure mnohem stejným způsobem, který je zálohovat zdrojů dat pomocí System Center Data Protection Manager (DPM). Azure Backup poskytuje flexibilitu při plán zálohování a vytvořit každý den, týdenní, měsíční nebo roční zálohu odkazuje a poskytuje možnosti zásad uchovávání informací pro různé body záloh. DPM poskytuje možnost ukládat kopie místního disku pro rychlé cíle plánované doby obnovení (RTO) a k uložení kopie do Azure pro hospodárná a dlouhodobé uchovávání.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>Podporované verze služby SharePoint a související scénáře ochrany
Azure Backup pro DPM podporuje následující scénáře:

| Úloha | Verze | Nasazení služby SharePoint | Typ nasazení aplikace DPM | DPM – System Center 2012 R2 | Ochrana a obnovení |
| --- | --- | --- | --- | --- | --- |
| SharePoint |SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0 |SharePoint nasadit jako fyzický server nebo virtuální počítač Hyper-V nebo VMware <br> -------------- <br> SQL AlwaysOn |Fyzický server nebo místní Hyper-V virtuálního počítače |Podporuje zálohování do Azure od kumulativní aktualizace 5 |Ochrana farmy služby SharePoint možnosti obnovení: Obnovení farmy, databáze a soubor nebo položka seznamu z bodů obnovení disku.  Obnovení farmy a databáze z bodů obnovení Azure. |

## <a name="before-you-start"></a>Než začnete
Existuje několik věcí, které je potřeba potvrdit před zálohování Sharepointové farmy do Azure.

### <a name="prerequisites"></a>Požadavky
Než budete pokračovat, ujistěte se, že jste splnili veškeré [požadavků na používání Microsoft Azure Backup](backup-azure-dpm-introduction.md#prerequisites-and-limitations) k ochraně úloh. Některé úlohy pro požadované součásti patří: Vytvořte trezor záloh, stáhnout přihlašovací údaje trezoru, nainstalovat agenta Azure Backup a zaregistrujete pomocí úložiště DPM nebo Azure Backup Server.

### <a name="dpm-agent"></a>Agent aplikace DPM
Na serveru, na kterém běží SharePoint, servery, na kterých běží SQL Server a všechny ostatní servery, které jsou součástí farmy služby SharePoint musí být nainstalován DPM agent. Další informace o tom, jak nastavit agenta ochrany najdete v tématu [instalace agenta ochrany](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  Jedinou výjimkou je, že nainstalujete agenta jenom na jednom webovém serveru front-end (WFE). DPM potřebuje agenta na jeden server WFE pouze, která bude sloužit jako vstupní bod pro ochranu.

### <a name="sharepoint-farm"></a>Farmy služby SharePoint
Pro každých 10 milionů položek ve farmě musí být alespoň 2 GB volného místa ve svazku, kde je umístěna složka aplikace DPM. Toto místo je nezbytné pro generování katalogu. Aplikace DPM k obnovení konkrétní položky (kolekce webů, weby, seznamy, knihovny dokumentů, složky, jednotlivé dokumenty a položky seznamu) vytvoří generování katalogu seznam adres URL, které jsou obsaženy v jednotlivých databázích obsahu. Zobrazí se seznam adres URL v podokno obnovitelných položek **obnovení** oblasti konzole správce aplikace DPM úlohy.

### <a name="sql-server"></a>SQL Server
Aplikace DPM běží pod účtem LocalSystem. Aplikace DPM k zálohování databází systému SQL Server, potřebuje oprávnění správce systému na tomto účtu pro server, na kterém běží SQL Server. Nastavte NT AUTHORITY\SYSTEM na *sysadmin* na serveru, na kterém běží systém SQL Server před zálohováním.

Pokud jsou farmy služby SharePoint databáze systému SQL Server, které jsou nakonfigurovány s aliasy systému SQL Server, nainstalujte komponenty klienta SQL serveru na front-end webovém serveru, který bude chráněný aplikací DPM.

### <a name="sharepoint-server"></a>SharePoint Server
Zatímco výkon závisí na mnoha faktorech, jako je například velikost Sharepointové farmy, jako obecné pokyny jeden server DPM dokáže chránit farmu služby SharePoint 25 TB.

### <a name="dpm-update-rollup-5"></a>DPM s kumulativní aktualizací 5
Zahajte ochranu farmy služby SharePoint do Azure, budete muset nainstalovat DPM s kumulativní aktualizací 5 nebo novější. S kumulativní aktualizací 5 nabízí možnost chránit Sharepointové farmy do Azure, pokud je farma je nakonfigurovaná pomocí SQL AlwaysOn.
Další informace najdete v blogovém příspěvku, který představuje [DPM kumulativní aktualizace 5](http://blogs.technet.com/b/dpm/archive/2015/02/11/update-rollup-5-for-system-center-2012-r2-data-protection-manager-is-now-available.aspx)

### <a name="whats-not-supported"></a>Co není podporováno
* Aplikace DPM chrání Sharepointové farmy nechrání vyhledávací indexy nebo databází aplikačních služeb. Je potřeba konfigurovat ochranu těchto databází samostatně.
* Aplikace DPM neposkytuje zálohování databází SQL serveru SharePoint, které jsou hostované na horizontální navýšení kapacity sdílené složky serveru systémů (SOFS).

## <a name="configure-sharepoint-protection"></a>Konfigurace ochrany Sharepointu
Před použitím aplikace DPM k ochraně služby SharePoint, musíte nakonfigurovat službu zapisovač SharePoint VSS (služby WSS Writer) s použitím **ConfigureSharePoint.exe**.

Můžete najít **ConfigureSharePoint.exe** ve složce [instalační cestě DPM] \bin na front-end webovém serveru. Tento nástroj poskytuje agenta ochrany s pověřeními pro farmu služby SharePoint. Můžete ho spustit na jednom serveru WFE. Pokud máte víc serverů WFE, vyberte pouze jednu když konfigurujete skupinu ochrany.

### <a name="to-configure-the-sharepoint-vss-writer-service"></a>Ke konfiguraci služby SharePoint VSS Writer service
1. Na serveru WFE, na příkazovém řádku přejděte do [umístění instalace aplikace DPM] \bin\
2. Zadejte ConfigureSharePoint - EnableSharePointProtection.
3. Zadejte přihlašovací údaje správce farmy. Tento účet musí být členem místní skupiny správců na serveru WFE. Pokud není správcem farmy místní správce, udělte na serveru WFE tato oprávnění:
   * Udělte skupině WSS_Admin_WPG úplnou kontrolu ke složce aplikace DPM (% Program Files%\Microsoft Data Protection Manager\DPM).
   * Udělte oprávnění ke čtení skupině WSS_Admin_WPG ke klíči registru aplikace DPM (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).

> [!NOTE]
> Bude potřeba znovu spusťte ConfigureSharePoint.exe pokaždé, když dojde ke změně v pověřeních správce farmy Sharepointu.
>
>

## <a name="back-up-a-sharepoint-farm-by-using-dpm"></a>Zálohování farmy služby SharePoint pomocí aplikace DPM
Po konfiguraci DPM a farmy služby SharePoint, jak bylo popsáno dříve Sharepointu můžete chránit pomocí DPM.

### <a name="to-protect-a-sharepoint-farm"></a>Chcete-li chránit farmu služby SharePoint
1. Z **ochrany** kartu z konzole správce aplikace DPM klikněte na tlačítko **nový**.
    ![Nová karta ochrany](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. Na **vybrat typ skupiny ochrany** stránku **vytvořením nové skupiny ochrany** průvodce, zaškrtněte políčko **servery**a potom klikněte na tlačítko **Další**.

    ![Vyberte skupinu ochrany typu](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. Na **vybrat členy skupiny** obrazovky, zaškrtněte políčko pro SharePoint server, který chcete chránit a klikněte na tlačítko **Další**.

    ![Vybrat členy skupiny](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > S nainstalovaným agentem DPM uvidíte serveru v průvodci. Aplikace DPM zobrazuje také jeho strukturu. Protože jste spustili ConfigureSharePoint.exe, aplikace DPM komunikuje se službou SharePoint VSS Writer service a její odpovídající databáze systému SQL Server a rozpozná strukturu farmy služby SharePoint, přidružených databází obsahu a všechny odpovídající položky.
   >
   >
4. Na **vybrat způsob ochrany dat** stránky, zadejte název **skupiny ochrany**a vyberte upřednostňovanou *metody ochrany*. Klikněte na **Další**.

    ![Vyberte způsob ochrany dat](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > Způsob ochrany disku pomáhá plnit krátký cíle plánované doby obnovení. Azure je cíl hospodárná a dlouhodobou ochranu ve srovnání s páskami. Další informace najdete v tématu [použití Azure Backup k náhradě páskové infrastruktury](https://azure.microsoft.com/documentation/articles/backup-azure-backup-cloud-as-tape/)
   >
   >
5. Na **zadat krátkodobé cíle** stránky, vyberte upřednostňovanou **rozsah uchování** a zjistíte, kdy chcete, aby vytváření záloh každý.

    ![Zadat krátkodobé cíle](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > Vzhledem k tomu, že obnovení je nejčastěji požadované pro data, která je kratší než pět dní, jsme vybrali rozsahem uchování 5 dní na disku a zajistit, že zálohování se stane během hodin mimo produkční, v tomto příkladu.
   >
   >
6. Zkontrolujte místo přidělené pro skupinu ochrany na disku fondu úložiště a potom na tlačítko **Další**.
7. Pro každou skupinu ochrany aplikace DPM přiděluje místo na disku k ukládání a správě repliky. V tomto okamžiku DPM musí vytvořit kopii vybraného data. Vyberte, jak a kdy má replika vytvořena a potom klikněte na tlačítko **Další**.

    ![Vyberte způsob vytvoření repliky](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > Pokud chcete mít jistotu, že se to týká síťový provoz, vyberte čas mimo pracovní doby.
   >
   >
8. DPM zajišťuje integritu dat pomocí provádí kontroly konzistence na replice. Existují dvě možnosti k dispozici. Můžete definovat plán pro spuštění kontroly konzistence nebo pokaždé, když se stane nekonzistentní se DPM dá spustit kontroly konzistence na replice automaticky. Vyberte upřednostňovanou možnost ověřování a potom klikněte na tlačítko **Další**.

    ![Kontrola konzistence](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. Na **zadat Data Online ochrany** vyberte Sharepointové farmy, který chcete chránit a potom klikněte na tlačítko **Další**.

    ![Aplikace DPM Protection1 služby SharePoint](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. Na **zadat plán Online zálohování** stránky, vyberte upřednostňovaný plán a pak klikněte na tlačítko **Další**.

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > DPM poskytuje maximálně dvě denní zálohování do Azure v různých časech. Azure Backup můžete také určit, jaká část šířky pásma sítě WAN, který lze použít pro zálohy v ve špičce a hodiny mimo špičku s použitím [omezení sítě Azure Backup](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).
    >
    >
11. V závislosti na plánu zálohování, který jste vybrali, na **zadat zásady Online uchovávání** vyberte zásady uchovávání informací pro denní, týdenní, měsíční a roční body záloh.

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > Aplikace DPM používá schéma uchovávání dědečka ze strany otce SYN ve které je možné zvolit jiné zásady uchovávání informací pro různé body záloh.
    >
    >
12. Podobně jako na disk, repliku odkaz na počáteční bod je potřeba vytvořit v Azure. Vyberte upřednostňovanou možnost vytvořit počáteční záložní kopii do Azure, a potom klikněte na **Další**.

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Zkontrolujte zvolená nastavení na **Souhrn** stránce a potom klikněte na tlačítko **vytvořit skupinu**. Zobrazí se zpráva o úspěchu a po vytvoření skupiny ochrany.

    ![Souhrn](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-dpm"></a>Obnovit položky služby SharePoint pomocí aplikace DPM z disku
V následujícím příkladu *položky obnovení Sharepointu* omylem odstraněný a je potřeba obnovit.
![Aplikace DPM Protection4 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Otevřít **konzoli správce aplikace DPM**. Všechny farmy služby SharePoint, které jsou chráněné službou DPM jsou uvedeny v **ochrany** kartu.

    ![Aplikace DPM Protection3 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. Chcete-li začít obnovení položky, vyberte **obnovení** kartu.

    ![Aplikace DPM Protection5 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. Hledáte služby SharePoint pro *položky obnovení Sharepointu* pomocí zástupných znaků podle hledání v rámci obnovení bodu rozsahu.

    ![Aplikace DPM Protection6 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Vyberte bod obnovení odpovídající ve výsledcích hledání klikněte pravým tlačítkem na položku a pak vyberte **obnovit**.
5. Můžete také procházet různé body obnovení a vyberte databázi nebo položku, kterou chcete obnovit. Vyberte **datum > čas obnovení**a potom vyberte správné **databáze > Sharepointové farmy > bod obnovení > položka**.

    ![Aplikace DPM Protection7 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Klikněte pravým tlačítkem na položku a pak vyberte **obnovit** otevřít **Průvodce obnovením**. Klikněte na **Další**.

    ![Revidovat výběr obnovení](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Vybrat typ obnovení, který chcete provést a pak klikněte na tlačítko **Další**.

    ![Typ obnovení](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > Výběr **obnovit na původní** v příkladu obnoví položky, která má původní webu služby SharePoint.
   >
   >
8. Vyberte **proces obnovení** , kterou chcete použít.

   * Vyberte **obnovit bez použití farmy obnovení** Pokud farmy služby SharePoint se nezměnil a je stejný jako bod obnovení, který se obnovuje.
   * Vyberte **ho obnovit pomocí farmy obnovení** Pokud od vytvoření bodu obnovení změnila farmy služby SharePoint.

     ![Proces obnovení](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Zadejte pracovní umístění instance systému SQL Server pro obnovení databáze dočasně a zadejte pracovní sdílené složky na serveru DPM a serveru, na kterém je spuštěna služba SharePoint k obnovení položky.

    ![Pracovní Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    Aplikace DPM připojí databáze obsahu, který je hostitelem Sharepointových položek do dočasné instanci systému SQL Server. Z databáze obsahu server aplikace DPM obnoví položku a umístí ji na pracovní umístění souboru na serveru DPM. Položku, která je teď na pracovním umístění serveru DPM musí být exportovány do pracovního umístění na farmě služby SharePoint.

    ![Pracovní Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Vyberte **zadat možnosti obnovení**a použít nastavení zabezpečení pro farmu služby SharePoint nebo použít nastavení zabezpečení bodu obnovení. Klikněte na **Další**.

    ![Možnosti obnovení](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > Můžete omezit využití šířky pásma sítě. To minimalizuje dopad na produkční server během pracovní doby.
    >
    >
11. Zkontrolujte souhrnné informace a pak klikněte na tlačítko **obnovit** zahájíte obnovení souboru.

    ![Obnovení souhrnu](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Teď vyberte **monitorování** kartu **konzoli správce aplikace DPM** zobrazíte **stav** obnovení.

    ![Stav obnovení](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > Soubor je nyní obnovit. Můžete aktualizovat web služby SharePoint ke kontrole se obnovil soubor.
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>Obnovení databáze služby SharePoint z Azure pomocí DPM
1. Obnovení databáze obsahu služby SharePoint, projděte si různé body obnovení (jak je uvedeno dříve) a vyberte bod obnovení, který chcete obnovit.

    ![Aplikace DPM Protection8 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Klikněte dvakrát na bod obnovení služby SharePoint k zobrazení k dispozici informace o katalogu služby SharePoint.

   > [!NOTE]
   > Protože farmy služby SharePoint je chráněný pro dlouhodobé uchovávání v Azure, je k dispozici na serveru aplikace DPM bez katalogu informacemi (metadata). V důsledku toho pokaždé, když se potřeby nutné obnovit databázi obsahu služby SharePoint bodu v čase, budete muset znovu katalogu farmy služby SharePoint.
   >
   >
3. Klikněte na tlačítko **opětovného zařazení do katalogu**.

    ![Aplikace DPM Protection10 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    **Opětovně zařadit do katalogu cloudových** otevře se okno stavu.

    ![Aplikace DPM Protection11 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Po dokončení do katalogu se stav změní na *úspěch*. Klikněte na **Zavřít**.

    ![Aplikace DPM Protection12 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. Klikněte na objekt Sharepointu v DPM **obnovení** kartu struktura databáze obsahu. Klikněte pravým tlačítkem na položku a pak klikněte na tlačítko **obnovit**.

    ![Aplikace DPM Protection13 služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. V tomto okamžiku postupujte podle kroků obnovení dříve v tomto článku k obnovení databáze obsahu služby SharePoint z disku.

## <a name="faqs"></a>Nejčastější dotazy

### <a name="which-versions-of-dpm-support-sql-server-2014-and-sql-2012-sp2"></a>Které verze aplikace DPM podporuje SQL Server 2014 a SQL 2012 (SP2)?
Aplikace DPM 2012 R2 s kumulativní aktualizací 4 podporuje oboje.

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>Můžete obnovit Sharepointových položek do původního umístění, pokud je služba SharePoint nakonfigurována pomocí AlwaysOn serveru SQL (ochrana na disku)?
Ano, položka je možné obnovit do původního webu služby SharePoint.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>Můžete obnovit do původního umístění databáze služby SharePoint, pokud je služba SharePoint nakonfigurována s použitím SQL AlwaysOn?
Protože SharePoint databází nakonfigurovaných v SQL AlwaysOn, jejich nelze upravit, pokud je skupina dostupnosti odebrána. Aplikace DPM v důsledku toho nelze obnovit databázi do původního umístění. Můžete obnovit databázi systému SQL Server na jinou instanci systému SQL Server.

## <a name="next-steps"></a>Další postup
* Další informace o DPM ochrany Sharepointu – viz [seriál videí – DPM ochrany služby SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
* Kontrola [poznámky k verzi pro System Center 2012 – Data Protection Manager](https://technet.microsoft.com/library/jj860415.aspx)
* Kontrola [zpráva k vydání verze pro aplikaci Data Protection Manager v System Center 2012 SP1](https://technet.microsoft.com/library/jj860394.aspx)
