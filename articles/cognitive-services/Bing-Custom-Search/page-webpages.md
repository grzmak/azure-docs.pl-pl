---
title: Przejrzyj dostępne stron sieci Web — Bing Custom Search
titlesuffix: Azure Cognitive Services
description: Pokazuje, jak stronicować za pośrednictwem wszystkich stron sieci Web, która usługa Bing Custom Search może zwracać.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: f7d0a2f9c5a0e272d9f46ab76360ea7bf91449cc
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815963"
---
# <a name="paging-webpages"></a>Stronicowanie stron sieci Web 

Po wywołaniu interfejsu API wyszukiwania niestandardowego Bing zwraca listę wyników. Lista jest podzbiorem całkowita liczba wyników, które mogą dotyczyć zapytania. Aby uzyskać szacowana liczba dostępnych wyników, dostęp do obiektu odpowiedzi [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#totalestimatedmatches) pola.  
  
W poniższym przykładzie przedstawiono `totalEstimatedMatches` pola, które obejmuje odpowiedzi sieci Web.  
  
```  
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```  
  
Na stronie przy użyciu dostępnych stron sieci Web, należy użyć [liczba](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#count) i [przesunięcie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#offset) parametry zapytania.  
  
`count` Parametr określa liczbę wyników do zwrócenia w odpowiedzi. Maksymalna liczba wyników, których może żądać w odpowiedzi jest 50. Wartość domyślna wynosi 10. Rzeczywista liczba dostarczonych może być mniejsza niż żądana.

`offset` Parametr określa liczbę wyników do pominięcia. `offset` Jest liczony od zera i powinna być mniejsza niż (`totalEstimatedMatches` - `count`).  
  
Jeśli chcesz wyświetlić 15 stron sieci Web na każdej stronie, należy ustawić `count` do 15 i `offset` na 0, aby pobrać pierwszej strony wyników. Na kolejnych stronach, możesz zwiększyć `offset` 15 (na przykład, 15, 30).  
  
Poniżej przedstawiono przykład, który żąda 15 stron sieci Web, rozpoczynając od przesunięcia 45.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Jeśli wartość domyślna `count` wartość działa w przypadku implementacji, należy określić `offset` parametr zapytania.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  

