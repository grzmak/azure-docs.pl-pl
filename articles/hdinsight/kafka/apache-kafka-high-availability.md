---
title: Wysoka dostępność dzięki platformie Apache Kafka — Azure HDInsight
description: Dowiedz się, jak zapewnić wysoką dostępność dzięki platformie Apache Kafka w usłudze Azure HDInsight. Dowiedz się, jak przeprowadzić ponowne równoważenie replik partycji na platformie Kafka, tak aby znajdowały się one w różnych domenach błędów w regionie świadczenia usługi Azure, który udostępnia usługę HDInsight.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: bd3b02d54e0a65411e45f0422a0d245645d59096
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049975"
---
# <a name="high-availability-of-your-data-with-apache-kafka-on-hdinsight"></a>Wysoka dostępność danych dzięki platformie Apache Kafka w usłudze HDInsight

Dowiedz się, jak skonfigurować repliki partycji dla tematów platformy Kafka w celu korzystania z bazowej konfiguracji regałów na sprzęt. Ta konfiguracja zapewnia dostępność danych przechowywanych na platformie Apache Kafka w usłudze HDInsight.

## <a name="fault-and-update-domains-with-kafka"></a>Domeny błędów i domeny aktualizacji z platformą Kafka

Domena błędów to logiczna grupa bazowego sprzętu w centrum danych platformy Azure. Wszystkie domeny błędów korzystają ze wspólnego źródła zasilania i przełącznika sieciowego. Maszyny wirtualne i dyski zarządzane, które implementują węzły w klastrze usługi HDInsight są rozdzielone między te domeny błędów. Taka architektura ogranicza wpływ potencjalnych awarii sprzętu fizycznego.

W każdym regionie świadczenia usługi Azure znajduje się określona liczba domen błędów. Aby uzyskać listę domen i informacje o liczbie zawartych w nich domen błędów, zobacz dokument [Zestawy dostępności](../../virtual-machines/windows/regions-and-availability.md#availability-sets).

> [!IMPORTANT]
> Platforma Kafka nie uwzględnia domen błędów. W przypadku utworzenia tematu na platformie Kafka wszystkie repliki partycji mogą być przechowywane w tej samej domenie błędów. Aby rozwiązać ten problem, usługa HDInsight udostępnia [narzędzie do ponownego równoważenia partycji platformy Kafka](https://github.com/hdinsight/hdinsight-kafka-tools).

## <a name="when-to-rebalance-partition-replicas"></a>Kiedy należy stosować ponowne równoważenie replik partycji

Aby zapewnić najwyższą dostępność danych na platformie Kafka, należy stosować ponowne równoważenie replik partycji dla tematu w następujących sytuacjach:

* Po utworzeniu nowego tematu lub partycji

* Po przeskalowaniu klastra w górę

## <a name="replication-factor"></a>Współczynnik replikacji

> [!IMPORTANT]
> Zalecamy wybranie regionu świadczenia usługi Azure zawierającego trzy domeny błędów oraz użycie współczynnika replikacji o wartości 3.

Jeśli musisz wybrać region, który zawiera tylko dwie domeny błędów, użyj współczynnika replikacji o wartości 4, aby równomiernie rozłożyć repliki na dwie domeny błędów.

Aby zapoznać się z przykładem obejmującym tworzenie tematów i ustawianie współczynnika replikacji, zobacz dokument [Start with Kafka on HDInsight (Rozpoczynanie pracy z platformą Kafka w usłudze HDInsight)](apache-kafka-get-started.md).

## <a name="how-to-rebalance-partition-replicas"></a>Jak zastosować ponowne równoważenie replik partycji

Aby ponownie zrównoważyć wybrane tematy, użyj [narzędzia do ponownego równoważenia partycji platformy Kafka](https://github.com/hdinsight/hdinsight-kafka-tools). Narzędzie to należy uruchomić w sesji połączenia SSH z węzłem głównym klastra Kafka.

Aby uzyskać więcej informacji dotyczących nawiązywania połączenia z usługą HDInsight przy użyciu protokołu SSH, zobacz dokument [Używanie protokołu SSH z usługą HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="next-steps"></a>Kolejne kroki

* [Scalability of Kafka on HDInsight (Skalowalność platformy Kafka w usłudze HDInsight)](apache-kafka-scalability.md)
* [Mirroring with Kafka on HDInsight (Dublowanie przy użyciu platformy Kafka w usłudze HDInsight)](apache-kafka-mirroring.md)
