---
title: Samouczek — automatyzowanie kompilacji obrazu kontenera w ramach aktualizacji obrazu podstawowego za pomocą usługi Azure Container Registry Tasks
description: Z tego samouczka dowiesz się, jak skonfigurować zadanie w celu automatycznego wyzwalania kompilacji obrazu kontenera w chmurze po zaktualizowaniu obrazu podstawowego.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 54e8892787fa2b7b093609ee5d09f3a87e103411
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856585"
---
# <a name="tutorial-automate-image-builds-on-base-image-update-with-azure-container-registry-tasks"></a>Samouczek: automatyzowanie kompilacji obrazu w ramach aktualizacji obrazu podstawowego za pomocą usługi Azure Container Registry Tasks

Usługa ACR Tasks obsługuje automatyczne wykonywanie kompilacji podczas aktualizacji obrazu podstawowego kontenera, takich jak wprowadzanie poprawek systemu operacyjnego lub platformy aplikacji w jednym z obrazów podstawowych. Z tego samouczka dowiesz się, jak w usłudze ACR Tasks utworzyć zadanie, które wyzwala kompilację w chmurze po wypchnięciu obrazu podstawowego kontenera do rejestru.

Ten samouczek, będący ostatnią częścią serii, obejmuje:

> [!div class="checklist"]
> * Tworzenie obrazu podstawowego
> * Tworzenie zadania kompilacji obrazu aplikacji
> * Aktualizowanie obrazu podstawowego w celu wyzwolenia zadania obrazu aplikacji
> * Wyświetlanie wyzwolonego zadania
> * Weryfikowanie zaktualizowanego obrazu aplikacji

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli chcesz użyć interfejsu wiersza polecenia platformy Azure lokalnie, musisz zainstalować wersję **2.0.46** interfejsu wiersza polecenia platformy Azure lub nowszą. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja interfejsu wiersza polecenia lub jego uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][azure-cli].

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="complete-the-previous-tutorials"></a>Ukończenie poprzednich samouczków

W tym samouczku założono, że zostały już wykonane czynności opisane w dwóch pierwszych samouczkach z tej serii, takie jak:

* Tworzenie rejestru kontenerów platformy Azure
* Tworzenie rozwidlenia przykładowego repozytorium
* Klonowanie przykładowego repozytorium
* Tworzenie osobistego tokenu dostępu usługi GitHub

Jeśli dwa pierwsze samouczki nie zostały jeszcze ukończone, zapoznaj się z nimi przed kontynuowaniem:

[Tworzenie obrazów kontenera w chmurze przy użyciu usługi Azure Container Registry Tasks](container-registry-tutorial-quick-task.md)

[Automatyzowanie kompilacji obrazu kontenera za pomocą usługi Azure Container Registry Tasks](container-registry-tutorial-build-task.md)

### <a name="configure-the-environment"></a>Konfigurowanie środowiska

Wypełnij te zmienne środowiskowe powłoki przy użyciu wartości odpowiednich dla danego środowiska. Ten krok nie jest ściśle wymagany, ale trochę ułatwia wykonywanie przedstawionych w tym samouczku wielowierszowych poleceń interfejsu wiersza polecenia platformy Azure. Jeśli te zmienne środowiskowe nie zostaną wypełnione, należy ręcznie zastąpić każdą wartość we wszystkich miejscach występowania w przykładowych poleceniach.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Obrazy podstawowe

Pliki Dockerfile definiujące większość obrazów kontenera określają obraz nadrzędny, na którym się one opierają. Jest on często określany jako *obraz podstawowy*. Obrazy podstawowe przeważnie zawierają system operacyjny, na przykład [Alpine Linux][base-alpine] lub [Windows Nano Server][base-windows], w którym są stosowane pozostałe warstwy kontenera. Mogą również zawierać platformy aplikacji, takie jak [Node.js][base-node] lub [.NET Core][base-dotnet].

### <a name="base-image-updates"></a>Aktualizacje obrazu podstawowego

Obraz podstawowy jest często aktualizowany przez element utrzymujący obrazu w celu uwzględnienia nowych funkcji lub ulepszeń systemu operacyjnego lub platformy w obrazie. Poprawki zabezpieczeń są kolejną typową przyczyną aktualizacji obrazu podstawowego.

Po zaktualizowaniu obrazu podstawowego konieczne jest ponowne skompilowanie wszystkich obrazów kontenera w powiązanym rejestrze w celu uwzględnienia nowych funkcji i poprawek. Usługa ACR Tasks oferuje możliwość automatycznego kompilowania obrazów podczas aktualizacji obrazu podstawowego kontenera.

### <a name="base-image-update-scenario"></a>Scenariusz aktualizacji obrazu podstawowego

W tym samouczku przedstawiono scenariusz aktualizacji obrazu podstawowego. [Przykładowy kod][code-sample] obejmuje dwa pliki Dockerfile: obraz aplikacji i obraz określony w nim jako podstawowy. W poniższych sekcjach utworzysz zadanie usługi ACR Tasks, które automatycznie wyzwala kompilację obrazu aplikacji, gdy nowa wersja obrazu podstawowego zostanie wypchnięta do rejestru kontenerów.

[Dockerfile-app][dockerfile-app]: mała aplikacja internetowa Node.js, która renderuje statyczną stronę internetową wyświetlającą wersję środowiska Node.js, na której się opiera. Ciąg wersji jest symulowany: wyświetla zawartość zmiennej środowiskowej `NODE_VERSION`, którą zdefiniowano w obrazie podstawowym.

[Dockerfile-base][dockerfile-base]: obraz, który plik `Dockerfile-app` określa jako podstawowy. Opiera się ona na obrazie [Node][base-node] i uwzględnia zmienną środowiskową `NODE_VERSION`.

W poniższych sekcjach utworzysz zadanie, zaktualizujesz wartość `NODE_VERSION` w pliku Dockerfile obrazu podstawowego, a następnie użyjesz usługi ACR Tasks do skompilowania obrazu podstawowego. Gdy usługa ACR Tasks wypycha nowy obraz podstawowy do rejestru, następuje automatyczne wyzwolenie kompilacji obrazu aplikacji. Opcjonalnie możesz uruchomić obraz kontenera aplikacji lokalnie, aby zobaczyć inne ciągi wersji we wbudowanych obrazach.

## <a name="build-the-base-image"></a>Tworzenie obrazu podstawowego

Rozpocznij od skompilowania obrazu podstawowego przy użyciu funkcji *szybkich zadań* w usłudze ACR Tasks. Zgodnie z opisem w [pierwszym samouczku](container-registry-tutorial-quick-task.md) z serii, w tym procesie następuje nie tylko kompilacja obrazu, ale także wypchnięcie go do rejestru kontenerów, jeśli kompilacja zakończy się pomyślnie.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task"></a>Tworzenie zadania podrzędnego

Następnie utwórz zadanie przy użyciu polecenia [az acr task create][az-acr-task-create]:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> Jeśli wcześniej utworzono zadania za pomocą wersji zapoznawczej przy użyciu polecenia `az acr build-task`, trzeba je utworzyć ponownie przy użyciu polecenia [az acr task][az-acr-task].

To zadanie jest podobne do szybkiego zadania utworzonego w [poprzednim samouczku](container-registry-tutorial-build-task.md). Przesyła ono do usługi ACR Tasks instrukcję wyzwolenia kompilacji obrazu, gdy zatwierdzenia są wypychane do repozytorium określonego przez element `--context`.

Różnica w jego zachowaniu polega na tym, że wyzwala on również kompilację obrazu po zaktualizowaniu jego *obrazu podstawowego*. Plik Dockerfile określony przez argument `--file`, [Dockerfile-app][dockerfile-app], obsługuje określanie obrazu z tego samego rejestru jako podstawowego:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Po uruchomieniu zadania usługa ACR Tasks wykrywa zależności obrazu. Jeśli obraz podstawowy określony w instrukcji `FROM` znajduje się w tym samym rejestrze lub w publicznym repozytorium Docker Hub, dodaje ona punkt zaczepienia w celu zapewnienia, że ten obraz zostanie ponownie skompilowany zawsze po zaktualizowaniu jego bazy.

## <a name="build-the-application-container"></a>Kompilowanie kontenera aplikacji

Aby umożliwić usłudze ACR Tasks określanie i śledzenie zależności obrazu kontenera — które obejmują jego obraz podstawowy — **musisz** najpierw wyzwolić jego zadanie **co najmniej raz**.

Użyj polecenia [az acr task run][az-acr-task-run] w celu ręcznego uruchomienia zadania i skompilowania obrazu aplikacji:

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Po ukończeniu zadania zanotuj **identyfikator przebiegu** (na przykład „da6”), jeśli chcesz wykonać poniższy krok opcjonalny.

### <a name="optional-run-application-container-locally"></a>Opcjonalnie: lokalne uruchamianie aplikacji kontenera

Jeśli pracujesz lokalnie (nie w usłudze Cloud Shell) i masz zainstalowaną platformę Docker, uruchom kontener, aby zobaczyć renderowaną aplikację w przeglądarce internetowej przed ponownym skompilowaniem jej obrazu podstawowego. Jeśli używasz usługi Cloud Shell, pomiń tę sekcję (usługa Cloud Shell nie obsługuje poleceń `az acr login` ani `docker run`).

Najpierw zaloguj się do rejestru kontenerów przy użyciu polecenia [az acr login][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Teraz uruchom komputer lokalnie przy użyciu polecenia `docker run`. Zastąp element **\<run-id\>** identyfikatorem przebiegu znalezionym w danych wyjściowych poprzedniego kroku (na przykład „da6”).

```azurecli
docker run -d -p 8080:80 $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Przejdź do elementu http://localhost:8080 w przeglądarce. Powinien zostać wyświetlony numer wersji środowiska Node.js renderowany na stronie internetowej, podobny do następującego. W kolejnym kroku wersja zostanie zwiększona przez dodanie „a” do ciągu wersji.

![Zrzut ekranu przedstawiający przykładową aplikację renderowaną w przeglądarce][base-update-01]

## <a name="list-the-builds"></a>Tworzenie listy kompilacji

Następnie utwórz listę przebiegów zadań, które wykonała usługa ACR Tasks dla rejestru, używając polecenia [az acr task list-runs][az-acr-task-list-runs]:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Jeśli ukończono poprzedni samouczek (a rejestr nie został usunięty), powinny pojawić się dane wyjściowe podobne do poniższych. Zanotuj liczbę przebiegów zadań i najnowszą wartość RUN ID (identyfikator przebiegu), aby porównać dane wyjściowe po zaktualizowaniu obrazu podstawowego w następnej sekcji.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK            PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da6       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual      2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="update-the-base-image"></a>Aktualizowanie obrazu podstawowego

W tym miejscu przeprowadzisz symulację poprawki platformy w obrazie podstawowym. Edytuj plik **Dockerfile-base** i dodaj „a” po numerze wersji zdefiniowanym w elemencie `NODE_VERSION`:

```Dockerfile
ENV NODE_VERSION 9.11.2a
```

Uruchom szybkie zadanie w celu skompilowania zmodyfikowanego obrazu podstawowego. Zanotuj **identyfikator przebiegu** w danych wyjściowych.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Gdy kompilacja zostanie ukończona i usługa ACR Tasks wypchnie nowy obraz podstawowy do rejestru, następuje wyzwolenie kompilacji obrazu aplikacji. Może upłynąć kilka minut, zanim utworzone wcześniej zadanie wyzwoli kompilację obrazu aplikacji, ponieważ musi wykryć nowo utworzony i wypchnięty obraz podstawowy.

## <a name="list-updated-build"></a>Utworzenie listy zaktualizowanej kompilacji

Teraz, gdy obraz podstawowy został skompilowany, ponownie utwórz listę przebiegów zadań w celu porównania ze starszą listą. Jeśli na początku dane wyjściowe nie różnią się, okresowo uruchamiaj polecenie, aby zobaczyć nowy przebieg zadania na liście.

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Dane wyjściowe będą podobne do następujących. Element TRIGGER (wyzwalacz) dla ostatnio wykonanej kompilacji powinien mieć wartość „Aktualizacja obrazu”, wskazując, że zadanie zostało uruchomione przez funkcję szybkiego zadania obrazu podstawowego.

```console
$ az acr task list-builds --registry $ACR_NAME --output table

Run ID    TASK            PLATFORM    STATUS     TRIGGER       STARTED               DURATION
--------  --------------  ----------  ---------  ------------  --------------------  ----------
da8       taskhelloworld  Linux       Succeeded  Image Update  2018-09-17T23:11:50Z  00:00:33
da7                       Linux       Succeeded  Manual        2018-09-17T23:11:27Z  00:00:35
da6       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual        2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit    2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual        2018-09-17T22:29:59Z  00:00:57
```

Jeśli chcesz wykonać poniższy krok opcjonalny polegający na uruchomieniu nowo skompilowanego kontenera w celu sprawdzenia zaktualizowanego numeru wersji, zanotuj wartość **RUN ID** dla kompilacji wyzwolonej przez aktualizację obrazu (w powyższych danych wyjściowych jest to „da8”).

### <a name="optional-run-newly-built-image"></a>Opcjonalnie: uruchamianie nowo utworzonego obrazu

Jeśli pracujesz lokalnie (nie w usłudze Cloud Shell) i masz zainstalowaną platformę Docker, uruchom nowy obraz aplikacji po ukończeniu kompilacji. Zastąp element `<run-id>` identyfikatorem przebiegu RUN ID uzyskanym w poprzednim kroku. Jeśli używasz usługi Cloud Shell, pomiń tę sekcję (usługa Cloud Shell nie obsługuje polecenia `docker run`).

```bash
docker run -d -p 8081:80 $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Przejdź do elementu http://localhost:8081 w przeglądarce. Powinien zostać wyświetlony zaktualizowany numer wersji środowiska Node.js (z literą „a”) na stronie internetowej:

![Zrzut ekranu przedstawiający przykładową aplikację renderowaną w przeglądarce][base-update-02]

Pamiętaj, że obraz **podstawowy** został zaktualizowany za pomocą nowego numeru wersji, ale ostatnio skompilowany obraz **aplikacji** wyświetla nową wersję. Usługa ACR Tasks pobrała zmianę do obrazu podstawowego i automatycznie ponownie skompilowała obraz aplikacji.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Aby usunąć wszystkie zasoby, które zostały utworzone w tej serii samouczków, w tym rejestr kontenerów, wystąpienie kontenera, magazyn kluczy i jednostkę usługi, należy zastosować następujące polecenia:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób konfigurowania zadania w celu automatycznego wyzwalania kompilacji obrazu kontenera po zaktualizowaniu obrazu podstawowego powiązanego z obrazem. Teraz przejdź do szkolenia dotyczącego uwierzytelniania rejestru kontenerów.

> [!div class="nextstepaction"]
> [Uwierzytelnianie w usłudze Azure Container Registry](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-task-create]: /cli/azure/acr#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr#az-acr-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-task-list-runs]: /cli/azure/acr#az-acr-task-list-runs
[az-acr-task]: /cli/azure/acr#az-acr-task

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png