| Zasób | Limit domyślny | Limit maksymalny |
| --- | --- | --- |
| Zasoby na [grupy zasobów](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (na typ zasobu) |800 |Różni się dla typu zasobu |
| Wdrożenia dla każdej grupy zasobów w historii wdrożenia |800 |800 |
| Zasoby na wdrożenie |800 |800 |
| Blokady zarządzania (na unikatową nazwę zakres) |20 |20 |
| Liczba tagów (na zasób lub grupa zasobów) |15 |15 |
| Długość klucza tagu |512 |512 |
| Długość wartości tagu |256 |256 |


#### <a name="template-limits"></a>Limity szablonu

| Wartość | Limit domyślny | Limit maksymalny |
| --- | --- | --- |
| Parametry |256 |256 |
| Zmienne |256 |256 |
| Zasoby (w tym liczba kopii) |800 |800 |
| Dane wyjściowe |64 |64 |
| Wyrażenie szablonu |24 576 znaki |24 576 znaki |
| Zasoby w wyeksportowanymi szablonami |200 |200 | 
| Rozmiar szablonu |1 MB |1 MB |
| Rozmiar pliku parametrów |64 KB |64 KB |

Pewne ograniczenia szablonu może przekroczyć przy użyciu zagnieżdżonych szablonów. Aby uzyskać więcej informacji, zobacz [podczas wdrażania zasobów platformy Azure, za pomocą połączonymi szablonami](../articles/azure-resource-manager/resource-group-linked-templates.md). Aby zmniejszyć liczbę parametrów, zmienne ani danych wyjściowych, możesz połączyć kilka wartości do obiektu. Aby uzyskać więcej informacji, zobacz [obiektów jako parametrów](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

Jeśli przekroczysz limit 800 wdrożeń dla grupy zasobów, należy usunąć wdrożenia z historii, które nie są już potrzebne. Można usunąć wpisów z historii z [Usuń wdrożenie grupy az](/cli/azure/group/deployment#az_group_deployment_delete) wiersza polecenia platformy Azure lub [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) w programie PowerShell. Usuwanie wpisu z historii wdrożenia nie ma wpływu na zasoby wdrażania. 