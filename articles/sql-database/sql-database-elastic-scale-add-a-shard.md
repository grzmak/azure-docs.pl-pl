---
title: Dodawanie fragmentu, za pomocą narzędzi elastycznych baz danych | Dokumentacja firmy Microsoft
description: Interfejsy API elastycznego skalowania umożliwiają dodawanie nowych fragmentów do fragmentu zestawu.
services: sql-database
ms.service: sql-database
subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: c70ef0c7dc2a5ac112034cefc7aa67a08a58b21a
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163056"
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Dodawanie fragmentu, za pomocą narzędzi elastycznych baz danych
## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Aby dodać nowy zakres lub klucz fragmentu
Aplikacje często konieczne dodanie nowych fragmentów do obsługi danych, która oczekuje się od nowych kluczy lub kluczy zakresy, mapy fragmentów, która już istnieje. Na przykład może być konieczne aprowizowanie nowych fragmentów dla nowej dzierżawy aplikacji podzielonej na fragmenty według Identyfikatora dzierżawy lub miesięczny podzielonej na fragmenty danych może wymagać nowego fragmentu aprowizowane przed rozpoczęciem każdego nowego miesiąca. 

Jeśli nowy zakres wartości klucza nie jest już częścią istniejącego mapowania, jest prosty do dodawania nowych fragmentów i Skojarz nowy klucz lub zakresu do tego fragmentu. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Przykład: dodanie fragmentu i jego zakres do istniejącego mapowania fragmentów
W tym przykładzie użyto TryGetShard ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.trygetshard), [.NET](https://msdn.microsoft.com/library/azure/dn823929.aspx)) CreateShard ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.createshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)), CreateRangeMapping ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.createrangemapping), [.NET](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\))) metody i tworzy instancję ShardLocation ([Java](/java/api/com.microsoft.azure.elasticdb.shard.base._shard_location), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)) klasy. W przykładzie poniżej bazę danych o nazwie **sample_shard_2** i wszystkie obiekty niezbędne schematu wewnątrz niej zostały utworzone na potrzeby przechowywania zakresu [300, 400).  

```csharp
// sm is a RangeShardMap object.
// Add a new shard to hold the range being added. 
Shard shard2 = null; 

if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
{ 
    shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
} 

// Create the mapping and associate it with the new shard 
sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 
```

Dla wersji platformy .NET umożliwia także środowiska PowerShell jako alternatywę do tworzenia nowego Menedżera mapowań fragmentów. Przykład jest dostępny [tutaj](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Aby dodać fragmentu dla pustej części istniejący zakres
W niektórych sytuacjach może masz już zamapowana zakres fragmentu i częściowo wypełnione danymi, ale teraz ma nadchodzące dane były kierowane w innym fragmencie. Na przykład możesz fragmentu przez zakres dni zostały już przydzielonym 50 dni do fragmentu, ale w dniu 24 mają przyszłych dane znajdą się w innym fragmencie. Elastyczna baza danych [narzędzie do dzielenia i scalania](sql-database-elastic-scale-overview-split-and-merge.md) mogą wykonać tę operację, ale jeśli przenoszenia danych nie jest konieczne (na przykład dane dotyczące zakresu dni [25, 50), oznacza to, w dniu 25, włącznie z 50 wzajemnie, jeszcze nie istnieje) można przeprowadzić to korzystając wyłącznie z interfejsów API zarządzania dla mapy fragmentów bezpośrednio.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Przykład: podział zakresu i przypisywanie pusty fragment do nowo dodanego fragmentów
Utworzono bazę danych o nazwie "sample_shard_2" i wszystkie obiekty niezbędne schematu wewnątrz niej.  

```csharp
// sm is a RangeShardMap object.
// Add a new shard to hold the range we will move 
Shard shard2 = null; 

if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
{ 
    shard2 = sm.CreateShard(new ShardLocation(shardServer,"sample_shard_2"));  
} 

// Split the Range holding Key 25 
sm.SplitMapping(sm.GetMappingForKey(25), 25); 

// Map new range holding [25-50) to different shard: 
// first take existing mapping offline 
sm.MarkMappingOffline(sm.GetMappingForKey(25)); 

// now map while offline to a different shard and take online 
RangeMappingUpdate upd = new RangeMappingUpdate(); 
upd.Shard = shard2; 
sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 
```

**Ważne**: Użyj tej techniki, tylko jeśli masz pewność, że zakres dla zaktualizowano mapowanie jest pusty.  Poprzednich metod nie sprawdzają dane dla zakresu, przeniesione, więc zaleca się obejmują sprawdzenia w kodzie.  Jeśli istnieją wiersze w zakresie przenoszone, dystrybucji rzeczywiste dane nie będą zgodne mapowania fragmentów zaktualizowane. Użyj [narzędzie do dzielenia i scalania](sql-database-elastic-scale-overview-split-and-merge.md) do wykonania tej operacji, zamiast tego w tych przypadkach.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

