---
title: Nastavení proměnných prostředí ve službě Azure Container Instances
description: Zjistěte, jak nastavit proměnné prostředí v kontejnerech, které spustíte ve službě Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 11/19/2018
ms.author: danlep
ms.openlocfilehash: 7ec99a79bd1c054c89dffcf7a179725929b89360
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805599"
---
# <a name="set-environment-variables"></a>Nastavení proměnných prostředí

Nastavení proměnných prostředí ve vaší službě container instances umožňuje poskytovat dynamickou konfiguraci aplikace nebo skript spustit v kontejneru. Chcete-li nastavit proměnné prostředí v kontejneru, zadejte je při vytváření instance kontejneru. Můžete nastavit proměnné prostředí při spuštění kontejneru s [rozhraní příkazového řádku Azure](#azure-cli-example), [prostředí Azure PowerShell](#azure-powershell-example)a [webu Azure portal](#azure-portal-example).

Například pokud spustíte [microsoft/aci-wordcount] [ aci-wordcount] image kontejneru, můžete upravit své chování tak, že zadáte následující proměnné prostředí:

*NumWords*: Počet odeslaných do STDOUT slov.

*MinLength*: Minimální počet znaků v aplikaci word, které se mají spočítat. Větší číslo ignoruje běžná slova, jako je "z" a "the".

Pokud je potřeba předat tajné kódy jako proměnné prostředí, Azure Container Instances podporuje [zabezpečení hodnoty](#secure-values) pro kontejnery Windows i Linuxu.

## <a name="azure-cli-example"></a>Příklad rozhraní příkazového řádku Azure

Pokud chcete zobrazit výchozí výstup [microsoft/aci-wordcount] [ aci-wordcount] kontejner, nejprve spustit s tímto [az container vytvořit] [ az-container-create] příkazu (ne proměnné prostředí zadaný):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Pokud chcete upravit výstup, spusťte druhý kontejner s `--environment-variables` argument přidat, zadáte hodnoty pro *NumWords* a *MinLength* proměnné. (Tento příklad předpokládá, že používáte rozhraní příkazového řádku v prostředí Bash nebo Azure Cloud Shell. Pokud používáte příkazového řádku Windows, určete proměnné pomocí dvojité uvozovky, například `--environment-variables "NumWords"="5" "MinLength"="8"`.)

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables 'NumWords'='5' 'MinLength'='8'
```

Jakmile ukazovat stav obou kontejnery *ukončeno* (použijte [az container show] [ az-container-show] postup kontroly stavu), zobrazit svoje protokoly s [protokoly kontejneru az] [ az-container-logs] chcete zobrazit výstup.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

Výstup z kontejnerů ukazují, jak jste upravili chování skriptu druhý kontejner nastavením proměnné prostředí.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Příklad pomocí Azure Powershellu

Nastavení proměnných prostředí v prostředí PowerShell se podobá rozhraní příkazového řádku, ale používá `-EnvironmentVariable` argument příkazového řádku.

Nejprve spusťte [microsoft/aci-wordcount] [ aci-wordcount] kontejneru v její výchozí konfiguraci s tímto [New-AzContainerGroup] [ new-Azcontainergroup] příkaz:

```azurepowershell-interactive
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Nyní spusťte následující příkaz [New-AzContainerGroup] [ new-Azcontainergroup] příkazu. Tohohle Určuje, *NumWords* a *MinLength* proměnné prostředí po naplnění proměnnou pole `envVars`:

```azurepowershell-interactive
$envVars = @{'NumWords'='5';'MinLength'='8'}
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Jakmile se stav obou kontejnery *ukončeno* (použijte [Get-AzContainerInstanceLog] [ azure-instance-log] postup kontroly stavu), o přijetí změn s jejich protokoly [ Get-AzContainerInstanceLog] [ azure-instance-log] příkazu.

```azurepowershell-interactive
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

Výstup pro každý kontejner ukazuje, jak jste upravili skript spusťte kontejnerem nastavením proměnné prostředí.

```console
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Příklad pomocí portálu Azure

Chcete-li nastavit proměnné prostředí při spuštění kontejneru na webu Azure Portal, zadejte je do **konfigurace** stránce při vytváření kontejneru.

Při nasazení pomocí portálu jste aktuálně omezeni na tři proměnné a je nutné je zadat v tomto formátu: `"variableName":"value"`

Chcete-li zobrazit příklad, spusťte [microsoft/aci-wordcount] [ aci-wordcount] kontejner s *NumWords* a *MinLength* proměnné.

1. V **konfigurace**, nastavte **zásady restartování** k *při selhání*
2. Zadejte `"NumWords":"5"` první proměnné, vyberte **Ano** pod **přidat další proměnné prostředí**a zadejte `"MinLength":"8"` druhé proměnné. Vyberte **OK** ověřte a pak nasaďte kontejner.

![Stránce portálu zobrazují prostředí proměnné povolení tlačítka a textová pole][portal-env-vars-01]

Chcete-li zobrazit protokoly kontejneru, v části **nastavení** vyberte **kontejnery**, pak **protokoly**. Podobně jako výstup ukazuje předchozí rozhraní příkazového řádku a Powershellu oddíly, že uvidíte, jak byla změněna skript chování proměnné prostředí. Zobrazují se jenom pět slova, každý s minimální délku osmi znaků.

![Portalu zobrazující výstup protokolu kontejneru][portal-env-vars-02]

## <a name="secure-values"></a>Zabezpečených hodnot

Objekty zabezpečené hodnotami jsou určeny pro uchování citlivých informací, jako jsou hesla nebo klíče pro vaši aplikaci. Použití zabezpečených hodnot pro proměnné prostředí je bezpečnější a flexibilnější, než jejího zahrnutí do image vašeho kontejneru. Další možností je použití tajné svazky, je popsáno v [připojit tajný svazek ve službě Azure Container Instances](container-instances-volume-secret.md).

Proměnné prostředí s zabezpečených hodnot nejsou viditelná ve vlastnostech vašeho kontejneru – jejich hodnoty je přístupný pouze z v rámci kontejneru. Například vlastnosti kontejneru zobrazit v portálu Azure portal nebo rozhraní příkazového řádku Azure zobrazení pouze zabezpečenou proměnnou název, ne jeho hodnotu.

Nastavte proměnnou zabezpečené prostředí tak, že zadáte `secureValue` vlastnost místo standardní `value` pro typ proměnné. Dvě proměnné, které jsou definovány v následující kód YAML ukazují tyto dva typy proměnných.

### <a name="yaml-deployment"></a>Nasazení YAML

Vytvoření `secure-env.yaml` souboru následujícím fragmentem kódu.

```yaml
apiVersion: 2018-10-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - name: 'NOTSECRET'
          value: 'my-exposed-value'
        - name: 'SECRET'
          secureValue: 'my-secret-value'
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Spuštěním následujícího příkazu nasaďte skupinu kontejnerů s YAML (název skupiny prostředků podle potřeby upravte):

```azurecli-interactive
az container create --resource-group myResourceGroup --file secure-env.yaml
```

### <a name="verify-environment-variables"></a>Zkontrolujte proměnné prostředí

Spustit [az container show] [ az-container-show] příkazu proměnné prostředí vašeho kontejneru dotazu:

```azurecli-interactive
az container show --resource-group myResourceGroup --name securetest --query 'containers[].environmentVariables'
```

Odpověď JSON ukazuje, jak proměnnou prostředí nezabezpečené klíč a hodnotu, ale pouze název proměnné zabezpečeném prostředí:

```json
[
  [
    {
      "name": "NOTSECRET",
      "secureValue": null,
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET",
      "secureValue": null,
      "value": null
    }
  ]
]
```

S [az container exec] [ az-container-exec] příkaz, který umožňuje spuštění příkazu v spuštěný kontejner, můžete ověřit, jestli je nastavená proměnná zabezpečeném prostředí. Spuštěním následujícího příkazu spusťte prostředí bash interaktivní relaci v kontejneru:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name securetest --exec-command "/bin/bash"
```

Po otevření interaktivní prostředí v rámci kontejneru, dostanete `SECRET` hodnota proměnné:

```console
root@caas-ef3ee231482549629ac8a40c0d3807fd-3881559887-5374l:/# echo $SECRET
my-secret-value
```

## <a name="next-steps"></a>Další postup

Scénáře založené na úlohách, jako jsou dávkové zpracování velkou datovou sadu s několika kontejnery, může přinést vlastní proměnné prostředí za běhu. Další informace o spuštěné kontejnery založené na úlohách najdete v tématu [spouštění kontejnerizovaných úloh ve službě Azure Container Instances](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-Azcontainerinstancelog
[azure-powershell-install]: /powershell/azure/azurerm/install-Az-ps
[new-Azcontainergroup]: /powershell/module/azurerm.containerinstance/new-Azcontainergroup
[portal]: https://portal.azure.com
