---
title: Poznámky k verzi Azure Data Box brány ve verzi Preview | Dokumentace Microsoftu
description: Popisuje důležité otevřených problémů a jejich řešení Azure Data Box brány se systémem ve verzi Preview.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: 0265de5b224e62d188fe6e3b9322d5c2e3f77fa1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883130"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Poznámky k verzi Azure Data Box brány ve verzi Preview

## <a name="overview"></a>Přehled

Následující zpráva k vydání verze Identifikujte kritické otevřené problémy a vyřešené problémy pro Microsoft Azure Data Box brány ve verzi Preview verzi.

Zpráva k vydání verze se průběžně aktualizuje, a při zjištění zásadních problémů vyžadujících alternativní řešení, se přidají. Před nasazením vaše brána pole dat, pečlivě si prostudujte informace obsažené v poznámkách k verzi.

Ve verzi Preview odpovídá verzi softwaru **Data Box brány ve verzi Preview verze 2.0**.

## <a name="issues-fixed-in-preview-release"></a>Problémy opravené ve verzi Preview

Následující tabulka obsahuje souhrn chyby opravené v této verzi.

| Ne. | Problém |
| --- | --- |
| **1.** | V této verzi Pokud je soubor, který byl nahrán vygeneroval jiný nástroj (AzCopy) aktualizovat a potom aktualizovat tak, aby zvýšení/rozšiřuje velikost souboru pak následující chyba se vyskytuje: *Chyba 400: InvalidBlobOrBlock (zadaný obsah objektu blob nebo blok je neplatný.)*|
| **2.** |Z důvodu chyby v této verzi, může se zobrazit instance s kódem chyby 110 v *error.xml* s názvy položek nelze rozpoznat. | 
| **3.** |Z důvodu chyby v této verzi může se zobrazit instance s kódem chyby 2003 při nahrávání určité soubory. | 
| **4.** |V této verzi můžete aktualizovat jenom jedna sdílená složka v čase. | 


## <a name="known-issues-in-preview-release"></a>Známé problémy ve verzi Preview

Následující tabulka obsahuje souhrn známé problémy pro bránu dat pole spuštěna ve verzi preview.

| Ne. | Funkce | Problém | Alternativní řešení a poznámky |
| --- | --- | --- | --- |
| **1.** |Aktualizace |Brána pole dat zařízení, vytvořené ve starší verzi preview verze nelze aktualizovat na tuto verzi. |Stažení Image virtuálního disku z nové verze a nakonfigurujte a nasaďte nová zařízení. Další informace najdete v části [Příprava na nasazení Azure Data Box Gateway](data-box-gateway-deploy-prep.md). |
| **2.** |Zřízené datového disku |Po zřízení některých zadané velikosti datového disku a vytvoří odpovídající pole bránu dat, nesmí zmenšovat datový disk. Probíhá pokus o zmenšit výsledky disku za následek ztrátu všech místních dat v zařízení. | |
| **3.** |Přejmenovat |Přejmenování objektů se nepodporuje. |Pokud tato funkce je důležitá pro pracovní postup, obraťte se na Microsoft Support. |
| **4.** |Kopírovat| Pokud soubor jen pro čtení je zkopírován do zařízení, nezachová se vlastnost jen pro čtení. | |
| **5.** |Typy souborů | Nejsou podporovány následující typy souborů Linux: znak soubory, soubory bloku, sokety, kanály, symbolické odkazy.  |Kopírování tyto souborů sdílet výsledky v 0-length souborech vytvářených v systému souborů NFS. Tyto soubory zůstávají v chybovém stavu a jsou rovněž uvedeny v *error.xml*. |
| **6.** |Odstranění | Z důvodu chyby v této verzi Pokud se odstraní sdílenou složku systému souborů NFS, pak sdílenou složku se možná neodstranily. Stav sdílené složky se zobrazí *odstranění*.  |K tomu dojde pouze v případě, že sdílená složka používá představuje název souboru. |
| **7.** |Obnovení | Mezi operace aktualizace není zachováno oprávnění a seznamy řízení přístupu (ACL).  | |
| **8.** |Kopírovat | Kopírování dat se nezdaří s chybou:  Požadovanou operaci nelze dokončit z důvodu omezení systému souborů.  |Tato chyba nastane, pokud Stream alternativní Data (reklamy) přidružené k souboru přesahuje 128 KB (maximální limit pro odolný systém souborů).  |
| **9.** |Symbolické odkazy |Symbolické odkazy nejsou podporovány.  |Symbolické odkazy k adresářům za následek adresáře nikdy načtení označený v režimu offline. V důsledku toho nemusí zobrazit šedé napříč na adresáře, které označuje, že adresáře jsou offline a související obsah byl zcela nahráli do Azure. |
| **10.** |Sdílené složky |Aktualizuje existující kontejner s objekty BLOB stránky do sdílené složky objektů Blob bloku (nebo naopak) vede k nahrání chyby na úpravy souboru.  |K tomuto chování dochází, když jste provedli následující kroky: <li> Vytvořte sdílenou složku objektů Blob bloku v zařízení. </li><li> Přidružení sdílené složky cloudového kontejneru, který obsahuje objekty BLOB stránky.</li><li>Obnovte tuto sdílenou složku. </li><li>Upravte některé aktualizovat soubory, které jsou už uložené jako objekty BLOB stránky v cloudu.</li> Odeslat chyby jsou vidět. |
| **11.** |Online nápověda |Odkazy nápovědy na webu Azure Portal nesmí odkazovat na dokumentaci.|Odkazy nápovědy bude fungovat ve verzi všeobecné dostupnosti. |

## <a name="next-steps"></a>Další postup

- [Příprava na nasazení Azure Data Box Gateway](data-box-gateway-deploy-prep.md).


