---
title: Rychlý start k řízení zařízení ze služby Azure IoT Hub (Java) | Microsoft Docs
description: V tomto rychlém startu spustíte dvě ukázkové aplikace Java. První aplikace je back-endová aplikace, která může vzdáleně řídit zařízení připojená k vašemu centru. Druhá aplikace simuluje zařízení připojené k vašemu centru, které je možné řídit vzdáleně.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/22/2019
ms.openlocfilehash: 82586838c9b442ce27d6a18f45fb93370e532701
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011564"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-java"></a>Rychlý start: Řízení zařízení připojená ke službě IoT hub (Java)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub je služba Azure, která umožňuje ingestovat velké objemy telemetrických dat ze zařízení IoT do cloudu a spravovat zařízení z cloudu. V tomto rychlém startu použijete *přímou metodu* k řízení simulovaného zařízení připojeného k centru IoT. Přímé metody můžete použít k provádění vzdálených změn chování zařízení připojeného k centru IoT.

Rychlý start používá dvě předem napsané aplikace Java:

* Aplikaci simulovaného zařízení, která odpovídá na přímé metody volané z back-endové aplikace. Aby bylo možné přijímat volání přímé metody, připojí se tato aplikace ke koncovému bodu centra IoT pro konkrétní zařízení.

* Back-endovou aplikaci, která na simulovaném zařízení volá přímé metody. Aby na zařízení bylo možné volat přímou metodu, připojí se tato aplikace ke koncovému bodu vašeho centra IoT na straně služby.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Dvě ukázkové aplikace, které spustíte v tomto rychlém startu, jsou napsány pomocí Javy. Na vývojovém počítači musíte mít Java SE 8 nebo novější.

Javu pro různé platformy si můžete stáhnout z webu [Oracle](https://aka.ms/azure-jdks).

Aktuální verzi Javy na vývojovém počítači můžete ověřit pomocí následujícího příkazu:

```cmd/sh
java -version
```

K sestavení ukázek potřebujete nainstalovat Maven 3. Maven pro různé platformy si můžete stáhnout z webu [Apache Maven](https://maven.apache.org/download.cgi).

Aktuální verzi Mavenu na vývojovém počítači můžete ověřit pomocí následujícího příkazu:

```cmd/sh
mvn --version
```

Pokud jste to ještě neudělali, stáhněte si ukázkový projekt Java z webu https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip a extrahujte archiv ZIP.

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

Pokud jste dokončili předchozí [rychlý start: Odesílání telemetrických dat ze zařízení do služby IoT hub](quickstart-send-telemetry-java.md), můžete tento krok přeskočit.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrování zařízení

Pokud jste dokončili předchozí [rychlý start: Odesílání telemetrických dat ze zařízení do služby IoT hub](quickstart-send-telemetry-java.md), můžete tento krok přeskočit.

Zařízení musí být zaregistrované ve vašem centru IoT, aby se mohlo připojit. V tomto rychlém startu zaregistrujete simulované zařízení pomocí služby Azure Cloud Shell.

1. Ve službě Azure Cloud Shell spusťte následující příkazy pro přidání rozšíření rozhraní příkazového řádku IoT Hub a vytvoření identity zařízení.

   **YourIoTHubName**: Nahraďte tento zástupný text pod názvem, který jste zvolili pro službu IoT hub.

   **MyJavaDevice**: Název zařízení, které se registrace. Použití **MyJavaDevice** jak je znázorněno. Pokud zvolíte jiný název pro vaše zařízení, musíte použít tento název v rámci tohoto článku a aktualizujte název zařízení v ukázkové aplikace před spuštěním je.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyJavaDevice
    ```

2. Spuštěním následujícího příkazu ve službě Azure Cloud Shell získejte _připojovací řetězec zařízení_ pro zařízení, které jste právě zaregistrovali:

   **YourIoTHubName**: Nahraďte tento zástupný text pod názvem, který jste vybrali pro službu IoT hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyJavaDevice \
      --output table
    ```

    Poznamenejte si připojovací řetězec zařízení, který vypadá nějak takto:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Tuto hodnotu použijete později v tomto rychlém startu.

## <a name="retrieve-the-service-connection-string"></a>Načtení připojovacího řetězce služby

Potřebujete také _připojovací řetězec služby_, který back-endové aplikaci umožní připojení k vašemu centru IoT a načtení zpráv. Následující příkaz načte připojovací řetězec služby pro vaše centrum IoT:

**YourIoTHubName**: Nahraďte tento zástupný text pod názvem, který jste zvolili pro službu IoT hub.

```azurecli-interactive
az iot hub show-connection-string --hub-name YourIoTHubName --output table
```

Poznamenejte si připojovací řetězec služby, který vypadá nějak takto:

`HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={YourSharedAccessKey}`

Tuto hodnotu použijete později v tomto rychlém startu. Připojovací řetězec služby se liší od připojovacího řetězce zařízení.

## <a name="listen-for-direct-method-calls"></a>Naslouchání voláním přímé metody

Aplikace simulovaného zařízení se připojí ke koncovému bodu v centru IoT pro konkrétní zařízení, odešle simulovaná telemetrická data a z vašeho centra naslouchá voláním přímé metody. Volání přímé metody z centra v tomto rychlém startu nařídí zařízení, aby změnilo interval, ve kterém se odesílají telemetrická data. Simulované zařízení odešle po spuštění přímé metody zpět do centra potvrzení.

1. V okně místního terminálu přejděte do kořenové složky ukázkového projektu Java. Potom přejděte do složky **iot-hub\Quickstarts\simulated-device-2**.

2. V libovolném textovém editoru otevřete soubor **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java**.

    Hodnotu proměnné `connString` nahraďte připojovacím řetězcem zařízení, který jste si předtím poznamenali. Změny pak uložte do souboru **SimulatedDevice.java**.

3. V okně místního terminálu pomocí následujících příkazů nainstalujte požadované knihovny a sestavte aplikaci simulovaného zařízení:

    ```cmd/sh
    mvn clean package
    ```

4. Spuštěním následujících příkazů v okně místního terminálu spusťte aplikaci simulovaného zařízení:

    ```cmd/sh
    java -jar target/simulated-device-2-1.0.0-with-deps.jar
    ```

    Následující snímek obrazovky ukazuje výstup, zatímco aplikace simulovaného zařízení odesílá telemetrická data do vašeho centra IoT:

    ![Spuštění simulovaného zařízení](./media/quickstart-control-device-java/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Volání přímé metody

Back-endová aplikace se připojí ke koncovému bodu vašeho centra IoT na straně služby. Aplikace provádí volání přímé metody na zařízení prostřednictvím centra IoT a čeká na potvrzení. Back-endová aplikace služby IoT Hub se obvykle spouští v cloudu.

1. V jiném okně místního terminálu přejděte do kořenové složky ukázkového projektu Java. Potom přejděte do složky **iot-hub\Quickstarts\back-end-application**.

2. V libovolném textovém editoru otevřete soubor **src/main/java/com/microsoft/docs/iothub/samples/BackEndApplication.java**.

    Hodnotu proměnné `iotHubConnectionString` nahraďte připojovacím řetězcem služby, který jste si předtím poznamenali. Změny pak uložte do souboru **BackEndApplication.java**.

3. V okně místního terminálu pomocí následujících příkazů nainstalujte požadované knihovny a sestavte back-endovou aplikaci:

    ```cmd/sh
    mvn clean package
    ```

4. Spuštěním následujících příkazů v okně místního terminálu spusťte back-endovou aplikaci:

    ```cmd/sh
    java -jar target/back-end-application-1.0.0-with-deps.jar
    ```

    Následující snímek obrazovky ukazuje výstup, zatímco aplikace provádí volání přímé metody na zařízení a obdrží potvrzení:

    ![Spuštění back-endové aplikace](./media/quickstart-control-device-java/BackEndApplication.png)

    Po spuštění back-endové aplikace se v okně konzoly se simulovaným zařízením zobrazí zpráva a rychlost odesílání zpráv se změní:

    ![Změna simulovaného klienta](./media/quickstart-control-device-java/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Další postup

V tomto rychlém startu volat přímé metody v zařízení z aplikace back-end a přidal odpověď na volání přímé metody v aplikaci simulovaného zařízení.

Informace o tom, jak směrovat zprávy typu zařízení-cloud do různých cílů v cloudu, najdete v dalším kurzu.

> [!div class="nextstepaction"]
> [Kurz: Telemetrická data trasy pro různé koncové body pro zpracování](tutorial-routing.md)