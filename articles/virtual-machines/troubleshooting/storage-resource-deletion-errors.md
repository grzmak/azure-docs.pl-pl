---
title: Rozwiązywanie problemów z błędami usuwania zasobów magazynu, na maszynach wirtualnych z systemem Linux na platformie Azure | Dokumentacja firmy Microsoft
description: Jak rozwiązywać problemy podczas usuwania zasobów magazynu zawierające dołączonymi dyskami VHD.
keywords: ''
services: virtual-machines
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 2ec5caab32e12411f5ccab4a9a6b98d3c4e57c0b
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414061"
---
# <a name="troubleshoot-storage-resource-deletion-errors"></a>Rozwiązywanie problemów z błędami usuwania zasobów magazynu

W niektórych przypadkach może wystąpić jeden z następujących błędów podczas próby usunięcia konta magazynu platformy Azure, kontenera lub obiektu blob we wdrożeniu usługi Azure Resource Manager:

>**Nie można usunąć konta magazynu "StorageAccountName". Błąd: Nie można usunąć konta magazynu z powodu jego artefakty są nadal używane.**

>**Nie można usunąć # poza # kontenerów:<br>wirtualne dyski twarde: obecnie występuje dzierżawy w kontenerze i identyfikator dzierżawy nie został określony w żądaniu.**

>**Nie można usunąć # poza # obiektów blob:<br>BlobName.vhd: w obiekcie blob aktualnie jest dzierżawy, a nie Identyfikatora dzierżawy została określona w żądaniu.**

Wirtualne dyski twarde używane w maszynach wirtualnych platformy Azure to pliki VHD przechowywane jako stronicowe obiekty BLOB na koncie magazynu w warstwie standardowa lub premium na platformie Azure. Aby uzyskać więcej informacji o dyskach platformy Azure, zobacz [o niezarządzanych i zarządzanych Magazyn dyskowy dla maszyn wirtualnych systemu Linux platformy Microsoft](../linux/about-disks-and-vhds.md). 

Azure uniemożliwia usunięcie dysku, który jest dołączony do maszyny Wirtualnej w celu uniknięcia uszkodzenia. Uniemożliwia ona usunięcie kontenerów i kont magazynu, które mają stronicowych obiektów blob, który jest dołączony do maszyny Wirtualnej. 

To proces, można usunąć konta magazynu, kontenera lub obiektu blob podczas odbierania jeden z następujących błędów: 
1. [Identyfikowanie obiektów blob dołączonych do maszyny Wirtualnej](#step-1-identify-blobs-attached-to-a-vm)
2. [Usuń maszyny wirtualne, za pomocą dołączonych **dysku systemu operacyjnego**](#step-2-delete-vm-to-detach-os-disk)
3. [Odłącz wszystko **dyski danych** z pozostałych maszyn wirtualnych](#step-3-detach-data-disk-from-the-vm)

Spróbuj ponownie, usuwanie konta magazynu, kontenera lub obiektu blob, po wykonaniu tych kroków.

## <a name="step-1-identify-blob-attached-to-a-vm"></a>: Krok 1 obiektów blob dołączonych do maszyny Wirtualnej

### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Scenariusz 1: Usuwanie obiektu blob — zidentyfikować dołączonych maszyny Wirtualnej
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **wszystkie zasoby**. Przejdź do konta magazynu, w obszarze **usługi obiektów Blob** wybierz **kontenerów**, a następnie przejdź do obiektu blob do usunięcia.
3. Jeśli obiekt blob **stanu dzierżawy** jest **wydzierżawiony**, a następnie kliknij prawym przyciskiem myszy i wybierz **edytować metadane** aby otworzyć okienko metadanych obiektu Blob. 

    ![Zrzut ekranu przedstawiający portal, z magazynu obiektów blob konta i kliknij prawym przyciskiem myszy > "Edytuj metadane" wyróżniony](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. W okienku Metadane obiektu Blob, sprawdź i Zapisz wartość **MicrosoftAzureCompute_VMName**. Ta wartość jest nazwa maszyny Wirtualnej, wirtualny dysk twardy jest podłączony do. (Zobacz **ważne** Jeśli to pole nie istnieje)
5. W okienku Metadane obiektu Blob, sprawdź i Zapisz wartość **MicrosoftAzureCompute_DiskType**. Identyfikuje tę wartość, jeśli dołączonym dysku systemu operacyjnego lub dysku z danymi (zobacz **ważne** Jeśli to pole nie istnieje). 

     ![Zrzut ekranu przedstawiający portal, otwórz okienko "Metadane obiektu Blob" magazynu](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Jeśli typ obiektu blob dysku **OSDisk** postępuj zgodnie z [krok 2: usuwanie maszyny Wirtualnej, aby odłączyć dysk systemu operacyjnego](#step-2-delete-vm-to-detach-os-disk). W przeciwnym razie, jeśli typ obiektu blob dysku **DataDisk** postępuj zgodnie z instrukcjami w [krok 3: dołączanie dysku danych z maszyny Wirtualnej](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Jeśli **MicrosoftAzureCompute_VMName** i **MicrosoftAzureCompute_DiskType** nie są wyświetlane w metadanych obiektu blob, oznacza to, że obiekt blob jest jawnie dzierżawiony i nie jest dołączony do maszyny Wirtualnej. Nie można usunąć obiektów blob dzierżawy bez przerywania pierwszy dzierżawy. Przerwij dzierżawę, kliknij prawym przyciskiem myszy w obiekcie blob i wybierz **Break lease**. Dzierżawy obiektów blob, które nie są dołączone do maszyny Wirtualnej zapobiec usunięciu obiektu blob, ale nie uniemożliwia usunięcie kontenera lub konta magazynu.

### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Scenariusz 2: Usuwanie kontenera — zidentyfikować wszystkie obiekty BLOB w kontenerze, które są dołączone do maszyn wirtualnych
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **wszystkie zasoby**. Przejdź do konta magazynu, w obszarze **usługę Blob Service** wybierz **kontenery**i Znajdź kontenera do usunięcia.
3. Kliknij, aby otworzyć kontenera i zostanie wyświetlona lista obiektów blob wewnątrz niego. Zidentyfikuj wszystkie obiekty BLOB z typem obiektu Blob = **stronicowych obiektów blob** i stanu dzierżawy = **wydzierżawiony** z tej listy. Postępuj zgodnie z [scenariusz 1](#step-1-identify-blobs-attached-to-a-vm) do identyfikowania maszyny Wirtualnej związany z każdą z tych obiektów blob.

    ![Zrzut ekranu przedstawiający portal, za pomocą obiektów blob konta magazynu i "Dzierżawy stanu" z "Wydzierżawiony" wyróżniony](./media/troubleshoot-vhds/utd-disks-sm.png)

4. Postępuj zgodnie z [kroku 2](#step-2-delete-vm-to-detach-os-disk) i [kroku 3](#step-3-detach-data-disk-from-the-vm) można usunąć maszyny wirtualne z **OSDisk** i odłączanie **DataDisk**. 

### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Scenariusz 3: Usuwanie magazynu konta — zidentyfikować wszystkie obiekty BLOB w ramach konta magazynu, które są dołączone do maszyn wirtualnych
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **wszystkie zasoby**. Przejdź do konta magazynu, w obszarze **usługę Blob Service** wybierz **kontenery**.

    ![Zrzut ekranu przedstawiający portal, za pomocą kontenerów kont magazynów i "Dzierżawy stanu" z "Wydzierżawiony" wyróżniony](./media/troubleshoot-vhds/utd-containers-sm.png)

3. W **kontenery** okienku zidentyfikować wszystkie kontenery gdzie **stanu dzierżawy** jest **wydzierżawiony** i postępuj zgodnie z [scenariuszu 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) dla każdego  **Dzierżawiony** kontenera.
4. Postępuj zgodnie z [kroku 2](#step-2-delete-vm-to-detach-os-disk) i [kroku 3](#step-3-detach-data-disk-from-the-vm) można usunąć maszyny wirtualne z **OSDisk** i odłączanie **DataDisk**. 

## <a name="step-2-delete-vm-to-detach-os-disk"></a>Krok 2: Usuwanie maszyny Wirtualnej, aby odłączyć dysk systemu operacyjnego
Jeśli wirtualny dysk twardy jest dyskiem systemu operacyjnego, należy usunąć maszynę Wirtualną, przed usunięciem dołączonego dysku VHD. Wykonywać żadnych dodatkowych akcji jest wymagana w przypadku dysków danych podłączonych do tej samej maszyny Wirtualnej, po wykonaniu tych kroków:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **maszyn wirtualnych**.
3. Wybierz wirtualny dysk twardy dołączony do maszyny Wirtualnej.
4. Upewnij się, że nic nie jest aktywnie przy użyciu maszyny wirtualnej i nie są już potrzebne maszyny wirtualnej.
5. W górnej części **szczegóły maszyny wirtualnej** okienku wybierz **Usuń**, a następnie kliknij przycisk **tak** o potwierdzenie.
6. Należy usunąć maszynę Wirtualną, ale mogą być zachowywane przez wirtualny dysk twardy. Jednak wirtualny dysk twardy powinien być już dołączony do maszyny Wirtualnej lub ma do niej dzierżawy. Może upłynąć kilka minut, zanim dzierżawy mogą być wprowadzane. Aby sprawdzić, czy dzierżawa zostanie zwolniona, przejdź do lokalizacji obiektu blob i w **właściwości obiektu Blob** okienku **stanu dzierżawy** powinien być **dostępne**.

## <a name="step-3-detach-data-disk-from-the-vm"></a>Krok 3: Dołączanie dysku danych z maszyny Wirtualnej
Jeśli wirtualny dysk twardy jest dyskiem danych, odłącz wirtualny dysk twardy z maszyny Wirtualnej, aby usunąć tę dzierżawę:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **maszyn wirtualnych**.
3. Wybierz wirtualny dysk twardy dołączony do maszyny Wirtualnej.
4. Wybierz **dysków** na **szczegóły maszyny wirtualnej** okienka.
5. Wybierz dysk danych można usunąć wirtualny dysk twardy jest podłączony do. Można określić, które obiekt blob jest dołączony na dysku, sprawdzając adres URL wirtualnego dysku twardego.
6. Możesz sprawdzić lokalizacji obiektu blob, klikając na dysku, aby zaewidencjonować ścieżkę **identyfikator URI dysku VHD** pola.
7. Wybierz **Edytuj** w górnej części **dysków** okienka.
8. Kliknij przycisk **odłączyć ikonę** dysku danych do usunięcia.

     ![Zrzut ekranu przedstawiający portal, otwórz okienko "Metadane obiektu Blob" magazynu](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. Wybierz pozycję **Zapisz**. Dysk jest obecnie odłączona od maszyny Wirtualnej, i wirtualny dysk twardy nie jest już uzyska dzierżawę. Może upłynąć kilka minut, zanim dzierżawy mogą być wprowadzane. Aby sprawdzić, czy po udostępnieniu dzierżawy, przejdź do lokalizacji obiektu blob i w **właściwości obiektu Blob** okienku **stanu dzierżawy** . wartość powinna być **odblokowany** lub **Dostępne**.

[Storage deletion errors in Resource Manager deployment]: #storage-delete-errors-in-rm

