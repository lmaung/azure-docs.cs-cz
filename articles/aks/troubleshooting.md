---
title: S řešením běžných problémů služby Azure Kubernetes
description: Zjistěte, jak odstraňovat potíže a řešit běžné problémy při použití služby Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 53061d4d09ac2769e59269701467a22f292cd919
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959761"
---
# <a name="aks-troubleshooting"></a>Řešení potíží s AKS

Při vytváření nebo spravovat clustery Azure Kubernetes Service (AKS), čas od času může dojít k potížím. Tento článek podrobně popisuje některé běžné problémy a postup řešení potíží.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>Obecně platí, kde najdu informace o ladění problémů Kubernetes?

Zkuste [oficiální příručce pro řešení potíží s clustery Kubernetes](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).
K dispozici je také [Průvodce odstraňováním potíží](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md), vydaná technický pracovník Microsoftu pro řešení potíží s podů, uzly, clustery a další funkce.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Chyba "byla překročena kvóta" dochází při vytvoření nebo aktualizace. Co bych měl/a dělat? 

Je potřeba [žádosti jader](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>Co je maximální nastavení podů podle uzlů AKS?

Maximální nastavení podů jeden uzel je 30 ve výchozím nastavení, pokud nasadíte cluster AKS na portálu Azure portal.
Maximální nastavení podů jeden uzel je 110 ve výchozím nastavení, pokud nasadíte cluster AKS v Azure CLI. (Ujistěte se, že používáte nejnovější verzi Azure CLI). Toto výchozí nastavení lze změnit pomocí `–-max-pods` příznak v `az aks create` příkazu.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>InsufficientSubnetSize chybě dochází při nasazování clusteru AKS pomocí rozšířeného sítě. Co bych měl/a dělat?

Pokud se používá Azure CNI (rozšířeného sítě), AKS preallocates IP adresy podle "max podů" podle počtu uzlů nakonfigurované. Počet uzlů v clusteru AKS může být kdekoli v rozmezí 1 a 110. Na základě nakonfigurovaných maximální počet podů podle počtu uzlů, velikost podsítě musí být větší než "součinu počtu používaných uzlů a maximální počet podů na uzel". Následující základní rovnice popisuje toto:

Velikost podsítě > počet uzlů v clusteru (s přihlédnutím budoucí požadavky na škálování) * max podů na jeden uzel.

Další informace najdete v tématu [naplánujte IP adresování pro váš cluster](configure-azure-cni.md#plan-ip-addressing-for-your-cluster).

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Moje pod se zasekla v automatickém režimu CrashLoopBackOff. Co bych měl/a dělat?

Mohou existovat různé důvody pod se zablokuje a v tomto režimu. Podívejte se do:

* Pod, s použitím `kubectl describe pod <pod-name>`.
* Protokoly pomocí `kubectl log <pod-name>`.

Další informace o řešení problémů v podu najdete v tématu [ladění aplikací](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Pokouším RBAC povolit v existujícím clusteru. Jak to mám udělat?

Bohužel povolení řízení přístupu na základě role (RBAC) na stávajících clusterů není v tuto chvíli nepodporuje. Musíte explicitně vytvořit nových clusterů. Pokud používáte rozhraní příkazového řádku, je standardně povolená RBAC. Pokud používáte portál AKS, není k dispozici v pracovním postupu vytvoření přepínací tlačítko Povolit RBAC.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Vytvoření clusteru pomocí RBAC povolit pomocí Azure CLI s výchozí hodnoty nebo na webu Azure portal a nyní zobrazuje počet upozornění na řídicí panel Kubernetes. Řídicí panel použitý k bez upozornění fungovat. Co bych měl/a dělat?

Důvod pro upozornění na řídicím panelu je, že cluster je nyní povoleno pomocí RBAC a byl zakázán přístup k němu ve výchozím nastavení. Obecně platí tento přístup je dobrým zvykem, protože výchozí vystavení řídicího panelu pro všechny uživatele clusteru může vést k ohrožení zabezpečení. Pokud budete chtít povolit řídicího panelu, postupujte podle kroků v [tento příspěvek na blogu](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Nejde se připojit k řídicímu panelu. Co bych měl/a dělat?

Nejjednodušší způsob, jak získat přístup k vaší službě mimo cluster, je spustit `kubectl proxy`, proxy servery požadavky odeslané na místním hostiteli port 8001 na serveru Kubernetes API. Odtud můžete API server proxy serveru do služby: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Pokud se nezobrazí řídicí panel Kubernetes, zkontrolujte, zda `kube-proxy` pod běží v `kube-system` oboru názvů. Pokud není ve spuštěném stavu, odstraňte pod a bude restartován.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Nedaří se mi zprovoznit protokoly pomocí kubectl protokoly nebo nejde se připojit k serveru rozhraní API. Získávám "Chyba ze serveru: Chyba volání back-endu: vytáčení tcp...". Co bych měl/a dělat?

Ujistěte se, že se nezmění výchozí skupinu zabezpečení sítě a, že je port 22 otevřený pro připojení k serveru rozhraní API. Zkontrolujte, zda `tunnelfront` pod běží v *kube-system* pomocí oboru názvů `kubectl get pods --namespace kube-system` příkazu. Pokud tomu tak není, Vynutit odstranění pod a restartuje.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Mohu provést upgrade nebo změna velikosti a Power BI Desktop zobrazuje "zpráva: Chyba při změně hodnoty vlastnosti 'elementu imageReference' není povoleno". Jak tento problém vyřešit?

Vám může zobrazovat tato chyba vzhledem k tomu, že jste upravili značky v agentské uzly v clusteru AKS. Úprava a odstranění značek a dalších vlastností prostředků ve skupině prostředků MC_ * může vést k neočekávaným výsledkům. Upravit prostředky ve skupině MC_ * AKS clusteru dělí cíle úrovně služeb (SLO).

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Zobrazuje chyby, které se cluster je v chybovém stavu a upgrade nebo změna velikosti nebude fungovat, dokud se nevyřeší

*Tato pomoc s řešením potíží se přesměruje z https://aka.ms/aks-cluster-failed*

Tato chyba nastane, pokud clustery zadejte stavu selhání z několika důvodů. Postupujte podle následujících kroků, abyste před opakováním dříve nezdařené operace vyřešit stav vašeho clusteru se nezdařilo:

1. Dokud je cluster z `failed` stavu, `upgrade` a `scale` operace nebude úspěšné. Běžné problémy kořenové a jejich řešení patří:
    * Škálování s **kvóta dostatek výpočetních prostředků (CRP)**. Pokud chcete vyřešit, nejprve škálování clusteru zpět do stavu stabilní cíle v rámci kvóty. Potom postupujte podle těchto [postup k vyžádání kvóta výpočetních prostředků zvýšit](../azure-supportability/resource-manager-core-quotas-request.md) před pokusem o vertikální navýšení kapacity znovu za počáteční kvóty.
    * Škálování clusteru se službou rozšířeného sítě a **prostředků nedostatečná podsítě (síť)**. Pokud chcete vyřešit, nejprve škálování clusteru zpět do stavu stabilní cíle v rámci kvóty. Potom postupujte podle [zvýšení postupem požadovat kvóty prostředků](../azure-resource-manager/resource-manager-quota-errors.md#solution) před pokusem o vertikální navýšení kapacity znovu za počáteční kvóty.
2. Po vyřešení základní příčinu selhání upgradu clusteru musí být v úspěšném stavu. Po ověření bylo úspěšné. stav původní operaci opakujte.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Zobrazuje chyby při pokusu o upgrade nebo určený počet číslic, které stav clusteru se aktuálně vrácení upgradu nebo selhal upgrade

*Tato pomoc s řešením potíží se přesměruje z https://aka.ms/aks-pending-upgrade*

Operace clusteru jsou omezené, když dochází k upgradu aktivní operace nebo byl pokus o upgrade, ale následně se nezdařilo. K diagnostice problému spusťte `az aks show -g myResourceGroup -n myAKSCluster -o table` získat podrobný stav ve vašem clusteru. Na základě výsledku:

* Pokud je aktivním upgradu clusteru, počkejte, až do doby ukončení operace. Pokud byla úspěšná, zkuste to znovu dříve nezdařené operace.
* Pokud cluster selhal upgrade, postupujte podle kroků uvedených [výše](#im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade-directed-from-httpsakamsaks-pending-upgrade)