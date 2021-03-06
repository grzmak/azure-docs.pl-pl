---
title: Konfigurowanie alertów zabezpieczeń dla ról katalogu usługi Azure AD w usłudze PIM | Dokumentacja firmy Microsoft
description: Dowiedz się, jak skonfigurować alerty zabezpieczeń dla ról katalogu usługi Azure AD w usłudze Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4e1cb47989011f179c54061bd29ae55b4ff86d80
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669415"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Konfigurowanie alertów zabezpieczeń dla ról katalogu usługi Azure AD w usłudze PIM
## <a name="security-alerts"></a>Alerty zabezpieczeń
Azure Privileged Identity Management (PIM) generuje alerty w przypadku działania związane z niebezpieczne lub podejrzane w danym środowisku. Po wyzwoleniu alertu ona wyświetlona na pulpicie nawigacyjnym usługi PIM. Wybierz alert, aby wyświetlić raport zawierający listę użytkowników lub ról, które wyzwoliła alert.

![Alerty zabezpieczeń pulpitu nawigacyjnego usługi PIM — zrzut ekranu](./media/pim-how-to-configure-security-alerts/PIM_security_dash.png)

| Alerty | Ważność | Wyzwalacz | Zalecenie |
| --- | --- | --- | --- |
| **Role są przypisywane poza usługą PIM** |Wysoka |Użytkownikowi przypisano trwale ról uprzywilejowanych, poza interfejsu usługi PIM. |Przejrzyj użytkowników na liście i Cofnij przypisanie, ich z uprzywilejowane role przypisane poza usługą PIM. |
| **Role są aktywowane zbyt często** |Medium |Wystąpiło zbyt wiele reaktywacji o tej samej roli w ustalonym terminie w ustawieniach. |Skontaktuj się z użytkownika, aby zobaczyć, dlaczego one uaktywniono rolę tak wiele razy. Może być limitu czasu jest zbyt mała w przypadku ich do wykonywania swoich zadań lub może ich za pomocą skryptów automatycznej aktywacji roli. Upewnij się, że czas trwania aktywacji w ramach swojej roli ustawiono wystarczająco długo, aby je do wykonywania ich zadań. |
| **Role nie wymagają uwierzytelniania wieloskładnikowego w celu aktywacji** |Medium |Dostępne są role, bez włączony w ustawieniach usługi MFA. |Firma Microsoft wymaga uwierzytelniania Wieloskładnikowego dla najbardziej wysoce uprzywilejowanych ról, ale zdecydowanie zachęcamy włączyć usługę MFA do aktywacji wszystkich ról. |
| **Użytkownicy nie są z ich ról uprzywilejowanych** |Małe |Ma kwalifikujących się Administratorzy, którzy nie został ostatnio aktywowany ich ról. |Uruchamianie przeglądu dostępu w celu określenia użytkowników, którzy nie muszą już mieć dostęp. |
| **Istnieje zbyt wiele Administratorzy globalni** |Małe |Istnieją administratorzy charakter bardziej globalny niż zalecane. |Jeśli masz dużą liczbę administratorów globalnych, jest prawdopodobne, że użytkownicy otrzymują więcej uprawnień niż jest to wymagane. Przenieść użytkowników do mniej uprzywilejowanych ról lub upewnij niektóre z nich kwalifikuje się do roli zamiast trwale przypisane. |

### <a name="severity"></a>Ważność
* **Wysoka**: wymaga natychmiastowego działania z powodu naruszenia zasad. 
* **Średnia**: nie wymagać natychmiastowego działania, ale sygnalizuje potencjalne naruszenie zasad.
* **Niska**: nie wymagać natychmiastowego działania, ale sugeruje zmianę preferrable zasady.

## <a name="configure-security-alert-settings"></a>Konfigurowanie ustawień alertów zabezpieczeń
Można dostosować niektóre alerty zabezpieczeń w usłudze PIM chcesz pracować w swoim środowisku i cele zabezpieczeń. Wykonaj następujące kroki, aby dotrzeć do bloku ustawienia:

1. Zaloguj się do [witryny Azure portal](https://portal.azure.com/) i wybierz **usługi Azure AD Privileged Identity Management** kafelka na pulpicie nawigacyjnym.
2. Wybierz **zarządzane ról uprzywilejowanych** > **ustawienia** > **ustawienia alertów**.
   
    ![Przejdź do ustawień alertów zabezpieczeń](./media/pim-how-to-configure-security-alerts/PIM_security_settings.png)

### <a name="roles-are-being-activated-too-frequently-alert"></a>Alert "Role są aktywowane zbyt często"
Ten alert jest wyzwalana, gdy użytkownik aktywuje tej samej roli uprzywilejowanej wiele razy w danym okresie. Można skonfigurować zarówno okres czasu, jak i liczbę aktywacji.

* **Okres odnowienia uaktywnienia**: Określ w dni, godziny, minuty i sekundy okres czasu, którego chcesz użyć do śledzenia podejrzanych odnowienia.
* **Liczba odnowień aktywacji**: Określ liczbę aktywacji od 2 do 100, które uważasz za Alberta alertu w przedziale czasu została wybrana opcja. Możesz zmienić to ustawienie za pomocą suwaka lub wpisanie liczby w polu tekstowym.

### <a name="there-are-too-many-global-administrators-alert"></a>Alert "Istnieją zbyt wielu administratorów globalnych"
PIM wyzwala alert, jeśli są spełnione dwa różne kryteria, a następnie możesz skonfigurować oba z nich. Najpierw należy do osiągnięcia progu administratorów globalnych. Po drugie procent swoje przypisania roli całkowita musi być administratorów globalnych. Jeśli tylko spełniać jeden z tych pomiarów, alert nie jest widoczna.  

* **Minimalna liczba administratorów globalnych**: liczba administratorów globalnych, od 2 do 100, należy rozważyć niebezpieczne kwoty.
* **Procent administratorów globalnych**: Określ wartość procentową administratorów, którzy Administratorzy globalni, od 0% do 100%, to niebezpieczne w danym środowisku.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Alert "Administratorzy nie są za pomocą ich ról uprzywilejowanych"
Ten alert wyzwalacze, jeśli użytkownik przejdzie określoną ilość czasu, bez uaktywniania roli.

* **Liczba dni**: Określ liczbę dni z zakresu od 0 do 100, które użytkownik może go bez uaktywniania roli.

## <a name="next-steps"></a>Kolejne kroki

- [Konfigurowanie ustawień roli w katalogu usługi Azure AD w usłudze PIM](pim-how-to-change-default-settings.md)
