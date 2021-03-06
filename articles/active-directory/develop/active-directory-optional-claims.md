---
title: Dowiedz się, jak podaj opcjonalny oświadczenia do aplikacji usługi Azure AD | Dokumentacja firmy Microsoft
description: Wskazówki dotyczące dodawania niestandardowych lub dodatkowe oświadczenia języka SAML 2.0 i tokenów Web JSON (JWT) tokeny wystawione przez usługę Azure Active Directory.
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: c42e8978a94730669f3c3f879d1d26c4426bd9da
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079142"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app-public-preview"></a>Porady: dostarczanie opcjonalnych oświadczeń do aplikacji usługi Azure AD (publiczna wersja zapoznawcza)

Ta funkcja jest używana przez deweloperów aplikacji, aby określić, które oświadczenia, że chcą w tokenach wysyłanych do swoich aplikacji. Możesz użyć opcjonalnych oświadczeń:
- Wybierz dodatkowe oświadczenia, które mają zostać objęte tokenów dla aplikacji.
- Zmień zachowanie niektórych oświadczenia, które zwraca tokenów usługi Azure AD.
- Dodaj i dostęp do oświadczenia niestandardowe dla swojej aplikacji. 

> [!Note]
> Ta funkcja jest obecnie w publicznej wersji zapoznawczej. Przygotuj się na przywracanie lub usuwanie wszelkich zmian. Ta funkcja jest dostępna w dowolnej subskrypcji usługi Azure AD w publicznej wersji zapoznawczej. Gdy ta funkcja stanie się ogólnie dostępna, niektóre cechy funkcji mogą jednak wymagać subskrypcję usługi Azure AD premium.

Listę standardowych oświadczeń i jak są używane w tokenach, zobacz [podstawy tokeny wystawione przez usługę Azure AD](v1-id-and-access-tokens.md). 

Jednym z celów [punktu końcowego v2.0 usługi Azure AD](active-directory-appmodel-v2-overview.md) jest mniejsze rozmiary tokenu, aby zapewnić optymalną wydajność przez klientów.  W wyniku kilku oświadczenia, wcześniej uwzględnione w dostępu i identyfikator tokenów nie są już dostępne w wersji 2.0 tokenów i musi monit o wpisanie specjalnie dla poszczególnych aplikacji.

  

**Tabela 1: zastosowanie**

| Typ konta | Punkt końcowy w wersji 1.0 | Punkt końcowy v2.0  |
|--------------|---------------|----------------|
| Osobiste konto Microsoft  | N/d - użyty RPS biletów | Obsługa dostępne |
| Konto Azure AD          | Obsługiwane                          | Obsługiwane z zastrzeżeniami      |

> [!Important]
> W tej chwili aplikacji obsługujących konta osobiste i usługi Azure AD (za pośrednictwem [portalu rejestracji aplikacji](https://apps.dev.microsoft.com)) nie można użyć oświadczeń opcjonalne.  Jednak aplikacje zarejestrowane dla właśnie Azure AD przy użyciu punktu końcowego v2.0 można uzyskać opcjonalnych oświadczenia, które są wymagane w manifeście.

## <a name="standard-optional-claims-set"></a>Zestaw standardowych opcjonalnych oświadczeń
Zestaw oświadczeń opcjonalne, domyślnie dostępne do użycia przez aplikacje są wymienione poniżej.  Aby dodać opcjonalny oświadczenia niestandardowe dla swojej aplikacji, zobacz [rozszerzenia katalogów](active-directory-optional-claims.md#Configuring-custom-claims-via-directory-extensions)poniżej.  Należy pamiętać, że podczas dodawania oświadczeń **token dostępu**, zostaną zastosowane do tokenów dostępu do żądanego *dla* aplikacji (internetowego interfejsu API), nie tych *przez* aplikacji.  Dzięki temu niezależnie od tego klienta, uzyskiwanie dostępu do interfejsu API, odpowiednie dane są obecne w tokenie dostępu, których używają do uwierzytelniania względem interfejsu API.

> [!Note]
>Większość te oświadczenia mogą być dołączane w tokenów Jwt dla wersji 1.0 i tokenów w wersji 2.0, ale nie tokeny SAML, z wyjątkiem w przypadku, gdy wskazane w kolumnie Typ tokenu.  Ponadto podczas opcjonalnych oświadczeń tylko obecnie są obsługiwane dla użytkowników usługi AAD, zarządzanych kont usług pomocy technicznej jest dodawany.  Gdy MSA ma opcjonalnych oświadczeń obsługuje punktu końcowego v2.0, kolumna typu użytkownika określa, czy roszczenie jest dostępna dla użytkowników usługi AAD lub zarządzanych kont usług.  

**Tabela 2: Zestaw standardowych opcjonalnego roszczenia**

| Name (Nazwa)                        | Opis   | Typ tokenu | Typ użytkownika | Uwagi  |
|-----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Czas, kiedy użytkownik ostatnio uwierzytelniony.  Zobacz specyfikacje OpenID Connect.| JWT        |           |  |
| `tenant_region_scope`      | Region zasobu dzierżawy | JWT        |           | |
| `signin_state`             | Zaloguj się w stanie oświadczeń   | JWT        |           | 6 wartości, są zwracane jako flagi:<br> "dvc_mngd": urządzenie jest zarządzane<br> "dvc_cmp": urządzenie jest zgodne<br> "dvc_dmjd": urządzenie jest przyłączone do domeny<br> "dvc_mngd_app": urządzenie jest zarządzane za pośrednictwem rozwiązania MDM<br> "inknownntwk": urządzenie jest wewnątrz znanej sieci.<br> "kmsi": Zachowaj mnie podpisane w był używany. <br> |
| `controls`                 | Atrybut wielowartościowy elementu oświadczenia, zawierająca kontrolki sesji wymuszane przez zasady dostępu warunkowego.  | JWT        |           | 3 wartości:<br> "app_res": aplikacja potrzebuje do wymuszania bardziej szczegółowe ograniczenia. <br> "ca_enf": została odroczona wymuszania dostępu warunkowego i jest nadal wymagana. <br> "no_cookie": ten token jest niewystarczająca do wymiany dla pliku cookie w przeglądarce. <br>  |
| `home_oid`                 | Dla użytkowników-gości, identyfikator obiektu użytkownika w dzierżawie macierzystego użytkownika.| JWT        |           | |
| `sid`                      | Identyfikator sesji, umożliwiający wylogowanie użytkownika sesji. | JWT        |           |         |
| `platf`                    | Platforma urządzeń    | JWT        |           | Ograniczone do zarządzanych urządzeń, które można sprawdzić typ urządzenia.|
| `verified_primary_email`   | Źródło PrimaryAuthoritativeEmail użytkownika      | JWT        |           |         |
| `verified_secondary_email` | Źródło SecondaryAuthoritativeEmail użytkownika   | JWT        |           |        |
| `enfpolids`                | Identyfikatory wymuszanych zasad. Lista zasad identyfikatorów, które zostały ocenione dla bieżącego użytkownika.  | JWT |  |  |
| `vnet`                     | Informacje o specyfikator sieci Wirtualnej.    | JWT        |           |      |
| `fwd`                      | Adres IP.| JWT    |   | Dodaje oryginalny adres IPv4 klienta (wewnątrz sieci Wirtualnej) |
| `ctry`                     | Kraj użytkownika | JWT |           | Usługa Azure AD zwraca `ctry` opcjonalnego roszczenia, jeśli jest obecny, a wartość oświadczenia jest kod standardowa kraju dwuliterowych, takich jak FR, JP, SZ i tak dalej. |
| `tenant_ctry`              | Kraj zasobów dzierżawy | JWT | | |
| `xms_pdl`          | Preferowana lokalizacja danych   | JWT | | W przypadku dzierżaw wielu regionów geograficznych jest 3-literowy kod, przedstawiający regionu geograficznego, użytkownik znajduje się w.  Aby uzyskać więcej informacji, zobacz [program Azure AD Connect dokumentacji dotyczącej Preferowana lokalizacja danych](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation). <br> Na przykład: `APC` dla Azja. |
| `xms_pl`                   | Preferowany język  | JWT ||Użytkownik preferowanego języka, jeśli ustawiona.  Źródło ich głównej dzierżawy w scenariuszach dostęp gościa.  Sformatowana LL DW ("en-us"). |
| `xms_tpl`                  | Dzierżawy preferowany język| JWT | | Dzierżawy zasobów preferowanego języka, jeśli ustawiona.  LL sformatowany ("PL"). |
| `ztdid`                    | Bezobsługowa identyfikator wdrożenia | JWT | | Tożsamość urządzenia używana dla [rozwiązania Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) |
| `acct`             | Stan konta użytkowników w dzierżawie.   | JWT, SAML | | Jeśli użytkownik jest członkiem dzierżawy, wartość jest `0`.  Jeśli są one gościa, wartość jest `1`.  |
| `upn`                      | Oświadczenie UserPrincipalName.  | JWT, SAML  |           | Mimo że to oświadczenie jest automatycznie dołączane, możesz je określić jako opcjonalnego roszczenia, aby dołączyć dodatkowe właściwości, aby zmodyfikować jego zachowanie w przypadku użytkownika gościa.  <br> Dodatkowe właściwości: <br> `include_externally_authenticated_upn` <br> `include_externally_authenticated_upn_without_hash` |

### <a name="v20-optional-claims"></a>Opcjonalne oświadczeń w wersji 2.0

Te oświadczenia są zawsze dołączane w tokenach v1.0, ale nie zostały uwzględnione w tokenów w wersji 2.0, chyba że żądanie.  Te oświadczenia są tylko odpowiednie dla elementów Jwt (identyfikator tokenów i tokenów dostępu).  

**Tabela 3: Tylko do wersji 2.0 opcjonalnych oświadczeń**

| Token JWT oświadczeń     | Name (Nazwa)                            | Opis                                                                                                                    | Uwagi |
|---------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|
| `ipaddr`      | Adres IP                      | Adres IP klienta, zalogowany z.                                                                                      |       |
| `onprem_sid`  | Identyfikator zabezpieczeń w środowisku lokalnym |                                                                                                                                |       |
| `pwd_exp`     | Czas wygaśnięcia hasła        | Data i godzina jaką hasło wygaśnie.                                                                                    |       |
| `pwd_url`     | Zmień hasło, adres URL             | Adres URL, który użytkownik może odwiedzić, aby zmienić swoje hasło.                                                                        |       |
| `in_corp`     | Wewnątrz sieci firmowej        | Sygnały, jeśli klient jest logowania się z siecią firmową. Jeśli nie są one oświadczenia nie dołączono                     |       |
| `nickname`    | Pseudonim                        | Dodatkową nazwę użytkownika, niezależnie od imię lub nazwisko.                                                             |       |                                                                                                                |       |
| `family_name` | Nazwisko                       | Zawiera ostatni nazwę, nazwisko lub nazwę rodziny użytkownika, zgodnie z definicją w obiekcie użytkownika usługi Azure AD. <br>"family_name":"Miller" |       |
| `given_name`  | Imię                      | Zawiera pierwszy lub "" Nazwa użytkownika, według stawki ustalonej w obiekcie użytkownika usługi Azure AD.<br>"given_name": "Piotr"                   |       |

### <a name="additional-properties-of-optional-claims"></a>Dodatkowe właściwości opcjonalnych oświadczeń

Aby zmienić sposób, w jaki oświadczenie jest zwracany można skonfigurować kilka opcjonalnych oświadczeń.  Te dodatkowe właściwości są najczęściej używane migracji aplikacji lokalnych przy użyciu różnych danych oczekiwania (na przykład `include_externally_authenticated_upn_without_hash` może ułatwić realizację klientów, którzy nie może obsługiwać hashmarks (`#`) nazwę UPN)

**Tabela 4: Wartości do konfigurowania standardowa oświadczenia opcjonalne**

| Nazwa właściwości                                     | Nazwa właściwości dodatkowe                                                                                                             | Opis |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `upn`                                                 |                                                                                                                                      |  Może służyć do odpowiedzi SAML i tokenu JWT.            |
| | `include_externally_authenticated_upn`              | Obejmuje gościa nazwy UPN jako przechowywane w dzierżawie zasobów.  Na przykład: `foo_hometenant.com#EXT#@resourcetenant.com`                            |             
| | `include_externally_authenticated_upn_without_hash` | Jak wyżej, poza tym, że hashmarks (`#`) są zastępowane znakami podkreślenia (`_`), na przykład `foo_hometenant.com_EXT_@resourcetenant.com` |             

> [!Note]
>Określanie nazwy upn opcjonalnego roszczenia, bez dodatkowych właściwości nie powoduje zmiany wszelkich zachowań — aby zobaczyć nowe oświadczenie wystawionych w tokenie, co najmniej jeden z dodatkowych właściwości muszą zostać dodane. 

#### <a name="additional-properties-example"></a>Przykład dodatkowe właściwości

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

Ten obiekt OptionalClaims powoduje, że identyfikator tokenu zwracana do klienta do uwzględnienia innej nazwy upn z dodatkowych głównej dzierżawy i informacje o zasobach dzierżawy.  To spowoduje jedynie zmianę `upn` oświadczenia w tokenie, jeśli użytkownik Gość w dzierżawie, (które używa innego dostawcy tożsamości do uwierzytelniania). 

## <a name="configuring-optional-claims"></a>Konfigurowanie opcjonalnych oświadczeń

Można skonfigurować opcjonalny oświadczenia dla danej aplikacji, modyfikując manifest aplikacji (Zobacz przykład poniżej). Aby uzyskać więcej informacji, zobacz [opis artykułu manifestu aplikacji usługi Azure AD](reference-app-manifest.md).

**Schemat przykładowych:**

```json
"optionalClaims":  
   {
       "idToken": [
             { 
                   "name": "auth_time", 
                   "essential": false
              }
        ],
 "accessToken": [ 
             {
                    "name": "ipaddr", 
                    "essential": false
              }
        ],
"saml2Token": [ 
              { 
                    "name": "upn", 
                    "essential": true
               },
               { 
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": true
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>Typ OptionalClaims

Deklaruje opcjonalnych oświadczeń, żądane przez aplikację. Aplikację można skonfigurować opcjonalny oświadczeń ma zostać zwrócone w każdej z trzech rodzajów tokenów (identyfikator tokenu, token, SAML 2 token dostępu) może odbierać z usługi tokenu zabezpieczającego. Aplikację można skonfigurować różne zestawy opcjonalnych oświadczeń, które ma zostać zwrócone w każdego typu tokenu. Właściwość OptionalClaims Jednostka aplikacji jest obiektem OptionalClaims.

**Tabela 5: Właściwości typu OptionalClaims**

| Name (Nazwa)        | Typ                       | Opis                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Kolekcja (OptionalClaim) | Opcjonalne oświadczenia zwrócone w token JWT Identyfikatora.     |
| `accessToken` | Kolekcja (OptionalClaim) | Opcjonalne oświadczenia zwrócone w tokenie dostępu JWT. |
| `saml2Token`  | Kolekcja (OptionalClaim) | Opcjonalne oświadczenia zwrócone w tokenie języka SAML.       |

### <a name="optionalclaim-type"></a>Typ OptionalClaim

Zawiera opcjonalnego roszczenia skojarzone z aplikacją lub ta jednostka usługi. Właściwości idToken, accessToken i saml2Token [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) typ jest kolekcją OptionalClaim.
Jeśli jest obsługiwany przez określonych oświadczenia, można również zmodyfikować zachowanie OptionalClaim, korzystając z pola dodatkowe właściwości.

**Tabela 6: Właściwości typu OptionalClaim**

| Name (Nazwa)                 | Typ                    | Opis                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | Nazwa opcjonalnego roszczenia.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | Źródło (obiektu katalogu) oświadczenia. Istnieją wstępnie zdefiniowane oświadczeń i zdefiniowanych przez użytkownika z właściwościami rozszerzenia. Jeśli wartość źródłowa jest równa null, oświadczenie jest wstępnie zdefiniowanych opcjonalnego roszczenia. Jeśli wartość źródłowa to użytkownika, wartość właściwości name jest właściwość rozszerzenia z obiektu użytkownika. |
| `essential`            | Edm.Boolean             | Jeśli ma wartość true, oświadczenia, określony przez klienta jest niezbędne do zapewnienia sprawnego autoryzacji umożliwiający określonego zadania, żądane przez użytkownika końcowego. Wartość domyślna to false.                                                                                                                 |
| `additionalProperties` | Kolekcja (Edm.String) | Dodatkowe właściwości oświadczenia. Jeśli właściwość istnieje w tej kolekcji, modyfikuje zachowanie opcjonalnego roszczenia określony we właściwości name.                                                                                                                                                   |
## <a name="configuring-custom-claims-via-directory-extensions"></a>Konfigurowanie oświadczenia niestandardowe, za pośrednictwem rozszerzenia katalogów

Oprócz zestaw standardowych opcjonalnych oświadczeń tokenów można również skonfigurować do uwzględnienia rozszerzenia schematu katalogu (zobacz [artykułu rozszerzenia schematu katalogu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) Aby uzyskać więcej informacji).  Ta funkcja jest przydatna do dołączania dodatkowych informacji dotyczących użytkowników, Twoja aplikacja może używać — na przykład, dodatkowe identyfikator lub opcji konfiguracji ważne, ustawionego przez użytkownika. 

> [!Note]
> Rozszerzenia schematu katalogu są funkcją tylko do usługi AAD, więc jeśli aplikacja manifestu żądań niestandardowego rozszerzenia i użytkownika konta Microsoft loguje się do aplikacji, te rozszerzenia nie zostaną zwrócone. 

### <a name="values-for-configuring-additional-optional-claims"></a>Wartości dotyczące konfigurowania dodatkowych, opcjonalnych oświadczeń

W przypadku atrybutów rozszerzenia, należy użyć pełnej nazwy rozszerzenia (w formacie: `extension_<appid>_<attributename>`) w manifeście aplikacji. `<appid>` Musi być zgodny z identyfikatorem aplikacji żądających oświadczeń. 

W ramach tokenu JWT, te oświadczenia będzie emitowane w następującym formacie nazwy: `extn.<attributename>`.

W tokeny SAML te oświadczenia będzie emitowane przy użyciu następującego formatu identyfikatora URI: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="optional-claims-example"></a>Przykład opcjonalnych oświadczeń

W tej sekcji można opisano scenariusz, aby zobaczyć, jak skorzystać z funkcji opcjonalnych oświadczeń dla aplikacji.
Brak dostępnych wiele opcji do aktualizacji właściwości na konfigurację tożsamości aplikacji, aby włączyć i skonfigurować opcjonalne oświadczeń:
-   Można zmodyfikować manifest aplikacji. W poniższym przykładzie użyje tej metody do przeprowadzenia konfiguracji. Odczyt [opis dokumentu manifestu aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) pierwszy wprowadzenie do manifestu.
-   Istnieje również możliwość pisania aplikacji, która używa [interfejsu API programu Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) zaktualizowania aplikacji. [Jednostki i odwołania do typów złożonych](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) w dokumentacji interfejsu API programu Graph przewodnik pomaga w konfigurowaniu opcjonalnych oświadczeń.

**Przykład:** w poniższym przykładzie należy zmodyfikować manifest aplikacji, aby dodawać oświadczenia dostępu, identyfikator i SAML tokenów przeznaczonych dla aplikacji.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
1. Po użytkownik został uwierzytelniony, należy wybrać dzierżawy usługi Azure AD, wybierając je z prawym górnym rogu strony.
1. Wybierz **rozszerzenia usługi Azure AD** z panelu nawigacyjnym po lewej stronie i kliknij przycisk **rejestracje aplikacji**.
1. Znajdź aplikację, którą chcesz skonfigurować opcjonalne oświadczeń na liście i kliknij go.
1. Na stronie aplikacji kliknij **manifestu** aby otworzyć Edytor manifestu w tekście. 
1. Można bezpośrednio edytować manifest za pomocą tego edytora. Manifest jest zgodna schematu dla [Jednostka aplikacji](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)i formaty automatyczne raz zapisać manifestu. Nowe elementy zostaną dodane do `OptionalClaims` właściwości.

      ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
      "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
      "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }
      ```
      W tym przypadku różne oświadczenia opcjonalne zostały dodane do każdego rodzaju token, który aplikacja może odbierać. Tokeny Identyfikatora będą teraz zawierać nazwę UPN dla użytkowników federacyjnych w pełnej postaci (`<upn>_<homedomain>#EXT#@<resourcedomain>`). Tokeny dostępu, które inne komputery klienckie zażądają tej aplikacji będzie teraz obejmować oświadczenia auth_time. Tokeny SAML będzie teraz zawierać rozszerzenia schematu katalogu skypeId (w tym przykładzie identyfikator aplikacji dla tej aplikacji jest ab603c56068041afb2f6832e2a17e237).  Tokeny SAML udostępni Identyfikator Skype jako `extension_skypeId`.

1. Po zakończeniu aktualizowania manifestu kliknij **Zapisz** można zapisać manifestu

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej na temat standardowych oświadczenia dostarczane przez usługę Azure AD.

- [Tokeny Identyfikatora](id-tokens.md)
- [Tokeny dostępu](access-tokens.md)
