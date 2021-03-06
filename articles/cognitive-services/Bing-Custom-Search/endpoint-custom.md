---
title: Punkt końcowy wyszukiwania niestandardowego Bing
titlesuffix: Azure Cognitive Services
description: Podsumowanie punktu końcowego interfejsu API wyszukiwania niestandardowego Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: da448cdeaf6fcbe10cba8e5e2613214f8e0cee18
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815192"
---
# <a name="custom-search"></a>Wyszukiwanie niestandardowe
Usługa Bing Custom Search umożliwia utworzenie środowiska wyszukiwania dostosowane dla tematów, które Cię interesują. Wyniki wyszukiwania, dostosowane do zawartości są interesujące zamiast widoczne dla użytkowników na stronę za pośrednictwem wyników wyszukiwania, które mają zawartość nie ma znaczenia.

## <a name="custom-search-endpoint"></a>Punkt końcowy niestandardowego wyszukiwania
Aby uzyskać wyniki za pomocą interfejsu API wyszukiwania niestandardowego Bing, Wyślij `GET` żądanie następujący punkt końcowy. Umożliwia dalsze Definiowanie specyfikacji w nagłówki i parametry adresu URL.

Punkt końcowy: Zwraca sugestie dotyczące wyszukiwania jako wyniki JSON, które dotyczą dane wejściowe użytkownika zdefiniowane przez `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Przykłady, których opisano sposób konfigurowania źródeł wyszukiwania niestandardowego, zobacz [samouczek](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page). Aby uzyskać szczegółowe informacje o nagłówków, parametrów, kody na rynku, obiekty odpowiedzi, błędów itd., zobacz [interfejs API wyszukiwania niestandardowego Bing w wersji 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) odwołania.

## <a name="custom-search-response-json"></a>Wyszukiwanie niestandardowe odpowiedzi JSON
Żądanie niestandardowego wyszukiwania zwraca wyniki w postaci obiektów JSON, zobacz [obiekty odpowiedzi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Niestandardowego automatycznego sugerowania
Niestandardowy interfejs API automatycznego sugerowania pozwala wysyłać zapytania częściowego wyszukiwany termin do usługi Bing i wrócić Lista proponowanych zapytań, które można skonfigurować. Za pomocą niestandardowego automatycznego sugerowania należy dodać sugestie zwracane przez interfejs API i opcjonalnie Określ, czy zawierają sugestie generowane przez usługę Bing.

## <a name="custom-autosuggest-endpoint"></a>Niestandardowego automatycznego sugerowania punktu końcowego
Aby zażądać podpowiedzi dla zapytania niestandardowe, Wyślij żądanie Pobierz do:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Aby uzyskać informacji na temat definiowania niestandardowych sugestii, zobacz [zdefiniować sugestie wyszukiwania niestandardowego](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Niestandardowe wyszukiwanie obrazów
Niestandardowy interfejs API wyszukiwania obrazów pozwala wysyłać zapytania wyszukiwania usługi Bing i uzyskanie listy odpowiednie obrazy z wystąpienia wyszukiwania niestandardowego.

## <a name="custom-image-search-endpoint"></a>Punkt końcowy wyszukiwania niestandardowego obrazu
Aby zażądać obrazów z wystąpienia wyszukiwania niestandardowego, Wyślij żądanie Pobierz do następującego adresu URL:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Aby uzyskać informacje o konfigurowaniu wystąpienia wyszukiwania niestandardowego, zobacz [skonfigurować środowisko wyszukiwania niestandardowego](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view).

## <a name="next-steps"></a>Kolejne kroki
**Bing** interfejsy API obsługują akcji wyszukiwania, które zwracają wyniki według ich typu. Wszystkie punkty końcowe wyszukiwania zwracają wyniki w postaci obiektów odpowiedzi JSON.  Wszystkie punkty końcowe obsługują zapytań, które zwracają określonego języka i/lub lokalizacji długość geograficzna, szerokość i wyszukiwania usługi radius.

Aby uzyskać pełne informacje na temat parametrów obsługiwanych przez każdy punkt końcowy zobacz strony pomocy dla każdego typu.
Przykłady podstawowe żądań przy użyciu interfejsu API wyszukiwania niestandardowego, zobacz [przewodniki szybkiego startu dla wyszukiwania niestandardowego](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)