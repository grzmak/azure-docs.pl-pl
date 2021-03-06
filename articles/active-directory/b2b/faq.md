---
title: Współpraca B2B usługi Active Directory Azure — często zadawane pytania | Dokumentacja firmy Microsoft
description: Uzyskaj odpowiedzi na często zadawane pytania dotyczące współpracy B2B usługi Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: reference
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b4407ac6b7a0d9fdbf52b84fb94223c32868f0c5
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983856"
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Współpraca B2B usługi Active Directory Azure — często zadawane pytania

Te często zadawane pytania (FAQ) dotyczące współpracy w usłudze Azure Active Directory (Azure AD) business-to-business (B2B) są okresowo aktualizowane, aby uwzględnić nowe tematy.

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Dlatego jest bardziej intuicyjne dla naszych użytkowników gości współpracy B2B możemy dostosować naszą stronę logowania?
Oczywiście! Zobacz nasze [wpis w blogu o tej funkcji](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Aby uzyskać więcej informacji o tym, jak dostosować stronę logowania Twojej organizacji, zobacz [dodać znakowanie firmowe do Zaloguj się i strony panelu dostępu](../fundamentals/customize-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Użytkowników we współpracy B2B dostęp do usługi SharePoint Online i OneDrive?
Tak. Jednak jest możliwość wyszukiwania dla istniejących użytkowników-gości w usłudze SharePoint Online przy użyciu selektora osób **poza** domyślnie. Aby włączyć opcję, aby wyszukać istniejących użytkowników-gości, ustaw **ShowPeoplePickerSuggestionsForGuestUsers** do **na**. Możesz włączyć następujące ustawienie na poziomie dzierżawy lub na poziomie zbioru witryn. To ustawienie można zmienić za pomocą polecenia cmdlet Set-SPOTenant i SPOSite zestawu. Z tych poleceń cmdlet elementy członkowskie można wyszukiwać wszystkich istniejących użytkowników-gości w katalogu. Zmiany w zakresie dzierżawy nie wpływają na witryn usługi SharePoint Online, które już zostały udostępnione.

### <a name="is-the-csv-upload-feature-still-supported"></a>Jest funkcja przekazywania pliku CSV są nadal obsługiwane?
Tak. Aby uzyskać więcej informacji o korzystaniu z funkcji przekazywania plików CSV, zobacz [tego przykładu z programu PowerShell](code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Jak dostosować wiadomości e-mail z zaproszeniem
Prawie wszystko, co o procesie zapraszającej można dostosować za pomocą [zaproszenie B2B interfejsów API](customize-invitation-api.md).

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Użytkownicy-goście, można zresetować swoje metody uwierzytelniania wieloskładnikowego?
Tak. Użytkownicy-goście mogą resetować swoje metody uwierzytelniania wieloskładnikowego tak samo jak regularne użytkowników.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Której organizacji jest odpowiedzialny za licencje na usługę uwierzytelnianie wieloskładnikowe?
Organizacji zapraszającej jest przeprowadzane uwierzytelnianie wieloskładnikowe. Organizacji zapraszającej musisz upewnić się, że organizacja charakteryzuje się wystarczającą liczbę licencji dla użytkowników B2B, którzy korzystają z uwierzytelniania Multi-Factor Authentication.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Co zrobić, jeśli w organizacji partnera, który jest już ma skonfigurować uwierzytelnianie wieloskładnikowe? Można możemy zaufania uwierzytelniania wieloskładnikowego i nie używać własną usługę uwierzytelnianie wieloskładnikowe?
Ta funkcja jest planowana w przyszłej wersji, tak, po czym można wybrać określone partnerów do wykluczenia z usługi uwierzytelnianie wieloskładnikowe (zapraszający organizacji).

### <a name="how-can-i-use-delayed-invitations"></a>Jak używać opóźnione zaproszenia?
Organizacja chcieć dodają użytkowników we współpracy B2B, udostępnić je do aplikacji, zgodnie z potrzebami, a następnie Wyślij zaproszenia. Zaproszenie współpracy B2B interfejs API umożliwia dostosowywanie przepływu pracy przy dołączaniu.

### <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Czy mogę utworzyć użytkowników-gości widoczne w globalnej listy adresowej Exchange?
Tak. Domyślnie obiekty gościa nie są widoczne w Twojej organizacji globalnej liście adresowej, ale można użyć programu PowerShell usługi Azure Active Directory, aby stały się widoczne. Aby uzyskać więcej informacji, zobacz **mogę sprawdzić, że obiekty gościa widoczna na globalnej liście adresowej?** w [dostęp gościa w grup usługi Office 365](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ).

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Można utworzyć użytkownika-gościa ograniczony administrator?
Naturalnie. Aby uzyskać więcej informacji, zobacz [dodawania użytkowników-gości do roli](add-guest-to-role.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Współpraca B2B w usłudze Azure AD pozwala użytkownikom B2B dostępu witryny Azure portal?
Chyba że użytkownik przypisany do roli ograniczony administrator lub administrator globalny, użytkowników we współpracy B2B nie będzie wymagać dostępu do witryny Azure portal. Jednak użytkowników we współpracy B2B, którzy mają przypisaną rolę ograniczony administrator lub administrator globalny może uzyskać dostęp do portalu. Ponadto jeśli użytkownika gościa, który nie jest przypisany jeden z tych ról Administrator uzyskuje dostęp do portalu, użytkownik może być mogli korzystać z niektórych części środowiska. Rola użytkownika gościa ma niektóre uprawnienia w katalogu.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Czy można zablokować dostęp do witryny Azure portal dla użytkowników-gości?
Tak! Po skonfigurowaniu tych zasad, należy zachować ostrożność uniknąć przypadkowego blokowanie dostępu do członków i administratorów.
Aby zablokować dostęp użytkownika-gościa do [witryny Azure portal](https://portal.azure.com), użyj zasad dostępu warunkowego w interfejsie API modelu klasycznym wdrożeniu platformy Windows Azure:
1. Modyfikowanie **wszyscy użytkownicy** grupy tak, aby zawierała tylko elementy członkowskie.
  ![Modyfikowanie grupy zrzutu ekranu](media/faq/modify-all-users-group.png)
2. Utwórz grupę dynamiczną, która zawiera użytkowników-gości.
  ![Utwórz zrzut ekranu z grupy](media/faq/group-with-guest-users.png)
3. Skonfigurowanie zasad dostępu warunkowego do Blokuj użytkowników-gości z dostęp do portalu, jak pokazano w poniższym klipie wideo:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Współpraca B2B w usłudze Azure AD obsługuje uwierzytelnianie wieloskładnikowe i konta poczty e-mail odbiorcy?
Tak. Oba konta e-mail uwierzytelniania i konsumentów usługi Multi-Factor Authentication są obsługiwane dla współpracy B2B usługi Azure AD.

### <a name="do-you-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Czy planowane jest do obsługi resetowania haseł dla użytkowników współpracy B2B usługi Azure AD?
Tak. Poniżej przedstawiono istotne szczegóły dotyczące samoobsługowego resetowania haseł (SSPR) dla użytkownika B2B, kto otrzymał zaproszenie z organizacji partnerskiej:
 
* Samoobsługowe Resetowanie HASEŁ występuje tylko w dzierżawę tożsamości użytkownika B2B.
* Jeśli dzierżawa tożsamości konta Microsoft, jest używany na koncie Microsoft mechanizm samoobsługowego resetowania HASEŁ.
* Jeśli dzierżawa tożsamości jest just-in-time (JIT) lub "wirusowego" dzierżawy, jest wysyłany pocztą e-mail resetowania hasła.
* W przypadku innych dzierżaw następnie standardowego procesu samoobsługowego resetowania HASEŁ dla użytkowników B2B. Element członkowski funkcji samoobsługowego resetowania HASEŁ dla użytkowników B2B, w ramach zasobu, np. dzierżawców jest zablokowany. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Jest resetowania hasła dostępne dla użytkowników-gości w just-in-time (JIT) lub "wirusowego" dzierżawy, którzy zaakceptować zaproszenia mających lub adres e-mail szkoły, ale który nie ma istniejącego konta usługi Azure AD?
Tak. Można wysłać poczty resetowania hasła, która umożliwia użytkownikowi zresetowanie hasła w dzierżawy JIT.

### <a name="does-microsoft-dynamics-365-provide-online-support-for-azure-ad-b2b-collaboration"></a>Microsoft Dynamics 365 oferuje pomocy online do współpracy B2B usługi Azure AD?
Tak, Dynamics 365 (online) zapewnia obsługę współpracy B2B usługi Azure AD. Aby uzyskać więcej informacji, zobacz artykuł Dynamics 365 [zaprosić użytkowników przy użyciu funkcji współpracy B2B usługi Azure AD](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration).

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Co to jest okres istnienia początkowe hasło dla nowo utworzonego użytkownika współpracy B2B?
Usługa Azure AD ma stały zestaw znaków, siły hasła i konta wymagania blokady, które stosuje się jednakowo do wszystkich usługi Azure AD w chmurze kont użytkowników. Konta użytkowników w chmurze są kontami, które nie są Sfederowane za pomocą innego dostawcy tożsamości, na przykład 
* Konto Microsoft
* Facebook
* Usługi federacyjne Active Directory
* Innej dzierżawy w chmurze (na potrzeby współpracy B2B)

W przypadku kont federacyjnych zasady haseł zależy od zasad, które są stosowane w dzierżawy w środowisku lokalnym i ustawienia konta Microsoft użytkownika.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Organizacja może być wskazane różnych doświadczeń w swoich aplikacjach dla użytkowników dzierżawy i użytkowników-gości. Czy istnieje standardowa wskazówki dotyczące to? Jest obecność dostawcy tożsamości oświadczeń poprawny model używany?
 Użytkownik-Gość można użyć dowolnego dostawcy tożsamości do uwierzytelniania. Aby uzyskać więcej informacji, zobacz [właściwości użytkownika współpracy B2B](user-properties.md). Użyj **UserType** właściwości w celu określenia środowisko użytkownika. **UserType** oświadczeń jest obecnie niedostępna w tokenie. Aplikacje powinny używać interfejsu API programu Graph do wysyłania zapytań o katalog dla użytkownika i UserType.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>Gdzie mogę znaleźć udostępnianie rozwiązań i przesyłaj pomysły społeczność współpracy B2B?
Stale słuchamy opinii, aby poprawić współpracę B2B. Zachęcamy do udostępniania użytkownikowi scenariuszy, najlepsze rozwiązania i co Ci się podoba współpracy B2B usługi Azure AD. Dołącz do dyskusji w [społeczności technicznej firmy Microsoft](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Zachęcamy także do przesyłania Twoje pomysły i głosuj na przyszłych funkcji w [pomysły współpracy B2B](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Tak, aby użytkownik po prostu "gotowe" możemy wysłać zaproszenie, który jest automatycznie zrealizowany? Lub użytkownik zawsze należy kliknąć na adres URL realizacji?
Osoba zapraszająca mogą zapraszać innych użytkowników w organizacji partnerskiej odpowiedzialnej za pomocą interfejsu użytkownika, skryptów programu PowerShell lub interfejsów API. Następnie zapraszającej można wysłać użytkownik-Gość bezpośredni link do udostępnionej aplikacji. W większości przypadków jest już trzeba otworzyć wiadomości e-mail z zaproszeniem i kliknij adres URL realizacji. Aby uzyskać więcej informacji, zobacz [realizacja zaproszenia współpracy B2B usługi Azure Active Directory](redemption-experience.md).

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Współpraca B2B działanie partnerów zaproszonych korzysta federacyjnych można dodać własny mechanizm uwierzytelniania w środowisku lokalnym?
Jeśli partner ma dzierżawę usługi Azure AD, sfederowaną infrastruktury uwierzytelniania w środowisku lokalnym, lokalne logowanie jednokrotne (SSO) automatycznie uzyskuje się. Jeśli partner nie ma dzierżawę usługi Azure AD, zostanie utworzone konto usługi Azure AD dla nowych użytkowników. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Po próbie B2B usługi Azure AD nie zaakceptował gmail.com i outlook.com adresy e-mail i że B2C został użyty podczas tych rodzajów kont?
Zostaną usunięte z różnicami B2B i firma klient (B2C) współpracy, zgodnie z którą tożsamości są obsługiwane. Tożsamość używana jest powód, dla wybór między używaniem B2B przy użyciu usługi B2C. Aby dowiedzieć się, jak wybranie opcji usługi współpracy, zobacz [współpracy porównania B2B i B2C w usłudze Azure Active Directory](compare-with-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Jakie aplikacje i usługi obsługują użytkowników-gości B2B w usłudze Azure?
Wszystkimi aplikacjami platformy Azure zintegrowanych z usługą AD obsługuje użytkowników-gości B2B w usłudze Azure. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Nasi partnerzy braku uwierzytelniania wieloskładnikowego możemy wymusić uwierzytelnianie wieloskładnikowe dla użytkowników-gości B2B?
Tak. Aby uzyskać więcej informacji, zobacz [dostęp warunkowy dla użytkowników współpracy B2B](conditional-access.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>W programie SharePoint można zdefiniować listę "Zezwalaj" lub "odmowa" dla użytkowników zewnętrznych. Możemy to zrobić na platformie Azure?
Tak. Platforma Azure obsługuje współpracy B2B usługi AD list zezwalania i odmowy dla. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Jakie licencje musimy użyć usługi Azure AD B2B?
Aby uzyskać informacje o licencjach, aby Twoja organizacja musi korzystać z usługi Azure AD B2B, zobacz [współpracy B2B usługi Azure Active Directory, wskazówki dotyczące licencjonowania](licensing-guidance.md).

### <a name="next-steps"></a>Kolejne kroki

- [Czym jest współpraca B2B w usłudze Azure AD?](what-is-b2b.md)

