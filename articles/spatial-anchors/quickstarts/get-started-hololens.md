---
title: Rychlý start – vytvoření aplikace HoloLens s Azure prostorových kotvy | Dokumentace Microsoftu
description: V tomto rychlém startu se dozvíte, jak sestavit aplikaci HoloLens pomocí prostorových ukotvení.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: a0e34ad8847ed3740af72b4c27dfbc0090cf3dfa
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752184"
---
# <a name="quickstart-create-a-hololens-app-with-azure-spatial-anchors-in-cwinrt-and-directx"></a>Rychlý start: Vytvoření aplikace HoloLens s Azure prostorových kotev vztahů v jazyce C + +/ WinRT a DirectX

Tento rychlý start popisuje, jak vytvořit aplikaci pomocí HoloLens [prostorových kotvy Azure](../overview.md) v jazyce C + +/ WinRT a rozhraní DirectX. Azure prostorových kotvy je služba napříč platformami pro vývojáře, která vám umožní vytvořit prostředí hybridní realita s využitím objektů, které se zachovávají jejich umístění na zařízeních v čase. Jakmile budete hotovi, budete mít aplikaci HoloLens, který můžete uložit a odvolat prostorových ukotvení.

Dozvíte se, jak provést tyto akce:

> [!div class="checklist"]
> * Vytvoření účtu prostorových kotvy
> * Konfigurace prostorový kotvy účtu identifikátor a klíč účtu
> * Nasazení a spuštění na zařízení HoloLens

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý start, ujistěte se, že máte následující:

- Počítače s Windows s <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 +</a> součástí **vývoj pro univerzální platformu Windows** pracovního vytížení a **Windows 10 SDK (10.0.17763.0 nebo novější)** komponenta.
- [C + +/ WinRT rozšíření aplikace Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix) pro Visual Studio musí nainstalovat z [Visual Studio Marketplace](https://marketplace.visualstudio.com/).
- Zařízení HoloLens s [vývojářský režim](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) povolena. Tento článek vyžaduje zařízení HoloLens se [Windows 10. října 2018 Update](https://blogs.windows.com/windowsexperience/2018/10/02/find-out-whats-new-in-windows-and-office-in-october/) (označované také jako RS5). Chcete-li aktualizovat na nejnovější verzi na HoloLens, otevřete **nastavení** aplikaci, přejděte na **aktualizace a zabezpečení**, vyberte **vyhledat aktualizace** tlačítko.
- Aplikaci musíte nastavit **spatialPerception** schopností ve svém manifestu AppX.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Otevřete ukázkový projekt

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Otevřít `HoloLens\DirectX\SampleHoloLens.sln` v sadě Visual Studio.

## <a name="configure-account-identifier-and-key"></a>Nakonfigurujte identifikátor účtu a klíč

Dalším krokem je konfigurace aplikace použijte identifikátor účtu a klíč účtu zaznamenané dříve při nastavování zdroje prostorových ukotvení.

Otevřít `HoloLens\DirectX\SampleHoloLens\ViewController.cpp`.

Vyhledejte `SpatialAnchorsAccountKey` pole a nahraďte `Set me` klíčem účtu.

Vyhledejte `SpatialAnchorsAccountId` pole a nahraďte `Set me` s identifikátor účtu.

## <a name="deploy-the-app-to-your-hololens"></a>Nasazení aplikace na vašich HoloLens

Změnit **konfigurace řešení** k **vydání**, změňte **platforma řešení** k **x86**a vyberte **zařízení**  z možnosti cíl nasazení.

![Visual Studio Configuration](./media/get-started-hololens/visual-studio-configuration.png)

Zapne zařízení HoloLens, přihlaste a připojte ho k počítači pomocí kabelu USB.

Vyberte **ladění** > **spustit ladění** k nasazení vaší aplikace a spusťte ladění.

Postupujte podle pokynů v aplikaci umístit a odvolat ukotvení.

V sadě Visual Studio, zastavte aplikaci tak, že vyberete **Zastavit ladění** nebo stiskněte **Shift + F5**.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Kurz: Sdílená složka prostorových kotvy napříč zařízeními](../tutorials/tutorial-share-anchors-across-devices.md)
