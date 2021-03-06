---
title: Terminologia dotycząca usługi Azure Service Fabric siatki | Dokumentacja firmy Microsoft
description: Poznaj typowe terminy dla usługi Azure Service Fabric siatki.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 7c3ce5571c54d6c613114ea49999e450934c8ff4
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136202"
---
# <a name="service-fabric-mesh-terminology"></a>Terminologia usługi Service Fabric siatki

Usługa Azure Service Fabric siatki jest w pełni zarządzana usługa, która pozwala deweloperom na wdrażanie aplikacji mikrousług bez zarządzania maszynami wirtualnymi, magazynu i sieci. Ten artykuł szczegółowo opisuje terminologię, używane przez usługi Azure Service Fabric siatki Zrozumienie pojęć używanych w dokumentacji.

## <a name="service-fabric"></a>Service Fabric

Service Fabric to platforma systemów rozproszonych typu "open source", która ułatwia pakowanie, wdrażanie i zarządzanie nimi skalowalnych i niezawodnych mikrousług. Usługa Service Fabric jest koordynatora, na której działają usługi Service Fabric siatki. Usługa Service Fabric udostępnia opcje, jak tworzyć i uruchamiać aplikacje mikrousług. Tworzenie usług i wybierz miejsce uruchomić aplikację z wielu opcji środowiska, można użyć dowolnej platformy.

## <a name="deployment-and-application-models"></a>Modele wdrażania i aplikacji 

Aby wdrożyć usługi, musisz opisują, jak powinno być ono uruchomione. Usługa Service Fabric obsługuje trzy różne modele wdrażania:

### <a name="resource-model"></a>Model zasobów
Zasoby są wszystkie elementy, które można wdrożyć osobno dla usługi Service Fabric, łącznie z aplikacjami, usługami, sieci i woluminy. Zasoby są definiowane przy użyciu pliku YAML lub plik JSON przy użyciu schematu modelu zasobów platformy Azure.

Model zasobu jest najprostszym sposobem opisują aplikacje usługi Service Fabric. Głównym celem jest na proste wdrażanie i zarządzanie usług konteneryzowanych.

Zasoby można wdrożyć wszędzie tam, gdzie działa Usługa Service Fabric.

### <a name="native-model"></a>Natywny model
Model aplikacji natywnej udostępnia swoje aplikacje za pomocą pełnej niskiego poziomu dostępu do usługi Service Fabric. Aplikacje i usługi są definiowane jako zarejestrowanych typów w plikach manifestu XML.

Natywny model obsługuje struktury usług Reliable Services, która zapewnia dostęp do środowiska uruchomieniowego usługi Service Fabric interfejsów API i Zarządzanie klastrem interfejsów API w języku C# i Java. Natywny model obsługuje również dowolnego kontenerów i plików wykonywalnych.

Natywny model nie jest obsługiwana w środowisku usługi Service Fabric siatki.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) jest częścią projektu platformy Docker. Usługa Service Fabric udostępnia ograniczoną obsługę wdrażania aplikacji przy użyciu narzędzia Docker Compose modelu.

## <a name="environments"></a>Środowiska

Usługa Service Fabric to technologia platformie typu open source, opartych na kilka różnych usług i produktów. Firma Microsoft udostępnia następujące opcje:

 - **Usługa Service Fabric siatki**: w pełni zarządzana Usługa do uruchamiania aplikacji usługi Service Fabric w systemie Microsoft Azure.
 - **Usługa Azure Service Fabric**: Oferta klastra usługi Service Fabric hostowane w systemie Azure. Umożliwia integrację usługi Service Fabric i infrastruktury platformy Azure, wraz z uaktualniania i zarządzania konfiguracją z klastrami usługi Service Fabric.
 - **Autonomicznego usługi Service Fabric**: zestaw funkcji wspomagających instalacji i konfiguracji do [wdrożyć klastry usługi Service Fabric w dowolnym miejscu](/azure/service-fabric/service-fabric-deploy-anywhere) (lokalnie lub na wszystkich dostawców chmury). Nie są zarządzane przez platformę Azure.
 - **Klaster projektowy usługi Service Fabric**: udostępnia środowisko programowania lokalnego na Windows, Linux lub Mac na potrzeby opracowywania aplikacji usługi Service Fabric.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Macierz obsługi środowiska, framework i modelu wdrażania
Różne środowiska mają różne poziomy wsparcia dla platformy i modele wdrażania. W poniższej tabeli opisano obsługiwane framework i kombinacji modelu wdrażania.

| Typ aplikacji | Opisany przez | Usługa Azure Service Fabric siatki | Klastry usługi platformy Azure (dowolny system operacyjny)| Klaster lokalny — Windows | Klaster lokalny — Linux | Klaster lokalny — Mac | Klaster autonomiczny (Windows)
|---|---|---|---|---|---|---|---|---|---|
| Aplikacje siatki usługi Service Fabric | Model zasobów (YAML & JSON) | Obsługiwane |Nieobsługiwane | Obsługiwane |Nieobsługiwane | Nieobsługiwane | Nieobsługiwane |
|Aplikacje natywne usługi Service Fabric | Model aplikacji natywnych (XML) | Nieobsługiwane| Obsługiwane|Obsługiwane|Obsługiwane|Obsługiwane|Obsługiwane|

W poniższej tabeli opisano modele innej aplikacji i narzędzi, który istnieje dla nich względem usługi Service Fabric.

| Typ aplikacji | Opisany przez | Visual Studio 2017 | Visual Studio 2015 | Eclipse | VS Code | INTERFEJS SFCTL | INTERFEJS WIERSZA POLECENIA AZ | PowerShell
|---|---|---|---|---|---|---|---|---|---|
| Aplikacje siatki usługi Service Fabric | Model zasobów (YAML & JSON) | Obsługiwane |Nieobsługiwane |Nieobsługiwane |Nieobsługiwane |Nieobsługiwane | Obsługiwane — tylko w środowisku siatki | Nieobsługiwane
|Aplikacje natywne usługi Service Fabric | Model aplikacji natywnych (XML) | Obsługiwane| Obsługiwane|Obsługiwane|Obsługiwane|Obsługiwane|Obsługiwane|Obsługiwane|

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat usługi Service Fabric siatki, zapoznaj się z omówieniem:
- [Omówienie usługi Service Fabric siatki](service-fabric-mesh-overview.md)
