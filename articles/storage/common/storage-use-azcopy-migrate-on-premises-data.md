---
title: Migracja danych lokalnych do usługi Azure Storage za pomocą narzędzia AzCopy | Microsoft Docs
description: Użyj narzędzia AzCopy do migracji lub kopiowania danych z lub do obiektu blob, tabeli i zawartości pliku. W łatwy sposób przeprowadź migrację danych z magazynu lokalnego do usługi Azure Storage.
services: storage
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: b238e0f8059e7b4e5223c72ebed04f7d5178fbf2
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830960"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Samouczek: Migracja danych lokalnych do magazynu w chmurze za pomocą narzędzia AzCopy

Narzędzie AzCopy to narzędzie wiersza polecenia służące do kopiowania danych do lub z magazynu Azure Blob Storage, Azure Files lub Azure Table przy użyciu prostych poleceń. Polecenia te zostały zaprojektowane w celu uzyskania optymalnej wydajności. Dane można kopiować między systemem plików i kontem magazynu lub między kontami magazynu.  

Możesz pobrać dwie wersje narzędzia AzCopy:

* [Narzędzie AzCopy w systemie Linux](storage-use-azcopy-linux.md) jest oparte na platformie .NET Core Framework. Jest ono przeznaczone dla platform Linux, oferując opcje wiersza polecenia w stylu POSIX. 
* [Narzędzie AzCopy w systemie Windows](storage-use-azcopy.md) jest oparte na platformie .NET Framework. Oferuje ono opcje wiersza polecenia w stylu systemu Windows. 
 
Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta magazynu 
> * Używanie narzędzia AzCopy do przekazania wszystkich danych
> * Modyfikowanie danych do celów testowych
> * Tworzenie zaplanowanego zadania lub usługi cron do identyfikowania nowych plików do przekazania

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten samouczek, pobierz najnowszą wersję narzędzia AzCopy w systemie [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) lub [Windows](http://aka.ms/downloadazcopy). 

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

>[!NOTE]
>Aby móc pobierać obiekty blob z regionu pomocniczego do magazynu lokalnego i na odwrót, ustaw pozycję **Replikacja** na wartość **Magazyn geograficznie nadmiarowy dostępny do odczytu**. Wybranie tej opcji spowoduje utworzenie konta [magazynu geograficznie nadmiarowego](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage). 
>
>

## <a name="create-a-container"></a>Tworzenie kontenera

Obiekty blob są zawsze przesyłane do kontenera. Możesz używać kontenerów do organizowania grup obiektów blob w sposób podobny do organizowania plików w folderach na komputerze. 

Wykonaj następujące kroki, aby utworzyć kontener:

1. Wybierz przycisk **Konta magazynu** ze strony głównej, a następnie wybierz utworzone konto magazynu.
2. Wybierz pozycję **Obiekty blob** w obszarze **Usługi**, a następnie wybierz pozycję **Kontener**. 

   ![Tworzenie kontenera](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Nazwy kontenerów muszą zaczynać się literą lub cyfrą. Mogą one zawierać tylko litery, cyfry i znaki łącznika (-). Aby uzyskać dodatkowe informacje o regułach nazewnictwa kontenerów i obiektów blob, zobacz temat [Naming and Referencing Containers, Blobs, and Metadata (Nazewnictwo i odwoływanie się do kontenerów, obiektów blob i metadanych)](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="upload-all-files-in-a-folder-to-blob-storage"></a>Przekazywanie wszystkich plików w folderze do magazynu obiektów blob

Za pomocą narzędzia AzCopy możesz przekazać wszystkie pliki w folderze do magazynu obiektów blob w systemie [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) lub [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Aby przekazać wszystkie obiekty blob w folderze, wprowadź następujące polecenie narzędzia AzCopy:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S
---

Zastąp wartości `<key>` i `key` kluczem konta. W witrynie Azure Portal możesz pobrać klucz konta, wybierając pozycję **Klucze dostępu** w obszarze **Ustawienia** w ramach konta magazynu. Wybierz klucz, a następnie wklej go do polecenia narzędzia AzCopy. Jeśli określony kontener docelowy nie istnieje, narzędzie AzCopy utworzy go i przekaże do niego plik. Zaktualizuj ścieżkę źródłową na Twój katalog danych, a następnie zastąp wartość **myaccount** w docelowym adresie URL nazwą konta magazynu.

Aby przekazać zawartość określonego katalogu do magazynu obiektów blob w sposób rekursywny, określ opcję `--recursive` (system Linux) lub `/S` (system Windows). Po uruchomieniu narzędzia AzCopy z jedną z tych opcji wszystkie podfoldery i znajdujące się w nich pliki również zostaną przekazane.

## <a name="upload-modified-files-to-blob-storage"></a>Przekazywanie zmodyfikowanych plików do magazynu obiektów blob
Narzędzia AzCopy można użyć do [przekazania plików](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) na podstawie daty ich ostatniej modyfikacji. Aby z tego skorzystać, zmodyfikuj pliki lub utwórz nowe pliki w katalogu źródłowym do celów testowych. Aby przekazać tylko zaktualizowane lub nowe pliki, do polecenia narzędzia AzCopy dodaj parametr `--exclude-older` (system Linux) lub `/XO` (system Windows).

Jeśli chcesz skopiować tylko zasoby źródłowe, które nie istnieją w miejscu docelowym, określ zarówno parametry `--exclude-older` i `--exclude-newer` (system Linux) lub `/XO` i `/XN` (system Windows) w poleceniu narzędzia AzCopy. Narzędzie AzCopy przekazuje tylko zaktualizowane dane na podstawie ich sygnatury czasowej.
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S /XO
---

## <a name="create-a-scheduled-task-or-cron-job"></a>Tworzenie zaplanowanego zadania lub zadania cron 
Możliwe jest utworzenie zaplanowanego zadania lub zadania cron służącego do uruchamiania skryptu polecenia narzędzia AzCopy. Skrypt identyfikuje i przekazuje nowe dane lokalne do magazynu w chmurze zgodnie z określonym interwałem. 

Skopiuj polecenia narzędzia AzCopy do edytora tekstu. Zaktualizuj wartości parametrów polecenia narzędzia AzCopy na odpowiednie wartości. Zapisz plik jako `script.sh` (system Linux) lub `script.bat` (system Windows) na potrzeby narzędzia AzCopy. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

Narzędzie AzCopy jest uruchamiane z opcją `--verbose` (system Linux) lub `/V` (system Windows). Dane wyjściowe są przekierowywane do pliku dziennika. 

W tym samouczku polecenie [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) jest używane do utworzenia zaplanowanego zadania w systemie Windows. Polecenie [Crontab](http://crontab.org/) służy do tworzenia zadania cron w systemie Linux. 
 Polecenie **Schtasks** umożliwia administratorowi tworzenie, usuwanie, zmienianie, uruchamianie i kończenie zaplanowanych zadań oraz wykonywanie względem nich zapytań na komputerze lokalnym lub zdalnym. Zadanie **cron** umożliwia użytkownikom systemów Linux i Unix uruchamianie poleceń lub skryptów w określonym dniu i godzinie za pomocą [wyrażeń cron](https://en.wikipedia.org/wiki/Cron#CRON_expression).


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Aby utworzyć zadanie cron w systemie Linux, wprowadź następujące polecenie z poziomu terminalu: 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Określenie wyrażenia cron `*/5 * * * * ` w poleceniu wskazuje, że skrypt powłoki `script.sh` powinien być uruchamiany co pięć minut. Możesz zaplanować uruchamianie skryptu codziennie, co miesiąc lub co rok o określonej godzinie. Aby dowiedzieć się więcej na temat ustawiania daty i godziny wykonywania zadań, zobacz [wyrażenia cron](https://en.wikipedia.org/wiki/Cron#CRON_expression). 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Aby utworzyć zaplanowane zadanie w systemie Windows, wpisz następujące polecenie w wierszu polecenia lub w programie PowerShell:

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Polecenie używa:
- Parametru `/SC` w celu określenia harmonogramu minutowego.
- Parametru `/MO` w celu określenia interwału wynoszącego 5 minut.
- Parametru `/TN` w celu określenia nazwy zadania.
- Parametru `/TR` w celu określenia ścieżki do pliku `script.bat`. 

Aby dowiedzieć się więcej o tworzeniu zaplanowanego zadania w systemie Windows, zobacz [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---
 
Aby sprawdzić, czy zaplanowane zadanie lub zadanie cron jest działa poprawnie, utwórz nowe pliki w katalogu `myfolder`. Poczekaj pięć minut, aby sprawdzić, czy nowe pliki zostały przekazane do konta magazynu. Przejdź do katalogu dzienników, aby wyświetlić dzienniki danych wyjściowych zaplanowanego zadania lub zadania cron. 

Aby dowiedzieć się więcej o sposobach przenoszenia danych lokalnych do usługi Azure Storage i na odwrót, zobacz [Przenoszenie danych do i z usługi Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat usługi Azure Storage i narzędzia AzCopy, zapoznaj się z następującymi zasobami:

* [Wprowadzenie do usługi Azure Storage](../storage-introduction.md)
* [Transferowanie danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) 
* [Transferowanie danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md) 
* [Tworzenie konta magazynu](../storage-create-storage-account.md)
* [Zarządzanie obiektami blob za pomocą Eksploratora usługi Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

