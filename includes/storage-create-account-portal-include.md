---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/18/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d59cb64e1499f703bf6913ab422d0f979caca324
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47008736"
---
Aby utworzyć konto magazynu ogólnego przeznaczenia w wersji 2 w witrynie Azure Portal, wykonaj następujące kroki:

1. W witrynie Azure Portal rozwiń menu po lewej stronie, aby otworzyć menu usług, a następnie wybierz pozycję **Wszystkie usługi**. Następnie przewiń w dół do pozycji **Storage** i wybierz pozycję **Konta magazynu**. W oknie **Konta magazynu**, które zostanie wyświetlone, wybierz pozycję **Dodaj**.
1. Wybierz subskrypcję, w ramach której chcesz utworzyć konto magazynu.
1. W polu **Grupa zasobów** kliknij pozycję **Utwórz nową**. Wprowadź nazwę nowej grupy zasobów, jak pokazano na poniższej ilustracji.

    ![Zrzut ekranu przedstawiający sposób tworzenia grupy zasobów w portalu](./media/storage-create-account-portal-include/create-resource-group.png)

1. Następnie wprowadź nazwę konta magazynu. Wybrana nazwa musi być unikatowa w obrębie platformy Azure, musi mieć długość od 3 do 24 znaków oraz może zawierać tylko cyfry i małe litery.
1. Wybierz lokalizację konta magazynu lub użyj lokalizacji domyślnej.
1. Pozostaw poniższe pola ustawione na wartości domyślne:
    - Pole **Model wdrażania** jest domyślnie ustawiane na wartość **Resource Manager**.
    - Pole **Wydajność** jest domyślnie ustawiane na wartość **Standardowa**.
    - Pole **Rodzaj konta** jest domyślnie ustawiane na wartość **StorageV2 (ogólnego przeznaczenia v2)**.
    - Pole **Replikacja** jest domyślnie ustawiane na wartość **Magazyn lokalnie nadmiarowy (LRS)**.
    - **Warstwa dostępu** jest domyślnie ustawiana na **Gorąca**.

1. Kliknij pozycję **Przejrzyj i utwórz** , aby przejrzeć ustawienia konta magazynu i utworzyć konto.

Aby uzyskać więcej informacji na temat typów kont magazynu i innych ustawień konta magazynu, zobacz [Azure storage account overview (Omówienie kont usługi Azure Storage)](https://docs.microsoft.com/azure/storage/common/storage-account-overview). Więcej informacji na temat grup zasobów znajduje się w temacie [Omówienie usługi Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). 
