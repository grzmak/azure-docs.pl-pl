---
title: Monitorowanie wydajności klastra — Azure HDInsight
description: Sposób monitorowania klastra usługi HDInsight dla pojemności i wydajności.
services: hdinsight
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/27/2017
ms.author: maxluk
ms.openlocfilehash: 434b3ecf65aaa5ecea81f5a9773f1bc6e8f6f2be
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43092331"
---
# <a name="monitor-cluster-performance"></a>Monitorowanie wydajności klastra

Monitorowanie kondycji i wydajności klastra usługi HDInsight ma zasadnicze znaczenie dla zachowanie maksymalnej wydajności i wykorzystania zasobów. Monitorowanie może również pomóc adres kodu lub błędy konfiguracji klastra.

Poniższe sekcje opisują, jak zoptymalizować klastra podczas ładowania, YARN kolejki wydajności i dostępności magazynu.

## <a name="cluster-loading"></a>Ładowanie klastra

Klastry Hadoop powinien równoważyć ładowania w węzłach klastra. Ta równoważenia zapobiega zadań przetwarzania jest ograniczony przez ilość pamięci RAM, procesora CPU lub zasobów dysku.

Aby wysokiego poziomu poznać węzłów klastra i ich ładowanie, zaloguj się do [Interfejsu sieci Web Ambari](hdinsight-hadoop-manage-ambari.md), a następnie wybierz **hosty** kartę. Hosty są wyświetlane według ich w pełni kwalifikowanych nazw domen. Stan każdego hosta jest wyświetlany za pomocą wskaźnika kolorowe kondycji:

| Kolor | Opis |
| --- | --- |
| Czerwony | Co najmniej jeden składnik główny hosta nie działa. Zatrzymaj wskaźnik myszy, aby wyświetlić etykietkę narzędzia list składniki dotknięte problemem. |
| Orange | Co najmniej jeden element podrzędny składnika na hoście nie działa. Zatrzymaj wskaźnik myszy, aby wyświetlić etykietkę narzędzia list składniki dotknięte problemem. |
| Żółty | Ambari serwer nie otrzymał pulsu z hosta do więcej niż trzy minuty. |
| Zielony | Normalny stan pracy. |

Widoczna jest także kolumny, przedstawiający liczbę rdzeni i ilość pamięci RAM dla każdego hosta, a średnie wykorzystanie dysku i obciążenia.

![Karta hosty](./media/hdinsight-key-scenarios-to-monitor/hosts-tab.png)

Wybierz dowolny z nazw hostów, aby uzyskać szczegółowy widok składników działających na tym hoście i ich metryki. Metryki są wyświetlane jako możliwy oś czasu procesora CPU użycia, obciążenia, użycie dysku, użycie pamięci, użycie sieci i liczby procesów.

![Szczegóły dotyczące hostów](./media/hdinsight-key-scenarios-to-monitor/host-details.png)

Zobacz [HDInsight Zarządzanie klastrami przy użyciu interfejsu użytkownika sieci Web Ambari](hdinsight-hadoop-manage-ambari.md) Aby uzyskać szczegółowe informacje na temat alertów i wyświetlanie metryk.

## <a name="yarn-queue-configuration"></a>Konfiguracja kolejki usługi YARN

Usługi Hadoop ma różne usługi działające na różnych jego rozproszoną platformę. YARN (jeszcze inny zasób moduł negocjowania) służy do koordynowania tych usług, przydzielania zasobów klastra i zarządza dostępem do wspólnego zestawu danych.

YARN dwóch obowiązki JobTracker, zarządzanie zasobami oraz planowanie/monitorowania zadań, jest podzielony na dwie demonów: globalnego Menedżera zasobów i poszczególnych aplikacji ApplicationMaster (AM).

Jest ResourceManager *czystego harmonogramu*i wyłącznie rozstrzyga o kolejności przetwarzania dostępnych zasobów między wszystkie współzawodniczącym aplikacjom. Menedżera zasobów gwarantuje, że wszystkie zasoby są zawsze w użyciu, optymalizacji dla różnych stałych, takich jak umowy SLA, gwarancji wydajności i tak dalej. ApplicationMaster negocjuje zasobów z Menedżera zasobów i współdziała z NodeManager(s) do wykonywania i monitorowania kontenerów i ich zużycia zasobów.

Gdy wiele dzierżaw współużytkuje to dużego klastra, jest konkurencję w odniesieniu do zasobów klastra. CapacityScheduler jest podłączany harmonogram, który pomaga w udostępnianie przez kolejkowanie się żądania zasobów. Obsługuje również CapacityScheduler *hierarchiczne kolejek* aby upewnić się, że zasoby są współdzielone między podrzędnych kolejek organizacji, przed kolejek inne aplikacje mogą korzystać z bezpłatnych zasobów.

YARN pozwala przydzielać zasoby do tych kolejek, a dowiesz się, czy wszystkie dostępne zasoby są przypisane. Aby wyświetlić informacje dotyczące usługi kolejki, zaloguj się do Interfejsu sieci Web Ambari, a następnie wybierz **menedżera kolejki YARN** z górnego menu.

![Menedżer kolejki usługi YARN](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager.png)

Na stronie Menedżera kolejki YARN to lista kolejek usługi po lewej stronie, oraz wartość procentowa pojemności przypisany do każdego.

![Strona szczegółów Menedżera kolejki YARN](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Aby uzyskać bardziej szczegółowy widok Twojej kolejki, z poziomu pulpitu nawigacyjnego systemu Ambari, wybierz **YARN** usługę z listy po lewej stronie. Następnie w obszarze **szybkich łączy** menu rozwijanego wybierz opcję **interfejsu użytkownika Menedżera zasobów** poniżej z aktywnego węzła.

![Łącze menu interfejsu użytkownika Menedżera zasobów](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

W Interfejsie Menedżera zasobów, wybierz **harmonogramu** z menu po lewej stronie. Wyświetlenie listy Twojej kolejki poniżej *kolejek aplikacji*. Tutaj można zobaczyć pojemności używane dla każdej kolejki, w jakim stopniu zadania są dystrybuowane między nimi, i tego, czy dowolne zadania są ograniczone do zasobu.

![Łącze menu interfejsu użytkownika Menedżera zasobów](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui.png)

## <a name="storage-throttling"></a>Ograniczenie magazynu

Klaster wąskim gardłem może się zdarzyć na poziomie magazynu. Tego rodzaju wąskim gardłem jest w większości przypadków ze względu na *blokuje* we/wy (operacje We/Wy), które się zdarzyć, gdy uruchomionych zadań wysyłać więcej we/wy niż usługi storage może obsłużyć. Blokada tworzy kolejkę żądań We/Wy oczekujących na przetworzenie aż po przetworzeniu bieżącego systemu IOs. Bloki są ze względu na *ograniczenie magazynu*, który nie jest to limit fizycznych, ale raczej dotyczy limitu narzuconego przez usługi storage, Umowa dotycząca poziomu usług (SLA). Ten limit zapewnia, że nie jednego klienta lub dzierżawy może zająć całą usługę. Umowy SLA ogranicza liczbę operacji We-Wy na sekundę (IOPS) dla usługi Azure Storage — Aby uzyskać szczegółowe informacje, zobacz [usługi Azure Storage dotyczące skalowalności i cele wydajności](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Jeśli używasz usługi Azure Storage, aby uzyskać informacje na temat monitorowania problemów związanych z magazynu, w tym ograniczanie przepustowości, zobacz [monitorowanie, diagnozowanie i rozwiązywanie problemów z usługi Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Jeśli magazyn zapasowy Twojego klastra usługi Azure Data Lake Store (ADLS), Twoja ograniczanie przepływności jest najprawdopodobniej z powodu ograniczeń przepustowości. Ograniczanie w takiej sytuacji może być identyfikowany przez monitorowanie błędów ograniczania dostępności w dziennikach zadania. Azure Data Lake Store sekcja ograniczania przepustowości usługi odpowiednie w następujących artykułach:

* [Wskazówki dotyczące dostrajania wydajności oprogramowania Hive w usługach HDInsight i Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [Wskazówki dotyczące dostrajania wydajności oprogramowania MapReduce w usługach HDInsight i Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [Wskazówki dotyczące dostrajania wydajności oprogramowania Storm w usługach HDInsight i Azure Data Lake Store](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="next-steps"></a>Kolejne kroki

Skorzystaj z następujących linków, aby uzyskać więcej informacji na temat monitorowania klastrów i rozwiązywania problemów:

* [Analizowanie dzienników usługi HDInsight](hdinsight-debug-jobs.md)
* [Debugowanie aplikacji przy użyciu dzienników YARN](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Włączanie zrzutów sterty dla usług Hadoop w HDInsight opartych na systemie Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
