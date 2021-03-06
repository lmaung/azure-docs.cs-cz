---
title: Kurz pro používání v aplikaci ASP.NET Core dynamickou konfiguraci konfigurace aplikace pro Azure | Dokumentace Microsoftu
description: V tomto kurzu se dozvíte, jak se dynamicky aktualizovat konfigurační data pro aplikace ASP.NET Core
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 44ae922b182874eef378d4868fb278c3c76252db
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884790"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Kurz: Použít dynamické konfiguraci aplikace ASP.NET Core

ASP.NET Core je modulární konfigurace systému, který podporuje čtení konfigurační data z nejrůznějších zdrojů, stejně jako zpracování změn v reálném čase bez způsobení aplikací k restartování. Podporuje vazby konfigurační nastavení silného typu třídy rozhraní .NET a vkládání do kódu pomocí různých `IOptions<T>` vzory. Jedna z následujících vzorů, konkrétně `IOptionsSnapshot<T>`, nabízí automatické načtení konfigurace aplikace, když se změní podkladová data. Můžete vložit `IOptionsSnapshot<T>` do řadiče ve vaší aplikaci přístup k nejnovější konfigurace uložené v konfiguraci aplikací Azure. Kromě toho můžete nastavit Klientská knihovna ASP.NET Core konfigurace aplikace průběžně monitorovat a načíst všechny změny v konfiguraci obchodě s aplikacemi pomocí cyklického dotazování v pravidelných intervalech, které definujete.

Tento kurz ukazuje, jak můžete implementovat konfigurace dynamické aktualizace ve vašem kódu. Sestaví ve webové aplikaci zavedený rychlých startech. Kompletní [vytvoření aplikace ASP.NET Core s konfigurací aplikace](./quickstart-aspnet-core-app.md) první předtím, než budete pokračovat.

K dokončení kroků v tomto rychlém startu můžete použít jakýkoli editor kódu. Skvělou volbou je však editor [Visual Studio Code](https://code.visualstudio.com/), který je dostupný pro platformy Windows, macOS a Linux.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavení aplikace se aktualizovat jeho konfiguraci v reakci na změny v úložišti konfigurace aplikace
> * Vložit nejnovější konfiguraci v řadiči vaší aplikace

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý start, nainstalujte [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Znovu načíst data z konfigurace aplikace

1. Otevřít *Program.cs* a aktualizovat `CreateWebHostBuilder` metodu tak, že přidáte `config.AddAzureAppConfiguration()` metody.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.Connect(settings["ConnectionStrings:AppConfig"])
                    .Watch("TestApp:Settings:BackgroundColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:FontColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:Message", TimeSpan.FromSeconds(1)));
            })
            .UseStartup<Startup>();
    ```

    Druhý parametr `.Watch` metoda je interval dotazování, kdy Klientská knihovna ASP.NET dotazuje obchod s aplikacemi konfigurace zobrazit, pokud došlo ke změně jakékoli nakonfigurovat specifické nastavení.

2. Přidat *Settings.cs* souboru, který definuje a implementuje nový `Settings` třídy.

    ```csharp
    namespace TestAppConfig
    {
        public class Settings
        {
            public string BackgroundColor { get; set; }
            public long FontSize { get; set; }
            public string FontColor { get; set; }
            public string Message { get; set; }
        }
    }
    ```

3. Otevřít *Startup.cs* a aktualizovat `ConfigureServices` metody k vytvoření vazby na konfigurační data `Settings` třídy.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));

        services.Configure<CookiePolicyOptions>(options =>
        {
            options.CheckConsentNeeded = context => true;
            options.MinimumSameSitePolicy = SameSiteMode.None;
        });

        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

## <a name="use-the-latest-configuration-data"></a>Použít nejnovější konfigurační data

1. Otevřít *HomeController.cs* v *řadiče* adresář, aktualizace `HomeController` třídy pro příjem `Settings` pomocí vkládání závislostí a zkontrolujte použití jeho hodnoty.

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        public HomeController(IOptionsSnapshot<Settings> settings)
        {
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }
    }
    ```

2. Otevřít *Index.cshtml* v *zobrazení* > *Domů* adresáře a nahraďte jeho obsah následujícím kódem:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @ViewData["BackgroundColor"]
        }
        h1 {
            color: @ViewData["FontColor"];
            font-size: @ViewData["FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@ViewData["Message"]</h1>
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Sestavte a spusťte aplikaci místně

1. Pokud chcete aplikaci sestavit pomocí .NET Core CLI, spusťte v příkazovém prostředí následující příkaz:

        dotnet build

2. Po úspěšném dokončení sestavení spuštěním následujícího příkazu místně spusťte webovou aplikaci:

        dotnet run

3. Spusťte okno prohlížeče a přejděte na `http://localhost:5000`, což je výchozí adresa URL pro webové aplikace hostované místně.

    ![Místní spuštění aplikace rychlý start](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. Přihlaste se k [webu Azure portal](https://aka.ms/azconfig/portal), klikněte na tlačítko **všechny prostředky** a instance aplikace, který konfigurace úložiště, který jste vytvořili v rychlém startu.

5. Klikněte na tlačítko **klíč/hodnota Explorer** a aktualizujte hodnoty následujících klíčů:

    | Klíč | Hodnota |
    |---|---|
    | TestAppSettings:BackgroundColor | Modrá |
    | TestAppSettings:FontColor | lightGray |
    | TestAppSettings:Message | Data z konfigurace aplikace Azure – nyní se živé aktualizace! |

6. Aktualizujte stránku v prohlížeči zobrazíte nové nastavení konfigurace.

    ![Místní aktualizace aplikace rychlý start](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Další postup

V tomto kurzu jste přidali do Azure se identita spravované služby pro přístup ke konfiguraci aplikací pro zjednodušení a zlepšení Správa přihlašovacích údajů pro vaši aplikaci. Další informace o použití konfigurací aplikací, i nadále ukázky Azure CLI.

> [!div class="nextstepaction"]
> [Ukázky rozhraní příkazového řádku](./cli-samples.md)
