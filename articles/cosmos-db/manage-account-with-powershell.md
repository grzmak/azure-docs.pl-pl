---
title: Automatyzacja usługi Azure Cosmos DB — zarządzanie za pomocą programu Powershell | Dokumentacja firmy Microsoft
description: Użyciu programu Azure Powershell zarządzania kontami usługi Azure Cosmos DB.
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 04/21/2017
ms.author: sngun
ms.openlocfilehash: 82ab30ebab1b69d5ae636702b3b56d3792c09010
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394577"
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>Tworzenie konta usługi Azure Cosmos DB przy użyciu programu PowerShell

Następujący przewodnik zawiera opis polecenia do automatyzacji zarządzania konta bazy danych Azure Cosmos DB przy użyciu programu Azure Powershell. Obejmuje również polecenia do zarządzania kluczami konta i priorytetów trybu failover w [multiregionalne konta baz danych] [dystrybucji data-globally.md]. Aktualizowanie konta bazy danych pozwala na modyfikowanie zasad spójności i dodawanie/usuwanie regionów. Dla wielu platform zarządzania kontem usługi Azure Cosmos DB, możesz użyć [wiersza polecenia platformy Azure](cli-samples.md), [interfejsu API REST dostawcy zasobów][rp-rest-api], lub [witryny Azure portal ](create-sql-api-dotnet.md#create-account).

## <a name="getting-started"></a>Wprowadzenie

Postępuj zgodnie z instrukcjami w [jak zainstalować i skonfigurować program Azure PowerShell] [ powershell-install-configure] do zainstalowania i zaloguj się do swojego konta usługi Azure Resource Manager w programie Powershell.

### <a name="notes"></a>Uwagi

* Jeśli chcesz, wykonaj następujące polecenia, bez wymagania potwierdzenia użytkownika, dołącz `-Force` flagi do polecenia.
* Następujące polecenia są synchroniczne.

## <a id="create-documentdb-account-powershell"></a> Tworzenie konta usługi Azure Cosmos DB

To polecenie umożliwia tworzenie konta bazy danych usługi Azure Cosmos DB. Konfigurowanie nowego konta bazy danych jako jednym regionie lub [multiregionalne] [dystrybucji data-globally.md] niektóre [zasad spójności](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` Nazwa lokalizacji region zapisu konta bazy danych. Ta lokalizacja musi mieć wartość 0 priorytet trybu failover. Musi istnieć dokładnie jeden region zapisu na konto bazy danych.
* `<read-region-location>` Nazwa lokalizacji odczytu z regionu odczytu konta bazy danych. Ta lokalizacja musi mieć wartość priorytetu trybu failover w większą niż 0. Może istnieć więcej niż jeden regionów odczytu dla konta bazy danych.
* `<ip-range-filter>` Określa zbiór adresów IP lub zakres adresów IP w formacie CIDR, które będą uwzględniane jako lista dozwolonych adresów IP klienta dla danego konta bazy danych. Zakresów adresów IP musi być z wartościami rozdzielanymi przecinkami i nie może zawierać spacji. Aby uzyskać więcej informacji, zobacz [pomocy technicznej systemu Azure Cosmos DB zapory](firewall-support.md)
* `<default-consistency-level>` Domyślny poziom spójności usługi Azure Cosmos DB. Aby uzyskać więcej informacji, zobacz [poziomy spójności w usłudze Azure Cosmos DB](consistency-levels.md).
* `<max-interval>` W przypadku użycia spójności powiązana nieaktualność, ta wartość przedstawia czas ilość nieaktualność (w sekundach) tolerowane. Zaakceptowane dla tej wartości to 1-100.
* `<max-staleness-prefix>` Używane z spójności powiązana nieaktualność, ta wartość reprezentuje liczbę żądań starych tolerowane. Zaakceptowane dla tej wartości to 1 – 2 147 483 647.
* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<resource-group-location>` Lokalizacja grupy zasobów platformy Azure, do której należy nowe konto bazy danych Azure Cosmos DB.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB, który ma zostać utworzony. Można używać go tylko małe litery, cyfry, "-" znaków i musi należeć do zakresu od 3 do 50 znaków.

Przykład: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Uwagi
* Poprzedni przykład tworzy konto bazy danych za pomocą dwóch regionach. Użytkownik może również utworzyć konto bazy danych z jednego regionu, (który jest wyznaczony jako region zapisu i mają wartość priorytetu trybu failover, 0) lub więcej niż dwóch regionach. Aby uzyskać więcej informacji, zobacz [multiregionalne konta baz danych] [dystrybucji data-globally.md].
* Lokalizacje musi być regionów, w których usługa Azure Cosmos DB jest ogólnie dostępna. Bieżącą listę regionów znajduje się na [regionów platformy Azure, strona](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a> Aktualizacja konta bazy danych usługi Azure Cosmos DB

To polecenie umożliwia aktualizowanie właściwości konta bazy danych Azure Cosmos DB. Obejmuje to zasady zgodności i lokalizacje, w których istnieje konto bazy danych w.

> [!NOTE]
> To polecenie umożliwia dodawanie i usuwanie regionów, ale nie pozwala na modyfikowanie priorytetów trybu failover. Aby zmodyfikować priorytetów trybu failover, zobacz [poniżej](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` Nazwa lokalizacji region zapisu konta bazy danych. Ta lokalizacja musi mieć wartość 0 priorytet trybu failover. Musi istnieć dokładnie jeden region zapisu na konto bazy danych.
* `<read-region-location>` Nazwa lokalizacji odczytu z regionu odczytu konta bazy danych. Ta lokalizacja musi mieć wartość priorytetu trybu failover w większą niż 0. Może istnieć więcej niż jeden regionów odczytu dla konta bazy danych.
* `<default-consistency-level>` Domyślny poziom spójności usługi Azure Cosmos DB. Aby uzyskać więcej informacji, zobacz [poziomy spójności w usłudze Azure Cosmos DB](consistency-levels.md).
* `<ip-range-filter>` Określa zbiór adresów IP lub zakres adresów IP w formacie CIDR, które będą uwzględniane jako lista dozwolonych adresów IP klienta dla danego konta bazy danych. Zakresów adresów IP musi być z wartościami rozdzielanymi przecinkami i nie może zawierać spacji. Aby uzyskać więcej informacji, zobacz [pomocy technicznej systemu Azure Cosmos DB zapory](firewall-support.md)
* `<max-interval>` W przypadku użycia spójności powiązana nieaktualność, ta wartość przedstawia czas ilość nieaktualność (w sekundach) tolerowane. Zaakceptowane dla tej wartości to 1-100.
* `<max-staleness-prefix>` Używane z spójności powiązana nieaktualność, ta wartość reprezentuje liczbę żądań starych tolerowane. Zaakceptowane dla tej wartości to 1 – 2 147 483 647.
* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<resource-group-location>` Lokalizacja grupy zasobów platformy Azure, do której należy nowe konto bazy danych Azure Cosmos DB.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB do zaktualizowania.

Przykład: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a> Usuwanie konta bazy danych usługi Azure Cosmos DB

To polecenie umożliwia usunięcie istniejącego konta bazy danych Azure Cosmos DB.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB do usunięcia.

Przykład:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a> Uzyskiwanie właściwości konta bazy danych usługi Azure Cosmos DB

To polecenie umożliwia pobieranie właściwości istniejącego konta bazy danych Azure Cosmos DB.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB.

Przykład:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a> Aktualizacji tagów konta bazy danych usługi Azure Cosmos DB

Na poniższym przykładzie opisano sposób ustawiania [tagów zasobów platformy Azure] [ azure-resource-tags] dla usługi Azure Cosmos DB konto bazy danych.

> [!NOTE]
> To polecenie może być łączone z polecenia tworzenia lub aktualizacji, dodając `-Tags` flagę odpowiadającego mu parametru.

Przykład:

    $tags = @{"dept" = "Finance"; environment = "Production"}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDB/databaseAccounts"  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a> Wyświetl listę kluczy konta

Podczas tworzenia konta usługi Azure Cosmos DB, Usługa generuje dwa klucze wzorca dostępu, których można użyć do uwierzytelniania podczas uzyskiwania dostępu do konta usługi Azure Cosmos DB. Zapewniając dwa klucze dostępu, usługi Azure Cosmos DB pozwala na ponowne generowanie kluczy nie zakłóceń z kontem usługi Azure Cosmos DB. Dostępne są również klucze tylko do odczytu w celu uwierzytelniania operacji tylko do odczytu. Istnieją dwa klucze odczytu i zapisu (podstawowych i pomocniczych) i dwa klucze tylko do odczytu (podstawowych i pomocniczych).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB.

Przykład:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a> Lista parametrów połączenia

W przypadku kont bazy danych MongoDB przy użyciu następującego polecenia można pobrać parametry połączenia, które umożliwiają połączenie aplikacji bazy danych MongoDB z kontem bazy danych.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB.

Przykład:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a> Ponowne generowanie klucza konta

Należy zmieniać klucze dostępu do swojego konta usługi Azure Cosmos DB, okresowo, aby zabezpieczyć połączenia. Dwa klucze dostępu są przypisywane umożliwia utrzymanie połączeń do konta usługi Azure Cosmos DB przy użyciu jednego klucza dostępu, jednocześnie ponownie generując drugi klucz dostępu.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB.
* `<key-kind>` Jeden z czterech typów kluczy: ["Primary" | " Pomocniczy "|" PrimaryReadonly "|" SecondaryReadonly"], który chcesz ponownie wygenerować.

Przykład:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a> Modyfikuj pracy awaryjnej priorytet konta bazy danych usługi Azure Cosmos DB

Dla multiregionalne konta baz danych możesz zmienić priorytet trybu failover w różnych regionach, w których istnieje konto bazy danych Azure Cosmos DB w. Aby uzyskać więcej informacji na temat trybu failover w ramach Twojego konta bazy danych Azure Cosmos DB, zobacz [globalna dystrybucja danych za pomocą usługi Azure Cosmos DB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>` Nazwa lokalizacji region zapisu konta bazy danych. Ta lokalizacja musi mieć wartość 0 priorytet trybu failover. Musi istnieć dokładnie jeden region zapisu na konto bazy danych.
* `<read-region-location>` Nazwa lokalizacji odczytu z regionu odczytu konta bazy danych. Ta lokalizacja musi mieć wartość priorytetu trybu failover w większą niż 0. Może istnieć więcej niż jeden regionów odczytu dla konta bazy danych.
* `<resource-group-name>` Nazwa [grupy zasobów platformy Azure] [ azure-resource-groups] do której należy do nowego konta usługi Azure Cosmos DB w bazie danych.
* `<database-account-name>` Nazwa konta bazy danych Azure Cosmos DB.

Przykład:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Kolejne kroki

* Nawiązywanie połączenia przy użyciu platformy .NET, zobacz [nawiązywanie połączenia i odpytywanie za pomocą platformy .NET](create-sql-api-dotnet.md).
* Nawiązywanie połączenia przy użyciu środowiska Node.js, zobacz [nawiązywanie połączenia i odpytywanie za pomocą środowiska Node.js i aplikację MongoDB](create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
