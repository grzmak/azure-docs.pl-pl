---
title: Azure Database dla reguły zapory serwera MariaDB
description: W tym artykule opisano reguły zapory dla usługi Azure Database dla serwera MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 930e561c1777ccfcb8046c824853ebb12367cee5
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248104"
---
# <a name="azure-database-for-mariadb-server-firewall-rules"></a>Azure Database dla reguły zapory serwera MariaDB
Zapora uniemożliwia wszelki dostęp do serwera bazy danych, do momentu określenia komputerów, które mają uprawnienia. Zapora udziela dostępu do serwera, na podstawie źródłowego adresu IP każdego żądania.

Aby skonfigurować zaporę, należy utworzyć reguły zapory określające zakresy dopuszczalnych adresów IP. Można utworzyć reguły zapory na poziomie serwera.

**Reguły zapory:** te reguły umożliwiają klientom dostęp do całej bazy danych Azure dla serwera MariaDB, oznacza to, że wszystkie bazy danych na tym samym serwerze logicznym. Reguły zapory na poziomie serwera można skonfigurować przy użyciu witryny Azure portal lub poleceń interfejsu wiersza polecenia platformy Azure. Aby utworzyć reguły zapory na poziomie serwera, musi być właścicielem bądź współautorem subskrypcji.

## <a name="firewall-overview"></a>Omówienie zapory
Wszystkie bazy danych jest dostęp do usługi Azure Database dla serwera MariaDB domyślnie blokowany przez zaporę. Aby rozpocząć korzystanie z serwera z innego komputera, należy określić co najmniej jedną regułę zapory na poziomie serwera, aby umożliwić dostęp do serwera. Reguły zapory, aby określić, które IP zakresów adresów z Internetu, aby zezwolić na użycie. Dostęp do witryny portalu Azure, sama nie ma wpływu reguły zapory.

Próby nawiązania połączenia z Internetem a platformą Azure muszą najpierw przejść przez zaporę zanim dotrą do usługi Azure Database dla bazy danych MariaDB, jak pokazano na poniższym diagramie:

![Przykładowy przepływ działania zapory](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Łączenie się z Internetu
Reguły zapory na poziomie serwera mają zastosowanie do wszystkich baz danych w usłudze Azure Database dla serwera MariaDB.

Jeśli adres IP żądania do jednego z zakresów określonych w regułach zapory na poziomie serwera, połączenie zostanie ustanowione.

Jeśli adres IP żądania znajduje się poza zakresów określonych w dowolnej reguły zapory na poziomie serwera lub na poziomie bazy danych, żądanie połączenia kończy się niepowodzeniem.

## <a name="connecting-from-azure"></a>Łączenie z platformy Azure
Aby umożliwić aplikacjom z platformy Azure, nawiązać połączenia z usługi Azure Database dla serwera MariaDB, należy włączyć połączenia platformy Azure. Na przykład, aby hostować aplikację usługi Azure Web Apps lub aplikację, która działa na Maszynie wirtualnej platformy Azure lub do nawiązywania połączenia bramy zarządzania danymi usługi Azure Data Factory. Zasoby nie muszą znajdować się w tej samej sieci wirtualnej (VNet) lub grupy zasobów dla reguły zapory, aby włączyć te połączenia. Gdy aplikacja platformy Azure próbuje połączyć się z serwerem bazy danych, zapora sprawdza, czy połączenia platformy Azure są dozwolone. Istnieje kilka metod, aby włączyć te typy połączeń. Ustawienie zapory z początkowym i końcowym adresem równym 0.0.0.0 wskazuje, że te połączenia są dozwolone. Alternatywnie, można ustawić **zezwolić na dostęp do usług platformy Azure** opcję **ON** w portalu pochodzące ze **zabezpieczenia połączeń** okienka, a następnie wybierz pozycję **Zapisz**. Jeśli próba połączenia nie jest dozwolona, żądanie nie dociera usługi Azure Database dla serwera MariaDB.

> [!IMPORTANT]
> Ta opcja konfiguruje zaporę w celu zezwalania na wszystkie połączenia z platformy Azure, w tym połączenia z subskrypcji innych klientów. W przypadku wybrania tej opcji upewnij się, że uprawnienia logowania i użytkownika zezwalają na dostęp tylko uprawnionym użytkownikom.
> 

![Konfigurowanie Zezwalaj na dostęp do usług platformy Azure w portalu](./media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Programowe zarządzanie regułami zapory
Oprócz witryny Azure portal reguły zapory można zarządzać programowo przy użyciu wiersza polecenia platformy Azure. 

<!--See also [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)-->

## <a name="troubleshooting-the-database-firewall"></a>Rozwiązywanie problemów z zaporą bazy danych
Podczas dostępu do usługi Microsoft Azure Database dla usługi serwera MariaDB nie zachowywać się zgodnie z oczekiwaniami, należy rozważyć następujące kwestie:

* **Zmiany do listy dozwolonych nie zostały uwzględnione jeszcze:** może być jak opóźnienia 5 minut, zanim zmiany do usługi Azure Database dla serwera MariaDB zapory konfiguracji zostały wprowadzone.

* **Nazwa logowania nie ma autoryzacji lub użyto nieprawidłowego hasła:** Jeśli nazwy logowania nie ma uprawnień w usłudze Azure Database dla serwera MariaDB lub użyte hasło jest nieprawidłowe, nastąpi odmowa połączenia do usługi Azure Database dla serwera MariaDB. Utworzenie ustawień zapory zapewnia klientom jedynie możliwość próby nawiązania połączenia z serwerem, ale każdy klient musi podać niezbędne poświadczenia zabezpieczeń.

* **Dynamiczny adres IP:** Jeśli masz połączenie internetowe za pomocą dynamicznego adresowania IP i problemy z przejściem przez zaporę, możesz wypróbować jedną z następujących rozwiązań:

* Poproś usługodawcy internetowego (ISP) dla zakresu adresów IP, które są przypisane do komputerów klienckich uzyskujących dostęp do usługi Azure Database dla serwera MariaDB, a następnie dodać zakres adresów IP jako reguły zapory.

* Pobierz statyczne adresy IP dla komputerów klienckich, a następnie dodaj te adresy IP jako reguły zapory.

## <a name="next-steps"></a>Kolejne kroki
- [Tworzenie i zarządzanie nimi — Azure Database dla MariaDB reguł zapory przy użyciu witryny Azure portal](./howto-manage-firewall-portal.md)

<!--
- [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md) -->
