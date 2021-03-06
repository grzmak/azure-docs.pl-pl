---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: include
ms.date: 04/06/2018
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: 76e1a22e02ad6b8161ec18f3806c88ffc54fe123
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "34665382"
---
Jeśli obecnie używasz usługi Azure Table Storage, po rozpoczęciu korzystania z interfejsu API tabel usługi Azure Cosmos DB uzyskasz następujące korzyści:

| | Azure Table Storage | Interfejs API tabel usługi Azure Cosmos DB |
| --- | --- | --- |
| Opóźnienie | Niewielkie, ale brak górnych granic opóźnienia. | Milisekundowe opóźnienie odczytu i zapisu oraz odczyt o opóźnieniu <10 ms i zapis o opóźnieniu <15 ms w 99. percentylu, w dowolnej skali, w dowolnym miejscu na świecie. |
| Przepływność | Zmienny model przepływności. Tabele mają limit skalowalności 20 000 operacji/s. | Wysoka skalowalność dzięki [dedykowanej zarezerwowanej przepływności na tabelę](../articles/cosmos-db/request-units.md), gwarantowanej umowami SLA. Konta nie mają górnego limitu przepływności i obsługują >10 milionów operacji/s na tabelę. |
| Dystrybucja globalna | Jeden region z jednym opcjonalnym dodatkowym regionem odczytu, co zapewnia wysoką dostępność. Nie można zainicjować trybu failover. | [Kompleksowa dystrybucja globalna](../articles/cosmos-db/distribute-data-globally.md) do ponad 30 regionów. Obsługa [automatycznego i ręcznego trybu failover](../articles/cosmos-db/regional-failover.md) w dowolnym momencie i każdym miejscu na świecie. |
| Indeksowanie | Tylko podstawowy indeks PartitionKey i RowKey. Brak dodatkowych indeksów. | Automatyczne i kompletne indeksowanie wszystkich właściwości, brak zarządzania indeksem. |
| Zapytanie | Wykonanie zapytania wykorzystuje indeks klucza podstawowego, a w przeciwnym przypadku skanuje. | Zapytania mogą korzystać z automatycznego indeksowania właściwości, co skraca czas odpowiedzi. |
| Spójność | Na poziomie „strong” w regionie podstawowym, na poziomie „eventual” w regionie pomocniczym. | [Pięć dobrze zdefiniowanych poziomów spójności](../articles/cosmos-db/consistency-levels.md), równoważących dostępność, opóźnienia, przepływność i spójność w zależności od potrzeb aplikacji. |
| Cennik | Optymalizacja pod kątem magazynu. | Optymalizacja pod kątem przepływności. |
| Umowy SLA | Dostępność na poziomie 99,99%. | Umowa SLA gwarantująca dostępność na poziomie co najmniej 99,99% dla wszystkich kont w obrębie jednego regionu i wszystkich kont w wielu regionach w przypadku rozluźnionej spójności, a także dostępność do odczytu na poziomie co najmniej 99,999% dla wszystkich kont bazy danych w wielu regionach w ramach [wiodących w branży, kompleksowych umów SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) dotyczących ogólnej dostępności. |
