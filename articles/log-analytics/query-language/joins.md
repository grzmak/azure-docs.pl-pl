---
title: Sprzężenia w zapytaniach usługi Azure Log Analytics | Dokumentacja firmy Microsoft
description: Ten artykuł zawiera lekcji dotyczących używania sprzężeń w języku zapytań usługi Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 39a461a27e8d9d6d1b9712449586bfabf6124d22
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989451"
---
# <a name="joins-in-log-analytics-queries"></a>Sprzężenia w zapytań usługi Log Analytics

> [!NOTE]
> Należy wykonać [Rozpoczynanie pracy z usługą portalu analiza](get-started-analytics-portal.md) i [wprowadzenie do zapytań](get-started-queries.md) przed wykonaniem tej lekcji.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Przyłączenia pozwalają analizować dane z wielu tabel w jednym zapytaniu. Scalają wiersze dwa zestawy danych, dopasowując wartości określone kolumny.


```Kusto
SecurityEvent 
| where EventID == 4624     // sign-in events
| project Computer, Account, TargetLogonId, LogonTime=TimeGenerated
| join kind= inner (
    SecurityEvent 
    | where EventID == 4634     // sign-out events
    | project TargetLogonId, LogoffTime=TimeGenerated
) on TargetLogonId
| extend Duration = LogoffTime-LogonTime
| project-away TargetLogonId1 
| top 10 by Duration desc
```

W tym przykładzie pierwszego zestawu danych służy do przefiltrowania wszystkich zdarzeń logowania. To jest sprzężony z drugiego zestawu danych, który filtruje dla wszystkich zdarzeń wylogowania. Przewidywany kolumny są _komputera_, _konta_, _TargetLogonId_, i _TimeGenerated_. Zestawy danych są powiązane za pomocą udostępnionego kolumny _TargetLogonId_. Wynikiem jest jeden rekord dla korelacji, który ma czas logowania i wylogowywania.

Jeśli oba zestawy danych ma kolumny z tej samej nazwy, kolumny zestawu danych po prawej stronie zapewnianą numer indeksu, dlatego w tym przykładzie wyniki będą widoczne _TargetLogonId_ przy użyciu wartości z tabeli po lewej stronie i  _TargetLogonId1_ przy użyciu wartości z tabeli po prawej stronie. W tym przypadku drugi _TargetLogonId1_ kolumny został usunięty przy użyciu `project-away` operatora.

> [!NOTE]
> Aby zwiększyć wydajność, należy zachować tylko odpowiednich kolumn sprzężonych-zestawów danych, za pomocą `project` operatora.


Użyj następującej składni, aby dołączyć dwa zestawy danych i połączone klucz ma pod inną nazwą, między dwiema tabelami:
```
Table1
| join ( Table2 ) 
on $left.key1 == $right.key2
```

## <a name="lookup-tables"></a>Tabele odnośników
Typowym zastosowaniem sprzężeń używa statycznego mapowania wartości za pomocą `datatable` , mogą pomóc w transformacji wyniki w sposób bardziej zawartości. Na przykład aby wzbogacić zabezpieczeń dane zdarzeń z nazwą zdarzenia dla każdego zdarzenia identyfikatora.

```Kusto
let DimTable = datatable(EventID:int, eventName:string)
  [
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4658, "The handle to an object was closed",
    4656, "A handle to an object was requested",
    4690, "An attempt was made to duplicate a handle to an object",
    4663, "An attempt was made to access an object",
    5061, "Cryptographic operation",
    5058, "Key file operation"
  ];
SecurityEvent
| join kind = inner
 DimTable on EventID
| summarize count() by eventName
```

![Dołącz do przy użyciu elementu datatable](media/joins/dim-table.png)

## <a name="join-kinds"></a>Dołącz do typów
Określ typ sprzężenia z _rodzaj_ argumentu. Każdy typ wykonuje różne dopasowanie między rekordami w danej tabeli, zgodnie z opisem w poniższej tabeli.

| Typ przyłączenia | Opis |
|:---|:---|
| innerunique | Jest to domyślny tryb złączenia. Najpierw znajdują się wartości dopasowane kolumny w tabeli po lewej stronie, a zduplikowane wartości są usuwane.  Następnie zestaw unikatowych wartości jest dopasowywana tabelą po prawej stronie. |
| wewnętrzny | Tylko pasujące rekordy w obu tabelach są uwzględnione w wynikach. |
| wartości leftouter | Wszystkie rekordy w tabeli po lewej stronie i rekordy w tabeli po prawej znajdują się w wynikach. Właściwości niedopasowane dane wyjściowe zawierają wartości null.  |
| leftanti | Rekordy z lewej strony, które nie mają zgodne z prawej strony znajdują się w wynikach. Tabela wyników zawiera tylko kolumny z tabeli po lewej stronie. |
| leftsemi | Rekordy z lewej strony, które mają zgodne z prawej strony znajdują się w wynikach. Tabela wyników zawiera tylko kolumny z tabeli po lewej stronie. |


## <a name="best-practices"></a>Najlepsze praktyki

Należy wziąć pod uwagę następujące kwestie, aby uzyskać optymalną wydajność:

- Umożliwia filtr czasu dla każdej tabeli ograniczenie rekordy, które muszą być obliczane w celu utworzenia sprzężenia.
- Użyj `where` i `project` zmniejszenie liczby wierszy i kolumn w tabelach danych wejściowych przed sprzężenia.
* Jeśli jedna tabela jest zawsze mniejsza niż ten drugi, użyć jej jako z lewej strony sprzężenia.


## <a name="next-steps"></a>Kolejne kroki
Zobacz inne lekcje dla przy użyciu języka zapytań usługi Log Analytics:

- [Operacje na ciągach](string-operations.md)
- [Funkcje agregacji](aggregations.md)
- [Zaawansowane agregacji](advanced-aggregations.md)
- [JSON i struktur danych](json-data-structures.md)
- [Pisanie zapytań zaawansowanych](advanced-query-writing.md)
- [Wykresy](charts.md)