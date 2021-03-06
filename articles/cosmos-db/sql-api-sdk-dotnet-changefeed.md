---
title: 'Azure Cosmos DB: Interfejs API programu .NET zmiany źródła danych procesora, zestawu SDK i zasobów | Dokumentacja firmy Microsoft'
description: Dowiedz się wszystkiego o interfejsu API procesora kanału informacyjnego zmian i zestawu SDK, w tym daty wydania, daty wycofania i zmiany między poszczególnymi wersjami zmiany źródła danych procesora zestawu SDK programu .NET.
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/21/2018
ms.author: maquaran
ms.openlocfilehash: 553917a29b3564fff71d6ab994ec199891cbaae7
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409105"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>Pobierz procesora źródło zmian .NET SDK: I informacje o wersji
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Kanał informacyjny zmian .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java (asynchroniczny)](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Dostawca zasobów REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [Bulkexecutor — platforma .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulkexecutor — platforma Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**Zestaw SDK do pobrania**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**Dokumentacja interfejsu API**|[Zmień dokumentację referencyjną interfejsu API bibliotece procesora zestawienia](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Wprowadzenie**|[Wprowadzenie zmian źródła danych procesora zestawu .NET SDK](change-feed.md)|
|**Bieżącej struktury obsługiwanej**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Informacje o wersji

### <a name="v2-builds"></a>kompilacje w wersji 2

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Dodano obsługę kolekcje partycjonowane dzierżawy. Klucz partycji musi być zdefiniowany jako/identyfikator.
* Pomocnicza, zmiana powodująca niezgodność: metody interfejsu IChangeFeedDocumentClient i klasa ChangeFeedDocumentClient zostały zmienione na obejmują parametry RequestOptions i token anulowania. IChangeFeedDocumentClient jest punktem zaawansowanych rozszerzeń, który pozwala na dostarczenie niestandardową implementację klientem dokumentu do korzystania z procesora zestawienia zmian, np. dekoracji DocumentClient i przechwytywać wszystkie wywołania do niego wykonaj dodatkowe śledzenia obsługi błędów , itp. Dzięki tej aktualizacji kodu, który implementuje IChangeFeedDocumentClient należy zostać zmienione w celu uwzględnienia nowych parametrów w implementacji.
* Diagnostyka drobne ulepszenia.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Dodano nowy interfejs API, zadanie&lt;IReadOnlyList&lt;RemainingPartitionWork&gt; &gt; IRemainingWorkEstimator.GetEstimatedRemainingWorkPerPartitionAsync(). Może to służyć do pracy szacowany dla każdej partycji.
* Obsługuje Microsoft.Azure.DocumentDB zestawu SDK w wersji 2.0. Wymaga Microsoft.Azure.DocumentDB 2.0 lub nowszej.

### <a name="a-name206206"></a><a name="2.0.6"/>2.0.6
* Dodano ChangeFeedEventHost.HostName właściwość publiczna dla compativility z v1.

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Naprawiono wyścigu, która występuje podczas dzielenia partycji. Sytuacja wyścigu może prowadzić do Uzyskiwanie dzierżawy i natychmiast utraty go podczas dzielenia partycji i powoduje rywalizacji o zasoby. Wyścig warunek naprawienia w tej wersji.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4
* ZESTAW SDK W WERSJI OGÓLNIE DOSTĘPNEJ

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3-Prerelease
* Rozwiązano następujące problemy:
  * Sytuacji podziału partycji może istnieć zduplikowane przetwarzania dokumentów przed podziału.
  * Interfejs API GetEstimatedRemainingWork zwrócił wartość 0, gdy dzierżawy nie znajdowały się w kolekcji dzierżawy.

* Następujące wyjątki są ujawniane. Rozszerzenia, które implementują IPartitionProcessor może zgłosić tych wyjątków.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-Prerelease
* Drobne zmiany interfejsu API:
  * Usunięte ChangeFeedProcessorOptions.IsAutoCheckpointEnabled, który został oznaczony jako przestarzały.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-Prerelease
* Ulepszenia w zakresie stabilności:
  * Lepszej obsługi inicjowania magazynu dzierżawy. Gdy magazyn dzierżawy jest pusta, tylko jedno wystąpienie procesora można go zainicjować, pozostałe będzie czekać.
  * Odnawianie dzierżawy stabilny/wydajne więcej/wydania. Odnowienia i zwalniania dzierżawy z jednej partycji jest niezależna od innych odnawiania. W wersji 1, która została wykonana po kolei wszystkich partycji.
* Nowe interfejsy API w wersji 2:
  * Wzorzec Konstruktor do tworzenia elastycznych procesora: klasa ChangeFeedProcessorBuilder.
    * Może być dowolną kombinacją parametrów.
    * Może to potrwać wystąpienia DocumentClient dla kolekcji monitorowania i/lub dzierżawy (opcja nie jest dostępna w wersji 1).
  * IChangeFeedObserver.ProcessChangesAsync przyjmuje teraz token anulowania.
  * IRemainingWorkEstimator — narzędzie do szacowania pracy pozostałej może służyć oddzielnie z procesora.
  * Nowe punkty rozszerzeń:
    * IParitionLoadBalancingStrategy — dla niestandardowych równoważenia obciążenia partycji między wystąpieniami procesora.
    * ILease, ILeaseManager — do zarządzania niestandardowej dzierżawy.
    * IPartitionProcessor — niestandardowe przetwarzanie zmian na partycji.
* Rejestrowanie - używa [LibLog](https://github.com/damianh/LibLog) biblioteki.
* 100% zgodne z poprzednimi wersjami z interfejsem API w wersji 1.
* Nowy kod podstawowy.
* Zgodne z [zestawu .NET SDK SQL](sql-api-sdk-dotnet.md) wersji 1.21.1 lub nowszej.

### <a name="v1-builds"></a>kompilacje w wersji 1

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* Dodano rejestrowanie większej ilości.
* Naprawiono przeciek DocumentClient podczas wywoływania szacowania pracy oczekujące wiele razy.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Poprawki w szacowania pracy oczekujące.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Ulepszenia w zakresie stabilności.
  * Poprawki dotyczące obsługi problem anulowane zadania, które mogą prowadzić do zatrzymania obserwatorów na partycji.
* Pomoc techniczna dla ręcznego tworzenia punktów kontrolnych.
* Zgodne z [zestawu .NET SDK SQL](sql-api-sdk-dotnet.md) wersji 1.21 i nowsze wersje.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Dodano obsługę programu .NET Standard 2.0. Obsługuje teraz pakiet `netstandard2.0` i `net451` monikerów framework.
* Zgodne z [zestawu .NET SDK SQL](sql-api-sdk-dotnet.md) wersji 1.17.0 lub nowszej.
* Zgodne z [SQL platformy .NET Core SDK](sql-api-sdk-dotnet-core.md) wersji 1.5.1 lub nowszej.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Rozwiązuje problem z wyliczeniem oszacowanie pracy pozostałej, gdy kanału informacyjnego zmian jest pusta lub oczekiwała żadnej pracy.
* Zgodne z [zestawu .NET SDK SQL](sql-api-sdk-dotnet.md) wersji 1.13.2 lub nowszej.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Dodaje metodę, aby uzyskać szacunkową ilość pozostałej pracy, które mają być przetwarzane w kanału informacyjnego zmian.
* Zgodne z [zestawu .NET SDK SQL](sql-api-sdk-dotnet.md) wersji 1.13.2 lub nowszej.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* ZESTAW SDK W WERSJI OGÓLNIE DOSTĘPNEJ
* Zgodne z [zestawu .NET SDK SQL](sql-api-sdk-dotnet.md) wersji 1.14.1 i poniżej.


## <a name="release--retirement-dates"></a>Daty wydania i wycofania
Firma Microsoft zapewnia powiadomienie co najmniej **12 miesięcy** ewentualnej wycofanie zestawu SDK w celu złagodzenia przejścia do nowszych/obsługiwanych wersji.

Nowe funkcje i funkcjonalność i optymalizacje są dodawane tylko do bieżącego zestawu SDK, w związku z tym zalecane jest, zawsze uaktualnienie do najnowszej wersji zestawu SDK tak szybko, jak to możliwe. 

Wszelkie żądania do usługi Cosmos DB przy użyciu wycofane zestawu SDK zostanie odrzucone przez usługę.

<br/>

| Wersja | Data wydania | Data wygaśnięcia |
| --- | --- | --- |
| [1.3.3](#1.3.3) |08 maja 2018 r. |--- |
| [1.3.2](#1.3.2) |18 kwietnia 2018 r. |--- |
| [1.3.1](#1.3.1) |13 marca 2018 r. |--- |
| [1.2.0](#1.2.0) |31 października 2017 r. |--- |
| [1.1.1](#1.1.1) |29 sierpnia 2017 r. |--- |
| [1.1.0](#1.1.0) |13 sierpnia 2017 r. |--- |
| [1.0.0](#1.0.0) |07 lipca 2017 r. |--- |


## <a name="faq"></a>Często zadawane pytania
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zobacz także
Aby dowiedzieć się więcej na temat usługi Cosmos DB, zobacz [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stronę usługi. 

