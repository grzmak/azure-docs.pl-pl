---
title: Pobieranie wyników przeglądu dostępu usługi Azure AD | Microsoft Docs
description: Jak pobrać wyniki przeglądów dostępu usługi Azure Active Directory.
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
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 299e7884311466a34f020735de895ea2ad9945ce
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45608235"
---
# <a name="retrieve-access-review-results"></a>Pobieranie wyników przeglądu dostępu

Administratorzy mogą używać usługi Azure Active Directory (Azure AD) do [tworzenia przeglądów dostępu](create-access-review.md) dla członków grupy lub użytkowników przypisanych do aplikacji.  Użytkownik należący do roli **Administrator globalny**, **Administrator kont użytkowników**, **Administrator zabezpieczeń** lub **Czytelnik zabezpieczeń** może także przeczytać wyniki przeglądu dostępu.  Aby przypisać użytkowników do jednej z tych ról, administrator ról uprzywilejowanych może użyć usługi Azure AD PIM do uprawnienia użytkownika do aktywowania roli, a administrator globalny może trwale [przypisać użytkownika do roli](../fundamentals/active-directory-users-assign-role-azure-portal.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="locating-an-access-review"></a>Lokalizowanie przeglądu dostępu

Jeśli wiesz, który program zawiera przegląd dostępu, przejdź na stronę przeglądów dostępu, wybierz pozycję **Programy** i wybierz program zawierający kontrolę przeglądu dostępu.  Następnie wybierz pozycję **Kontrole** i kontrolę przeglądu dostępu. Jeśli program zawiera wiele kontroli, możesz filtrować kontrole pod kątem określonego typu i sortować je według stanu. Istnieje również możliwość wyszukiwania według nazwy kontroli przeglądu dostępu lub nazwy wyświetlanej jej twórcy. 

Jeśli nie wiesz, który program zawiera przegląd dostępu, przejdź na stronę przeglądów dostępu i wybierz pozycję **Kontrole**.  Możesz filtrować kontrole pod kątem określonego typu i sortować je według stanu, a także wyszukiwać według nazwy kontroli przeglądu dostępu lub nazwy wyświetlanej jej twórcy. 

## <a name="retrieving-the-results-for-a-one-time-access-review"></a>Pobieranie wyników jednorazowego przeglądu dostępu

Jeśli przeglądy są wykonywane jednorazowo, postęp aktywnego przeglądu dostępu i wyniki ukończonego przeglądu dostępu można uzyskać w sekcji **Wyniki**.  Istnieje możliwość wpisania nazwy wyświetlanej lub nazwy głównej użytkownika, którego dostęp jest przeglądany, w celu wyświetlenia tylko dostępów tego użytkownika.  Aby pobrać wszystkie wyniki ukończonego przeglądu dostępu, kliknij przycisk **Pobierz**.

## <a name="retrieving-the-results-for-multiple-instances-of-a-recurring-access-review"></a>Pobieranie wyników dla wielu wystąpień cyklicznego przeglądu dostępu

Aby wyświetlić postęp aktywnego cyklicznego przeglądu dostępu, kliknij sekcję **Wyniki**.  Istnieje możliwość wpisania nazwy wyświetlanej lub nazwy głównej użytkownika, którego dostęp jest przeglądany.

Aby wyświetlić wyniki ukończonego wystąpienia cyklicznego przeglądu dostępu, wybierz pozycję **Historia przeglądów**, a następnie wybierz konkretne wystąpienie z listy ukończonych wystąpień przeglądu dostępu na podstawie daty rozpoczęcia i zakończenia wystąpienia.   Wyniki dla tego wystąpienia można uzyskać w sekcji **Wyniki**.  Istnieje możliwość wpisania nazwy wyświetlanej lub nazwy głównej użytkownika, którego dostęp jest przeglądany, w celu wyświetlenia tylko dostępów tego użytkownika.  Aby pobrać wszystkie wyniki ukończonego wystąpienia cyklicznego przeglądu dostępu, kliknij przycisk **Pobierz**.


## <a name="removing-users-from-an-access-review"></a>Usuwanie użytkowników z przeglądu dostępu

Domyślnie usunięty użytkownik pozostaje usunięty w usłudze Azure AD przez 30 dni i w tym okresie administrator może przywrócić go w razie potrzeby.  Po 30 dniach użytkownik jest trwale usuwany.  Ponadto przy użyciu portalu usługi Azure Active Directory administrator globalny może jawnie [trwale usunąć niedawno usuniętego użytkownika](../fundamentals/active-directory-users-restore.md) przed upłynięciem tego okresu.  Gdy użytkownik zostanie trwale usunięty, nowe dane dotyczące tego użytkownika będą usuwane z aktywnych przeglądów dostępu.  Informacje inspekcji dotyczące usuniętych użytkowników pozostają w dzienniku inspekcji.

## <a name="next-steps"></a>Kolejne kroki

- [Zarządzanie dostępem użytkowników za pomocą przeglądów dostępu w usłudze Azure AD](manage-user-access-with-access-reviews.md)
- [Zarządzanie dostępem gości za pomocą przeglądów dostępu w usłudze Azure AD](manage-guest-access-with-access-reviews.md)
- [Zarządzanie programami i kontrolami obejmującymi przeglądy dostępu w usłudze Azure AD](manage-programs-controls.md)
- [Create an access review for members of a group or access to an application (Tworzenie przeglądu dostępu dla członków grupy lub na potrzeby dostępu do aplikacji)](create-access-review.md)
- [Tworzenie przeglądu dostępu użytkowników ról administracyjnych usługi Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)


