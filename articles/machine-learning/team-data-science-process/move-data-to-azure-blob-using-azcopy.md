---
title: Przenoszenie danych do i z usługi Azure Blob Storage przy użyciu narzędzia AzCopy | Dokumentacja firmy Microsoft
description: Przenoszenie danych do oraz z usługi Azure Blob Storage za pomocą usługi AzCopy
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: c309ceb2-0e83-4a07-b16d-c997dcd62d5c
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 41e89aab65b19e22ad6f8fe0d3087c4e7f5430ab
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393408"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Przenoszenie danych do i z usługi Azure Blob Storage przy użyciu narzędzia AzCopy
AzCopy to narzędzie wiersza polecenia, zaprojektowana pod kątem przekazywanie, pobieranie i kopiowanie danych do i z obiektów blob, plików i usługi table storage Microsoft Azure.

Aby uzyskać instrukcje na temat instalowania narzędzia AzCopy oraz dodatkowe informacje na temat używania go z platformą Azure, zobacz [rozpoczęcie korzystania z wiersza polecenia Azcopy](../../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Jeśli używasz maszyny Wirtualnej, który został skonfigurowany za pomocą skryptów, dostarczone przez [maszyny wirtualne do analizy danych na platformie Azure](virtual-machines.md), a następnie narzędzie AzCopy jest już zainstalowane na maszynie Wirtualnej.
> 
> [!NOTE]
> Pełne wprowadzenie do usługi Azure blob storage, zapoznaj się [podstawowe informacje o usłudze Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) i [usługi Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
W tym dokumencie przyjęto założenie, że masz subskrypcję platformy Azure, konta magazynu i odpowiedniego klucza magazynu dla tego konta. Przed Trwa przekazywanie/pobieranie danych, musisz wiedzieć, usługa Azure storage konta nazwy i klucza konta usługi.

* Aby skonfigurować subskrypcję platformy Azure, zobacz [bezpłatnej miesięcznej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
* Aby uzyskać instrukcje dotyczące tworzenia konta magazynu oraz w celu uzyskania konta i informacje o kluczu, zobacz [kontach magazynu Azure o](../../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>Uruchom polecenia narzędzia AzCopy
Aby uruchomić polecenia narzędzia AzCopy, Otwórz okno polecenia i przejdź do katalogu instalacyjnego programu AzCopy na komputerze, w którym znajduje się AzCopy.exe pliku wykonywalnego. 

Podstawowa składnia polecenia narzędzia AzCopy jest:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> Można dodać lokalizacji instalacji narzędzia AzCopy do ścieżki systemowej i następnie uruchom polecenia z dowolnego katalogu. Domyślnie narzędzie AzCopy jest zainstalowane na *% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* lub *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-to-an-azure-blob"></a>Przekazywanie plików do obiektu blob platformy Azure
Aby przekazać plik, użyj następującego polecenia:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Pobierz pliki z obiektu blob platformy Azure
Aby pobrać plik z obiektu blob platformy Azure, użyj następującego polecenia:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Przenoszenie obiektów blob między kontenerów platformy Azure
Aby przenieść obiekty BLOB między kontenerów platformy Azure, użyj następującego polecenia:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Porady dotyczące korzystania z narzędzia AzCopy
> [!TIP]
> 1. Gdy **przekazywania** plików */S* przekazuje plikach cyklicznie. Bez tego parametru nie są ładowane pliki w podkatalogach.  
> 2. Gdy **pobieranie** pliku */S* wyszukuje rekursywnie kontenera, dopóki wszystkie pliki w określonym katalogu i jego podkatalogach lub wszystkie pliki, które spełniają określony wzorzec w podanym katalogu i jego podkatalogach są pobierane.  
> 3. Nie można określić **konkretny obiekt blob, plik** można pobrać przy użyciu */Source* parametru. Aby pobrać określony plik, określ nazwę pliku obiektu blob do pobrania przy użyciu */wzorca* parametru. **/S** parametru można mieć narzędzia AzCopy, poszukaj rekursywnie wzorzec nazwy pliku. Bez parametru wzorzec AzCopy pobiera wszystkie pliki w tym katalogu.
> 
> 

