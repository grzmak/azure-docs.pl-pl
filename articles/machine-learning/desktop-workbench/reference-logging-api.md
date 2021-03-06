---
title: Dokumentacja usługi Azure ML API rejestrowania | Dokumentacja firmy Microsoft
description: Rejestrowanie dokumentacja interfejsu API.
services: machine-learning
author: akshaya-a
ms.author: akannava
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/25/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 7084251102984445e7c2341b78b44f85811ebea7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958224"
---
# <a name="logging-api-reference"></a>Dokumentacja interfejsu API rejestrowania

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Biblioteki rejestrowania usługi Azure ML pozwala programowi na tworzyć metryki i pliki, które są śledzone przez usługę historii do późniejszej analizy. 

## <a name="uploading-metrics"></a>Przekazywanie metryki

```python
# import logging API package
from azureml.logging import get_azureml_logger

# initialize a logger object
logger = get_azureml_logger()

# log "scalar" metrics
logger.log("simple integer value", 7)
logger.log("simple float value", 3.141592)
logger.log("simple string value", "this is a string metric")

# log a list of numerical values. 
# this automatically creates a chart in the Run History details page
logger.log("chart data points", [1, 3, 5, 10, 6, 4])
```

Domyślnie wszystkie metryki są przesyłane asynchronicznie, tak, aby przesyłania nie utrudniać wykonywania programu. Może to spowodować szeregowania problemy, gdy wiele metryk są wysyłane w przypadki brzegowe. Na przykład będzie dwie metryki rejestrowane, w tym samym czasie, ale w przypadku niektórych powodów, dla którego użytkownik wolisz dokładnie porządkowanie zachowane. Innym przypadkiem jest gdy Metryka musi być śledzona, przed uruchomieniem kodu, które jest znana potencjalnie awaria następuje szybko. W obu przypadkach rozwiązaniem jest _oczekiwania_ aż metryka jest w całości przed kontynuowaniem:

```python
# blocking call
logger.log("my metric 1", 1).wait()
logger.log("my metric 2", 2).wait()
```

## <a name="consuming-metrics"></a>Korzystanie z metryk

Metryki są przechowywane w usłudze historii i powiązane z przebiegu, który je. Karta Historia uruchamiania i poniższego polecenia interfejsu wiersza polecenia umożliwiają pobrać je (i artefakty poniżej), po zakończeniu przebiegu.

```azurecli
# show the last run
$ az ml history last

# list all past runs
$ az ml history list 

# show a paritcular run
$ az ml history info -r <runid>
```

## <a name="artifacts-files"></a>Artefakty (pliki)

Oprócz metryk usługi Azure ml umożliwia użytkownikowi również pliki śledzenia. Domyślnie, wszystkie pliki zapisane w `outputs` folderu względem katalogu roboczego programu (folder projektu w kontekście obliczeniowym) są przekazywane do usługi historii i śledzone do późniejszej analizy. Zastrzeżenie: to, że rozmiar poszczególnych plików musi być mniejszy niż 512 MB.


```Python
# Log content as an artifact
logger.upload("artifact/path", "This should be the contents of artifact/path in the service")
```

## <a name="consuming-artifacts"></a>Korzystanie z artefaktów

Aby wydrukować zawartość artefaktów, które były śledzone, użytkownika można użyć na karcie Historia uruchamiania dla danego uruchomienia do **Pobierz** lub **Podwyższ poziom** artefaktu lub użyj poniższych poleceń interfejsu wiersza polecenia, aby osiągnąć ten sam efekt.

```azurecli
# show all artifacts generated by a run
$ az ml history info -r <runid> -a <artifact/path>

# promote a particular artifact
$ az ml history promote -r <runid> -ap <artifact/prefix> -n <name of asset to create>
```
## <a name="next-steps"></a>Kolejne kroki
- Przeprowadzenie [klasyfikowania tutoria irysów, część 2](tutorial-classifying-iris-part-2.md) się interfejs API rejestrowania akcji.
- Przegląd [sposobu użycia historii uruchamiania i metryk modelu w aplikacji Azure Machine Learning Workbench](how-to-use-run-history-model-metrics.md) zrozumienie lepszy sposób rejestrowania interfejsów API można używać w historii uruchamiania.
