---
title: Luki w zabezpieczeniach wykryte przez usługi Azure Active Directory Identity Protection | Dokumentacja firmy Microsoft
description: Omówienie luk w zabezpieczeniach wykryte przez usługi Azure Active Directory Identity Protection.
services: active-directory
keywords: Usługa Azure active directory identity protection rozwiązania cloud discovery, zarządzanie aplikacji, zabezpieczenia, ryzyka, poziom ryzyka, luk w zabezpieczeniach, zasady zabezpieczeń
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 680e52fefd8256b3ac270e8d721f27645ced49eb
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40004264"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Luki w zabezpieczeniach wykryte przez usługi Azure Active Directory Identity Protection
Luki w zabezpieczeniach występują luki w środowisku, które może być wykorzystane przez osobę atakującą. Firma Microsoft zaleca tych luk w celu zwiększenia poziomu bezpieczeństwa organizacji i uniemożliwić osobom atakującym ich wykorzystania.


![luki w zabezpieczeniach](./media/vulnerabilities/101.png "luk w zabezpieczeniach")



Poniższe sekcje zawierają omówienie usterki zgłoszone przez ochronę tożsamości.

## <a name="multi-factor-authentication-registration-not-configured"></a>Rejestracja korzystająca z uwierzytelniania wieloskładnikowego nie jest skonfigurowana
Tę lukę w zabezpieczeniach ułatwia kontrolowanie wdrażania usługi Azure Multi-Factor Authentication w Twojej organizacji. 

Usługa Azure Multi-Factor authentication udostępnia drugą warstwę zabezpieczeń do uwierzytelniania użytkowników. Jego pomaga w zabezpieczeniu dostępu do danych i aplikacji, a jednocześnie spełnia wymagania użytkowników dotyczące prostoty procesu logowania. Zapewnia ona silne uwierzytelnianie szerokiemu zakresowi prostych opcji weryfikacji — połączenie telefoniczne, wiadomość SMS lub aplikacja mobilna powiadomień lub weryfikacji kodu i innych firm tokenów OATH.

Firma Microsoft zaleca, wymagają usługi Azure Multi-Factor Authentication do logowania użytkownika. Uwierzytelnianie wieloskładnikowe odgrywa kluczową rolę w zasadach dostępu warunkowego na podstawie ryzyka dostępne za pośrednictwem Identity Protection.

Aby uzyskać więcej informacji, zobacz [co to jest uwierzytelnianie wieloskładnikowe systemu Azure?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Niezarządzane aplikacje w chmurze
Tę lukę w zabezpieczeniach pomaga zidentyfikować niezarządzane aplikacje w chmurze w Twojej organizacji.

W nowoczesnych przedsiębiorstwach działom IT są często nie wszystkie aplikacje, które korzystają użytkownicy w organizacji do wykonania swojej pracy w chmurze. To proste zobaczyć, dlaczego administratorzy będą niepokoją nieautoryzowanego dostępu do danych firmowych, wyciekom danych i innych zagrożeń bezpieczeństwa. 

Firma Microsoft zaleca, aby wdrożyć rozwiązania Cloud Discovery odnajdywania aplikacji w chmurze niezarządzanych i zarządzanie te aplikacje przy użyciu usługi Azure Active Directory.

Aby uzyskać więcej informacji, zobacz [rozwiązania Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Alerty zabezpieczeń z usługi Privileged Identity Management
Tę lukę w zabezpieczeniach pomaga użytkownikom odnajdywanie ich i rozwiązywanie alertów dotyczących tożsamości uprzywilejowanych w Twojej organizacji.  

Aby użytkownicy mogli wykonywać operacje uprzywilejowane, organizacje muszą udzielić użytkownikom tymczasowy lub stały uprzywilejowanego dostępu w usłudze Azure AD, zasobów platformy Azure lub usługi Office 365 lub innych aplikacji SaaS. Każda z tych użytkowników uprzywilejowanych zwiększa obszar narażony na Twojej organizacji. Tę lukę w zabezpieczeniach ułatwia identyfikowanie użytkowników z dostępem uprzywilejowanym niepotrzebne i podjąć odpowiednie działania w celu zmniejszenia lub wyeliminowania ryzyka, które stanowią one. 

Zaleca się, że Twoja organizacja używa usługi Azure AD Privileged Identity Management do zarządzanie, sterowanie i monitorowanie uprzywilejowanych tożsamości oraz ich dostępu do zasobów w usłudze Azure AD, a także innych usług online firmy Microsoft, takich jak usługi Office 365 lub Microsoft Intune.

Aby uzyskać więcej informacji, zobacz [usługi Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md). 

## <a name="see-also"></a>Zobacz także

[Usługa Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

