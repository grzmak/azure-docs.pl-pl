---
title: Wdrażanie szablonów za pomocą programu Visual Studio w usłudze Azure Stack | Dokumentacja firmy Microsoft
description: Dowiedz się, jak wdrażanie szablonów za pomocą programu Visual Studio w usłudze Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 1d61313018b0cd16594772676172737a7d8fc2cd
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366697"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Wdrażanie szablonów w usłudze Azure Stack przy użyciu programu Visual Studio

*Dotyczy: Usługa Azure Stack zintegrowane systemy i usługi Azure Stack Development Kit*

Aby wdrożyć szablony usługi Azure Resource Manager do usługi Azure Stack, można użyć programu Visual Studio.

## <a name="to-deploy-a-template"></a>Aby wdrożyć szablon

1. [Instalowanie i połączyć](azure-stack-install-visual-studio.md) do usługi Azure Stack przy użyciu programu Visual Studio.
2. Otwórz program Visual Studio.
3. Wybierz **pliku**, a następnie wybierz pozycję **New**. W **nowy projekt**, wybierz opcję **grupy zasobów platformy Azure**.
4. Wprowadź **nazwa** dla nowego projektu, a następnie wybierz **OK**.
5. W **wybierz szablon Azure**, wybierz **szybkiego startu platformy Azure Stack** z listy rozwijanej.
6. Wybierz **101-create-storage-account**, a następnie wybierz pozycję **OK**.
7. W nowym projekcie, rozwiń węzeł **szablony** w węźle **Eksploratora rozwiązań** wyświetlić dostępnych szablonów.
8. W **Eksploratora rozwiązań**, wybierz nazwę projektu, a następnie wybierz **Wdróż**. Wybierz **nowe wdrożenie**.
9. W **wdrażanie w grupie zasobów**, użyj **subskrypcji** listy rozwijanej wybierz subskrypcję usługi Microsoft Azure Stack.
10. Z **grupy zasobów** listy, wybierz istniejącą grupę zasobów lub Utwórz nową.
11. Z **lokalizację grupy zasobów** listy, wybierz lokalizację, a następnie wybierz **Wdróż**.
12. W **Edytuj parametry**, podaj wartości parametrów, (które różnią się przez szablon), a następnie wybierz pozycję **Zapisz**.

## <a name="next-steps"></a>Kolejne kroki

* [Wdrażanie szablonów za pomocą wiersza polecenia](azure-stack-deploy-template-command-line.md)
* [Tworzenie szablonów usługi Azure Stack](azure-stack-develop-templates.md)
