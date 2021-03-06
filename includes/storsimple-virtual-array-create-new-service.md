---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: storage
author: alkohli
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 4285ee94d2f2a9b5fff9d1896cdf7013f88801ab
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45979176"
---
#### <a name="to-create-a-new-service"></a>Aby utworzyć nową usługę

1.  Przy użyciu poświadczeń konta Microsoft, zaloguj się do witryny Azure portal pod tym adresem URL: <https://portal.azure.com/>. Jeśli wdrażane urządzenia w portalu dla instytucji rządowych, zaloguj się na: <https://portal.azure.us/>

2.  W witrynie Azure portal kliknij pozycję **+ Utwórz zasób** &gt; **magazynu** &gt; **seria urządzeń wirtualnych StorSimple**.

    ![Utwórz nową usługę](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  W **Menedżer urządzeń StorSimple** otwartym bloku, wykonaj następujące czynności:

    1.  Podaj **unikatową nazwę zasobu** dla swojej usługi. Nazwa zasobu jest przyjazna nazwa, która może służyć do identyfikowania usługi. Nazwa może zawierać od 2 do 50 znaków, które mogą być literami, cyframi i łącznikami. Nazwa musi zaczynać i kończyć się literą lub cyfrą.

    2.  Wybierz opcję **Subskrypcja** z listy rozwijanej. Subskrypcja jest połączona z kontem rozliczeniowym. To pole nie jest widoczne, jeśli istnieje tylko jedna subskrypcja.

    3.  Aby uzyskać **grupy zasobów**, wybierz istniejącą lub Utwórz nową grupę. Aby uzyskać więcej informacji, zobacz [Grupy zasobów na platformie Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Wypełnij pole **Lokalizacja** dla usługi. Zobacz [regionów platformy Azure](https://azure.microsoft.com/regions/#services) Aby uzyskać więcej informacji o tym, jakie usługi są dostępne w poszczególnych regionach. Ogólnie rzecz biorąc, wybierz **lokalizacji** najbliżej regionu geograficznego, w której chcesz wdrożyć urządzenie. Możesz także użyć następujących składników:

        -   Jeśli masz istniejące obciążenia na platformie Azure, które również zamierzasz wdrożyć na urządzeniu StorSimple, firma Microsoft zaleca użycie tego centrum danych.

        -   Magazyn Menedżera urządzeń StorSimple i platformą Azure może być w dwóch różnych lokalizacjach. W takim przypadku należy utworzyć konta usługi Menedżer urządzeń StorSimple i usługi Azure Storage oddzielnie. Aby utworzyć konto usługi Azure storage, przejdź do usługi Azure Storage w witrynie Azure portal i wykonaj kroki opisane w [Tworzenie konta magazynu](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). Po utworzeniu konta dodaj je do usługi Menedżer urządzeń StorSimple, wykonując czynności w temacie [Configure a new storage account for the service](https://azure.microsoft.com/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service) (Konfigurowanie nowego konta magazynu dla usługi).

        -   Jeśli wdrażane urządzenia wirtualnego w portalu dla instytucji rządowych, usługa Menedżer urządzeń StorSimple jest dostępna w lokalizacji USA — Iowa i USA — Wirginia.

    5.  Wybierz **Utwórz nowe konto usługi Azure storage** automatyczne tworzenie konta magazynu z usługą. Określ **nazwa konta magazynu**. Jeśli potrzebujesz danych w innej lokalizacji, usuń zaznaczenie tego pola.

    6.  Zaznacz pole **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz szybko łączyć się z tą usługą na pulpicie nawigacyjnym.

    7.  Kliknij pozycję **Utwórz**, aby utworzyć usługę Menedżer urządzeń StorSimple.

        ![Utwórz nową usługę](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Nastąpi przekierowanie do **usługi** strony docelowej. Tworzenie usługi potrwa kilka minut. Po pomyślnym utworzeniu usługi użytkownik zostanie odpowiednio powiadomiony i stan usługi zmieni się na **Aktywny**.


