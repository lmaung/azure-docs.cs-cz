---
title: Funkce Azure rychlé obnovení
description: Funkce Rychlé obnovení a nejčastější dotazy pro virtuální počítač Azure backup zásobníku, modelu nasazení Resource Manager
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: sogup
ms.openlocfilehash: 3f6d614d1487174a361d562212c08ec5b3aaf220
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960016"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Získejte lepší zálohování a obnovení výkonu pomocí funkce Azure Backup rychlé obnovení

> [!NOTE]
> Na základě zpětné vazby od uživatelů jsme přejmenovali **zásobník záloh virtuálních počítačů V2** k **rychlé obnovení** abyste snížili nejasnosti s funkcemi Azure Stack.

Nový model pro rychlé obnovení obsahuje následující vylepšení funkcí:

* Možnost používat snímkům pořízeným jako součást, která je k dispozici pro obnovení bez čekání na přenos dat do trezoru na dokončení úlohy zálohování. Snižuje dobu čekání pro snímky ke zkopírování do trezoru před aktivace operace obnovení.
* Zkracuje dobu zálohování a obnovení tak, že zachová snímky místně, ve výchozím nastavení dva dny. Tento trezor výchozí je možné konfigurovat na libovolnou hodnotu mezi 1 až 5 dnů uvolní.
* Podporuje disk o velikosti až 4 TB.
* Podporuje disky SSD na úrovni Standard.
*   Možnost používat nespravovaného virtuálního počítače na původní účty úložiště (na disk), při obnovování. Tato schopnost existuje i v případě, že virtuální počítač obsahuje disky, které jsou distribuovány mezi různými účty úložiště. Urychluje operace obnovení pro celou řadu konfigurací virtuálních počítačů



## <a name="whats-new-in-this-feature"></a>Co je nového v této funkci

V současné době úloha zálohování se skládá ze dvou fází:

1.  Pořízení snímku virtuálního počítače.
2.  Snímek virtuálního počítače přenosu do trezoru služby Azure Recovery Services.

Vytvoření bodu obnovení se považuje za pouze po dokončení fáze 1 a 2. Jako součást tohoto upgradu je bod obnovení vytvořen, jakmile dokončení snímku a tohoto bodu obnovení snímku typu je možné provést obnovení pomocí stejný tok obnovení. Tento bod obnovení na portálu Azure portal můžete identifikovat pomocí "snímku" jako typ bodu obnovení a po snímku se přenesou do trezoru, typ bodu obnovení se změní na "snímek a trezor".

![Úloha zálohování ve virtuálním počítači zásobník záloh modelu nasazení Resource Manager – úložiště a trezoru](./media/backup-azure-vms/instant-rp-flow.png)

Ve výchozím nastavení snímky zůstanou zachovány dva dny. Tato funkce umožňuje operace obnovení z těchto snímků existuje řezem dolů časů obnovení. Snižuje čas, který se vyžaduje k transformaci a kopírování dat z trezoru zpět do uživatelského účtu úložiště pro scénáře nespravovaný disk a pro uživatele spravovaného disku a vytvoří spravovaných disků z dat služby Recovery Services.

## <a name="feature-considerations"></a>Důležité informace o funkci

* Snímky se ukládají společně s disky, a zvýšit tak vytvoření bodu obnovení a to ke zrychlení operací obnovení. V důsledku toho se zobrazí náklady na úložiště, které odpovídají snímkům pořízeným během tohoto období.
* Přírůstkové snímky se ukládají jako objekty BLOB stránky. Všichni uživatelé používali nespravované disky se vám účtuje snímky uloženým v jejich účtu místní úložiště. Protože kolekce bodů obnovení, který se používá záloh virtuálních počítačů spravovaných pomocí snímky objektů blob na základní úrovni úložiště, za spravované disky se zobrazí náklady odpovídající do objektu blob ceny snímků a jsou přírůstkové.
* Pro účty služby premium storage snímkům pořízeným pro okamžité obnovení počet bodů vůči limit 10 TB přidělené místo.
* Získáte možnost konfigurace uchovávání snímku na základě potřeb obnovení. V závislosti na požadavku můžete nastavit uchování snímků pro minimálně jeden den v okně zásady zálohování, jak je popsáno níže. To vám umožňují snížení nákladů pro uchovávání dat snímku, pokud nechcete provádět obnovení často.
* Toto je jeden směrové upgradu po upgradu na rychlé obnovení, budete se moci vrátit.


>[!NOTE]
>Pomocí této rychlé obnovení upgradu, doba uchovávání snímek u všech zákazníků (**nové i stávající zahrnuté obě**) se nastaví na výchozí hodnotu dvou dnů. Však můžete nastavit dobu trvání podle vašich požadavků na libovolnou hodnotu mezi 1 až 5 dnů uvolní.


## <a name="cost-impact"></a>Cenově dopad

Přírůstkovými snímky se ukládají v účtu úložiště Virtuálního počítače, které se používají pro rychlé obnovení. Přírůstkový snímek znamená, že místo obsazené snímku je rovna místo obsazené stránky, které jsou zapsány po vytvoření snímku. Fakturace je stále za GB je stejný, jak je uvedeno v využité místo obsazena snímek a cena za GB [stránce s cenami](https://azure.microsoft.com/pricing/details/managed-disks/).


## <a name="upgrading-to-instant-restore"></a>Upgrade na rychlé obnovení

Pokud použijete Azure portal, zobrazí se oznámení na řídicím panelu trezoru. Toto oznámení se týká podporu velkých disků a zvýšení rychlosti zálohování a obnovení.
Otevření obrazovky pro upgrade na rychlé obnovení, klikněte na banner.

![Úloha zálohování v modelu nasazení Resource Manager zásobník záloh virtuálních počítačů – podpora oznámení](./media/backup-azure-vms/instant-rp-banner.png)

Klikněte na **upgradovat** jak je znázorněno v následujícím snímku obrazovky:

![Úloha zálohování v zásobníku zálohování virtuálních počítačů modelu nasazení Resource Manager – upgrade](./media/backup-azure-vms/instant-rp.png)

Alternativně můžete přejít na **vlastnosti** stránce trezoru, chcete-li získat **upgradovat** v části **zásobník záloh virtuálních počítačů**.

![Úloha zálohování v zásobníku zálohování virtuálních počítačů – stránka Vlastnosti](./media/backup-azure-vms/instant-restore-capability-properties.png)


## <a name="configure-snapshot-retention-using-azure-portal"></a>Konfigurace uchovávání snímek pomocí webu Azure portal
Tato možnost je teď dostupná v **střed USA – západ**, **Indie – jih**, **Austrálie – východ**, a **Severní Evropa**.

Upgradovaný uživatelů na webu Azure Portal uvidíte pole přidá **zásady zálohování virtuálních počítačů** okně v části **rychlé obnovení** části. Doba uchování snímků z můžete změnit **zásady zálohování virtuálního počítače** okno pro všechny virtuální počítače přidružené k určité zásady zálohování.

![Funkce Rychlé obnovení](./media/backup-azure-vms/instant-restore-capability.png)

## <a name="upgrade-to-instant-restore-using-powershell"></a>Upgrade provést rychlé obnovení pomocí prostředí PowerShell

Pokud chcete samoobslužné a upgradovat na rychlé obnovení, spusťte následující rutiny z Powershellu se zvýšenými oprávněními terminálu:

1.  Přihlaste se ke svému účtu Azure:

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Vyberte předplatné, pro kterou chcete zaregistrovat:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Zaregistrujte toto předplatné:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="upgrade-to-instant-restore-using-cli"></a>Upgrade provést rychlé obnovení pomocí rozhraní příkazového řádku

Spusťte následující příkazy z prostředí:

1.  Přihlaste se ke svému účtu Azure:

    ```
    az login
    ```

2.  Vyberte předplatné, pro kterou chcete zaregistrovat:

    ```
    az account set --subscription "Subscription Name"
    ```

3.  Zaregistrujte toto předplatné:

    ```
    az feature register --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
    ```

## <a name="verify-that-the-upgrade-is-successful"></a>Ověřte, že upgrade se úspěšně dokončila

### <a name="powershell"></a>PowerShell
Prostředí PowerShell terminálu se zvýšenými oprávněními spusťte následující rutinu:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" -ProviderNamespace Microsoft.RecoveryServices
```

### <a name="cli"></a>Rozhraní příkazového řádku
Z prostředí spusťte následující příkaz:

```
az feature show --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
```

Když je ve stavu "Registrováno", je vaše předplatné upgradovat na rychlé obnovení.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Jaké jsou náklady na důsledcích okamžitá obnova?
Snímky se ukládají společně s disky zrychlit vytvoření bodu obnovení a obnovení operací. V důsledku toho se zobrazí náklady na úložiště, které odpovídají uchování snímků vybrali jako součást zásad zálohování virtuálního počítače.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>V účtech Premium Storage snímkům pořízeným okamžité obnovení bodu zabírat omezení počtu snímků 10 TB?
Ano, pro účty služby premium storage snímkům pořízeným okamžité obnovení bodu zabírat 10 TB místa přiděleného snímku.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Jak funguje uchování snímků během období 5 dní?
Každý den nový snímek je převzata, a pak jednotlivým pět přírůstkových snímků. Velikost snímku závisí na četnosti změn dat, která je ve většině případů přibližně 2 – 7 %.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Rychlé obnovení snímku je přírůstkový snímek nebo úplné snímku?
Snímkům pořízeným jako součást funkce Rychlé obnovení jsou přírůstkových snímků.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Výpočet zvýšení přibližné náklady z důvodu funkce Rychlé obnovení
To závisí na četnosti změn virtuálního počítače. Do stabilního stavu můžete předpokládat, zvýšení nákladů je = snímku uchování období denní četnosti změn za náklady na úložiště virtuálního počítače za GB.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Pokud je typ obnovení pro bod obnovení "Snímek a trezor" a provádět operace obnovení, který typ obnovení, který se použije?
Pokud "snímek a trezor" je typ obnovení, obnovení bude automaticky provedeno z místní snímek, který se bude porovnávat mnohem rychlejší obnovení provést z trezoru.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>Co se stane, když vyberu doba uchování bodu obnovení (vrstva 2) menší než doba uchování snímků (1jádrové)?
Nový model neumožňuje odstranění bodu obnovení (úroveň 2) Pokud se odstraní snímek (1jádrové). Doporučujeme, abyste plánování doba uchování (úroveň 2) bodu obnovení delší než doba uchování snímků.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Proč je můj snímku existující i potom, co sada uchování období v zásady zálohování?
Pokud bod obnovení obsahuje snímky a, který je k dispozici nejnovější RP se uchovávají až do doby, které není další dostupnou úspěšnou zálohu. To je podle navrženého GC zásad ještě dnes, který zmocňuje nejméně jeden nejnovější předávající strana často být vždy k dispozici, v případě, že všechny zálohy, dále na selhat z důvodu chyby ve virtuálním počítači. V běžných scénářích RPs vyčistily ve maximálně 24 hodin po vypršení jejich platnosti.
