---
title: Przegląd usługi HDInsight 4.0 (wersja zapoznawcza) — Azure
description: Porównanie funkcji i ograniczeń usług HDInsight 3.6 i HDInsight 4.0 oraz rekomendacje dotyczące uaktualniania.
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: overview
ms.date: 10/04/2018
ms.openlocfilehash: ade162d0261b765336cbff9ea8a6429f9bd2d871
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801837"
---
# <a name="hdinsight-40-overview-preview"></a>Przegląd usługi HDInsight 4.0 (wersja zapoznawcza)

Usługa Azure HDInsight jest jedną z najpopularniejszych usług używanych przez klientów korporacyjnych na potrzeby analizy typu open source w narzędziach Hadoop i Spark na platformie Azure. Usługa HDInsight (HDI) 4.0 jest dystrybucją w chmurze składników usługi Hadoop z platformy [Hortonworks Data Platform (HDP) 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html). Ten artykuł zawiera informacje o najnowszym wydaniu usługi Azure HDInsight i sposobie jej uaktualniania.

## <a name="whats-new-in-hdi-40"></a>Co nowego w usłudze HDI 4.0?

### <a name="hive-30-and-llap"></a>Hive 3.0 i LLAP

Infrastruktura Hive z przetwarzaniem analitycznym o małym opóźnieniu (LLAP) używa serwerów trwałych zapytań i buforowania w pamięci w celu szybkiego dostarczania wyników zapytań SQL względem danych w zdalnym magazynie w chmurze. Hive LLAP wykorzystuje zestaw trwałych demonów, które wykonują fragmenty zapytań Hive. Wykonywanie zapytań z przetwarzaniem LLAP odbywa się podobnie jak w infrastrukturze Hive bez funkcji LLAP z tą różnicą, że zadania procesów roboczych działają wewnątrz demonów LLAP zamiast w kontenerach.

Przetwarzanie Hive LLAP daje następujące korzyści:

* Możliwość wykonywania szczegółowych analiz SQL z wykorzystaniem elementów, takich jak złożone sprzężenia, podzapytania, funkcji obsługi okien, sortowanie, funkcje zdefiniowane przez użytkownika i złożone agregacje, bez obniżania oczekiwanego poziomu wydajności i skalowalności.

* Możliwość wykonywania interaktywnych zapytań względem danych w tym samym magazynie, w którym dane są przygotowywane, co eliminuje konieczność przenoszenia danych z magazynu do innego aparatu na potrzeby przetwarzania analitycznego.

* Buforowanie wyników zapytań umożliwia ponowne wykorzystanie wcześniej obliczanych wyników zapytań, co pozwala zaoszczędzić czas i zasoby używane podczas uruchamiania zadań klastra wymaganych dla zapytania.

### <a name="hive-dynamic-materialized-views"></a>Dynamiczne zmaterializowane widoki Hive

Infrastruktura Hive obsługuje obecnie dynamiczne zmaterializowane widoki lub wstępne obliczanie istotnych podsumowań, które są używane do przyspieszania przetwarzania zapytań w hurtowni danych. Zmaterializowane widoki mogą być przechowywane w sposób natywny w infrastrukturze Hive i mogą bezproblemowo korzystać z przyspieszenia LLAP.

### <a name="hive-transactional-tables"></a>Tabele transakcyjne Hive

Usługa HDI 4.0 zawiera infrastrukturę Apache Hive 3, która wymaga zgodności pod względem niepodzielności, spójności, izolacji i trwałości (ACID) dla tabel transakcyjnych rezydujących w magazynie Hive. Tabele zgodne ze standardem ACID oraz przechowywane w nich dane są dostępne i zarządzane za pośrednictwem programu Hive. Dane przechowywane w tabelach CRUD, które zapewniają możliwość tworzenia, pobierania, aktualizacji i usuwania, muszą być w formacie pliku ORC (Optimized Row Column), ale tabele umożliwiające tylko wstawianie danych obsługują wszystkie formaty plików.

* Standard ACID w wersji 2 wprowadza ulepszenia wydajności zarówno w formacie magazynu, jak i aparatu wykonywania. 

* Standard ACID jest domyślnie włączony, co umożliwia pełną obsługę aktualizacji danych.

* Ulepszone funkcje ACID pozwalają na aktualizowanie i usuwanie na poziomie wiersza.

* Nie występuje negatywny wpływ na wydajność.

* Nie jest wymagana obsługa zasobników.

* Platforma Spark może odczytywać i zapisywać dane w tabelach Hive ACID za pośrednictwem łącznika magazynu Hive.

Dowiedz się więcej o infrastrukturze [Apache Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/hive-overview/content/hive_whats_new_in_this_release_hive.html).

### <a name="apache-spark"></a>Apache Spark

Platforma Apache Spark pobiera możliwe do zaktualizowania tabele i transakcje ACID za pomocą łącznika magazynu Hive. Łącznik magazynu Hive umożliwia rejestrowanie tabel transakcyjnych Hive jako tabel zewnętrznych na platformie Spark w celu uzyskania dostępu do wszystkich funkcji transakcyjnych. Poprzednie wersje obsługiwały tylko manipulowanie partycjami tabel. Łącznik magazynu Hive obsługuje również przesyłanie strumieniowe elementów DataFrame na potrzeby strumieniowego odczytu i zapisu w tabelach transakcyjnych oraz przesyłanie strumieniowe tabel Hive z platformy Spark.

Funkcje wykonawcze platformy Spark mogą łączyć się bezpośrednio z demonami Hive LLAP na potrzeby pobierania i aktualizowania danych w sposób transakcyjny, co umożliwia zachowanie kontroli nad danymi przez infrastrukturę Hive.

Platforma Apache Spark w usłudze HDInsight 4.0 obsługuje następujące scenariusze:

* Uruchamianie szkolenia modelu uczenia maszynowego z wykorzystaniem tej samej tabeli transakcyjnej, która jest używana na potrzeby raportowania.
* Wykorzystanie transakcji ACID w celu bezpiecznego dodania kolumn z modelu Spark ML do tabeli Hive.
* Uruchamianie zadania przesyłania strumieniowego platformy Spark podczas zestawiania zmian z tabeli przesyłania strumieniowego Hive.
* Tworzenie plików ORC bezpośrednio na podstawie zadania przesyłania strumieniowego ze strukturą platformy Spark.

Nie musisz się już martwić przypadkowymi próbami uzyskania dostępu do tabel transakcyjnych Hive bezpośrednio z platformy Spark, co prowadziło do niespójności wyników, duplikowania danych lub ich uszkodzenia. W usłudze HDI 4.0 tabele platformy Spark i tabel programu Hive są przechowywane w oddzielnych magazynach metadanych. Za pomocą łącznika magazynu danych Hive możesz jawnie zarejestrować tabele transakcyjne programu Hive jako tabele zewnętrzne platformy Spark.

Dowiedz się więcej o platformie [Apache Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html).


### <a name="oozie"></a>Oozie

Usługa HDI 4.0 zawiera system Apache Oozie 4.3.1 z następującymi zmianami:

* W systemie Oozie nie można już uruchamiać akcji programu Hive. Usunięto interfejs Hive CLI i zastąpiono go usługą BeeLine.

* Niepożądane zależności można wykluczyć z biblioteki udziałów, umieszczając wzorzec wykluczania w pliku **job.properties**.

Dowiedz się więcej o systemie [Apache Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/patch_oozie.html).

## <a name="how-to-upgrade-to-hdi-40"></a>Jak przeprowadzić uaktualnienie do usługi HDI 4.0

Podobnie jak w przypadku każdego głównego wydania, przed zaimplementowaniem najnowszej wersji w środowisku produkcyjnym należy dokładnie przetestować składniki. Usługa HDI 4.0 jest dostępna w celu rozpoczęcia procesu uaktualniania, ale aby uniknąć przypadkowych nieprawidłowości, domyślną opcją pozostaje usługa HDI w wersji 3.6.

Nie ma żadnych obsługiwanych ścieżek uaktualniania z poprzednich wersji usługi HDI do usługi HDI 4.0. Z powodu zmiany formatów magazynu metadanych i formatu danych obiektów blob usługa HDI 4.0 nie jest zgodna z poprzednimi wersjami. Ważne jest, aby oddzielić nowe środowisko usługi HDI 4.0 od bieżącego środowiska produkcyjnego. Jeśli wdrożysz usługę HDI 4.0 w swoim bieżącym środowisku, magazyn metadanych zostanie uaktualniony i nie będzie można cofnąć tej zmiany.  

## <a name="limitations"></a>Ograniczenia

* Usługa HDI 4.0 nie obsługuje środowiska MapReduce. Zamiast tego należy skorzystać ze środowiska Tez. Dowiedz się więcej o środowisku [Apache Tez](https://tez.apache.org/).

* Widok Hive nie jest już dostępny w usłudze HDI 4.0. 

* Interpreter powłoki w rozwiązaniu Apache Zeppelin nie jest obsługiwany w klastrach Spark i klastrach zapytań interakcyjnych.

* Nie można *wyłączyć* funkcji LLAP w klastrze Spark LLAP. Można tylko wyłączyć funkcję LLAP.

* Usługa Azure Data Lake Storage Gen2 nie pozwala zapisywać notesów Juypter w klastrze Spark.

## <a name="next-steps"></a>Następne kroki

* [Dokumentacja usługi Azure HDInsight](index.yml)
* [Informacje o wersji](hdinsight-release-notes.md)
