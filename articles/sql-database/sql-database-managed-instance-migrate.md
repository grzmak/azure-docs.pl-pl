---
title: Migracja wystąpienia programu SQL Server do wystąpienia zarządzanego Azure SQL Database | Dokumentacja firmy Microsoft
description: Dowiedz się, jak przeprowadzić migrację wystąpienia programu SQL Server do wystąpienia zarządzanego Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 6868b842f22a6d107936fcb1e49c46b0c1f58469
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345309"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Migracja wystąpienia programu SQL Server do wystąpienia zarządzanego Azure SQL Database

W tym artykule dowiesz się o metodach migracji programu SQL Server 2005 lub nowszych wersji do [wystąpienia zarządzanego Azure SQL Database](sql-database-managed-instance.md).

Na wysokim poziomie proces migracji bazy danych wygląda następująco:

![Proces migracji](./media/sql-database-managed-instance-migration/migration-process.png)

- [Ocena zgodności wystąpienia zarządzanego](#assess-managed-instance-compatibility)
- [Wybierz opcję połączenie aplikacji](sql-database-managed-instance-connect-app.md)
- [Wdrażanie do optymalnego rozmiaru wystąpienia zarządzanego](#deploy-to-an-optimally-sized-managed-instance)
- [Wybierz metodę migracji, a następnie przeprowadzić migrację](#select-migration-method-and-migrate)
- [Monitorowanie aplikacji](#monitor-applications)

> [!NOTE]
> Aby przeprowadzić migrację pojedynczej bazy danych do pojedynczej bazy danych lub elastycznej puli, zobacz [migracji bazy danych programu SQL Server do usługi Azure SQL Database](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Ocena zgodności wystąpienia zarządzanego

Najpierw Ustal, czy wystąpienie zarządzane jest zgodny z wymagania bazy danych aplikacji. Wystąpienie zarządzane jest zaprojektowany w celu zapewnienia łatwego lift- and -shift migracji dla większości istniejących aplikacji, które używają programu SQL Server w środowisku lokalnym lub na maszynach wirtualnych. Jednak czasami może być wymagane funkcje lub możliwości, które nie są jeszcze obsługiwane i kosztów wdrożenia obejście tego problemu jest zbyt wysokie.

Użyj [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) do wykrywania potencjalnych problemy ze zgodnością mogących mieć wpływ na funkcjonalność bazy danych w usłudze Azure SQL Database. Program DMA nie obsługuje jeszcze zarządzane wystąpienia jako lokalizację docelową migracji, ale zalecane do uruchamiania oceny w bazie danych Azure SQL i dokładnie przejrzyj listę zgłoszonych potrafiło i problemy ze zgodnością z dokumentacji produktu. Zobacz [funkcji usługi Azure SQL Database](sql-database-features.md) Aby sprawdzić, czy istnieją niektóre zgłoszone problemy z blokowaniem, nie blokują w wystąpieniu zarządzanym, ponieważ większość blokowania problemy, co uniemożliwia migracji do usługi Azure SQL Database zostały usunięte z zarządzanego Wystąpienie. Dla wystąpienia funkcji, takich jak zapytania wielu baz danych, transakcje między bazami danych w ramach tego samego wystąpienia serwera połączonego innych SQL źródeł, CLR, globalne tabele tymczasowe, widoki poziomu wystąpienia, Usługa Service Broker i podobne są dostępne w wystąpieniach zarządzanych.

W przypadku niektórych zgłaszane problemy z blokowaniem, które nie są usuwane w wystąpieniu zarządzanym usługi Azure SQL Database, może być konieczne należy wziąć pod uwagę alternatywnych opcji, takich jak [programu SQL Server na maszynach wirtualnych platformy Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Oto kilka przykładów:

- W razie potrzeby bezpośredniego dostępu do systemu operacyjnego lub systemu plików, na przykład instalowanie innych firm lub niestandardowych agentów na tej samej maszyny wirtualnej z programem SQL Server.
- W przypadku ścisłej zależności od funkcji, które nadal nie są obsługiwane, takich jak typu FileStream / FileTable, PolyBase i transakcje dla wielu wystąpień.
- Jeśli bezwzględnie musisz pozostać na określonej wersji programu SQL Server (2012, na przykład).
- Jeśli wymagania dotyczące obliczeń są znacznie niższe tego wystąpienia zarządzanego oferuje w publicznej wersji zapoznawczej (jeden rdzeń wirtualny, na przykład) i konsolidacji bazy danych nie jest dopuszczalne opcji.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Wdrażanie do optymalnego rozmiaru wystąpienia zarządzanego

Wystąpienie zarządzane jest przeznaczony dla lokalnych obciążeń, które planuje przenieść do chmury. Wprowadza [nowy model zakupu](sql-database-service-tiers-vcore.md) , zapewnia większą elastyczność przy wyborze odpowiedniego poziomu zasobów dla obciążeń. W środowisku lokalnym masz prawdopodobnie przyzwyczajeni do ustalania rozmiaru te obciążenia za pomocą rdzeni fizycznych i przepustowość we/wy. Nowy model zakupu dla wystąpienia zarządzanego opiera się na rdzeni wirtualnych lub "rdzeni wirtualnych," przy użyciu dodatkowego miejsca do magazynowania i we/wy dostępne oddzielnie. Model rdzenia wirtualnego jest prostszy sposób, aby poznać wymagania dotyczące obliczeń w chmurze, a nie za środowiska lokalnego już dziś. Ten nowy model umożliwia utworzenie odpowiedniego rozmiaru docelowego środowiska w chmurze.

Można wybrać obliczeniowych i zasobów magazynowania we wdrożeniu czasu, a następnie zmienić je później bez przerwy w działaniu swojej aplikacji za pomocą [witryny Azure portal](sql-database-scale-resources.md):

![ustalanie rozmiaru wystąpienia zarządzanego](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Aby dowiedzieć się, jak utworzyć infrastrukturę sieci wirtualnej i wystąpienie zarządzane, zobacz [utworzysz wystąpienie zarządzane](sql-database-managed-instance-get-started.md).

> [!IMPORTANT]
> Ważne jest, aby zachować swoje docelowej sieci wirtualnej i podsieci zawsze zgodnie z [wymagania dotyczące zarządzanych sieci wirtualnej wystąpienia](sql-database-managed-instance-vnet-configuration.md#requirements). Wszelkie niezgodności mogą uniemożliwić tworzenie nowych wystąpień lub używanie tych systemów, które zostały już utworzone.

## <a name="select-migration-method-and-migrate"></a>Wybierz metodę migracji, a następnie przeprowadzić migrację

Zarządzane scenariusze użytkownika elementy docelowe wystąpienie wymagających migracji pamięci masowej bazy danych ze środowiska lokalnego lub implementacji bazy danych IaaS. Są one optymalnym wyborem, gdy trzeba lift- and -shift zaplecza aplikacji, które regularnie Użyj poziomu wystąpienia i / lub funkcje między bazami danych. W przypadku tego scenariusza, całe wystąpienie można przenieść do odpowiedniego środowiska na platformie Azure bez konieczności ponownego projektowania Twojej aplikacji.

Aby przenieść wystąpienia programu SQL, musisz starannie zaplanować:

- Migracja wszystkich baz danych, które muszą być wspólnie rozmieszczonych (tymi uruchomionych na tym samym wystąpieniu)
- Migracja obiektów na poziomie wystąpienia, które zależy aplikacja, tym logowania, poświadczenia, operatorów i zadań agenta SQL i wyzwalaczy na poziomie serwera.

Wystąpienie zarządzane to w pełni zarządzana usługa, która umożliwia delegowanie niektórych działań DBA regularne do platformy, jak są wbudowane. W związku z tym, niektórych danych na poziomie wystąpienia nie należy migrować, np. zadań konserwacyjnych regularnie Twórz kopie zapasowe lub konfiguracji zawsze włączonej jako [wysokiej dostępności](sql-database-high-availability.md) jest wbudowany.

Wystąpienie zarządzane obsługuje następujące opcje migracji bazy danych (obecnie są to tylko migracja obsługiwanych metod):

- Azure Database Migration Service — migracji prawie bez przestojów
- Natywne `RESTORE DATABASE FROM URL` - używa natywne kopie zapasowe z programu SQL Server i wymaga pewnych przestojów.

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

[Usługi Azure Database Migration Service (DMS)](../dms/dms-overview.md) to w pełni zarządzana usługa ustalono, aby umożliwić bezproblemowe migracje z wielu źródłowych baz danych do platformy danych Azure przy minimalnych przestojach. Ta usługa usprawnia zadania wymagane do przenoszenia istniejących innych firm i baz danych programu SQL Server na platformie Azure. Opcje wdrożenia w publicznej wersji zapoznawczej obejmują usługi Azure SQL Database, wystąpienia zarządzanego i programu SQL Server w maszynie wirtualnej platformy Azure. Usługa DMS jest zalecaną metodą migracji obciążenia przedsiębiorstwa.

Jeśli używasz programu SQL Server Integration Services (SSIS) na program SQL Server w środowisku lokalnym, usługa DMS nie obsługuje jeszcze Migrowanie wykazu usług SSIS (SSISDB), która przechowuje pakietów usług SSIS, ale mogą aprowizować środowiska Azure-SSIS Integration Runtime (IR) w usłudze Azure Data Factory (ADF), których będzie Tworzenie nowej bazy danych SSISDB w usłudze Azure SQL Database/wystąpienia zarządzanego, a następnie ponownie wdrożyć pakiety do niego, zobacz [tworzenie środowiska Azure-SSIS IR w usłudze ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

Aby dowiedzieć się więcej na temat tego scenariusza i konfiguracji kroki przez usługę DMS, zobacz [migracji lokalnej bazy danych do wystąpienia zarządzanego przy użyciu usługi DMS](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>Natywne przywracania z adresu URL

Przywróć natywnego wykonywania kopii zapasowych (pliki z rozszerzeniem bak) pobrane z lokalnego programu SQL Server lub [programu SQL Server na maszynach wirtualnych](https://azure.microsoft.com/services/virtual-machines/sql-server/), która jest dostępna na [usługi Azure Storage](https://azure.microsoft.com/services/storage/), jest jednym z kluczowych możliwości na wystąpieniu zarządzanym bazy danych SQL, Migracja bazy danych umożliwia szybkie i łatwe w trybie offline.

Na poniższym diagramie przedstawiono ogólny przegląd procesu:

![procedury migracji](./media/sql-database-managed-instance-migration/migration-flow.png)

Poniższa tabela zawiera więcej informacji dotyczących metod, których można używać w zależności od wersji programu SQL Server dla źródła, które są uruchomione:

|Krok|Aparat SQL i wersji|Tworzenie kopii zapasowej / Przywracanie — metoda|
|---|---|---|
|Umieść tworzenie kopii zapasowych w usłudze Azure Storage|Pakietu CU2 wcześniejsze SQL 2012 z dodatkiem SP1|Przekaż plik bak bezpośrednio do usługi Azure storage|
||PAKIETU W CU2 2012 Z DODATKIEM SP1 — 2016|Bezpośrednie kopii zapasowej przy użyciu przestarzałe [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) składni|
||2016 i nowsze wersje|Przy użyciu kopii zapasowej bezpośredniego [przy użyciu POŚWIADCZEŃ sygnatury dostępu Współdzielonego](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Przywróć z usługi Azure Storage do wystąpienia zarządzanego|[Przywróć z adresu URL przy użyciu POŚWIADCZEŃ sygnatury dostępu Współdzielonego](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - Podczas migracji bazy danych chronionych przez [funkcji Transparent Data Encryption](transparent-data-encryption-azure-sql.md) odpowiedni certyfikat z wdrożenia lokalne czy IaaS programu SQL Server do bazy danych wystąpienia zarządzanego Azure SQL przy użyciu opcji przywracania natywnych, muszą zostać zmigrowane przed przywróceniem bazy danych. Aby uzyskać szczegółowe instrukcje, zobacz [cert TDE migracji do wystąpienia zarządzanego](sql-database-managed-instance-migrate-tde-certificate.md)
> - Przywracanie bazy danych systemu nie jest obsługiwane. Aby przeprowadzić migrację obiektów na poziomie wystąpienia (przechowywane w bazach danych master i msdb), zaleca się ich skryptu i uruchamianie skryptów T-SQL w wystąpieniu docelowym.

Aby uzyskać szybki start przedstawiający sposób przywracania kopii zapasowej bazy danych do wystąpienia zarządzanego przy użyciu poświadczeń sygnatury dostępu Współdzielonego, zobacz [Przywracanie z kopii zapasowej do wystąpienia zarządzanego](sql-database-managed-instance-get-started-restore.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

## <a name="monitor-applications"></a>Monitorowanie aplikacji

Śledź sposób działania aplikacji i wydajności, po zakończeniu migracji. W wystąpieniu zarządzanym niektóre zmiany są włączyć tylko raz [poziom zgodności bazy danych został zmieniony](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Migracja bazy danych do usługi Azure SQL Database przechowuje jej oryginalnego poziom zgodności, w większości przypadków. Jeśli poziom zgodności bazy danych użytkownika był 100 lub wyższym przed migracją, pozostaje taki sam po migracji. Jeśli poziom zgodności bazy danych użytkownika 90 przed migracją w bazie danych uaktualniony poziom zgodności jest ustawiony na 100, czyli poziom najniższą obsługiwany zgodność w wystąpieniu zarządzanym. Poziom zgodności bazy danych systemu jest 140.

Aby zmniejszyć ryzyko migracji, należy zmienić poziom zgodności bazy danych dopiero po monitorowania wydajności. Użyj zapytania Store jako optymalne narzędzia w celu uzyskania informacji na temat wydajność obciążeń przed i po zmianie poziomu zgodności bazy danych, jak wyjaśniono w [zachować wydajność stabilność podczas uaktualnienia do nowszej wersji programu SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Po użytkownik pracuje na w pełni zarządzanej platformy, należy wykonać korzyści, które są dostarczane automatycznie w ramach usługi SQL Database. Na przykład nie trzeba tworzyć kopie zapasowe na wystąpieniu zarządzanym — usługa wykonuje kopie zapasowe dla Ciebie automatycznie. Już nie musisz martwić o planowania, wykonywania i zarządzanie kopiami zapasowymi. Wystąpienie zarządzane zapewnia możliwość przywracania do dowolnego punktu w czasie w ramach tego przechowywania okresu przy użyciu [punktu w czasie odzyskiwania (Odzyskiwanie)](sql-database-recovery-using-backups.md#point-in-time-restore). W publicznej wersji zapoznawczej okres przechowywania zostanie usunięty z siedmiu dni.
Ponadto nie trzeba już martwić się o Konfigurowanie wysokiej dostępności jako [wysokiej dostępności](sql-database-high-availability.md) jest wbudowany.

Do zwiększenia poziomu zabezpieczeń, należy rozważyć zastosowanie niektórych funkcji, które są dostępne:

- Uwierzytelnianie usługi Azure Active Directory na poziomie bazy danych
- Użyj [zaawansowane funkcje zabezpieczeń](sql-database-security-overview.md) takich jak [inspekcji](sql-database-managed-instance-auditing.md), [wykrywania zagrożeń](sql-advanced-threat-protection.md), [zabezpieczenia na poziomie wiersza](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), i [dynamiczne Maskowanie danych](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ) do zabezpieczenia Twojego wystąpienia.

## <a name="next-steps"></a>Kolejne kroki

- Aby uzyskać informacje o wystąpieniach zarządzanych, zobacz [co to jest wystąpienie zarządzane?](sql-database-managed-instance.md).
- Aby uzyskać samouczek, który obejmuje Przywracanie z kopii zapasowej, zobacz [utworzysz wystąpienie zarządzane](sql-database-managed-instance-get-started.md).
- Samouczek przedstawiający podczas migracji przy użyciu usługi DMS zobacz [migracji lokalnej bazy danych do wystąpienia zarządzanego przy użyciu usługi DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
