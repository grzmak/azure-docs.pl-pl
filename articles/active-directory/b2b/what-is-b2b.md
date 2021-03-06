---
title: Co to jest współpraca w usłudze Azure Active Directory B2B? | Microsoft Docs
description: Model współpracy w usłudze Azure Active Directory B2B przyznaje prawa dostępu użytkownika-gościa, aby móc bezpiecznie udostępniać zasoby i współpracować z partnerami zewnętrznymi.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: overview
ms.date: 09/14/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: 609f48d0c2ba41d658850ade367d9b225941b105
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982612"
---
# <a name="what-is-guest-user-access-in-azure-active-directory-b2b"></a>Co to jest dostęp użytkowników-gości w usłudze Azure Active Directory B2B?

Model współpracy między firmami (B2B) w usłudze Azure Active Directory (Azure AD) umożliwia bezpieczne udostępnianie aplikacji i usług firmy użytkownikom-gościom z dowolnej organizacji, zachowując kontrolę nad własnymi danymi firmowymi. Pracuj bezpiecznie z partnerami zewnętrznymi, dużymi lub małymi, nawet jeśli nie korzystają oni z usługi Azure AD lub nie mają działu IT. Prosty proces zapraszania i realizacji umożliwia partnerom uzyskiwanie dostępu do zasobów firmy przy użyciu własnych poświadczeń. Deweloperzy mogą używać międzyfirmowych interfejsów API usługi Azure AD do dostosowywania procesu zapraszania lub pisania aplikacji, takich jak samoobsługowe portale tworzenia kont.

Obejrzyj film wideo, aby dowiedzieć się, jak bezpiecznie współpracować z użytkownikami-gośćmi, zapraszając ich do logowania do aplikacji i usług firmy przy użyciu ich własnych tożsamości.

Poniższy film oferuje przydatne omówienie tej możliwości.

>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="collaborate-with-any-partner-using-their-identities"></a>Współpraca z dowolnym partnerem z użyciem jego tożsamości
Dzięki usłudze Azure AD B2B partner używa własnego rozwiązania do zarządzania tożsamościami, dlatego nie ma żadnych zewnętrznych kosztów administracyjnych dla organizacji. 
- Partner korzysta z własnych tożsamości i poświadczeń; usługa Azure AD nie jest wymagana. 
- Nie trzeba zarządzać zewnętrznymi kontami lub hasłami. 
- Nie trzeba zsynchronizować konta ani zarządzać cyklami życia kont.  

![dodawanie członka](media/what-is-b2b/add-member.png)

## <a name="invite-guest-users-with-a-simple-invitation-and-redemption-process"></a>Zapraszanie użytkowników-gości za pomocą prostego procesu zapraszania i realizacji
Użytkownicy-goście logują się do aplikacji i usług za pomocą własnych tożsamości służbowych lub społecznościowych. Jeśli użytkownik-gość nie ma konta Microsoft lub konta usługi Azure AD, zostanie ono utworzone, gdy użytkownik zrealizuje zaproszenie. 
- Zaproś użytkowników-gości przy użyciu wybranych przez nich tożsamości poczty e-mail.
- Wyślij bezpośredni link do aplikacji lub wyślij zaproszenie do własnego portalu dostępu użytkownika-gościa. 
- Użytkownicy-goście wykonują kilka prostych kroków w celu realizacji.

![panel dostępu](media/what-is-b2b/consentscreen.png)

## <a name="use-policies-to-securely-share-your-apps-and-services"></a>Bezpieczne udostępnianie aplikacji i usług przy użyciu zasad
Zasady autoryzacji umożliwiają ochronę zawartości firmowej. Zasady dostępu warunkowego, takie jak uwierzytelnianie wieloskładnikowe, można wymuszać:
- Na poziomie dzierżawy.
- Na poziomie aplikacji.
- W przypadku określonych gości w celu ochrony aplikacji i danych firmowych.

![dodawanie użytkowników-gości](media/what-is-b2b/tutorial-mfa-policy-2.png)


## <a name="easily-add-guest-users-in-the-azure-ad-portal"></a>Łatwe dodawanie użytkowników-gości w portalu usługi Azure AD

Jako administrator możesz łatwo dodawać użytkowników-gości do organizacji w witrynie Azure Portal.
- Utwórz nowego użytkownika-gościa w usłudze Azure AD podobnie jak w przypadku dodawania nowego użytkownika.
- Użytkownik-gość natychmiast otrzymuje możliwe do dostosowania zaproszenie, które pozwala mu na zalogowanie się do panelu dostępu.
- Użytkowników-gości w katalogu można przypisywać do aplikacji lub grup.  

![dodawanie użytkowników-gości](media/what-is-b2b/adding-b2b-users-admin.png)

## <a name="let-application-and-group-owners-manage-their-own-guest-users"></a>Zezwalanie właścicielom aplikacji i grup na zarządzanie własnymi użytkownikami-gośćmi

Zarządzanie użytkownikami-gośćmi można delegować do właścicieli aplikacji, aby mogli oni bezpośrednio dodawać użytkowników-gości do dowolnej aplikacji firmy Microsoft lub innej firmy. 
 - Administratorzy konfigurują samoobsługowe zarządzanie aplikacjami i grupami.
 - Użytkownicy inni niż administratorzy używają własnego [panelu dostępu](https://myapps.microsoft.com) w celu dodania użytkowników-gości do aplikacji lub grup.

![dodawanie użytkowników-gości](media/what-is-b2b/access-panel-manage-app.png)

## <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Używanie interfejsów API i przykładowego kodu do prostego tworzenia aplikacji do dodania

Zaproś partnerów zewnętrznych do współpracy w sposób dostosowany do potrzeb swojej organizacji.
- Skorzystaj z [interfejsów API zapraszania do współpracy w usłudze B2B](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) w celu dostosowywania środowisk dodawania, w tym budowania samoobsługowych portali tworzenia kont. 
- Skorzystaj z przykładowego kodu, który udostępniamy na potrzeby portalu samoobsługowego [w serwisie Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![portal tworzenia konta](media/what-is-b2b/sign-up-portal.png)

## <a name="next-steps"></a>Następne kroki

- [Licensing guidance for Azure AD B2B collaboration (Wskazówki dotyczące licencjonowania w ramach współpracy B2B w usłudze Azure AD)](licensing-guidance.md)
- [Add B2B collaboration guest users in the portal (Dodawanie użytkowników-gości współpracy B2B w portalu)](add-users-administrator.md)
- [Understand the invitation redemption process (Opis procesu realizacji zaproszenia)](redemption-experience.md)
- Jak zawsze możesz skontaktować się z zespołem ds. produktu, aby podzielić się opiniami, problemami i sugestiami za pośrednictwem naszej [społeczności technicznej firmy Microsoft](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).