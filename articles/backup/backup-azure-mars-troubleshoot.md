---
title: Rozwiązywanie problemów ze składnikiem Azure Backup Agent
description: Rozwiązywanie problemów z instalowaniem i rejestrowaniem agenta usługi Azure Backup
services: backup
author: saurabhsensharma
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 7/25/2018
ms.author: saurse
ms.openlocfilehash: 2c8978cfba8fc56d4dbc565cb3a91c75d9d54679
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700199"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent-issues"></a>Rozwiązywanie problemów dotyczących agenta usługi Microsoft Azure Recovery Services (MARS)
## <a name="recommended-steps"></a>Zalecane czynności
Zapoznaj się z tym artykułem, aby naprawić błędy podczas konfiguracji, rejestracji, kopię zapasową i przywrócić dane przy użyciu agenta usług MARS.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Podano nieprawidłowe poświadczenia magazynu. Plik jest uszkodzony lub jest nie ma najnowszych poświadczeń skojarzonych z usługą odzyskiwania.
| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
| ---     | ---     | ---    |
| **Error** </br> *Podano nieprawidłowe poświadczenia magazynu. Plik jest uszkodzony lub jest nie ma najnowszych poświadczeń skojarzonych z usługą odzyskiwania. (ID: 34513)* | <ul><li> Poświadczenia magazynu są nieprawidłowe (oznacza to, że zostały pobrane przed upływem terminu rejestracji więcej niż 48 godzin).<li>Agenta usług MARS nie może pobrać pliki do katalogu Windows Temp. <li>Poświadczenia magazynu znajdują się w lokalizacji sieciowej. <li>Protokół TLS 1.0 jest wyłączony.<li> Skonfigurowany serwer proxy blokuje połączenia. <br> |  <ul><li>Pobierz nowe poświadczenia magazynu.<li>Przejdź do **Opcje internetowe** > **zabezpieczeń** > **Internet**. Następnie wybierz pozycję **Poziom niestandardowy**i przewijanie, aż zostanie wyświetlony plik, Pobierz sekcji. Następnie wybierz pozycję **Włącz**.<li>Może być również konieczne Dodaj witrynę do [Zaufane witryny](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Zmień ustawienia, aby używać serwera proxy. Następnie szczegółowo Serwer proxy serwera. <li> Zgodna daty i godziny z komputera.<li>Przejdź do C:/Windows/Temp, a następnie sprawdź, czy istnieją więcej niż 60 000 lub 65 000 pliki z rozszerzeniem .tmp. Jeśli istnieją, należy usunąć te pliki.<li>Możesz zweryfikować tego pakietu SDP uruchomiona na serwerze. Jeśli otrzymasz komunikat o błędzie informujący, pobierania plików nie są dozwolone, jest prawdopodobne, że istnieją dużej liczby plików w katalogu C:/Windows/Temp.<li>Upewnij się, że masz zainstalowane środowisko .NET framework 4.6.2. <li>Jeśli protokół TLS 1.0 zostało wyłączone z powodu zgodności ze standardami PCI, zapoznaj się z tym [strona rozwiązywania problemów](https://support.microsoft.com/help/4022913). <li>Jeśli masz oprogramowanie antywirusowe zainstalowane na serwerze, należy wyłączyć następujące pliki ze skanowania antywirusowego: <ul><li>CBengine.exe<li>CSC.exe, który jest powiązany z .NET Framework. Brak CSC.exe dla każdej wersji platformy .NET, który jest zainstalowany na serwerze. Wyklucz pliki CSC.exe, które są powiązane z wszystkich wersji programu .NET framework na tym serwerze. <li>Lokalizacja folderu lub pamięci podręcznej plików tymczasowych. <br>*Domyślna lokalizacja dla tymczasowy folder lub ścieżka lokalizacji pamięci podręcznej to C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Agent usług Microsoft Azure Recovery Service nie może nawiązać połączenia z programem Kopia zapasowa Microsoft Azure

| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
| ---     | ---     | ---    |
| **Error** </br><ol><li>*Agent usług Microsoft Azure Recovery Service nie może nawiązać połączenia z programem Kopia zapasowa Microsoft Azure. (IDENTYFIKATOR: 100050) Sprawdź ustawienia sieci i upewnij się, że jesteś w stanie połączyć się z Internetem*<li>*Wymagane uwierzytelnianie serwera proxy (407)* |Serwer proxy blokuje połączenia. |  <ul><li>Przejdź do **Opcje internetowe** > **zabezpieczeń** > **Internet**. Następnie wybierz pozycję **Poziom niestandardowy** i przewijanie, aż zostanie wyświetlony plik, Pobierz sekcji. Wybierz pozycję **Włącz**.<li>Może być również konieczne Dodaj witrynę do [Zaufane witryny](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Zmień ustawienia, aby używać serwera proxy. Następnie szczegółowo Serwer proxy serwera. <li>Jeśli masz oprogramowanie antywirusowe zainstalowane na serwerze, Wyklucz następujące pliki ze skanowania oprogramowania antywirusowego. <ul><li>CBEngine.exe (zamiast dpmra.exe).<li>CSC.exe (powiązanego z .NET Framework). Brak CSC.exe dla każdej wersji platformy .NET, który jest zainstalowany na serwerze. Wyklucz pliki CSC.exe, które są powiązane z wszystkich wersji programu .NET framework na tym serwerze. <li>Lokalizacja folderu lub pamięci podręcznej plików tymczasowych. <br>*Domyślna lokalizacja dla tymczasowy folder lub ścieżka lokalizacji pamięci podręcznej to C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Nie można ustawić klucza szyfrowania na potrzeby bezpiecznych kopii zapasowych

| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
| ---     | ---     | ---    |      
| **Error** </br>*Nie można ustawić klucza szyfrowania dla bezpiecznych kopii zapasowych aktywacji nie powiodło się całkowicie, ale hasło szyfrowania został zapisany w następującym pliku*. |<li>Serwer jest już zarejestrowany w innym magazynie.<li>Podczas konfigurowania hasła jest uszkodzony| Wyrejestruj serwer z magazynu i ponownie zarejestrować się za pomocą nowego hasła.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Aktywacja nie została ukończona pomyślnie. Bieżąca operacja nie powiodła się z powodu wewnętrznego błędu usługi [0x1FC07]

| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
|---------|---------|---------|
|**Error** </br><ol>*Aktywacja nie została pomyślnie ukończona. Bieżąca operacja nie powiodła się z powodu wewnętrznego błędu usługi [0x1FC07]. Spróbuj ponownie wykonać operację po pewnym czasie. Jeśli problem będzie się powtarzać, skontaktuj się z pomocą techniczną firmy Microsoft*     | <li> Folder tymczasowy znajduje się na woluminie, który ma za mało miejsca. <li> Folder tymczasowy został niepoprawnie przeniesiony do innej lokalizacji. <li> Brakuje pliku OnlineBackup.KEK.         | <li>Uaktualnianie do [najnowszej wersji](http://aka.ms/azurebackup_agent) agenta usług MARS.<li>Przenieś pliki tymczasowe lokalizacja folderu lub pamięci podręcznej do woluminu z ilością wolnego miejsca równa 5 – 10% całkowitego rozmiaru danych kopii zapasowej. Aby poprawnie przenieść lokalizację pamięci podręcznej, skorzystaj z procedury opisanej w [pytania dotyczące agenta usługi Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Upewnij się, że plik OnlineBackup.KEK jest obecny. <br>*Domyślna lokalizacja dla tymczasowy folder lub ścieżka lokalizacji pamięci podręcznej to C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="error-34506-the-encryption-passphrase-stored-on-this-computer-is-not-correctly-configured"></a>Błąd 34506. Hasło szyfrowania zapisane na tym komputerze nie jest prawidłowo skonfigurowane.

| Szczegóły błędu | Możliwe przyczyny | Zalecane akcje |
|---------|---------|---------|
|**Error** </br><ol>*Błąd 34506. Hasło szyfrowania zapisane na tym komputerze nie jest prawidłowo skonfigurowane*.    | <li> Folder tymczasowy znajduje się na woluminie, który ma za mało miejsca. <li> Folder tymczasowy został niepoprawnie przeniesiony do innej lokalizacji. <li> Brakuje pliku OnlineBackup.KEK.        | <li>Uaktualnianie do [najnowszej wersji](http://aka.ms/azurebackup_agent) agenta usług MARS.<li>Przenieś folder tymczasowy lub lokalizację pamięci podręcznej do woluminu z wolne miejsce odpowiadające 5 – 10% całkowitego rozmiaru danych kopii zapasowej. Aby poprawnie przenieść lokalizację pamięci podręcznej, skorzystaj z procedury opisanej w [pytania dotyczące agenta usługi Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Upewnij się, że plik OnlineBackup.KEK jest obecny. <br>*Domyślna lokalizacja dla tymczasowy folder lub ścieżka lokalizacji pamięci podręcznej to C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.         |

## <a name="backups-do-not-run-as-per-schedule"></a>Kopie zapasowe nie są uruchamiane zgodnie z harmonogramem
Wykonaj następujące czynności, jeśli zaplanowane kopie zapasowe nie wyzwalane automatycznie, podczas ręcznego tworzenia kopii zapasowych działać bez problemów. 
 
<li>Upewnij się, że program PowerShell 3.0 lub nowszej jest zainstalowane na serwerze. Aby sprawdzić wersję programu PowerShell, uruchom następujące polecenie i upewnij się, że *głównych* numer wersji jest równy lub większy niż 3.

`$PSVersionTable.PSVersion`
<li>Upewnij się, że następująca ścieżka jest częścią *PSMODULEPATH* zmiennej środowiskowej.

`<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`
<li>Jeśli zasady wykonywania programu powershell dla *LocalMachine* jest ustawiony na ograniczone, polecenia cmdlet programu powershell, która powoduje uruchomienie zadania tworzenia kopii zapasowej może zakończyć się niepowodzeniem. Wykonaj następujące polecenia w trybie podniesionych uprawnień, aby sprawdzić i ustawić zasady wykonywania na albo *Unrestricted* lub *RemoteSigned*

`PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

`PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`
<li>Wybierz kolejno Panel sterowania -> Narzędzia administracyjne -> Harmonogram zadań -> Rozwiń Microsoft -> Wybierz opcję tworzenia kopii zapasowej Online
<li>Kliknij dwukrotnie zadania "Microsoft OnlineBackup", a następnie przejdź do karty "Wyzwalaczy".
<li>Upewnij się, że "Status" zadanie jest ustawiona na wartość "Włączony". W przeciwnym razie kliknij pozycję "Edytuj" i zaznacz pole wyboru "Enabled"
<li>Przejdź do *opcje zabezpieczeń* części *ogólne* kartę
<li>Upewnij się, że konto użytkownika, wybrany do uruchomienia tego zadania jest *systemu* lub grupy administratorów lokalnych na serwerze

> [!TIP]
> Zalecane jest ponowne uruchomienie serwera po wykonaniu kroków powyżej, aby upewnić się, że zmiany są stosowane spójnie


## <a name="troubleshooting-restore-issues"></a>Rozwiązywanie problemów przywracania

### <a name="failure-to-mount-the-recovery-volume-during-recovery-of-files-and-folders"></a>Nie można zainstalować woluminu odzyskiwania podczas odzyskiwania plików i folderów
Jeśli usługa Azure Backup nie pomyślnie zainstalować woluminu odzyskiwania, nawet po kilku minutach kliknięcie **instalacji** lub kończy się niepowodzeniem, można zainstalować woluminu odzyskiwania z co najmniej jeden błąd, wykonaj poniższe kroki, aby odzyskiwać normalnie.

1.  Anuluj proces ciągłego instalacji, w przypadku, gdy została uruchomiona przez kilka minut.

2.  Upewnij się, że korzystasz z najnowszej wersji agenta usługi Azure Backup. Aby dowiedzieć się, informacje o wersji agenta usługi Azure Backup, kliknij **o Recovery Agent usług Microsoft Azure** na **akcje** okienko kopia zapasowa Microsoft Azure konsoli i upewnij się, że **Wersji** liczba jest większa lub równa wyższa niż wersja, o których wspomniano w [w tym artykule](https://go.microsoft.com/fwlink/?linkid=229525). Możesz pobrać najnowszą wersję z [tutaj](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Przejdź do **Menedżera urządzeń** -> **kontrolery magazynu** i upewnij się, czy można zlokalizować **inicjatora iSCSI firmy Microsoft**. Jeśli możesz go znaleźć, bezpośrednio przejść do kroku 7 poniżej. 

4.  Jeśli nie można zlokalizować Usługa inicjatora iSCSI firmy Microsoft, jak wspomniano wcześniej w kroku 3, sprawdź, czy możesz znaleźć pozycji w obszarze **Menedżera urządzeń** -> **kontrolery magazynu** o nazwie  **Nieznane urządzenie** przy użyciu Identyfikatora sprzętu **ROOT\ISCSIPRT**.

5.  Kliknij prawym przyciskiem myszy **nieznane urządzenie** i wybierz **aktualizacji sterowników**.

6.  Aktualizacja sterownika, wybierając opcję **wyszukiwanie automatycznie oprogramowania zaktualizowanym sterowniku**. Zakończenie aktualizacji należy zmienić **nieznane urządzenie** do **inicjatora iSCSI firmy Microsoft** jak pokazano poniżej. 

    ![Szyfrowanie](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Przejdź do **Menedżera zadań** -> **usługi (lokalne)** -> **Usługa inicjatora iSCSI firmy Microsoft**. 

    ![Szyfrowanie](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Uruchom ponownie usługę inicjatora iSCSI firmy Microsoft, kliknięcie prawym przyciskiem myszy usługę, klikając polecenie zatrzymania i dalsze kliknij prawym przyciskiem myszy ponownie i klikając **Start**.

9.  Ponów próbę odzyskiwania za pomocą natychmiastowe przywracanie. 

Jeśli odzyskiwanie nadal kończy się niepowodzeniem, ponowny rozruch serwera/klienta. Jeśli ponowne uruchomienie komputera nie jest pożądane lub odzyskiwania nadal kończy się niepowodzeniem nawet po zakończeniu ponownego uruchomienia serwera, spróbuj wykonać odzyskiwanie z alternatywnej maszyny, wykonując kroki wymienione w [w tym artykule](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Kontakt z pomocą techniczną
Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) można szybko rozwiązać swój problem.

## <a name="next-steps"></a>Kolejne kroki
* Dowiedz się więcej [sposób wykonywania kopii zapasowej systemu Windows Server przy użyciu agenta usługi Azure Backup](tutorial-backup-windows-server-to-azure.md).
* Jeśli chcesz przywrócić kopię zapasową, w tym artykule znajdziesz informacje dotyczące [przywracania plików na maszynę z systemem Windows](backup-azure-restore-windows-server.md).
