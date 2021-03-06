---
title: Alerty i metryki DNS platformy Azure | Dokumentacja firmy Microsoft
description: Więcej informacji na temat usługi Azure DNS, metryki i alerty.
services: dns
documentationcenter: na
author: vhorne
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: victorh
ms.openlocfilehash: de29c24556522abeaff8d942edc027c7444c3ed3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965029"
---
# <a name="azure-dns-metrics-and-alerts"></a>Alerty i metryki DNS platformy Azure
System DNS Azure jest usługą hostingu dla domen DNS, która umożliwia rozpoznawanie nazw przy użyciu infrastruktury Microsoft Azure. W tym artykule opisano, metryk i alertów w usłudze DNS platformy Azure.

## <a name="azure-dns-metrics"></a>Metryki usługi Azure DNS

Usługa DNS platformy Azure generuje dane pomiarowe dla klientów umożliwić im monitorowania określonych aspektów ich tych hostowanych w usłudze DNS. Ponadto za pomocą metryk usługi Azure DNS można skonfigurować i otrzymywać alerty na podstawie warunków zainteresowania. Metryki są udostępniane za pośrednictwem [usługi Azure Monitor](../azure-monitor/index.yml). Usługa DNS platformy Azure zawiera następujące metryki za pomocą usługi Azure Monitor dla stref DNS:

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

Można również wyświetlić [definicji tych metryk](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkdnszones) na stronie dokumentacji usługi Azure Monitor.
>[!NOTE]
> W tej chwili te metryki są dostępne tylko dla publicznymi strefami DNS hostowana w usłudze Azure DNS. W przypadku stref prywatnych, hostowanych w usłudze Azure DNS tych metryk nie będzie dostarczać dane dla tych stref. Ponadto metryk i alertów funkcja jest obsługiwana tylko w chmurze publicznej Azure. Obsługa chmur suwerennych będzie występować w późniejszym czasie. 

Precyzyjne wymiaru dla tych metryk jest strefy DNS.

### <a name="query-volume"></a>Wolumin zapytań

*Wolumin zapytań* metryki w usłudze Azure DNS przedstawia wolumin zapytań DNS (zapytanie ruch), odebrane przez usługę Azure DNS dla strefy DNS. Jednostka miary jest licznik i agregacji to suma wszystkich zapytań odebranych za pośrednictwem przez pewien czas. Aby wyświetlić tę metrykę, wybierać interfejs Eksploratora metryk (wersja zapoznawcza) na karcie monitorowanie w witrynie Azure portal. Wybierz strefę DNS z listy rozwijanej zasobów, wybierz metrykę wolumin zapytań i wybierz sumy jako agregacja. Poniższy zrzut ekranu przedstawia przykład.  Aby uzyskać więcej informacji na temat Eksploratora metryk środowiska i wykresów, zobacz [Eksploratora metryk usługi Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-charts.md).

![Wolumin zapytań](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Rysunku: Metryki platformy Azure wolumin zapytań DNS*

### <a name="record-set-count"></a>Liczba zestawu rekordów
*Liczba zestawu rekordów* Metryka przedstawia liczbę zestawów rekordów w usłudze Azure DNS dla strefy DNS. Zliczane są wszystkie zdefiniowanych zestawów rekordów w strefie. Jednostka miary jest licznik i agregacji jest maksymalną liczbę wszystkich zestawów rekordów. Zaznacz, aby wyświetlić tę metrykę **metryki (wersja zapoznawcza)** środowisko Eksploratora z **Monitor** kartę w witrynie Azure portal. Wybierz strefę DNS z **zasobów** listę rozwijaną, wybierz opcję **liczba zestawu rekordów** metryki, a następnie wybierz **Max** jako **agregacji** . Aby uzyskać więcej informacji na temat Eksploratora metryk środowiska i wykresów, zobacz [Eksploratora metryk usługi Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Liczba zestawu rekordów](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Rysunku: Metryki usługi Azure DNS Ustaw liczbę rekordów*


### <a name="record-set-capacity-utilization"></a>Wykorzystanie pojemności zestawu rekordów
*Wykorzystanie pojemności zestawu rekordów* metryki w usłudze Azure DNS przedstawia wartość procentową wykorzystania pojemności zestawu rekordów dla strefy DNS. Każdej strefy DNS w usłudze Azure DNS podlega limitu zestawu rekordów, który określa maksymalną liczbę zestawów rekordów, które są dozwolone dla tej strefy (zobacz [limity DNS](dns-zones-records.md#limits)). Dzięki temu ta metryka dowiesz się, jak blisko są w celu osiągnięcia limitu zestawu rekordów. Na przykład jeśli masz 500 zestawy rekordów skonfigurowany dla strefy DNS i strefy ma domyślny limit 5000 rekordów, metryki RecordSetCapacityUtilization wyświetli wartość 10%, (które są uzyskiwane przez 500 podziału, 5000). Jest to jednostka miary **procent** i **agregacji** typ jest **maksymalna**. Aby wyświetlić tę metrykę, wybierać interfejs Eksploratora metryk (wersja zapoznawcza) na karcie monitorowanie w witrynie Azure portal. Wybierz strefę DNS z listy rozwijanej zasobów, wybierz metryki użycia pojemności zestawu rekordów i wybierz maksymalny jako agregacja. Poniższy zrzut ekranu przedstawia przykład. Aby uzyskać więcej informacji na temat Eksploratora metryk środowiska i wykresów, zobacz [Eksploratora metryk usługi Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Liczba zestawu rekordów](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Rysunku: Metryki usługi Azure DNS wykorzystanie pojemności zestawu rekordów*

## <a name="alerts-in-azure-dns"></a>Alerty w usłudze Azure DNS
Usługa Azure Monitor zapewnia możliwość alert względem dostępne wartości metryk. Metryki DNS są dostępne w nowym środowisku podczas konfigurowania alertu. Zgodnie z opisem w części [usługi Azure Monitor alertów dokumentacji](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), wybierz strefę DNS jako zasobu, wybierz typ sygnału metryki i Konfigurowanie alertów logiki oraz innych parametrów, takich jak **okres**i **częstotliwość**. Można uściślić [grupy akcji](../monitoring-and-diagnostics/monitoring-action-groups.md) dla kiedy warunek alertu jest spełniony, zgodnie z którą alert będzie świadczona za pośrednictwem wybranego działania. Aby uzyskać więcej informacji na temat konfigurowania alertów w połączeniu z metrykami usługi Azure Monitor, zobacz [Utwórz, Wyświetl, alerty i zarządzaj nimi przy użyciu usługi Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). 

## <a name="next-steps"></a>Kolejne kroki
- Dowiedz się więcej o [system DNS Azure](dns-overview.md).
