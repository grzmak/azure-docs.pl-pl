---
title: Wypychanie obrazu platformy Docker do prywatnego rejestru platformy Azure
description: Wypychanie i ściąganie obrazów platformy Docker do prywatnego rejestru kontenerów na platformie Azure za pomocą interfejsu wiersza polecenia platformy Docker
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 11/29/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 24cccd4745d611196046168f0125e7ef2a184e15
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576495"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Wypchnij swój pierwszy obraz do prywatnego rejestru kontenerów platformy Docker za pomocą interfejsu wiersza polecenia platformy Docker

Rejestr kontenera platformy Azure przechowuje prywatne obrazy kontenerów platformy [Docker](http://hub.docker.com) i zarządza nimi podobnie, jak [koncentrator platformy Docker](https://hub.docker.com/) przechowuje publiczne obrazy platformy Docker. Możesz użyć [interfejsu wiersza polecenia platformy Docker](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) dla [logowania](https://docs.docker.com/engine/reference/commandline/login/), [wypychania](https://docs.docker.com/engine/reference/commandline/push/), [ściągnięcia](https://docs.docker.com/engine/reference/commandline/pull/)oraz innych operacji na kontenerze rejestr.

W poniższych krokach możesz pobrać oficjalnego [obraz Nginx](https://store.docker.com/images/nginx) z publicznego rejestru koncentratora platformy Docker, otaguj go do swojej prywatnej usługi Azure container registry, Wypchnij go do rejestru i wyciągniesz go z rejestru.

## <a name="prerequisites"></a>Wymagania wstępne

* **Usługa Azure Container Registry** — Tworzy rejestr kontenera w subskrypcji platformy Azure. Na przykład użyć [witryny Azure portal](container-registry-get-started-portal.md) lub [wiersza polecenia platformy Azure](container-registry-get-started-azure-cli.md).
* **Interfejs CLI platformy docker** — Aby skonfigurować lokalny komputer jako hosta platformy Docker i uzyskać dostęp do poleceń interfejsu wiersza polecenia platformy Docker, zainstaluj [Docker](https://docs.docker.com/engine/installation/).

## <a name="log-in-to-a-registry"></a>Zaloguj się do rejestru

Istnieją [kilka sposobów uwierzytelniania](container-registry-authentication.md) do prywatnego rejestru kontenera. Zalecaną metodą podczas pracy w wierszu polecenia jest poleceniem wiersza polecenia platformy Azure [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login). Na przykład, aby zalogować się do rejestru o nazwie *myregistry*:

```azurecli
az acr login --name myregistry
```

Możesz również zalogować się przy użyciu [docker login](https://docs.docker.com/engine/reference/commandline/login/). Poniższy przykład przekazuje identyfikator i hasło [nazwy głównej usługi](../active-directory/develop/app-objects-and-service-principals.md) Azure Active Directory. Na przykład, Niewykluczone, że [przypisać nazwę główną usługi](container-registry-authentication.md#service-principal) do rejestru dla scenariusza automatyzacji.

```Bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Oba polecenia zwracają `Login Succeeded` po ukończeniu. Jeśli używasz `docker login`, może być również wyświetlany ostrzeżenie o zabezpieczeniach zalecające użycie `--password-stdin` parametru. Użycie tego parametru wykracza poza zakres tego artykułu, jednak zalecamy zastosowanie tego najlepszego rozwiązania. Aby uzyskać więcej informacji, zobacz [docker login](https://docs.docker.com/engine/reference/commandline/login/) poleceń.

> [!TIP]
> Zawsze określać w pełni kwalifikowana nazwa rejestru (wszystkie małe litery) zastosowania `docker login` i po tagu Wypychanie do rejestru obrazów. W przykładach w tym artykule, jest w pełni kwalifikowana nazwa *myregistry.azurecr.io*.

## <a name="pull-the-official-nginx-image"></a>Ściągnij oficjalny obraz serwera Nginx

Najpierw Ściągnij publiczny obraz kontenera Nginx na komputerze lokalnym.

```Bash
docker pull nginx
```

## <a name="run-the-container-locally"></a>Uruchamianie kontenera w środowisku lokalnym

Wykonaj poniższe [platformy docker, uruchom](https://docs.docker.com/engine/reference/run/) polecenie, aby uruchomić lokalne wystąpienie kontenera Nginx interaktywnie (`-it`) na porcie 8080. `--rm` Argument określa, że kontener powinny zostać usunięte po jej zatrzymaniu.

```Bash
docker run -it --rm -p 8080:80 nginx
```

Przejdź do [ http://localhost:8080 ](http://localhost:8080) można wyświetlić domyślną stronę sieci web obsługiwane przez serwer Nginx w uruchomionego kontenera. Powinieneś widzieć stronę podobny do następującego:

![Kontener Nginx na komputerze lokalnym](./media/container-registry-get-started-docker-cli/nginx.png)

Ponieważ uruchomiony kontener interaktywnie za pomocą `-it`, zostanie wyświetlony, serwer Nginx danych wyjściowych w wierszu polecenia po przejściu do niego w przeglądarce.

Aby zatrzymać i usunąć kontener, naciśnij klawisz `Control` + `C`.

## <a name="create-an-alias-of-the-image"></a>Tworzenie aliasu obrazu

Użyj [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) utworzyć alias obrazu z w pełni kwalifikowaną ścieżką do Twojego rejestru. W tym przykładzie jest określana przestrzeń nazw `samples`, aby uniknąć zaśmiecania katalogu głównego rejestru.

```Bash
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Aby uzyskać więcej informacji na temat znakowanie z obszary nazw, zobacz [przestrzenie nazw repozytoriów](container-registry-best-practices.md#repository-namespaces) części [najlepsze rozwiązania dotyczące usługi Azure Container Registry](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>Wypchnij obraz do rejestru

Teraz, po otagowaniu obrazu z w pełni kwalifikowaną ścieżką do rejestru prywatnego, można wypchnąć go do rejestru przy użyciu [docker push](https://docs.docker.com/engine/reference/commandline/push/):

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Ściągnij obraz z rejestru

Użyj [polecenie docker pull](https://docs.docker.com/engine/reference/commandline/pull/) polecenie, aby ściągnąć obraz z rejestru:

```Bash
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Uruchamianie kontenera Nginx

Użyj [platformy docker, uruchom](https://docs.docker.com/engine/reference/run/) polecenia do uruchomienia obrazu zostały pobrane z rejestru:

```Bash
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Przejdź do [ http://localhost:8080 ](http://localhost:8080) Aby wyświetlić działający kontener.

Aby zatrzymać i usunąć kontener, naciśnij klawisz `Control` + `C`.

## <a name="remove-the-image-optional"></a>Usuń obraz (opcjonalnie)

Jeśli nie potrzebujesz już obraz Nginx, możesz usunąć ją lokalnie za pomocą [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) polecenia.

```Bash
docker rmi myregistry.azurecr.io/samples/nginx
```

Aby usunąć obrazy z usługi Azure container registry, można użyć polecenia wiersza polecenia platformy Azure [az acr repozytorium delete](/cli/azure/acr/repository#az-acr-repository-delete). Na przykład następujące polecenie usuwa manifest odwołuje się tag, wszelkie dane skojarzone warstwy i wszystkie inne tagi odwołujące się do manifestu.

```azurecli
az acr repository delete --name myregistry --repository samples/nginx --tag latest --manifest
```

## <a name="next-steps"></a>Kolejne kroki

Skoro znasz już podstawy, możesz korzystać z rejestru! Wdrażanie obrazów kontenera z rejestru na:

* [Usługa Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md)
* [Usługa Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)
