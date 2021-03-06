---
title: Zarządzanie aktualizacjami dla wielu maszyn wirtualnych platformy Azure
description: W tym artykule opisano sposób zarządzania aktualizacjami dla maszyn wirtualnych platformy Azure.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 23f86581b5ecc5257ccb246c7199eef4246efb08
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498236"
---
# <a name="manage-updates-for-multiple-machines"></a>Zarządzanie aktualizacjami dla wielu maszyn

Rozwiązanie Update Management umożliwia zarządzanie aktualizacjami i poprawkami dla maszyn wirtualnych Windows i Linux. Korzystając z konta usługi [Azure Automation](automation-offering-get-started.md), można wykonywać następujące czynności:

- Dołączanie maszyn wirtualnych
- Ocenianie stanu dostępnych aktualizacji
- Zaplanować instalację wymaganych aktualizacji
- Przejrzeć wyniki wdrażania, aby sprawdzić, czy aktualizacje zostały pomyślnie zastosowane do wszystkich maszyn wirtualnych, dla których włączono rozwiązanie Update Management

## <a name="prerequisites"></a>Wymagania wstępne

Aby korzystać z rozwiązania Update Management, potrzebne są:

- Konto Uruchom jako usługi Azure Automation. Aby dowiedzieć się, jak utworzyć, zobacz [wprowadzenie do usługi Azure Automation](automation-offering-get-started.md).
- Maszyna wirtualna lub komputer z zainstalowanym obsługiwanym systemem operacyjnym.

## <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

Rozwiązanie Update Management jest obsługiwana w następujących systemach operacyjnych:

|System operacyjny  |Uwagi  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Obsługuje tylko zaktualizować oceny.         |
|Windows Server 2008 R2 z dodatkiem SP1 lub nowszy     |Windows PowerShell 4.0 lub nowszy jest wymagany. ([Pobierz platformę WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))</br> Programu Windows PowerShell 5.1 jest zalecane w celu zwiększenia niezawodności. ([Pobierz platformę WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) i 7 (x64)      | Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.        |
|Red Hat Enterprise 6 (x86/x64) i 7 (x64)     | Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.        |
|SUSE Linux Enterprise Server 11 (x86/x64) i 12 (x64)     | Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.        |
|Ubuntu 12.04 LTS, 14.04 LTS i 16.04 LTS — x86/x64 64      |Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.         |

> [!NOTE]
> Aby w przypadku systemu Ubuntu uniknąć stosowania aktualizacji poza oknem obsługi, zmień konfigurację pakietu Unattended-Upgrade tak, aby wyłączyć automatyczne aktualizacje. Aby uzyskać więcej informacji, zobacz [temat poświęcony aktualizacjom automatycznym w podręczniku systemu Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.

To rozwiązanie nie obsługuje Log Analytics Agent dla systemu Linux, który jest skonfigurowany do raportowania do wielu obszarów roboczych usługi Azure Log Analytics.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Włączanie rozwiązania Update Management dla maszyn wirtualnych platformy Azure

W witrynie Azure portal Otwórz konto usługi Automation, a następnie wybierz **rozwiązanie Update management**.

Wybierz **Dodaj maszyny wirtualne platformy Azure**.

![Dodaj kartę maszyny Wirtualnej platformy Azure](./media/manage-update-multi/update-onboard-vm.png)

Wybierz maszynę wirtualną, którą chcesz dołączyć. 

W obszarze **Włączanie rozwiązania Update Management**, wybierz opcję **Włącz** dołączyć maszyny wirtualnej.

![Okno dialogowe Włączanie rozwiązania Update Management](./media/manage-update-multi/update-enable.png)

Po zakończeniu operacji dołączania, rozwiązanie Update Management jest włączone dla maszyny wirtualnej.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Włączanie rozwiązania Update Management dla maszyn wirtualnych poza platformą Azure i komputerów

Aby dowiedzieć się, jak włączanie rozwiązania Update Management dla maszyn wirtualnych innych niż Windows na platformie Azure i komputerów, zobacz [Windows łączenie komputerów do usługi Log Analytics na platformie Azure](../log-analytics/log-analytics-windows-agent.md).

Aby dowiedzieć się, jak włączanie rozwiązania Update Management dla maszyn wirtualnych Linux platformy Azure i komputerów, zobacz [łączenie komputerów z systemem Linux do usługi Log Analytics](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Wyświetlanie komputerów dołączonych do konta usługi Automation

Po włączeniu zarządzania aktualizacjami dla maszyn, można wyświetlić informacje o maszynie, wybierając **komputerów**. Można przeglądać informacje o *NazwaKomputera*, *stan zgodności*, *środowiska*, *typ systemu operacyjnego*, *krytycznych i zainstalowane aktualizacje zabezpieczeń*, *innych zainstalowanych aktualizacji*, i *Aktualizuj gotowość agenta* dla komputerów.

  ![Karta z wyświetlonymi komputerami](./media/manage-update-multi/update-computers-tab.png)

Komputery, które zostały niedawno włączono rozwiązanie Update Management może nie być jeszcze ocenione. Jest w stanie stan zgodności dla tych komputerów **nie oceniono**. Oto lista możliwych wartości dla stanu zgodności:

- **Zgodne**: komputery, które są krytycznymi i aktualizacjami zabezpieczeń.

- **Niezgodne**: komputery, na których brakuje co najmniej jeden krytycznych lub aktualizacji zabezpieczeń.

- **Nie oceniono**: dane oceny aktualizacji nie zostały odebrane z komputera w oczekiwanym czasie. W przypadku komputerów z systemem Linux przedział czasu oczekiwania jest w ciągu ostatnich 3 godzin. W przypadku komputerów Windows oczekiwanego przedział czasu jest w ciągu ostatnich 12 godzin.

Aby wyświetlić stan agenta, wybierz link w **AKTUALIZUJ gotowość AGENTA** kolumny. Wybranie tej opcji powoduje otwarcie **hybrydowy proces roboczy** okienku i wyświetla stan hybrydowy proces roboczy. Na poniższej ilustracji przedstawiono przykład agenta, który nie został połączony z rozwiązania Update Management przez dłuższy czas:

![Karta z wyświetlonymi komputerami](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Wyświetlanie oceny aktualizacji

Po włączeniu rozwiązania Update Management zostanie otwarte okienko **Update Management**. Możesz wyświetlić listę brakujących aktualizacji na karcie **Brakujące aktualizacje**.

## <a name="collect-data"></a>Zbieranie danych

Agenci zainstalowani na maszynach wirtualnych i komputerów zbierania danych o aktualizacjach. Agenci wysyłają dane do usługi Azure Update Management.

### <a name="supported-agents"></a>Obsługiwani agenci

W poniższej tabeli opisano połączone źródła obsługiwane przez to rozwiązanie:

| Połączone źródło | Obsługiwane | Opis |
| --- | --- | --- |
| Agenci dla systemu Windows |Yes |Rozwiązanie Update Management zbiera informacje o aktualizacjach systemu z agentów dla Windows i inicjuje instalowanie wymaganych aktualizacji. |
| Agenci dla systemu Linux |Yes |Rozwiązanie Update Management zbiera informacje o aktualizacjach systemu z agentów dla systemu Linux i inicjuje instalowanie wymaganych aktualizacji w obsługiwanych dystrybucjach. |
| Grupa zarządzania programu Operations Manager |Yes |Rozwiązanie Update Management zbiera informacje o aktualizacjach systemu z agentów w połączonej grupie zarządzania. |
| Konto usługi Azure Storage |Nie |Usługa Azure Storage nie zawiera informacji o aktualizacjach systemu. |

### <a name="collection-frequency"></a>Częstotliwość zbierania

Skanowanie jest uruchamiane dwa razy dziennie dla każdego zarządzanego komputera z Windows. Co 15 minut wywoływany jest interfejs API Windows Aby wykonać zapytanie o czas ostatniej aktualizacji ustalić, czy stan się zmienił. Jeśli stan został zmieniony, rozpoczyna się skanowanie pod kątem zgodności. Skanowanie jest uruchamiane co 3 godziny przez każdego zarządzanego komputera z systemem Linux.

Może upłynąć od 30 minut do 6 godzin dla pulpitu nawigacyjnego wyświetlić zaktualizowane dane z zarządzanych komputerów.

## <a name="schedule-an-update-deployment"></a>Planowanie wdrożenia aktualizacji

Aby zainstalować aktualizacje, Zaplanuj wdrożenie zgodnie z oknem harmonogram i usługi release. Możesz wybrać typy aktualizacji, które mają zostać uwzględnione we wdrożeniu. Możesz na przykład uwzględnić aktualizacje krytyczne lub aktualizacje zabezpieczeń i wykluczyć pakiety zbiorcze aktualizacji.

Aby zaplanować nowe wdrożenie aktualizacji dla co najmniej jednej maszyny wirtualnej w obszarze **rozwiązanie Update management**, wybierz opcję **Zaplanuj wdrażanie aktualizacji**.

W **nowe wdrożenie aktualizacji** okienku określ następujące informacje:

- **Nazwa**: wprowadź unikatową nazwę identyfikującą wdrożenie aktualizacji.
- **System operacyjny**: Wybierz **Windows** lub **Linux**.
- **Grupy można zaktualizować (wersja zapoznawcza)**: Definiowanie zapytań, w zależności od kombinacji subskrypcji, grupy zasobów, lokalizacje i tagi, do tworzenia grupy dynamicznej maszyn wirtualnych platformy Azure, aby uwzględnić w danym wdrożeniu. Aby dowiedzieć się więcej, zobacz [grupy dynamiczne](automation-update-management.md#using-dynamic-groups)
- **Maszyny do zaktualizowania**: Wybierz zapisane wyszukiwanie, zaimportowane grupy, lub wybierz maszyny, aby wybrać maszyn, które chcesz zaktualizować. Jeśli wybierzesz pozycję **Maszyny**, gotowość maszyny będzie wyświetlana w kolumnie **AKTUALIZUJ GOTOWOŚĆ AGENTA**. Widać stan kondycji komputera, zanim zaplanowane wdrożenie aktualizacji. Aby dowiedzieć się więcej na temat różnych metod tworzenia grup komputerów w usłudze Log Analytics, zobacz [Grupy komputerów w usłudze Log Analytics](../log-analytics/log-analytics-computer-groups.md)

  ![Nowe okienko wdrożenia aktualizacji](./media/manage-update-multi/update-select-computers.png)

- **Klasyfikacja aktualizacji**: Wybierz typy oprogramowania uwzględnione we wdrożeniu aktualizacji. Aby uzyskać opis typy klasyfikacji, zobacz [klasyfikacje aktualizacji](automation-update-management.md#update-classifications). Dostępne są następujące typy klasyfikacji:
  - Aktualizacje krytyczne
  - Aktualizacje zabezpieczeń
  - Pakiety zbiorcze aktualizacji
  - Pakiety funkcji
  - Dodatki Service Pack
  - Aktualizacje definicji
  - Narzędzia
  - Aktualizacje

- **Aktualizacje do dołączania lub wykluczania** — spowoduje to otwarcie **uwzględniania/wykluczania** strony. Na osobnych kartach są aktualizacje być dołączone lub wykluczone. Aby uzyskać dodatkowe informacje na temat sposobu obsługi dołączania, zobacz [zachowanie dołączania](automation-update-management.md#inclusion-behavior)

- **Ustawienia harmonogramu**: możesz zaakceptować domyślną datę i godzinę, czyli 30 minut po bieżącej godzinie. Można również określić inny czas.

   Możesz też określić, czy wdrożenie ma występować raz, czy zgodnie z harmonogramem cyklicznym. Aby skonfigurować Harmonogram cykliczny, w obszarze **cyklu**, wybierz opcję **cyklicznie**.

   ![Okno dialogowe Ustawienia harmonogramu](./media/manage-update-multi/update-set-schedule.png)

- **Wstępnie skrypty i skrypty wykonywane po**: Wybierz skrypty do uruchomienia przed i po wdrożeniu. Aby dowiedzieć się więcej, zobacz [skrypty zarządzania przed i po](pre-post-scripts.md).
- **Okno konserwacji (w minutach)**: Podaj okres czasu, który ma zostać przeprowadzone wdrażanie aktualizacji. To ustawienie pozwala zagwarantować, że zmiany będą wprowadzane w ramach zdefiniowanych okien obsługi.

- **Ponowne uruchomienie kontroli** — to ustawienie określa sposób obsługi ponownego uruchamiania dla wdrożenia aktualizacji.

   |Opcja|Opis|
   |---|---|
   |Ponowne uruchomienie komputera, jeśli jest to wymagane| **(Ustawienie domyślne)**  w razie potrzeby, ponowne uruchomienie jest inicjowane, jeśli zezwala na okno obsługi.|
   |Zawsze uruchamiaj ponownie|Ponowne uruchomienie jest inicjowane niezależnie od tego, czy jest wymagane. |
   |Nigdy nie uruchamiaj ponownie|Niezależnie od tego, jeśli ponowne uruchomienie jest wymagane, ponownego uruchamiania są pomijane.|
   |Tylko ponowne uruchomienie — aktualizacje nie zostaną zainstalowane|Ta opcja instalacji aktualizacji ignoruje i tylko inicjuje ponowne uruchomienie komputera.|

Po zakończeniu konfigurowania harmonogramu wybierz **Utwórz** przycisk, aby powrócić do pulpitu nawigacyjnego stanu. **Zaplanowane** tabeli przedstawiono harmonogram wdrożenia utworzony.

## <a name="view-results-of-an-update-deployment"></a>Wyświetlanie wyników wdrażania aktualizacji

Po rozpoczęciu zaplanowanego wdrażania stan tego wdrożenia można sprawdzić na karcie **Wdrożenia aktualizacji** w obszarze rozwiązania **Update Management**.

Jeśli wdrożenie jest aktualnie uruchomione, wyświetlany jest stan **W toku**. Po pomyślnym zakończeniu wdrożenia stan zmienia się na **Powodzenie**.

W przypadku błędu co najmniej jednej aktualizacji w ramach wdrożenia jest wyświetlany stan **Częściowe niepowodzenie**.

![Stan wdrożenia aktualizacji](./media/manage-update-multi/update-view-results.png)

Aby wyświetlić pulpit nawigacyjny wdrożenia aktualizacji, wybierz ukończone wdrożenie.

**Niezaktualizowanie** okienko zawiera całkowitą liczbę aktualizacji i wynikami wdrożenia maszyny wirtualnej. Tabela po prawej stronie zawiera szczegółowy podział każdej aktualizacji i wyniki instalacji. Wyniki instalacji mogą mieć jedną z następujących wartości:

- **Nie podjęto próby**: nie zainstalowano aktualizacji z powodu niewystarczającego czasu dostępności oparte na zdefiniowanym oknie konserwacji.
- **Powodzenie**: aktualizacja powiodła się.
- **Niepowodzenie**: aktualizacja nie powiodła się.

Aby wyświetlić wszystkie wpisy dziennika utworzone przez wdrożenie, wybierz pozycję **Wszystkie dzienniki**.

Aby wyświetlić strumień zadań elementu runbook, który zarządza wdrożeniem aktualizacji na docelowej maszynie wirtualnej, wybierz Kafelek danych wyjściowych.

Aby wyświetlić szczegółowe informacje o błędach związanych z wdrożeniem, wybierz pozycję **Błędy**.

## <a name="next-steps"></a>Kolejne kroki

- Aby dowiedzieć się więcej na temat zarządzania aktualizacjami, w tym dzienniki, dane wyjściowe i błędów, zobacz [rozwiązanie do zarządzania aktualizacjami w usłudze Azure](../operations-management-suite/oms-solution-update-management.md).
