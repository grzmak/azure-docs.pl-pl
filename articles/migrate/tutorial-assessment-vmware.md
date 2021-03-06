---
title: Odnajdowanie lokalnych maszyn wirtualnych VMware i ocenianie ich gotowości do migracji na platformę Azure za pomocą usługi Azure Migrate | Microsoft Docs
description: W tym artykule opisano sposób odnajdowania lokalnych maszyn wirtualnych VMware i oceniania ich gotowości do migracji na platformę Azure za pomocą usługi Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 09/21/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b2bb6636aef9e26a81988d344f04f23c23ea1622
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161883"
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Odnajdowanie i ocenianie lokalnych maszyn wirtualnych VMware pod kątem migracji na platformę Azure

Usługa [Azure Migrate](migrate-overview.md) ocenia obciążenia lokalne pod kątem migracji na platformę Azure.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta używanego przez usługę Azure Migrate do odnajdowania lokalnych maszyn wirtualnych.
> * Tworzenie projektu w usłudze Azure Migrate.
> * Konfigurowanie lokalnej maszyny wirtualnej modułu zbierającego w celu odnajdowania lokalnych maszyn wirtualnych VMware do oceny.
> * Grupowanie maszyn wirtualnych i tworzenie oceny.


Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).


## <a name="prerequisites"></a>Wymagania wstępne

- **VMware**: do zarządzania maszynami wirtualnymi, które mają być poddane migracji, musi być używany program vCenter Server w wersji 5.5, 6.0 lub 6.5. Ponadto wymagany jest jeden host ESXi z wersją 5.0 lub nowszą w celu wdrożenia maszyny wirtualnej modułu zbierającego.
- **Konto serwera vCenter**: wymagane jest konto tylko do odczytu w celu uzyskania dostępu do serwera vCenter. To konto jest używane w usłudze Azure Migrate do odnajdowania lokalnych maszyn wirtualnych.
- **Uprawnienia**: na serwerze vCenter są wymagane uprawnienia do tworzenia maszyn wirtualnych przez zaimportowanie pliku w formacie OVA.
- **Ustawienia statystyk**: to wymaganie wstępne dotyczy tylko modelu jednorazowego odnajdywania. Aby jednorazowe odnajdywanie działało, przed rozpoczęciem wdrażania należy ustawić poziom 3 w ustawieniach statystyk serwera vCenter. Jeśli poziom będzie niższy niż 3, ocena zostanie wykonana, ale nie zostaną zebrane dane dotyczące wydajności magazynu i sieci. W takim przypadku zalecenia dotyczące rozmiaru zostaną określone na podstawie danych dotyczących wydajności procesora i pamięci oraz danych konfiguracyjnych dysku i kart sieciowych.

## <a name="create-an-account-for-vm-discovery"></a>Tworzenie konta na potrzeby odnajdowania maszyny wirtualnej

Usługa Azure Migrate wymaga dostępu do serwerów VMware w celu automatycznego wykrywania maszyn wirtualnych do oceny. Utwórz konto VMware z poniższymi właściwościami. To konto jest wybierane podczas instalowania usługi Azure Migrate.

- Typ użytkownika: co najmniej użytkownik tylko do odczytu
- Uprawnienia: obiekt centrum danych –> propagacja do obiektu podrzędnego, rola = tylko do odczytu
- Szczegóły: użytkownik przypisany na poziomie centrum danych z dostępem do wszystkich obiektów w centrum danych.
- Aby ograniczyć dostęp, przypisz do obiektów podrzędnych (hostów vSphere, magazynów danych, maszyn wirtualnych i sieci) rolę Bez dostępu z propagacją do obiektu podrzędnego.


## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Tworzenie projektu

1. W witrynie Azure Portal kliknij pozycję **Utwórz zasób**.
2. Wyszukaj ciąg **Azure Migrate** i w wynikach wyszukiwania wybierz usługę **Azure Migrate**. Następnie kliknij pozycję **Utwórz**.
3. Wprowadź nazwę projektu i wybierz subskrypcję platformy Azure używaną na jego potrzeby.
4. Utwórz nową grupę zasobów.
5. Określ lokalizację geograficzną, w której chcesz utworzyć projekt, a następnie kliknij pozycję **Utwórz**. Projekt usługi Azure Migrate można utworzyć tylko w lokalizacji geograficznej Stanów Zjednoczonych. Można jednak zaplanować migrację do dowolnej docelowej lokalizacji platformy Azure. Geografia projektu jest używana wyłącznie do przechowywania metadanych zebranych z lokalnych maszyn wirtualnych.

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>Pobieranie urządzenia modułu zbierającego

Usługa Azure Migrate tworzy lokalną maszynę wirtualną, nazywaną modułem zbierającym. Ta maszyna wirtualna służy do odnajdowania lokalnych maszyn wirtualnych VMware i wysyłania dotyczących ich metadanych do usługi Azure Migrate. Aby skonfigurować urządzenie modułu zbierającego, należy pobrać plik OVA i zaimportować go na lokalny serwer vCenter w celu utworzenia maszyny wirtualnej.

1. W projekcie usługi Azure Migrate kliknij pozycję **Wprowadzenie** > **Odnajdź i oceń** > **Odnajdź maszyny**.
2. W obszarze **Odnajdź maszyny** dostępne są dwie opcje dla urządzenia. Kliknij przycisk **Pobierz**, aby pobrać odpowiednie urządzenie na podstawie swoich preferencji.

    a. **Odnajdywanie jednorazowe:** urządzenie dla tego modelu komunikuje się z serwerem vCenter, aby zebrać metadane dotyczące maszyn wirtualnych. Podczas zbierania danych wydajności maszyn wirtualnych opiera się ono na historycznych danych wydajności przechowywanych na serwerze vCenter i zbiera historię wydajności za ostatni miesiąc. W tym modelu usługa Azure Migrate zbiera średnie dane licznika (w porównaniu z licznikiem szczytowym) dla każdej metryki, [dowiedz się więcej] (https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected). Ponieważ jest to odnajdywanie jednorazowe, zmiany w środowisku lokalnym nie są odzwierciedlane po ukończeniu odnajdywania. Jeśli chcesz odzwierciedlić zmiany, musisz przeprowadzić ponowne odnajdywanie w tym samym środowisku dla tego samego projektu.

    b. **Ciągłe odnajdywanie:** urządzenie dla tego modelu stale profiluje środowisko lokalne w celu zebrania danych użycia w czasie rzeczywistym dla każdej maszyny wirtualnej. W tym modelu liczniki szczytowe są zbierane dla każdej metryki (użycie procesora CPU, użycie pamięci itp.). Ten model nie zależy od ustawień statystyk serwera vCenter na potrzeby zbierania danych o wydajności. Z urządzenia możesz w dowolnym momencie zatrzymać ciągłe profilowanie.

    > [!NOTE]
    > Funkcjonalność ciągłego odnajdywania jest dostępna w wersji zapoznawczej.

3. W obszarze **Skopiuj poświadczenia projektu** skopiuj identyfikator projektu i klucz. Będą potrzebne do skonfigurowania modułu zbierającego.

    ![Pobieranie pliku OVA](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Weryfikowanie urządzenia modułu zbierającego

Przed wdrożeniem pliku OVA sprawdź, czy jest on bezpieczny.

1. Na maszynie, na którą pobrano plik, otwórz okno wiersza polecenia administratora.
2. Uruchom następujące polecenie, aby wygenerować skrót pliku OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Przykład użycia: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Wygenerowany skrót powinien odpowiadać następującym ustawieniom.

#### <a name="one-time-discovery"></a>Jednorazowe odnajdywanie

  OVA w wersji 1.0.9.14

  **Algorytm** | **Wartość skrótu**
  --- | ---
  MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
  SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
  SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

  OVA w wersji 1.0.9.12

  **Algorytm** | **Wartość skrótu**
  --- | ---
  MD5 | d0363e5d1b377a8eb08843cf034ac28a
  SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
  SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

  OVA w wersji 1.0.9.8

  **Algorytm** | **Wartość skrótu**
  --- | ---
  MD5 | b5d9f0caf15ca357ac0563468c2e6251
  SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
  SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff


  OVA w wersji 1.0.9.7

  **Algorytm** | **Wartość skrótu**
  --- | ---
  MD5 | d5b6a03701203ff556fa78694d6d7c35
  SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
  SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

#### <a name="continuous-discovery"></a>Ciągłe odnajdywanie

  Do OVA w wersji 1.0.10.4

  **Algorytm** | **Wartość skrótu**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be

## <a name="create-the-collector-vm"></a>Tworzenie maszyny wirtualnej modułu zbierającego

Zaimportuj pobrany plik na serwer vCenter.

1. W konsoli klienta vSphere kliknij pozycję **File** (Plik)  >  **Deploy OVF Template** (Wdróż szablon OVF).

    ![Wdrażanie pliku OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. W kreatorze wdrażania szablonu OVF na stronie **Source** (Źródło) określ lokalizację pliku OVA.
3. W polach **Name** (Nazwa) i **Location** (Lokalizacja) wprowadź przyjazną nazwę maszyny wirtualnej modułu zbierającego oraz obiekt magazynu, w którym będzie hostowana maszyna wirtualna.
5. W polu **Host/Cluster** (Host/klaster) określ host lub klaster, na którym będzie działać maszyna wirtualna modułu zbierającego.
7. W obszarze Magazyn określ docelowy magazyn dla maszyny wirtualnej modułu zbierającego.
8. W obszarze **Disk Format** (Format dysku) określ typ i rozmiar dysku.
9. W obszarze **Network Mapping** (Mapowanie sieci) określ sieć, z którą będzie się łączyć maszyna wirtualna modułu zbierającego. Sieć musi mieć połączenie z Internetem w celu wysyłania metadanych do platformy Azure.
10. Sprawdź poprawność ustawień, a następnie kliknij pozycję **Finish** (Zakończ).

## <a name="run-the-collector-to-discover-vms"></a>Uruchamianie modułu zbierającego w celu odnalezienia maszyn wirtualnych

1. W konsoli klienta vSphere kliknij maszynę wirtualną prawym przyciskiem myszy, a następnie kliknij pozycję **Open Console** (Otwórz konsolę).
2. Określ preferencje dotyczące języka, strefy czasowej i hasła dla urządzenia.
3. Na pulpicie kliknij skrót **Run collector** (Uruchom moduł zbierający).
4. Kliknij przycisk **Sprawdź aktualizacje** na górnym pasku modułu zbierającego interfejsu użytkownika i sprawdź, czy działający moduł zbierający to najnowsza wersja. Jeśli tak nie jest, można pobrać najnowszy pakiet aktualizacji przy użyciu łącza i zaktualizować moduł zbierający.
5. W usłudze Azure Migrate Collector otwórz obszar **Set up prerequisites** (Skonfiguruj elementy wymagane wstępnie).
    - Zaakceptuj postanowienia licencyjne i przeczytaj informacje innych firm.
    - Moduł zbierający sprawdzi, czy maszyna wirtualna ma dostęp do Internetu.
    - Jeśli maszyna wirtualna uzyskuje dostęp do Internetu za pośrednictwem serwera proxy, kliknij pozycję **Proxy settings** (Ustawienia serwera proxy) i wprowadź adres serwera proxy oraz port nasłuchujący. Jeśli serwer proxy wymaga uwierzytelnienia, wprowadź poświadczenia. [Dowiedz się więcej](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity) na temat wymagań dotyczących łączności z Internetem i listy adresów URL, do których moduł zbierający uzyskuje dostęp.

    > [!NOTE]
    > Adres serwera proxy musi zostać wprowadzony w postaci http://ProxyIPAddress lub http://ProxyFQDN. Obsługiwane są tylko serwery proxy HTTP. Jeśli masz przechwytujący serwer proxy, nawiązanie połączenia internetowego może początkowo się nie udać, jeśli nie zaimportowano certyfikatu serwera proxy; [dowiedz się więcej](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity-with-intercepting-proxy) na temat rozwiązywania tego problemu poprzez zaimportowanie certyfikatu serwera proxy jako zaufanego certyfikatu na maszynie wirtualnej modułu zbierającego.

    - Moduł zbierający sprawdzi, czy usługa modułu zbierającego jest uruchomiona. Jest ona instalowana domyślnie na maszynie wirtualnej modułu zbierającego.
    - Pobierz i zainstaluj program VMware PowerCLI.

6. W obszarze **Specify vCenter Server details** (Określ szczegóły serwera vCenter) wykonaj następujące czynności:
    - Określ nazwę FQDN lub adres IP serwera vCenter.
    - W polach **User name** (Nazwa użytkownika) i **Password** (Hasło) wprowadź poświadczenia konta tylko do odczytu, którego moduł zbierający ma użyć do odnalezienia maszyn wirtualnych na serwerze vCenter.
    - W obszarze **Collection scope** (Zakres zbierania) wybierz zakres odnajdowania maszyn wirtualnych. Moduł zbierający odnajdzie tylko maszyny wirtualne we wskazanym zakresie. Zakresem może być określony folder, centrum danych albo klaster. Zakres nie powinien zawierać więcej niż 1500 maszyn wirtualnych. [Dowiedz się więcej](how-to-scale-assessment.md) o sposobach odnajdywania większego środowiska.

7. W obszarze **Specify migration project** (Określ projekt migracji) podaj identyfikator i klucz projektu usługi Azure Migrate, skopiowane z portalu. Jeśli nie zostały skopiowane, otwórz witrynę Azure Portal na maszynie wirtualnej modułu zbierającego. Na stronie **Omówienie** projektu kliknij polecenie **Odnajdź maszyny**, a następnie skopiuj wartości.  
8. W obszarze **View collection progress** (Wyświetl postęp zbierania) monitoruj stan odnajdywania. Dowiedz się więcej](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected) o tym, jakie dane są zbierane przez moduł zbierający usługi Azure Migrate.

> [!NOTE]
> Moduł zbierający obsługuje wyłącznie „Angielski (Stany Zjednoczone)” jako język systemu operacyjnego i język interfejsu modułu zbierającego.
> Jeśli zmienisz ustawienia na ocenianej maszynie, przed uruchomieniem oceny ponownie wyzwól odnajdywanie. W tym celu w module zbierającym użyj opcji **Ponownie uruchom zbieranie**. Po zakończeniu zbierania wybierz opcję **Oblicz ponownie** dla oceny w portalu, aby pobrać zaktualizowane wyniki oceny.



### <a name="verify-vms-in-the-portal"></a>Weryfikowanie maszyn wirtualnych w portalu

W przypadku jednorazowego odnajdywania czas odnajdywania zależy od liczby odnajdywanych maszyn wirtualnych. Zazwyczaj dla 100 maszyn wirtualnych, gdy moduł zbierający zakończy działanie, zakończenie zbierania danych konfiguracji i wydajności trwa około godziny. Możesz utworzyć oceny (zarówno oparte na wydajności, jak i oceny lokalne) natychmiast po zakończeniu odnajdywania.

W przypadku ciągłego odnajdywania (w wersji zapoznawczej) moduł zbierający będzie stale profilował środowisko lokalne i będzie nadal wysyłał dane dotyczące wydajności w godzinnych odstępach. Możesz przejrzeć maszyny w portalu po godzinie od rozpoczęcia odnajdywania. Zdecydowanie zalecamy poczekać co najmniej dzień przed utworzeniem jakichkolwiek ocen na podstawie wydajności dla maszyn wirtualnych.

1. W projekcie migracji kliknij pozycję **Zarządzaj** > **Maszyny**.
2. Sprawdź, czy maszyny wirtualne, które miały zostać odnalezione, są widoczne w portalu.


## <a name="create-and-view-an-assessment"></a>Tworzenie i wyświetlanie oceny

Po odnalezieniu maszyn wirtualnych należy je zgrupować i utworzyć ocenę.

1. Na stronie **Omówienie** projektu kliknij pozycję **+Utwórz ocenę**.
2. Kliknij pozycję **Wyświetl wszystko**, aby sprawdzić właściwości oceny.
3. Utwórz grupę i określ jej nazwę.
4. Wybierz maszyny wirtualne, które chcesz dodać do grupy.
5. Kliknij pozycję **Utwórz ocenę**, aby utworzyć grupę i ocenę.
6. Po utworzeniu oceny możesz wyświetlić ją w obszarze **Omówienie** > **Pulpit nawigacyjny**.
7. Kliknij polecenie **Eksportuj ocenę**, aby pobrać ocenę jako plik programu Excel.

> [!NOTE]
> W przypadku ciągłego odnajdywania zdecydowanie zalecamy poczekać co najmniej dzień po uruchomieniu odnajdywania, a przed utworzeniem oceny. Jeśli chcesz zaktualizować istniejącą ocenę przy użyciu najnowszych danych wydajności, możesz użyć polecenia **Oblicz ponownie** w ramach oceny, aby ją zaktualizować.

### <a name="assessment-details"></a>Szczegóły oceny

Ocena zawiera informacje na temat tego, czy lokalne maszyny wirtualne są zgodne na platformie Azure, jaki byłyby właściwy rozmiar maszyny wirtualnej do uruchamiania maszyny wirtualnej na platformie Azure oraz miesięczne szacowane koszty platformy Azure.

![Raport z oceny](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Gotowość na platformę Azure

Widok gotowości platformy Azure w ocenie przedstawia stan gotowości każdej maszyny wirtualnej. W zależności od właściwości maszyny wirtualnej każda maszyna wirtualna może być oznaczona jako:
- Gotowa na platformę Azure
- Warunkowo gotowa na platformę Azure
- Niegotowa na platformę Azure
- Nieznana gotowość

W przypadku maszyn wirtualnych, które są gotowe do migracji, usługa Azure Migrate wyświetla zalecany rozmiar maszyny wirtualnej na platformie Azure. Zalecenie dotyczące rozmiaru określane przez usługę Azure Migrate zależy od kryterium ustalania rozmiaru określonego we właściwościach oceny. Jeśli kryterium ustalania rozmiaru jest oparte na wydajności, zalecenie dotyczące rozmiaru jest ustalane z uwzględnieniem historii wydajności maszyn wirtualnych (procesor i pamięć) oraz dysków (liczba operacji we/wy na sekundę i przepływność). Jeśli kryterium ustalania rozmiaru brzmi „tak jak lokalnie”, usługa Azure Migrate nie bierze pod uwagę danych dotyczących wydajności maszyny wirtualnej i dysków. Zalecenia dla rozmiaru maszyny wirtualnej na platformie Azure są ustalane na podstawie analizy rozmiaru lokalnej maszyny wirtualnej, ustalanie rozmiaru dysku zależy natomiast od typu magazynu określonego we właściwościach oceny (wartość domyślna to dyski premium). [Dowiedz się więcej](concepts-assessment-calculation.md) o sposobie określania rozmiaru w usłudze Azure Migrate.

Dla maszyn wirtualnych, które nie są gotowe lub są warunkowo gotowe na platformę Azure, usługa Azure Migrate opisuje problemy z gotowością i proponuje instrukcje rozwiązania problemu.

Maszyny wirtualne, dla których usługa Azure Migrate nie może ustalić gotowości na platformę Azure (z powodu niedostępności danych), zostały oznaczone jako nieznana gotowość.

Oprócz gotowości na platformę Azure i ustalenia rozmiaru, usługa Azure Migrate sugeruje również narzędzia, których możesz użyć do migracji maszyny wirtualnej. Wymaga to bardziej szczegółowego odnajdowania w środowisku lokalnym. [Dowiedz się więcej](how-to-get-migration-tool.md) na temat sposobu przeprowadzania bardziej szczegółowego odnajdowania przez zainstalowanie agentów na komputerach lokalnych. Jeśli na maszynach lokalnych nie zainstalowano agentów, zalecane jest przeprowadzenie migracji metodą „lift-and-shift” przy użyciu usługi [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Jeśli na maszynach lokalnych zainstalowano agentów, usługa Azure Migrate analizuje procesy uruchomione na maszynie i sprawdza, czy dana maszyna to maszyna bazy danych. W przypadku maszyny bazy danych sugerowane jest użycie usługi [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). W przeciwnym razie sugerowanym narzędziem migracji jest usługa Azure Site Recovery.

  ![Ocena gotowości](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Szacowany koszt miesięczny

Ten widok przedstawia łączne koszty zasobów obliczeniowych i magazynowych w przypadku korzystania z maszyn wirtualnych na platformie Azure oraz szczegóły dla poszczególnych maszyn. Koszty są szacowane z uwzględnieniem zalecanych przez usługę Azure Migrate rozmiarów maszyny i jej dysków oraz właściwości oceny.

> [!NOTE]
> Szacowany koszt podany w usłudze Azure Migrate dotyczy korzystania z lokalnych maszyn wirtualnych jako maszyn wirtualnych platformy Azure w ramach rozwiązania typu IaaS (infrastruktura jako usługa). Usługa Azure Migrate nie uwzględnia kosztów rozwiązań typu PaaS (platforma jako usługa) i SaaS (oprogramowanie jako usługa).

Szacowany miesięczny koszt zasobów obliczeniowych i magazynowych jest agregowany dla wszystkich maszyn wirtualnych w grupie.

![Ocena kosztu maszyn wirtualnych](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Ocena zaufania

Każda bazująca na wydajności ocena w usłudze Azure Migrate jest skojarzona z oceną zaufania obejmującą zakres od 1 gwiazdki do 5 gwiazdek (1 gwiazdka to najniższa ocena, 5 gwiazdek — najwyższa). Ocena zaufania jest przypisana do oceny na podstawie dostępności punktów danych potrzebnych do obliczenia oceny. Ocena zaufania do oceny pomaga oszacować niezawodność zaleceń dotyczących rozmiaru określanych przez usługę Azure Migrate. Ocena zaufania nie ma przełożenia na oceny lokalne.

W przypadku ustalania rozmiaru na podstawie wydajności usługa Azure Migrate potrzebuje danych o użyciu procesora CPU i pamięci maszyny wirtualnej. Ponadto dla każdego dysku podłączonego do maszyny wirtualnej potrzebuje ona danych o liczbie operacji odczytu/zapisu na sekundę i przepływności. Analogicznie dla każdej karty sieciowej podłączonej do maszyny wirtualnej usługa Azure Migrate potrzebuje danych o ruchu wchodzącym/wychodzącym sieci do ustalenia rozmiaru na podstawie wydajności. Jeśli którekolwiek z powyższych danych użycia są niedostępne w programie vCenter Server, zalecenie dotyczące rozmiaru określone przez usługę Azure Migrate może nie być wiarygodne. W zależności od odsetka dostępnych punktów danych ocena zaufania dla oceny jest określana w następujący sposób:

   **Dostępność punktów danych** | **Ocenę zaufania**
   --- | ---
   0%–20% | 1 gwiazdka
   21%–40% | 2 gwiazdki
   41%–60% | 3 gwiazdki
   61%–80% | 4 gwiazdki
   81%–100% | 5 gwiazdek

Ocena może nie mieć dostępnych wszystkich punktów danych z jednego z następujących powodów:

**Jednorazowe odnajdywanie**

- Ustawienie statystyk w programie vCenter Server jest inne niż poziom 3. Ponieważ model jednorazowego odnajdywania zależy od ustawień statystyk serwera vCenter, jeśli ustawienie statystyk na serwerze vCenter jest niższe niż poziom 3, dane wydajności dla dysku i sieci nie są zbierane z serwera vCenter. W takim przypadku zalecenie określane przez usługę Azure Migrate dla dysku i sieci nie opiera się na użyciu. Bez uwzględniania liczby operacji we/wy na sekundę/przepływności dysku usługa Azure Migrate nie może określić, czy dysk będzie potrzebować dysku w warstwie Premium na platformie Azure, dlatego w tym przypadku usługa Azure Migrate zaleca wszystkie dyski w warstwie Standardowa.
- Ustawienie statystyk na serwerze vCenter zostało ustawione na poziom 3 przez krótszy czas przed rozpoczęciem odnajdywania. Rozważmy na przykład scenariusz, w którym dzisiaj zmienisz poziom ustawień statystyk na 3 i jutro rozpoczniesz odnajdowanie przy użyciu urządzenia modułu zbierającego (po 24 godzinach). Jeśli tworzysz ocenę dla jednego dnia, masz wszystkie punkty danych i oceną zaufania dla oceny będzie 5 gwiazdek. Ale jeśli zmieniasz czas trwania wydajności we właściwościach oceny na jeden miesiąc, ocena zaufania spada, ponieważ dane o wydajności dysku i sieci dla ostatniego miesiąca byłyby niedostępne. Jeśli chcesz wziąć pod uwagę dane wydajności za ostatni miesiąc, zaleca się utrzymanie ustawienia statystyk programu vCenter Server na poziomie 3 przez jeden miesiąc przed rozpoczęciem odnajdywania.

**Ciągłe odnajdywanie**

- Nie profilujesz swojego środowiska przez czas trwania, dla którego tworzysz ocenę. Jeśli na przykład tworzysz ocenę z czasem trwania wydajności ustawionym na 1 dzień, musisz poczekać co najmniej dzień po uruchomieniu odnajdywania, aby zebrać wszystkie punkty danych.

**Typowe przyczyny**  

- Kilka maszyn wirtualnych zostało wyłączonych w czasie, dla którego jest obliczana ocena. Jeśli którakolwiek maszyna wirtualna została odłączona od zasilania na pewien czas, nie będziemy mogli zebrać danych o wydajności dla tego okresu.
- Kilka maszyn wirtualnych zostało utworzonych w czasie, dla którego jest obliczana ocena. Jeśli na przykład tworzysz ocenę dla historii wydajności za ostatni miesiąc, ale kilka maszyn wirtualnych zostało utworzonych w środowisku tylko tydzień temu. W takich przypadkach historia wydajności nowych maszyn wirtualnych nie będzie dotyczyła całego czasu oceny.

> [!NOTE]
> Jeśli ocena zaufania dowolnej oceny jest poniżej 4 gwiazdek dla modelu odnajdywania jednorazowego, zalecamy zmianę ustawienia poziomu statystyk serwera vCenter na 3, odczekanie przez czas, który chcesz wziąć pod uwagę podczas oceny (1 dzień/1 tydzień/1 miesiąc), a następnie przeprowadzenie odnajdywania i oceny. W przypadku modelu ciągłego odnajdywania poczekaj co najmniej dzień, aby urządzenie przeprowadziło profilowanie w środowisku, a następnie *Oblicz ponownie* ocenę. Jeśli nie można wykonać powyższego, ustalanie rozmiaru na podstawie wydajności może nie być wiarygodne i zaleca się, aby przełączyć się na *ustalanie rozmiaru jako lokalnego*, zmieniając właściwości oceny.

## <a name="next-steps"></a>Następne kroki

- [Dowiedz się](how-to-modify-assessment.md), jak dostosować ocenę zgodnie z wymaganiami.
- Dowiedz się, jak tworzyć grupy o wysokim stopniu pewności na potrzeby oceny za pomocą [mapowania zależności maszyn](how-to-create-group-machine-dependencies.md).
- [Dowiedz się więcej](concepts-assessment-calculation.md) na temat sposobu obliczania ocen.
- [Dowiedz się](how-to-scale-assessment.md), jak odnajdować i oceniać maszyny wirtualne w dużym środowisku VMware.
- [Dowiedz się więcej](resources-faq.md) na temat usługi Azure Migrate z odpowiedzi na Często zadawane pytania
