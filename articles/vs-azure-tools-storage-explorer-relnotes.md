---
title: Informacje o wersji programu Microsoft Azure Storage Explorer
description: Informacje o wersji programu Microsoft Azure Storage Explorer
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: ''
ms.assetid: ''
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2018
ms.author: cawa
ms.openlocfilehash: 708b80787337d549ebc5e66bca21e734620616ac
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388302"
---
# <a name="microsoft-azure-storage-explorer-release-notes"></a>Informacje o wersji programu Microsoft Azure Storage Explorer

Ten artykuł zawiera informacje o wersji programu Azure Storage Explorer 1.4.3 wersji, a także informacje o wersji dla wcześniejszych wersji.

[Microsoft Azure Storage Explorer](./vs-azure-tools-storage-manage-with-storage-explorer.md) jest aplikacją autonomiczną, która umożliwia łatwą obsługę danych w usłudze Azure Storage w Windows, macOS i Linux.

## <a name="version-144"></a>Wersja 1.4.4
10/15/2018 r.

### <a name="download-azure-storage-explorer-144"></a>Pobierz bezpłatnie Eksplorator magazynu Azure 1.4.4
- [Usługa Azure Storage Explorer 1.4.4 dla Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Eksplorator usługi Azure Storage 1.4.4 dla komputerów Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Eksplorator usługi Azure Storage 1.4.4 dla systemu Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="hotfixes"></a>Poprawki
* Wersja interfejsu Api Azure Resource Management została wycofana odblokowania użytkowników dla administracji USA. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Trwa ładowanie pokręteł teraz używają animacji CSS skrócenie GPU wykorzystywane przez Eksploratora magazynu. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Nowa
* Załączniki zewnętrznego zasobu, takie jak w przypadku połączenia SAS i emulatorów, została znacznie ulepszona. Teraz możesz wykonywać następujące czynności:
   * Dostosuj nazwę wyświetlaną zasób, który jest podłączany. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Dołącz do wielu emulatorów lokalnego przy użyciu różnych portów. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Dodaj zasoby dołączone do paska szybki dostęp. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Eksplorator usługi Storage obsługuje teraz usuwanie nietrwałe. Możesz:
   * Skonfiguruj zasady usuwanie nietrwałe, klikając prawym przyciskiem myszy w węźle kontenery obiektów Blob konta magazynu.
   * Widok nietrwale usunięte obiekty BLOB w Edytorze obiektów Blob, wybierając pozycję "Active i usunięte obiekty BLOB" w menu rozwijanym obok paska nawigacji.
   * Usuniętych nietrwale usunięte obiekty BLOB.

### <a name="fixes"></a>Poprawki
* Akcja "Konfiguruj ustawienia specyfikacji CORS" nie jest już dostępna dla kont usługi Premium Storage ponieważ kont usługi Premium Storage nie obsługują mechanizmu CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Teraz jest właściwością sygnatura dostępu współdzielonego dla usługi z dołączoną sygnatury dostępu Współdzielonego. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* Akcja "Ustaw domyślna Warstwa dostępu" jest teraz dostępne dla obiektów Blob i magazynu GPV2 konta, które zostały przypięte do paska szybki dostęp. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Czasami Eksploratora usługi Storage zakończy się niepowodzeniem pokazać klasycznych kont magazynu. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Znane problemy
* Przy użyciu emulatorów, takich jak Emulator usługi Azure Storage lub Azurite, należy je nasłuchiwać połączeń na ich domyślne porty. W przeciwnym razie Eksploratora usługi Storage nie będzie można połączyć się z nimi.
* Jeśli używasz programu VS dla komputerów Mac i nigdy nie zostały utworzone niestandardowej konfiguracji usługi AAD, możesz nie mieć możliwości logowania. Aby obejść ten problem, usuń zawartość ~ /. IdentityService/AadConfigurations. Jeśli to nie niedogodność, należy dodać komentarz dotyczący [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite nie została jeszcze w pełni zaimplementowana wszystkie interfejsy API usługi Storage. W związku z tym może występować nieoczekiwanych błędów lub zachowanie w przypadku używania Azurite dla magazynem projektowym.
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przekazywanie z folderu usługi OneDrive nie działa z powodu błędów w środowisku NodeJS. Błąd został rozwiązany, ale nie są jeszcze zintegrowane z elektronów.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="previous-releases"></a>Poprzednie wersje

* [Wersja 1.4.3](#version-143)
* [W wersji 1.4.2](#version-142)
* [Wersja 1.4.1](#version-141)
* [Wersja 1.3.0](#version-130)
* [Wersji 1.2.0 lub nowszej](#version-120)
* [Wersji 1.1.0](#version-110)
* [Wersja 1.0.0](#version-100)
* [Wersja 0.9.6](#version-096)
* [Wersja 0.9.5](#version-095)
* [Wersja 0.9.4 i 0.9.3](#version-094-and-093)
* [Wersja 0.9.2](#version-092)
* [Wersja 0.9.1 i 0.9.0](#version-091-and-090)
* [Wersja 0.8.16](#version-0816)
* [Wersja 0.8.14](#version-0814)
* [Wersja 0.8.13](#version-0813)
* [Wersja 0.8.12 i 0.8.11 i 0.8.10](#version-0812-and-0811-and-0810)
* [Wersja 0.8.9 i 0.8.8](#version-089-and-088)
* [Wersja 0.8.7](#version-087)
* [Wersja 0.8.6](#version-086)
* [Wersja 0.8.5](#version-085)
* [Wersja 0.8.4](#version-084)
* [Wersja 0.8.3](#version-083)
* [Wersja 0.8.2](#version-082)
* [Wersja 0.8.0](#version-080)
* [Wersja 0.7.20160509.0](#version-07201605090)
* [Wersja 0.7.20160325.0](#version-07201603250)
* [Wersja 0.7.20160129.1](#version-07201601291)
* [Wersja 0.7.20160105.0](#version-07201601050)
* [Wersja 0.7.20151116.0](#version-07201511160)

## <a name="version-143"></a>Wersja 1.4.3
10/11/2018 r.

### <a name="hotfixes"></a>Poprawki
* Wersja interfejsu Api Azure Resource Management została wycofana odblokowania użytkowników dla administracji USA. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Trwa ładowanie pokręteł teraz używają animacji CSS skrócenie GPU wykorzystywane przez Eksploratora magazynu. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Nowa
* Załączniki zewnętrznego zasobu, takie jak w przypadku połączenia SAS i emulatorów, została znacznie ulepszona. Teraz możesz wykonywać następujące czynności:
   * Dostosuj nazwę wyświetlaną zasób, który jest podłączany. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Dołącz do wielu emulatorów lokalnego przy użyciu różnych portów. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Dodaj zasoby dołączone do paska szybki dostęp. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Eksplorator usługi Storage obsługuje teraz usuwanie nietrwałe. Możesz:
   * Skonfiguruj zasady usuwanie nietrwałe, klikając prawym przyciskiem myszy w węźle kontenery obiektów Blob konta magazynu.
   * Widok nietrwale usunięte obiekty BLOB w Edytorze obiektów Blob, wybierając pozycję "Active i usunięte obiekty BLOB" w menu rozwijanym obok paska nawigacji.
   * Usuniętych nietrwale usunięte obiekty BLOB.

### <a name="fixes"></a>Poprawki
* Akcja "Konfiguruj ustawienia specyfikacji CORS" nie jest już dostępna dla kont usługi Premium Storage ponieważ kont usługi Premium Storage nie obsługują mechanizmu CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Teraz jest właściwością sygnatura dostępu współdzielonego dla usługi z dołączoną sygnatury dostępu Współdzielonego. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* Akcja "Ustaw domyślna Warstwa dostępu" jest teraz dostępne dla obiektów Blob i magazynu GPV2 konta, które zostały przypięte do paska szybki dostęp. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Czasami Eksploratora usługi Storage zakończy się niepowodzeniem pokazać klasycznych kont magazynu. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Znane problemy
* Przy użyciu emulatorów, takich jak Emulator usługi Azure Storage lub Azurite, należy je nasłuchiwać połączeń na ich domyślne porty. W przeciwnym razie Eksploratora usługi Storage nie będzie można połączyć się z nimi.
* Jeśli używasz programu VS dla komputerów Mac i nigdy nie zostały utworzone niestandardowej konfiguracji usługi AAD, możesz nie mieć możliwości logowania. Aby obejść ten problem, usuń zawartość ~ /. IdentityService/AadConfigurations. Jeśli to nie niedogodność, należy dodać komentarz dotyczący [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite nie została jeszcze w pełni zaimplementowana wszystkie interfejsy API usługi Storage. W związku z tym może występować nieoczekiwanych błędów lub zachowanie w przypadku używania Azurite dla magazynem projektowym.
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przekazywanie z folderu usługi OneDrive nie działa z powodu błędów w środowisku NodeJS. Błąd został rozwiązany, ale nie są jeszcze zintegrowane z elektronów.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-142"></a>W wersji 1.4.2
09/24/2018 r.

### <a name="hotfixes"></a>Poprawki
* Aktualizowanie wersji interfejsu Api zarządzania zasobów platformy Azure na 2018-07-01, aby dodać obsługę nowego rodzaju konta usługi Azure Storage. [#652](https://github.com/Microsoft/AzureStorageExplorer/issues/652)

### <a name="new"></a>Nowa
* Załączniki zewnętrznego zasobu, takie jak w przypadku połączenia SAS i emulatorów, została znacznie ulepszona. Teraz możesz wykonywać następujące czynności:
   * Dostosuj nazwę wyświetlaną zasób, który jest podłączany. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Dołącz do wielu emulatorów lokalnego przy użyciu różnych portów. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Dodaj zasoby dołączone do paska szybki dostęp. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Eksplorator usługi Storage obsługuje teraz usuwanie nietrwałe. Możesz:
   * Skonfiguruj zasady usuwanie nietrwałe, klikając prawym przyciskiem myszy w węźle kontenery obiektów Blob konta magazynu.
   * Widok nietrwale usunięte obiekty BLOB w Edytorze obiektów Blob, wybierając pozycję "Active i usunięte obiekty BLOB" w menu rozwijanym obok paska nawigacji.
   * Usuniętych nietrwale usunięte obiekty BLOB.

### <a name="fixes"></a>Poprawki
* Akcja "Konfiguruj ustawienia specyfikacji CORS" nie jest już dostępna dla kont usługi Premium Storage ponieważ kont usługi Premium Storage nie obsługują mechanizmu CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Teraz jest właściwością sygnatura dostępu współdzielonego dla usługi z dołączoną sygnatury dostępu Współdzielonego. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* Akcja "Ustaw domyślna Warstwa dostępu" jest teraz dostępne dla obiektów Blob i magazynu GPV2 konta, które zostały przypięte do paska szybki dostęp. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Czasami Eksploratora usługi Storage zakończy się niepowodzeniem pokazać klasycznych kont magazynu. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Znane problemy
* Przy użyciu emulatorów, takich jak Emulator usługi Azure Storage lub Azurite, należy je nasłuchiwać połączeń na ich domyślne porty. W przeciwnym razie Eksploratora usługi Storage nie będzie można połączyć się z nimi.
* Jeśli używasz programu VS dla komputerów Mac i nigdy nie zostały utworzone niestandardowej konfiguracji usługi AAD, możesz nie mieć możliwości logowania. Aby obejść ten problem, usuń zawartość ~ /. IdentityService/AadConfigurations. Jeśli to nie niedogodność, należy dodać komentarz dotyczący [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite nie została jeszcze w pełni zaimplementowana wszystkie interfejsy API usługi Storage. W związku z tym może występować nieoczekiwanych błędów lub zachowanie w przypadku używania Azurite dla magazynem projektowym.
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przekazywanie z folderu usługi OneDrive nie działa z powodu błędów w środowisku NodeJS. Błąd został rozwiązany, ale nie są jeszcze zintegrowane z elektronów.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-141"></a>Wersja 1.4.1
08/28/2018 r.

### <a name="hotfixes"></a>Poprawki
* Przy pierwszym uruchomieniu programu Storage Explorer nie mógł wygenerować klucz używany do szyfrowania poufnych danych. Może to spowodować problemy, korzystając z paska szybki dostęp i dołączanie zasobów. [#535](https://github.com/Microsoft/AzureStorageExplorer/issues/535)
* Jeśli Twoje konto nie wymagało uwierzytelniania Wieloskładnikowego dla swojej dzierżawy głównej, ale dla niektórych innych dzierżaw, Eksploratora usługi Storage nie będzie można subskrypcji listy. Teraz po zarejestrowaniu się przy użyciu tego konta, Eksploratora usługi Storage wyświetli monit o ponownie wprowadzić swoje poświadczenia i wykonać uwierzytelnianie wieloskładnikowe. [#74](https://github.com/Microsoft/AzureStorageExplorer/issues/74)
* Eksplorator usługi Storage nie może dołączyć zasoby z platformy Azure (Niemcy) i Azure instytucji rządowych USA. [#572](https://github.com/Microsoft/AzureStorageExplorer/issues/572)
* Jeśli zalogowano się do dwóch kont, które miały ten sam adres e-mail Eksploratora usługi Storage czasami zakończy się niepowodzeniem do wyświetlenia Twoich zasobów w widoku drzewa. [#580](https://github.com/Microsoft/AzureStorageExplorer/issues/580)
* Na wolniejszych Windows maszynach na ekranie powitalnym zajęłoby czasami znaczną ilość czasu. [#586](https://github.com/Microsoft/AzureStorageExplorer/issues/586)
* Czy pojawi się okno dialogowe Połącz, nawet wtedy, gdy było dołączone konta lub usługi. [#588](https://github.com/Microsoft/AzureStorageExplorer/issues/588)

### <a name="new"></a>Nowa
* Załączniki zewnętrznego zasobu, takie jak w przypadku połączenia SAS i emulatorów, została znacznie ulepszona. Teraz możesz wykonywać następujące czynności:
   * Dostosuj nazwę wyświetlaną zasób, który jest podłączany. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Dołącz do wielu emulatorów lokalnego przy użyciu różnych portów. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Dodaj zasoby dołączone do paska szybki dostęp. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Eksplorator usługi Storage obsługuje teraz usuwanie nietrwałe. Możesz:
   * Skonfiguruj zasady usuwanie nietrwałe, klikając prawym przyciskiem myszy w węźle kontenery obiektów Blob konta magazynu.
   * Widok nietrwale usunięte obiekty BLOB w Edytorze obiektów Blob, wybierając pozycję "Active i usunięte obiekty BLOB" w menu rozwijanym obok paska nawigacji.
   * Usuniętych nietrwale usunięte obiekty BLOB.

### <a name="fixes"></a>Poprawki
* Akcja "Konfiguruj ustawienia specyfikacji CORS" nie jest już dostępna dla kont usługi Premium Storage ponieważ kont usługi Premium Storage nie obsługują mechanizmu CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Teraz jest właściwością sygnatura dostępu współdzielonego dla usługi z dołączoną sygnatury dostępu Współdzielonego. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* Akcja "Ustaw domyślna Warstwa dostępu" jest teraz dostępne dla obiektów Blob i magazynu GPV2 konta, które zostały przypięte do paska szybki dostęp. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Czasami Eksploratora usługi Storage zakończy się niepowodzeniem pokazać klasycznych kont magazynu. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Znane problemy
* Przy użyciu emulatorów, takich jak Emulator usługi Azure Storage lub Azurite, należy je nasłuchiwać połączeń na ich domyślne porty. W przeciwnym razie Eksploratora usługi Storage nie będzie można połączyć się z nimi.
* Jeśli używasz programu VS dla komputerów Mac i nigdy nie zostały utworzone niestandardowej konfiguracji usługi AAD, możesz nie mieć możliwości logowania. Aby obejść ten problem, usuń zawartość ~ /. IdentityService/AadConfigurations. Jeśli to nie niedogodność, należy dodać komentarz dotyczący [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite nie została jeszcze w pełni zaimplementowana wszystkie interfejsy API usługi Storage. W związku z tym może występować nieoczekiwanych błędów lub zachowanie w przypadku używania Azurite dla magazynem projektowym.
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przekazywanie z folderu usługi OneDrive nie działa z powodu błędów w środowisku NodeJS. Błąd został rozwiązany, ale nie są jeszcze zintegrowane z elektronów.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-130"></a>Wersja 1.3.0
07/09/2018 r.

### <a name="new"></a>Nowa
* Uzyskiwanie dostępu do kontenerów $web używane przez statycznej witryny sieci Web jest teraz obsługiwane. Dzięki temu można łatwo przekazywać i zarządzania plikami i folderami używane przez witryny sieci Web. [#223](https://github.com/Microsoft/AzureStorageExplorer/issues/223)
* Na pasku aplikacji w systemie macOS została zreorganizowana. Zmiany obejmują menu Plik, niektóre zmiany klucza skrótu i kilka nowych poleceń w menu aplikacji. [#99](https://github.com/Microsoft/AzureStorageExplorer/issues/99)
* Punkt końcowy urzędu do logowania się na platformie Azure instytucji rządowych USA została zmieniona na https://login.microsoftonline.us/
* Ułatwienia dostępu: Włączeniu czytnika ekranu, nawigacji za pomocą klawiatury teraz współpracuje z tabel, służący do wyświetlania elementów po prawej stronie. Za pomocą klawiszy strzałek do nawigacji wierszy i kolumn, Enter, aby wywołać akcje domyślne klawisz menu kontekst, otwórz menu kontekstowe dla elementu i Shift lub kontrolować wybór wielokrotny. [#103](https://github.com/Microsoft/AzureStorageExplorer/issues/103)

### <a name="fixes"></a>Poprawki
*  Na niektórych komputerach procesy podrzędne zostały zajmuje dużo czasu do uruchomienia. Jeśli ta sytuacja może mieć miejsce, pojawią się błędem "proces podrzędny nie można uruchomić w odpowiednim czasie". Teraz został zwiększony czas przydzielony na wykonanie procesu podrzędnego, można uruchomić z 20 do 90 sekund. Jeśli są nadal objęte tym problemem, Skomentuj na połączonych problem w usłudze GitHub. [#281](https://github.com/Microsoft/AzureStorageExplorer/issues/281)
* Przy użyciu sygnatury dostępu Współdzielonego, który nie ma uprawnienia do odczytu, nie było możliwe przekazywanie dużych obiektów blob. Logiki w celu przekazania została zmodyfikowana, aby pracować w tym scenariuszu. [#305](https://github.com/Microsoft/AzureStorageExplorer/issues/305)
* Ustawienie poziom dostępu publicznego do kontenera spowoduje usunięcie wszystkich zasad dostępu i na odwrót. Teraz zasady i dostępu publicznego dostępu zostaną zachowane, ustawiając jedną z dwóch. [#197](https://github.com/Microsoft/AzureStorageExplorer/issues/197)
* "AccessTierChangeTime" został obcięty w oknie dialogowym właściwości. Ten problem został rozwiązany. [#145](https://github.com/Microsoft/AzureStorageExplorer/issues/145)
* "Microsoft Azure Storage Explorer —" prefiks brakowało okna dialogowego Tworzenie nowego katalogu. Ten problem został rozwiązany. [#299](https://github.com/Microsoft/AzureStorageExplorer/issues/299)
* Ułatwienia dostępu: Okno dialogowe Dodawanie jednostki trudno było je przejść, korzystając z VoiceOver. Wprowadzono ulepszenia. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Ułatwienia dostępu: Kolor tła przycisk Zwiń/Rozwiń okienko akcji i właściwości była niespójna przy użyciu podobnych kontrolek interfejsu użytkownika w kompozycji wysoki kontrast — kolor czarny. Kolor został zmieniony. [#123](https://github.com/Microsoft/AzureStorageExplorer/issues/123)
* Ułatwienia dostępu: Motyw wysoki kontrast — kolor czarny fokus, ustawianie stylów dla przycisku "X" w oknie dialogowym właściwości nie były widoczne. Ten problem został rozwiązany. [#243](https://github.com/Microsoft/AzureStorageExplorer/issues/243)
* Ułatwienia dostępu: Karty akce a Vlastnosti brakuje kilku wartości aria, które zakończyły się środowisko czytnika ekranu niepoprawne. Brak wartości aria został dodany. [#316](https://github.com/Microsoft/AzureStorageExplorer/issues/316)
* Ułatwienia dostępu: Węzłów zwinięty drzewa po lewej stronie zostały nie zostało dostarczone rozwinięte aria musi mieć wartość false. Ten problem został rozwiązany. [#352](https://github.com/Microsoft/AzureStorageExplorer/issues/352)

### <a name="known-issues"></a>Znane problemy
* Odłączania od zasobu dołączone za pomocą identyfikatora URI połączenia SAS, takich jak kontener obiektów blob może spowodować błąd uniemożliwiający inne załączniki z pojawią się poprawnie. Aby obejść ten problem, wystarczy odświeżyć węzeł grupy. Zobacz [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/537) Aby uzyskać więcej informacji.
* Jeśli używasz programu VS dla komputerów Mac i nigdy nie zostały utworzone niestandardowej konfiguracji usługi AAD, możesz nie mieć możliwości logowania. Aby obejść ten problem, usuń zawartość ~ /. IdentityService/AadConfigurations. Jeśli to nie niedogodność, należy dodać komentarz dotyczący [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite nie została jeszcze w pełni zaimplementowana wszystkie interfejsy API usługi Storage. W związku z tym może występować nieoczekiwanych błędów lub zachowanie w przypadku używania Azurite dla magazynem projektowym.
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przekazywanie z folderu usługi OneDrive nie działa z powodu błędów w środowisku NodeJS. Błąd został rozwiązany, ale nie są jeszcze zintegrowane z elektronów.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Usługa Azure Stack nie obsługuje następujące funkcje i próby ich wykorzystania podczas pracy z usługą Azure Stack może spowodować nieoczekiwane błędy:
   * Udziały plików
   * Poziomy dostępu
   * Usuwanie nietrwałe
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-120"></a>Wersji 1.2.0 lub nowszej
06/12/2018 r.

### <a name="new"></a>Nowa
* Jeśli Eksploratora usługi Storage nie można załadować subskrypcji z podzbiorem dzierżawcy, następnie wszystkich pomyślnie załadowany w subskrypcji będą wyświetlane wraz z komunikatu o błędzie specjalnie dla dzierżaw, które nie powiodło się. [#159](https://github.com/Microsoft/AzureStorageExplorer/issues/159)
* W Windows gdy aktualizacja jest dostępna, możesz teraz możliwość "Aktualizuj przy zamknięciu". Gdy ta opcja jest pobierana, Instalator aktualizacji zostanie uruchomiony po zamknięciu programu Storage Explorer. [#21](https://github.com/Microsoft/AzureStorageExplorer/issues/21)
* Przywracanie migawki został dodany do menu kontekstowe edytora udziału plików podczas wyświetlania migawki udziału plików. [#131](https://github.com/Microsoft/AzureStorageExplorer/issues/131)
* Przycisk Wyczyść kolejkę zawsze jest teraz włączony. [#135](https://github.com/Microsoft/AzureStorageExplorer/issues/135)
* Obsługa logowania do usług AD FS usługi Azure Stack została ponownie włączona. Usługa Azure Stack w wersji 1804 lub nowszej jest wymagana. [#150](https://github.com/Microsoft/AzureStorageExplorer/issues/150)

### <a name="fixes"></a>Poprawki
* Jeśli migawki dla udziału plików, którego nazwa została prefiksu innego udziału plików na tym samym koncie magazynu jest wyświetlane, następnie migawki dla udziału plików będą również wyświetlane. Ten problem został rozwiązany. [#255](https://github.com/Microsoft/AzureStorageExplorer/issues/255)
* Gdy dołączone, za pomocą sygnatury dostępu Współdzielonego, przywracanie pliku z migawki udziału plików spowoduje błąd. Ten problem został rozwiązany. [#211](https://github.com/Microsoft/AzureStorageExplorer/issues/211)
* Podczas przeglądania migawek obiektu blob, akcji podwyższanie poziomu migawki została włączona, gdy wybrano podstawowy obiekt blob i pojedynczej migawki. Akcja teraz jest włączony tylko jeśli wybrano opcję pojedynczej migawki. [#230](https://github.com/Microsoft/AzureStorageExplorer/issues/230)
* Jeśli pojedynczego zadania (takie jak pobieranie obiektu blob) zostało uruchomione i nowszym nie powiodło się, go czy nie automatycznego ponownego do momentu rozpoczęcia inne zadanie tego samego typu. Wszystkie zadania powinien teraz automatycznie ponownych prób, niezależnie od tego, jak wiele zadań można ustawić w kolejce.
* Edytory otwarty nowo kontenery obiektów blob utworzonych w konta GPV2 i kont usługi Blob Storage nie ma kolumny warstwy dostępu. Ten problem został rozwiązany. [#109](https://github.com/Microsoft/AzureStorageExplorer/issues/109)
* Kolumny warstwy dostępu może czasami są wyświetlane, gdy konto magazynu lub kontenera obiektów blob został dołączony przy użyciu sygnatury dostępu Współdzielonego. Kolumna będzie zawsze wyświetlana, ale z pustą wartość, jeśli nie ustawiono warstwy dostępu. [#160](https://github.com/Microsoft/AzureStorageExplorer/issues/160)
* Ustawienie warstwy dostępu do nowo przesłanym blokowy obiekt blob został wyłączony. Ten problem został rozwiązany. [#171](https://github.com/Microsoft/AzureStorageExplorer/issues/171)
* Jeśli przycisk "Zachowaj kartę Otwórz" została wywołana przy użyciu klawiatury, fokus klawiatury może zostać utracone. Teraz fokus zostanie przesunięty na kartę która została zachowana open. [#163](https://github.com/Microsoft/AzureStorageExplorer/issues/163)
* Zapytania w Konstruktorze zapytań VoiceOver nie był zawierający opis użyteczne bieżący operator. Jest teraz bardziej opisowe. [#207](https://github.com/Microsoft/AzureStorageExplorer/issues/207)
* Linki dzielenia na strony dla różnych edytorów nie jest opisowy. Zostały zmienione za bardziej opisowe. [#205](https://github.com/Microsoft/AzureStorageExplorer/issues/205)
* W oknie dialogowym Dodawanie jednostki VoiceOver został nie informuje o jakie kolumny elementu input było częścią. Nazwa bieżącej kolumny teraz znajduje się w opisie elementu. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Przyciski radiowe i pola wyboru nie miał widoczne obramowanie z fokusem. Ten problem został rozwiązany. [#237](https://github.com/Microsoft/AzureStorageExplorer/issues/237)

### <a name="known-issues"></a>Znane problemy
* Przy użyciu emulatorów, takich jak Emulator usługi Azure Storage lub Azurite, należy je nasłuchiwać połączeń na ich domyślne porty. W przeciwnym razie Eksploratora usługi Storage nie będzie można połączyć się z nimi.
* Jeśli używasz programu VS dla komputerów Mac i nigdy nie zostały utworzone niestandardowej konfiguracji usługi AAD, możesz nie mieć możliwości logowania. Aby obejść ten problem, usuń zawartość ~ /. IdentityService/AadConfigurations. Jeśli to nie niedogodność, należy dodać komentarz dotyczący [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite nie została jeszcze w pełni zaimplementowana wszystkie interfejsy API usługi Storage. W związku z tym może występować nieoczekiwanych błędów lub zachowanie w przypadku używania Azurite dla magazynem projektowym.
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przekazywanie z folderu usługi OneDrive nie działa z powodu błędów w środowisku NodeJS. Błąd został rozwiązany, ale nie są jeszcze zintegrowane z elektronów.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-110"></a>Wersji 1.1.0
05/09/2018 r.

### <a name="new"></a>Nowa
* Eksplorator usługi Storage obsługuje teraz użycie Azurite. Uwaga: połączenie Azurite jest zakodowana w domyślnych punktów końcowych rozwoju.
* Eksplorator usługi Storage obsługuje teraz warstw dostępu tylko do obiektów Blob i konta magazynu GPV2. Dowiedz się więcej na temat warstw dostępu [tutaj](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).
* Czas rozpoczęcia nie jest już wymagany podczas generowania sygnatury dostępu Współdzielonego.

### <a name="fixes"></a>Poprawki
* Podczas pobierania subskrypcji dla instytucji rządowych USA kont zostało przerwane. Ten problem został rozwiązany. [#61](https://github.com/Microsoft/AzureStorageExplorer/issues/61)
* Poprawnie czas wygaśnięcia zasady dostępu nie został on zapisany. Ten problem został rozwiązany. [#50](https://github.com/Microsoft/AzureStorageExplorer/issues/50)
* Podczas generowania adresu URL sygnatury dostępu Współdzielonego dla elementu w kontenerze, nazwa elementu została nie są dołączane do adresu URL. Ten problem został rozwiązany. [#44](https://github.com/Microsoft/AzureStorageExplorer/issues/44)
* Podczas tworzenia sygnatury dostępu Współdzielonego, czas wygaśnięcia, znajdujących się w przeszłości czasami będzie wartością domyślną. Jest to spowodowane Eksploratora usługi Storage przy użyciu ostatnio używany czas rozpoczęcia i utraty ważności jako wartości domyślne. Teraz za każdym razem, gdy możesz otworzyć okno dialogowe sygnatury dostępu Współdzielonego, jest generowany nowy zestaw wartości domyślnych. [#35](https://github.com/Microsoft/AzureStorageExplorer/issues/35)
* Podczas kopiowania między kontami magazynu, 24-godzinny sygnatury dostępu Współdzielonego jest generowany. Jeśli kopia trwała dłużej niż 24 godziny, kopia może zakończyć się niepowodzeniem. Zwiększyliśmy sygnatury dostępu Współdzielonego w celu ostatni 1 tydzień Aby zmniejszyć prawdopodobieństwo kopię kończy się niepowodzeniem z powodu wygasły sygnatury dostępu Współdzielonego. [#62](https://github.com/Microsoft/AzureStorageExplorer/issues/62)
* W przypadku niektórych działań kliknij "Anuluj", nie będzie zawsze działać. Ten problem został rozwiązany. [#125](https://github.com/Microsoft/AzureStorageExplorer/issues/125)
* W przypadku niektórych działań szybkość transferu była nieprawidłowa. Ten problem został rozwiązany. [#124](https://github.com/Microsoft/AzureStorageExplorer/issues/124)
* Pisownię wyrazu "Wstecz", w menu Widok była nieprawidłowa. Teraz została poprawnie wpisana. [#71](https://github.com/Microsoft/AzureStorageExplorer/issues/71)
* Na ostatniej stronie Instalatora Windows ma przycisk "Dalej". Został on zmieniony na przycisku "Zakończ". [#70](https://github.com/Microsoft/AzureStorageExplorer/issues/70)
* Fokus na klawiszu TAB nie był widoczny dla przycisków w oknach dialogowych podczas korzystania z motywu czarny połączenia Hybrydowego. Jest teraz widoczne. [#64](https://github.com/Microsoft/AzureStorageExplorer/issues/64)
* Wielkość liter w wyrazie "Auto-Rozwiąż" dla akcji w dzienniku aktywności była nieprawidłowa. Teraz jest poprawna. [#51](https://github.com/Microsoft/AzureStorageExplorer/issues/51)
* Podczas usuwania jednostki z tabeli, okno dialogowe z prośbą o potwierdzenie wyświetlana ikona błędu. Okno dialogowe używa teraz ikonę ostrzeżenia. [#148](https://github.com/Microsoft/AzureStorageExplorer/issues/148)

### <a name="known-issues"></a>Znane problemy
* Jeśli używasz programu VS dla komputerów Mac i nigdy nie zostały utworzone niestandardowej konfiguracji usługi AAD, możesz nie mieć możliwości logowania. Aby obejść ten problem, usuń zawartość ~ /. IdentityService/AadConfigurations. Jeśli to nie niedogodność, należy dodać komentarz dotyczący [ten problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite nie została jeszcze w pełni zaimplementowana wszystkie interfejsy API usługi Storage. W związku z tym może występować nieoczekiwanych błędów lub zachowanie w przypadku używania Azurite dla magazynem projektowym.
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przekazywanie z folderu usługi OneDrive nie działa z powodu błędów w środowisku NodeJS. Błąd został rozwiązany, ale nie są jeszcze zintegrowane z elektronów.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-100"></a>Wersja 1.0.0
16/04/2018

### <a name="new"></a>Nowa
* Rozszerzone uwierzytelnianie umożliwia Eksploratora usługi Storage do użycia tego samego konta magazynu jako Visual Studio 2017. Aby użyć tej funkcji, należy ponownie zaloguj się do kont i ponownie ustaw filtrowane subskrypcji.
* W przypadku kont usługi Azure Stack wspierana przez usługi AAD Eksploratora usługi Storage, pobiera subskrypcji usługi Azure Stack po włączeniu docelowej usługi Azure Stack. Nie jest już konieczne Utwórz środowisko niestandardowe nazwy logowania.
* Dodano kilka skróty umożliwiają szybsze nawigowanie. Obejmują one przełączanie różnych zespołów i przechodzenia między edytorów. Wyświetlić menu Widok, aby uzyskać więcej informacji.
* Opinie Eksploratora magazynu znajduje się teraz w witrynie GitHub. Dotrzeć do naszej stronie problemów, klikając przycisk opinii w dolnym lewym lub przechodząc do [ https://github.com/Microsoft/AzureStorageExplorer/issues ](https://github.com/Microsoft/AzureStorageExplorer/issues). Swobodnie sugestie, zgłaszania problemów, zadawać pytania lub pozostaw żaden inny rodzaj opinii.
* Jeśli zostały przekroczone problemy z certyfikatami SSL i nie można odnaleźć certyfikatu powodujący problemy, teraz możesz uruchamiać Eksplorator usługi Storage z poziomu wiersza polecenia przy użyciu `--ignore-certificate-errors` flagi. Gdy uruchomiony przy użyciu tej flagi, Eksploratora usługi Storage będzie ignorować błędy certyfikatu SSL.
* Jest teraz opcję "Pobierz" w menu kontekstowym dla obiektów blob i plików elementów.
* Ulepszone ułatwienia dostępu i obsługę czytników zawartości ekranu. Jeśli możesz polegać na funkcje ułatwień dostępu, zobacz nasze [dokumentacji ułatwień dostępu](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-accessibility) Aby uzyskać więcej informacji.
* Eksplorator usługi Storage korzysta z elektronów 1.8.3

### <a name="breaking-changes"></a>Zmiany powodujące niezgodność
* Eksplorator usługi Storage zostało przełączone nową bibliotekę uwierzytelniania. W ramach przełącznika do biblioteki trzeba będzie ponownie zaloguj się do kont i ponownie ustaw filtrowane subskrypcji
* Metoda używana do szyfrowania poufnych danych został zmieniony. Może to spowodować niektórych elementów paska szybki dostęp bez potrzeby ponownego dodania i/lub niektórzy z was dołączone zasoby wymagające ponownie dołączyć.

### <a name="fixes"></a>Poprawki
* Niektórzy użytkownicy się za serwery proxy miałby przekazywaniu obiektów blob do grupy lub Pobieranie przerwane przez "Nie można rozpoznać" komunikat o błędzie. Ten problem został rozwiązany.
* W razie logowania zostało podczas przy użyciu linku bezpośredniego, klikając polecenie w wierszu polecenia "Sign-In" będzie wyskakujące puste okno dialogowe. Ten problem został rozwiązany.
* W systemie Linux, jeśli Eksplorator usługi Storage to nie można uruchomić z powodu awarii procesu procesora GPU, użytkownik będzie teraz informowany o awarii, informację, aby użyć "--procesora gpu disable" przełącznika i Eksploratora usługi Storage będą następnie automatycznie ponownie uruchom za pomocą przełącznika włączone.
* Nieprawidłowy dostęp zasady były trudne do tożsamości w oknie dialogowym zasad dostępu. Zasady dostępu nieprawidłowe identyfikatory podane są teraz w kolorze czerwonym, aby uzyskać lepszą widoczność.
* Dziennik aktywności, czasami musi duże obszary odstępów między poszczególnymi częściami działania. Ten problem został rozwiązany.
* W edytorze zapytań tabeli jeśli left klauzulę timestamp w nieprawidłowym stanie, a następnie podjęto próbę modyfikacji inną klauzulę zablokować edytora. Edytor przywróci klauzuli timestamp ostatni stan prawidłowy po wykryciu zmiany w klauzuli innego.
* Jeśli zostanie wstrzymana podczas wpisywania w zapytaniu wyszukiwania w widoku drzewa, wyszukiwanie będzie rozpocznie się i fokus, czy skradziony z pola tekstowego. Teraz należy jawnie rozpocząć wyszukiwanie, naciskając klawisz "Enter" lub klikając na przycisk start wyszukiwania.
* Polecenie "Uzyskaj sygnaturę dostępu współdzielonego" czasami zostałoby wyłączone, kliknięcie prawym przyciskiem myszy plik w udziale plików. Ten problem został rozwiązany.
* Jeśli węzeł drzewa zasób mający fokus został odfiltrowany podczas wyszukiwania, może nie kartę w drzewie zasobów i użyj klawiszy strzałek, aby nawigować drzewo zasobów. Teraz Jeśli węzeł drzewa wąsko zdefiniowany zasób jest ukryty, pierwszy węzeł w drzewie zasobów automatycznie skoncentruje się.
* To dodatkowe separator czasami będzie widoczny na pasku narzędzi edytora. Ten problem został rozwiązany.
* Pole tekstowe łączy do stron nadrzędnych czasami spowodowałoby przepełnienie. Ten problem został rozwiązany.
* Edytory obiektów Blob i udział plików będzie czasami stale Odśwież podczas przekazywania wielu plików jednocześnie. Ten problem został rozwiązany.
* Funkcja "Folder statystyki" miał bezcelowe w widoku Zarządzanie migawki udziału plików. Teraz została wyłączona.
* W systemie Linux w menu Plik nie były wyświetlane. Ten problem został rozwiązany.
* Podczas przekazywania folderu w udziale plików, domyślnie, tylko zawartość folderu zostały przekazane. Teraz zachowanie domyślne jest przekazywanie zawartości folderu do dopasowania folderu w udziale plików.
* Kolejność przycisków w oknach dialogowych kilka miał została wycofana. Ten problem został rozwiązany.
* Różne zabezpieczeń związane z poprawkami.

### <a name="known-issues"></a>Znane problemy
* W rzadkich przypadkach fokus drzewa może zakończyć się zatrzymaniem na szybki dostęp. Aby odklej fokus, można na nim Odśwież wszystko.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. Jest to spowodowane użyto rozwiązanie filtr anulowania, które są opisane w tym miejscu.
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Użytkownicy systemu Linux, musisz zainstalować [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-096"></a>Wersja 0.9.6
2018-02/28

### <a name="fixes"></a>Poprawki
* Wystąpił problem nie oczekiwano obiektów blob i pliki w edytorze. Ten problem został rozwiązany.
* Problem spowodował, że przełączanie widoków migawki niepoprawne wyświetlanie elementów. Ten problem został rozwiązany.

### <a name="known-issues"></a>Znane problemy
* Eksplorator usługi Storage nie obsługują kont usług AD FS.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. To dlatego używamy opisane obejście filtr Anuluj [tutaj](https://github.com/Azure/azure-storage-node/issues/317).
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia, aby filtrowanie subskrypcji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-095"></a>Wersja 0.9.5
02 06 2018

### <a name="new"></a>Nowa

* Pomoc techniczna dla migawek udziałów plików:
    * Tworzenie i zarządzanie migawek dla udziałów plików.
    * Łatwo przełączać widoki między migawek udziałów plików, gdy eksplorujesz.
    * Przywróć poprzednie wersje plików.
* Obsługa usługi Azure Data Lake Store (wersja zapoznawcza):
    * Połączenie z zasobami usługi ADLS na wielu kontach.
    * Nawiązywanie połączenia i udostępnianie zasobów usługi ADLS za pomocą identyfikatorów URI usługi ADL.
    * Wykonywanie cyklicznie operacji podstawowego pliku/folderu.
    * Przypnij poszczególnych folderów do paska szybki dostęp.
    * Wyświetlanie statystyk folderów.

### <a name="fixes"></a>Poprawki
* Zwiększona wydajność uruchamiania.
* Różne poprawki.

### <a name="known-issues"></a>Znane problemy
* Eksplorator usługi Storage nie obsługują kont usług AD FS.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. Jest to spowodowane użyto rozwiązanie filtr anulowania, które są opisane w tym miejscu.
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia, aby filtrowanie subskrypcji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:

```
./StorageExplorer.exe --disable-gpu
```

* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>Wersja 0.9.4 i 0.9.3
2018-01/21

### <a name="new"></a>Nowa
* Istniejącego okna Eksploratora usługi Storage będą używane ponownie gdy:
    * Otwieranie linków bezpośrednich generowane w Eksploratorze usługi Storage.
    * Otwieranie Eksploratora usługi Storage w portalu.
    * Otwieranie Eksploratora usługi Storage z rozszerzenia Azure Storage w programie VS Code (wkrótce).
* Dodano możliwość otwierania nowego okna Eksploratora usługi Storage z w Eksploratorze usługi Storage.
    * Windows jest to opcja "W nowym oknie" w Menu Plik i w menu kontekstowym na pasku zadań.
    * Dla komputerów Mac jest opcja "W nowym oknie" w menu aplikacji.

### <a name="fixes"></a>Poprawki
* Rozwiązano problem z zabezpieczeniami. Przeprowadź uaktualnienie do 0.9.4 przy najbliższej sposobności.
* Stary działania zostały nie odpowiednio są czyszczone. Ma to wpływu na wydajność długo działające zadania. One są teraz są czyszczone poprawnie.
* Akcje związane z dużą liczbą plików i katalogów spowoduje, że od czasu do czasu Eksploratora usługi Storage można zablokować. Żądania na platformie Azure dla udziałów plików teraz są ograniczone do ograniczania wykorzystania zasobów systemowych.

### <a name="known-issues"></a>Znane problemy
* Eksplorator usługi Storage nie obsługują kont usług AD FS.
* Klawisze skrótów dla "Widoku Eksploratora" i "Widoku zarządzania kontami" powinien być Ctrl / Cmd + Shift + E i Ctrl / Cmd + Shift + A odpowiednio.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. Jest to spowodowane użyto rozwiązanie filtr anulowania, które są opisane w tym miejscu.
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia, aby filtrowanie subskrypcji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:
```
./StorageExplorer --disable-gpu
```
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Wersja 0.9.2
11/01/2017

### <a name="hotfixes"></a>Poprawki
* Nieoczekiwane dane zmiany były możliwe, podczas edytowania Edm.DateTime wartości dla jednostek z tabeli w zależności od lokalnej strefy czasowej. Edytor korzysta z pola tekstowego, dając precyzyjny, spójną kontrolę nad Edm.DateTime wartości.
* Trwa przekazywanie/pobieranie grupy obiektów blob, gdy dołączony nazwą i kluczem nie może uruchomić. Ten problem został rozwiązany.
* Wcześniej Eksploratora usługi Storage będzie tylko monit o ponowne uwierzytelnianie stare konto, jeśli jeden lub więcej subskrypcji do konta został wybrany. Teraz Eksploratora usługi Storage wyświetli monit nawet wtedy, gdy konto jest w pełni odfiltrowane.
* Domena punktów końcowych dla usługi Azure instytucji rządowych USA była nieprawidłowa. Został rozwiązany.
* Przycisk Zastosuj w oknie Zarządzanie kontami czasami był trudny do kliknij. Już nie powinno to nastąpić.

### <a name="new"></a>Nowa
* Obsługa usługi Azure Cosmos DB (wersja zapoznawcza):
    * [Dokumentację w trybie online](./cosmos-db/storage-explorer.md)
    * Tworzenie baz danych i kolekcji
    * Manipulowanie danymi
    * Zapytanie, tworzenie lub usuwanie dokumentów
    * Zaktualizuj procedury składowane, funkcje zdefiniowane przez użytkownika lub wyzwalaczy
    * Używać parametrów połączenia, aby połączyć się i zarządzanie bazami danych
* Zwiększona wydajność Trwa przekazywanie/pobieranie wielu małych obiektów blob.
* Jeśli występują błędy w grupie przekazywania obiektu blob lub grupa pobierania obiektów blob, należy dodać akcji "Ponów próbę wykonania All".
* Eksplorator usługi Storage teraz wstrzyma iteracji podczas pobierania/przekazywania obiektów blob, jeśli wykryje, że połączenie sieciowe zostało utracone. Następnie można wznowić iteracji, po ponownym nawiązaniu połączenia sieciowego.
* Dodanie do "Zamknij", "Zamknij inne" i "Zamknij" karty przy użyciu menu kontekstowego.
* Eksplorator usługi Storage korzysta z natywnych oknami dialogowymi i menu kontekstowe natywnych.
* Eksplorator usługi Storage jest teraz łatwiej dostępne. Ulepszenia obejmują:
    * Obsługiwane czytniki zawartości ekranu ulepszone, NVDA na Windows i VoiceOver na komputerze Mac
    * Ulepszone motyw o wysokim kontraście
    * Fokus klawiatury i przełączania klawiatury poprawki

### <a name="fixes"></a>Poprawki
* Jeśli próbowano otworzyć lub pobrać obiekt blob z nieprawidłową nazwą pliku Windows, operacja zakończy się niepowodzeniem. Eksplorator usługi Storage teraz wykryje, jeśli nazwa obiektu blob jest nieprawidłowy i zapytaj, czy chcesz pominąć obiektu blob lub Zakoduj je. Eksplorator usługi Storage wykryje również, jeśli nazwa pliku wydaje się być kodowany i pytanie, jeśli chcesz odkodować go przed przekazaniem.
* Podczas przekazywania obiektów blob edytor dla docelowym kontenerem obiektów blob może czasami nie prawidłowo odświeżenia. Ten problem został rozwiązany.
* Obsługa kilku rodzaje parametrów połączeń i identyfikatorów URI sygnatury dostępu Współdzielonego jest uwzględniona. Firma Microsoft zostały rozwiązane wszystkie znane problemy, ale należy wysłać opinię, jeśli dodatkowo wystąpią problemy.
* Powiadomienie o aktualizacji został przerwany dla niektórych użytkowników w 0.9.0. Ten problem został rozwiązany, a dla osób, wpływ usterka, możesz ręcznie pobrać najnowszą wersję Eksploratora usługi Storage [tutaj](https://azure.microsoft.com/features/storage-explorer/).

### <a name="known-issues"></a>Znane problemy
* Eksplorator usługi Storage nie obsługują kont usług AD FS.
* Klawisze skrótów dla "Widoku Eksploratora" i "Widoku zarządzania kontami" powinien być Ctrl / Cmd + Shift + E i Ctrl / Cmd + Shift + A odpowiednio.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. Jest to spowodowane użyto rozwiązanie filtr anulowania, które są opisane w tym miejscu.
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia, aby filtrowanie subskrypcji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:
```
./StorageExplorer --disable-gpu
```
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>Wersja 0.9.1 i 0.9.0
10/20/2017
### <a name="new"></a>Nowa
* Obsługa usługi Azure Cosmos DB (wersja zapoznawcza):
    * [Dokumentację w trybie online](./cosmos-db/storage-explorer.md)
    * Tworzenie baz danych i kolekcji
    * Manipulowanie danymi
    * Zapytanie, tworzenie lub usuwanie dokumentów
    * Zaktualizuj procedury składowane, funkcje zdefiniowane przez użytkownika lub wyzwalaczy
    * Używać parametrów połączenia, aby połączyć się i zarządzanie bazami danych
* Zwiększona wydajność Trwa przekazywanie/pobieranie wielu małych obiektów blob.
* Jeśli występują błędy w grupie przekazywania obiektu blob lub grupa pobierania obiektów blob, należy dodać akcji "Ponów próbę wykonania All".
* Eksplorator usługi Storage teraz wstrzyma iteracji podczas pobierania/przekazywania obiektów blob, jeśli wykryje, że połączenie sieciowe zostało utracone. Następnie można wznowić iteracji, po ponownym nawiązaniu połączenia sieciowego.
* Dodanie do "Zamknij", "Zamknij inne" i "Zamknij" karty przy użyciu menu kontekstowego.
* Eksplorator usługi Storage korzysta z natywnych oknami dialogowymi i menu kontekstowe natywnych.
* Eksplorator usługi Storage jest teraz łatwiej dostępne. Ulepszenia obejmują:
    * Obsługiwane czytniki zawartości ekranu ulepszone, NVDA na Windows i VoiceOver na komputerze Mac
    * Ulepszone motyw o wysokim kontraście
    * Fokus klawiatury i przełączania klawiatury poprawki

### <a name="fixes"></a>Poprawki
* Jeśli próbowano otworzyć lub pobrać obiekt blob z nieprawidłową nazwą pliku Windows, operacja zakończy się niepowodzeniem. Eksplorator usługi Storage teraz wykryje, jeśli nazwa obiektu blob jest nieprawidłowy i zapytaj, czy chcesz pominąć obiektu blob lub Zakoduj je. Eksplorator usługi Storage wykryje również, jeśli nazwa pliku wydaje się być kodowany i pytanie, jeśli chcesz odkodować go przed przekazaniem.
* Podczas przekazywania obiektów blob edytor dla docelowym kontenerem obiektów blob może czasami nie prawidłowo odświeżenia. Ten problem został rozwiązany.
* Obsługa kilku rodzaje parametrów połączeń i identyfikatorów URI sygnatury dostępu Współdzielonego jest uwzględniona. Firma Microsoft zostały rozwiązane wszystkie znane problemy, ale należy wysłać opinię, jeśli dodatkowo wystąpią problemy.
* Powiadomienie o aktualizacji został przerwany dla niektórych użytkowników w 0.9.0. Ten problem został rozwiązany, a dla osób, wpływ usterka, możesz ręcznie pobrać najnowszą wersję Eksploratora usługi Storage [tutaj](https://azure.microsoft.com/features/storage-explorer/)

### <a name="known-issues"></a>Znane problemy
* Eksplorator usługi Storage nie obsługują kont usług AD FS.
* Klawisze skrótów dla "Widoku Eksploratora" i "Widoku zarządzania kontami" powinien być Ctrl / Cmd + Shift + E i Ctrl / Cmd + Shift + A odpowiednio.
* Przeznaczone dla usługi Azure Stack, przekazywanie pewne pliki jako uzupełnialnych obiektów blob może zakończyć się niepowodzeniem.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. Jest to spowodowane użyto rozwiązanie filtr anulowania, które są opisane w tym miejscu.
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia, aby filtrowanie subskrypcji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Powłoka elektronów wykorzystywane przez Eksploratora magazynu ma problemy z niektórych przyspieszania sprzętowego procesora GPU (jednostka przetwarzania grafiki). Jeśli Eksplorator usługi Storage wyświetla puste okno główne (pusty), możesz spróbować uruchomienie Eksploratora usługi Storage z poziomu wiersza polecenia i wyłączanie przyspieszenie procesora GPU, dodając `--disable-gpu` przełącznika:
```
./StorageExplorer --disable-gpu
```
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Wersja 0.8.16
8/21/2017

### <a name="new"></a>Nowa
* Po otwarciu obiektu blob, Eksploratora usługi Storage wyświetli monit przekazać pobrany plik, jeśli zmiana zostaje wykryta
* Udoskonalone funkcje logowania usługi Azure Stack
* Zwiększona wydajność Trwa przekazywanie/pobieranie wielu małych plików w tym samym czasie


### <a name="fixes"></a>Poprawki
* Dla niektórych typów obiektów blob wybierając pozycję "replace" podczas przekazywania konflikt czasami spowodowałaby przekazywania uruchamiany ponownie.
* W wersji 0.8.15 przekazywania będzie czasami zatrzymania na 99%.
* Podczas przekazywania plików do udziału plików, jeśli została wybrana opcja Przekaż do katalogu, który nie został jeszcze istnieje, przekazywanie zakończy się niepowodzeniem.
* Eksplorator usługi Storage została niepoprawnie Generowanie sygnatury czasowe dla sygnatury dostępu współdzielonego i zapytania w tabeli.


### <a name="known-issues"></a>Znane problemy
* Przy użyciu nazwy i parametry połączenia klucza aktualnie nie działa. Zostanie on rozwiązany w następnej wersji. W międzyczasie można dołączyć nazwą i kluczem.
* Jeśli podczas próby otwarcia pliku o nieprawidłowej nazwie pliku Windows, pliki do pobrania spowoduje błąd — nie znaleziono pliku.
* Po kliknięciu przycisku "Anuluj" do zadania, może upłynąć trochę czasu tego zadania anulować. Jest to ograniczenie biblioteki węzeł magazynu platformy Azure.
* Po zakończeniu przekazywania obiektów blob, karty, który zainicjował przekazywania zostanie odświeżony. Różni się od poprzedniego zachowania, a także spowoduje zostać przeniesiony z powrotem do kontenera, które znajdują się w katalogu głównym.
* Jeśli wybrano nieprawidłowy numer PIN/certyfikatu karty inteligentnej, należy uruchomić ponownie, aby mogła mieć Eksploratora usługi Storage zapomnij tej decyzji.
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia, aby filtrowanie subskrypcji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Dla użytkowników w systemie Ubuntu 14.04, konieczne będzie upewnij się, GCC jest aktualne — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Dla użytkowników w systemie Ubuntu 17.04 będą musieli zainstalować GConf — można to zrobić, uruchamiając następujące polecenia, a następnie ponownym uruchomieniu komputera:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Wersja 0.8.14
06/22/2017

### <a name="new"></a>Nowa

* Zaktualizowana wersja elektronów do 1.7.2, aby można było korzystać z kilku krytyczne aktualizacje zabezpieczeń
* Możesz teraz szybko uzyskiwać dostęp online przewodnik rozwiązywania problemów z menu Pomoc
* Rozwiązywanie problemów z Eksploratora usługi Storage [przewodnik][2]
* [Instrukcje] [ 3] na nawiązywanie połączenia z subskrypcją usługi Azure Stack

### <a name="known-issues"></a>Znane problemy

* Przycisków w oknie dialogowym potwierdzenia usunięcia folderu nie zarejestrujesz kliknięciami myszy w systemie Linux. Obejście polega na użyciu klawisza Enter
* Jeśli wybierzesz nieprawidłowego certyfikatu karty inteligentnej na numer PIN/następnie konieczne będzie ponowne uruchomienie, aby mogła mieć zapomnij decyzji Eksploratora usługi Storage
* Posiadanie więcej niż 3 grup obiektów blob lub przekazywania w tym samym czasie plików może powodować błędy
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia w celu filtrowania subskrypcji
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack aktualnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Ubuntu 14.04 instalacji wymaga wersji kompilatora gcc, zaktualizowane lub uaktualnione — kroki niezbędne do uaktualnienia są poniżej:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Wersja 0.8.13
05/12/2017

#### <a name="new"></a>Nowa

* Rozwiązywanie problemów z Eksploratora usługi Storage [przewodnik][2]
* [Instrukcje] [ 3] na nawiązywanie połączenia z subskrypcją usługi Azure Stack

#### <a name="fixes"></a>Poprawki

* Poprawiono: Przekazywanie pliku zadowoleni wysokiej powodować błąd braku pamięci
* Rozwiązano problem: Użytkownik może teraz Zaloguj się przy użyciu kodu PIN/karty inteligentnej
* Poprawiono: Otwarcie portalu teraz działa przy użyciu platformy Azure China, Azure (Niemcy), dla administracji USA i usługi Azure Stack
* Poprawiono: Podczas przekazywania folderu do kontenera obiektów blob, "Niedozwolona operacja" mógłby wystąpić błąd, czasami
* Rozwiązano: Zaznacz wszystko zostało wyłączone migawek i zarządzania nimi
* Naprawiono: Metadane obiektu blob podstawowy może spowodować zastąpienie po obejrzeniu właściwości jego migawek

#### <a name="known-issues"></a>Znane problemy

* Jeśli wybierzesz nieprawidłowego certyfikatu karty inteligentnej na numer PIN/następnie konieczne będzie ponowne uruchomienie, aby mogła mieć zapomnij decyzji Eksploratora usługi Storage
* Gdy jest powiększony wewnątrz lub na zewnątrz, poziom powiększenia chwilowo może zresetować do domyślnego poziomu
* Posiadanie więcej niż 3 grup obiektów blob lub przekazywania w tym samym czasie plików może powodować błędy
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia w celu filtrowania subskrypcji
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Ubuntu 14.04 instalacji wymaga wersji kompilatora gcc, zaktualizowane lub uaktualnione — kroki niezbędne do uaktualnienia są poniżej:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Wersja 0.8.12 i 0.8.11 i 0.8.10
04/07/2017

#### <a name="new"></a>Nowa

* Eksplorator usługi Storage zostanie automatycznie zamknięta podczas instalowania aktualizacji z powiadomienie o aktualizacji
* Szybki dostęp w miejscu udostępnia udoskonalone funkcje do pracy z często używanych zasobów
* W edytorze kontenera obiektów Blob można teraz zobaczyć które maszyny wirtualnej dzierżawy obiektu blob należy do
* Teraz można zwinąć panelu po lewej stronie
* Odnajdywanie teraz działa, w tym samym czasie jako plik do pobrania
* Użyj statystyki w edytorach kontenera obiektów Blob, udziału plików i tabeli, aby wyświetlić rozmiar zasobu lub zaznaczenie
* Możesz teraz zalogować do usługi Azure Active Directory (AAD) na podstawie konta usługi Azure Stack.
* Teraz możesz przekazać pliki w archiwum przekracza 32MB do kont usługi Premium storage
* Ulepszona obsługa ułatwień dostępu
* Teraz można dodać zaufany Base-64 zakodowane certyfikaty X.509 SSL, przechodząc do edycji -&gt; certyfikaty SSL —&gt; Importuj certyfikaty

#### <a name="fixes"></a>Poprawki

* Poprawiono: po odświeżeniu poświadczenia konta, w widoku drzewa może czasami będzie odświeżana automatycznie
* Poprawiono: Generowanie sygnatury dostępu Współdzielonego dla emulatora kolejek i tabel w wyniku nieprawidłowy adres URL
* Naprawiono: kont usługi premium storage można teraz rozszerzać po włączeniu serwera proxy
* Poprawiono: przycisk Zastosuj, na stronie zarządzania konta nie będzie działać gdyby konta 1 lub 0, zaznaczone
* Poprawiono: przekazywanie obiektów blob, które wymagają rozwiązania konfliktu może zakończyć się niepowodzeniem - rozwiązane w 0.8.11
* Poprawiono: wysyłanie opinii został w uszkodzone 0.8.11 - rozwiązane w 0.8.12

#### <a name="known-issues"></a>Znane problemy

* Po uaktualnieniu do wersji 0.8.10, należy odświeżyć wszystkie swoje poświadczenia.
* Gdy jest powiększony wewnątrz lub na zewnątrz, poziom powiększenia chwilowo może zresetować do domyślnego poziomu.
* Posiadanie więcej niż 3 grup obiektów blob lub przekazywania w tym samym czasie plików może powodować błędy.
* Na panelu ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia, aby filtrowanie subskrypcji.
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy.
* Mimo że usługę Azure Stack, obecnie nie obsługuje udziałów plików, udziały plików nadal pojawia się pod węzłem dołączone konto magazynu usługi Azure Stack.
* Ubuntu 14.04 instalacji wymaga wersji kompilatora gcc, zaktualizowane lub uaktualnione — kroki niezbędne do uaktualnienia są poniżej:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089-and-088"></a>Wersja 0.8.9 i 0.8.8
02/23/2017

>[!VIDEO https://www.youtube.com/embed/R6gonK3cYAc?ecver=1]

>[!VIDEO https://www.youtube.com/embed/SrRPCm94mfE?ecver=1]


#### <a name="new"></a>Nowa

* Eksplorator usługi Storage 0.8.9 automatycznie pobierze najnowszą wersję aktualizacji.
* Poprawka: przy użyciu portalu wygenerowanego identyfikatora URI połączenia SAS, aby dołączyć konto magazynu spowoduje wystąpienie błędu.
* Można teraz tworzyć, zarządzać i podwyższanie poziomu migawki obiektu blob.
* Użytkownik może teraz Zaloguj się do konta platformy Azure China, Azure (Niemcy) i dla administracji USA.
* Teraz można zmienić poziom powiększenia. Użyj opcji w menu Widok, aby powiększanie, zmniejszanie i zresetować powiększenie.
* Znaki Unicode są teraz obsługiwane w metadanych użytkownika do plików i obiektów blob.
* Ulepszenia ułatwień dostępu.
* Informacje o wersji w następnej wersji mogą być wyświetlane z powiadomienie o aktualizacji. Można również wyświetlić informacje o bieżącej wersji w menu Pomoc.

#### <a name="fixes"></a>Poprawki

* Naprawiono: numer wersji jest teraz prawidłowo wyświetlane w Panelu sterowania na Windows
* Naprawiono: wyszukiwanie nie jest już ograniczona do 50 000 węzłów
* Poprawiono: przekazywanie do udziału plików, uruchamiane zawsze, jeśli katalog docelowy nie istniał już
* Naprawiono: ulepszona stabilność dla długich przekazywanie i pobieranie

#### <a name="known-issues"></a>Znane problemy

* Gdy jest powiększony wewnątrz lub na zewnątrz, poziom powiększenia chwilowo może zresetować do domyślnego poziomu.
* Szybki dostęp działa tylko z elementami na podstawie subskrypcji. Zasoby lokalne lub dołączone za pomocą klucza lub tokenu sygnatury dostępu Współdzielonego nie są obsługiwane w tej wersji.
* Może potrwać kilka sekund, aby przejść do zasobu docelowego, w zależności od liczby zasobów, możesz mieć szybki dostęp.
* Posiadanie więcej niż 3 grup obiektów blob lub przekazywania w tym samym czasie plików może powodować błędy.

12/16/2016
### <a name="version-087"></a>Wersja 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Nowa

* Można wybrać sposób rozwiązywania konfliktów na początku sesji aktualizacja, pobieranie lub kopiowanie w oknie działań
* Umieść kursor nad kartę, aby wyświetlić pełną ścieżkę zasobów magazynu
* Po kliknięciu na karcie, synchronizuje się z lokalizacji, w okienku nawigacji po lewej stronie

#### <a name="fixes"></a>Poprawki

* Naprawiono: Eksplorator usługi Storage jest teraz zaufanych aplikacji dla komputerów Mac
* Naprawiono: Ubuntu 14.04 jest ponownie obsługiwane
* Naprawiono: Czasami Dodaj konto interfejsu użytkownika miga podczas ładowania subskrypcji
* Naprawiono: Czasami nie wszystkie zasoby magazynu zostały wymienione w okienku nawigacji po lewej stronie
* Poprawiono: Okienko akcji czasami jest wyświetlany pusty akcji
* Naprawiono: Rozmiar okna z ostatniej sesji zamknięte są teraz przechowywane
* Naprawiono: Możesz otworzyć wiele kart dla tego samego zasobu za pomocą menu kontekstowego

#### <a name="known-issues"></a>Znane problemy

* Szybki dostęp działa tylko z elementami na podstawie subskrypcji. Zasoby lokalne lub dołączone za pomocą klucza lub tokenu sygnatury dostępu Współdzielonego nie są obsługiwane w tej wersji
* Może potrwać kilka sekund, aby przejść do zasobu docelowego, w zależności od liczby zasobów, możesz mieć szybki dostęp
* Posiadanie więcej niż 3 grup obiektów blob lub przekazywania w tym samym czasie plików może powodować błędy
* Dojść wyszukiwania przeszukiwanie około 50 000 węzłów — dzięki temu może mieć wpływ na wydajność lub może spowodować, że nieobsługiwany wyjątek
* Po raz pierwszy w systemie macOS przy użyciu Eksploratora usługi Storage mogą pojawiać się wiele monitów pytania o uprawnienia użytkownika do uzyskania dostępu łańcucha kluczy. Sugerujemy, że wybierasz zawsze Zezwalaj na tak monit nie pojawi się ponownie

11/18/2016
### <a name="version-086"></a>Wersja 0.8.6

#### <a name="new"></a>Nowa

* Możesz teraz numer pin najczęściej używane usługi do paska szybki dostęp do prostą nawigację
* Teraz możesz otworzyć wiele edytorów na różnych kartach. Jednym kliknięciem, aby otworzyć kartę tymczasowe; Kliknij dwukrotnie, aby otworzyć kartę stałe. Możesz również kliknąć na karcie tymczasowej umożliwiają stałe karty
* Wprowadziliśmy wydajność i stabilność dla przekazuje i pobiera, szczególnie w przypadku dużych plików na komputerach szybkie
* Puste foldery "wirtualne" można teraz tworzyć kontenery obiektów blob
* Ponownie wprowadziliśmy zakresu wyszukiwania za pomocą naszego nowego wyszukiwania podciągu rozszerzone, dzięki czemu masz teraz dwie opcje wyszukiwania:
    * Globalne wyszukiwanie — po prostu wprowadź wyszukiwany termin w polu tekstowym wyszukiwania
    * Wyszukiwania zakresowego — kliknij na ikonę szkła powiększającego obok węzła, a następnie dodaj wyszukiwany termin do końca ścieżki, lub kliknij prawym przyciskiem myszy i wybierz "Wyszukiwania z tutaj"
* Dodaliśmy różne kompozycje: światła (ustawienie domyślne), ciemny, wysoki kontrast — kolor czarny i wysoki kontrast — kolor biały. Przejdź do edycji -&gt; motywy, aby zmienić swoje preferencje motywów
* Można zmodyfikować właściwości obiektów Blob i plików
* Zapewniamy obecnie obsługę zakodowany (kodowanie base64) i komunikatów w kolejce niekodowany
* W systemie Linux, 64-bitowych systemach operacyjnych jest teraz wymagany. W tej wersji obsługiwane jest ściąganie tylko 64-bitowego systemu Ubuntu 16.04.1 LTS
* Zaktualizowaliśmy nasze logo!

#### <a name="fixes"></a>Poprawki

* Poprawiono: Ekranu zamrożenia problemów
* Naprawiono: Zwiększone zabezpieczenia
* Poprawiono: Czasami zduplikowanego dołączonych kont może się pojawić
* Naprawiono: Obiekt blob z Niezdefiniowany typ zawartości może wygenerować wyjątku
* Poprawiono: Otwieranie panelu zapytania w pustej tabeli nie było możliwe
* Naprawiono: Różni się w usłudze wyszukiwania
* Naprawiono: Zwiększenie liczby zasobów ładowane z 50 do 100 po kliknięciu przycisku "Więcej obciążenia"
* Rozwiązano problem: Przy pierwszym uruchomieniu, jeśli konto jest zalogowany, możemy teraz wszystkie subskrypcje dla tego konta domyślnie wybierane

#### <a name="known-issues"></a>Znane problemy

* Ta wersja programu Storage Explorer nie działa w systemie Ubuntu 14.04
* Aby otworzyć wiele kart dla tego samego zasobu, stale klikaj tego samego zasobu. Kliknij przycisk na inny zasób i następnie wrócić, a następnie kliknij polecenie oryginalny zasób, aby otworzyć go ponownie w innej karcie
* Szybki dostęp działa tylko z elementami na podstawie subskrypcji. Zasoby lokalne lub dołączone za pomocą klucza lub tokenu sygnatury dostępu Współdzielonego nie są obsługiwane w tej wersji
* Może potrwać kilka sekund, aby przejść do zasobu docelowego, w zależności od liczby zasobów, możesz mieć szybki dostęp
* Posiadanie więcej niż 3 grup obiektów blob lub przekazywania w tym samym czasie plików może powodować błędy
* Dojść wyszukiwania przeszukiwanie około 50 000 węzłów — dzięki temu może mieć wpływ na wydajność lub może spowodować, że nieobsługiwany wyjątek

10/03/2016
### <a name="version-085"></a>Wersja 0.8.5

#### <a name="new"></a>Nowa

* Teraz użyć klucze SAS wygenerowany przez Portal, aby dołączyć do konta magazynu i zasoby

#### <a name="fixes"></a>Poprawki

* Poprawiono: sytuacja wyścigu podczas wyszukiwania czasami spowodowane węzłów innych niż rozwijania
* Naprawiono: "Użyj protokołu HTTP" nie działa w przypadku nawiązywania połączenia z kontami magazynu przy użyciu nazwy i klucza konta
* Naprawiono: Klucze sygnatur dostępu Współdzielonego (specjalnie wygenerowanych przez Portal tymi) zwracają błąd "ukośnikiem na końcu"
* Naprawiono: Importowanie tabeli problemów
    * Czasami zostały cofnięte klucza partycji i klucz wiersza
    * Nie można odczytać "wartości null" kluczy partycji

#### <a name="known-issues"></a>Znane problemy

* Obsługuje wyszukiwanie przeszukiwanie około 50 000 węzłów — dzięki temu może mieć wpływ na wydajność
* Usługa Azure Stack aktualnie nie obsługuje plików, więc próby rozwinięcia plików spowoduje wyświetlenie błędu

09/12/2016
### <a name="version-084"></a>Wersja 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Nowa

* Generowanie bezpośrednie linki do kont magazynu, kontenerów, kolejek, tabel lub udziały plików na potrzeby udostępniania i obsługuje łatwy dostęp do zasobów — Windows i Mac OS
* Wyszukaj kontenery obiektów blob, tabel, kolejek, udziały plików lub konta magazynu z pola wyszukiwania
* Teraz można pogrupować klauzul w Konstruktorze zapytań tabeli
* Zmień nazwę i kopiowania/wklejania kontenery obiektów blob, udziały plików, tabel, obiektów blob, foldery obiektów blob, plików i katalogów z wewnątrz dołączone sygnatury dostępu Współdzielonego konta i kontenery
* Zmiana nazwy i kopiowania obiektów blob kontenerów i udziałów plików teraz zachować, właściwości oraz metadanych

#### <a name="fixes"></a>Poprawki

* Poprawiono: nie można edytować jednostki z tabeli jeśli zawierają one właściwości logiczną lub binarny

#### <a name="known-issues"></a>Znane problemy

* Obsługuje wyszukiwanie przeszukiwanie około 50 000 węzłów — dzięki temu może mieć wpływ na wydajność

08/03/2016
### <a name="version-083"></a>Wersja 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Nowa

* Zmień nazwę kontenerów, tabel, udziały plików
* Ulepszony interfejs konstruktora zapytań
* Możliwości, aby zapisać i załadować zapytań
* Bezpośrednie linki do kont lub kontenery, kolejek, tabel magazynu lub udziałów, udostępniania i łatwe uzyskiwanie dostępu do zasobów plików (tylko Windows - macOS obsługuje już wkrótce!)
* Możliwość zarządzania i Konfiguruj reguły mechanizmu CORS

#### <a name="fixes"></a>Poprawki

* Naprawiono: Accounts Microsoft wymagają ponowne uwierzytelnianie co 8 – 12 godzin

#### <a name="known-issues"></a>Znane problemy

* Czasami interfejsu użytkownika może sprawiać wrażenie — maksymalizacja okna pomaga rozwiązać ten problem
* Zainstaluj system macOS może wymagać podwyższonym poziomem uprawnień
* Panel ustawień konta mogą być wyświetlane, należy ponownie wprowadzić poświadczenia w celu filtrowania subskrypcji
* Zmiana nazwy udziałów plików, kontenery obiektów blob i tabel nie pozwala zachować metadane lub innych właściwości w kontenerze, takie jak limit przydziału udziału plików, poziom dostępu publicznego lub zasady dostępu
* Zmiana nazwy obiektów blob (pojedynczo lub w kontenerze obiektów blob zmieniono nazwę) nie zostaną zachowane migawki. Wszystkie właściwości i metadanych obiektów blob, plików oraz jednostki są zachowywane podczas zmiany nazwy
* Kopiowanie lub zmiana nazwy zasobów nie działa w ramach dołączone sygnatury dostępu Współdzielonego konta

07/07/2016
### <a name="version-082"></a>Wersja 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Nowa

* Konta magazynu są pogrupowane według subskrypcji; Tworzenie magazynu i zasoby dołączone za pomocą sygnatury dostępu Współdzielonego lub klucza są wyświetlane w węźle (lokalne i dołączone)
* Wylogowanie z konta w panelu "Ustawienia konta platformy Azure"
* Skonfiguruj ustawienia serwera proxy, aby włączyć i zarządzać logowania
* Tworzenie i przerwać dzierżawy obiektu blob
* Kontenery Otwórz obiektów blob, kolejek, tabel i plików za pomocą jednego kliknięcia

#### <a name="fixes"></a>Poprawki

* Poprawiono: komunikatów w kolejce wstawiony z bibliotekami .NET lub Java są nie prawidłowo zdekodować z formatu base64
* Naprawiono: $metrics tabele nie są wyświetlane dla kont usługi Blob Storage
* Naprawiono: węzeł tabele nie działa w przypadku lokalnego magazynu (Programowanie)

#### <a name="known-issues"></a>Znane problemy

* Zainstaluj system macOS może wymagać podwyższonym poziomem uprawnień

06/15/2016
### <a name="version-080"></a>Wersja 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Nowa

* Obsługa udziału plików: przeglądania, przekazywania, pobierania, kopiowania plików i katalogów, identyfikatorów URI sygnatury dostępu Współdzielonego (Tworzenie i łączenie)
* Ulepszone środowisko użytkownika dotyczące łączenia za pomocą kluczy identyfikatorów URI sygnatury dostępu Współdzielonego lub konta magazynu
* Eksportowanie wyników zapytania tabeli
* Zmienianie kolejności kolumn w tabeli i dostosowywania
* Wyświetlanie $logs kontenery obiektów blob i tabel $metrics dla kont magazynu przy użyciu metryk włączone
* Ulepszone eksportowania i importowania zachowanie, zawiera teraz typ wartości właściwości

#### <a name="fixes"></a>Poprawki

* Poprawiono: przekazywanie lub pobieranie dużych obiektów blob może spowodować niekompletne przekazywanie/pobieranie
* Naprawiono: edytowanie, dodanie lub zaimportowanie jednostki z wartością ciągu numerycznego ("1") przekonwertuje go na double
* Poprawiono: Nie można rozwinąć węzła tabeli w lokalne Środowisko deweloperskie

#### <a name="known-issues"></a>Znane problemy

* $metrics tabele nie są widoczne dla kont usługi Blob Storage
* Kolejka komunikatów dodane programowo mogą nie być wyświetlane prawidłowo, jeśli komunikaty są zakodowane w formacie Base64

05/17/2016
### <a name="version-07201605090"></a>Wersja 0.7.20160509.0

#### <a name="new"></a>Nowa

* Awarie lepszą obsługę błędów dla aplikacji

#### <a name="fixes"></a>Poprawki

* Naprawiono usterkę, gdzie komunikaty o przerwaniu pracy czasami nie są one wyświetlane podczas należało poświadczeń logowania

#### <a name="known-issues"></a>Znane problemy

* Tabele: Dodawanie, edytowanie lub importowanie jednostki, która ma właściwość z wartością ambiguously liczbowe, takie jak "1" lub "1.0", a użytkownik próbuje wysłać go jako `Edm.String`, wartość będzie Wróć za pośrednictwem interfejsu API klienta jako Edm.Double

03/31/2016

### <a name="version-07201603250"></a>Wersja 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Nowa

* Tabela obsługi: wyświetlanie, wyszukiwanie, export, import i operacji CRUD dla jednostek
* Kolejki pomocy technicznej: przeglądanie, dodawanie dequeueing wiadomości
* Identyfikatorów URI dla kont magazynu
* Łączenie się z kontami magazynu przy użyciu identyfikatorów URI sygnatury dostępu Współdzielonego
* Powiadomienia o aktualizacji dla przyszłych aktualizacji do programu Storage Explorer
* Zaktualizowany wygląd i działanie

#### <a name="fixes"></a>Poprawki

* Ulepszenia wydajności i niezawodności

### <a name="known-issues-amp-mitigations"></a>Znane problemy dotyczące &amp; środki zaradcze

* Pobieranie obiektu blob w dużych plików nie działa prawidłowo — firma Microsoft zaleca używanie narzędzia AzCopy, podczas gdy ten problem można rozwiązać
* Poświadczenia konta nie zostaną pobrane ani pamięci podręcznej, jeśli nie można odnaleźć folderu głównego, lub nie można zapisać
* Jeśli firma Microsoft jest dodawanie, edytowanie lub importowanie jednostki, która ma właściwość z wartością ambiguously liczbowe, takie jak "1" lub "1.0", a użytkownik próbuje wysłać go jako `Edm.String`, wartość będzie Wróć za pośrednictwem interfejsu API klienta jako Edm.Double
* Podczas importowania plików CSV z rekordami wielowierszowy, dane mogą uzyskać pociąć lub zaszyfrowane

02/03/2016

### <a name="version-07201601291"></a>Wersja 0.7.20160129.1

#### <a name="fixes"></a>Poprawki

* Lepsza ogólna wydajność, gdy przekazywanie, pobieranie i kopiowanie obiektów blob

01/14/2016

### <a name="version-07201601050"></a>Wersja 0.7.20160105.0

#### <a name="new"></a>Nowa

* Pomoc techniczna Linux support (równoważności funkcji OSX)
* Dodaj kontenery obiektów blob za pomocą klucza sygnatury dostępu współdzielonego (SAS)
* Dodawanie konta usługi Storage dla platformy Azure (Chiny)
* Dodawanie kont magazynu przy użyciu niestandardowych punktów końcowych
* Otwieranie i wyświetlanie obiektów blob tekst i obraz zawartości
* Wyświetlanie i edytowanie właściwości obiektu blob i metadanych

#### <a name="fixes"></a>Poprawki

* Poprawiono: przekazywanie lub pobieranie dużej liczby obiektów blob (500 i nowsze) może czasami powodują, że aplikację, aby mieć biały ekran
* Poprawiono: podczas ustawiania poziom dostępu publicznego kontenera obiektów blob, nowa wartość nie jest aktualizowana do momentu ponownie ustaw fokus w kontenerze. Ponadto okno dialogowe zawsze wartość domyślna to "Nie publicznego dostępu", a nie rzeczywiste wartości bieżącej.
* Obsługa lepszej ogólnej klawiatury/ułatwień dostępu i interfejsu użytkownika
* Linki do stron nadrzędnych historii zawijany, gdy jest za długa białymi znakami
* Okno dialogowe sygnatury dostępu Współdzielonego obsługuje walidacji danych wejściowych
* Magazyn lokalny jest nadal dostępne, nawet jeśli wygasły poświadczenia użytkownika
* Po usunięciu kontenera obiektów blob otwarty Eksplorator obiektów blob po prawej stronie jest zamknięty.

#### <a name="known-issues"></a>Znane problemy

* Instalacja systemu Linux wymaga wersji kompilatora gcc zaktualizowane lub uaktualnione — kroki niezbędne do uaktualnienia są poniżej:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Wersja 0.7.20151116.0

#### <a name="new"></a>Nowa

* System macOS i Windows wersje
* Zaloguj się do wyświetlić konta magazynu — Użyj konta organizacji, Account Microsoft, funkcji 2FA itp.
* Lokalnym magazynem projektowym (Używanie emulatora magazynu tylko do Windows)
* Obsługa zasobów usługi Azure Resource Manager i Model Klasyczny
* Tworzenie i usuwanie obiektów blob, kolejek i tabel
* Wyszukiwanie konkretnych obiektów blob, kolejek i tabel
* Przejrzyj zawartość kontenerów obiektów blob
* Wyświetlać i przechodzić między katalogami
* Przekazywanie, pobieranie i usuwanie obiektów blob oraz folderów
* Wyświetlanie i edytowanie właściwości obiektu blob i metadanych
* Generowanie kluczy sygnatury dostępu Współdzielonego
* Zarządzanie i tworzenie przechowywanych zasad dostępu (SAP, Distributed File System)
* Wyszukaj obiekty BLOB według prefiksu
* Przeciągnij n umieszczają pliki w celu przekazywania lub pobierania

#### <a name="known-issues"></a>Znane problemy

* Kiedy ustawienie poziomie dostępu publicznego do kontenera obiektów blob, nowa wartość nie jest aktualizowana, dopóki ponownie ustaw fokus w kontenerze
* Podczas otwierania okna dialogowego, aby ustawić poziom dostępu publicznego go zawsze jest wyświetlany "Nie publicznego dostępu" jako domyślne, a nie rzeczywiste bieżącą wartość
* Nie można zmienić nazwy pobrany obiektów blob
* Wpisy dziennika aktywności będą czasami "zatrzymywane" w toku w stanie, gdy wystąpi błąd i nie jest wyświetlany błąd
* Czasami zawiesza się lub wyłącza całkowicie biały, podczas próby przekazywanie lub pobieranie dużej liczby obiektów blob
* Czasami anulowanie operacji kopiowania nie działa
* Podczas tworzenia kontenera (obiekt blob/kolejki/table), jeśli dane wejściowe nieprawidłową nazwę i kontynuować tworzenie drugiego, w ramach typu innego kontenera nie można ustawić fokus na nowy typ
* Nie można utworzyć nowego folderu lub zmień nazwę folderu




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
