---
title: Konfigurace zařízení v vzdáleného monitorování řešení kurz – Azure | Dokumentace Microsoftu
description: V tomto kurzu se dozvíte, jak nakonfigurovat zařízení připojená k akcelerátoru řešení vzdáleného monitorování.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/15/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 869f6d2391632c77e01e4383c1457f88b9171c8b
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56651188"
---
# <a name="tutorial-configure-devices-connected-to-your-monitoring-solution"></a>Kurz: Konfigurace zařízení připojených k řešení monitorování

V tomto kurzu použijete akcelerátor řešení vzdáleného monitorování ke konfiguraci a správě připojených zařízení IoT. Přidat nové zařízení k akcelerátoru řešení a konfigurace zařízení.

Společnost Contoso si objednala nové stroje pro rozšíření jednoho ze svých závodů. Při čekání na doručení nových strojů budete chtít spustit simulaci a otestovat chování vašeho řešení. Při spuštění simulace, přidat nové zařízení simulovaného modul akcelerátor řešení vzdálené monitorování a test, který toto simulované zařízení správně reaguje na aktualizace konfigurace. Přestože tento kurz využívá telemetrická Simulovaná zařízení, vývojář zařízení můžete implementovat přímých metod na [skutečné zařízení připojeno k akcelerátoru řešení vzdáleného monitorování](iot-accelerators-connecting-devices.md).

V tomto kurzu se naučíte:

>[!div class="checklist"]
> * Zřízení simulovaného zařízení
> * Test simulovaného zařízení
> * Změna konfigurace zařízení
> * Uspořádání zařízení

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="add-a-simulated-device"></a>Přidání simulovaného zařízení

Přejděte **Device Explorer** stránku v řešení a potom klikněte na tlačítko **+ nové zařízení**:

[![Zřízení simulovaného zařízení](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

Na panelu **Nové zařízení** zvolte **Simulované**, počet zařízení, která se mají zřídit, ponechte na hodnotě **1**, zvolte model zařízení **Vadný motor** a pak zvolením možnosti **Použít** vytvořte simulované zařízení:

[![Zřízení simulovaného zařízení motoru](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Test simulovaného zařízení

K otestování simulované modul zařízení odesílá telemetrii a vytváření sestav hodnoty vlastností, vyberte ho v seznamu zařízení na **Device Explorer** stránky. Na panelu **Podrobnosti o zařízení** se zobrazí aktuální informace o motoru:

[![Zobrazení nového simulovaného zařízení motoru](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

V části **Podrobnosti o zařízení** ověřte, že vaše nové zařízení odesílá telemetrická data. Pokud chcete zobrazit datový proud telemetrických dat o vibracích z vašeho zařízení, klikněte na **Vibrace**:

[![Výběr zobrazení datového proudu telemetrie](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

Na panelu **Podrobnosti o zařízení** se zobrazí další informace o zařízení, jako například hodnoty značek, podporované metody a vlastnosti, které zařízení hlásí.

Pokud chcete zobrazit podrobnou diagnostiku posuňte se na panelu **Podrobnosti o zařízení** dolů do části **Diagnostika**.

## <a name="reconfigure-a-device"></a>Změna konfigurace zařízení

Pokud chcete otestovat, můžete aktualizovat vlastnosti konfigurace modul, vyberte ho v seznamu zařízení na **Device Explorer** stránky. Pak klikněte na tlačítko **úlohy**a klikněte na tlačítko **vlastnosti**. Na panelu úloh se zobrazí hodnoty vlastností pro vybrané zařízení, které můžete aktualizovat:

[![Změna konfigurace zařízení](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Pokud chcete aktualizovat umístění motoru, nastavte název úlohy na **UpdateEngineLocation**, zeměpisnou délku na **-122.15**, umístění na **Factory 2**, zeměpisnou šířku na **47.62** a klikněte na **Použít**:

[![Aktualizace hodnoty vlastnosti zařízení](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

Pokud chcete sledovat stav úlohy, klikněte na **Zobrazit stav úlohy**:

[![Aktualizace hodnoty vlastnosti zařízení](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

Po dokončení úlohy přejděte na stránku **Řídicí panel**. Zařízení motoru se na mapě zobrazí ve svém novém umístění:

[![Zobrazení umístění motoru](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Uspořádání zařízení

Pokud si jako operátor chcete usnadnit uspořádání a správu zařízení, měli byste je označit názvem týmu. Contoso má dva různé týmy starající se o aktivity služeb u zákazníků:

* Tým pro chytrá vozidla spravuje nákladní vozy a prototypy zařízení.
* Tým pro chytré budovy spravuje chladiče, výtahy a motory.

Pokud chcete zobrazit všechna svá zařízení, přejděte na **Device Explorer** stránky a zvolte **všechna zařízení** filtru:

[![Zobrazení všech zařízení](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Přidání značek

Vyberte všechny **Nákladní vozy** a **Prototypy** zařízení. Pak klikněte na **Úlohy**.

Na panelu **Úlohy** vyberte **Značka**, nastavte název úlohy na **AddConnectedVehicleTag** a pak přidejte textovou značku **FieldService** s hodnotou **ConnectedVehicle**. Pak klikněte na **Použít**:

[![Přidání značky k nákladním vozům a prototypům zařízení](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

Na stránce zařízení vyberte všechna zařízení **chladiče**, **výtahu** a **motoru**. Pak klikněte na **Úlohy**.

Na panelu **Úlohy** vyberte **Značka**, nastavte název úlohy na **AddSmartBuildingTag** a pak přidejte textovou značku **FieldService** s hodnotou **SmartBuilding**. Pak klikněte na **Použít**:

[![Přidání značky k zařízením chladiče, výtahu a motoru](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Vytváření filtrů

Hodnoty značek teď můžete využít k vytváření filtrů. Na **Device Explorer** klikněte na **Správa skupin zařízení**:

[![Správa skupin zařízení](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

Vytvořte filtr textu, v jehož podmínce se používá značka s názvem **FieldService** a hodnotou **SmartBuilding**. Uložte filtr jako **Chytrá budova**:

[![Vytvoření filtru chytrých budov](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

Vytvořte filtr textu, v jehož podmínce se používá značka s názvem **FieldService** a hodnotou **ConnectedVehicle**. Uložte filtr jako **Připojené vozidlo**.

[![Vytvoření filtru připojených vozidel](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Operátor společnosti Contoso teď může zadávat dotazy na zařízení na základě operačního týmu:

[![Vytvoření filtru připojených vozidel](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se dozvěděli, jak konfigurovat a spravovat zařízení připojená k akcelerátoru řešení vzdáleného monitorování. Pokud se chcete naučit, jak používat akcelerátor řešení k provádění analýz původních příčin neočekávaného upozornění, pokračujte k dalšímu kurzu.

> [!div class="nextstepaction"]
> [Analýza původní příčiny pro upozornění](iot-accelerators-remote-monitoring-root-cause-analysis.md)
