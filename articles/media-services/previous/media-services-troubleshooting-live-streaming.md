---
title: Průvodce odstraňováním potíží pro živé streamování | Dokumentace Microsoftu
description: Toto téma nabízí návrhy na řešení problémů živého streamování.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: 8bb48044fc827cd0d1dbc11ef3ec72ca1bdcb11a
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997529"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Průvodce řešením potíží s živým streamováním  

Tento článek obsahuje návrhy, jak řešit potíže živého streamování.

## <a name="issues-related-to-on-premises-encoders"></a>Problémy související s místní kodéry
Tato část poskytuje návrhy, jak řešit problémy související s místních kodérů, které jsou konfigurované k odesílání datového proudu s jednou přenosovou rychlostí do AMS kanálů, které jsou povolené kódování v reálném čase.

### <a name="problem-would-like-to-see-logs"></a>Problém: Chcete zobrazit protokoly
* **Potenciální problém**: Nelze najít kodér protokoly, které mohou být užitečné při ladění problémů.
  
  * **Telestream Wirecast**: Obvykle lze najít protokoly pod C:\Users\{uživatelské jméno} \AppData\Roaming\Wirecast\ 
  * **Elemental Live**: Můžete najít na portálu pro správu obsahuje odkazy na protokoly. Klikněte na **statistiky**, pak **protokoly**. Na **soubory protokolů** stránky, zobrazí se seznam protokolů pro všechny položky Livestream; vyberte odpovídající vaší aktuální relace. 
  * **Flash Media Live Encoder**: Můžete najít **adresář protokolu...**  tak, že přejdete na **kódování protokolu** kartu.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problém: Neexistuje žádná možnost pro výstup progresivní datového proudu
* **Potenciální problém**: Kodér používá nebude automaticky zrušit prokládání. 
  
    **Řešení potíží**: Vyhledejte odstraňování prokládání možnost v rámci rozhraní kodér. Po povolení zrušení prokládání znovu zkontrolujte nastavení progresivní výstup. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problém: Pokusili několik nastavení výstupní kodér a ani nadále nejde navázat připojení.
* **Potenciální problém**: Azure kódování kanálu nebylo resetováno správně. 
  
    **Řešení potíží**: Ujistěte se, že kodér je už doručením (push) do AMS, zastavte a znovu nastavit kanál. Po spuštění znovu, zkuste se připojit váš kodér s novým nastavením. Pokud to problém stále nevyřeší, zkuste si vytvořit zcela nový kanál, někdy kanálů může být poškozený, po několika neúspěšných pokusech o.  
* **Potenciální problém**: Nejsou optimální velikost GOP nebo nastavení klíčových snímků. 
  
    **Řešení potíží**: Doporučená velikost nebo klíčového snímku interval GOP je 2 sekundy. Některé kodérů vypočítat toto nastavení v počet snímků, zatímco v jiných sekund. Příklad: Při výstupu 30 snímků za sekundu, bude velikost GOP 60 snímků, což je totéž jako 2 sekundy.  
* **Potenciální problém**: Uzavřené porty blokují datového proudu. 
  
    **Řešení potíží**: Při streamování přes RTMP, zkontrolujte nastavení brány firewall nebo proxy serveru pro potvrzení, že jsou otevřené odchozí porty 1935 a 1936. 

> [!NOTE]
> Pokud po provedení kroků řešení potíží, které stále nelze použít datový proud úspěšně, vyplňte lístek podpory na webu Azure portal.
> 
> 

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

