---
title: Inteligentne wykrywanie w usłudze Azure Application Insights | Dokumentacja firmy Microsoft
description: Usługa Application Insights wykonuje automatyczne szczegółowej analizy telemetrii aplikacji i ostrzega o potencjalnych problemach.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2016
ms.author: mbullwin
ms.openlocfilehash: fe1fe5d270dd8eb871301a8ec81375f35b2568da
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096586"
---
# <a name="smart-detection-in-application-insights"></a>Wykrywanie inteligentne w usłudze Application Insights
 Wykrywanie inteligentne automatycznie ostrzega o potencjalnych problemów z wydajnością w aplikacji sieci web. Wykonuje aktywnego analizy telemetrii, że aplikacja wysyła do [usługi Application Insights](app-insights-overview.md). Jeśli istnieje nagły wzrost częstotliwości awarii lub nietypowe wzorce wydajności klienta lub serwera, zostanie wyświetlony alert. Ta funkcja wymaga żadna konfiguracja. Działa on tak, jeśli aplikacja wysyła taką ilość telemetrii.

Alerty wykrywania inteligentnego dostęp zarówno z wiadomości e-mail, które otrzymujesz, jak i w bloku wykrywania inteligentnego.

## <a name="review-your-smart-detections"></a>Zapoznaj się z rozwiązaniami do wykrywania inteligentnego
Możesz odkryć wykrywania na dwa sposoby:

* **Otrzymasz wiadomość e-mail** z usługi Application Insights. Oto typowy przykład:
  
    ![Alert e-mail](./media/app-insights-proactive-diagnostics/03.png)
  
    Kliknij przycisk duże, aby otworzyć bardziej szczegółowo w portalu.
* **Na kafelku wykrywania inteligentnego** w Twojej aplikacji — omówienie blok liczbę ostatnich alertów. Kliknij Kafelek, aby wyświetlić listę ostatnich alertów.

![Wyświetl ostatnie wykrywania](./media/app-insights-proactive-diagnostics/04.png)

Wybierz alert, aby wyświetlić jego szczegóły.

## <a name="what-problems-are-detected"></a>Jakie problemy są wykrywane?
Istnieją trzy rodzaje wykrywania:

* [Wykrywanie inteligentne — anomalie](app-insights-proactive-failure-diagnostics.md). Firma Microsoft korzysta z uczenia maszynowego można ustawić oczekiwana liczba nieudanych żądań dla aplikacji, korelacji z obciążeniem i innych czynników. Współczynnik błędów przekroczy oczekiwanego koperty, wysyłamy alertu.
* [Inteligentne wykrywanie - anomalie wydajność](app-insights-proactive-performance-diagnostics.md). Otrzymuj powiadomienia, jeśli czas reakcji operacji lub zależności czasu trwania spowalnia w porównaniu do historycznych linii bazowych lub nazywamy nietypowego wzorca w czasie odpowiedzi lub czas ładowania strony.   
* [Inteligentne wykrywanie — problemy z usługą w chmurze Azure](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). Możesz otrzymywać alerty, jeśli aplikacja jest hostowana w usłudze Azure Cloud Services i wystąpienia roli wystąpiły błędy podczas uruchamiania, częste odtwarzanie lub awarie środowiska uruchomieniowego.

(Każde powiadomienie łącza pomocy prowadzą do odpowiednich artykułów.)

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Kolejne kroki
Te narzędzia diagnostyczne ułatwiają sprawdzanie danych telemetrycznych z Twojej aplikacji:

* [Eksplorator metryk](app-insights-metrics-explorer.md)
* [Eksplorator wyszukiwania](app-insights-diagnostic-search.md)
* [Analiza — zaawansowany język zapytań](app-insights-analytics-tour.md)

Wykrywanie inteligentne jest całkowicie automatyczny. A może chcesz skonfigurować niektóre alerty więcej?

* [Ręcznie skonfigurowane alertów dotyczących metryk](app-insights-alerts.md)
* [Testy sieci web dostępności](app-insights-monitor-web-app-availability.md) 

