---
title: Rozwiązanie Agent Health na platformie Azure | Dokumentacja firmy Microsoft
description: Ten artykuł ma na celu pomóc Ci zrozumieć, jak korzystać z tego rozwiązania do monitorowania kondycji Twoich agentów raportujących bezpośrednio do usługi Log Analytics lub System Center Operations Manager.
services: operations-management-suite
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: magoedte
ms.openlocfilehash: e9ac4b7f7a0900f8b49f95c0db9ba5e9c962fa0e
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404039"
---
#  <a name="agent-health-solution-in-azure"></a>Rozwiązanie Agent Health na platformie Azure
Rozwiązanie Agent Health na platformie Azure pomaga zrozumieć, dla wszystkich agentów raportujących do obszaru roboczego usługi Log Analytics lub System Center Operations Manager grupy zarządzania podłączone bezpośrednio do usługi Log Analytics, które są nie odpowiada i wysyłaniem operacyjne dane.  Można także śledzić liczbę wdrożonych agentów i ich geograficzne rozmieszczenie oraz wykonywać inne zapytania dające informacje na temat rozmieszczenia agentów wdrożonych na platformie Azure, w innych środowiskach w chmurze i lokalnie.    

## <a name="prerequisites"></a>Wymagania wstępne
Przed wdrożeniem tego rozwiązania, upewnij się, masz aktualnie obsługiwanych [agenci Windows](../log-analytics/log-analytics-windows-agent.md) raportowania do obszaru roboczego usługi Log Analytics lub serwerowi [grupy zarządzania programu Operations Manager](../log-analytics/log-analytics-om-agents.md) zintegrowana z usługą obszar roboczy.    

## <a name="solution-components"></a>Składniki rozwiązania
To rozwiązanie składa się z następujących zasobów, które są dodawane do Twojego obszaru roboczego, i bezpośrednio połączonych agentów lub grupy zarządzania połączonej z programem Operations Manager.

### <a name="management-packs"></a>Pakiety administracyjne
Jeśli grupa zarządzania programu System Center Operations Manager jest połączona z obszarem roboczym usługi Log Analytics, następujące pakiety administracyjne są instalowane w programie Operations Manager.  Te pakiety administracyjne są również instalowane na bezpośrednio połączonych komputerach z systemem Windows po dodaniu tego rozwiązania. W przypadku tych pakietów administracyjnych nie trzeba niczego konfigurować ani niczym zarządzać.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack  (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Aby uzyskać więcej informacji na temat aktualizowania pakietów administracyjnych rozwiązania, zobacz artykuł [Connect Operations Manager to Log Analytics](../log-analytics/log-analytics-om-agents.md) (Połączenie programu Operations Manager z usługą Log Analytics).

## <a name="configuration"></a>Konfigurowanie
Dodaj rozwiązanie Agent Health do swojego obszaru roboczego usługi Log Analytics, za pomocą procesu opisanego w [Dodawanie rozwiązań](../log-analytics/log-analytics-add-solutions.md). Nie są wymagane żadne dalsze czynności konfiguracyjne.


## <a name="data-collection"></a>Zbieranie danych
### <a name="supported-agents"></a>Obsługiwani agenci
W poniższej tabeli opisano połączone źródła, które obsługuje to rozwiązanie.

| Połączone źródło | Obsługiwane | Opis |
| --- | --- | --- |
| Agenci dla systemu Windows | Yes | Zdarzenia pulsu są zbierane z bezpośrednich agentów systemu Windows.|
| Grupa zarządzania programu System Center Operations Manager | Yes | Zdarzenia pulsu są zbierane z agentów raportujących do grupy zarządzania co 60 sekund, a następnie przekazywane do usługi Log Analytics. Bezpośrednie połączenie agenta programu Operations Manager z usługą Log Analytics nie jest wymagane. Dane zdarzeń pulsu są przekazywane z grupy zarządzania do repozytorium usługi Log Analytics.|

## <a name="using-the-solution"></a>Użycie rozwiązania
Po dodaniu rozwiązania do obszaru roboczego usługi Log Analytics, **Agent Health** zostanie dodany Kafelek do pulpitu nawigacyjnego. Ten kafelek pokazuje całkowitą liczbę agentów oraz liczbę nieodpowiadających agentów w ciągu ostatnich 24 godzin.<br><br> ![Kafelek rozwiązania Agent Health na pulpicie nawigacyjnym](./media/monitoring-solution-agenthealth/agenthealth-solution-tile-homepage.png)

Kliknij kafelek **Agent Health**, aby otworzyć pulpit nawigacyjny rozwiązania **Agent Health**.  Na pulpicie nawigacyjnym znajdują się kolumny wymienione w poniższej tabeli. Każda kolumna zawiera listę dziesięciu najważniejszych zdarzeń według liczby odpowiadającej kryteriom tej kolumny dla określonego zakresu czasu. Możesz uruchomić przeszukiwanie rejestru zwracające pełną listę, wybierając pozycję **Zobacz wszystko** w prawej dolnej części kolumny lub klikając kolumnę nagłówka.

| Kolumna | Opis |
|--------|-------------|
| Liczba agentów w miarę upływu czasu | Trend liczby agentów w okresie siedmiu dni dla agentów systemu Linux i Windows.|
| Liczba nieodpowiadających agentów | Lista agentów, którzy nie wysłali pulsu w ciągu ostatnich 24 godzin.|
| Rozkład według typu systemu operacyjnego | Rozkład liczby agentów systemu Windows i Linux w Twoim środowisku.|
| Rozkład według wersji agenta | Rozkład różnych wersji agentów zainstalowanych w Twoim środowisku i liczba agentów dla każdej z nich.|
| Rozkład według kategorii agenta | Rozkład różnych kategorii agentów wysyłających zdarzenia pulsu: bezpośredni agenci, agenci programu OpsMgr i serwer zarządzania programu OpsMgr.|
| Rozkład według grupy zarządzania | Rozkład różnych grup zarządzania SCOM w Twoim środowisku.|
| Lokalizacja geograficzna agentów | Rozkład różnych krajów, w których masz agentów, i łączna liczba agentów zainstalowanych w każdym kraju.|
| Liczba zainstalowanych bram | Liczba serwerów, które mają zainstalowanej bramy usługi Log Analytics, a lista tych serwerów.|

![Przykład pulpitu nawigacyjnego rozwiązania Agent Health](./media/monitoring-solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="log-analytics-records"></a>Rekordy usługi Log Analytics
Rozwiązanie tworzy jeden typ rekordu, w obszarze roboczym usługi Log Analytics.  

### <a name="heartbeat-records"></a>Rekordy Heartbeat
Tworzony jest rekord o typie **Heartbeat**.  Te rekordy mają właściwości podane w poniższej tabeli.  

| Właściwość | Opis |
| --- | --- |
| Typ | *Heartbeat*|
| Kategoria | Wartością jest *Direct Agent*, *SCOM Agent* lub *SCOM Management Server*.|
| Computer | Nazwa komputera.|
| OSType | System operacyjny Windows lub Linux.|
| OSMajorVersion | Wersja główna systemu operacyjnego.|
| OSMinorVersion | Wersja pomocnicza systemu operacyjnego.|
| Wersja | Wersja agenta programu log Analytics lub agenta programu Operations Manager.|
| SCAgentChannel | Wartością jest *Direct* i/lub *SCManagementServer*.|
| IsGatewayInstalled | Jeśli zainstalowano bramę usługi Log Analytics, wartość jest *true*, w przeciwnym razie wartość jest *false*.|
| ComputerIP | Adres IP komputera.|
| RemoteIPCountry | Lokalizacja geograficzna, w której wdrożony jest komputer.|
| ManagementGroupName | Nazwa grupy zarządzania programu Operations Manager.|
| SourceComputerId | Unikatowy identyfikator komputera.|
| RemoteIPLongitude | Długość geograficzna lokalizacji geograficznej komputera.|
| RemoteIPLatitude | Szerokość geograficzna lokalizacji geograficznej komputera.|

Każdy agent raportujący do serwera zarządzania programu Operations Manager będzie wysyłać dwa pulsy, a wartość właściwości SCAgentChannel będzie obejmować obie **bezpośredniego** i **SCManagementServer** zależności od tego, jakie dziennika Analiza źródła danych i rozwiązania, które zostały włączone w Twojej subskrypcji. Jeżeli pamiętasz, dane z rozwiązań są albo przesyłane bezpośrednio z serwera zarządzania programu Operations Manager do usługi Log Analytics lub ze względu na ilość danych zgromadzonych na agencie, wysyłane bezpośrednio z agenta do usługi Log Analytics. W przypadku zdarzeń pulsu, które mają wartość **SCManagementServer**, wartość ComputerIP jest adresem IP serwera zarządzania, ponieważ dane są faktycznie przekazywane przez niego.  W przypadku pulsów z parametrem SCAgentChannel ustawionym na wartość **Direct** jest to publiczny adres IP agenta.  

## <a name="sample-log-searches"></a>Przykładowe wyszukiwania dzienników
Poniższa tabela zawiera przykładowe wyszukiwania dzienników dla rekordów zbieranych przez to rozwiązanie.

| Zapytanie | Opis |
|:---|:---|
| Heartbeat &#124; distinct Computer |Łączna liczba agentów |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(24h) |Liczba nieodpowiadających agentów w ciągu ostatnich 24 godzin |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(15m) |Liczba nieodpowiadających agentów w ciągu ostatnich 15 minut |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer in ((Heartbeat &#124; where TimeGenerated > ago(24h) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Komputery w trybie online (w ciągu ostatnich 24 godzin) |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer !in ((Heartbeat &#124; where TimeGenerated > ago(30m) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Całkowita liczba agentów w trybie offline w ciągu ostatnich 30 minut (dla ostatnich 24 godzin) |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Pobierz trend liczby agentów w miarę upływu czasu według wartości OSType|
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Rozkład według typu systemu operacyjnego |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by Version |Rozkład według wersji agenta |
| Heartbeat &#124; summarize AggregatedValue = count() by Category |Rozkład według kategorii agenta |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by ManagementGroupName | Rozkład według grupy zarządzania |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by RemoteIPCountry |Lokalizacja geograficzna agentów |
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |Liczba zainstalowanych bram analizy dzienników |




## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się więcej na temat [alertów w usłudze Log Analytics](../log-analytics/log-analytics-alerts.md), aby poznać szczegóły generowania alertów z usługi Log Analytics.
