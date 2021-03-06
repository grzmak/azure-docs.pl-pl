---
title: Uwierzytelnianie w usłudze Azure Active Directory
description: Dowiedz się więcej o uwierzytelnianiu w usłudze Azure Active Directory (Azure AD), modelu aplikacji, interfejsie API, aprowizowaniu i najczęstszych scenariuszach uwierzytelniania obsługiwanych przez usługę Azure AD.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, sureshja, hirsin
ms.custom: aaddev
ms.openlocfilehash: f541212cd549cde21bc3b2048bf633c57a911653
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967731"
---
# <a name="what-is-authentication"></a>Co to jest uwierzytelnianie?

*Uwierzytelnianie* to wezwanie strony do podania wiarygodnych poświadczeń. Stanowi to podstawę utworzenia podmiotu zabezpieczeń używanego do obsługi tożsamości i kontroli dostępu. Mówiąc prościej, jest to proces udowadniania, że jest się tym, za kogo się podaje. Uwierzytelnianie jest czasami określane terminem AuthN.

*Autoryzacja* to przyznanie uprawnienia uwierzytelnionemu podmiotowi zabezpieczeń do wykonania jakiegoś zadania. Autoryzacja określa, do jakich danych można uzyskać dostęp oraz jakie zadania można na nich wykonać. Autoryzacja jest czasami określana terminem AuthZ.

Usługa Azure Active Directory (Azure AD) upraszcza uwierzytelnianie dla deweloperów aplikacji, zapewniając tożsamość jako usługę z obsługą zgodnych ze standardami branżowymi protokołów, takich jak OAuth 2.0 i OpenID Connect, a także biblioteki typu open-source dla różnych platform, które ułatwiają szybkie rozpoczęcie pisania kodu.

Istnieją dwa główne przypadki użycia w modelu programowania w usłudze Azure AD:

* Podczas przepływu autoryzacji OAuth 2.0 — gdy właściciel zasobu przyznaje autoryzację do aplikacji klienckiej, co umożliwia klientowi uzyskanie dostępu do zasobów właściciela zasobu.
* Podczas dostępu do zasobu przez klienta — zgodnie z implementacją serwera zasobów, przy użyciu wartości oświadczeń obecnych w tokenie dostępu na potrzeby podejmowania na ich podstawie decyzji dotyczących kontroli dostępu.

## <a name="authentication-basics-in-azure-active-directory"></a>Podstawy uwierzytelniania w usłudze Azure Active Directory

Rozpatrzymy tutaj najprostszy scenariusz, w którym wymagana jest tożsamość: użytkownik w przeglądarce internetowej musi się uwierzytelnić w aplikacji internetowej. Na poniższym diagramie przedstawiono ten scenariusz:

![Przegląd logowania do aplikacji internetowej](./media/authentication-scenarios/basics_of_auth_in_aad.png)

Poniżej opisano poszczególne składniki wyświetlane na diagramie:

* Usługa Azure AD jest dostawcą tożsamości. Dostawca tożsamości jest odpowiedzialny za weryfikację tożsamości użytkowników i aplikacji, które istnieją w katalogu organizacji. Wystawia on także tokeny zabezpieczające po pomyślnym uwierzytelnieniu tych użytkowników i aplikacji.
* Aplikacja, która chce oddelegować uwierzytelnianie do usługi Azure AD, musi być zarejestrowana w usłudze Azure AD. Usługa Azure AD rejestruje i unikatowo identyfikuje aplikację w katalogu.
* Deweloperzy mogą używać bibliotek uwierzytelniania typu open-source w usłudze Azure AD, aby uprościć proces uwierzytelniania dzięki automatycznej obsłudze szczegółów dotyczących protokołu. Aby uzyskać więcej informacji, zobacz tematy dotyczące [bibliotek uwierzytelniania w wersji 2.0](reference-v2-libraries.md) i [bibliotek uwierzytelniania w wersji 1.0](active-directory-authentication-libraries.md) usługi Azure AD.
* Po uwierzytelnieniu użytkownika aplikacja musi zweryfikować jego token zabezpieczający, aby się upewnić, że uwierzytelnianie zakończyło się pomyślnie. Procesy zachodzące w aplikacji zostały opisane w różnych przewodnikach Szybki start, samouczkach i przykładach kodu w różnych językach i strukturach.
  * Aby szybko utworzyć aplikację i dodać do niej funkcje, takie jak między innymi uzyskiwanie tokenów, odświeżanie tokenów, logowanie użytkownika i wyświetlanie informacji o użytkowniku, zapoznaj się z sekcją **Przewodniki Szybki start** w dokumentacji.
  * Aby uzyskać szczegółowe, oparte na scenariuszach procedury zadań dla deweloperów uwierzytelniania, takich jak uzyskiwanie tokenów dostępu i korzystanie z nich w wywołaniach interfejsu API programu Microsoft Graph i innych interfejsów API, implementowanie logowania firmy Microsoft przy użyciu tradycyjnej aplikacji internetowej opartej na przeglądarce za pomocą protokołu OpenID Connect i innych, zapoznaj się z sekcją **Samouczki** w dokumentacji.
  * Aby pobrać przykłady kodu, przejdź do witryny [GitHub](https://github.com/Azure-Samples?q=active-directory).
* Przepływ żądań i odpowiedzi dotyczących procesu uwierzytelniania jest określony przez używany protokół uwierzytelniania, taki jak OAuth 2.0, OpenID Connect, WS-Federation lub SAML 2.0. Aby uzyskać więcej informacji na temat protokołów, zobacz sekcję **Pojęcia > Protokoły** w dokumentacji.

W przykładowym scenariuszu powyżej aplikacje można sklasyfikować według tych dwóch ról:

* Aplikacje, które wymagają bezpiecznego dostępu do zasobów.
* Aplikacje, które same pełnią rolę zasobu.

Teraz, po zapoznaniu się z podstawami, kontynuuj czytanie, aby zrozumieć model oraz interfejs API aplikacji tożsamości, poznać sposób działania aprowizacji w usłudze Azure AD i uzyskać linki do szczegółowych informacji na temat typowych scenariuszy obsługiwanych przez usługę Azure AD.

## <a name="application-model"></a>Model aplikacji

Usługa Azure AD reprezentuje aplikacje działające według określonego modelu, który został zaprojektowany w celu pełnienia dwóch głównych funkcji:

* **Identyfikowanie aplikacji zgodnie z obsługiwanymi protokołami uwierzytelniania** — obejmuje to wyliczanie wszystkich identyfikatorów, adresów URL, wpisów tajnych i powiązanych informacji, które są wymagane podczas uwierzytelniania. Tutaj usługa Azure AD pełni następujące funkcje:

    * Przechowuje wszystkie dane wymagane do obsługi uwierzytelniania w czasie wykonywania.
    * Przechowuje wszystkie dane konieczne do zdecydowania, jakich zasobów może wymagać aplikacja w celu uzyskania dostępu oraz czy konkretne żądanie powinno zostać spełnione i pod jakimi warunkami.
    * Udostępnia infrastrukturę do implementowania aprowizacji aplikacji w ramach dzierżawy dewelopera aplikacji i dowolnej innej dzierżawy usługi Azure AD.

* **Obsługa zgody użytkownika podczas żądania tokenu i umożliwienie dynamicznej aprowizacji aplikacji między dzierżawami** — tutaj usługa Azure AD pełni następujące funkcje:

    * Umożliwia użytkownikom i administratorom dynamiczne wyrażanie zgody lub odmawianie zgody dla aplikacji na uzyskiwanie przez nią dostępu do zasobów w ich imieniu.
    * Umożliwia administratorom ostateczne zdecydowanie, jakie działania mogą podejmować aplikacje, którzy użytkownicy mogą używać określonych aplikacji i w jaki sposób uzyskiwany jest dostęp do zasobów katalogu.

W usłudze Azure AD **obiekt aplikacji** opisuje aplikację jako jednostkę abstrakcyjną. Deweloperzy pracują z aplikacjami. Podczas wdrażania usługa Azure AD używa danego obiektu aplikacji jako konspektu do utworzenia **jednostki usługi**, która reprezentuje konkretne wystąpienie aplikacji w katalogu lub dzierżawie. To właśnie jednostka usługi definiuje, jakie działania może w rzeczywistości wykonywać aplikacja w konkretnym katalogu docelowym, kto może z niej korzystać, do jakich zasobów ma ona dostęp itd. Usługa Azure AD tworzy jednostkę usługi na podstawie obiektu aplikacji poprzez wyrażenie **zgody**.

Na poniższym diagramie przedstawiono uproszczony przepływ aprowizowania usługi Azure AD sterowany poprzez wyrażenie zgody.

![Uproszczony przepływ aprowizowania sterowany poprzez wyrażenie zgody](./media/authentication-scenarios/simplified-provisioning-flow-consent.png)

Ten przepływ aprowizowania składa się z następujących etapów:

|   |   |
|---|---|
| 1 | Użytkownik B próbuje zalogować się w aplikacji. |
| 2 | Poświadczenia użytkownika są odbierane i weryfikowane. |
| 3 | Użytkownik jest monitowany o wyrażenie zgody na przyznanie aplikacji dostępu do dzierżawy B. |
| 4 | Usługa Azure AD używa obiektu aplikacji A jako konspektu do utworzenia jednostki usługi w B. |
| 5 | Użytkownik otrzymuje żądany token. |
|   |   |

Ten proces może być powtarzany dowolną liczbę razy dla innych dzierżaw (C, D itd.). Katalog A przechowuje konspekt dla aplikacji (obiekt aplikacji). Użytkownicy i administratorzy wszystkich pozostałych dzierżaw, dla których aplikacja otrzymała zgodę, zachowują kontrolę nad tym, jakie działania może wykonywać aplikacja przy użyciu odpowiedniego obiektu jednostki usługi w każdej dzierżawie. Aby uzyskać więcej informacji, zobacz [Application and service principal objects in Azure AD](app-objects-and-service-principals.md) (Obiekty aplikacji i jednostki usługi w usłudze Azure AD).

## <a name="claims-in-azure-ad-security-tokens"></a>Oświadczenia w tokenach zabezpieczających usługi Azure AD

Tokeny zabezpieczające (tokeny dostępu i identyfikatora) wydane przez usługę Azure AD zawierają oświadczenia lub potwierdzenia informacji o podmiocie, który został uwierzytelniony. Aplikacje mogą używać oświadczeń do wykonywania różnych zadań, takich jak:

* Sprawdzanie poprawności tokenu
* Identyfikowanie dzierżawy katalogu podmiotu
* Wyświetlanie informacji o użytkowniku
* Określanie autoryzacji podmiotu

Oświadczenia obecne w dowolnym tokenie zabezpieczającym są zależne od typu tokenu, typu poświadczenia użytego do uwierzytelnienia użytkownika i konfiguracji aplikacji.

W poniższej tabeli znajduje się krótki opis każdego typu oświadczenia emitowanego przez usługę Azure AD. Aby uzyskać bardziej szczegółowe informacje, zobacz [tokeny dostępu](access-tokens.md) i [tokeny Identyfikatora](id-tokens.md) wystawione przez usługę Azure AD.

| Oświadczenie | Opis |
| --- | --- |
| Identyfikator aplikacji | Określa aplikację, która korzysta z tokenu. |
| Grupy odbiorców | Identyfikuje zasób odbiorcy, dla którego jest przeznaczony token. |
| Application Authentication Context Class Reference | Wskazuje, jak klient został uwierzytelniony (klient publiczny lub klient poufny). |
| Błyskawiczne uwierzytelnianie | Rejestruje datę i godzinę wystąpienia uwierzytelniania. |
| Metoda uwierzytelniania | Wskazuje, jak podmiot tokenu został uwierzytelniony (hasło, certyfikat itp.). |
| Imię | Udostępnia imię użytkownika w postaci, w jakiej jest ono ustawione w usłudze Azure AD. |
| Grupy | Zawiera identyfikatory obiektów grup usługi Azure AD, których członkiem jest użytkownik. |
| Dostawca tożsamości | Rejestruje dostawcę tożsamości, który uwierzytelnił podmiot tokenu. |
| Wystawiony o | Rejestruje godzinę, o której został wystawiony token (często używane w celu odświeżenia tokenu). |
| Wystawca | Identyfikuje usługę STS, która emitowała token, a także dzierżawę usługi Azure AD. |
| Nazwisko | Udostępnia nazwisko użytkownika w postaci, w jakiej jest ono ustawione w usłudze Azure AD. |
| Name (Nazwa) | Udostępnia zrozumiałą wartość identyfikującą podmiot tokenu. |
| Identyfikator obiektu | Zawiera niezmienny, unikatowy identyfikator podmiotu w usłudze Azure AD. |
| Role | Zawiera przyjazne nazwy ról aplikacji usługi Azure AD, które udzielono użytkownikowi. |
| Zakres | Wskazuje uprawnienia przyznane aplikacji klienckiej. |
| Podmiot | Określa podmiot zabezpieczeń, dla którego token określa informacje. |
| Identyfikator dzierżawy | Zawiera niezmienny, unikatowy identyfikator dzierżawy katalogu, który wystawił token. |
| Czas życia tokenu | Definiuje przedział czasu, przez który token jest prawidłowy. |
| Nazwa główna użytkownika | Zawiera nazwę główną użytkownika podmiotu. |
| Wersja | Zawiera numer wersji tokenu. |

## <a name="next-steps"></a>Następne kroki

* Dowiedz się więcej o [typach aplikacji i scenariuszach obsługiwanych w usłudze Azure Active Directory](app-types.md)
