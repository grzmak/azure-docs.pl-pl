---
title: Jednostki usługi dla usługi Azure Kubernetes Service (AKS)
description: Tworzenie jednostki usługi Azure Active Directory dla klastra w usłudze Azure Kubernetes Service (AKS) i zarządzanie nią
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: ef3139c4b3f06644b219e177fad0c094ed600fb6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394594"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Jednostki usługi w usłudze Azure Kubernetes Service (AKS)

Do współpracy z interfejsami API platformy Azure klaster usługi AKS wymaga [jednostki usługi Azure Active Directory][aad-service-principal]. Jednostka usługi jest potrzebna do dynamicznego tworzenia innych zasobów platformy Azure, takich jak usługa Azure Load Balancer lub usługa Azure Container Registry i zarządzania nimi.

W tym artykule przedstawiono sposób tworzenia jednostki usługi dla klastra usługi AKS i zarządzania nią.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Aby utworzyć jednostkę usługi Azure AD, musisz mieć uprawnienia do zarejestrowania aplikacji w swojej dzierżawie usługi Azure AD i przypisania aplikacji do roli w swojej subskrypcji. Jeśli nie masz niezbędnych uprawnień, może być konieczne zwrócenie się z prośbą do administratora usługi Azure AD lub subskrypcji o przyznanie niezbędnych uprawnień lub wstępne utworzenie jednostki usługi do użycia z klastrem usługi AKS.

Musisz również mieć zainstalowany i skonfigurowany interfejs wiersza polecenia platformy Azure w wersji 2.0.46 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Automatyczne tworzenie i używanie jednostki usługi

Podczas tworzenia klastra usługi AKS w witrynie Azure Portal lub przy użyciu polecenia [az aks create][az-aks-create] platforma Azure może automatycznie generować jednostkę usługi.

W poniższym przykładzie dotyczącym interfejsu wiersza polecenia platformy Azure nie została określona jednostka usługi. W tym scenariuszu interfejs wiersza polecenia platformy Azure tworzy jednostkę usługi dla klastra usługi AKS. Aby można było pomyślnie ukończyć tę operację, Twoje konto platformy Azure musi mieć odpowiednie uprawnienia do tworzenia jednostki usługi.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="manually-create-a-service-principal"></a>Ręczne tworzenie jednostki usługi

Aby ręcznie utworzyć jednostkę usługi przy użyciu interfejsu wiersza polecenia platformy Azure, użyj polecenia [az ad sp create-for-rbac][az-ad-sp-create]. W poniższym przykładzie parametr `--skip-assignment` zapobiega przypisaniu jakichkolwiek dodatkowych przypisań:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Dane wyjściowe będą podobne do poniższego przykładu. Zanotuj własne wartości `appId` i `password`. Te wartości będą używane podczas tworzenia klastra usługi AKS w następnej sekcji.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2018-09-25-21-10-19",
  "name": "http://azure-cli-2018-09-25-21-10-19",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Określanie jednostki usługi dla klastra usługi AKS

Aby użyć istniejącej jednostki usługi podczas tworzenia klastra usługi AKS za pomocą polecenia [az aks create][az-aks-create], użyj parametrów `--service-principal` i `--client-secret` w celu określenia właściwości `appId` i `password` z danych wyjściowych polecenia [az ad sp create-for-rbac][az-ad-sp-create]:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

W przypadku wdrażania klastra usługi AKS przy użyciu witryny Azure Portal na stronie *Uwierzytelnianie* okna dialogowego **Tworzenie klastra Kubernetes**, wybierz opcję **Konfigurowanie jednostki usługi**. Wybierz pozycję **Użyj istniejącej**, a następnie określ następujące wartości:

- **Identyfikator klienta jednostki usługi** to Twój *appId*
- **Klucz tajny klienta jednostki usługi** to wartość *hasła*

![Obraz przedstawiający przechodzenie do aplikacji Azure Vote](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="additional-considerations"></a>Dodatkowe zagadnienia

Podczas korzystania z jednostek usług AKS i Azure AD należy pamiętać o następujących kwestiach.

- Jednostka usługi dla rozwiązania Kubernetes jest częścią konfiguracji klastra. Nie należy jednak używać tożsamości do wdrażania klastra.
- Każda jednostka usługi jest skojarzona z aplikacją usługi Azure AD. Jednostka usługi dla klastra Kubernetes może zostać skojarzona z dowolną prawidłową nazwą aplikacji usługi Azure AD (na przykład *https://www.contoso.org/example*). Adres URL dla aplikacji nie musi być rzeczywistym punktem końcowym.
- Podczas określania **identyfikatora klienta** jednostki usługi użyj wartości `appId`.
- Na głównej maszynie wirtualnej i maszynach wirtualnych węzłów w klastrze Kubernetes poświadczenia jednostki usługi są przechowywane w pliku `/etc/kubernetes/azure.json`
- Gdy używasz polecenia [az aks create][az-aks-create], aby automatycznie wygenerować jednostkę usługi, poświadczenia jednostki usługi są zapisywane w pliku `~/.azure/aksServicePrincipal.json` na maszynie użytej do uruchomienia polecenia.
- Usunięcie klastra AKS utworzonego za pomocą polecenia [az aks create][az-aks-create] nie powoduje usunięcia utworzonej automatycznie jednostki usługi.
    - Aby usunąć jednostkę usługi, należy najpierw uzyskać jej identyfikator za pomocą polecenia [az ad app list][az-ad-app-list]. Poniższy przykład obejmuje wykonanie zapytania dotyczącego klastra o nazwie *myAKSCluster*, a następnie usunięcie aplikacji na podstawie identyfikatora za pomocą polecenia [az ad app delete][az-ad-app-delete]. Zastąp odpowiednie nazwy własnymi wartościami:

        ```azurecli
        az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
        az ad app delete --id <appId>
        ```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat nazw głównych usług Azure Active Directory, zobacz [Application and service principal objects (Obiekty aplikacji i jednostki usługi)][service-principal]

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
