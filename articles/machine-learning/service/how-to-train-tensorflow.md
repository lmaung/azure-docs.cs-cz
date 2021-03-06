---
title: Trénování modelů s TensorFlow & Keras
titleSuffix: Azure Machine Learning service
description: Zjistěte, jak spustit jeden uzel nebo pro distribuované trénování TensorFlow a Keras modelů s TensorFlow a Keras odhady
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 02/21/2019
ms.custom: seodec18
ms.openlocfilehash: b06151b9cb5603ef54c8e84d261bb7a4e7885e1b
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56880457"
---
# <a name="train-tensorflow-and-keras-models-with-azure-machine-learning-service"></a>Trénování TensorFlow a Keras modelů pomocí služby Azure Machine Learning

Pro výuku hluboké neuronové sítě (DNN) s využitím TensorFlow, Azure Machine Learning nabízí vlastní `TensorFlow` třídu `Estimator`. Azure SDK `TensorFlow` estimator (nechcete conflated s [ `tf.estimator.Estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator) třídy) vám umožní snadno odesílat úlohy trénování TensorFlow pro spuštění jedním uzlem a distribuované na výpočetní prostředky Azure.

## <a name="single-node-training"></a>Trénování jedním uzlem
Školení s `TensorFlow` odhad je podobný používání [základní `Estimator` ](how-to-train-ml-models.md), proto nejdřív přečíst článek s postupy a ujistěte se, že rozumíte konceptům uvedeným existuje.
  
Ke spuštění úlohy TensorFlow, vytvořit instanci `TensorFlow` objektu. Měli jste již vytvořili vaše [cílové výpočetní prostředí](how-to-set-up-training-targets.md#amlcompute) objekt `compute_target`.

```Python
from azureml.train.dnn import TensorFlow

script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py',
                    conda_packages=['scikit-learn'], # in case you need scikit-learn in train.py
                    use_gpu=True)
```

Jsme zde, zadejte následující parametry pro konstruktor TensorFlow:

Parametr | Popis
--|--
`source_directory` | Místní adresář, který obsahuje vše potřebné pro trénovací úlohu kódu. Tato složka se zkopíruje z místního počítače pro vzdálený výpočetní
`script_params` | Slovník zadání argumentů příkazového řádku pro cvičný skript `entry_script`, ve formě < argument příkazového řádku, hodnota > páry
`compute_target` | Cílové vzdálené výpočetní prostředí, který cvičný skript se spustí, v tomto případě Azure Machine Learning Compute ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) clusteru
`entry_script` | Cesta k souboru (vzhledem k `source_directory`) z trénovací skript ke spuštění na vzdálené výpočetní prostředky. Tento soubor a další soubory, na kterých závisí, se musí nacházet v této složce
`conda_packages` | Seznam balíčků Python nainstalovat přes conda vyžadované cvičný skript. V tomto případě cvičný skript používá `sklearn` pro načítání dat, proto zadejte tento balíček k instalaci.  Konstruktor má jiný parametr s názvem `pip_packages` , můžete použít pro všechny balíčky pip potřeby
`use_gpu` | Tento příznak nastavit `True` využití GPU pro vzdělávání. Výchozí hodnota je `False`.

Vzhledem k tomu, že používáte TensorFlow estimator, bude použita výchozí kontejner použitý k trénování TensorFlow balíčku a související závislosti nutné ke školení na procesory a grafickými procesory.

Pak odešlete úlohu TensorFlow:
```Python
run = exp.submit(tf_est)
```

## <a name="keras-support"></a>Podpora Keras
[Keras](https://keras.io/) je Oblíbené vysoké úrovně DNN rozhraní Python API, která podporuje TensorFlow, CNTK nebo Theano jako back-EndY. Pokud používáte TensorFlow jako back-end, snadno můžete TensFlow estimator k natrénování modelu Keras. Tady je příklad TensorFlow odhaduje se přidá do něj Keras:

```Python
from azureml.train.dnn import TensorFlow

keras_est = TensorFlow(source_directory='./my-keras-proj',
                       script_params=script_params,
                       compute_target=compute_target,
                       entry_script='keras_train.py',
                       pip_packages=['keras'], # just add keras through pip
                       use_gpu=True)
```
Výše uvedené konstruktor estimator TensorFlow instruuje služby Azure Machine Learning k instalaci Keras pomocí pip do prostředí provádění. A `keras_train.py` pak můžete importovat rozhraní Keras API k natrénování modelu Keras. Úplný příklad, prozkoumejte [tento poznámkový blok Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb).

## <a name="distributed-training"></a>Distribuované trénování
Odhad TensorFlow také umožňuje trénování modelů ve velkém měřítku napříč clustery CPU a GPU virtuálních počítačů Azure. Můžete snadno spouštět distribuované trénování tensorflowu se několik volání rozhraní API, zatímco bude Azure Machine Learning spravovat na pozadí, infrastruktury a potřeby provádět tyto úlohy Orchestrace.

Azure Machine Learning podporuje dvě metody distribuované trénování TensorFlow:
* Na základě MPI distribuované trénování pomocí [Horovod](https://github.com/uber/horovod) framework
* nativní [distribuované TensorFlow](https://www.tensorflow.org/deploy/distributed) prostřednictvím parametru metody serveru

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) je kanál – allreduce open source platforma pro distribuované trénování vypracovanou organizací cccppf Uber.

Ke spuštění distribuovaných TensorFlow pomocí rozhraní Horovod, vytvořte objekt TensorFlow následujícím způsobem:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi',
                    use_gpu=True)
```

Výše uvedený kód zveřejňuje následující nové parametry pro konstruktor TensorFlow:

Parametr | Popis | Výchozí
--|--|--
`node_count` | Počet uzlů pro trénovací úlohu. | `1`
`process_count_per_node` | Počet procesů (nebo "pracovníky") pro spuštění na každý uzel.|`1`
`distributed_backend` | Back-endu pro spouštění distribuovaných školení, která nabízí odhadu prostřednictvím MPI. Pokud chcete provést paralelní nebo distribuované trénování (například `node_count`> 1 nebo `process_count_per_node`> 1 nebo obě) MPI (a Horovod), nastavte `distributed_backend='mpi'`. Implementace MPI používá technologii Azure Machine Learning je [otevřít MPI](https://www.open-mpi.org/). | `None`

Výše uvedený příklad spustí distribuované trénování s dva pracovní procesy, jeden pracovní proces na jeden uzel.

Horovod a jeho závislosti se nainstaluje za vás, tak importujte ji do vašeho skriptu školení `train.py` následujícím způsobem:

```Python
import tensorflow as tf
import horovod
```

Nakonec odešlete úlohu TensorFlow:
```Python
run = exp.submit(tf_est)
```

### <a name="parameter-server"></a>Server parametrů
Můžete také spustit [nativní distribuované TensorFlow](https://www.tensorflow.org/deploy/distributed), model serveru parametr, který používá. V této metodě trénování napříč clusterem parametr serverů a pracovních procesů. Zaměstnanci vypočítat přechody během cvičení, zatímco parametr servery agregovat přechody.

Vytvoření objektu TensorFlow:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps',
                    use_gpu=True)
```

Věnujte pozornost na následující parametry pro konstruktor TensorFlow ve výše uvedeném kódu:

Parametr | Popis | Výchozí
--|--|--
`worker_count` | Počet pracovních procesů. | `1`
`parameter_server_count` | Počet serverů parametrů. | `1`
`distributed_backend` | Back-end pro distribuované trénování. Nastavení provedete distribuované trénování prostřednictvím parametrů serveru `distributed_backend='ps'` | `None`

#### <a name="note-on-tfconfig"></a>Poznámka: na `TF_CONFIG`
Budete také potřebovat síťové adresy a porty u clusteru [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), takže se nastaví Azure Machine Learning `TF_CONFIG` proměnnou prostředí za vás.

`TF_CONFIG` Proměnná prostředí je řetězec formátu JSON. Tady je příklad proměnné pro parametr serveru:
```
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Pokud používáte TensorFlow na vysoké úrovni [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) rozhraní API, TensorFlow, provede analýzu to `TF_CONFIG` proměnné a sestavení clusteru spec za vás. 

Pokud místo toho použijete TensorFlow na nižší úrovni základní rozhraní API pro trénování, budete muset analyzovat `TF_CONFIG` proměnné a sestavení `tf.train.ClusterSpec` sami v kódu školení. V [v tomto příkladu](https://aka.ms/aml-notebook-tf-ps), provedli byste proto v **cvičný skript** následujícím způsobem:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

Po dokončení zápisu cvičný skript a vytváření objektu TensorFlow, můžete odeslat trénovací úlohy:
```Python
run = exp.submit(tf_est)
```

## <a name="examples"></a>Příklady

Prozkoumejte různé [poznámkových bloků v distribuované obsáhlého learningu na Githubu](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Další postup
* [Sledovat spustit metriky během cvičení](how-to-track-experiments.md)
* [Vyladění hyperparameters](how-to-tune-hyperparameters.md)
* [Nasazení trénovaného modelu](how-to-deploy-and-where.md)
