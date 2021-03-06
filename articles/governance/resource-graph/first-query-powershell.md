---
title: Uruchamianie pierwszego zapytania usługi Resource Graph przy użyciu programu Azure PowerShell
description: W tym artykule przedstawiono kroki umożliwiające włączenie modułu usługi Resource Graph dla programu Azure PowerShell i uruchomienie pierwszego zapytania.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 1a2bc5626e94f5fcb0ec8c2be8d91c8fc6484e0b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224566"
---
# <a name="run-your-first-resource-graph-query-using-azure-powershell"></a>Uruchamianie pierwszego zapytania usługi Resource Graph przy użyciu programu Azure PowerShell

Pierwszym krokiem do korzystania z usługi Azure Resource Graph jest zainstalowanie modułu dla programu Azure PowerShell. Ten przewodnik Szybki start przeprowadzi Cię przez proces dodawania modułu do instalacji programu Azure PowerShell.

Po zakończeniu tego procesu będziesz mieć moduł dodany do wybranej instalacji programu Azure PowerShell i uruchomisz swoje pierwsze zapytanie usługi Resource Graph.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="add-the-resource-graph-module"></a>Dodaj moduł usługi Resource Graph

Aby włączyć program Azure PowerShell do wykonywania zapytań do usługi Azure Resource Graph, musisz dodać moduł. Ten moduł może być używany z lokalnie zainstalowanym środowiskiem Windows PowerShell i programem PowerShell Core, jak również [obrazem platformy Docker programu Azure PowerShell](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Wymagania podstawowe

Moduł usługi Azure Resource Graph wymaga następującego oprogramowania:

- Programu Azure PowerShell w wersji 6.3.0 lub nowszej. Jeśli jeszcze go nie zainstalowano, postępuj zgodnie z [tymi instrukcjami](/powershell/azure/install-azurerm-ps).

  - W przypadku programu PowerShell Core użyj wersji **Az** modułu Azure PowerShell.

  - W przypadku środowiska Windows PowerShell użyj wersji **AzureRm** modułu Azure PowerShell.

  > [!NOTE]
  > Obecnie nie zaleca się instalowania modułu w usłudze Cloud Shell.

- Moduł PowerShellGet. Jeśli jeszcze nie został on zainstalowany lub zaktualizowany, postępuj zgodnie z [tymi instrukcjami](/powershell/gallery/installing-psget).

### <a name="powershell-core"></a>Program PowerShell Core

Moduł usługi Resource Graph dla programu PowerShell Core to **Az.ResourceGraph**.

1. Za pomocą **administracyjnego** monitu programu PowerShell Core uruchom następujące polecenie:

   ```powershell
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Zweryfikuj, czy moduł został zaimportowany i ma poprawną wersję (0.2.0):

   ```powershell
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

1. Włącz wsteczne aliasy dla wersji **Az** do wersji **AzureRm** za pomocą następującego polecenia:

   ```powershell
   # Enable backwards alias compatibility
   Enable-AzureRmAlias
   ```

### <a name="windows-powershell"></a>Windows PowerShell

Moduł usługi Resource Graph dla programu Windows PowerShell to **AzureRm.ResourceGraph**.

1. Za pomocą **administracyjnego** monitu środowiska Windows PowerShell uruchom następujące polecenie:

   ```powershell
   # Install the Resource Graph (prerelease) module from PowerShell Gallery
   Install-Module -Name AzureRm.ResourceGraph -AllowPrerelease
   ```

1. Zweryfikuj, czy moduł został zaimportowany i ma poprawną wersję (0.1.0-preview):

   ```powershell
   # Get a list of commands for the imported AzureRm.ResourceGraph module
   Get-Command -Module 'AzureRm.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>Uruchamianie pierwszego zapytania usługi Resource Graph

Teraz, gdy moduł programu Azure PowerShell został dodany do Twojego wybranego środowiska, nadszedł czas na wypróbowanie prostego zapytania usługi Resource Graph. Zapytanie zwróci pierwsze pięć zasobów platformy Azure przy użyciu właściwości **Name** i **Resource Type** każdego zasobu.

1. Uruchom swoje pierwsze zapytanie usługi Azure Resource Graph za pomocą polecenia cmdlet `Search-AzureRmGraph`:

   ```powershell
   # Login first with Connect-AzureRmAccount

   # Run Azure Resource Graph query
   Search-AzureRmGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Ponieważ to przykładowe zapytanie nie udostępnia modyfikatora sortowania takiego jak `order by`, wielokrotne uruchomienie tego zapytania będzie prawdopodobnie zwracało inny zestaw zasobów dla każdego żądania.

1. Zaktualizuj zapytanie, dodając modyfikator `order by` do właściwości **Name**:

   ```powershell
   # Run Azure Resource Graph query with 'order by'
   Search-AzureRmGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > Tak samo jak w przypadku pierwszego zapytania, wielokrotne uruchomienie tego zapytania prawdopodobnie zwróci inny zestaw zasobów dla każdego żądania. Kolejność poleceń zapytania jest ważna. W tym przykładzie polecenie `order by` następuje po poleceniu `limit`. Spowoduje to najpierw ograniczenie wyników zapytania, a następnie ich uporządkowanie.

1. Zaktualizuj zapytanie, aby najpierw wykonywało polecenie `order by` w celu sortowania według właściwości **Name**, a następnie polecenie `limit` w celu ograniczenia do 5 pierwszych wyników:

   ```powershell
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzureRmGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Gdy końcowe zapytanie zostanie uruchomione wielokrotnie, zakładając, że nic się nie zmieniło w Twoim środowisku, zwrócone wyniki będą spójne i zgodne z oczekiwaniami — uporządkowane według właściwości **Name**, ale nadal ograniczone do pierwszych 5 wyników.

## <a name="cleanup"></a>Czyszczenie

Jeśli chcesz usunąć moduł usługi Resource Graph ze środowiska programu Azure PowerShell, możesz to zrobić za pomocą następującego polecenia:

```powershell
# Remove the Resource Graph module from the Azure PowerShell environment
Remove-Module -Name 'AzureRm.ResourceGraph'
```

> [!NOTE]
> Nie powoduje ono usunięcia wcześniej pobranego pliku modułu. Powoduje tylko usunięcie go z uruchomionej sesji programu PowerShell.

## <a name="next-steps"></a>Następne kroki

- Uzyskaj więcej informacji na temat [języka zapytań](./concepts/query-language.md)
- Dowiedz się, jak [eksplorować zasoby](./concepts/explore-resources.md)
- Uruchamianie pierwszego zapytania za pomocą [interfejsu wiersza polecenia platformy Azure](first-query-azurecli.md)
- Zobacz przykłady [zapytań dla początkujących](./samples/starter.md)
- Zobacz przykłady [zapytań zaawansowanych](./samples/advanced.md)
- Podziel się opinią na platformie [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)