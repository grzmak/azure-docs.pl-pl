---
title: Dzienniki aktywności usługi Azure Active Directory w usłudze Azure Monitor (wersja zapoznawcza) | Microsoft Docs
description: Omówienie dzienników aktywności usługi Azure Active Directory w usłudze Azure Monitor (wersja zapoznawcza)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: concept
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 3a4fc814a40bf370a137a2045c6218d3ee4b8778
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49395653"
---
# <a name="azure-ad-activity-logs-in-azure-monitor-preview"></a>Dzienniki aktywności usługi Azure AD w usłudze Azure Monitor (wersja zapoznawcza)

Możesz teraz kierować dzienniki aktywności usługi Azure Active Directory (Azure AD) na własne konto magazynu lub do centrum zdarzeń przy użyciu usługi Azure Monitor. Publiczna wersja zapoznawcza dzienników usługi Azure Active Directory w usłudze Azure Monitor umożliwia:

* Archiwizowanie dzienników inspekcji dla konta usługi Azure Storage, co pozwala przechowywać dane przez długi czas.
* Przesyłanie strumieniowe dzienników inspekcji do centrum zdarzeń platformy Azure na potrzeby analizy przeprowadzanej przy użyciu popularnych narzędzi do zarządzania informacjami i zdarzeniami zabezpieczeń (SIEM), takich jak Splunk i QRadar.
* Integrowanie dzienników inspekcji z niestandardowymi rozwiązaniami dziennika przez przesyłanie ich strumieniowo do centrum zdarzeń.
* Dzienniki aktywności wysyłania usługi Azure AD do usługi Log Analytics, aby umożliwić rozbudowane wizualizacje, monitorowania i alertów dla połączonych danych.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Obsługiwane raporty

Przy użyciu tej funkcji można przekierowywać dzienniki aktywności dotyczące inspekcji i dzienniki aktywności dotyczące logowań na konto usługi Azure Storage, do centrum zdarzeń lub niestandardowego rozwiązania. 

* **Dzienniki inspekcji**: [raport działań dotyczący dzienników inspekcji](concept-audit-logs.md) zapewnia dostęp do historii wszystkich zadań wykonanych w dzierżawie.
* **Dzienniki logowania**: przy użyciu [raportu działań dotyczącego logowań](concept-sign-ins.md) można określić, kto wykonał zadania zgłoszone w dziennikach inspekcji.

> [!NOTE]
> Dzienniki aktywności inspekcji i logowania związane z funkcjami B2C nie są obecnie obsługiwane.
>

## <a name="prerequisites"></a>Wymagania wstępne

Aby używać tej funkcji, potrzebujesz następujących elementów:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji platformy Azure, możesz skorzystać z [bezpłatnej wersji próbnej](https://azure.microsoft.com/free/).
* [Licencja](https://azure.microsoft.com/pricing/details/active-directory/) usługi Azure AD w warstwie Bezpłatna, Podstawowa, Premium 1 lub Premium 2, aby uzyskać dostęp do dzienników inspekcji usługi Azure AD w witrynie Azure Portal. 
* Dzierżawa usługi Azure AD.
* Użytkownik będący **administratorem globalnym** lub **administratorem zabezpieczeń** dla tej dzierżawy usługi Azure AD.
* [Licencja](https://azure.microsoft.com/pricing/details/active-directory/) usługi Azure AD w warstwie Premium 1 lub Premium 2, aby uzyskać dostęp do dzienników logowania usługi Azure AD w witrynie Azure Portal. 

W zależności od tego, gdzie chcesz przekierować dane dziennika inspekcji, będziesz potrzebować dowolnego spośród poniższych elementów:

* Konto usługi Azure Storage, do którego masz uprawnienia *ListKeys*. Zalecamy używanie konta magazynu ogólnego, a nie konta magazynu obiektów blob. Aby uzyskać informacje o cenach magazynu, zobacz [kalkulator cen usługi Azure Storage](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Przestrzeń nazw usługi Azure Event Hubs potrzeby integracji z rozwiązaniami innych firm.
* Obszar roboczy usługi Azure Log Analytics do wysyłania dzienników do usługi Log Analytics.

## <a name="cost-considerations"></a>Kwestie związane z kosztami

Jeśli masz już licencję usługi Azure AD, potrzebujesz subskrypcji platformy Azure do skonfigurowania magazynu konta i centrum zdarzeń. Subskrypcja platformy Azure jest dostępna bezpłatnie, ale trzeba płacić za korzystanie z zasobów platformy Azure, w tym konto magazynu używane na potrzeby archiwizacji oraz centrum zdarzeń używane do przesyłania strumieniowego. Ilość danych, a w związku z tym koszt, może różnić się znacznie w zależności od rozmiaru dzierżawy. 

### <a name="storage-size-for-activity-logs"></a>Rozmiar magazynu dla dzienników aktywności

Każde zdarzenie dziennika inspekcji używa około 2 KB magazynu danych. W przypadku dzierżawy z 100 000 użytkowników, którzy generują około 1,5 miliona zdarzeń dziennie, będziesz potrzebować około 3 GB magazynu danych na dzień. Ponieważ operacje zapisu są przetwarzane w partiach w około pięciominutowych odstępach, możesz oczekiwać około 9000 operacji zapisu miesięcznie. 

Poniższa tabela zawiera oszacowanie kosztów w zależności od rozmiaru dzierżawy w przypadku konta magazynu ogólnego przeznaczenia w wersji 2 w regionie Zachodnie stany USA z okresem przechowywania co najmniej jeden rok. Aby utworzyć bardziej dokładne oszacowanie dla ilości danych, którą przewidujesz dla aplikacji, użyj [kalkulatora cen usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/). 

| Kategoria dziennika | Liczba użytkowników | Zdarzenia dziennie | Ilość danych na miesiąc (szac.) | Koszt za miesiąc (szac.) | Koszt za rok (szac.) |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Inspekcja | 100 000 | 1,5&nbsp;mln | 90 GB | 1,93 USD | 23,12 USD |
| Inspekcja | 1000 | 15 000 | 900 MB | 0,02 USD | 0,24 USD |
| Logowania | 1000 | 34 800 | 4 GB | 0,13 USD | 1,56 USD |
| Logowania | 100 000 | 15&nbsp;mln | 1,7 TB | 35,41 USD | 424,92 USD | 


### <a name="event-hub-messages-for-activity-logs"></a>Komunikaty centrum zdarzeń dotyczące dzienników aktywności

Zdarzenia są przetwarzane w partiach w około 5-minutowych odstępach i wysyłane jako pojedynczy komunikat zawierający wszystkie zdarzenia, które wystąpiły w tym przedziale czasu. Maksymalny rozmiar komunikatu w centrum zdarzeń wynosi 256 KB, a jeśli łączny rozmiar wszystkich komunikatów w danym przedziale czasu przekracza ten wolumen, wysyłane jest wiele komunikatów. 

Na przykład w przypadku dużej dzierżawy z ponad 100 000 użytkowników przeważnie występuje 18 zdarzeń na sekundę, co oznacza szybkość wynoszącą 5400 zdarzeń co pięć minut. Ponieważ dzienniki inspekcji mają rozmiar około 2 KB na zdarzenie, odpowiada to 10,8 MB danych. W związku z tym 43 komunikaty są wysyłane do centrum zdarzeń w ciągu tego pięciominutowego interwału. 

Poniższa tabela zawiera szacowany koszt na miesiąc dla podstawowego centrum zdarzeń w regionie Zachodnie stany USA w zależności od ilości danych zdarzeń. W celu obliczenia dokładnego oszacowania dla ilości danych przewidywanej w przypadku danej aplikacji użyj [kalkulatora cen usługi Event Hub](https://azure.microsoft.com/pricing/details/event-hubs/).

| Kategoria dziennika | Liczba użytkowników | Zdarzenia na sekundę | Zdarzenia na pięciominutowy interwał | Wolumen na interwał | Komunikaty na interwał | Komunikaty na miesiąc | Koszt za miesiąc (szac.) |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Inspekcja | 100 000 | 18 | 5400 | 10,8 MB | 43 | 371 520 | 10,83 USD |
| Inspekcja | 1000 | 0.1 | 52 | 104 KB | 1 | 8640 | 10,80 USD |
| Logowania | 1000 | 178 | 53 400 | 106,8&nbsp;MB | 418 | 3 611 520 | 11,06 USD |  

### <a name="log-analytics-cost-considerations"></a>Usługa log Analytics kwestii związanych z kosztami

Aby przejrzeć koszty związane z zarządzaniem obszaru roboczego usługi Log Analytics, zobacz [Zarządzanie kosztami przez kontrolowanie ilości danych i przechowywania w usłudze Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-cost-storage).

## <a name="frequently-asked-questions"></a>Często zadawane pytania

Ta sekcja zawiera odpowiedzi na często zadawane pytania i znane problemy z dziennikami usługi Azure AD w usłudze Azure Monitor.

**Pytanie: Gdzie mam zacząć?** 

**Odpowiedź**: W tym artykule opisano elementy potrzebne do wdrożenia tej funkcji. Po spełnieniu wymagań wstępnych przejdź do samouczków, które mogą pomóc skonfigurować dzienniki i skierować je do centrum zdarzeń.

---

**Pytanie: Które dzienniki są uwzględnione?**

**Odpowiedź:** Za pomocą tej funkcji można przekierowywać dzienniki logowań i dzienniki inspekcji, chociaż zdarzenia inspekcji związane z usługąB2C nie są obecnie uwzględniane. Aby dowiedzieć się, jakie typy dzienników i które oparte na funkcjach dzienniki są obecnie obsługiwane, zapoznaj się ze [schematem dziennika inspekcji](reference-azure-monitor-audit-log-schema.md) i [schematem dziennika logowania](reference-azure-monitor-sign-ins-log-schema.md). 

---

**Pytanie: Jak szybko po wykonaniu akcji odpowiednie dzienniki pojawiają się w centrach zdarzeń?**

**Odpowiedź**: Dzienniki powinny być widoczne w centrum zdarzeń w ciągu dwóch do pięciu minut po wykonaniu akcji. Aby uzyskać więcej informacji na temat usługi Event Hubs, zobacz [Co to jest usługa Azure Event Hubs?](../../event-hubs/event-hubs-about.md)

---

**Pytanie: Jak szybko po wykonaniu akcji odpowiednie dzienniki pojawiają się na kontach magazynu?**

**Odpowiedź:** W przypadku kont usługi Azure Storage opóźnienie wynosi od 5 do 15 minut po wykonaniu akcji.

---

**Pytanie: Ile będzie kosztować przechowywanie danych?**

**Odpowiedź:** koszt przechowywania danych zależy od rozmiaru dzienników oraz wybranego okresu przechowywania. Aby uzyskać listę szacowanych kosztów dzierżaw, które zależą od woluminu wygenerowanych dzienników, przejdź do sekcji [Rozmiar magazynu dla dzienników aktywności](#storage-size-for-activity-logs).

---

**Pytanie: Ile będzie kosztować przesyłanie strumieniowe danych do centrum zdarzeń?**

**Odpowiedź:** Koszty przesyłania strumieniowego zależą od liczby komunikatów odbieranych na minutę. W tym artykule omówiono sposób obliczania kosztów i przedstawiono listę szacowanych kosztów w oparciu o liczbę komunikatów. 

---

**Pytanie: Jak mogę zintegrować dzienniki aktywności usługi Azure AD z moim systemem SIEM?**

**Odpowiedź**: Możesz to zrobić na dwa sposoby:

- Za pomocą usługi Azure Monitor i usługi Event Hubs prześlij strumieniowo dzienniki do systemu SIEM. Najpierw [prześlij strumieniowo dzienniki do centrum zdarzeń](tutorial-azure-monitor-stream-logs-to-event-hub.md), a następnie [skonfiguruj narzędzie SIEM](tutorial-azure-monitor-stream-logs-to-event-hub.md#access-data-from-your-event-hub) za pomocą skonfigurowanego centrum zdarzeń. 

- Użyj [interfejsu API raportowania programu Graph](concept-reporting-api.md) w celu uzyskania dostępu do danych i wypchnięcia ich do systemu SIEM przy użyciu własnych skryptów.

---

**Pytanie: Jakie narzędzia SIEM są obecnie obsługiwane?** 

**Odpowiedź:** Obecnie usługa Azure Monitor jest obsługiwana przez narzędzia [Splunk](tutorial-integrate-activity-logs-with-splunk.md), QRadar i [Sumo Logic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory). Aby uzyskać więcej informacji na temat sposobu działania łączników, zobacz [Stream Azure monitoring data to an event hub for consumption by an external tool (Przesyłanie strumieniowe danych monitorowania platformy Azure do centrum zdarzeń do użycia przez zewnętrzne narzędzie)](../../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

---

**Pytanie: Jak mogę zintegrować dzienniki aktywności usługi Azure AD z moim wystąpieniem usługi Splunk?**

**Odpowiedź**: Najpierw [przekieruj dzienniki aktywności usługi Azure AD do centrum zdarzeń](quickstart-azure-monitor-stream-logs-to-event-hub.md), a następnie postępuj zgodnie z podanymi instrukcjami, aby [zintegrować dzienniki aktywności z usługą Splunk](tutorial-integrate-activity-logs-with-splunk.md).

---

**Pytanie: Jak mogę zintegrować dzienniki aktywności usługi Azure AD z moją logiką Sumo?** 

**Odpowiedź**: Najpierw [przekieruj dzienniki aktywności usługi Azure AD do centrum zdarzeń](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory), a następnie postępuj zgodnie z podanymi instrukcjami, aby [zainstalować aplikację usługi Azure AD i wyświetlić pulpity nawigacyjne w logice SumoLogic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards).

---

**Pytanie: Czy można uzyskać dostęp do danych z centrum zdarzeń bez użycia zewnętrznego narzędzia SIEM?** 

**Odpowiedź:** Tak. Aby uzyskać dostęp do dzienników z aplikacji niestandardowej, możesz użyć [interfejsu API usługi Event Hubs](../../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md). 

---


## <a name="next-steps"></a>Kolejne kroki

* [Archiwizowanie dzienników aktywności na koncie magazynu](quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Kierowanie dzienników aktywności do centrum zdarzeń](quickstart-azure-monitor-stream-logs-to-event-hub.md)
* [Integrowanie dzienników aktywności z usługą Log Analytics](howto-integrate-activity-logs-with-log-analytics.md)
