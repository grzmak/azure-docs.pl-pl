---
title: Usługa Azure Site Recovery Rozwiązywanie problemów z błędów i problemów z replikacją Azure – Azure | Dokumentacja firmy Microsoft
description: Rozwiązywanie problemów z błędów i problemów podczas replikowania maszyn wirtualnych platformy Azure w celu odzyskiwania po awarii
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: sujayt
ms.openlocfilehash: 86d6c77dab817cf755c34bdd699ee1158e852f37
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42060200"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Rozwiązywanie problemów z replikacją maszyny Wirtualnej platformy Azure do platformy Azure

W tym artykule opisano, jakie są najczęstsze problemy w usłudze Azure Site Recovery podczas replikacji i odzyskiwania maszyn wirtualnych platformy Azure z jednego regionu do innego regionu i wyjaśniono, jak rozwiązywać problemy z nimi. Aby uzyskać więcej informacji o obsługiwanych konfiguracjach, zobacz [macierz obsługi do replikowania maszyn wirtualnych platformy Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemy z przydziałami zasobów platformy Azure (kod błędu: 150097)
Twoja subskrypcja powinna być włączona w celu tworzenia maszyn wirtualnych platformy Azure w regionie docelowym, który ma być używany jako swój region odzyskiwania po awarii. Ponadto subskrypcji powinny mieć wystarczającego limitu przydziału, włączone, aby tworzyć maszyny wirtualne o określonym rozmiarze. Domyślnie usługa Site Recovery wybiera ten sam rozmiar dla docelowej maszyny Wirtualnej, co źródłowa maszyna wirtualna. Jeśli dopasowania rozmiaru nie jest dostępna, najbardziej zbliżony rozmiar jest pobierany automatycznie. W przypadku nie pasującego rozmiar, który obsługuje konfiguracji źródłowej maszyny Wirtualnej, zostanie wyświetlony ten komunikat o błędzie:

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenie**
--- | --- | ---
150097<br></br>**Komunikat**: nie można włączyć replikacji dla maszyny wirtualnej VmName. | — Identyfikator subskrypcji nie może być włączona do tworzenia maszyn wirtualnych w lokalizacji docelowej w regionie.</br></br>— Identyfikator subskrypcji, może nie być włączone lub nie ma wystarczającego limitu przydziału do tworzenia określonych rozmiarów maszyn wirtualnych w lokalizacji docelowej w regionie.</br></br>-Odpowiedni rozmiar docelowej maszyny Wirtualnej pasujący źródłowej maszyny Wirtualnej karty Sieciowej, liczba (2) nie zostanie odnaleziony dla Identyfikatora subskrypcji w lokalizacji docelowej w regionie.| Skontaktuj się z pomocą [Azure pomocy technicznej dotyczącej rozliczeń](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) umożliwiające tworzenia maszyn wirtualnych o wymaganych rozmiarach w lokalizacji docelowej dla Twojej subskrypcji. Po włączeniu ponów próbę wykonania operacji zakończonej niepowodzeniem.

### <a name="fix-the-problem"></a>Rozwiąż problem
Możesz skontaktować się ze [Azure pomocy technicznej dotyczącej rozliczeń](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) włączenie subskrypcji do tworzenia maszyn wirtualnych o wymaganych rozmiarach w lokalizacji docelowej.

Jeśli lokalizacja docelowa ma ograniczenie pojemności, wyłącz replikację i włącz go w innej lokalizacji, w którym Twoja subskrypcja ma wystarczający przydział do tworzenia maszyn wirtualnych o wymaganych rozmiarach.

## <a name="trusted-root-certificates-error-code-151066"></a>Zaufane certyfikaty główne (kod błędu: 151066)

Jeśli wszystkie najnowsze certyfikaty zaufany główny urząd certyfikacji nie istnieje na maszynie Wirtualnej, zadanie "Włącz replikację" może zakończyć się niepowodzeniem. Bez certyfikatów uwierzytelniania i autoryzacji, wywołań usługi Site Recovery z maszyny Wirtualnej zakończyć się niepowodzeniem. Zostanie wyświetlony komunikat o błędzie o zadaniu zakończonym niepowodzeniem "Włącz replikację" Site Recovery:

**Kod błędu:** | **Możliwa przyczyna** | **Zalecenia**
--- | --- | ---
151066<br></br>**Komunikat**: Konfiguracja Site Recovery nie powiodła się. | Wymagane zaufane certyfikaty główne używane na potrzeby autoryzacji i uwierzytelniania nie są dostępne na komputerze. | — Dla maszyny Wirtualnej z systemem operacyjnym Windows upewnij się, że zaufane certyfikaty główne na maszynie. Aby uzyskać informacje, zobacz [Konfigurowanie zaufanych certyfikatów głównych i certyfikatów niedozwolonych](https://technet.microsoft.com/library/dn265983.aspx).<br></br>— Dla maszyny Wirtualnej z systemem operacyjnym Linux postępuj zgodnie z wytycznymi dla zaufanych certyfikatów głównych opublikowanymi przez dystrybutora wersji systemu operacyjnego Linux.

### <a name="fix-the-problem"></a>Rozwiąż problem
**Windows**

Zainstalowanie wszystkich najnowszych aktualizacji Windows na maszynie Wirtualnej, tak aby wszystkie zaufane certyfikaty główne są obecne na maszynie. Jeśli pracujesz w środowisku bez połączenia, postępuj zgodnie z standardowego procesu aktualizacji Windows w Twojej organizacji do uzyskania certyfikatów. Jeśli na maszynie Wirtualnej nie są wymagane certyfikaty, wywołań do usługi Site Recovery zakończyć się niepowodzeniem ze względu na bezpieczeństwo.

Postępuj zgodnie z typowe Windows update management lub certyfikat proces zarządzania aktualizacjami w organizacji, aby pobrać wszystkie najnowsze certyfikaty główne i listę odwołania certyfikatów zaktualizowane na maszynach wirtualnych.

Aby sprawdzić, czy problem został rozwiązany, przejdź do login.microsoftonline.com za pomocą przeglądarki na maszynie wirtualnej.

**Linux**

Postępuj zgodnie ze wskazówkami dystrybutora systemu Linux, aby uzyskać najnowsze zaufane certyfikaty główne i najnowsze listy odwołania certyfikatów na maszynie Wirtualnej.

Ponieważ SuSE Linux używa łączy symbolicznych, aby utrzymywać listę certyfikatów, wykonaj następujące kroki:

1.  Zaloguj się jako użytkownik root.

2.  Uruchom następujące polecenie, aby zmienić katalog.

      ``# cd /etc/ssl/certs``

3. Sprawdź, czy certyfikat urzędu certyfikacji firmy Symantec główny jest obecny.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Jeśli certyfikat urzędu certyfikacji firmy Symantec główny nie zostanie znaleziony, uruchom następujące polecenie, aby pobrać plik. Sprawdź pod kątem błędów, a następnie postępuj zgodnie z zalecaną akcję dotyczącą awarie sieci.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Sprawdź, czy urząd certyfikacji certyfikatu głównego Baltimore jest obecny.

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Jeśli certyfikat głównego urzędu certyfikacji Baltimore nie zostanie znaleziony, Pobierz certyfikat.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. Sprawdź, czy certyfikatu DigiCert_Global_Root_CA jest obecny.

    ``# ls DigiCert_Global_Root_CA.pem``

8. Jeśli DigiCert_Global_Root_CA nie zostanie znaleziony, uruchom następujące polecenia, aby pobrać certyfikat.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Uruchom rehash — skrypt, aby zaktualizować certyfikatu podmiotu skróty dla nowo pobranego certyfikatów.

    ``# c_rehash``

10. Sprawdź, jeśli temat wyznacza wartość skrótu utworzonemu łączy symbolicznych dla certyfikatów.

    - Polecenie

      ``# ls -l | grep Baltimore``

    - Dane wyjściowe

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Polecenie

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Dane wyjściowe

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Polecenie

      ``# ls -l | grep DigiCert_Global_Root``

    - Dane wyjściowe

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

11. Tworzenie kopii pliku VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem za pomocą b204d74a.0 nazwy pliku

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. Tworzenie kopii pliku Baltimore_CyberTrust_Root.pem za pomocą 653b494a.0 nazwy pliku

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Tworzenie kopii pliku DigiCert_Global_Root_CA.pem za pomocą 3513523f.0 nazwy pliku

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Sprawdź, czy pliki są obecne.  

    - Polecenie

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Dane wyjściowe

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Połączenia ruchu wychodzącego dla zakresów adresów URL Site Recovery lub adres IP (kod błędu: 151037 lub 151072)

W przypadku replikacji usługi Site Recovery do pracy, łączność wychodząca z określonych adresów URL lub IP zakresów jest wymagane z maszyny Wirtualnej. Jeśli maszyna wirtualna znajduje się za zaporą lub używa reguł Sieciowej grupy zabezpieczeń sieci do sterowania ruchem wychodzącym, może być jedną z tych problemów twarzy.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151037-br"></a>Problem 1: Nie można zarejestrować maszyny wirtualnej platformy Azure z usługą Site Recovery (151037) </br>
- **Możliwa przyczyna** </br>
  - Używasz sieciowej grupy zabezpieczeń do kontrolowania dostępu wychodzącego na maszynie Wirtualnej i wymagany adres IP nie są na liście dozwolonych dla wychodzącego dostępu do zakresów.
  - Używasz narzędzi zapory innych firm, a nie są wymagane zakresy adresów IP/URL na liście dozwolonych.


- **Rozdzielczość**
   - Jeśli używasz serwera proxy zapory do kontrolowania połączenia sieciowego ruchu wychodzącego na maszynie Wirtualnej, upewnij się, że wstępnie wymagane adresy URL lub zakresy IP centrów danych na liście dozwolonych. Aby uzyskać informacje, zobacz [wskazówki dotyczące serwera proxy zapory](https://aka.ms/a2a-firewall-proxy-guidance).
   - Jeśli używasz reguł sieciowych grup zabezpieczeń do sterowania połączenia sieciowego ruchu wychodzącego na maszynie Wirtualnej, upewnij się, że zakresy adresów IP centrum danych wymagań wstępnych na liście dozwolonych. Aby uzyskać informacje, zobacz [wytyczne dotyczące grupy zabezpieczeń sieci](https://aka.ms/a2a-nsg-guidance).
   - Do listy dozwolonych [odpowiednie adresy URL](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) lub [wymaganych zakresów adresów IP](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), postępuj zgodnie z instrukcjami w [sieci dokument ze wskazówkami dotyczącymi](site-recovery-azure-to-azure-networking-guidance.md).

### <a name="issue-2-site-recovery-configuration-failed-151072"></a>Problem 2: Konfiguracja Site Recovery nie powiodła się (151072)
- **Możliwa przyczyna** </br>
  - Nie można nawiązać połączenia z punktami końcowymi usługi Site Recovery


- **Rozdzielczość**
   - Jeśli używasz serwera proxy zapory do kontrolowania połączenia sieciowego ruchu wychodzącego na maszynie Wirtualnej, upewnij się, że wstępnie wymagane adresy URL lub zakresy IP centrów danych na liście dozwolonych. Aby uzyskać informacje, zobacz [wskazówki dotyczące serwera proxy zapory](https://aka.ms/a2a-firewall-proxy-guidance).
   - Jeśli używasz reguł sieciowych grup zabezpieczeń do sterowania połączenia sieciowego ruchu wychodzącego na maszynie Wirtualnej, upewnij się, że zakresy adresów IP centrum danych wymagań wstępnych na liście dozwolonych. Aby uzyskać informacje, zobacz [wytyczne dotyczące grupy zabezpieczeń sieci](https://aka.ms/a2a-nsg-guidance).
   - Do listy dozwolonych [odpowiednie adresy URL](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) lub [wymaganych zakresów adresów IP](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), postępuj zgodnie z instrukcjami w [sieci dokument ze wskazówkami dotyczącymi](site-recovery-azure-to-azure-networking-guidance.md).

### <a name="issue-3-a2a-replication-failed-when-the-network-traffic-goes-through-on-premise-proxy-server-151072"></a>Problem 3: Nie powiodło się, gdy ruch sieciowy przechodzi przez serwer proxy w środowisku lokalnym (151072) replikacji — A2A
 - **Możliwa przyczyna** </br>
   - Ustawienia niestandardowego serwera proxy są nieprawidłowe i agenta usługi mobilności dla usługi ASR nie auto wykrył ustawienia serwera proxy z programu Internet Explorer


 - **Rozdzielczość**
  1.    Agent usługi mobilności wykrywa ustawienia serwera proxy programu Internet Explorer na Windows i /etc/environment w systemie Linux.
  2.  Jeśli wolisz skonfigurować serwer proxy tylko w przypadku usługi mobilności dla usługi ASR, można podać szczegóły serwera proxy w ProxyInfo.conf znajdujących się na:</br>
      - ``/usr/local/InMage/config/`` na ***systemu Linux***
      - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` na ***Windows***
  3.    ProxyInfo.conf powinna mieć ustawienia serwera proxy w następującym formacie pliku INI. </br>
                   *[proxy]*</br>
                   *Adres =http://1.2.3.4*</br>
                   *Port = 567*</br>
  4. Agent usługi mobilności dla usługi ASR obsługuje tylko ***nieuwierzytelnione serwery proxy***.

### <a name="fix-the-problem"></a>Rozwiąż problem
Do listy dozwolonych [odpowiednie adresy URL](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) lub [wymaganych zakresów adresów IP](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), postępuj zgodnie z instrukcjami w [sieci dokument ze wskazówkami dotyczącymi](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Nie znaleziono maszyny (kod błędu: 150039) dysku

Nowy dysk dołączony do maszyny Wirtualnej musi być zainicjowany.

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia**
--- | --- | ---
150039<br></br>**Komunikat**: dysk danych platformy Azure (DiskName) (DiskURI) przy użyciu numeru jednostki logicznej (LUN) (LUNValue) nie został zamapowany do odpowiadającego dysku zgłaszanego z poziomu maszyny Wirtualnej, która ma taką samą wartość jednostki LUN. | -Nowy dysk danych został dołączony do maszyny Wirtualnej, ale nie został on zainicjowany.</br></br>-Dysk danych wewnątrz maszyny Wirtualnej nie zgłasza poprawnie wartości LUN, w którym dysk został dołączony do maszyny Wirtualnej.| Upewnij się, czy są inicjowane dyski z danymi, a następnie spróbuj ponownie wykonać operację.</br></br>Aby uzyskać Windows: [dołączania i zainicjować nowy dysk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Dla systemu Linux: [zainicjować nowy dysk danych w systemie Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Rozwiąż problem
Upewnij się, dyski danych zostały zainicjowane, a następnie spróbuj ponownie wykonać operację:

- Aby uzyskać Windows: [dołączania i zainicjować nowy dysk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Dla systemu Linux: [Dodaj nowy dysk danych w systemie Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Jeśli problem będzie się powtarzać, skontaktuj się z pomocą techniczną.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Nie można wyświetlić maszyny Wirtualnej platformy Azure do wyboru w "Włącz replikację"

 **Przyczyny 1: Grupa zasobów i źródłowa maszyna wirtualna znajdują się w innej lokalizacji** <br>
Usługa Azure Site Recovery obecnie które, że źródłowa grupa zasobów region i maszyny wirtualne powinny być w tej samej lokalizacji. Jeśli nie jest to użytkownik nie będzie mogła znaleźć maszyny wirtualnej w czasie ochrony.

**Przyczyny 2: Grupa zasobów nie jest częścią wybranej subskrypcji** <br>
Nie można odnaleźć grupy zasobów w czasie ochrony, jeśli nie jest częścią danej subskrypcji. Upewnij się, że grupa zasobów należy do subskrypcji, która jest używana.

 **Przyczyny 3: Nieaktualną konfigurację** <br>
Jeśli chcesz włączyć replikację maszyny Wirtualnej nie jest widoczny, jego może z powodu nieaktualną konfigurację Site Recovery pozostać na maszynie Wirtualnej platformy Azure. Nieaktualną konfigurację, może pozostać na Maszynie wirtualnej platformy Azure w następujących przypadkach:

- Włączyć replikację dla maszyny Wirtualnej platformy Azure przy użyciu usługi Site Recovery, a następnie usunąć magazyn usługi Site Recovery bez jawnie wyłączenie replikacji na maszynie Wirtualnej.
- Możesz włączyć replikację dla maszyny Wirtualnej platformy Azure przy użyciu usługi Site Recovery, a następnie usunąć grupę zasobów zawierającą magazyn usługi Site Recovery bez jawnie wyłączenie replikacji na maszynie Wirtualnej.

### <a name="fix-the-problem"></a>Rozwiąż problem

Możesz użyć [usuwanie starych skryptu konfiguracji usługi ASR](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) i usunąć nieaktualną konfigurację Site Recovery na maszynie Wirtualnej platformy Azure. Powinien być widoczny po usunięciu nieaktualną konfigurację maszyny Wirtualnej.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Nie można wybrać maszynę wirtualną do ochrony 
 **Przyczyny 1: maszyna wirtualna ma pewne rozszerzenia instalowana w stanie nie powiodło się lub nie odpowiada** <br>
 Przejdź do maszyny wirtualnej > Ustawienia > rozszerzenia i sprawdź, czy istnieją jakiekolwiek rozszerzenia w stanie niepowodzenia. Odinstaluj rozszerzenie nie powiodło się i spróbuj ponownie ochronę maszyny wirtualnej.<br>
 **Przyczyny 2: [stan inicjowania obsługi administracyjnej maszyny Wirtualnej jest nieprawidłowy](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>Stan inicjowania obsługi administracyjnej maszyny Wirtualnej jest nieprawidłowy (kod błędu: 150019)

Aby włączyć replikację na maszynie Wirtualnej, powinna być w stanie inicjowania obsługi administracyjnej **Powodzenie**. Możesz sprawdzić stan maszyny Wirtualnej, wykonując poniższe kroki.

1.  Wybierz **Eksploratora zasobów** z **wszystkich usług** w witrynie Azure portal.
2.  Rozwiń **subskrypcje** listy i wybierz swoją subskrypcję.
3.  Rozwiń **ResourceGroups** listy i wybierz grupę zasobów maszyny wirtualnej.
4.  Rozwiń **zasobów** listy i wybierz maszynę wirtualną
5.  Sprawdź **provisioningState** pola w widoku wystąpienia po prawej stronie.

### <a name="fix-the-problem"></a>Rozwiąż problem

- Jeśli **provisioningState** jest, skontaktuj się z pomocą techniczną, podając szczegóły, aby rozwiązać.
- Jeśli **provisioningState** jest **aktualizowanie**, innego rozszerzenia mogą być wprowadzenie wdrożone. Sprawdź, czy wszystkie trwające operacje na maszynie Wirtualnej, poczekaj na ukończenie i ponów próbę wykonania nie powiodło się, Usługa Site Recovery ich **włączyć replikację** zadania.

## <a name="unable-to-select-target-virtual-network---network-selection-tab-is-grayed-out"></a>Nie można wybrać docelowej sieci wirtualnej — karta wybór sieciowych jest wyszarzona.

**Przyczyny 1: Jeśli Twoja maszyna wirtualna jest podłączona do sieci, która jest już zamapowana do sieci docelowej.**
- Jeśli źródłowa maszyna wirtualna jest częścią sieci wirtualnej i inną maszynę Wirtualną z tej samej sieci wirtualnej jest już zamapowana z siecią w docelowej grupie zasobów, następnie według wyboru sieci domyślne listy rozwijanej zostaną wyłączone.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**Przyczyny 2: Jeśli uprzednio chronionych maszyn wirtualnych przy użyciu usługi Azure Site Recovery i wyłączone z replikacji.**
 - Wyłączanie replikacji maszyny Wirtualnej nie powoduje usunięcia mapowanie sieci. Musi on być usunięte z magazynu usługi recovery, gdy maszyna wirtualna była chroniona. </br>
 Przejdź do magazynu usług odzyskiwania > infrastruktura usługi Site Recovery > Mapowanie sieci. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - Sieć docelowa konfigurowanych podczas instalacji odzyskiwania po awarii można zmienić po początkowej skonfigurowane, po włączeniu ochrony maszyny Wirtualnej. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - Należy pamiętać, że zmiana mapowania sieci ma wpływ na wszystkie chronione maszyny wirtualne korzystające z mapowanie w określonej sieci.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ błąd usługi kopiowania woluminów w tle (kod błędu: 151025)
**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia**
--- | --- | ---
151025<br></br>**Komunikat**: nie można zainstalować rozszerzenia usługi Site recovery | -Usługa "COM + System Application" wyłączona.</br></br>-Usługa "Kopiowanie woluminów w tle" jest wyłączona.| Ustaw tryb automatycznego lub ręcznego uruchamiania usług "COM + System Application" i "Kopiowanie woluminów w tle".

### <a name="fix-the-problem"></a>Rozwiąż problem

Można otworzyć konsoli "Usługi" i upewnij się, "COM + System Application" i "Kopiowanie woluminów w tle" nie są ustawione na wartość "Wyłączony" dla "Typ uruchamiania".
  ![Błąd modelu COM](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="next-steps"></a>Kolejne kroki
[Replikowanie maszyn wirtualnych platformy Azure](site-recovery-replicate-azure-to-azure.md)
