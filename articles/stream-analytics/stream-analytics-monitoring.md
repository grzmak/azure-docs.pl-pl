---
title: Zrozumienie zadanie monitorowania w programie Azure Stream Analytics
description: W tym artykule opisano sposób monitorowania zadania usługi analiza strumienia Azure
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 4b048705c80b7776ab0ab6823e27659a01eedeb5
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907454"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Informacje o monitorowaniu zadania usługi analiza strumienia i jak monitorować zapytań

## <a name="introduction-the-monitor-page"></a>Wprowadzenie: Strona monitorowania
Azure portalu, zarówno powierzchni metryki wydajności, który może służyć do monitorowania i rozwiązywania problemów z wydajność zapytań i zadania. Aby wyświetlić te metryki, przejdź do zadania usługi analiza strumienia są wyświetlenie metryki i wyświetlić **monitorowanie** sekcji na stronie przeglądu.  

![Monitorowanie łącza](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Zostanie wyświetlone okno, jak pokazano:

![Monitorowanie zadań pulpitu nawigacyjnego](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metryki dostępne dla usługi analiza strumienia
| Metryka                 | Definicja                               |
| ---------------------- | ---------------------------------------- |
| % wykorzystania SU       | Wykorzystanie jednostek przesyłania strumieniowego przypisany do zadania z kartę Skala zadania. Jeżeli ten wskaźnik osiągnie 80%, lub powyżej, istnieje wysokie prawdopodobieństwo, czy przetwarzanie zdarzeń może zostać opóźnione lub zatrzymana czyni postępy. |
| Zdarzenia wejściowe           | Ilość danych odebranych przez zadanie usługi Stream Analytics, liczbę zdarzeń. Może być używany do sprawdzania, czy zdarzenia są wysyłane do źródła danych wejściowych. |
| Zdarzenia wyjściowe          | Ilość danych przesyłanych przez zadanie usługi Stream Analytics do obiektu docelowego dane wyjściowe w liczby zdarzeń. |
| Zdarzenia poza kolejnością.    | Liczba zdarzeń odebranych poza kolejnością, które zostały porzucone lub podane skorygowaną sygnatury czasowej, opierając się na zasadzie kolejności zdarzeń. Może to mieć wpływ konfigurację ustawienia porządku poza tolerancji. |
| Błędy konwersji danych | Liczba błędów konwersji danych poniesionych przez zadanie usługi Stream Analytics. |
| Błędy w czasie wykonywania         | Całkowita liczba błędów związanych z przetwarzania zapytania (z wyjątkiem znaleziono błędy podczas wprowadzania zdarzenia lub wyniki outputing) |
| Opóźnione zdarzenia wejściowe      | Liczba zdarzeń przybywających późne ze źródła, którego albo została porzucona lub ich sygnatury czasowej została zmieniona na prawidłową, na podstawie konfiguracji zasady kolejność zdarzeń ustawienie późne przyjęcia tolerancji. |
| Żądania funkcji      | Liczba wywołań funkcji usługi Azure Machine Learning (jeśli istnieje). |
| Żądania funkcji zakończone niepowodzeniem | Liczba zakończonych niepowodzeniem wywołania funkcji usługi Azure Machine Learning (jeśli istnieje). |
| Zdarzenia funkcji        | Liczba zdarzeń wysłanych do funkcji usługi Azure Machine Learning (jeśli istnieje). |
| Zdarzenia wejściowe (bajty)      | Ilość danych odebranych przez zadanie usługi Stream Analytics, w bajtach. Może być używany do sprawdzania, czy zdarzenia są wysyłane do źródła danych wejściowych. |


## <a name="customizing-monitoring-in-the-azure-portal"></a>Dostosowywanie monitorowania w portalu Azure
Można dopasować typ wykresu, metryki wyświetlane i zakres w ustawieniach Edytuj wykres czasu. Aby uzyskać więcej informacji, zobacz [jak dostosować monitorowanie](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Wykres monitorowania godziny zapytań](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Najnowsze dane wyjściowe
Innego interesujące punkt danych, aby monitorować zadanie po raz ostatni dane wyjściowe, wyświetlany na stronie przeglądu.
Ten czas jest czas aplikacji (tj. czas za pomocą sygnatury czasowej ze źródła danych zdarzeń) najnowsze dane wyjściowe zadania.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Kolejne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

