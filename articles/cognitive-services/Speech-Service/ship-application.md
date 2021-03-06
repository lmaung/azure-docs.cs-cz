---
title: Vyvíjejte aplikace s rozhraním Speech SDK – hlasové služby
titleSuffix: Azure Cognitive Services
description: Zjistěte, jak vytvářet aplikace s využitím sadou SDK pro řeč.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: f3522f065d22ce276174fbd165c37df3914e32b9
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56962022"
---
# <a name="ship-an-application"></a>Dodávejte aplikace

Podívejte se [sadou SDK pro řeč licence](https://aka.ms/csspeech/license201809), stejně jako [oznámení k softwaru třetích stran](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html) při distribuci Azure Cognitive Services sadou SDK pro řeč. Projděte si také, [prohlášení o ochraně osobních údajů Microsoft](https://aka.ms/csspeech/privacy).

V závislosti na platformě existují různých závislostí ke spuštění vaší aplikace.

## <a name="windows"></a>Windows

Cognitive Services SDK řeči je testován na Windows 10 a Windows serveru 2016.

Cognitive Services řeči SDK vyžaduje [Microsoft Visual C++ Redistributable pro Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) v systému. Instalační programy pro nejnovější verzi si můžete stáhnout `Microsoft Visual C++ Redistributable for Visual Studio 2017` tady:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Pokud vaše aplikace používá spravovaného kódu `.NET Framework 4.6.1` nebo novější je nutné na cílovém počítači.

U mikrofon vstup je potřeba nainstalovat knihovny Media Foundation. Tyto knihovny jsou součástí sady Windows 10 a Windows serveru 2016. Mikrofon se nepoužívá jako vstupní zvuková zařízení, je možné použít sadu SDK pro řeč bez těchto knihoven.

Požadované sadou SDK pro řeč soubory je možné nasadit ve stejném adresáři jako vaši aplikaci. Tímto způsobem vaší aplikace můžete přistupovat přímo na knihovny. Ujistěte se, zda že jste vybrali správnou verzi (Win32/x64), která odpovídá vaší aplikace.

| Název | Funkce
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core SDK pro nativní a spravovaná nasazení
| `Microsoft.CognitiveServices.Speech.csharp.dll` | vyžaduje se pro spravované nasazení

>[!NOTE]
> Od verze 1.3.0 soubor `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (k dispozici v předchozích verzích) už není potřeba. Funkce je teď integrovaná v core SDK.

## <a name="linux"></a>Linux

Sadou SDK pro řeč aktuálně podporuje Ubuntu 16.04 a 18.04 distribucí.
Pro nativní aplikaci, je třeba dodávat knihovně sadou SDK pro řeč `libMicrosoft.CognitiveServices.Speech.core.so`.
Ujistěte se, zda že jste vybrali verzi (x86, x64), která odpovídá vaší aplikace. V závislosti na verzi systému Linux můžete také potřebovat zahrnout tyto závislosti:

* Sdílené knihovny knihovna GNU C (včetně knihovny POSIX vlákna programování `libpthreads`)
* Knihovny OpenSSL (`libssl.so.1.0.0`)
* Sdílená knihovna pro aplikace ALSA (`libasound.so.2`)

V Ubuntu by měl knihovny GNU C již nainstalován ve výchozím nastavení. Poslední tři lze nainstalovat pomocí těchto příkazů:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2 wget
```

## <a name="next-steps"></a>Další postup

* [Získání zkušebního předplatného služby Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Zjistěte, jak rozpoznávat řeč v jazyce C#](quickstart-csharp-dotnet-windows.md)
