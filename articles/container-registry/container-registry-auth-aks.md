---
title: Uwierzytelnianie za pomocą usługi Azure Container Registry z usługi Azure Kubernetes Service
description: Dowiedz się, jak zapewnić dostęp do obrazów w prywatnego rejestru kontenera z usługi Azure Kubernetes Service za pomocą jednostki usługi Azure Active Directory.
services: container-service
author: dlepow
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.openlocfilehash: d08fc0cb8e3203a60cbd426145ec50bb3636e758
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857129"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Uwierzytelnianie za pomocą usługi Azure Container Registry z usługi Azure Kubernetes Service

Podczas korzystania z usługi Azure Container Registry (ACR) przy użyciu usługi Azure Kubernetes Service (AKS), mechanizm uwierzytelniania musi zostać ustanowione. Ten artykuł szczegółowo opisuje zalecanym konfiguracjom dotyczącym uwierzytelniania między tymi dwoma usługami platformy Azure.

## <a name="grant-aks-access-to-acr"></a>AKS udzielanie dostępu do usługi ACR

Podczas tworzenia klastra usługi AKS Azure tworzy również usługę jednostki do obsługi klastra sprawność działania, za pomocą innych zasobów platformy Azure. Ta jednostka usługi generowanych automatycznie służy do uwierzytelniania za pomocą rejestru ACR. Aby to zrobić, należy utworzyć usługę Azure AD [przypisania roli](../role-based-access-control/overview.md#role-assignments) która przyznaje klastra jednostce usługi dostępu do rejestru kontenerów.

Użyj następującego skryptu, aby przyznać jednostce generowanych przez usługi AKS usługi dostępu do usługi Azure container registry. Modyfikowanie `AKS_*` i `ACR_*` zmiennych w danym środowisku przed uruchomieniem skryptu.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Dostęp za pomocą wpisie tajnym rozwiązania Kubernetes

W niektórych przypadkach nie można przypisać rolę wymagane do udostępnienie go do usługi ACR jednostki generowane automatycznie usługi AKS. Na przykład ze względu na model zabezpieczeń Twojej organizacji, możesz nie mieć wystarczających uprawnień w katalogu usługi Azure AD, aby przypisać rolę do jednostki usługi AKS, generowane. W takim przypadku można utworzyć nową jednostkę usługi, a następnie przyznać jej dostęp do rejestru kontenerów, za pomocą wpisu tajnego ściągania obrazów platformy Kubernetes.

Użyj następującego skryptu, aby utworzyć nową jednostkę usługi, (użyjesz jej poświadczenia wpisie tajnym rozwiązania Kubernetes obraz ściągnięcia). Modyfikowanie `ACR_NAME` zmiennej w środowisku przed uruchomieniem skryptu.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create a 'Reader' role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --role Reader --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principal client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

Można teraz przechowywać poświadczenia nazwy głównej usługi w usłudze Kubernetes [klucz tajny ściągania obrazów][image-pull-secret], która będzie stanowiła odwołanie klastra usługi AKS, podczas uruchamiania kontenerów.

Należy użyć następującego **kubectl** polecenie, aby utworzyć wpisu tajnego rozwiązania Kubernetes. Zastąp `<acr-login-server>` z w pełni kwalifikowanej nazwy usługi Azure container registry (jest w formacie "acrname.azurecr.io"). Zastąp `<service-principal-ID>` i `<service-principal-password>` przy użyciu wartości uzyskanych przez uruchomienie skryptu poprzedniego. Zastąp `<email-address>` z dowolnego adresu e-mail poprawnie sformułowany.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Wpisie tajnym rozwiązania Kubernetes można teraz używać we wdrożeniach pod, określając jej nazwę (w tym przypadku "acr-auth") w `imagePullSecrets` parametru:

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
