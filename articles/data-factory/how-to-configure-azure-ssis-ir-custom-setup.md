---
title: Dostosowywanie ustawień środowiska Azure-SSIS integration Runtime | Dokumentacja firmy Microsoft
description: W tym artykule opisano sposób instalowania dodatkowych składników, lub zmienić ustawienia za pomocą interfejsu niestandardowego Instalatora dla środowiska Azure-SSIS integration runtime
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/21/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: ad3ec09f039b38290929289c7bca77664b0fb554
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441789"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Dostosowywanie ustawień środowiska Azure-SSIS integration Runtime

Interfejs niestandardowego Instalatora dla środowiska Azure-SSIS Integration Runtime zapewnia interfejs do dodawania własnych kroki konfiguracji podczas inicjowania obsługi lub ponownej konfiguracji z usługi Azure-SSIS IR. Instalacja niestandardowa pozwala zmienić domyślne operacyjnych, konfiguracji lub środowisku (na przykład, aby uruchomić dodatkowe usługi Windows lub utrwalić poświadczenia dostępu do udziałów plików) lub zainstalowania dodatkowych składników (na przykład, zestawy sterowników i rozszerzenia) w każdym węźle usługi Azure-SSIS IR.

Przygotowywanie skryptu i skojarzone z nią pliki i przekazujemy je do kontenera obiektów blob na koncie usługi Azure Storage, należy skonfigurować ustawienia niestandardowe. Podaj sygnatury dostępu współdzielonego (SAS) jednolite zasobów identyfikator (URI) dla kontenera usługi aprowizacji lub zmienić konfigurację usługi Azure-SSIS IR. Każdy węzeł środowiska IR Azure-SSIS następnie pobrać skrypt i skojarzone z nią pliki z kontenera i Twoje niestandardowy program instalacyjny zostanie uruchomiony z podwyższonym poziomem uprawnień. Po zakończeniu instalacji niestandardowej każdy węzeł wysyła standardowe dane wyjściowe wykonywania i innych dzienników do kontenera.

Można zainstalować składniki bezpłatnej i bez licencji i płatna lub licencjonowanych składników. Jeśli jesteś niezależnym dostawcą oprogramowania, zobacz [sposobu tworzenia wersji płatnej lub licencjonowane składniki dla środowiska Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md).


## <a name="current-limitations"></a>Bieżące ograniczenia

-   Jeśli chcesz używać `gacutil.exe` do instalowania zestawów w globalnej pamięci podręcznej zestawów (GAC), musisz podać `gacutil.exe` jako część instalacji niestandardowej lub użyj kopii podane w kontenerze publicznej wersji zapoznawczej.

-   Jeśli chcesz odwoływać się do podfolderu w skrypcie, `msiexec.exe` nie obsługuje `.\` notacji, aby odwoływać się do folderu głównego. Użyj polecenia podobnego `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` zamiast `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

-   Jeśli zachodzi potrzeba dołączania środowiska IR Azure-SSIS przy użyciu instalacji niestandardowej do sieci wirtualnej, jest obsługiwane tylko sieć wirtualną usługi Azure Resource Manager. Klasyczna sieć wirtualna nie jest obsługiwane.

-   Udział administracyjny nie jest obecnie obsługiwana na platformie Azure-SSIS IR.

## <a name="prerequisites"></a>Wymagania wstępne

Dostosowywanie środowiska IR Azure-SSIS, potrzebne są następujące elementy:

-   [Subskrypcja platformy Azure](https://azure.microsoft.com/)

-   [Serwer usługi Azure SQL Database lub wystąpienia zarządzanego](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Aprowizowanie środowiska Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Konto usługi Azure Storage](https://azure.microsoft.com/services/storage/). Dla niestandardowego instalatora przekazywanie i przechowywanie skrypt instalacji niestandardowej i skojarzone z nią pliki w kontenerze obiektów blob. Proces instalacji niestandardowej również przekazuje jej dzienniki wykonywania do tego samego kontenera obiektów blob.

## <a name="instructions"></a>Instrukcje

1.  Pobierz i zainstaluj [programu Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) (w wersji 5.4 lub nowszej).

1.  Przygotuj skrypt instalacji niestandardowej i jego skojarzone pliki (na przykład, .bat, .cmd, .exe, .dll, msi lub ps1).

    1.  Konieczne jest posiadanie plik skryptu o nazwie `main.cmd`, który jest punktem wejścia ustawień niestandardowych.

    1.  Jeśli chcesz, aby dodatkowe dzienniki generowane przez innych narzędzi (na przykład `msiexec.exe`) do przekazania do kontenera, określ zmienną środowiskową wstępnie zdefiniowanych `CUSTOM_SETUP_SCRIPT_LOG_DIR` jako folder dziennika w skryptach (na przykład `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

1.  Pobieranie, instalowanie i uruchamianie [Eksploratora usługi Azure Storage](http://storageexplorer.com/).

    1.  W obszarze **(lokalne i dołączone)** po prawej stronie wybierz **kont magazynu** i wybierz **łączenie z usługą Azure storage**.

       ![Łączenie z usługą Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

    1.  Wybierz **Użyj klucza i nazwy konta magazynu** i wybierz **dalej**.

       ![Używanie nazwy i klucza konta magazynu](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

    1.  Wprowadź nazwę konta usługi Azure Storage i klucz, wybierz opcję **dalej**, a następnie wybierz pozycję **Connect**.

       ![Podaj nazwę konta magazynu i klucza](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

    1.  W ramach konta usługi Azure Storage, usługi połączone, kliknij prawym przyciskiem myszy **kontenery obiektów Blob**, wybierz opcję **Utwórz kontener obiektów Blob**i nadaj nazwę nowego kontenera.

       ![Tworzenie kontenera obiektów blob](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

    1.  Wybierz nowy kontener, a następnie Przekaż skrypt instalacji niestandardowej oraz skojarzone z nią pliki. Upewnij się, że przekazujesz `main.cmd` na najwyższym poziomie kontenera, a nie w dowolnym folderze. 

       ![Przekazywanie plików do kontenera obiektów blob](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

    1.  Kliknij prawym przyciskiem myszy kontener, a następnie wybierz pozycję **Uzyskaj sygnaturę dostępu współdzielonego**.

       ![Uzyskaj sygnaturę dostępu współdzielonego dla kontenera](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

    1.  Z czasem wygaśnięcia wystarczająco długi i Odczyt, tworzenie identyfikatora URI sygnatury dostępu Współdzielonego kontenera, zapisu i listy uprawnień. Należy pobrać i uruchomić skrypt instalacji niestandardowej i skojarzone z nią pliki, zawsze wtedy, gdy dowolny węzeł środowiska IR Azure-SSIS odtwarzany z obrazu ponownym identyfikator URI sygnatury dostępu Współdzielonego. Musisz mieć uprawnienia zapisu można przekazać dzienniki wykonywania instalacji.

        > [!IMPORTANT]
        > Upewnij się, że identyfikator URI sygnatury dostępu Współdzielonego nie wygasa i Instalacja niestandardowa zasoby są zawsze dostępne podczas całego cyklu życia środowiska IR Azure-SSIS, od tworzenia do usunięcia, zwłaszcza, jeśli regularnie zatrzymujesz i uruchamiasz środowiska IR Azure-SSIS w tym okresie.

       ![Generuj sygnaturę dostępu współdzielonego dla kontenera](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

    1.  Skopiuj i Zapisz identyfikator URI sygnatury dostępu Współdzielonego kontenera.

       ![Skopiuj i Zapisz sygnatura dostępu współdzielonego](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

    1.  W przypadku udostępniania lub zmienić konfigurację środowiska IR Azure-SSIS przy użyciu programu PowerShell, przed rozpoczęciem korzystania z usługi Azure-SSIS IR, uruchomić `Set-AzureRmDataFactoryV2IntegrationRuntime` polecenia cmdlet z identyfikatora URI sygnatury dostępu Współdzielonego kontenera jako wartość dla nowych `SetupScriptContainerSasUri` parametru. Na przykład:

       ```powershell
       Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName `
                                                  -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

       Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                    -Name $MyAzureSsisIrName `
                                                    -ResourceGroupName $MyResourceGroupName
       ```

    1.  Po zakończeniu ustawień niestandardowych i uruchamia środowiska IR Azure-SSIS, można znaleźć standardowe dane wyjściowe `main.cmd` i loguje się do niego inne wykonywania `main.cmd.log` folderu kontenera magazynu.

1.  Aby zobaczyć inne przykłady Instalacja niestandardowa, należy połączyć się z kontenerem publicznej wersji zapoznawczej, za pomocą Eksploratora usługi Azure Storage.

    a.  W obszarze **(lokalne i dołączone)**, kliknij prawym przyciskiem myszy **kont magazynu**, wybierz opcję **łączenie z usługą Azure storage**, wybierz opcję **za pomocą parametrów połączenia lub dostępu współdzielonego identyfikatora URI sygnatury**, a następnie wybierz pozycję **dalej**.

       ![Łączenie się z magazynem platformy Azure przy użyciu sygnatura dostępu współdzielonego](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

    b.  Wybierz **korzystania z identyfikatora URI sygnatury dostępu Współdzielonego** i wprowadź następujący identyfikator URI sygnatury dostępu Współdzielonego dla kontenera publicznej wersji zapoznawczej. Wybierz **dalej**, a następnie wybierz **Connect**.

       `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

       ![Podaj sygnaturę dostępu współdzielonego dla kontenera](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

    c. Wybierz kontener połączonych publicznej wersji zapoznawczej, a następnie kliknij dwukrotnie ikonę `CustomSetupScript` folderu. W tym folderze są następujące elementy:

       1. A `Sample` folder zawierający niestandardowe Instalatora, aby zainstalować podstawowe zadania w każdym węźle usługi Azure-SSIS IR. Zadanie nie działa jednak uśpienia przez kilka sekund. Folder zawiera także `gacutil` folder, który zawiera `gacutil.exe`. Ponadto `main.cmd` zawiera komentarze, aby utrwalić poświadczenia dostępu do udziałów plików.

       1. A `UserScenarios` folder, który zawiera kilka niestandardowych ustawień dla rzeczywiste scenariusze użytkowników.

    ![Zawartość kontenerów publicznej wersji zapoznawczej](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

    d. Kliknij dwukrotnie `UserScenarios` folderu. W tym folderze są następujące elementy:

       1. A `.NET FRAMEWORK 3.5` folder zawierający niestandardowe Instalatora, aby zainstalować starszą wersję programu .NET Framework, które mogą być wymagane dla niestandardowych składników w każdym węźle usługi Azure-SSIS IR.

       1. A `BCP` folder zawierający niestandardowe Instalatora, aby zainstalować narzędzia wiersza polecenia programu SQL Server (`MsSqlCmdLnUtils.msi`), w tym programu do kopiowania zbiorczego (`bcp`), w każdym węźle usługi Azure-SSIS IR.

       1. `EXCEL` Folder, który zawiera niestandardowe ustawienia do instalowania zestawów typu open source (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll`, i `ExcelDataReader.dll`) w każdym węźle usługi Azure-SSIS IR.

       1. `MSDTC` Folder, który zawiera niestandardowe ustawienia do modyfikowania konfiguracji zabezpieczeń i sieci dla usługi transakcji Koordynator MSDTC (Microsoft Distributed) w każdym węźle usługi Azure-SSIS IR. Aby upewnić się, że usługa MSDTC została uruchomiona, Dodaj zadanie wykonania procesu na początku przepływ sterowania w pakietach można wykonać następujące polecenie: `%SystemRoot%\system32\cmd.exe /c powershell -Command "Start-Service MSDTC"` 

       1. `ORACLE ENTERPRISE` Folder, który zawiera skrypt instalacji niestandardowej (`main.cmd`) i plik konfiguracji instalacji dyskretnej (`client.rsp`) Aby zainstalować sterownik Oracle OCI w każdym węźle Twojego środowiska Azure-SSIS IR Enterprise Edition. Ta konfiguracja pozwala Użyj Menedżera połączeń bazy danych Oracle, źródłowym i docelowym. Najpierw pobierz najnowszego klienta Oracle — na przykład `winx64_12102_client.zip` — od [Oracle](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) , a następnie przekaż go razem z `main.cmd` i `client.rsp` do kontenera. Jeśli używasz TNS nawiązać połączenia z programem Oracle, należy również pobrać `tnsnames.ora`, edytować go i przekaż go do kontenera, dzięki czemu mogą być kopiowane do folderu instalacyjnego programu Oracle podczas instalacji.

       1. `ORACLE STANDARD` Folder, który zawiera skrypt instalacji niestandardowej (`main.cmd`) do zainstalowania sterownika Oracle ODP.NET w każdym węźle usługi Azure-SSIS IR. Ta konfiguracja pozwala Użyj Menedżera połączeń ADO.NET, źródłowym i docelowym. Najpierw pobierz najnowszy sterownik Oracle ODP.NET — na przykład `ODP.NET_Managed_ODAC122cR1.zip` — od [Oracle](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html), a następnie przekaż go razem z `main.cmd` do kontenera.

       1. `SAP BW` Folder, który zawiera skrypt instalacji niestandardowej (`main.cmd`) Aby zainstalować zestaw łącznika SAP .NET (`librfc32.dll`) w każdym węźle Twojego środowiska Azure-SSIS IR Enterprise Edition. Ta konfiguracja pozwala Użyj Menedżera połączeń systemu SAP BW, źródłowym i docelowym. Najpierw należy przekazać 64-bitowe czy 32-bitową wersję `librfc32.dll` z folderu instalacyjnego programu SAP do kontenera, wraz z `main.cmd`. Skrypt następnie kopiuje zestawu SAP do `%windir%\SysWow64` lub `%windir%\System32` folderu podczas instalacji.

       1. A `STORAGE` folder, który zawiera niestandardowe ustawienia instalacji programu Azure PowerShell w każdym węźle usługi Azure-SSIS IR. Ta konfiguracja umożliwia wdrażanie, a następnie uruchom SSIS umieszcza uruchamianą [skryptów programu PowerShell do manipulowania konta usługi Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Kopiuj `main.cmd`, próbkę `AzurePowerShell.msi` (lub zainstaluj najnowszą wersję) i `storage.ps1` do kontenera. Na użytek PowerShell.dtsx jako szablon pakietów. Szablon pakietu łączy [zadania pobierania obiektów Blob Azure](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), jakie pliki do pobrania `storage.ps1` jako skrypt programu PowerShell można modyfikować i [wykonać zadanie procesu](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) , który jest wykonywany skrypt w każdym węźle.

       1. A `TERADATA` folder, który zawiera skrypt instalacji niestandardowej (`main.cmd)`, jego skojarzonego pliku (`install.cmd`) i pakiety instalacyjne (`.msi`). Te pliki zainstalować Teradata łączników interfejsu API TPT i sterownik ODBC w każdym węźle Twojego środowiska Azure-SSIS IR Enterprise Edition. Ta konfiguracja umożliwia używanie, Teradata Menedżera połączeń, źródłowym i docelowym. Najpierw Pobierz plik zip 15.x Teradata narzędzia i programy narzędziowe (TTU) (na przykład `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) z [Teradata](http://partnerintelligence.teradata.com), a następnie przekaż go wraz z powyższymi `.cmd` i `.msi` plików do kontenera.

    ![Folderów w folderze scenariusze użytkownika](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

    e. Aby wypróbować te przykłady Instalacja niestandardowa, skopiuj i Wklej zawartość z wybrany folder do kontenera. Po aprowizacji lub zmienić konfigurację środowiska IR Azure-SSIS przy użyciu programu PowerShell, uruchom `Set-AzureRmDataFactoryV2IntegrationRuntime` polecenia cmdlet z identyfikatora URI sygnatury dostępu Współdzielonego kontenera jako wartość dla nowych `SetupScriptContainerSasUri` parametru.

## <a name="next-steps"></a>Kolejne kroki

-   [Enterprise Edition, środowiska Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Jak tworzyć aplikacje płatne lub licencjonowane niestandardowych składników dla środowiska Azure-SSIS integration runtime](how-to-develop-azure-ssis-ir-licensed-components.md)
