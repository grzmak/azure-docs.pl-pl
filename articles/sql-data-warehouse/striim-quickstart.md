---
title: Szybki start Striim za pomocą usługi Azure SQL Data Warehouse | Dokumentacja firmy Microsoft
description: Szybko Rozpocznij pracę z Striim i Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: 09f3126725b9a2cd6a8277447c0eed4a16838429
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355516"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Portal SQL magazynu danych Striim Azure Marketplace oferuje Przewodnik instalacji

Ten przewodnik Szybki Start założono, że masz już istniejące wystąpienie usługi SQL Data Warehouse.

Wyszukaj Striim w witrynie Azure Marketplace, a następnie wybierz Striim integracji danych, do opcji/SQL Data Warehouse (przejściowa) 

![Zainstaluj Striim][install]

Konfigurowanie maszyny Wirtualnej Striim przy użyciu określonych właściwości, zanotowaną Striim nazwę klastra, hasła i hasło administratora

![Konfigurowanie Striim][configure]

Po wdrożeniu, kliknij pozycję <VM Name>masternode — w witrynie Azure portal kliknij pozycję Połącz, a następnie skopiuj logowania przy użyciu lokalnego konta maszyny Wirtualnej 

![Połącz Striim do usługi SQL Data Warehouse][connect]

Pobierz sqljdbc42.jar z <https://www.microsoft.com/en-us/download/details.aspx?id=54671> na komputer lokalny. 

Otwórz okno wiersza polecenia i zmień katalogi, do którego został pobrany plik jar JDBC. Punkt połączenia usługi plik jar z maszyną wirtualną Striim, uzyskiwanie adres i hasło z portalu Azure

![Skopiuj plik jar z maszyną wirtualną][copy-jar]

Otwiera inne okno wiersza polecenia lub użyj ssh narzędzia ssh do Striim klastra

![SSH do klastra][ssh]

Wykonaj następujące polecenia, aby przenieść plik jar JDBC do katalogu lib firmy Striim i uruchamianie i zatrzymywanie serwera.

   1. "sudo" su
   2. TMP dysku CD
   3. mV sqljdbc42.jar /opt/striim/lib
   4. węzłami striim systemctl stop
   5. danych striim stop systemctl-dbms
   6. danych striim start systemctl-dbms
   7. węzeł striim systemctl początkowy

![Uruchomienie klastra Striim][start-striim]

Teraz Otwórz ulubioną przeglądarkę i przejdź do <DNS Name>: 9080

![Przejdź do ekranu logowania][navigate]

Zaloguj się przy użyciu nazwy użytkownika i hasła, możesz skonfigurować w witrynie Azure portal i wybierz swoje preferowane kreatora, aby rozpocząć pracę, lub przejdź na stronę aplikacji, aby rozpocząć korzystanie z przeciągania i upuszczania interfejsu użytkownika

![Zaloguj się przy użyciu poświadczeń serwera][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png