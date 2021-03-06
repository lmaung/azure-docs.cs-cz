---
title: SQL Database zotavení po havárii | Dokumentace Microsoftu
description: Přečtěte si pokyny a osvědčené postupy při používání Azure SQL Database provést zotavení po havárii.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 5d754ae558d485036a9a55f573a3f40162ed9f84
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2019
ms.locfileid: "55561123"
---
# <a name="performing-disaster-recovery-drill"></a>Provedení postupu zotavení po havárii

Doporučuje se, že se pravidelně provádí ověření připravenosti aplikace pro pracovní postup obnovení. Ověřuje se chování aplikace a důsledky ztrátě dat a/nebo narušení zahrnuje tohoto převzetí služeb při selhání je osvědčené technické praxe. Je také požadavek většina oborových standardů jako součást Certifikační obchodní kontinuity podnikových procesů.

Provedení postupu zotavení po havárii zahrnuje:

* Simulaci výpadku úroveň dat
* Obnovování
* Ověření obnovení příspěvek integrity aplikace

V závislosti na tom, jak můžete [určená aplikace pro zajištění kontinuity](sql-database-business-continuity.md), pracovní postup ke spuštění na postup zotavení se může lišit. Tento článek popisuje osvědčené postupy pro provádění zotavení po havárii v rámci služby Azure SQL Database.

## <a name="geo-restore"></a>Geografické obnovení

Aby se zabránilo ztrátě dat při provedení postupu zotavení po havárii, proveďte přejít pomocí testovacího prostředí vytvořit kopii produkčního prostředí a pomocí ověření pracovního postupu převzetí služeb při selhání aplikace.

### <a name="outage-simulation"></a>Simulaci výpadku

Pro simulaci výpadek, můžete přejmenovat zdrojové databáze. Tato změna názvu způsobí selhání připojení k aplikaci.

### <a name="recovery"></a>Obnovení

* Provést geografické obnovení databáze do jiného serveru, jak je popsáno [tady](sql-database-disaster-recovery.md).
* Změna konfigurace aplikace pro připojení k obnovené databázi a postupujte podle [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) příručka k dokončení obnovení.

### <a name="validation"></a>Ověření

Dokončení přejít pomocí ověření obnovení příspěvek integrity aplikace (včetně připojovacích řetězců, přihlášení, základní funkce testování nebo druhé ověření postupů signoffs standardní aplikace).

## <a name="failover-groups"></a>Skupiny převzetí služeb při selhání

Pro databáze, který je chráněný pomocí skupin převzetí služeb při selhání zahrnuje podrobné cvičení plánované převzetí služeb při selhání na sekundární server. Plánované převzetí služeb při selhání zajistí, že primární a sekundární databází ve skupině převzetí služeb při selhání zůstávají synchronizované při přepínání role. Na rozdíl od neplánované převzetí služeb při selhání tato operace nemá za následek ztrátu dat, tak na postup zotavení lze provádět v produkčním prostředí.

### <a name="outage-simulation"></a>Simulaci výpadku

Pro simulaci výpadek, můžete zakázat webové aplikace nebo virtuálního počítače připojené k databázi. Pro klienty webového způsobí selhání připojení k této simulaci výpadku.

### <a name="recovery"></a>Obnovení

* Ujistěte se, že konfigurace aplikace v oblasti bodů zotavení po Havárii do sekundární, Bývalé, který bude plně přístupné nový primární.
* Zahájit [plánované převzetí služeb při selhání](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) skupiny převzetí služeb při selhání ze sekundárního serveru.
* Postupujte podle [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) příručka k dokončení obnovení.

### <a name="validation"></a>Ověření

Dokončení přejít pomocí ověření obnovení příspěvek integrity aplikace (včetně připojení, základní funkce testování nebo další ověření vyžaduje pro přechod signoffs).

## <a name="next-steps"></a>Další postup

* Další informace o scénářích obchodní kontinuity podnikových procesů najdete v tématu [scénáře kontinuitu podnikových procesů](sql-database-business-continuity.md).
* Další informace o Azure SQL Database, automatické zálohování, naleznete v tématu [automatické zálohování SQL Database](sql-database-automated-backups.md)
* Další informace o obnovení pomocí automatizovaného zálohování, naleznete v tématu [obnovení databáze ze záloh spouštěných službou](sql-database-recovery-using-backups.md).
* Další informace o možnosti rychlejší obnovení najdete v tématu [aktivní geografickou replikaci](sql-database-active-geo-replication.md) a [-automatické převzetí služeb při selhání skupiny](sql-database-auto-failover-group.md).
