---
title: Vytváření a spouštění úloh v aplikaci Azure IoT Central | Dokumentace Microsoftu
description: Azure IoT Central úlohy umožňují hromadně možnosti správy zařízení, jako je například aktualizace vlastností zařízení, nastavení nebo spuštění příkazu.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 02/04/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: e418ec7d22622c341abd972763d78ac2f0df46d9
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773247"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Vytvoření a spuštění úlohy v aplikaci Azure IoT Central

Microsoft Azure IoT Central můžete použít ke správě připojených zařízení ve velkém měřítku pomocí úlohy. Funkce úlohy umožňuje provádět hromadné aktualizace vlastností zařízení, nastavení a příkazy. Tento článek vás provede tom, jak začít používat úlohy ve své aplikaci.

## <a name="create-and-run-a-job"></a>Vytvoření a spuštění úlohy

V této části se dozvíte, jak vytvořit a spustit úlohu. Každý krok prochází příklad, který ukazuje, jak spustit úlohu pro Automat chladicí zařízení, které potřebují mít zvýšit rychlost uvádění ventilátor.

1. V navigačním podokně přejděte do úlohy.

1. Klikněte na tlačítko **+ nová** zahájíte vytváření nové úlohy.

    ![Vytvoření nové úlohy](./media/howto-run-a-job-experimental/createnewjob.png)

1. Zadejte název a popis, který vám pomůže identifikovat úlohu, kterou vytváříte.

1. Vyberte sadu zařízení chcete použít pro vaše úloha. Po výběru zařízení nastavení, zobrazí se vám na pravé straně naplnit zařízení v rámci sady vybraných zařízení. Pokud vyberete sada přerušeno zařízení, žádná zařízení se zobrazí a zobrazí se zpráva s vysvětlením, že vaše zařízení sada je přerušeno.

1. Potom vyberte typ úlohy, které se bude definován (nastavení, vlastnost nebo příkaz). Klikněte na tlačítko **+** vedle typu úlohy vybraná a přidat požadované operace.

    ![Konfigurace úlohy](./media/howto-run-a-job-experimental/configurejob.png)

1. Na pravé straně zvolte byste chtěli spouštět úlohy zařízení. Klepnutím na horní zaškrtávací políčko, jsou vybrány všechna zařízení v sadě celého zařízení. Kliknutím zaškrtněte políčko vedle názvu jsou vybrány všechna zařízení na aktuální stránce.

1. Jakmile požadované zařízení jste vybrali, zvolte **spustit**. Úloha se teď budou zobrazovat na vaší hlavní **úlohy** stránky. V tomto zobrazení budete moct zobrazit aktuálně spuštěné úlohy a historii některého dříve spouštění úloh. Spuštěná úloha se vždy zobrazí v horní části seznamu.

    ![Zobrazit úlohu](./media/howto-run-a-job-experimental/viewjob.png)

    > [!NOTE]
    > Bude moct zobrazit historie dřív spuštěných úloh po dobu až 30 dnů.

1. Pokud chcete získat přehled o vaší úlohy, klikněte na název úlohy, které chcete zobrazit v seznamu. Tento přehled obsahuje podrobnosti o úloze, zařízení a stavy zařízení.

    ![Zobrazení stavu zařízení](./media/howto-run-a-job-experimental/viewdevicestatus.png)

### <a name="stop-a-running-job"></a>Zastavit spuštěné úlohy

Pokud chcete zastavit úlohu, která je aktuálně spuštěna, klikněte na název spuštěné úlohy, která byste chtěli zastavit. Zvolte **Zastavit** tlačítko na panelu. Uvidíte, že se že stav úlohy změnilo tak, aby odrážely, úloha se zastavila.

   ![Zastavit úlohu](./media/howto-run-a-job-experimental/stopjob.png)

### <a name="run-a-stopped-job"></a>Spustit úlohu zastaveno

Pokud chcete spustit úlohu, která je nyní zastavena, klikněte na název zastavenou úlohu, která chcete spustit. Zvolte **spustit** tlačítko na panelu. Uvidíte, že stav úlohy se změnila tak, aby odrážely, že úloha je nyní spuštěna znovu.

   ![Obnovit úlohy](./media/howto-run-a-job-experimental/resumejob.png)

## <a name="view-the-job-status"></a>Zobrazení stavu úlohy

Po vytvoření úlohy **stav** sloupec aktualizují a zobrazí se nejnovější stavová zpráva úlohy. Stavové zprávy znamenají tohle:

| Zpráva o stavu       | Stav význam                                          |
| -------------------- | ------------------------------------------------------- |
| Dokončeno            | Tato úloha byla provedena na všech zařízeních.              |
| Selhalo               | Tato úloha má se nezdařilo a není plně proveden v zařízení.  |
| Čekající na vyřízení              | Tato úloha ještě nezačala provádění na zařízení.        |
| Spuštěno              | Tato úloha právě probíhá na zařízeních.             |
| Zastaveno              | Tato úloha byla ručně zastavena uživatelem.           |

Ve zprávě o stavu Následuje přehled zařízení v rámci úlohy. Tyto stavy zařízení znamenají tohle:

| Zpráva o stavu       | Stav význam                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Úspěch            | Počet zařízení, která úlohy byl úspěšně proveden na.  |
| Selhalo               | Počet zařízení, která úloha se nezdařila se promítnou u.      |

### <a name="view-the-device-status"></a>Zobrazení stavu zařízení

Chcete-li zobrazit stav jednotlivých zařízení v rámci úlohy, klikněte na název úlohy. Zde uvidíte podrobnosti o úloze a všechna zařízení, které byly částí této konkrétní úlohy. Vedle názvu každé zařízení zobrazí se vám jeden z následujících zpráv:

| Zpráva o stavu       | Stav význam                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Dokončeno            | Úloha byla provedena na tomto zařízení.                                     |
| Selhalo               | Úlohu se nepodařilo spustit na tomto zařízení. Doprovodné chybové zprávě se zobrazí další informace.  |
| Čekající na vyřízení              | Úloha ještě neprovedlo na tomto zařízení.                                  |

> [!NOTE]
> Pokud zařízení byla odstraněna, nebudete moci vybrat zařízení a zobrazí se jako odstraněné s ID zařízení.

## <a name="next-steps"></a>Další postup

Teď, když jste se naučili, jak vytvářet úlohy v aplikaci Azure IoT Central, tady jsou některé další kroky:

- [Použití sad zařízení](howto-use-device-sets-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Správa zařízení](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Verze šablony zařízení](howto-version-devicetemplate.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
