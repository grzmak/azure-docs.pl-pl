---
title: Powiadomienia usługi Azure Active Directory Identity Protection | Dokumentacja firmy Microsoft
description: Dowiedz się, jak powiadomienia obsługi działań mających na badania.
services: active-directory
keywords: Usługa Azure active directory identity protection odnajdywania aplikacji w chmurze, zarządzanie aplikacji, zabezpieczenia, ryzyka, poziom ryzyka, luk w zabezpieczeniach, zasady zabezpieczeń
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 0a546acd05246e011fa66abea8a667d0b3513588
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005902"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Powiadomienia usługi Azure Active Directory Identity Protection

Usługa Azure AD Identity Protection wysyła dwa rodzaje automatyczne powiadomienie o wiadomości e-mail, aby ułatwić zarządzanie użytkownikiem i ryzyko zdarzeń o podwyższonym ryzyku:

- Narażeni użytkownicy wykryto wiadomości e-mail
- Cotygodniowe podsumowanie wiadomości e-mail.

Ten artykuł zawiera omówienie zarówno wiadomości e-mail z powiadomieniem.


## <a name="users-at-risk-detected-email"></a>Narażeni użytkownicy wykryto wiadomości e-mail

W odpowiedzi na wykryte konta na ryzyko, Azure AD Identity Protection generuje alerty w wiadomościach e-mail z **wykryto narażonych użytkowników** jako podmiotu. Wiadomość e-mail zawiera link do ** [użytkownicy oflagowani w związku z ryzykiem](../reports-monitoring/concept-user-at-risk.md) ** raportu. Najlepszym rozwiązaniem należy natychmiast Zbadaj narażeni użytkownicy.

![Narażeni użytkownicy wykryto wiadomości e-mail](./media/notifications/01.png)


### <a name="configuration"></a>Konfigurowanie

Jako administrator możesz ustawić:

- **Poziom ryzyka, wyzwalającego Generowanie tę wiadomość e-mail** — Domyślnie poziom ryzyka jest równa "Wysoka" ryzyka.
- **Adresaci wiadomości e-mail** — domyślnie adresatów zawierają wszystkich administratorów globalnych. Administratorzy globalni można również dodać inne Administratorzy globalni, Administratorzy zabezpieczeń, czytelnicy zabezpieczeń jako adresatów.  


Aby otworzyć okno dialogowe powiązane, kliknij przycisk **alerty** w **ustawienia** części **Identity Protection** strony.

![Narażeni użytkownicy wykryto wiadomości e-mail](./media/notifications/05.png)


## <a name="weekly-digest-email"></a>Cotygodniowe podsumowanie wiadomości e-mail.

Co tydzień wiadomość e-mail z podsumowaniem zawiera podsumowanie nowych zdarzeń o podwyższonym ryzyku.  
Zawiera ona:

- Narażeni użytkownicy

- Podejrzane działania

- Wykryto luk w zabezpieczeniach

- Linki do powiązanych raportów w Identity Protection

    ![Korygowanie](./media/notifications/400.png "korygowania")

### <a name="configuration"></a>Konfigurowanie

Jako administrator możesz przełączyć wysyła cotygodniowe wiadomość e-mail z podsumowaniem.

![Ryzyka użytkownika](./media/notifications/62.png "ryzyka użytkownika")

Aby otworzyć okno dialogowe powiązane, kliknij przycisk **cotygodniowe podsumowanie** w **ustawienia** części **Identity Protection** strony.

![Narażeni użytkownicy wykryto wiadomości e-mail](./media/notifications/04.png)


## <a name="see-also"></a>Zobacz także

- [Usługa Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
