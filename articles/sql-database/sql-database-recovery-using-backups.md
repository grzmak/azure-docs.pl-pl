---
title: Przywróć bazę danych Azure SQL database z kopii zapasowej | Dokumentacja firmy Microsoft
description: Więcej informacji na temat przywracania w momencie, która umożliwia przywracanie usługi Azure SQL Database do wcześniejszego punktu w czasie (do 35 dni).
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/08/2018
ms.openlocfilehash: ad82ae6158bbf343dd84be125a8839e992ad3634
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868160"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Odzyskiwanie bazy danych Azure SQL za pomocą bazy danych automatycznych kopii zapasowych

Domyślnie kopie zapasowe bazy danych SQL są przechowywane w georeplikowanym magazynie obiektów blob (RA-GRS). Poniższe opcje są dostępne dla bazy danych odzyskiwania przy użyciu [automatyczne kopie zapasowe bazy danych](sql-database-automated-backups.md):

- Utwórz nową bazę danych na tym samym serwerze logicznym odzyskiwane do określonego punktu w czasie w okresie przechowywania.
- Tworzenie bazy danych na tym samym serwerze logicznym odzyskiwane do czasu usunięcia dla usuniętej bazy danych.
- Utwórz nową bazę danych na dowolnym serwerze logicznym w dowolnym regionie przywrócona do stanu najnowszej kopii zapasowych.

Jeśli skonfigurowano [kopii zapasowych, długoterminowe przechowywanie](sql-database-long-term-retention.md) można również utworzyć nową bazę danych z dowolnej kopii zapasowej od lewej do prawej, na dowolnym serwerze logicznym, w dowolnym regionie.  

> [!IMPORTANT]
> Nie można zastąpić istniejącą bazę danych podczas przywracania.

Korzystając z warstwy usługi Standard lub Premium, przywróconej bazy danych wiąże się koszt dodatkowego magazynu w następujących warunkach:

- Przywracanie P11 – P15 S4-S12 lub P1 – P6, jeśli maksymalnego rozmiaru bazy danych jest większa niż 500 GB.
- Przywracanie P1 – P6 do S4-S12, jeśli maksymalnego rozmiaru bazy danych jest większa niż 250 GB.

Dodatkowy koszt jest, ponieważ maksymalny rozmiar przywróconej bazy danych jest większe niż wielkość magazynu oferowanego w pakiecie dla rozmiaru obliczeń i zaaprowizowanego magazynu dodatkowego aprowizowanego ponad uwzględnioną kwotę jest bardzo obciążona.  Aby uzyskać szczegóły dodatkowego magazynu, zobacz [SQL Database, cennik](https://azure.microsoft.com/pricing/details/sql-database/).  Jeśli rzeczywistą ilość miejsca jest mniejsza niż wielkość magazynu oferowanego w pakiecie, następnie to dodatkowych kosztów można uniknąć przez ograniczenie maksymalnego rozmiaru bazy danych do uwzględnioną kwotę.  

> [!NOTE]
> [Automatyczne kopie zapasowe bazy danych](sql-database-automated-backups.md) są używane podczas tworzenia [kopiowanie bazy danych](sql-database-copy.md).

## <a name="recovery-time"></a>Godzina odzyskiwania

Czas odzyskiwania, aby przywrócić bazę danych przy użyciu kopii zapasowych automatycznych bazy danych ma wpływ na kilka czynników:

- Rozmiar bazy danych
- Rozmiar obliczeń bazy danych
- Liczba zaangażowanych dzienniki transakcji
- Zmniejszenia liczby działań, który ma być powtórzone odzyskiwanie do punktu przywracania
- Przepustowość sieci, jeśli przywracania jest w innym regionie
- Liczba żądań współbieżnych przywracania przetwarzanych w regionie docelowym
  
Dla bardzo dużych i/lub aktywnej bazy danych przywracania może zająć kilka godzin. Jeśli region jest długotrwały Przestój, jest możliwe, że istnieją dużą liczbę żądań przywracania geograficznego przetwarzanych przez inne regiony. W przypadku dużej liczby żądań, czas odzyskiwania może się zwiększyć dla baz danych, w tym regionie. Większość bazy danych przywraca ukończone w ciągu 12 godzin.

W przypadku pojedynczej subskrypcji istnieją pewne ograniczenia na liczbę żądań współbieżnych przywracania (w tym punktu przywracania, przywracanie geograficzne i przywrócenie z długoterminowe przechowywanie kopii zapasowej) są przesyłane i zanalizować:

|  | **Maksymalna liczba jednoczesnych żądań przetwarzanych** | **Maksymalna liczba jednoczesnych żądań przesyłane** |
| :--- | --: | --: |
|Pojedynczą bazę danych (na subskrypcję)|10|60|
|Puli elastycznej (na pulę)|4|200|
||||

Nie ma żadnych funkcji wbudowanych zbiorczo przywracania. [Usługi Azure SQL Database: pełne odzyskiwanie serwera](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) skrypt jest przykładem jednym ze sposobów wykonywania tego zadania.

> [!IMPORTANT]
> Aby odzyskać za pomocą automatycznych kopii zapasowych, musi być członkiem roli Współautor serwera SQL Server w ramach subskrypcji lub być właścicielem subskrypcji — zobacz [RBAC: wbudowane role](../role-based-access-control/built-in-roles.md). Odzyskiwanie można przeprowadzić za pomocą witryny Azure Portal, programu PowerShell lub interfejsu API REST. Nie można używać języka Transact-SQL.

## <a name="point-in-time-restore"></a>Przywracanie do określonego momentu

Można przywrócić istniejącą bazę danych do wcześniejszego punktu w czasie jako nową bazę danych na tym samym serwerze logicznym przy użyciu portalu Azure [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), lub [interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [!TIP]
> Aby uzyskać przykładowy skrypt programu PowerShell przedstawiający sposób wykonywania w momencie przywracania bazy danych, zobacz [przywrócić bazę danych SQL przy użyciu programu PowerShell](scripts/sql-database-restore-database-powershell.md).

Można przywrócić bazy danych do dowolnej warstwy usługi lub rozmiaru obliczeń i jako pojedynczą bazę danych lub do puli elastycznej. Upewnij się, że masz wystarczające zasoby, na serwerze logicznym lub w puli elastycznej, na którym odbywa się przywracanie bazy danych. Po wykonaniu tych czynności, przywróconej bazy danych jest normalne, w pełni dostępne online bazy danych. Przywrócona baza danych jest rozliczana według normalnych stawek za użycie na podstawie jego warstwy usług i rozmiaru obliczeń. Do czasu ukończenia przywracania bazy danych nie są naliczane opłaty.

Ogólnie przywrócić bazę danych do wcześniejszego punktu w celach recovery. Po wykonaniu tej czynności można traktować przywróconej bazy danych jako mogą zastąpić oryginalnej bazy danych lub użyć go do pobierania danych z, a następnie zaktualizuj oryginalnej bazy danych.

- **Zastępowania bazy danych**

   Jeśli przywróconej bazy danych jest przeznaczony jako zamiennika dla oryginalnej bazy danych, należy sprawdzić rozmiar obliczeń i/lub warstwy usług są odpowiednie, a także skalowanie bazy danych, jeśli to konieczne. Można zmienić nazwy oryginalnej bazy danych, a następnie nadaj przywróconej bazy danych do oryginalnej nazwy przy użyciu [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) polecenia języka T-SQL.

- **Odzyskiwanie danych**

   Jeśli planowane jest do pobierania danych z przywróconej bazy danych, aby odzyskać sprawność po błędzie użytkownika lub aplikacji, musisz zapisu i wykonywania skryptów odzyskiwania danych niezbędnych do wyodrębniania danych z przywróconej bazy danych do oryginalnej bazy danych. Mimo, że operacja przywracania może zająć dużo czasu, przywracania bazy danych jest widoczny na liście baz danych w całym procesie przywracania. Jeśli usuniesz bazę danych podczas przywracania, operacja przywracania została anulowana i nie są naliczane dla bazy danych, która nie została ukończona, przywracania.

### <a name="recover-to-a-point-in-time-using-azure-portal"></a>Odzyskiwanie do punktu w czasie za pomocą witryny Azure portal

Aby odzyskać do punktu w czasie za pomocą witryny Azure portal, otwórz stronę bazy danych, a następnie kliknij przycisk **przywrócić** na pasku narzędzi.

![punkt w czasie przywracania](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Przywracanie usuniętej bazy danych

Można przywrócić usuniętą bazę danych do czasu usunięcia dla usuniętej bazy danych na tym samym serwerze logicznym przy użyciu portalu Azure [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), lub [REST (createMode = przywracanie)](https://msdn.microsoft.com/library/azure/mt163685.aspx). Można przywrócić usuniętą bazę danych do wcześniejszego punktu w czasie przechowywania, używając [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase).

> [!Note]
> Przywracanie usuniętej bazy danych nie jest dostępny w wystąpieniu zarządzanym.
> [!TIP]
> Aby uzyskać przykładowy skrypt programu PowerShell przedstawiająca sposób przywrócić usuniętą bazę danych, zobacz [przywrócić bazę danych SQL przy użyciu programu PowerShell](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Jeśli usuniesz wystąpienia serwera usługi Azure SQL Database, jego baz danych również zostaną usunięte i nie można go odzyskać. Obecnie nie jest obsługiwane dla przywracanie usuniętych serwera.

### <a name="recover-a-deleted-database-using-the-azure-portal"></a>Odzyskiwanie usuniętej bazy danych przy użyciu witryny Azure portal

Aby odzyskać usuniętą bazę danych podczas jego [okres przechowywania modelu opartego na jednostkach DTU](sql-database-service-tiers-dtu.md) lub [okres przechowywania model oparty na rdzeniach wirtualnych](sql-database-service-tiers-vcore.md) przy użyciu witryny Azure portal, otwórz stronę na serwerze i w obszarze operacji, kliknij przycisk **Usuniętych baz danych**.

![usunięte database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![Usunięto database przywracania-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Przywracanie geograficzne

Możesz przywrócić bazę danych SQL na dowolnym serwerze w dowolnym regionie systemu Azure z ostatnich replikowanej geograficznie pełnych i różnicowych kopii zapasowych. Przywracanie geograficzne korzysta z geograficznie nadmiarowej kopii zapasowej jako źródła i może służyć do odzyskiwania bazy danych, nawet jeśli bazy danych lub centrum danych jest niedostępna z powodu awarii.

> [!Note]
> Przywracanie geograficzne nie jest dostępny w wystąpieniu zarządzanym.

Funkcja przywracania geograficznego jest domyślną opcję odzyskiwania, gdy baza danych jest niedostępna z powodu zdarzenia w regionie, w którym hostowana jest baza danych. Jeśli na dużą skalę zdarzenia w regionie skutkuje brakiem dostępu aplikacji bazy danych, można przywrócić bazę danych z replikacją geograficzną kopii zapasowych na serwerze w każdym innym regionie. Występuje opóźnienie między po różnicowej kopii zapasowej i jego replikacją geograficzną na platformie Azure blob w innym regionie. To opóźnienie może trwać do godziny, więc w przypadku poważnej awarii, może wystąpić do utraty danych jedną godzinę. Poniższa ilustracja przedstawia przywracania bazy danych z ostatnią dostępną kopią zapasową w innym regionie.

![Funkcja przywracania geograficznego](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Aby uzyskać przykładowy skrypt programu PowerShell przedstawiająca sposób wykonać przywracanie geograficzne, zobacz [przywrócić bazę danych SQL przy użyciu programu PowerShell](scripts/sql-database-restore-database-powershell.md).

W momencie przywracania na pomocnicza geograficzna nie jest obecnie obsługiwane. Przywracanie do punktu w czasie jest możliwe tylko w głównej bazie danych. Aby uzyskać szczegółowe informacje na temat przy użyciu przywracania geograficznego odzyskiwania sprawności po awarii, zobacz [odzyskiwanie sprawności po awarii](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Odzyskiwanie z kopii zapasowych jest najbardziej podstawowym dostępne w bazie danych SQL z najdłuższym cel punktu odzyskiwania (RPO) i szacowany czas odzyskiwania (ERT) rozwiązania odzyskiwania po awarii. W przypadku rozwiązań przy użyciu mały rozmiar baz danych, (np. podstawowej usługi do warstwy lub mały rozmiar baz danych w elastycznej puli dzierżaw) funkcja przywracania geograficznego jest często rozsądne rozwiązanie odzyskiwania po awarii przy użyciu ERT 12 godzin. W przypadku rozwiązań przy użyciu dużych baz danych i wymagają krótszy odzyskiwania razy, należy rozważyć użycie [trybu Failover grupy i aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md). Aktywna replikacja geograficzna oferuje znacznie niższe. cel punktu odzyskiwania i ERT, jak wymaga jedynie można zainicjować trybu failover do pomocniczej stale replikowane. Aby uzyskać więcej informacji na temat opcji ciągłości biznesowej, zobacz [Przegląd ciągłości](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Funkcja przywracania geograficznego w witrynie Azure portal

Do przywracania geograficznego w bazie danych podczas jego [okres przechowywania modelu opartego na jednostkach DTU](sql-database-service-tiers-dtu.md) lub [okres przechowywania model oparty na rdzeniach wirtualnych](sql-database-service-tiers-vcore.md) przy użyciu witryny Azure portal, otwórz stronę bazy danych SQL, a następnie kliknij przycisk **Dodaj** . W **wybierz źródło** pola tekstowego, wybierz **kopii zapasowej**. Określ kopię zapasową, z którego można przeprowadzić operację odzyskiwania, w regionie, jak i na serwerze wybranym.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programowe odzyskiwanie systemu za pomocą automatycznych kopii zapasowych

Jak już wspomniano, oprócz witryny Azure portal odzyskiwanie bazy danych można wykonać programowo przy użyciu programu Azure PowerShell lub interfejsu API REST. W poniższych tabelach opisano zestaw poleceń dostępnych.

### <a name="powershell"></a>PowerShell

| Polecenie cmdlet | Opis |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Pobiera co najmniej jedną bazę danych. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Pobiera usuniętą bazę danych, którą można przywrócić. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Pobiera geograficznie nadmiarową kopię zapasową bazy danych. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Przywraca bazę danych SQL. |
|  | |

### <a name="rest-api"></a>Interfejs API REST

| Interfejs API | Opis |
| --- | --- |
| [REST (createMode = odzyskiwanie)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Przywraca bazę danych |
| [Pobierz Utwórz lub zaktualizuj stan bazy danych](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Zwraca stan podczas operacji przywracania |
|  | |

## <a name="summary"></a>Podsumowanie

Automatyczne kopie zapasowe chronić baz danych użytkownika i błędy aplikacji, usuwać przypadkowym bazy danych, lub długotrwały przestojów. Ta wbudowana funkcja jest dostępna dla wszystkich warstw usług i rozmiarów wystąpień obliczeniowych.

## <a name="next-steps"></a>Kolejne kroki

- Omówienie ciągłości działania i scenariuszach można znaleźć [omówienie ciągłości działania](sql-database-business-continuity.md).
- Aby dowiedzieć się więcej na temat usługi Azure SQL Database automatyczne kopie zapasowe, zobacz [bazy danych SQL, automatyczne kopie zapasowe](sql-database-automated-backups.md).
- Aby dowiedzieć się więcej o długoterminowym przechowywaniu, zobacz [długoterminowego przechowywania](sql-database-long-term-retention.md).
- Aby dowiedzieć się o szybsze opcje odzyskiwania, zobacz [trybu Failover grupy i aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md).  
