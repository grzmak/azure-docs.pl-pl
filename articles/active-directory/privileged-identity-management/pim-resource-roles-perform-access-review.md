---
title: Wykonywanie przeglądu dostępu z Moje role zasobów platformy Azure w usłudze PIM | Dokumentacja firmy Microsoft
description: Dowiedz się, jak przeprowadzić przegląd dostępu role zasobów platformy Azure w usłudze Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: a96a1de7828797f1124280fca95a3358210b55b7
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189722"
---
# <a name="perform-an-access-review-of-my-azure-resource-roles-in-pim"></a>Wykonywanie przeglądu dostępu z Moje role zasobów platformy Azure w usłudze PIM
Privileged Identity Management (PIM) dla zasobów platformy Azure upraszcza, jak zarządzać uprzywilejowanego dostępu do zasobów na platformie Azure w przedsiębiorstwach. 

Jeśli masz przypisaną rolę administracyjną, administratorem ról uprzywilejowanych Twoja organizacja może poprosić o regularnie upewnij się, nadal potrzebujesz tej roli dla zadania. Możesz otrzymać wiadomość e-mail zawierającą łącze lub możesz przejść bezpośrednio do [witryny Azure portal](https://portal.azure.com). Wykonaj kroki opisane w tym artykule, aby wykonać własnym przeglądu przypisane role.

Jeśli jesteś administratorem ról uprzywilejowanych, zainteresowani przeglądów dostępu, Dowiedz się więcej o [sposobu uruchamiania przeglądu dostępu](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Dodawanie aplikacji Privileged Identity Management
Można użyć aplikacji PIM usługi Azure Active Directory (Azure AD) w [witryny Azure portal](https://portal.azure.com/) przeprowadzić zapoznania się z nimi. Jeśli nie masz aplikację w portalu, wykonaj następujące kroki, aby rozpocząć pracę.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Wybierz użytkownika nazwy w prawym górnym rogu witryny Azure Portal, a następnie wybierz katalog, w którym można będzie działać.
3. Wybierz **wszystkich usług**i użyj **filtru** wyszukać *usługi Azure AD Privileged Identity Management*.
4. Sprawdź **Przypnij do pulpitu nawigacyjnego**, a następnie wybierz pozycję **Utwórz**. Otwiera się aplikacja usługi PIM.

## <a name="approve-or-deny-access"></a>Zatwierdź lub Odrzuć dostęp
Podczas zatwierdzania lub nie zezwoli na dostęp, możesz teraz po prostu informuje recenzenta czy możesz nadal używać tej roli lub nie. Wybierz **Zatwierdź** aby pozostać w tej roli, lub **Odmów** Jeśli nie potrzebujesz już dostęp. Jego stan jest zmieniany tylko wtedy, gdy recenzent stosuje wyniki.

Wykonaj następujące kroki, aby znaleźć i zakończyć Przegląd dostępu:
1. Przejdź do aplikacji Azure AD PIM.
2. Wybierz **Przegląd dostępu wszystkich użytkowników** bloku.

   ![Zrzut ekranu usługi PIM aplikacji, za pomocą bloku dostępu Przejrzyj wybrane](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Wybierz pozycję Przegląd, który chcesz wykonać. 
4. Wybierz opcję **zatwierdzić** lub **Odmów**. W **zapewnia Przyczyna pole**, może być konieczne w celu uwzględnienia przyczynę swojej decyzji.

   ![Strona szczegółów zrzut ekranu Przegląd](media/azure-pim-resource-rbac/rbac-access-review-choice.png)

## <a name="next-steps"></a>Kolejne kroki

- [Wykonywanie przeglądu dostępu z Moje role katalogu usługi Azure AD w usłudze PIM](pim-how-to-perform-security-review.md)