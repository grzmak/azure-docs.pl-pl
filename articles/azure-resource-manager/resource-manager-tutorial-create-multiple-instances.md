---
title: Tworzenie wielu wystąpień zasobów przy użyciu usługi Azure Resource Manager | Microsoft Docs
description: Dowiedz się, jak utworzyć szablon usługi Azure Resource Manager w celu utworzenia wielu wystąpień zasobów platformy Azure.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/10/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 63a18a6ae0ee4c6e0a01bd7ac4a26a4fb89746c2
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419492"
---
# <a name="tutorial-create-multiple-resource-instances-using-resource-manager-templates"></a>Samouczek: tworzenie wielu wystąpień zasobów przy użyciu szablonów usługi Resource Manager

Dowiedz się, jak wykonywać iteracje w Twoim szablonie usługi Azure Resource Manager w celu utworzenia wielu wystąpień zasobu platformy Azure. W ostatnim samouczku został zmodyfikowany istniejący szablon w celu utworzenia zaszyfrowanego konta usługi Azure Storage. W tym samouczku możesz zmodyfikować ten sam szablon w celu utworzenia trzech wystąpień konta magazynu.

> [!div class="checklist"]
> * Otwieranie szablonu szybkiego startu
> * Edytowanie szablonu
> * Wdrożenie szablonu

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć pracę z tym artykułem, potrzebne są następujące zasoby:

* [Program Visual Studio Code](https://code.visualstudio.com/)
* Rozszerzenie Narzędzia usługi Resource Manager. Aby zainstalować to rozszerzenie, zobacz [Instalacja rozszerzenia Narzędzia usługi Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Otwieranie szablonu szybkiego startu

Szablon używany w tym przewodniku Szybki start ma nazwę [Create a standard storage account (Tworzenie standardowego konta magazynu)](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Szablon definiuje zasób konta usługi Azure Storage.

1. W programie Visual Studio Code wybierz pozycję **File (Plik)**>**Open File (Otwórz plik)**.
2. W polu **File name (Nazwa pliku)** wklej następujący adres URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wybierz pozycję **Open (Otwórz)**, aby otworzyć plik.
4. Wybierz pozycję **File (Plik)**>**Save As (Zapisz jako)**, aby zapisać plik jako **azuredeploy.json** na komputerze lokalnym.

## <a name="edit-the-template"></a>Edytowanie szablonu

Celem tego samouczka jest użycie iteracji zasobu, aby utworzyć trzy konta magazynu.  Przykładowy szablon tworzy tylko jedno konto magazynu. 

Z poziomu programu Visual Studio Code wprowadź następujące cztery zmiany:

![Usługa Azure Resource Manager tworzy wiele wystąpień](./media/resource-manager-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Dodaj element `copy` do definicji zasobu konta magazynu. W elemencie copy określ liczbę iteracji i nazwę tej pętli. Wartość licznika musi być dodatnią liczbą całkowitą i nie może przekraczać 800.
2. Funkcja `copyIndex()` zwraca bieżącą iterację w pętli. Funkcja `copyIndex()` rozpoczyna liczenie od zera. Aby przesunąć wartość indeksu, możesz przekazać wartość do funkcji copyIndex(). Na przykład *copyIndex(1)*.
3. Usuń element **variables**, ponieważ nie jest już używany.
4. Usuń element **outputs**.

Ukończony szablon wygląda następująco:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ]
}
```

Aby uzyskać więcej informacji na temat tworzenia wielu wystąpień, zobacz [Wdrażanie wielu wystąpień zasobu lub właściwości w szablonach usługi Azure Resource Manager](./resource-group-create-multiple.md)

## <a name="deploy-the-template"></a>Wdrożenie szablonu

Zapoznaj się z sekcją [Wdrażanie szablonu](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) przewodnika Szybki Start programu Visual Studio Code, aby uzyskać procedurę wdrażania.

Aby wyświetlić wszystkie trzy konta magazynu, pomiń parametr --name:

# <a name="clitabcli"></a>[Interfejs wiersza polecenia](#tab/CLI)
```cli
az storage account list --resource-group <ResourceGroupName>
```

# <a name="powershelltabpowershell"></a>[Program PowerShell](#tab/PowerShell)

```powershell
Get-AzureRmStorageAccount -ResourceGroupName <ResourceGroupName>
```

---

Porównaj nazwy kont magazynu z definicją nazwy w szablonie.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy zasoby platformy Azure nie będą już potrzebne, wyczyść wdrożone zasoby, usuwając grupę zasobów.

1. W witrynie Azure Portal wybierz pozycję **Grupa zasobów** z menu po lewej stronie.
2. Wprowadź nazwę grupy zasobów w polu **Filtruj według nazwy**.
3. Wybierz nazwę grupy zasobów.  W grupie zasobów zostanie wyświetlonych łącznie sześć zasobów.
4. Wybierz pozycję **Usuń grupę zasobów** z górnego menu.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób tworzenia wielu wystąpień konta magazynu. Do tej pory zostało utworzone jedno konto magazynu lub wiele wystąpień konta magazynu. W następnym samouczku utworzysz szablon z wieloma zasobami i wieloma typami zasobów. Niektóre zasoby zawierają zasoby zależne.

> [!div class="nextstepaction"]
> [Tworzenie zasobów zależnych](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
