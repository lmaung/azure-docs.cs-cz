---
title: Kurz vytvoření vlastní C# modul – Azure IoT Edge | Dokumentace Microsoftu
description: V tomto kurzu se dozvíte, jak vytvořit modul IoT Edge s kódem v jazyce C# a jak ho nasadit na hraniční zařízení.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 21f59ad03f2ff91b3c33675b0994b7f565bf5031
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2019
ms.locfileid: "55563197"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Kurz: Vývoj C# modul IoT Edge a nasazení simulovaného zařízení

Moduly Azure IoT Edge můžete použít k nasazení kódu, který implementuje obchodní logiku přímo do zařízení IoT Edge. Tento kurz vás povede při vytvoření a nasazení modulu IoT Edge, který filtruje data ze senzoru. Budete používat simulované zařízení IoT Edge, které jste vytvořili v rychlých startech o nasazení Azure IoT Edge na simulované zařízení ve [Windows](quickstart.md) nebo [Linuxu](quickstart-linux.md). V tomto kurzu se naučíte:    

> [!div class="checklist"]
> * Použití Visual Studio Code k vytvoření modulu IoT Edge, která je založena na sadě SDK .NET Core 2.1.
> * Používat Visual Studio Code a Docker k vytvoření image Dockeru a jejímu publikování v registru.
> * Nasadit modul do zařízení IoT Edge.
> * Zobrazit vygenerovaná data.

>[!NOTE]
>Můžete také použít [Visual Studio 2017 vyvíjet, ladit a nasadit moduly IoT Edge](how-to-visual-studio-develop-csharp-module.md).

Modul IoT Edge, který v tomto kurzu vytvoříte, filtruje teplotní údaje generované zařízením. Zprávy posílá dále, jen když teplota překročí zadanou prahovou hodnotu. Tento typ analýzy v zařízení Edge je užitečný kvůli zmenšení objemu dat přenášených a ukládaných do cloudu. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Požadavky

Zařízení Azure IoT Edge:

* Jako hraniční zařízení můžete použít svůj vývojový počítač nebo virtuální počítač podle postupu v rychlém startu pro zařízení s [Linuxem](quickstart-linux.md) nebo [Windows](quickstart.md).

Cloudové prostředky:

* [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) úrovně Free nebo Standard v Azure. 

Prostředky pro vývoj:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Rozšíření jazyka C# pro Visual Studio Code (využívající OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* [Nástroje Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) pro Visual Studio Code. 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/).


## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

V tomto kurzu se pomocí nástroje Azure IoT pro Visual Studio Code sestavíte modul a vytvořte **image kontejneru** ze souborů. Tuto image pak nasdílíte do **registru**, ve kterém se ukládají a spravují vaše image. Nakonec nasadíte svou image z registru pro spuštění na zařízení IoT Edge.  

Pro uložení imagí kontejnerů, můžete použít jakýkoli registr kompatibilní s Dockerem. Jsou dvě oblíbené služby registrů Dockeru [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) a [Docker Hubu](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). V tomto kurzu se používá služba Azure Container Registry. 

Pokud ještě nemáte registr kontejnerů, postupujte podle těchto kroků a vytvořte nový v Azure:

1. Na webu [Azure Portal](https://portal.azure.com) vyberte **Vytvořit prostředek** > **Kontejnery** > **Container Registry**.

2. Zadejte následující hodnoty pro vytvoření registru kontejneru:

   | Pole | Hodnota | 
   | ----- | ----- |
   | Název registru | Zadejte jedinečný název. |
   | Předplatné | V rozevíracím seznamu vyberte předplatné. |
   | Skupina prostředků | Pro všechny testovací prostředky, které vytvoříte v průběhu rychlých startů a kurzů pro IoT Edge, doporučujeme použít stejnou skupinu prostředků. Například **IoTEdgeResources**. |
   | Umístění | Zvolte umístění, které je blízko vás. |
   | Uživatel s rolí správce | Nastavte na **Povolit**. |
   | Skladová jednotka (SKU) | Vyberte **Basic**. | 

5. Vyberte **Vytvořit**.

6. Po vytvoření registru kontejneru do něj přejděte a vyberte **Přístupové klíče**. 

7. Zkopírujte hodnoty pro **Přihlašovací server**, **Uživatelské jméno** a **Heslo**. Tyto hodnoty použijete v pozdější části kurzu a zajistit tak přístup do registru kontejneru. 

## <a name="create-an-iot-edge-module-project"></a>Vytvoření projektu modulu IoT Edge
Následujícím postupem se vytvoří projekt modul IoT Edge, které je založené na .NET Core 2.0 SDK pomocí Visual Studio Code a nástroje Azure IoT.

### <a name="create-a-new-solution"></a>Vytvoření nového řešení

Vytvořte šablonu řešení v jazyce C#, kterou můžete přizpůsobit pomocí vlastního kódu. 

1. V nástroji Visual Studio Code zvolte **View** (Zobrazit)  > **Command Palette** (Paleta příkazů). Otevře se paleta příkazů VS Code. 

2. V paletu příkazů zadejte a spusťte příkaz **Azure: Přihlaste se** a postupujte podle pokynů k přihlášení účtu Azure. Pokud už přihlášení jste, můžete tento krok přeskočit.

3. V paletu příkazů zadejte a spusťte příkaz **Azure IoT Edge: Nové řešení IoT Edge**. Vytvořte řešení podle výzev, které se zobrazí na paletě příkazů.

   | Pole | Hodnota |
   | ----- | ----- |
   | Vyberte složku | Zvolte umístění na vývojovém počítači, ve kterém VS Code vytvoří soubory řešení. |
   | Zadejte název řešení | Zadejte popisný název pro vaše řešení nebo přijměte výchozí **EdgeSolution**. |
   | Vyberte šablonu modulu | Zvolte  **C# modulu**. |
   | Zadejte název modulu | Dejte modulu název **CSharpModule**. |
   | Zadejte pro modul úložiště imagí Dockeru | Úložiště imagí zahrnuje název registru kontejneru a název image kontejneru. Svou image kontejneru je předem z názvu, který jste zadali v předchozím kroku. Nahraďte **localhost:5000** hodnotou přihlašovacího serveru z vašeho registru kontejneru Azure. Přihlašovací server můžete získat na stránce Přehled vašeho registru kontejneru na webu Azure Portal. <br><br>Finální bitové kopie úložiště bude vypadat jako \<název registru\>.azurecr.io/csharpmodule. |
 
   ![Zadání úložiště imagí Dockeru](./media/tutorial-csharp-module/repository.png)

V okně nástroje VS Code se načte pracovní prostor řešení IoT Edge. Pracovní prostor řešení obsahuje pět komponent nejvyšší úrovně. Složka **modules** obsahuje kód jazyka C# pro váš modul a také soubory Dockerfile pro sestavení modulu jako image kontejneru. V souboru **\.env** jsou uložené přihlašovací údaje k vašemu registru kontejneru. Soubor **deployment.template.json** obsahuje informace, které modul runtime IoT Edge používá k nasazení modulů do zařízení. A **deployment.debug.template.json** souboru kontejnery ladicí verzi modulů. Složku **\.vscode** ani soubor **\.gitignore** v tomto kurzu upravovat nebudete.

Pokud jste při vytváření řešení nezadali registr kontejneru, ale přijali jste výchozí hodnotu localhost:5000, nebudete mít soubor \.env. 

<!--
   ![C# solution workspace](./media/tutorial-csharp-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Přidání přihlašovacích údajů registru

V souboru prostředí jsou uložené přihlašovací údaje pro registr kontejneru, které soubor sdílí s modulem runtime IoT Edge. Modul runtime tyto přihlašovací údaje potřebuje k přetažení vašich privátních imagí do zařízení IoT Edge. 

1. V průzkumníku VS Code otevřete soubor .env. 
2. Aktualizujte pole hodnotami **uživatelské jméno** a **heslo**, které jste zkopírovali z registru kontejneru Azure. 
3. Soubor uložte. 

### <a name="update-the-module-with-custom-code"></a>Aktualizace modulu pomocí vlastního kódu

1. V průzkumníku VS Code otevřete **modules** > **CSharpModule** > **Program.cs**.

2. Nahoře v oboru názvů **CSharpModule** přidejte tři příkazy **using** pro typy, které se použijí později:

    ```csharp
    using System.Collections.Generic;     // For KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // For TwinCollection
    using Newtonsoft.Json;                // For JsonConvert
    ```

3. Do třídy **Program** přidejte proměnnou **temperatureThreshold**. Tato proměnná nastaví hodnotu, kterou musí naměřená teplota překročit, aby se data odeslala do IoT Hubu. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

4. Do třídy **Program** přidejte třídy **MessageBody**, **Machine** a **Ambient**. Tyto třídy definují očekávané schéma textu příchozích zpráv.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
    ```

5. V metodě **Init** kódem vytvoříte a nakonfigurujete objekt **ModuleClient**. Tento objekt modulu umožňuje připojit se k místnímu modulu runtime IoT Edge kvůli odesílání a přijímání zpráv. Modul runtime IoT Edge poskytne modulu připojovací řetězec používaný metodou **Init**. Po vytvoření objektu **ModuleClient** kód přečte hodnotu **TemperatureThreshold** z požadovaných vlastností dvojčete modulu. a zaznamená zpětné volání přijatých zpráv z IoT Edge Hubu prostřednictvím koncového bodu **input1**. Nahraďte metodu **SetInputMessageHandlerAsync** novou metodou a přidejte metodu **SetDesiredPropertyUpdateCallbackAsync** pro aktualizace požadovaných vlastností. Pokud chcete tuto změnu provést, nahraďte poslední řádek metody **Init** následujícím kódem:

    ```csharp
    // Register a callback for messages that are received by the module.
    // await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read the TemperatureThreshold value from the module twin's desired properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    await OnDesiredPropertiesUpdate(moduleTwin.Properties.Desired, ioTHubModuleClient);
    
    // Attach a callback for updates to the module twin's desired properties.
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);

    // Register a callback for messages that are received by the module.
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

6. Do třídy **Program** přidejte metodu **onDesiredPropertiesUpdate**. Tato metoda přijímá aktualizace požadovaných vlastností dvojčete modulu a aktualizuje proměnnou **temperatureThreshold** na stejnou hodnotu. Všechny moduly mají vlastní dvojče, abyste mohli kód, který je spuštěný v modulu, konfigurovat přímo z cloudu.

    ```csharp
    static Task OnDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

7. Nahraďte metodu **PipeMessage** metodou **FilterMessages**. Tato metoda se volá pokaždé, když modul dostane zprávu z IoT Edge Hubu. Odfiltruje zprávy, které hlásí nižší teploty, než je prahová hodnota nastavená ve dvojčeti modulu. Do zprávy také přidá vlastnost **MessageType**, která má nastavenou hodnotu **Alert**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);
        try
        {
            ModuleClient moduleClient = (ModuleClient)userContext;
            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get the message body.
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await moduleClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed.
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed.
            var moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed.
            ModuleClient moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

8. Uložte soubor Program.cs.

9. V průzkumníku VS Code otevřete soubor **deployment.template.json** v pracovním prostoru řešení IoT Edge. Tento soubor říká agentovi, IoT Edge které moduly chcete nasadit, v tomto případě **tempSensor** a **CSharpModule**a informuje Centrum IoT Edge, jak můžete směrovat zprávy mezi nimi. Rozšíření Visual Studio Code automaticky naplní většinu informací, že v šablonu nasazení, ale ověřte, že je vše přesné pro vaše řešení: 

   1. Výchozí platformu IoT Edge je nastavena **amd64** ve vaší VS Code stavového řádku, což znamená, že vaše **CSharpModule** je nastavena na Linuxu amd64 verzi image. Změnit výchozí platforma ve stavovém řádku z **amd64** k **arm32v7** nebo **windows amd64** Pokud tomu tak architektuře zařízení IoT Edge. 

      ![Aktualizace modulu image platformy](./media/tutorial-csharp-module/image-platform.png)

   2. Zkontrolujte, jestli má šablona správný název modulu, ne výchozí název **SampleModule**, který jste změnili při vytváření řešení IoT Edge.

   3. **RegistryCredentials** části ukládá přihlašovací údaje registru Dockeru, tak, aby se agent IoT Edge můžete vyžádat bitové kopie modulu. Skutečná uživatelská jména a hesla se ukládají do souboru .env, který git ignoruje. Pokud jste to ještě neudělali, přidejte do souboru .env svoje přihlašovací údaje.  

   4. Pokud chcete získat další informace o manifesty nasazení, přečtěte si téma [zjistěte, jak nasadit moduly a vytvářet ve službě IoT Edge](module-composition.md).

10. Přidejte do manifestu nasazení dvojče modulu **CSharpModule**. Vložte následující obsah JSON do dolní části oddílu **modulesContent** za dvojče modulu **$edgeHub**: 

   ```json
       "CSharpModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Dvojče zařízení přidat do šablony nasazení](./media/tutorial-csharp-module/module-twin.png)

11. Uložte soubor deployment.template.json.


## <a name="build-your-iot-edge-solution"></a>Sestavení řešení IoT Edge

V předchozí části jste vytvořili řešení IoT a do modulu **CSharpModule** jste přidali kód k odfiltrování zpráv, ve kterých je hlášená teplota počítače nižší než přípustná mezní hodnota. Teď je potřeba vytvořit toto řešení jako image kontejneru a odeslat ho do registru kontejneru. 

1. Zadáním následujícího příkazu v integrovaném terminálu editoru Visual Studio Code se přihlaste k Dockeru. Potom můžete odeslat image modulu do služby Azure Container Registry.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Použijte uživatelské jméno, heslo a přihlašovací server zkopírované ze služby Azure Container Registry v první části. Tyto hodnoty můžete také načíst z oddílu **Přístupové klíče** v registru na webu Azure Portal.

2. V průzkumníku VS Code klikněte pravým tlačítkem na soubor deployment.template.json a vyberte **Build and Push IoT Edge solution** (Vytvořit a odeslat řešení IoT Edge). 

Když editoru Visual Studio Code sdělíte, že má sestavit vaše řešení, nejdříve se načtou informace ze šablony nasazení a v nové složce s názvem **config** se vygeneruje soubor deployment.json. Pak se v integrovaném terminálu spustí dva příkazy: `docker build` a `docker push`. Tyto dva příkazy sestaví kód, provedou kontejnerizaci vaší knihovny CSharpModule.dll a odešlou ji do registru kontejneru, který jste zadali při inicializaci řešení. 

Úplnou adresu image kontejneru se značkou můžete vidět v integrovaném terminálu VS Code. Adresa image je sestavená z informací v souboru module.json a má formát \<úložiště\>:\<verze\>-\<platforma\>. V tomto kurzu by měla vypadat takto: název_registru.azurecr.io/csharpmodule:0.0.1-amd64.

>[!TIP]
>Pokud obdržíte chybu při vytváření a nasdílení změn modul, proveďte následující kontroly:
>* Přihlásili jste k Dockeru ve Visual Studio Code pomocí přihlašovacích údajů ze služby container registry? Tyto přihlašovací údaje se liší od těch, které používáte k přihlášení k webu Azure portal.
>* Správnost vašeho kontejneru úložiště? Otevřít **moduly** > **cmodule** > **module.json** a najít **úložiště** pole. Úložiště imagí by měl vypadat jako  **\<registryname\>.azurecr.io/csharpmodule**. 
>* Sestavujete stejného typu kontejnerů, které běží v počítači pro vývoj? Visual Studio Code výchozí kontejnery Linuxu amd64. Pokud vývojovém počítači běží kontejnery Windows nebo Linuxem arm32v7 kontejnery, aktualizujte platformy na modrý stavového řádku v dolní části okna VS Code tak, aby odpovídaly vaši kontejnerovou platformu.

## <a name="deploy-and-run-the-solution"></a>Nasazení a spuštění řešení

V článku Rychlý start, pomocí kterého jste nastavili své zařízení IoT Edge, jste nasadili modul pomocí webu Azure Portal. Můžete také nasadit moduly pomocí rozšíření Azure IoT Hub Toolkit (dříve rozšíření Azure IoT Toolkit) pro Visual Studio Code. Pro svůj scénář už máte připravený manifest nasazení – soubor **deployment.json**. Teď stačí jen vybrat zařízení, na které se nasazení provede.

1. V nástroji VS Code paletu příkazů, spusťte **Azure IoT Hub: Vyberte službu IoT Hub**. 

2. Zvolte předplatné a centrum IoT obsahující zařízení IoT Edge, které chcete nakonfigurovat. 

3. V průzkumníku VS Code rozbalte oddíl **Azure IoT Hub Devices** (Zařízení Azure IoT Hub). 

4. Klikněte pravým tlačítkem na název vašeho zařízení IoT Edge a pak vyberte **Create Deployment for Single Device** (Vytvořit nasazení pro jedno zařízení). 

   ![Vytvoření nasazení pro jedno zařízení](./media/tutorial-csharp-module/create-deployment.png)

5. Vyberte ve složce **config** soubor **deployment.json** a klikněte na **Select Edge Deployment Manifest** (Vybrat manifest nasazení Edge). Nepoužívejte soubor deployment.template.json. 

6. Klikněte na tlačítko pro obnovení. Měl by se zobrazit spuštěný nový modul **CSharpModule** společně s modulem **TempSensor** a moduly **$edgeAgent** a **$edgeHub**.  

## <a name="view-generated-data"></a>Zobrazení vygenerovaných dat

Po použití manifestu nasazení pro zařízení IoT Edge začne modul runtime IoT Edge na zařízení shromažďovat informace o novém nasazení a jednat podle nich. Všechny moduly spuštěné na zařízení, které nejsou zahrnuté do manifestu nasazení, se zastaví. Všechny moduly, které na zařízení chybí, se spustí. 

Stav zařízení IoT Edge můžete zobrazit v části **Zařízení Azure IoT Hub** v průzkumníku Visual Studio Code. Rozbalením podrobností o zařízení zobrazíte seznam nasazených a spuštěných modulů. 

Na samotném zařízení IoT Edge můžete stav modulů nasazení zobrazit pomocí příkazu `iotedge list`. Měly by se zobrazit čtyři moduly: dva moduly runtime IoT Edge, tempSensor a vlastní modul, který jste vytvořili v tomto kurzu. Spuštění všech modulů může několik minut trvat, proto příkaz spusťte znovu, pokud se zpočátku všechny nezobrazí. 

Pokud chcete zobrazit zprávy, které jednotlivé moduly generují, použijte příkaz `iotedge logs <module name>`. 

Zprávy přicházející do centra IoT můžete zobrazit pomocí Visual Studio Code. 

1. Když chcete monitorovat data, která přichází do služby IoT Hub, vyberte tři tečky (**...**) a potom vyberte **Start Monitoring D2C Messages** (Zahájit monitorování zpráv D2C).
2. Pokud chcete monitorovat zprávy D2C pro konkrétní zařízení, klikněte pravým tlačítkem na příslušné zařízení v seznamu a vyberte **Start Monitoring D2C Messages** (Zahájit monitorování zpráv D2C).
3. Pokud chcete monitorování dat zastavit, spusťte příkaz **Azure IoT Hub: Zastavit monitorování zpráv D2C** v paletu příkazů. 
4. Pokud chcete zobrazit nebo aktualizovat dvojče modulu, klikněte pravým tlačítkem na příslušný modul v seznamu a vyberte **Edit module twin** (Upravit dvojče modulu). Pokud chcete aktualizovat dvojče modulu, uložte soubor JSON dvojčete, klikněte pravým tlačítkem na oblast editoru a vyberte **Update Module Twin** (Aktualizovat dvojče modulu).
5. Pokud chcete zobrazit protokoly Dockeru, nainstalujte [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) pro VS Code. Spuštěné moduly můžete vyhledat místně v průzkumníku Dockeru. Výběrem možnosti **Show Logs** (Zobrazit protokoly) v místní nabídce je zobrazíte protokoly v integrovaném terminálu.
 
## <a name="clean-up-resources"></a>Vyčištění prostředků 

Pokud máte v plánu pokračovat k dalšímu doporučenému článku, můžete si vytvořené prostředky a konfigurace uschovat a znovu je použít. Také můžete dál používat stejné zařízení IoT Edge jako testovací zařízení. 

Jinak můžete místní konfigurace a prostředky Azure vytvořené v tomto článku odstranit, abyste se vyhnuli poplatkům. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili modul IoT Edge s kódem pro filtrování nezpracovaných dat generovaných zařízením IoT Edge. Až budete chtít vytvářet vlastní moduly, získáte další informace na stránce o [vývoji modulu jazyka C# pomocí Azure IoT Edge pro Visual Studio Code](how-to-develop-csharp-module.md). Pokračujte dalšími kurzy, ve kterých se naučíte další způsoby, jak vám může Azure IoT Edge pomoct přeměnit data na obchodní informace o hraničním zařízení.

> [!div class="nextstepaction"]
> [Uložení dat na hraničních zařízeních s využitím databází SQL Serveru](tutorial-store-data-sql-server.md)
