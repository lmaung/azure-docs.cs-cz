---
title: Vytvoření webové aplikace Node.js – Azure App Service | Dokumentace Microsoftu
description: Během několika minut můžete nasadit svou první aplikaci Node.js Hello World pomocí služby Azure App Service Web Apps.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 237f498d1ebe2b402c86f1a4aed66a7ed443ccfa
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652528"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Vytvoření webové aplikace Node.js ve službě Azure

> [!NOTE]
> Tento článek nasadí aplikaci do služby App Service ve Windows. Nasazení do služby App Service v _Linuxu_ je popsané v tématu [Vytvoření webové aplikace v Node.js ve službě Azure App Service v Linuxu](./containers/quickstart-nodejs.md).
>

[Azure App Service ](overview.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  Tento rychlý start ukazuje, jak nasadit aplikaci Node.js do služby Azure App Service. Vytvoříte webovou aplikaci pomocí [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), ale můžete také spustit tyto příkazy místně s [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli). Nasadit ukázkový kód Node.js do webové aplikace pomocí [az webapp nasazení zdroj config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) příkazu.  

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Tento postup můžete použít v případě počítačů se systémem Mac, Windows nebo Linux. K dokončení kroků trvá přibližně 3 minuty.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Stažení ukázky

Ve službě Cloud Shell vytvořte adresář rychlého startu a přejděte do něj.

```azurecli-interactive
mkdir quickstart

cd quickstart
```

Potom spusťte následující příkaz, pomocí kterého do tohoto adresáře naklonujete úložiště ukázkové aplikace.

```azurecli-interactive
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Při spuštění příkaz zobrazí podobné informace jako v následujícím příkladu:

```bash
Cloning into 'nodejs-docs-hello-world'...
remote: Counting objects: 40, done.
remote: Total 40 (delta 0), reused 0 (delta 0), pack-reused 40
Unpacking objects: 100% (40/40), done.
Checking connectivity... done.
```

> [!NOTE]
> Ukázka index.js Nastaví port pro naslouchání na process.env.PORT. Tato proměnná prostředí je přiřazena službou App Service.
>

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-scus.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-scus.md)]

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ve službě Cloud Shell pomocí příkazu [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) vytvořte v plánu služby App Service `myAppServicePlan` webovou aplikaci.

V následujícím příkladu nahraďte `<app_name>` globálně jedinečným názvem aplikace (platné znaky jsou `a-z`, `0-9` a `-`).

```azurecli-interactive
# Bash and Powershell
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name>
```

Po vytvoření webové aplikace Azure CLI zobrazí výstup podobný následujícímu příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="set-nodejs-runtime"></a>Nastavení modulu runtime Node.js

Nastavte 10.14.1 uzel modulu runtime. Pokud chcete zobrazit všechny podporované moduly runtime, spusťte příkaz [`az webapp list-runtimes`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes).

```azurecli-interactive
# Bash and Powershell
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITE_NODE_DEFAULT_VERSION=10.14.1
```

Přejděte do vaší nově vytvořené webové aplikace. Nahraďte `<app_name>` s jedinečným názvem aplikace.

```
http://<app_name>.azurewebsites.net
```

Vaše nová webová aplikace by měla vypadat takto: ![Prázdná stránka webové aplikace](media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)

## <a name="deploy-zip-file"></a>Nasazení souboru ZIP

Ve službě Cloud Shell přejděte do kořenového adresáře aplikace, vytvořte nový soubor ZIP pro ukázkový projekt.

```azurecli-interactive
cd nodejs-docs-hello-world  

zip -r myUpdatedAppFiles.zip *.*
```

Nasazení souboru ZIP do vaší webové aplikace s použitím [az webapp nasazení zdroj config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) příkazu.  

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src myUpdatedAppFiles.zip
```

Tento příkaz nasadí soubory a adresáře ze souboru ZIP do vaší výchozí složky aplikací služby App Service (`\home\site\wwwroot`) a restartuje aplikaci. Pokud je nakonfigurovaný nějaký vlastní proces sestavení, spustí se také. Další informace najdete v tématu [Kudu dokumentaci](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="browse-to-the-app"></a>Přechod do aplikace

V prohlížeči zadejte adresu nasazené aplikace.

```
http://<app_name>.azurewebsites.net
```

Vzorový kód Node.js je spuštěný ve webové aplikaci služby Azure App Service.

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

> [!NOTE]
> V Azure App Service se aplikace spouští ve službě IIS pomocí modulu [iisnode](https://github.com/Azure/iisnode). Kvůli povolení spuštění aplikace pomocí modulu iisnode obsahuje kořenový adresář aplikace soubor web.config. Tento soubor může číst služba IIS a dokumentaci k nastavením souvisejícím s modulem iisnode najdete v [úložišti iisnode na GitHubu](https://github.com/Azure/iisnode/blob/master/src/samples/configuration/web.config).

**Blahopřejeme!** Nasadili jste svoji první aplikaci Node.js do služby App Service.

## <a name="update-and-redeploy-the-code"></a>Aktualizace a opětovné nasazení kódu

Ve službě Cloud Shell zadejte `code index.js` otevřete editor pro Cloud Shell.

![Kód index.js](media/app-service-web-get-started-nodejs-poc/code-indexjs.png)

Proveďte malou změnu textu ve volání na `response.end`:

```javascript
response.end("Hello Azure!");
```

Uložte změny a zavřete editor. K uložení použijte příkaz `^S` a k zavření příkaz `^Q`.

Vytvořte soubor ZIP a nasaďte ji pomocí [az webapp nasazení zdroj config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) příkazu.  

```azurecli-interactive
# Bash
zip -r myUpdatedAppFiles.zip *.*

az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src myUpdatedAppFiles.zip
```

Vraťte se do okna prohlížeče, které se otevřelo v kroku **Přechod do aplikace**, a aktualizujte zobrazení stránky.

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Správa nové aplikace Azure

Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

V levé nabídce klikněte na tlačítko **App Services**a pak klikněte na název aplikace Azure.

![Přechod do aplikace Azure na portálu](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.

![Stránka služby App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Node.js s databází MongoDB](app-service-web-tutorial-nodejs-mongodb-app.md)
