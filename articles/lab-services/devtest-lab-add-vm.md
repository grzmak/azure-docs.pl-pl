---
title: Dodawanie maszyny Wirtualnej do laboratorium Azure DevTest Labs | Dokumentacja firmy Microsoft
description: Dowiedz się, jak dodać maszynę wirtualną do laboratorium Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: ce95a2177260e97113fd5e639671075eb6ad40cd
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215020"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Dodawanie maszyny Wirtualnej do laboratorium Azure DevTest Labs
Jeśli masz już [utworzyć pierwszą maszynę Wirtualną z](devtest-lab-create-first-vm.md), prawdopodobnie tak czy z wstępnie załadowane [obrazu z witryny marketplace](devtest-lab-configure-marketplace-images.md). Teraz, jeśli chcesz dodać kolejnych maszyn wirtualnych do środowiska laboratoryjnego, można także *podstawowy* oznacza to jedną [obrazu niestandardowego](devtest-lab-create-template.md) lub [formuły](devtest-lab-manage-formulas.md). Ten samouczek przeprowadzi Cię przez dodawanie maszyny Wirtualnej do laboratorium w usłudze DevTest Labs przy użyciu witryny Azure portal.

W tym artykule przedstawiono również sposób zarządzania artefaktów maszyny wirtualnej w środowisku laboratoryjnym.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Kroki, aby dodać maszyny Wirtualnej do laboratorium Azure DevTest Labs
1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Wybierz **wszystkich usług**, a następnie wybierz pozycję **DevTest Labs** z listy.
1. Zaznacz na liście laboratoriów laboratorium, w którym chcesz utworzyć maszynę Wirtualną.  
1. W laboratorium **Przegląd** okienku wybierz **+ Dodaj**.  

    ![Dodawanie przycisku maszyny Wirtualnej](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Na **wybierz podstawowej** okienku Wybierz podstawowy dla maszyny Wirtualnej.
1. Na **maszyny wirtualnej** okienku **nazwę maszyny wirtualnej** jest wstępnie wypełniane automatycznie o unikatowej nazwie wygenerowany automatycznie. Nazwa odnosi się do nazwy użytkownika w ramach swój adres e-mail, następuje unikatowy numer 3-cyfrowego. Ta funkcja zaoszczędzić czas myśleć o nazwę komputera, a następnie wpisz go za każdym razem, gdy tworzysz maszynę. To pole automatycznie wypełniony nazwą wybranego można zastąpić, jeśli chcesz. Aby zastąpić nazwę automatycznie wypełniony dla maszyny Wirtualnej, wprowadź nazwę w **nazwę maszyny wirtualnej** pola tekstowego. 

    ![Okienko maszyn wirtualnych laboratorium](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. **Nazwa_użytkownika** maszyny jest wstępnie wypełniony o unikatowej nazwie wygenerowany automatycznie. Nazwa odnosi się do nazwy użytkownika w ramach swój adres e-mail. Ta funkcja oszczędza czas do podejmowania decyzji o nazwę użytkownika, za każdym razem, gdy tworzysz nową maszynę. To pole automatycznie wypełniony przy użyciu nazwy użytkownika w wybranym można zastąpić ponownie, jeśli chcesz. Aby zastąpić wartość automatycznie wypełniony dla nazwy użytkownika, wprowadź wartość w **nazwa_użytkownika** pola tekstowego. Ten użytkownik otrzymuje **administratora** uprawnień na maszynie wirtualnej.     
1. Aby uzyskać **hasło**:
    
    W przypadku tworzenia pierwszej maszyny Wirtualnej w środowisku laboratoryjnym, wprowadź hasło w **wpisz wartość** pola tekstowego. Aby zapisać to hasło jako domyślne hasło w magazynie kluczy Azure skojarzonego z laboratorium, zaznacz **Zapisz jako domyślne hasło**. Domyślne hasło jest zapisywane w usłudze key vault o nazwie: **VmPassword**. Podczas próby tworzenia kolejnych maszyn wirtualnych w środowisku laboratoryjnym **VmPassword** jest automatycznie wybierany do **hasło**. Aby zastąpić wartości, wyczyść **używać hasła zapisane** pole wyboru, a następnie wprowadź hasło. 

    Można również najpierw zapisać wpisów tajnych w magazynie kluczy, a następnie użyć go podczas tworzenia maszyny Wirtualnej w środowisku laboratoryjnym. Aby uzyskać więcej informacji, zobacz [Store wpisów tajnych w magazynie kluczy](devtest-lab-store-secrets-in-key-vault.md). Aby użyć hasła przechowywane w usłudze key vault, wybierz **używać hasła zapisane**i określ wartości klucza, który odpowiada klucz tajny (hasło). 
3. **Typ dysku maszyny wirtualnej** Określa typy dysku magazynu jest dozwolone dla maszyn wirtualnych w środowisku laboratoryjnym.
4. Wybierz **rozmiar maszyny wirtualnej** i wybierz jeden z wstępnie zdefiniowanych elementów, które określają rdzeni procesora, pamięci RAM i rozmiar dysku twardego maszyny wirtualnej, aby utworzyć.
5. Wybierz **artefaktów** i — z listy artefaktów — wybierz i skonfiguruj artefaktów, które chcesz dodać do obrazu podstawowego.
    **Uwaga:** Jeśli jesteś nowym użytkownikiem usługi DevTest Labs, lub konfigurowanie artefaktów, dotyczą [Dodaj istniejący artefakt do maszyny Wirtualnej](#add-an-existing-artifact-to-a-vm) sekcji, a następnie wróć tutaj po zakończeniu.
6. Wybierz **Zaawansowane ustawienia** skonfigurować opcje wygaśnięcia i Opcje sieci maszyny Wirtualnej. 

   Aby ustawić opcję wygaśnięcia, wybierz ikonę kalendarza, aby określić datę, na którym maszyna wirtualna zostaną automatycznie usunięte.  Domyślnie maszyna wirtualna nigdy nie wygasa. 
1. Aby wyświetlić lub skopiuj szablon usługi Azure Resource Manager, zapoznaj się [szablonu Zapisz Azure Resource Manager](#save-azure-resource-manager-template) sekcji, a następnie wróć tutaj po zakończeniu.
1. Wybierz **Utwórz** dodać określoną maszynę Wirtualną do laboratorium.
1. W okienku laboratorium Wyświetla stan tworzenia maszyny Wirtualnej — najpierw jako **tworzenie**, następnie jako **systemem** po uruchomieniu maszyny Wirtualnej.

> [!NOTE]
> [Dodaj Maszynę wirtualną przejęcia](devtest-lab-add-claimable-vm.md) dowiesz się, jak utworzyć maszynę Wirtualną przejęcia tak, aby był dostępny do użytku przez dowolnego użytkownika w środowisku laboratoryjnym.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Dodaj istniejący artefakt do maszyny Wirtualnej
Podczas tworzenia maszyny Wirtualnej, można dodać artefaktami. Każde laboratorium, zawiera artefakty z publicznego repozytorium artefaktów laboratoria DevTest, jak również artefakty, które zostały utworzone i dodane do repozytorium artefaktów.

* Usługa Azure DevTest Labs *artefaktów* umożliwiają określenie *akcje* które są wykonywane, gdy maszyna wirtualna jest aprowizowana, takie jak uruchamianie skryptów środowiska Windows PowerShell, uruchamianie poleceń powłoki Bash i instalowania oprogramowania.
* Artefakt *parametry* można dostosować artefaktu dla określonego scenariusza

Aby dowiedzieć się, jak tworzyć artefakty, zapoznaj się z artykułem [Dowiedz się, jak tworzyć własne artefakty do użycia z usługą DevTest Labs](devtest-lab-artifact-author.md).

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Wybierz **wszystkich usług**, a następnie wybierz pozycję **DevTest Labs** z listy.
1. Zaznacz na liście laboratoriów laboratorium zawierającej maszynę Wirtualną, z którym chcesz pracować.  
1. Wybierz **Moje maszyny wirtualne**.
1. Wybierz odpowiednią maszynę Wirtualną.
1. Wybierz **Zarządzanie artefaktami**. 
1. Wybierz **artefakty stosowanie**.
1. Na **artefakty stosowanie** okienku zaznacz artefaktów, które chcesz dodać do maszyny Wirtualnej.
1. Na **Dodawanie artefaktu** okienku, wprowadź wymagane wartości parametrów i wszystkie parametry opcjonalne, które są potrzebne.  
1. Wybierz **Dodaj** Dodawanie artefaktu, a następnie wróć do **artefakty stosowanie** okienka.
1. Kontynuuj, dodawania artefaktów, zgodnie z potrzebami dla maszyny Wirtualnej.
1. Po dodaniu artefaktów można [zmienić kolejność, w którym są uruchamiane artefakty](#change-the-order-in-which-artifacts-are-run). Możesz również przejść z powrotem do [wyświetlić lub zmodyfikować artefakt](#view-or-modify-an-artifact).
1. Po zakończeniu dodawania artefaktów wybierz **Zastosuj**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Zmień kolejność uruchamiania artefaktów
Domyślnie akcje artefakty są wykonywane w kolejności, w której są dodawane do maszyny Wirtualnej. Następujące kroki pokazują jak zmienić kolejność, w którym są uruchamiane artefaktów.

1. W górnej części **artefakty stosowanie** okienku zaznacz łącze określającą liczbę artefaktów, które zostały dodane do maszyny Wirtualnej.
   
    ![Liczbę artefaktów, które dodano do maszyny Wirtualnej](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Na **wybrane artefaktów** okienku przeciągnij i upuść artefaktów w odpowiedni sposób. **Uwaga:** Jeśli masz problemy z przeciąganiem artefaktu, upewnij się, przeciągasz z lewej strony artefaktu. 
1. Po zakończeniu wybierz przycisk **OK**.  

## <a name="view-or-modify-an-artifact"></a>Wyświetlanie lub modyfikowanie artefakt
Poniższe kroki ilustrują wyświetlanie lub modyfikowanie parametrów artefaktu:

1. W górnej części **artefakty stosowanie** okienku zaznacz łącze określającą liczbę artefaktów, które zostały dodane do maszyny Wirtualnej.
   
    ![Liczbę artefaktów, które dodano do maszyny Wirtualnej](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Na **wybrane artefaktów** okienku wybierz artefakt, który chcesz wyświetlić lub edytować.  
1. Na **Dodawanie artefaktu** okienka, upewnij dowolne wymagane zmiany, a następnie wybierz **OK** zamknąć **Dodawanie artefaktu** okienka.
1. Wybierz **OK** zamknąć **wybrane artefaktów** okienka.

## <a name="save-azure-resource-manager-template"></a>Zapisz szablon usługi Azure Resource Manager
Szablon usługi Azure Resource Manager pozwala na deklaratywne Definiowanie powtarzalnych wdrożeń. Poniższe kroki wyjaśniają, jak można zapisać szablonu usługi Azure Resource Manager dla maszyny Wirtualnej Trwa tworzenie.
Po zapisaniu można użyć szablonu usługi Azure Resource Manager do [wdrażania nowych maszyn wirtualnych przy użyciu programu Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Na **maszyny wirtualnej** okienku wybierz **wyświetlanie usługi Azure Resource Manager szablonu**.
2. Na **widok usługi Azure Resource Manager szablon** okienku zaznacz tekst, szablon.
3. Skopiowanie zaznaczonego tekstu do Schowka.
4. Wybierz **OK** zamknąć **okienku Wyświetl usługi Azure Resource Manager szablon**.
5. Otwórz Edytor tekstów.
6. Wklej tekst szablon ze Schowka.
7. Zapisz plik w celu późniejszego użycia.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Kolejne kroki
* Po utworzeniu maszyny Wirtualnej można nawiązać maszyny Wirtualnej, wybierając **Connect** w okienku maszyny Wirtualnej.
* Dowiedz się, jak [Tworzenie niestandardowych artefaktów dla maszyn wirtualnych usługi DevTest Labs](devtest-lab-artifact-author.md).
* Zapoznaj się z [galerii szablonów DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
