---
title: MySQL hostowania serwerów w usłudze Azure Stack | Dokumentacja firmy Microsoft
description: Jak dodać wystąpienia MySQL aprowizacji za pośrednictwem dostawcy zasobów MySQL karty
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 833d8e7960bfb7ee3c135df57e6d4dfec97af037
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364670"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>Dodawanie serwerów hostingu dla dostawcy zasobów bazy danych MySQL

Możesz hostować wystąpienia programu MySQL na maszynie wirtualnej (VM) w [usługi Azure Stack](azure-stack-poc.md), lub na maszynie Wirtualnej poza środowiskiem usługi Azure Stack, tak długo, jak dostawcy zasobów bazy danych MySQL można połączyć się z wystąpieniem.

> [!NOTE]
> Należy utworzyć bazy danych MySQL na serwerze dostawcy zasobów MySQL. Dostawcy zasobów MySQL powinny zostać utworzone w domyślnej subskrypcji dostawcy, a serwerami hostingu MySQL powinny zostać utworzone w płatnych, subskrypcja użytkownika. Serwer dostawcy zasobów nie powinny służyć do baz danych użytkownika hosta.

MySQL w wersji 5.6, 5.7 i 8.0 mogą być używane do hostowania serwerów. MySQL RP nie obsługuje uwierzytelniania caching_sha2_password; które zostaną dodane w następnej wersji. Serwery MySQL 8.0 musi być skonfigurowana do używania mysql_native_password. Obsługiwane jest również MariaDB.

## <a name="connect-to-a-mysql-hosting-server"></a>Łączenie z serwerem hostingu MySQL

Upewnij się, że masz poświadczenia dla konta z uprawnieniami administratora systemu. Aby dodać serwer hostingu, wykonaj następujące kroki:

1. Zaloguj się do portalu usługi Azure Stack operatora jako administratora usługi.
2. Wybierz pozycję **Wszystkie usługi**.
3. W obszarze **zasoby administracyjne** wybierz kategorię **serwerów do hostingu MySQL** > **+ Dodaj**. Spowoduje to otwarcie **dodawania serwera hostingu MySQL** okno dialogowe, co pokazano na poniższym zrzucie ekranu.

   ![Konfigurowanie serwera hostingu](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. Podaj szczegóły połączenia wystąpienia serwera MySQL.

   * Aby uzyskać **nazwę serwera obsługującego MySQL**, podaj w pełni kwalifikowana nazwa domeny (FQDN) lub prawidłowym adresem IPv4. Nie używaj krótką nazwę maszyny Wirtualnej.
   * Domyślne wystąpienie MySQL nie podano, więc należy określić **rozmiar hostingu serwera w GB**. Wprowadź rozmiar, który znajduje się w pobliżu pojemności serwera bazy danych.
   * Zachowaj domyślne ustawienie dla **subskrypcji**.
   * Aby uzyskać **grupy zasobów**, Utwórz nową lub użyj istniejącej grupy.

   > [!NOTE]
   > Jeśli wystąpienie programu MySQL można uzyskać dostępu do dzierżawy i administrator usługi Azure Resource Manager, możesz ją umieścić pod kontrolą dostawcy zasobów. Jednak wystąpienie MySQL **musi** przydzielenia wyłącznie na potrzeby dostawcy zasobów.

5. Wybierz **jednostki SKU** otworzyć **Tworzenie jednostki SKU** okna dialogowego.

   ![Tworzenie jednostki SKU MySQL](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   Jednostka SKU **nazwa** powinny odzwierciedlać właściwości jednostki SKU, dzięki czemu użytkownicy mogą wdrażać swoje bazy danych do odpowiedniej jednostki SKU.

6. Wybierz **OK** do utworzenia jednostki SKU.
> [!NOTE]
> Jednostki SKU może potrwać do godziny mają być wyświetlane w portalu. Nie można utworzyć bazę danych, do momentu jednostka SKU jest wdrożona i uruchomiona.

7. W obszarze **dodawania serwera hostingu MySQL**, wybierz opcję **Utwórz**.

Podczas dodawania serwerów, należy je przypisać do nowej lub istniejącej jednostki SKU do odróżnienia ofert usług. Na przykład można mieć wystąpienia enterprise MySQL, który zapewnia zwiększoną bazy danych i automatyczne kopie zapasowe. Możesz zarezerwować tego serwera o wysokiej wydajności dla różnych działów w organizacji.

## <a name="security-considerations-for-mysql"></a>Zagadnienia dotyczące zabezpieczeń dla MySQL

Poniższe informacje dotyczą do planu odzyskiwania i hostowania serwerów MySQL:

* Upewnij się, że wszystkie serwery hostingu są skonfigurowane do komunikacji przy użyciu protokołu TLS 1.2. Zobacz [Konfigurowanie MySQL, aby użyć połączenia szyfrowane](https://dev.mysql.com/doc/refman/5.7/en/using-encrypted-connections.html).
* Zastosowana [Transparent Data Encryption](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-data-encryption.html).
* MySQL RP nie obsługuje uwierzytelniania caching_sha2_password.

## <a name="increase-backend-database-capacity"></a>Zwiększenie wydajności bazy danych zaplecza

Przez wdrożenie kolejnych serwerów MySQL w portalu Azure Stack, możesz zwiększyć pojemności bazy danych zaplecza. Dodaj te serwery do nowej lub istniejącej jednostki SKU. Po dodaniu serwera do istniejących jednostek SKU, upewnij się, że właściwości serwera są takie same jak innych serwerów w ramach jednostki SKU.

## <a name="sku-notes"></a>Informacje o jednostce SKU
Użyj nazwy jednostek SKU, która zawiera opis możliwości serwerów w ramach jednostki SKU, takich jak pojemność i wydajność. Nazwa służy jako pomocy, aby ułatwić użytkownikom wdrożyć swoje bazy danych do odpowiedniej jednostki SKU. Na przykład służy nazwy jednostki SKU do odróżnienia oferty usługi przez następujące cechy:
  
* Wysoka pojemność
* o wysokiej wydajności
* Wysoka dostępność

Najlepszym rozwiązaniem jest serwerami hostingu w jednostce SKU powinny mieć takie same charakterystyki zasobów i wydajności.

Nie można przypisać jednostki SKU do określonych użytkowników lub grup.

Jednostki SKU może potrwać do godziny mają być wyświetlane w portalu. Użytkownicy nie mogą tworzyć bazy danych, dopóki nie jest w pełni utworzona jednostka SKU.

Aby edytować jednostki SKU, przejdź do **wszystkich usług** > **karty MySQL** > **jednostki SKU**. Wybierz jednostkę SKU, aby zmodyfikować, wprowadź niezbędne zmiany i kliknij przycisk **Zapisz** można zapisać zmian. Aby usunąć jednostkę SKU, która nie jest już potrzebny, przejdź do **wszystkich usług** > **karty MySQL** > **jednostki SKU**. Kliknij prawym przyciskiem myszy nazwę jednostki SKU, a następnie wybierz pozycję **Usuń** go usunąć.

> [!TIP]
> Można edytować lub usunąć limity przydziałów dostawcy zasobów MySQL w tej samej lokalizacji.

## <a name="make-mysql-database-servers-available-to-your-users"></a>Udostępnić użytkownikom serwery bazy danych MySQL

Utwórz plany i oferty, aby udostępnić użytkownikom serwery bazy danych MySQL. Dodaj usługę Microsoft.MySqlAdapter zgodnie z planem i tworzenie nowego limitu przydziału. MySQL nie zezwala na ograniczenie rozmiaru bazy danych.

## <a name="next-steps"></a>Kolejne kroki

[Utwórz bazę danych MySQL](azure-stack-mysql-resource-provider-databases.md)