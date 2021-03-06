---
title: Zarządzanie kontami laboratorium w usłudze Azure Lab Services | Dokumentacja firmy Microsoft
description: Dowiedz się, jak utworzyć konto laboratorium, są wyświetlane wszystkie konta laboratorium lub Usuń konto laboratorium w subskrypcji platformy Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: fd43c62f1a291a59d5d373437a49b263d6af4cb3
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345892"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Zarządzanie kontami laboratorium w usłudze Azure Lab Services 
W usługach Azure Lab Services konta laboratorium jest kontenerem dla zarządzanych laboratoriów, takich jak laboratorium na potrzeby zajęć. Administrator konfiguruje konta laboratorium przy użyciu usługi Azure Lab Services i zapewnia dostęp do laboratorium właścicieli, którzy mogą tworzyć laboratoriów w ramach konta. W tym artykule opisano sposób tworzenia konta laboratorium, są wyświetlane wszystkie konta laboratorium lub usuwanie konta laboratorium.

## <a name="create-a-lab-account"></a>Tworzenie konta laboratorium
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Z menu głównego po lewej stronie wybierz polecenie **Utwórz zasób**.
3. Wyszukaj pozycję **Lab Services** w witrynie Azure Marketplace, a następnie wybierz pozycję **Lab Services** z listy rozwijanej. 
4. Wybierz pozycję **Lab Services (wersja zapoznawcza)** z przefiltrowanej listy usług. 
5. W oknie **Tworzenie konta laboratorium** wybierz pozycję **Utwórz**.
7. W oknie **Konto laboratorium** wykonaj następujące czynności: 
    1. W obszarze **Nazwa konta laboratorium** wprowadź nazwę. 
    2. Wybierz **subskrypcję platformy Azure**, w której chcesz utworzyć konto laboratorium.
    3. W obszarze **Grupa zasobów** wybierz pozycję **Utwórz nową** i wprowadź nazwę grupy zasobów.
    4. W obszarze **Lokalizacja** wybierz lokalizację/region, w których chcesz utworzyć konto laboratorium. 
    5. Wybierz pozycję **Utwórz**. 

        ![Okno Tworzenie konta laboratorium](../media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Jeśli nie widzisz strony konta laboratorium, wybierz przycisk **powiadomień**, a następnie kliknij przycisk **Przejdź do zasobu** w obszarze powiadomień. 

    ![Okno Tworzenie konta laboratorium](../media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Zobaczysz następującą stronę **konta laboratorium**:

    ![Strona konta laboratorium](../media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Dodawanie użytkownika do roli twórcy laboratorium
Aby skonfigurować laboratorium na potrzeby zajęć w ramach konta laboratorium, użytkownik musi być członkiem roli **Twórca laboratorium** na koncie laboratorium. Konto, którego użyto do utworzenia konta laboratorium, jest automatycznie dodawane do tej roli. Jeśli planujesz użyć tego samego konta użytkownika w celu utworzenia laboratorium na potrzeby zajęć, możesz pominąć ten krok. Aby użyć innego konta użytkownika do utworzenia laboratorium na potrzeby zajęć, wykonaj następujące czynności: 

1. Na stronie **Konto laboratorium** wybierz pozycję **(Kontrola dostępu (IAM))** i kliknij pozycję **+ Dodaj** na pasku narzędzi. 

    ![Strona konta laboratorium](../media/tutorial-setup-lab-account/access-control.png)
2. Na stronie **Dodawanie uprawnień** wybierz pozycję **Twórca laboratorium** w obszarze **Rola**, wybierz użytkownika do dodania do roli Twórcy laboratorium, a następnie wybierz pozycję **Zapisz**. 

    ![Dodawanie użytkownika do roli twórcy laboratorium](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Określanie obrazów w portalu Marketplace dostępnych dla właścicieli laboratorium
Jako właściciel konta laboratorium można określić obrazów witryny Marketplace, które twórców laboratorium umożliwia tworzenie laboratoriów w ramach konta laboratorium. 

1. W menu po lewej stronie wybierz pozycję **Obrazy w portalu Marketplace**. Domyślnie zobaczysz pełną listę obrazów (włączonych i wyłączonych). Tę listę można filtrować, aby wyświetlić tylko obrazy włączone/wyłączone, wybierając z listy rozwijanej u góry opcję **Tylko włączone**/**Tylko wyłączone**. 
    
    ![Strona Obrazy w portalu Marketplace](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    Obrazów witryny Marketplace, które są wyświetlane na liście są tylko te, które spełniają następujące warunki:
        
    - Tworzy jedną maszynę Wirtualną.
    - Używa usługi Azure Resource Manager do aprowizacji maszyn wirtualnych
    - Nie wymaga zakupu dodatkowych licencji planu
2. Aby w portalu Marketplace **wyłączyć** obraz, który zostały włączony, wykonaj jedną z następujących czynności: 
    1. Wybierz pozycję **... (wielokropek)** w ostatniej kolumnie, a następnie polecenie **Wyłącz obraz**. 

        ![Wyłączanie jednego obrazu](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Wybierz jeden lub większą liczbę obrazów z listy przez zaznaczenie pól wyboru przed nazwami obrazów, a następnie wybierz pozycję **Wyłącz wybrane obrazy**. 

        ![Wyłączanie wielu obrazów](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Podobnie, aby w portalu Marketplace **włączyć** obraz, wykonaj jedną z następujących czynności: 
    1. Wybierz pozycję **... (wielokropek)** w ostatniej kolumnie, a następnie polecenie **Włącz obraz**. 
    2. Wybierz jeden lub większą liczbę obrazów z listy przez zaznaczenie pól wyboru przed nazwami obrazów, a następnie wybierz pozycję **Włącz wybrane obrazy**. 

## <a name="view-lab-accounts"></a>Wyświetl konta laboratorium
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz **wszystkie zasoby** z menu. 
3. Wybierz **Lab Services** dla **typu**. 
    Można również filtrować według subskrypcji, grupy zasobów, lokalizacje i tagów. 

## <a name="delete-a-lab-account"></a>Usuwanie konta laboratorium
Wykonaj instrukcje z poprzedniej sekcji, która wyświetla kontami laboratorium na liście. Użyj poniższych instrukcji, aby usunąć konto laboratorium: 

1. Wybierz **konta laboratorium** , którą chcesz usunąć. 
2. Wybierz **Usuń** na pasku narzędzi. 
3. Typ **tak** o potwierdzenie.
4. Wybierz pozycję **Usuń**. 

## <a name="next-steps"></a>Kolejne kroki
Wprowadzenie do konfigurowania laboratorium przy użyciu usługi Azure Lab Services:

- [Konfigurowanie laboratorium na potrzeby zajęć](tutorial-setup-classroom-lab.md)
- [Konfigurowanie laboratorium](../tutorial-create-custom-lab.md)
