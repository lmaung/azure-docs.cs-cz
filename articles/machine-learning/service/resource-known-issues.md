---
title: Známé problémy a řešení potíží
titleSuffix: Azure Machine Learning service
description: Získat seznam známých problémů, řešení a řešení potíží pro službu Azure Machine Learning.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 3cf71de72a6005c59d76e2d88059a1ae16ec2970
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56817469"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Známé problémy a řešení problémů služby Azure Machine Learning

Tento článek vám pomůže najít a opravit chyby nebo při použití služby Azure Machine Learning došlo k selhání.

## <a name="sdk-installation-issues"></a>Problémy při instalaci sady SDK

**Chybová zpráva: Nelze odinstalovat 'PyYAML.**

Azure Machine Learning pro Python SDK: PyYAML je projektem organizace nainstalovaná distutils. Proto jsme nelze určit přesné soubory, které patří k němu dojde částečné odinstalovat. Pokud chcete pokračovat v instalaci sady SDK při tato chyba se ignoruje, použijte:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problémy s vytvářením, Azure Machine Learning Compute

Je vzácné pravděpodobné, že někteří uživatelé, kteří si vytvořili jejich pracovního prostoru Azure Machine Learning z portálu Azure portal před verze GA nemusí být možné vytvořit Azure Machine Learning Compute v daném pracovním prostoru. Můžete zvýšit žádost o podporu na službu nebo vytvořit nový pracovní prostor prostřednictvím portálu nebo pomocí sady SDK pro odblokování sami okamžitě.

## <a name="image-building-failure"></a>Chyba vytváření bitové kopie

Obrázek po nasazení webové služby vytvářet selhání. Alternativním řešením je přidat "pynacl == 1.2.1" jako pip závislosti systému Conda v souboru konfigurace image.

## <a name="deployment-failure"></a>Nasazení se nezdařilo.

Pokud zjistíte `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`, změna SKU pro virtuální počítače použité ve vašem nasazení, který má více paměti.

## <a name="fpgas"></a>FPGA
Nebude moct nasazovat modely na FPGA, dokud si vyžádáte a byla schválena pro FPGA kvótu. Chcete-li požádat o přístup, vyplňte formulář žádosti o kvóty: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Problémy s Databricks a Azure Machine Learning.

### <a name="failure-when-installing-packages"></a>Chyba při instalaci balíčků
Azure Machine Learning SDK chyby při instalaci v Databricks při instalaci dalších balíčků Některé balíčky, jako například `psutil`, může způsobit konflikty. Aby nedocházelo k chybám instalace, instalace balíčků zmrazení lib verzí. Tento problém souvisí s Databricks a není službou Azure Machine Learning SDK – které mohou nastat ho pomocí jiných knihoven příliš. Příklad:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
Alternativně můžete použít skripty init, pokud je zachovat otočena směrem problémů s instalací s knihoven Pythonu. Tento přístup není oficiálně podporovaných přístup. Můžete se podívat do [tohoto dokumentu](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

### <a name="cancel-an-automated-ml-run"></a>Zrušit automatizovaný běh ML
Při použití automatického technologie strojového učení v Databricks, pokud chcete zrušit běh a začněte nový experiment spustit, restartujte cluster Azure Databricks.

### <a name="10-iterations-for-automated-ml"></a>> 10 iterací pro automatizované ML
V nastavení automatizovaných ml, pokud máte více než 10 iterací, nastavte `show_output` k `False` odeslání příkazu run.


## <a name="azure-portal"></a>portál Azure
Pokud přejdete přímo na váš pracovní prostor z sdílet odkaz ze sady SDK nebo na portálu zobrazit, nebudete moct zobrazit stránka s přehledem normální s informace o předplatném v rozšíření. Nebudete také moci přepnout do jiného pracovního prostoru. Pokud je potřeba jiný pracovní prostor zobrazit, alternativním řešením je přejít přímo na [webu Azure portal](https://portal.azure.com) a vyhledejte název pracovního prostoru.

## <a name="diagnostic-logs"></a>Diagnostické protokoly
V některých případech může být užitečné, pokud může poskytnout diagnostické informace, pokud s žádostí o pomoc.
Zde je, kde live soubory protokolu:

## <a name="resource-quotas"></a>Kvóty prostředků

Další informace o [kvóty prostředků](how-to-manage-quotas.md) můžete setkat při práci se službou Azure Machine Learning.

## <a name="authentication-errors"></a>Chyby ověřování

Pokud provádíte operaci správy na cílové výpočetní prostředí ze vzdálené úlohy, zobrazí se jedné z následujících chyb:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Například můžete dojde k chybě při pokusu vytvořit nebo připojit cílové výpočetní prostředí z kanálu služby ML, které je odeslána pro vzdálené spuštění.

## <a name="get-more-support"></a>Získat další podporu.

Můžete odesílat žádosti o podporu a získat pomoc od technické podpory, fóra a další. [Víc se uč...](support-for-aml-services.md)
