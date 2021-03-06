---
title: Anuluj i Usuń zadanie usługi Azure Import/Export | Dokumentacja firmy Microsoft
description: Dowiedz się, jak anulowania i usuwania zadań dla usługi Microsoft Azure Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: e11cf974a5f33020c889844dd34f51612e13036e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521017"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Anulowanie i usuwanie zadań usługi Azure Import/Export

 Na żądanie, czy można anulować zadania, zanim zostanie w `Packaging` stanu, wywołaj [właściwości zadania aktualizacji](/rest/api/storageimportexport/jobs#Jobs_Update) operacji i ustaw `CancelRequested` elementu `true`. Zadanie zostało anulowane na zasadzie największej staranności. Jeśli dyski znajdują się w trakcie przesyłania danych, dane mogą być nadal przesyłane, mimo że zażądano anulowania.

 Anulowano zadanie zostanie przeniesiona do `Completed` stanu i są przechowywane przez 90 dni, w tym momencie zostanie usunięty.

 Aby usunąć zadanie, należy wywołać [Usuń zadanie](/rest/api/storageimportexport/jobs#Jobs_Delete) operacji przed zadanie zostało wysłane (oznacza to, że podczas wykonywania zadania w `Creating` stanu). Można również usunąć zadanie, gdy jest on w `Completed` stanu. Po usunięciu zadania jego informacje i jego stan nie są już dostępne za pośrednictwem interfejsu API REST lub witryny Azure portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Kolejne kroki

* [Przy użyciu interfejsu API REST usługi Import/Export](storage-import-export-using-the-rest-api.md)
