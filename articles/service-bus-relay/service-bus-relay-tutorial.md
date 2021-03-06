---
title: Zpřístupňují služby WCF REST v místním klientovi externí s využitím Azure WCF Relay | Dokumentace Microsoftu
description: Vytvoření aplikace klienta a služby pomocí WCF Relay.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 6927788fa79c567222a199064f5b375546ecf9ad
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615464"
---
# <a name="expose-an-on-premises-wcf-rest-service-to-external-client-by-using-azure-wcf-relay"></a>Zpřístupňují služby WCF REST v místním klientovi externí s využitím Azure WCF Relay

Tento kurz popisuje, jak vytvořit jednoduchý klienta WCF Relay, aplikace a služby pomocí Azure Relay. Podobný kurz, který používá [zasílání zpráv Service Bus](../service-bus-messaging/service-bus-messaging-overview.md), naleznete v tématu [Začínáme s frontami služby Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Absolvování tohoto kurzu pochopíte kroky, které jsou potřeba pro vytvoření aplikace klienta a služby WCF Relay. Stejně jako jejich protějšky původní WCF je služba konstrukci, která poskytuje jeden nebo více koncových bodů, každý z nich vystavuje jednu nebo víc operací služeb. Koncový bod služby specifikuje adresu, kde se dá služba najít, vazbu, která obsahuje informaci, že klient musí komunikovat se službou, a kontrakt, který definuje funkci, kterou služba klientovi poskytuje. Hlavní rozdíl mezi WCF a WCF Relay je, že je koncový bod vystavený v cloudu, a ne místně na vašem počítači.

Po dokončení řady témat v tomto kurzu budete mít funkční službu a klienta, který může vyvolat operace této služby. První téma popisuje, jak vytvořit účet. V dalších krocích se popisuje, jak definovat službu, která používá kontrakt, jak tuto službu implementovat a jak službu konfigurovat pomocí kódu. Taky se v nich popisuje, jak hostovat a spustit službu. Vytvořená služba se hostuje sama a klient a služba běží na stejném počítači. Službu můžete konfigurovat pomocí kódu nebo konfiguračního souboru.

Poslední tři kroky popisují, jak vytvořit klientskou aplikaci, nakonfigurovat klientskou aplikaci a vytvořit a použít klienta, který může přistupovat k funkcím hostitele.

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvořte obor názvů služby Relay.
> * Vytvoření kontraktu služby WCF
> * Implementace kontraktu WCF
> * Hostování a spuštění služby WCF, který chcete zaregistrovat do služby Relay
> * Vytvoření klienta WCF pro kontrakt služby
> * Konfigurace klienta WCF
> * Implementace klienta WFG
> * Spuštění aplikace. 

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu musí být splněné následující požadavky:

- Předplatné Azure. Pokud ho nemáte, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
- [Sada Visual Studio 2015 nebo novější](http://www.visualstudio.com). V příkladech v tomto kurzu se používá sada Visual Studio 2017.
- Azure SDK pro .NET. Nainstalujte ji z [stránky pro stažení sady SDK](https://azure.microsoft.com/downloads/).

## <a name="create-a-relay-namespace"></a>Vytvořit obor názvů služby Relay
Prvním krokem je vytvoření oboru názvů a získat [sdíleného přístupového podpisu (SAS)](../service-bus-messaging/service-bus-sas.md) klíč. Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu relay. Systém automaticky vygeneruje SAS klíč při vytvoření oboru názvů služby. Kombinace oboru názvů služby a klíče SAS poskytuje přihlašovací údaje pro Azure k ověření přístupu k aplikaci.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract"></a>Definování kontraktu služby WCF
Kontrakt služby specifikuje, jaké operace (termín webových služeb pro metody nebo funkce) služba podporuje. Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic. Každá metoda v rozhraní odpovídá konkrétní operaci služby. Na každé rozhraní musí mít aplikovaný atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) a na každou operace musí byt aplikovaný atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx). Pokud metoda v rozhraní, které má atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx), nemá atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taková metoda se nevystaví. Kód k těmto úlohám najdete v příkladu za postupem. Podrobnější diskuzi o kontraktech a službách najdete v dokumentaci WCF v části [Návrh a implementace služeb](https://msdn.microsoft.com/library/ms729746.aspx).

### <a name="create-a-relay-contract-with-an-interface"></a>Vytvoření kontraktu relay s rozhraní

1. Otevřete Visual Studio jako správce tak, že v nabídce **Start** kliknete na program pravým tlačítkem a vyberete možnost **Spustit jako správce**.
2. Vytvořte nový projekt konzolové aplikace. Klikněte na nabídku **Soubor** a vyberte možnost **Nový**, a pak klikněte na **Projekt**. V dialogu **Nový projekt** klikněte na **Visual C#** (pokud se **Visual C#** nezobrazí, podívejte se do části **Jiné jazyky**). Klikněte na tlačítko **Konzolová aplikace (.NET Framework)** šablony a pojmenujte ho **EchoService**. Projekt vytvoříte kliknutím na **OK**.

    ![Vytvoření konzolové aplikace][2]

3. Nainstalujte balíček Service Bus NuGet. Tento balíček automaticky přidá reference na knihovny Service Bus a WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je obor názvů, který vám umožňuje programový přístup k základním funkcím WCF. Service Bus používá mnoho objektů a atributů WCF k definování kontraktů služby.

    V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a potom klikněte na tlačítko **spravovat balíčky NuGet...** . Klikněte na kartu Procházet a pak vyhledejte **WindowsAzure.ServiceBus**. Zkontrolujte, že je v části **Verze** označený název projektu. Klikněte na **Instalovat** a přijměte podmínky použití.

    ![Balíček Service Bus][3]
4. V Průzkumníku řešení poklikejte na soubor Program.cs a pokud ještě není otevřený, otevře se v editoru Visual Studio.
5. Na začátek souboru přidejte následující příkazy:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. Změňte název oboru názvů z výchozího názvu **EchoService** na **Microsoft.ServiceBus.Samples**.

   > [!IMPORTANT]
   > Tento kurz používá obor názvů C# **Microsoft.ServiceBus.Samples**, které je obor názvů kontraktu podle typu, který se používá v konfiguračním souboru v spravované [konfigurace klienta WCF](#configure-the-wcf-client) krok. Při sestavování této ukázky můžete specifikovat jakýkoli obor názvů, který chcete – tento kurz ale bude fungovat jen tehdy, když odpovídajícím způsobem v konfiguračním souboru aplikace upravíte obor názvů kontraktu a služby. Obor názvů specifikovaný v souboru App.config musí být stejný jako obor názvů zadaný ve vašich souborech C#.
   >
   >
7. Přímo po `Microsoft.ServiceBus.Samples` deklarace oboru názvů, ale v rámci oboru názvů definujte nové rozhraní s názvem `IEchoContract` a použít `ServiceContractAttribute` atribut rozhraní s hodnotou oboru názvů `http://samples.microsoft.com/ServiceModel/Relay/`. Hodnota oboru názvů se liší od oboru názvů, které používáte v celém svém kódu. Místo toho se obor názvů používá jako jedinečný identifikátor pro tento kontrakt. Když explicitně zadáte obor názvů, zabráníte tím přidání výchozí hodnoty oboru názvů do názvu kontraktu. Po deklaraci oboru názvů, vložte následující kód:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > Obor názvů kontraktu služby obvykle obsahuje schéma pojmenování s informacemi o verzi. Informace o verzi, které jsou v oboru názvů kontraktu služby, službám umožňuje službám izolovat výrazné změny pomocí definice nové služby s novým oborem názvů, která bude vystavená na novém koncovém bodu. Tímto způsobem můžete dál používat starého kontraktu služby bez nutnosti ho aktualizovat klienty. Informace o verzi může mít podobu data nebo čísla sestavení. Další informace najdete v článku o [Správa verzí služeb](https://go.microsoft.com/fwlink/?LinkID=180498). Pro účely tohoto kurzu nebude mít schéma pojmenování oboru názvů kontraktu služby žádné informace o verzi.
   >
   >
8. V rámci `IEchoContract` rozhraní, deklarujte metodu pro jednu operaci `IEchoContract` kontrakt vystaví v rozhraní a použít `OperationContractAttribute` atribut do metody, kterou chcete vystavit v rámci veřejného kontraktu WCF Relay, následujícím způsobem:

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. Přímo po definici rozhraní `IEchoContract` deklarujte kanál, který zdědí vlastnosti z obou `IEchoContract` a také do rozhraní `IClientChannel`, jak je vidět tady:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanál je objekt WCF, kterým si hostitel a klient navzájem posílají informace. Později napíšete kód na kanál, aby se informace zrcadlily mezi oběma aplikacemi.
10. V nabídce **Sestavení** můžete kliknout na **Sestavit řešení** nebo stisknout **Ctrl+Shift+B** a potvrdit přesnost své dosavadní práce.

### <a name="example"></a>Příklad:

Následující kód ukazuje základní rozhraní, který definuje kontrakt WCF Relay.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Když je teď vytvořené rozhraní, můžete ho implementovat.

## <a name="implement-the-wcf-contract"></a>Implementace kontraktu WCF

Vytvoření Azure relay vyžaduje, abyste nejdřív vytvořili kontrakt, který se definuje pomocí rozhraní. Další informace o vytváření rozhraní najdete v předchozím kroku. Dalším krokem je implementace rozhraní. K tomu patří vytvoření třídy s názvem `EchoService`, která implementuje uživatelsky definované rozhraní `IEchoContract`. Po implementaci rozhraní nakonfigurujete rozhraní pomocí souboru App.config. Konfigurační soubor obsahuje informace potřebné pro aplikaci, jako je název služby, název kontraktu a typ protokolu, který se používá ke komunikaci se službou relay. Kód použitý k těmto úlohám najdete v příkladu za postupem. Obecnější diskuzi o způsobu implementace kontraktu služby najdete v dokumentaci WCF [Implementace kontraktů služby](https://msdn.microsoft.com/library/ms733764.aspx).

1. Vytvořte novou třídu s názvem `EchoService` přímo po definování rozhraní `IEchoContract`. Třída `EchoService` implementuje rozhraní `IEchoContract`.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Podobně jako u implementace jiných rozhraní můžete definici implementovat v jiném souboru. V tomto kurzu je ale implementace ve stejném souboru jako definice rozhraní a metoda `Main`.
2. Na rozhraní `IEchoContract` aplikujte atribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx). Atribut specifikuje název služby a obor názvů. Když to dokončíte, třída `EchoService` bude vypadat takto:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. Implementujte metodu `Echo` definovanou v rozhraní `IEchoContract` ve třídě `EchoService`.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. Klikněte na **Sestavení**, klikněte na **Sestavit řešení** a zkontrolujte přesnost své práce.

### <a name="define-the-configuration-for-the-service-host"></a>Definice konfigurace hostitele služby

1. Konfigurační soubor je velmi podobný konfiguračnímu souboru WCF. Obsahuje název služby, koncový bod (to znamená, umístění, které Azure Relay vystaví pro klienty a hostitele ke komunikaci mezi sebou) a vazbu (typ protokolu používaný pro komunikaci). Hlavní rozdíl je v tom, že nakonfigurovaný koncový bod služby odkazuje na vazbu [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding), která není součástí architektury .NET Framework. [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) je jedna z vazeb definovaných prostředím služby.
2. V **Průzkumníku řešení** poklikejte na soubor App.config a otevře se v editoru Visual Studio.
3. V elementu `<appSettings>` nahraďte zástupné texty názvem svého oboru názvů a klíčem SAS, který jste zkopírovali v jednom z předchozích kroků.
4. Ve značkách `<system.serviceModel>` přidejte element `<services>`. Můžete definovat více aplikacích s předáváním v jednom konfiguračním souboru. V tomto kurzu se ale definuje jen jedna.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. V elementu `<services>` přidejte element `<service>`, který definuje název služby.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. V elementu `<service>` definujte umístění koncového bodu kontraktu a také vazbu koncového bodu.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Koncový bod definuje, kde bude klient hledat hostitelskou aplikaci. Tento kurz později použije tento krok vytvořit identifikátor URI, která plně vystavuje hostitele přes Azure Relay. Vazba deklaruje, že ke komunikaci se službou relay používáme protokol TCP jako protokol.
7. V nabídce **Sestavení** klikněte na **Sestavit řešení** a zkontrolujte přesnost své práce.

### <a name="example"></a>Příklad:

Následující kód ukazuje implementaci kontraktu služby.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Následující kód ukazuje základní formát souboru App.config přidruženého k hostiteli služby.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-the-wcf-service-to-register-with-the-relay-service"></a>Hostování a spuštění služby WCF, který chcete zaregistrovat do služby relay

Tento krok popisuje, jak spouštět služby Azure Relay.

### <a name="create-the-relay-credentials"></a>Vytvořte přihlašovací údaje relay

1. V `Main()` vytvořte dvě proměnné, do kterých se uloží obor názvů a klíč SAS načtené z okna konzoly.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Klíč SAS se později použije pro přístup k projektu. Obor názvů se předá do `CreateServiceUri` jako parametr a vytvoří se URI služby.
2. Pomocí objektu [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) deklarujte, že jako typ pověření budete používat klíč SAS. Následující kód přidejte přímo za kód, který jste přidali v předchozím kroku.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a>Vytvoření bázové adresy pro službu

Po kódu přidaném v předchozím kroku, vytvořte `Uri` instance pro bázovou adresu služby. Toto URI specifikuje schéma Service Bus, obor názvů a cestu rozhraní služby.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

„sb“ je zkratka schématu Service Bus a indikuje, že používáme protokol TCP. To jsme předtím indikovali v konfiguračním souboru, když jsme specifikovali [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) jako vazbu.

V tomto kurzu je URI `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="create-and-configure-the-service-host"></a>Vytvoření a konfigurace hostitele služby

1. Nastavte režim připojení na `AutoDetect`.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Režim připojení popisuje protokol služba používá ke komunikaci se službou relay; pomocí protokolu HTTP nebo TCP. Pomocí výchozího nastavení `AutoDetect`, se služba pokusí připojit k Azure Relay přes TCP, pokud je k dispozici a HTTP, pokud TCP není k dispozici. Všimněte si, že tu je rozdíl oproti protokolu, který služba specifikuje pro komunikaci klienta. Jeho protokol se určuje podle požité vazby. Například můžete použít službu [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) vazba, která určuje, že její koncový bod s klienty komunikuje přes HTTP. Aby mohli zadat stejnou službu **ConnectivityMode.AutoDetect** tak, aby služba komunikovala s Azure Relay přes TCP.
2. Vytvořte hostitele služby pomocí URI, které jste předtím vytvořili v této části.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hostitel služby je objekt WCF, který instancuje službu. Tady jí předáte typ služby, kterou chcete vytvořit (typ `EchoService`) a taky adresu, na které chcete službu vystavit.
3. Na začátku souboru Program.cs přidejte odkazy na [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) a [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. Zpátky v `Main()` nakonfigurujte koncový bod, a povolte tak veřejný přístup.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Tento krok informuje o tom, který vaše aplikace může dá veřejně najít tím, že kontroluje informační kanál pro váš projekt ATOM služby relay. Pokud **DiscoveryType** nastavíte na **private**, služba by pro klienta pořád byla dostupná. Služba však nezobrazí při hledání obor názvů služby Relay. Místo toho by klient musel předem znát cestu ke koncovému bodu.
5. Použijte pověření služby na koncové body služby definované v souboru App.config:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Jak jsme uvedli v předchozím kroku, mohli jste v konfiguračním souboru deklarovat několik služeb a koncových bodů. Pokud byste to udělali, tento kód by prošel konfigurační soubor a vyhledal by všechny koncové body, na které by měl vaše pověření použít. V tomto kurzu má ale konfigurační soubor jen jeden koncový bod.

### <a name="open-the-service-host"></a>Otevření hostitele služby

1. Otevřete službu.

    ```csharp
    host.Open();
    ```
2. Informujte uživatele, že zpráva běží, a vysvětlete mu, jak službu ukončit.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. Po dokončení zavřete hostitele služby.

    ```csharp
    host.Close();
    ```
4. Stisknutím kláves **CTRL+SHIFT+B** sestavte projekt.

### <a name="example"></a>Příklad:

Váš kód dokončené služby by měl vypadat následovně. Kód obsahuje kontrakt a implementaci služby z předchozích kroků tohoto kurzu a hostuje službu v konzolové aplikaci.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Vytvoření klienta WCF pro kontrakt služby

Dalším krokem je vytvoření klientské aplikace a definování kontraktu služby, který implementujete v pozdějších krocích. Všimněte si, že hodně těchto kroků připomíná kroky k vytvoření služby: definování kontraktu, upravení App.config soubor, pomocí přihlašovacích údajů pro připojení ke službě relay a tak dále. Kód použitý k těmto úlohám najdete v příkladu za postupem.

1. Vytvořte pro klienta nový projekt v aktuálním řešení ve Visual Studiu, a to tímto postupem:

   1. V Průzkumníku řešení ve stejném řešení, které obsahuje službu, klikněte pravým tlačítkem na aktuální řešení (nikoli projekt) a klikněte na **Přidat**. Pak klikněte na **Nový projekt**.
   2. V **přidat nový projekt** dialogové okno, klikněte na tlačítko **Visual C#** (Pokud **Visual C#** nezobrazí, podívejte se do části **jiné jazyky**), vyberte **Konzolová aplikace (.NET Framework)** šablony a pojmenujte ho **EchoClient**.
   3. Klikněte na **OK**.
      <br />
2. V Průzkumníku řešení poklikejte na soubor Program.cs v projektu **EchoClient** a pokud ještě není otevřený, otevře se v editoru Visual Studio.
3. Změňte název oboru názvů z výchozího názvu `EchoClient` na `Microsoft.ServiceBus.Samples`.
4. Nainstalovat [balíček NuGet služby Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus): v Průzkumníku řešení klikněte pravým tlačítkem myši **EchoClient** projektu a pak klikněte na **spravovat balíčky NuGet**. Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Klikněte na **Instalovat** a přijměte podmínky použití.

    ![][3]
5. V souboru Program.cs přidejte příkaz `using` pro obor názvů [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx).

    ```csharp
    using System.ServiceModel;
    ```
6. Přidejte definici kontraktu služby do oboru názvů, jak je vidět v následujícím příkladu. Všimněte si, že je tato definice skoro stejná jako definice použitá v projektu **Service**. Tento kód byste měli přidat na začátek oboru názvů `Microsoft.ServiceBus.Samples`.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. Stisknutím kláves **CTRL+SHIFT+B** sestavte klienta.

### <a name="example"></a>Příklad:

Následující kód ukazuje aktuální stav souboru Program.cs v **EchoClient** projektu.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Konfigurace klienta WCF

V tomto kroku vytvoříte soubor App.config pro základní klientskou aplikaci, která bude mít přístup ke službě vytvořené v jednom z předchozích kroků tohoto kurzu. Tento soubor App.config definuje kontrakt, vazbu a název koncového bodu. Kód použitý k těmto úlohám najdete v příkladu za postupem.

1. V Průzkumníku řešení v projektu **EchoClient** poklikejte na **App.config.cs** a otevře se v editoru Visual Studio.
2. V elementu `<appSettings>` nahraďte zástupné texty názvem svého oboru názvů a klíčem SAS, který jste zkopírovali v jednom z předchozích kroků.
3. V elementu system.serviceModel element přidejte element `<client>`.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Tento krok deklaruje, že definujete klientskou aplikaci ve stylu WCF.
4. V elementu `client` definujte název, kontrakt a typ vazby koncového bodu.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Tento krok definuje název koncového bodu, kontrakt definovaný ve službě a fakt, že klientská aplikace používá TCP ke komunikaci s Azure Relay. Název koncového bodu se použije v následujícím kroku k propojení této konfigurace koncového bodu s URI služby.
5. Klikněte na tlačítko **souboru**, pak klikněte na tlačítko **Uložit vše**.

### <a name="example"></a>Příklad:

Následující kód ukazuje soubor App.config pro klienta Echo.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client"></a>Implementace klienta WFG
V tomto kroku implementujete základní klientskou aplikaci, která bude mít přístup ke službě, kterou jste vytvořili v jednom z předchozích kroků tohoto kurzu. Podobně jako služba klient provádí spoustu stejných operací jako přístup k Azure Relay:

1. Nastaví režim připojení.
2. Vytvoří URI, které vyhledá hostitelskou službu.
3. Definuje bezpečnostní pověření.
4. Aplikuje pověření na připojení.
5. Otevře připojení.
6. Provádí úlohy specifické pro aplikace.
7. Ukončí připojení.

Jedním z hlavních rozdílů je ale, že klientská aplikace používá kanál pro připojení ke službě relay, zatímco služba používá volání **ServiceHost**. Kód použitý k těmto úlohám najdete v příkladu za postupem.

### <a name="implement-a-client-application"></a>Implementace klientské aplikace
1. Nastavte režim připojení na **AutoDetect**. Do metody `Main()` aplikace **EchoClient** přidejte následující kód.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. Definujte proměnné, do kterých se uloží hodnoty obor názvů služby a klíče SAS načtené z konzoly.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. Vytvořte identifikátor URI, který definuje umístění hostitele ve vašem projektu Relay.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. Vytvořte objekt pověření pro koncový bod vašeho oboru názvů.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. Vytvořte objekt kanálu pro vytváření, který načte konfiguraci popsanou v souboru App.config.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Objekt kanálu pro vytváření je objekt WCF, který vytvoří kanál, přes který může služba komunikovat s klientskými aplikacemi.
6. Použijte přihlašovací údaje.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. Vytvořte a otevřete kanál pro službu.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. Napište základní uživatelské prostředí a funkci pro echo.

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Všimněte si, že kód používá instanci objektu kanálu jako proxy pro službu.
9. Zavřete kanál a zavřete objekt pro vytváření.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

### <a name="example"></a>Příklad:

Dokončený kód by měl vypadat následovně, zobrazuje, jak vytvořit klientskou aplikaci, jak volat operace služby a jak zavřít klienta po volání operace byla dokončena.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-the-applications"></a>Spuštění aplikací

1. Stisknutím kláves **CTRL+SHIFT+B** sestavte řešení. Sestavíte tím projekt klienta a projekt služby, které jste vytvořili v předchozích krocích.
2. Než spustíte klientskou aplikaci, musíte se ujistit, že aplikace služby běží. V Průzkumníku řešení ve Visual Studiu klikněte pravým tlačítkem na řešení **EchoService**, pak klikněte na **Vlastnosti**.
3. V dialogovém okně vlastností řešení klikněte na **Spouštěný projekt** a pak klikněte na tlačítko **Více projektů po spuštění**. Ujistěte se, že se **EchoService** v seznamu objeví jako první.
4. V poli **Akce** u projektů **EchoService** i **EchoClient** nastavte **Start**.

    ![][5]
5. Klikněte na **Závislosti projektu**. V poli **Projekty** klikněte na **EchoClient**. Ujistěte se, že je v poli **Závisí na** označené **EchoService**.

    ![][6]
6. Klikněte na **OK**, a dialog **Vlastnosti** se zavře.
7. Stiskněte klávesu **F5** a oba projekty se spustí.
8. Obě okna konzoly se otevřou a požádají vás o zadání oboru názvů. Nejdřív se musí spustit služba, takže v okně konzoly **EchoService** zadejte obor názvů, a pak stiskněte **Enter**.
9. Pak se zobrazí výzva k zadání klíče SAS. Zadejte klíč SAS a stiskněte Enter.

    Tady je příklad výstupu z okna konzoly. Nezapomeňte, tyto zadané hodnoty platí jen pro tento příklad.

    `Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`

    Aplikace služby do okna konzoly vypíše adresu, na které naslouchá, jak je vidět na následujícím příkladu.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`
10. V okně konzoly **EchoClient** zadejte stejný údaj, který jste zadali pro aplikaci služby. Stejným postupem jako v předchozích krocích zadejte stejný obor názvů a klíč SAS pro klientskou aplikaci.
11. Po zadání těchto hodnot klient otevře kanál ke službě a zobrazí se výzva k zadání nějakého textu, jak je vidět v následujícím příkladu výstupu konzoly.

    `Enter text to echo (or [Enter] to exit):`

    Zadejte nějaký text, který se má odeslat do aplikace služby, a stiskněte Enter. Tento text se odešle do služby pomocí operace služby Echo a objeví se v okně konzoly služby, jak je vidět v následujícím příkladu výstupu.

    `Echoing: My sample text`

    Klientská aplikace obdrží hodnotu vrácenou z operace `Echo`. Tou je původní text, který se vypíše do okna konzoly. Následující příklad ukazuje výstup okna konzoly klienta.

    `Server echoed: My sample text`
12. Tímto způsobem můžete dál posílat textové zprávy z klienta do služby. Když skončíte, stiskněte Enter v oknech konzoly klienta a služby a obě aplikace se ukončí.

## <a name="next-steps"></a>Další postup
Přejděte k následujícímu kurzu: 

> [!div class="nextstepaction"]
>[Vystavení služby WCF REST v místním klientovi mimo vaši síť](service-bus-relay-rest-tutorial.md)

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
