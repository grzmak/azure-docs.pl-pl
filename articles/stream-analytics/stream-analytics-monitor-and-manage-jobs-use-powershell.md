---
title: Monitorowanie zadań i zarządzanie nimi usługi Azure Stream Analytics przy użyciu programu PowerShell
description: W tym artykule opisano sposób używania programu Azure PowerShell i polecenia cmdlet, monitorowanie i zarządzanie nią zadań usługi Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 6812298bc90446a7b7e136ec81a67641c2a9e18f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43701612"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Monitorowanie i zarządzanie nimi zadania usługi Stream Analytics za pomocą poleceń cmdlet programu Azure PowerShell
Dowiedz się, jak monitorowanie i zarządzanie zasobami usługi Stream Analytics, za pomocą poleceń cmdlet programu Azure PowerShell i skryptów programu powershell, które są wykonywane podstawowe zadania usługi Stream Analytics.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Wymagania wstępne dotyczące uruchamiania poleceń cmdlet programu Azure PowerShell dla usługi Stream Analytics
* Utwórz grupę zasobów platformy Azure w ramach subskrypcji. Poniżej przedstawiono przykładowy skrypt programu Azure PowerShell. Uzyskać programu Azure PowerShell, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview);  

Program Azure PowerShell 0.9.8:  

         # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Program Azure PowerShell 1.0:  

         # Log in to your Azure account
        Connect-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName "your sub" | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> Zadania usługi Stream Analytics utworzone programowo monitorowanie jest włączone domyślnie nie jest konieczne.  Można ręcznie włączyć monitorowanie w witrynie Azure Portal, przechodząc do strony Monitor zadania i klikając przycisk Włącz lub programowo to zrobić, wykonując kroki znajdujące się na [Azure Stream Analytics — Monitor zadania usługi Stream Analytics Programowo](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Poleceń cmdlet programu PowerShell platformy Azure dla usługi Stream Analytics
Następujące polecenia cmdlet programu Azure PowerShell może służyć do monitorowania i zarządzania zadań usługi Azure Stream Analytics. Należy pamiętać, że programu Azure PowerShell ma różne wersje. 
**W przykładach wymienionych pierwsze polecenie jest dla programu Azure PowerShell 0.9.8, drugie polecenie to dla programu Azure PowerShell w wersji 1.0.** Polecenia programu Azure PowerShell w wersji 1.0 będzie ono mieć zawsze "AzureRM" w poleceniu.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Wyświetla listę wszystkich zadań usługi Stream Analytics, zdefiniowana w określonej grupie zasobów lub subskrypcji platformy Azure lub pobiera informacje o zadaniu o konkretnym zadaniu w grupie zasobów.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

To polecenie programu PowerShell zwraca informacje o wszystkich zadań usługi Stream Analytics w subskrypcji platformy Azure.

**Przykład 2**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

To polecenie programu PowerShell zwraca informacje o wszystkie zadania usługi Stream Analytics w grupie zasobów StreamAnalytics-domyślne-centralny-US.

**Przykład 3**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

To polecenie programu PowerShell zwraca informacje o zadania usługi Stream Analytics StreamingJob w grupie zasobów StreamAnalytics-domyślne-centralny-US.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Wyświetla listę wszystkich danych wejściowych, które są zdefiniowane w ramach określonego zadania usługi Stream Analytics lub pobiera informacje o określonych danych wejściowych.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

To polecenie programu PowerShell zwraca informacje o wszystkich danych wejściowych zdefiniowane w zadaniu StreamingJob.

**Przykład 2**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

To polecenie programu PowerShell zwraca informacje o danych wejściowych o nazwie EntryStream zdefiniowane w zadaniu StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Wyświetla listę wszystkich wyjść, które są zdefiniowane w ramach określonego zadania usługi Stream Analytics lub pobiera informacje o określonych danych wyjściowych.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

To polecenie programu PowerShell zwraca informacje o danych wyjściowych, zdefiniowane w zadaniu StreamingJob.

**Przykład 2**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

To polecenie programu PowerShell zwraca informacje o danych wyjściowych o nazwie zdefiniowane w zadaniu StreamingJob danych wyjściowych.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Pobiera informacje na temat limitu przydziału jednostek w danym regionie przesyłania strumieniowego.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

To polecenie programu PowerShell zwraca informacje o limitach przydziałów i użycie jednostek przesyłania strumieniowego w regionie środkowe stany USA.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Pobiera informacje o odpowiednie przekształcenie zdefiniowane w ramach zadania usługi Stream Analytics.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Program Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

To polecenie programu PowerShell zwraca informacji na temat przekształcania o nazwie StreamingJob w zadaniu StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>New-AzureStreamAnalyticsInput | New-AzureRMStreamAnalyticsInput
Tworzy nowe dane wejściowe w ramach zadania usługi Stream Analytics, lub aktualizuje istniejące określone dane wejściowe.

W pliku JSON lub w wierszu polecenia można określić nazwę danych wejściowych. Jeśli są określone oba nazwa w wierszu polecenia musi być taka sama jak w pliku.

Jeśli Określ dane wejściowe, która już istnieje i nie zostanie określony parametr-Force, polecenia cmdlet zapyta, czy chcesz zastąpić istniejące dane wejściowe.

Jeśli określisz wymusić parametru i określ istniejącą, wprowadź nazwę, danych wejściowych zostanie zastąpiony bez potwierdzenia.

Aby uzyskać szczegółowe informacje o strukturze pliku JSON i zawartość, zobacz [utworzyć dane wejściowe (usługi Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] części [Stream Analytics Management REST API Reference Biblioteka][stream.analytics.rest.api.reference].

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

To polecenie programu PowerShell tworzy nowe dane wejściowe z pliku Input.json. Jeśli istniejące dane wejściowe o nazwie określonej w pliku definicji danych wejściowych jest już zdefiniowany, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz go zastąpić.

**Przykład 2**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

To polecenie programu PowerShell tworzy nowe dane wejściowe w ramach zadania o nazwie EntryStream. Jeśli istniejące dane wejściowe o tej nazwie jest już zdefiniowany, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz go zastąpić.

**Przykład 3**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

To polecenie programu PowerShell zastępuje istniejące źródło danych wejściowych o nazwie EntryStream przy użyciu definicję z pliku definicji.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>New-AzureStreamAnalyticsJob | New-AzureRMStreamAnalyticsJob
Tworzy nowe zadanie usługi Stream Analytics na platformie Microsoft Azure lub aktualizacji definicji istniejącego określonego zadania.

Nazwa zadania można określić w pliku JSON lub w wierszu polecenia. Jeśli są określone oba nazwa w wierszu polecenia musi być taka sama jak w pliku.

Możesz określić nazwę zadania, która już istnieje i nie zostanie określony parametr-Force, polecenia cmdlet zapyta, czy chcesz zastąpić istniejące zadanie.

Jeśli określisz Force parametru i określ istniejącą nazwę zadania, definicja zadania zostanie zastąpiony bez potwierdzenia.

Aby uzyskać szczegółowe informacje o strukturze pliku JSON i zawartość, zobacz [Utwórz zadanie usługi Stream Analytics] [ msdn-rest-api-create-stream-analytics-job] części [Stream Analytics Management REST API Reference Library] [stream.analytics.rest.api.reference].

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

To polecenie programu PowerShell tworzy nowe zadanie z definicją w JobDefinition.json. Jeśli istniejące zadanie o nazwie określonej w pliku definicji zadania jest już zdefiniowany, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz go zastąpić.

**Przykład 2**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

To polecenie programu PowerShell zastępuje StreamingJob definicji zadania.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>New-AzureStreamAnalyticsOutput | New-AzureRMStreamAnalyticsOutput
Tworzy nowe dane wyjściowe w ramach zadania usługi Stream Analytics, lub aktualizuje istniejące dane wyjściowe.  

W pliku JSON lub w wierszu polecenia można określić nazwę wyjściowego. Jeśli są określone oba nazwa w wierszu polecenia musi być taka sama jak w pliku.

Możesz określić danych wyjściowych, który już istnieje i nie zostanie określony parametr-Force, zapyta, czy zastąpić istniejące dane wyjściowe polecenia cmdlet.

Jeśli określisz wymusić parametru i określ istniejące dane wyjściowe nazwy, dane wyjściowe zostanie zastąpiony bez potwierdzenia.

Aby uzyskać szczegółowe informacje o strukturze pliku JSON i zawartość, zobacz [Tworzenie danych wyjściowych (usługi Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] części [Stream Analytics Management REST API Reference Biblioteka][stream.analytics.rest.api.reference].

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

To polecenie programu PowerShell tworzy nowe dane wyjściowe o nazwie "Wyjście" w zadaniu StreamingJob. Jeśli istniejące dane wyjściowe o tej nazwie jest już zdefiniowany, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz go zastąpić.

**Przykład 2**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

To polecenie programu PowerShell zastępuje definicję dla "Wyjście" w zadaniu StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>New-AzureStreamAnalyticsTransformation | New-AzureRMStreamAnalyticsTransformation
Tworzy nowe przekształcenie w ramach zadania usługi Stream Analytics lub aktualizuje istniejące przekształcania.

W pliku JSON lub w wierszu polecenia można określić nazwę transformacji. Jeśli są określone oba nazwa w wierszu polecenia musi być taka sama jak w pliku.

Możesz określić transformacji, która już istnieje i nie zostanie określony parametr-Force, polecenia cmdlet zapyta, czy należy zastąpić istniejące przekształcania.

Jeśli określisz wymusić parametru i określ istniejącą nazwę transformacji, przekształcenie zostanie zastąpiony bez potwierdzenia.

Aby uzyskać szczegółowe informacje o strukturze pliku JSON i zawartość, zobacz [tworzenie transformacji (usługi Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] części [interfejsu API usługi Stream Analytics Management REST Odwołanie do biblioteki][stream.analytics.rest.api.reference].

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

To polecenie programu PowerShell tworzy nowe przekształcenie o nazwie StreamingJobTransform w zadaniu StreamingJob. Jeśli istniejące transformacji jest już zdefiniowany o tej nazwie, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz go zastąpić.

**Przykład 2**

Program Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Program Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 To polecenie programu PowerShell zastępuje definicję StreamingJobTransform w zadaniu StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput | Remove-AzureRMStreamAnalyticsInput
Asynchronicznie usuwa określone dane wejściowe z zadania usługi Stream Analytics w systemie Microsoft Azure.  
Jeśli określisz parametr-Force, danych wejściowych jest usuwany bez potwierdzenia.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Program Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

To polecenie programu PowerShell usuwa wejściowego elementu EventStream w zadaniu StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Remove-AzureStreamAnalyticsJob | Remove-AzureRMStreamAnalyticsJob
Asynchronicznie usuwa określonego zadania usługi Stream Analytics w systemie Microsoft Azure.  
Jeśli określisz parametr-Force, zadanie jest usuwany bez potwierdzenia.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Program Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

To polecenie programu PowerShell usuwa zadanie StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput | Remove-AzureRMStreamAnalyticsOutput
Asynchronicznie usuwa określonych danych wyjściowych z zadania usługi Stream Analytics w systemie Microsoft Azure.  
Jeśli określisz parametr-Force, danych wyjściowych jest usuwany bez potwierdzenia.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Program Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

To dane wyjściowe dane wyjściowe w zadaniu StreamingJob usuwa polecenie programu PowerShell.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob
Asynchronicznie służy do wdrażania i uruchamia zadanie usługi Stream Analytics na platformie Microsoft Azure.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Program Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

To polecenie programu PowerShell uruchamia zadanie StreamingJob z godziną rozpoczęcia niestandardowe dane wyjściowe równa 12 grudnia 2012 r., 12:12:12 UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob
Asynchronicznie Zatrzymuje zadanie usługi Stream Analytics, działających na platformie Microsoft Azure i zwalnia zasoby, które były, które były używane. Definicja zadania i metadane pozostaną dostępne w ramach subskrypcji za pośrednictwem witryny Azure portal i interfejsów API zarządzania tak, aby edytować i ponownie uruchomić zadanie. Nie będzie naliczana dla zadania w stanie zatrzymania.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Program Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

To polecenie programu PowerShell Zatrzymuje zadanie StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Sprawdza możliwość łączenia się określonych danych wejściowych z usługi Stream Analytics.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Program Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Tego polecenia programu PowerShell sprawdza stan połączenia danych wejściowych EntryStream w StreamingJob.  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
Badania możliwości usługi Stream Analytics, połączyć się z określonym produktem wyjściowym.

**Przykład 1**

Program Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Program Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Ten stan połączenia danych wyjściowych dane wyjściowe w StreamingJob testów polecenia programu PowerShell.  

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej
Aby uzyskać dalszą pomoc, Wypróbuj nasz [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Kolejne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

