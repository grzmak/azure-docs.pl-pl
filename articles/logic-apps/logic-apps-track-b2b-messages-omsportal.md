---
title: Śledzenie komunikatów B2B w usłudze Azure Log Analytics — Azure Logic Apps | Dokumentacja firmy Microsoft
description: Ścieżki komunikacji B2B dla konta integracji i Azure Logic Apps z usługą Azure Log Analytics
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 06/19/2018
ms.openlocfilehash: 666c998a781f13ea2a26ccfc0b94aeead0308f5b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405688"
---
# <a name="track-b2b-communication-with-azure-log-analytics"></a>Śledzenie B2B komunikacji z usługą Azure Log Analytics

Po skonfigurowaniu komunikacji B2B między dwiema uruchomionych procesów biznesowych lub aplikacji za pośrednictwem Twojego konta integracji tych jednostek mogą wymieniać komunikaty ze sobą. Aby sprawdzić, czy te komunikaty są przetwarzane prawidłowo, można śledzić AS2, X12, i EDIFACT komunikatów za pomocą [usługi Azure Log Analytics](../log-analytics/log-analytics-overview.md). Na przykład można użyć tych możliwości opartych na sieci web śledzenia do śledzenia komunikatów:

* Liczba komunikatów i stan
* Stan potwierdzenia
* Korelowanie komunikatów za pomocą potwierdzenia
* Szczegółowy komunikat o błędzie opisy błędów
* Możliwości wyszukiwania

## <a name="requirements"></a>Wymagania

* Aplikację logiki, która została skonfigurowana za pomocą rejestrowania diagnostycznego. Dowiedz się, [jak utworzyć aplikację logiki](quickstart-create-first-logic-app-workflow.md) i [jak skonfigurować rejestrowanie dla tej aplikacji logiki](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Konto integracji skonfigurowanej z funkcjami monitorowania i rejestrowania. Dowiedz się, [jak utworzyć konto integracji](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) i [jak skonfigurować monitorowanie i rejestrowanie dla tego konta](../logic-apps/logic-apps-monitor-b2b-message.md).

* Jeśli jeszcze nie, [publikowania danych diagnostycznych do usługi Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Po poprzednie wymagania zostały spełnione, należy mieć obszar roboczy w usłudze Log Analytics. Należy używać tego samego obszaru roboczego do śledzenia komunikacji B2B w usłudze Log Analytics. 
>  
> Dowiedz się, jeśli nie masz obszaru roboczego usługi Log Analytics [jak utworzyć obszar roboczy usługi Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="add-the-logic-apps-b2b-solution-to-azure"></a>Dodawanie rozwiązania B2B aplikacji logiki na platformie Azure

Aby śledzenie komunikatów B2B dla twojej aplikacji logiki w usłudze Log Analytics, należy dodać **B2B aplikacji logiki** rozwiązania do usługi Log Analytics. Dowiedz się więcej o [Dodawanie rozwiązań do usługi Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

1. W [witryny Azure portal](https://portal.azure.com), wybierz **wszystkich usług**. Wyszukaj "log analytics", a następnie wybierz **usługi Log Analytics** jak pokazano poniżej:

   ![Znajdowanie usługi Log Analytics](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. W obszarze **usługi Log Analytics**, Znajdź i wybierz swój obszar roboczy usługi Log Analytics. 

   ![Wybierz swój obszar roboczy usługi Log Analytics](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. W obszarze **zarządzania**, wybierz **podsumowanie obszaru roboczego**.

   ![Wybierz portal usługi Log Analytics](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Po otwarciu strony głównej wybierz **Dodaj** do zainstalowania rozwiązania B2B aplikacji logiki.    
   ![Wybierz z galerii rozwiązań](media/logic-apps-track-b2b-messages-omsportal/add-b2b-solution.png)

5. W obszarze **rozwiązań do zarządzania**, znajdowanie i tworzenie **B2B aplikacji logiki** rozwiązania.     
   ![Wybierz B2B aplikacji logiki](media/logic-apps-track-b2b-messages-omsportal/create-b2b-solution.png)

   Na stronie głównej, a na kafelku **komunikatów B2B usługi Logic Apps** będzie teraz widoczna. 
   Ten Kafelek aktualizuje liczba komunikatów podczas przetwarzania komunikatów B2B.

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-log-analytics"></a>Śledzenie stanu i szczegółów wiadomości w usłudze Log Analytics

1. Po przetworzeniu wiadomości B2B można wyświetlić stan i szczegóły dotyczące tych komunikatów. Na stronie Przegląd wybierz **komunikatów B2B usługi Logic Apps** kafelka.

   ![Liczba zaktualizowanych komunikatów](media/logic-apps-track-b2b-messages-omsportal/b2b-overview-tile.png)

   > [!NOTE]
   > Domyślnie **komunikatów B2B usługi Logic Apps** kafelkowi danych, w oparciu o jeden dzień. Aby zmienić zakres danych na inny interwał, wybierz formant zakresu w górnej części strony:
   > 
   > ![Zmień zakres danych](media/logic-apps-track-b2b-messages-omsportal/server-filter.png)
   >

2. Po wiadomości stan zostanie wyświetlony pulpit nawigacyjny, można wyświetlić więcej szczegółów dla typu szczegółowy komunikat o błędzie, który wyświetla dane w oparciu o jeden dzień. Wybierz Kafelek **AS2**, **X12**, lub **EDIFACT**.

   ![Wyświetlanie komunikatu stanu](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Zostanie wyświetlona lista komunikatów dla wybranej kafelka. 
   Aby dowiedzieć się więcej na temat właściwości dla każdego typu komunikatu, zobacz opisy te właściwości wiadomości:

   * [Właściwości komunikatu AS2](#as2-message-properties)
   * [Właściwości wiadomości X12](#x12-message-properties)
   * [Właściwości komunikatu EDIFACT](#EDIFACT-message-properties)

   Na przykład Oto jak może wyglądać lista komunikatu AS2:

   ![Wyświetlanie komunikatów AS2](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Aby wyświetlić lub wyeksportować dane wejściowe i wyjściowe dla określone komunikaty, zaznacz je, a następnie wybierz **Pobierz**. Po wyświetleniu monitu, Zapisz plik zip na komputer lokalny, a następnie Wyodrębnij ten plik. 

   Wyodrębniony folder zawiera folder dla każdego wybranego komunikatu. 
   Jeśli skonfigurowano potwierdzeń folderu wiadomości zawiera również pliki ze szczegółami potwierdzenia. 
   Każdy folder komunikatu ma co najmniej następujące pliki: 
   
   * Czytelny dla człowieka plików za pomocą ładunek danych wejściowych i szczegóły ładunek danych wyjściowych
   * Pliki zakodowane z wejściami i wyjściami

   Dla każdego typu komunikatu można znaleźć folderu i formaty nazwy pliku:

   * [Formaty nazw plików i folderów AS2](#as2-folder-file-names)
   * [X12 folderowi i plikowi nazwę formatów](#x12-folder-file-names)
   * [Formaty nazw plików i folderów EDIFACT](#edifact-folder-file-names)

   ![Pobierz pliki wiadomości](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Aby wyświetlić wszystkie akcje, które mają taki sam identyfikator przebiegu, na **wyszukiwanie w dzienniku** wybierz wiadomość z listy wiadomości.

   Możesz sortować te akcje według kolumny lub wyszukiwanie określonych wyników.

   ![Akcje z takimi samymi Identyfikator przebiegu](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Aby wyniki za pomocą wbudowanych zapytań wyszukiwania, wybierz opcję **ulubione**.

   * Dowiedz się, [jak tworzyć zapytania, dodając filtry](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Lub Dowiedz się więcej o [sposób znaleźć dane za pomocą funkcji wyszukiwań w usłudze Log Analytics](../log-analytics/log-analytics-log-searches.md).

   * Aby Zmień zapytanie w polu wyszukiwania, należy zaktualizować zapytania przy użyciu kolumny i wartości, które chcesz użyć jako filtry.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Opisy właściwości i formatów nazwy dla pola AS2, X 12 i komunikatów EDIFACT

Dla każdego typu komunikatu poniżej przedstawiono opisy właściwości i formaty nazw plików pobranych komunikatów.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Opisy właściwości komunikatu AS2

Poniżej przedstawiono opisy właściwości dla każdego komunikatu AS2.

| Właściwość | Opis |
| --- | --- |
| Nadawca | Partner gościa, określone w **ustawienia odbierania**, lub partner hosta określona w **ustawienia wysyłania** umowy AS2 |
| Odbiornik | Partner hosta określona w **ustawienia odbierania**, lub określone partner gościa **ustawienia wysyłania** umowy AS2 |
| Aplikacja logiki | Aplikacja logiki gdzie skonfigurować akcje AS2 |
| Stan | Stan komunikatu AS2 <br>Powodzenie = odebranych lub wysłanych prawidłowy komunikat AS2. Nie powiadomienia MDN jest skonfigurowany. <br>Powodzenie = odebranych lub wysłanych prawidłowy komunikat AS2. Ustawianie i odbierane powiadomienia MDN, lub są wysyłane powiadomienia MDN. <br>Nie powiodło się = Odebrano nieprawidłowy komunikat AS2. Nie powiadomienia MDN jest skonfigurowany. <br>Oczekujące = odebranych lub wysłanych prawidłowy komunikat AS2. Skonfigurowano powiadomienia MDN, a oczekiwano powiadomienia MDN. |
| Potwierdzenia | Stan komunikatu MDN <br>Zaakceptowane = odebranych lub wysłanych dodatnią powiadomienia MDN. <br>Oczekujące = oczekiwanie na odbierania lub wysyłania komunikatu MDN. <br>Odrzucone = odebrane lub wysyłane ujemna powiadomienia MDN. <br>Niewymagane = powiadomienia MDN nie jest skonfigurowany w umowie. |
| Kierunek | Kierunek komunikatu AS2 |
| Identyfikator korelacji | Identyfikator, który jest skorelowane wszystkich wyzwalaczy i akcji w aplikacji logiki |
| Identyfikator wiadomości | Identyfikator komunikatu AS2 z nagłówków komunikatu AS2 |
| Znacznik czasu | Czas, kiedy akcja AS2 przetworzony komunikat |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>AS2 formaty nazw plików pobranych komunikatów

Poniżej przedstawiono formaty nazw dla każdej pobranej folderu komunikatu AS2 i plików.

| Plik lub folder | Format nazwy |
| :------------- | :---------- |
| Folder wiadomości | [nadawcy] \_[odbiorcy]\_AS2\_[identyfikator korelacji]\_[identyfikator komunikatu]\_[sygnatura czasowa] |
| Dane wejściowe, dane wyjściowe i konfigurowanie plików potwierdzenie, jeśli | **Wprowadzony ładunek**: [nadawcy]\_[odbiorcy]\_AS2\_[identyfikator korelacji]\_input_payload.txt </p>**Ładunek danych wyjściowych**: [nadawcy]\_[odbiorcy]\_AS2\_[identyfikator korelacji]\_dane wyjściowe\_payload.txt </p></p>**Dane wejściowe**: [nadawcy]\_[odbiorcy]\_AS2\_[identyfikator korelacji]\_inputs.txt </p></p>**Dane wyjściowe**: [nadawcy]\_[odbiorcy]\_AS2\_[identyfikator korelacji]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 komunikatu opisy właściwości

Poniżej przedstawiono opisy właściwości dla każdego X12 wiadomości.

| Właściwość | Opis |
| --- | --- |
| Nadawca | Partner gościa, określone w **ustawienia odbierania**, lub partner hosta określona w **ustawienia wysyłania** dla X12 umowy |
| Odbiornik | Partner hosta określona w **ustawienia odbierania**, lub określone partner gościa **ustawienia wysyłania** dla X12 umowy |
| Aplikacja logiki | Aplikacja logiki gdzie X12 akcje zostały skonfigurowane. |
| Stan | X12 komunikatu stanu <br>Powodzenie = odebranych lub wysłanych X12 prawidłowy komunikat. Nie potwierdzenia funkcjonalności jest skonfigurowany. <br>Powodzenie = odebranych lub wysłanych X12 prawidłowy komunikat. Potwierdzenia funkcjonalnych została ustawiona i odebranych lub funkcjonalności potwierdzenia są wysyłane. <br>Nie powiodło się = odebranych lub wysłanych X12 nieprawidłowy komunikat. <br>Oczekujące = odebranych lub wysłanych X12 prawidłowy komunikat. Funkcjonalności potwierdzenia jest skonfigurowany, a oczekiwano funkcjonalności potwierdzenia. |
| Potwierdzenia | Funkcjonalności stan potwierdzenia (997) <br>Zaakceptowane = odebranych lub wysłanych pozytywne potwierdzenie funkcjonalności <br>Odrzucone = odebranych lub wysłanych ujemna potwierdzenie funkcjonalności <br>Oczekujące =, oczekiwano potwierdzenia funkcjonalności, ale nie otrzymano. <br>Oczekujące = wygenerowana potwierdzenia funkcjonalności, ale nie można wysłać do partnera. <br>Niewymagane = funkcjonalnej potwierdzenia nie zostało skonfigurowane. |
| Kierunek | X12 komunikatu kierunku |
| Identyfikator korelacji | Identyfikator, który jest skorelowane wszystkich wyzwalaczy i akcji w aplikacji logiki |
| Typ komunikatu | Typ komunikatu 12 EDI, X |
| ICN | Numer kontrolny wymiany dla X12 wiadomości |
| TSCN | Ustaw numer kontrolny transakcji dla X12 wiadomości |
| Znacznik czasu | Podczas gdy X12 akcji przetwarzania wiadomości |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 nazwy formatów plików pobranych komunikatów

Poniżej przedstawiono formaty nazwy dla każdego pobrane X12 folderów i plików komunikatów.

| Plik lub folder | Format nazwy |
| :------------- | :---------- |
| Folder wiadomości | [nadawcy] \_[odbiorcy]\_X12\_[numer wymiany kontrolny]\_[numer globalnej kontroli]\_[transakcji — — numer kontrolny zestawu-]\_[sygnatura czasowa] |
| Dane wejściowe, dane wyjściowe i konfigurowanie plików potwierdzenie, jeśli | **Wprowadzony ładunek**: [nadawcy]\_[odbiorcy]\_X12\_[numer wymiany kontrolny]\_input_payload.txt </p>**Ładunek danych wyjściowych**: [nadawcy]\_[odbiorcy]\_X12\_[numer wymiany kontrolny]\_dane wyjściowe\_payload.txt </p></p>**Dane wejściowe**: [nadawcy]\_[odbiorcy]\_X12\_[numer wymiany kontrolny]\_inputs.txt </p></p>**Dane wyjściowe**: [nadawcy]\_[odbiorcy]\_X12\_[numer wymiany kontrolny]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Opisy właściwości komunikatu EDIFACT

Poniżej przedstawiono opisy właściwości dla każdego komunikatu EDIFACT.

| Właściwość | Opis |
| --- | --- |
| Nadawca | Partner gościa, określone w **ustawienia odbierania**, lub partner hosta określona w **ustawienia wysyłania** umowy EDIFACT |
| Odbiornik | Partner hosta określona w **ustawienia odbierania**, lub określone partner gościa **ustawienia wysyłania** umowy EDIFACT |
| Aplikacja logiki | Aplikacja logiki gdzie skonfigurować akcje EDIFACT |
| Stan | Stan komunikatu EDIFACT <br>Powodzenie = odebranych lub wysłanych prawidłowy komunikat EDIFACT. Nie potwierdzenia funkcjonalności jest skonfigurowany. <br>Powodzenie = odebranych lub wysłanych prawidłowy komunikat EDIFACT. Potwierdzenia funkcjonalnych została ustawiona i odebranych lub funkcjonalności potwierdzenia są wysyłane. <br>Nie powiodło się = odebranych lub wysłanych nieprawidłowy komunikat EDIFACT <br>Oczekujące = odebranych lub wysłanych prawidłowy komunikat EDIFACT. Funkcjonalności potwierdzenia jest skonfigurowany, a oczekiwano funkcjonalności potwierdzenia. |
| Potwierdzenia | Funkcjonalności stan potwierdzenia (997) <br>Zaakceptowane = odebranych lub wysłanych pozytywne potwierdzenie funkcjonalności <br>Odrzucone = odebranych lub wysłanych ujemna potwierdzenie funkcjonalności <br>Oczekujące =, oczekiwano potwierdzenia funkcjonalności, ale nie otrzymano. <br>Oczekujące = wygenerowana potwierdzenia funkcjonalności, ale nie można wysłać do partnera. <br>Niewymagane = funkcjonalności potwierdzenia nie zostało skonfigurowane. |
| Kierunek | Kierunek komunikatu EDIFACT |
| Identyfikator korelacji | Identyfikator, który jest skorelowane wszystkich wyzwalaczy i akcji w aplikacji logiki |
| Typ komunikatu | Typ komunikatu EDIFACT |
| ICN | Numer kontrolny wymiany komunikatu EDIFACT |
| TSCN | Ustaw numer kontrolny transakcji dla komunikatu EDIFACT |
| Znacznik czasu | Czas, kiedy akcja EDIFACT przetworzony komunikat |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>Formaty nazw EDIFACT plików pobranych komunikatów

Poniżej przedstawiono formaty nazw dla każdej pobranej folderu komunikatu EDIFACT i plików.

| Plik lub folder | Format nazwy |
| :------------- | :---------- |
| Folder wiadomości | [nadawcy] \_[odbiorcy]\_EDIFACT\_[numer wymiany kontrolny]\_[numer globalnej kontroli]\_[transakcji — — numer kontrolny zestawu-]\_[sygnatura czasowa] |
| Dane wejściowe, dane wyjściowe i konfigurowanie plików potwierdzenie, jeśli | **Wprowadzony ładunek**: [nadawcy]\_[odbiorcy]\_EDIFACT\_[numer wymiany kontrolny]\_input_payload.txt </p>**Ładunek danych wyjściowych**: [nadawcy]\_[odbiorcy]\_EDIFACT\_[numer wymiany kontrolny]\_dane wyjściowe\_payload.txt </p></p>**Dane wejściowe**: [nadawcy]\_[odbiorcy]\_EDIFACT\_[numer wymiany kontrolny]\_inputs.txt </p></p>**Dane wyjściowe**: [nadawcy]\_[odbiorcy]\_EDIFACT\_[numer wymiany kontrolny]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Kolejne kroki

* [Wykonywanie zapytań dotyczących komunikatów B2B w usłudze Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Schematy śledzenia AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Schematy śledzenia X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Schematy śledzenia niestandardowe](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)
