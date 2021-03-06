---
title: Samouczek języka C# dotyczący synonimów w usłudze Azure Search | Microsoft Docs
description: W ramach tego samouczka dodasz funkcję synonimów do indeksu w usłudze Azure Search.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 8340c4dc2a855911073905a3aea93e19fc7b520d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990565"
---
# <a name="tutorial-add-synonyms-for-azure-search-in-c"></a>Samouczek: dodawanie synonimów na potrzeby usługi Azure Search w języku C#

Synonimy rozszerzają zapytanie, dopasowując wyrażenia uznane za semantycznie równoważne z wyrażeniem wejściowym. Przykładowo można sprawić, aby wyraz „samochód” pasował do dokumentów zawierających wyrażenia „auto” lub „pojazd”. 

W usłudze Azure Search synonimy są definiowane w *mapie synonimów* za pośrednictwem *reguł mapowania*, które kojarzą równoważne wyrażenia. Ten samouczek obejmuje niezbędne kroki, które trzeba wykonać, aby dodać synonimy i korzystać z nich za pomocą istniejącego indeksu. Omawiane kwestie:

> [!div class="checklist"]
> * Włączanie synonimów przez tworzenie i publikowanie reguł mapowania 
> * Przywoływanie mapy synonimów w ciągu zapytania

Możesz utworzyć wiele map synonimów, opublikować je jako zasób obejmujący usługę dostępny dla dowolnych indeksów, a następnie określić, której z nich należy używać na poziomie pola. W czasie realizacji zapytania, poza wyszukiwaniem indeksu, usługa Azure Search wyszukuje mapę synonimów, jeśli mapa została określona dla pól używanych w zapytaniu.

> [!NOTE]
> Synonimy są obsługiwane w najnowszej wersji interfejsu API i zestawu SDK (wersja interfejsu API: 2017-11-11, wersja zestawu SDK: 5.0.0). Obecnie witryna Azure Portal nie jest obsługiwana. Jeśli obsługa funkcji synonimów w witrynie Azure Portal byłaby dla Ciebie przydatna, przekaż swoją opinię na platformie [UserVoice](https://feedback.azure.com/forums/263029-azure-search)

## <a name="prerequisites"></a>Wymagania wstępne

Wymagania samouczka obejmują poniższe elementy:

* [Program Visual Studio](https://www.visualstudio.com/downloads/)
* [Usługa Azure Search](search-create-service-portal.md)
* [Biblioteka Microsoft.Azure.Search .NET](https://aka.ms/search-sdk)
* [Jak używać usługi Azure Search z poziomu aplikacji .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Omówienie

Zapytania „przed” i „po” demonstrują wartość synonimów. W tym samouczku użyjesz przykładowej aplikacji, która wykonuje zapytania względem przykładowego indeksu i zwraca wyniki. Przykładowa aplikacja tworzy mały indeks o nazwie „hotels” uzupełniony dwoma dokumentami. Aplikacja wykonuje zapytania wyszukiwania przy użyciu wyrażeń i fraz, które nie pojawiają się w indeksie, włącza funkcję synonimów, a następnie ponownie wykonuje te same wyszukiwania. Poniższy kod demonstruje ogólny przepływ.

```csharp
  static void Main(string[] args)
  {
      SearchServiceClient serviceClient = CreateSearchServiceClient();

      Console.WriteLine("{0}", "Cleaning up resources...\n");
      CleanupResources(serviceClient);

      Console.WriteLine("{0}", "Creating index...\n");
      CreateHotelsIndex(serviceClient);

      ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

      Console.WriteLine("{0}", "Uploading documents...\n");
      UploadDocuments(indexClient);

      ISearchIndexClient indexClientForQueries = CreateSearchIndexClient();

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Adding synonyms...\n");
      UploadSynonyms(serviceClient);
      EnableSynonymsInHotelsIndex(serviceClient);
      Thread.Sleep(10000); // Wait for the changes to propagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");

      Console.ReadKey();
  }
```
Kroki związane z tworzeniem i uzupełnianiem przykładowego indeksu zostały wyjaśnione w temacie [Jak używać usługi Azure Search z poziomu aplikacji .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

## <a name="before-queries"></a>Zapytania „przed”

Względem indeksu `RunQueriesWithNonExistentTermsInIndex` wykonaj zapytania wyszukiwania z użyciem wyrażeń „five star”, „internet” oraz „economy AND hotel”.
```csharp
Console.WriteLine("Search the entire index for the phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
Żaden ze zindeksowanych dokumentów nie zawiera tych wyrażeń, więc z pierwszego indeksu `RunQueriesWithNonExistentTermsInIndex` uzyskujemy następujące dane wyjściowe.
~~~
Search the entire index for the phrase "five star":

no document matched

Search the entire index for the term 'internet':

no document matched

Search the entire index for the terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a>Włączanie synonimów

Włączenie synonimów to dwuetapowy proces. Najpierw definiujemy i przekazujemy reguły synonimów, a następnie konfigurujemy pola, aby ich używały. Proces jest opisany w `UploadSynonyms` i `EnableSynonymsInHotelsIndex`.

1. Dodaj mapę synonimów do usługi wyszukiwania. W `UploadSynonyms` definiujemy cztery reguły w mapie synonimów „desc-synonymmap” i przekazujemy je do usługi.
```csharp
    var synonymMap = new SynonymMap()
    {
        Name = "desc-synonymmap",
        Format = "solr",
        Synonyms = "hotel, motel\n
                    internet,wifi\n
                    five star=>luxury\n
                    economy,inexpensive=>budget"
    };

    serviceClient.SynonymMaps.CreateOrUpdate(synonymMap);
```
Mapa synonimów musi być zgodna z formatem `solr` w standardzie open source. Format został wyjaśniony w artykule [Synonimy w usłudze Azure Search](search-synonyms.md) w sekcji `Apache Solr synonym format`.

2. Skonfiguruj pola z możliwością wyszukiwania, aby używać mapy synonimów w definicji indeksu. W `EnableSynonymsInHotelsIndex` włączamy synonimy w dwóch polach, `category` i `tags`, ustawiając właściwość `synonymMaps` na nazwę nowo przekazanej mapy synonimów.
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
Podczas dodawania mapy synonimów ponowne kompilowanie indeksu nie jest wymagane. Możesz dodać mapę synonimów do usługi, a następnie zmienić istniejące definicje pól w dowolnym indeksie, aby użyć nowej mapy synonimów. Dodawanie nowych atrybutów nie ma wpływu na dostępność indeksu. To samo dotyczy wyłączania synonimów dla pola. Możesz po prostu ustawić właściwość `synonymMaps` na pustą listę.
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a>Zapytania „po”

Po przekazaniu mapy synonimów i zaktualizowaniu indeksu do używania mapy synonimów drugie wywołanie `RunQueriesWithNonExistentTermsInIndex` zwraca następujące wyniki:

~~~
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
Pierwsze zapytanie znajduje dokument na podstawie reguły `five star=>luxury`. Drugie zapytanie rozszerza wyszukiwanie przy użyciu `internet,wifi`, a trzecie przy użyciu `hotel, motel` i `economy,inexpensive=>budget` w zakresie wyszukiwania dopasowanych dokumentów.

Dodanie synonimów całkowicie zmienia funkcję wyszukiwania. W tym samouczku oryginalne zapytania nie zwróciły znaczących wyników, chociaż dokumenty w naszym indeksie były odpowiednie. Włączając synonimy, możemy rozszerzyć indeks, aby objął popularne wyrażenia, nie zmieniając danych źródłowych w indeksie.

## <a name="sample-application-source-code"></a>Kod źródłowy przykładowej aplikacji
Możesz znaleźć pełny kod źródłowy przykładowej aplikacji używanej w tym poradniku w portalu [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Najszybszym sposobem wyczyszczenia środowiska po ukończeniu samouczka jest usunięcie grupy zasobów zawierającej usługę Azure Search. Możesz teraz usunąć tę grupę zasobów, aby trwale usunąć całą jej zawartość. W portalu nazwa grupy zasobów znajduje się na stronie Przegląd usługi Azure Search.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono użycie [interfejsu API REST synonimów](https://aka.ms/rgm6rq) w kodzie C# do utworzenia i opublikowania reguł mapowania, a następnie wywołania mapy synonimów w zapytaniu. Dodatkowe informacje można znaleźć w dokumentacji referencyjnej [zestawu .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) i [interfejsu API REST](https://docs.microsoft.com/rest/api/searchservice/).

> [!div class="nextstepaction"]
> [Jak używać synonimów w usłudze Azure Search](search-synonyms.md)
