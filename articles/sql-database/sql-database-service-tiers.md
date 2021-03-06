---
title: Usługa Azure SQL Database zakupu modeli | Dokumentacja firmy Microsoft
description: Więcej informacji na temat model zakupu modeli, które są dostępne bazy danych w usłudze Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/17/2018
ms.openlocfilehash: 3b2359564020eeeb209a7eb78d81782a675f125d
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379291"
---
# <a name="azure-sql-database-purchasing-models"></a>Usługa Azure SQL Database zakupu modeli

Usługa Azure SQL Database umożliwia łatwy zakup w pełni zarządzany aparat bazy danych PaaS, który spełnia Twoje potrzeby wydajności i kosztów. W zależności od modelu wdrażania usługi Azure SQL Database można wybrać model zakupu, która spełnia Twoje potrzeby:

- [Serwerami logicznymi](sql-database-logical-servers.md) w [usługi Azure SQL Database](sql-database-technical-overview.md) oferuje dwa modele zakupu zasobów obliczeniowych, magazynu i zasoby we/wy: [modelu zakupu opartego na jednostkach DTU](sql-database-service-tiers-dtu.md) i [oparty na rdzeniach wirtualnych model zakupu](sql-database-service-tiers-vcore.md). Ten model zakupu, można wybrać [pojedyncze bazy danych](sql-database-single-databases-manage.md) lub [pul elastycznych](sql-database-elastic-pool.md).
- [Wystąpienia zarządzane](sql-database-managed-instance.md) w usłudze Azure SQL Database tylko w ramach oferty [modelu zakupu opartego na rdzeniach wirtualnych](sql-database-service-tiers-vcore.md).

> [!IMPORTANT]
> [Bazy danych na dużą skalę (wersja zapoznawcza)](sql-database-service-tier-hyperscale.md) znajdują się w publicznej wersji zapoznawczej tylko w przypadku pojedynczych baz danych za pomocą rdzeni wirtualnych zakupem modelu.

Następujących tabel i wykresów porównania i porównać te dwa modele zakupu.

|**Model zakupu**|**Opis**|**Najlepsze dla**|
|---|---|---|
|Model oparty na jednostkach DTU|Ten model opiera się na powiązane miary obliczeniowe, magazynu i zasobów we/wy. Obliczenia rozmiarów są wyrażone w jednostkach transakcji bazy danych (Dtu) dla pojedynczych baz danych i jednostek transakcji elastic Database (Edtu) dla pul elastycznych. Aby uzyskać więcej informacji na temat jednostek Dtu i Edtu, zobacz [co to są jednostki Dtu i Edtu](sql-database-service-tiers.md#dtu-based-purchasing-model)?|Najlepsze dla klientów chcących zasobów proste, wstępnie skonfigurowanych opcji.| 
|Model oparty na rdzeniach wirtualnych|Ten model umożliwia niezależne Wybierz zasoby obliczeniowe i magazynowe. Umożliwia on również używać korzyści użycia hybrydowego platformy Azure dla programu SQL Server w celu uzyskania oszczędności kosztów.|Najlepsze dla klientów, którzy wartości elastyczności, kontroli i przejrzystości.|
||||  

![model cen](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>Model zakupu bazujący na rdzeniach wirtualnych 

Rdzeń wirtualny reprezentuje logiczny Procesor CPU z opcją wyboru generacji sprzętu i cechy fizyczne sprzętu (na przykład liczba rdzeni, pamięć, rozmiar magazynu). Model zakupu opartego na rdzeniach wirtualnych zapewnia Twojej elastyczności, kontroli, przejrzystości użycia poszczególnych zasobów i prostą metodę tłumaczenia wymagań obciążenia w chmurze lokalnie. Ten model umożliwia wybierz obliczeniowych, pamięci i magazynu, w zależności od ich potrzeb obciążenia. W opartych na rdzeniach wirtualnych model zakupu, można wybrać między [ogólnego przeznaczenia](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) i [krytyczne dla działania](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) warstwy usług dla obu [pojedyncze bazy danych](sql-database-single-database-scale.md), [ wystąpienia zarządzane](sql-database-managed-instance.md), i [pul elastycznych](sql-database-elastic-pool.md). Dla pojedynczych baz danych, można także [(wersja zapoznawcza) na dużą skalę](sql-database-service-tier-hyperscale.md) warstwy usług.

Model zakupu opartego na rdzeniach wirtualnych umożliwia niezależnie wybrać zasoby obliczeniowe i magazynowe, Dopasuj wydajność środowiska lokalnego i optymalizacja ceny. Oparty na rdzeniach wirtualnych model zakupu klienci płacą za zasoby:

- Obliczenia (warstwy usług + liczba rdzeni wirtualnych i ilość pamięci i generowanie sprzętu)
- Typ i ilość miejsca w magazynie danych i dziennika 
- Magazyn kopii zapasowych (RA-GRS) 

> [!IMPORTANT]
> Moc obliczeniowa, IOs, dane i Magazyn dzienników są naliczane zgodnie z bazy danych lub elastycznej puli. Magazyn kopii zapasowych jest rozliczane na każdej bazy danych. Aby uzyskać szczegółowe informacje o opłaty za wystąpienia zarządzanego, zapoznaj się [wystąpienia zarządzanego Azure SQL Database](sql-database-managed-instance.md).
> **Ograniczenia dotyczące regionów:** modelu zakupu opartego na rdzeniach wirtualnych nie jest jeszcze dostępna w następujących regionach: Europa Zachodnia, Francja środkowa, południowe Zjednoczone Królestwo, zachodnie Zjednoczone Królestwo i Australia południowo-wschodnia.

Jeśli bazy danych lub elastycznej puli zajmuje ponad 300 Konwersja jednostek DTU na rdzeniach wirtualnych może zmniejszyć koszt. Możesz również przekonwertować przy użyciu wybranego interfejsu API lub portalu Azure, bez przestojów. Jednak konwersja nie jest wymagana. Jeśli model zakupu jednostek DTU spełnia swoje wymagania biznesowe i wydajności, można nadal jej używać. Jeśli zdecydujesz się przekonwertować modelu rdzenia wirtualnego z modelu jednostek DTU, należy wybrać rozmiar obliczeń przy użyciu następujące reguły akceptacji: każdy 100 jednostek DTU w warstwie standardowa wymaga co najmniej 1 rdzeń wirtualny w warstwie przeznaczenie ogólne; Każdy 125 jednostek DTU w warstwie Premium wymaga co najmniej 1 rdzeń wirtualny w warstwie krytyczne dla działania firmy.

## <a name="dtu-based-purchasing-model"></a>Model zakupu w oparciu o jednostki DTU

Jednostki transakcji bazy danych (DTU) reprezentuje mieszany pomiar procesora CPU, pamięci, odczytuje i zapisuje. Model zakupu opartego na jednostkach DTU oferuje zestaw wstępnie skonfigurowane pakiety zasobów obliczeniowych i magazynu dostosowane do różnych poziomów wydajności aplikacji w pakiecie. Klienci, którzy preferują prostotę wstępnie skonfigurowanego pakietu i płatności w stałej kwocie każdego miesiąca, może się okazać modelu opartego na jednostkach DTU bardziej odpowiednie do ich potrzeb. W oparty na jednostkach DTU model zakupu, klienci mogą wybierać między **podstawowe**, **standardowa**, i **Premium** warstwy usług dla obu [pojedyncze bazy danych](sql-database-single-database-scale.md) i [pul elastycznych](sql-database-elastic-pool.md). Ten model zakupu nie jest dostępna w [wystąpienia zarządzane](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Jednostki transakcji bazy danych (Dtu)

Dla pojedynczej bazy danych Azure SQL na określonym obliczenia rozmiaru na [warstwy usług](sql-database-single-database-scale.md), firma Microsoft gwarantuje więc pewnego poziomu zasobów dla tej bazy danych (niezależnie od innej bazy danych w chmurze platformy Azure), zapewniając przewidywalne poziom wydajności. Ilość zasobów jest obliczany jako liczba jednostek transakcji bazy danych lub liczby jednostek Dtu i jest miarą powiązane obliczeń, magazynu i zasobów we/wy. Współczynnik obejmujący te zasoby był pierwotnie określany za [obciążenia porównawczego OLTP](sql-database-benchmark-overview.md), jest przeznaczona do typowym, rzeczywistym obciążeniom OLTP. Gdy obciążenie przekracza ilość dowolnego z następujących zasobów, przepływność jest ograniczone — wynikowy niska wydajność i przekroczenia limitu czasu. Zasoby używane przez obciążenie nie miało wpływu na zasoby dostępne dla innych baz danych SQL w chmurze platformy Azure, a zasoby używane przez inne obciążenia nie miało wpływu na zasoby dostępne dla usługi SQL database.

![obwiedni](./media/sql-database-what-is-a-dtu/bounding-box.png)

Liczba jednostek Dtu są najbardziej przydatne do zrozumienia względna ilość zasobów między bazami danych Azure SQL w różnych rozmiarów wystąpień obliczeniowych i warstwy usług. Na przykład podwojenie liczby jednostek Dtu przez zwiększenie rozmiaru obliczeń bazy danych odpowiada dwukrotnemu zwiększeniu zestawu zasobów dostępnych dla tej bazy danych. Na przykład baza danych Premium P11 z 1750 jednostkami DTU zapewnia 350 razy więcej mocy obliczeniowej DTU niż podstawowa baza danych z 5 jednostkami DTU.  

Aby uzyskać lepszy wgląd w użycie zasobów (DTU), obciążenia, należy użyć [usługi Azure SQL Database Query Performance Insight](sql-database-query-performance.md) do:

- Zidentyfikuj najpopularniejsze zapytania według liczby Procesor/czas trwania/wykonywania, które potencjalnie mogą być dostosowane w celu zwiększenia wydajności. Na przykład, zapytanie intensywnie korzystających z operacji We/Wy organizowanych przez użycie [techniki optymalizacji w pamięci](sql-database-in-memory.md) skuteczniej wykorzystać dostępnej pamięci w pewnym warstwę usługi i obliczenia rozmiaru.
- Przechodzenie do szczegółów kwerendy, wyświetlanie historii wykorzystania zasobów i tekstem.
- Zalecenia dotyczące, które pokazują akcje wykonywane przez dostrajania wydajności dostępu [SQL Database Advisor](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Jednostki transakcji elastic Database (Edtu)

Raczej niż zapewniają dedykowany zestaw zasobów (Dtu), które może nie zawsze być wymagane dla usługi SQL Database, która jest zawsze dostępna, można umieścić bazy danych [puli elastycznej](sql-database-elastic-pool.md) na serwerze bazy danych SQL, który udostępnia pulę zasobów między te bazy danych. Współdzielone zasoby w puli elastycznej są mierzone w elastycznych jednostkach transakcji bazy danych lub Edtu. Pule elastyczne zapewniają proste i ekonomiczne rozwiązanie umożliwiające zarządzanie celami wydajności dla wielu baz danych o znacznie zróżnicowanych i nieprzewidywalnych wzorcach użycia. Pula elastyczna gwarantuje, że zasoby nie mogą być używane przez jedną bazę danych w puli, gdy zawsze zapewnienie każdej bazy danych w puli ma minimalną ilość wymaganych zasobów dostępnych. 

![Wprowadzenie do usługi SQL Database: jednostki eDTU według warstwy i poziomu](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Puli jest przydzielana określona liczba jednostek Edtu za określoną cenę. Poszczególne bazy danych w ramach puli elastycznej mają możliwość elastycznego skalowania automatycznego w skonfigurowanych granicach. Bazy danych przy większych obciążeniach zajmie więcej jednostek Edtu w celu spełnienia określonych wymagań. Bazy danych pod obciążeniem jaśniejszy zajmie mniej jednostek Edtu. Bazy danych bez obciążenia będą nie zużywają jednostek Edtu. Aprowizowanie zasobów dla całej puli, zamiast na bazę danych, upraszcza zadania zarządzania, zapewniając przewidywalny budżet dla puli.

Dodatkowe jednostki eDTU można dodać do istniejącej puli bez przerwy w działaniu baz danych ani wpływu na bazy danych w puli. Podobnie jeśli dodatkowe jednostki eDTU nie są już potrzebne, można je usunąć z istniejącej puli w dowolnej chwili. Możesz dodawać lub odejmować baz danych do puli lub ograniczyć liczbę jednostek Edtu bazy danych można używać pod dużym obciążeniem, aby zarezerwować je dla innych baz danych. Jeśli bazy danych jest przewidywalnie niewystarczająco wykorzystuje zasoby, możesz przenieść ją z puli i skonfigurować go jako pojedynczą bazę danych z wymaganą ilością wymaganych zasobów.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Określenie liczby jednostek Dtu wymaganych przez obciążenie

Jeśli chcesz zmigrować istniejące obciążenie lokalne lub obciążenie maszyny wirtualnej programu SQL Server do usługi Azure SQL Database, możesz skorzystać z [Kalkulatora jednostek DTU ](http://dtucalculator.azurewebsites.net/), aby obliczyć przybliżoną liczbę wymaganych jednostek DTU. W przypadku istniejących obciążeń usługi Azure SQL Database, możesz użyć [SQL Database Query Performance Insight](sql-database-query-performance.md) Aby poznać użycie zasobów bazy danych (Dtu), aby uzyskać lepszy wgląd w celu optymalizacji działalności obciążenie. Można również użyć [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV, aby wyświetlić zużycie zasobów w ciągu ostatniej godziny. Alternatywnie widoku wykazu [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) przedstawia użycie zasobów w ciągu ostatnich 14 dni, ale o mniejszej dokładności pięciu minut.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Obciążenia, które korzystają z elastycznej puli zasobów

Pule są odpowiednie dla wielu baz danych o określonych wzorcach użycia. Dla danej bazy danych ten wzorzec charakteryzuje się średnio o niskim zapotrzebowaniu, przy użyciu stosunkowo rzadkimi okresami zwiększonego. Usługa SQL Database automatycznie ocenia historyczne użycie zasobów przez bazy danych na istniejącym serwerze usługi SQL Database i zaleca odpowiednią konfigurację puli w witrynie Azure Portal. Aby uzyskać więcej informacji, zobacz [Kiedy należy użyć puli elastycznej?](sql-database-elastic-pool.md)

## <a name="next-steps"></a>Kolejne kroki

- Dla modelu zakupu opartego na rdzeniach wirtualnych, zobacz [modelu zakupu opartego na rdzeniach wirtualnych](sql-database-service-tiers-vcore.md)
- Oparte na jednostkach DTU model zakupu, można zobaczyć [modelu zakupu opartego na jednostkach DTU](sql-database-service-tiers-dtu.md).
