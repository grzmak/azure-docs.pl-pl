---
title: Zarządzanie tożsamościami w usłudze Azure AD | Dokumentacja firmy Microsoft
description: Zarządzanie tożsamościami w usłudze Azure AD umożliwia zrównoważenia organizacji na potrzeby zabezpieczeń i pracownikom produktywność dzięki odpowiednie procesy i widoczności.
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
ms.date: 09/25/2018
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.openlocfilehash: 20b1c8673bfdb3b2207ed63749f79539c396642c
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167961"
---
# <a name="what-is-azure-ad-identity-governance"></a>Co to jest Zarządzanie tożsamościami usługi Azure AD?

Zarządzanie tożsamościami w usłudze Azure Active Directory (Azure AD) umożliwia zrównoważenia organizacji na potrzeby zabezpieczeń i pracownikom produktywność dzięki odpowiednie procesy i widoczności. Udostępnia możliwości, aby upewnić się, że odpowiednie użytkownicy mają prawo dostępu do odpowiednich zasobów i pozwala na ochronę, monitorować i sprawdzać uzyskać dostęp do krytycznych zasobów--przy jednoczesnym zapewnieniu produktywność pracowników.  

Zarządzanie tożsamościami oraz zapewnić organizacjom możliwość różnych pracowników, partnerów biznesowych oraz dostawców i usługach i aplikacjach, wykonaj następujące czynności:

- Zarządzanie cyklem życia tożsamości
- Zarządzanie cyklem życia dostępu
- Bezpieczne administrowanie

W szczególności jest przeznaczony ma pomóc organizacjom rozwiązać te cztery kluczowe pytania:

- Użytkowników, którzy powinni mieć dostęp do określonych zasobów?
- Co robią tych użytkowników z tym dostępem?
- Czy istnieją efektywne procedury nadzoru organizacji w celu zarządzania dostępem?
- Audytorzy sprawdzić, czy formanty działają?

## <a name="identity-lifecycle"></a>Cykl życia tożsamości

Zarządzanie tożsamościami oraz pomaga organizacjom osiągać równowagę między *produktywność* — jak szybko można osoby mają dostęp do zasobów potrzebują, np. gdy zostają dołączone w mojej organizacji? I *zabezpieczeń* — jak należy ich dostęp zmienić wraz z upływem czasu, takich jak z powodu zmian na status zatrudnienia osoby?  Zarządzanie cyklem życia tożsamości jest podstawą dla zarządzania tożsamościami i skuteczne nadzór w odpowiedniej skali wymaga modernizacji infrastruktury zarządzania cyklem życia tożsamości dla aplikacji.

W przypadku wielu organizacji cyklem życia tożsamości dla pracowników jest powiązany do reprezentacji tego użytkownika w systemie HCM (Zarządzanie kapitałem ludzkim).  Usługa Azure AD Premium również automatycznie obsługuje tożsamości użytkowników dla osób reprezentowane w produkcie Workday w usługi Active Directory i Azure Active Directory, zgodnie z opisem w [Workday samouczek provisioning (wersja zapoznawcza) dla ruchu przychodzącego](../saas-apps/workday-inbound-tutorial.md).  Usługa Azure AD Premium obejmuje również [programu Microsoft Identity Manager](/microsoft-identity-manager/), który można zaimportować rekordy z lokalnych systemów HCM, takich jak SAP, Oracle eBusiness i Oracle PeopleSoft.

Coraz większym stopniu scenariusze wymagają współpracy z osobami spoza organizacji. [Usługa Azure AD B2B](/azure/active-directory/b2b/) współpracy pozwala na bezpieczne udostępnianie aplikacji i usług Twojej organizacji za pomocą użytkowników-gości i partnerom zewnętrznym w każdej organizacji, przy zachowaniu kontroli nad danych firmowych.

## <a name="access-lifecycle"></a>Cykl życia dostępu

Organizacje wymagają procesu do zarządzania dostępem poza co początkowo udostępniony dla użytkownika podczas tworzenia tej tożsamości użytkownika.  Ponadto przedsiębiorstwa to organizacje muszą mieć możliwość skalowania efektywnie, aby można było tworzenie i wymuszanie zasad dostępu i kontroli w sposób ciągły.

Zazwyczaj IT delegatów dostępu zatwierdzeniu podjęte decyzje w kwestii osobom podejmującym decyzje biznesowe.  Ponadto IT może obejmować samych użytkowników.  Na przykład trzeba znać zasady firmy użytkowników uzyskujących dostęp do danych poufnych klientów w aplikacji marketingowych firmy w Europie. Użytkownicy-goście mogą jest niebranie pod uwagę wymagania dotyczące przetwarzania danych w organizacji, do której zostali zaproszeni.

Organizacje takie jak zautomatyzować proces cyklu życia dostępu za pomocą technologii [grup dynamicznych](../users-groups-roles/groups-dynamic-membership.md), powiązanych z aprowizacji użytkowników do [aplikacji SaaS](../saas-apps/tutorial-list.md) lub [aplikacje zintegrować przy użyciu SCIM](../manage-apps/use-scim-to-provision-users-and-groups.md).  Organizacje mogą również kontrolować, które [użytkownicy-goście mają dostęp do aplikacji lokalnych](../b2b/hybrid-cloud-to-on-premises.md).  Te mogą praw dostępu, a następnie być regularnie analizowane za pomocą cykliczne [przeglądy dostępu w usłudze Azure AD](access-reviews-overview.md).

Gdy użytkownik próbuje uzyskać dostęp do aplikacji, Azure AD wymusza [dostępu warunkowego](/azure/active-directory/conditional-access/) zasad. Na przykład zasady dostępu warunkowego obejmują wyświetlanie [warunki użytkowania](active-directory-tou.md) i [zapewnienie użytkownik zgodził się na te warunki](../conditional-access/require-tou.md) przed uzyskaniem dostępu do aplikacji.

## <a name="privileged-access-lifecycle"></a>Cykl życia uprzywilejowanego dostępu

W przeszłości uprzywilejowanego dostępu został opisany przez innych dostawców jako osobne możliwości od zarządzania tożsamościami. Jednak w firmie Microsoft uważamy regulujące uprzywilejowanego dostępu jest kluczowa część pakietu zarządzania tożsamościami oraz — szczególnie podane potencjał pod kątem nieprawidłowego użycia skojarzone z tych uprawnień może spowodować organizacji administratora. Pracownicy, dostawcy i wykonawców, które przejąć prawa administracyjne muszą podlegać.

Usługa Azure AD Privileged Identity Management (PIM) zapewnia dodatkową kontrolę, które dostosowane do zabezpieczania dostępu prawa do zasobów, w usłudze Azure AD platformy Azure i innych Microsoft Online Services.  Dostęp just in time, a zmiana roli alerty funkcjami oferowanymi przez usługi Azure AD PIM, oprócz uwierzytelniania wieloskładnikowego i dostęp warunkowy, zapewnić kompleksowy zestaw formantów nadzoru, aby ułatwić bezpiecznych zasobów firmy (katalog, Usługi Office 365, a role zasobów platformy Azure). Podobnie jak w przypadku innych form dostępu organizacje mogą używać przeglądów dostępu, aby skonfigurować cyklicznego wystawiły dostępu dla wszystkich użytkowników w rolach administratora.

## <a name="getting-started"></a>Wprowadzenie

Następujące konfiguracje oferują przewodnika po to, jakie zasady linii bazowej, firma Microsoft zaleca, aby zapewnić większe bezpieczeństwo i produktywność pracowników, gdy nie ma znaleźć idealne rozwiązanie lub zalecenie dla każdego klienta.

- [Konfiguracje dostępu tożsamości i urządzenia](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Zabezpieczanie dostępu uprzywilejowanego](../users-groups-roles/directory-admin-roles-secure.md)


### <a name="access-reviews"></a>Przeglądy dostępu

- [Co to jest przeglądu dostępu?](access-reviews-overview.md)
- [Zarządzanie dostępem użytkowników za pomocą przeglądów dostępu](manage-user-access-with-access-reviews.md)
- [Zarządzanie dostępem gości za pomocą przeglądów dostępu](manage-guest-access-with-access-reviews.md)
- [Uruchamianie przeglądu dostępu z rolą katalogu](../privileged-identity-management/pim-how-to-start-security-review.md)

### <a name="terms-of-use"></a>Warunki użytkowania

- [Co można zrobić z warunkami użytkowania?](active-directory-tou.md)

### <a name="privileged-identity-management"></a>Usługa Privileged identity management

- [Co to jest usługa Azure AD PIM?](../privileged-identity-management/pim-configure.md)
