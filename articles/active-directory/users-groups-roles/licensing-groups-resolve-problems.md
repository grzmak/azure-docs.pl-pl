---
title: Rozwiązywanie problemów z licencją dla grupy w usłudze Azure Active Directory | Dokumentacja firmy Microsoft
description: Jak identyfikowanie i rozwiązywanie problemów z licencją przydziału podczas korzystania z usługi Azure Active Directory na podstawie grupy licencji
services: active-directory
keywords: Licencjonowanie usługi Azure AD
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d64cf71ea3a44b7539835e3616150218e8b3635
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861771"
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Identyfikowanie i rozwiązywanie problemów z przypisania licencji dla grupy w usłudze Azure Active Directory

Licencjonowanie na podstawie grupy w usłudze Azure Active Directory (Azure AD) wprowadzono pojęcie użytkowników w stanie błąd licencjonowania. W tym artykule Wyjaśnijmy przyczyny, dlaczego użytkownicy może się to zakończyć w tym stanie.

Po przypisaniu licencji bezpośrednio do poszczególnych użytkowników, bez korzystania z licencjonowaniem opartym na grupy operacji przypisania może zakończyć się niepowodzeniem. Na przykład podczas wykonywania polecenia cmdlet programu PowerShell `Set-MsolUserLicense` w systemie użytkownika, polecenie cmdlet może się nie powieść wiele przyczyn, które są powiązane z logiką biznesową. Na przykład może być wystarczającej liczby licencji lub konflikt między dwiema planów usług, które nie można przypisać w tym samym czasie. Problem jest od razu ponownie Ci zgłoszony.

Podczas korzystania z oparte na grupach licencji, mogą wystąpić błędy ten sam, ale wystąpią w tle, a usługa Azure AD jest przypisywanie licencji. Z tego powodu błędy nie komunikowane użytkownikowi bezpośrednio. Zamiast tego są rejestrowane w obiekcie użytkownika i następnie raportowany za pośrednictwem portalu administracyjnego. Oryginalny zamiar licencji użytkownika nigdy nie zostaną utracone, ale jest rejestrowana w stanie błędu dla przyszłych badania i rozwiązywania problemów.

## <a name="how-to-find-license-assignment-errors"></a>Jak znaleźć błędy związane z przypisaniem licencji
**Aby znaleźć błędy związane z przypisaniem licencji**

   1. Aby wyszukać użytkowników w stanie błędu do określonej grupy, otwórz okienko dla grupy. W obszarze **licencji**, pojawi się powiadomienie w przypadku wszystkich użytkowników w stanie błędu.

   ![Grupa powiadomienia o błędzie](./media/licensing-groups-resolve-problems/group-error-notification.png)

   2. Wybierz powiadomienie, aby otworzyć listę wszystkich użytkowników, których to dotyczy. Możesz wybrać poszczególni pojedynczo, aby zobaczyć więcej szczegółów.

   ![Grupy, listę użytkowników w stanie Błąd](./media/licensing-groups-resolve-problems/list-of-users-with-errors.png)

   3. Aby znaleźć wszystkie grupy, które zawierają co najmniej jeden błąd w **usługi Azure Active Directory** Wybierz blok **licencji**, a następnie wybierz **Przegląd**. Pole informacyjne jest wyświetlane, gdy grupy wymagają Twojej uwagi.

   ![Przegląd informacji na temat grup w stanie Błąd](./media/licensing-groups-resolve-problems/group-errors-widget.png)

   4. Wybierz pole, aby wyświetlić listę wszystkich grup z błędami. Możesz wybrać każdej grupy, aby uzyskać więcej informacji.

   ![Omówienie, lista grup z błędami](./media/licensing-groups-resolve-problems/list-of-groups-with-errors.png)


W poniższych sekcjach znajdują się opis każdego potencjalnego problemu i sposobu rozwiązania go.

## <a name="not-enough-licenses"></a>Za mało licencji

**Problem:** nie ma wystarczającej liczby dostępnych licencji dla jednego z produktów, które określono w grupie. Musisz kupić więcej licencji dla produktu albo zwolnić nieużywanych licencji z innym użytkownikom lub grupom.

Aby zobaczyć, ile licencji są dostępne, przejdź do **usługi Azure Active Directory** > **licencji** > **wszystkie produkty**.

Aby zobaczyć, którzy użytkownicy i grupy zużywa bardzo licencji, wybierz produkt. W obszarze **licencjonowani użytkownicy**, możesz wyświetlić listę wszystkich użytkowników, którzy mieli licencje przypisane bezpośrednio lub za pośrednictwem co najmniej jedną grupę. W obszarze **licencjonowane grupy**, zobaczysz wszystkie grupy, które mają ten produktów przypisane.

**Program PowerShell:** poleceń cmdlet programu PowerShell Zgłoś ten błąd jako _CountViolation_.

## <a name="conflicting-service-plans"></a>Plany usług powodujące konflikty

**Problem:** jednego z produktów, które jest określone w grupie zawiera planu usługi, które są sprzeczne z innego planu usług, który jest już przypisana do użytkownika za pośrednictwem innego produktu. Niektóre plany usługi są skonfigurowane w taki sposób, że nie można przypisać do tego samego użytkownika planu usług, powiązane.

Rozważmy następujący przykład. Użytkownik ma licencję dla Office 365 Enterprise *E1* są przypisywane bezpośrednio przy użyciu wszystkich planów, które są włączone. Użytkownik został dodany do grupy, która ma Office 365 Enterprise *E3* produktu do niej przypisany. Produkt E3 zawiera planów usług, które nie mogą się pokrywać z planów, które są objęte E1, dzięki czemu grupy przypisanie licencji kończy się niepowodzeniem z powodu błędu "Plany usług powodujące konflikt". W tym przykładzie są plany usług powodujące konflikty:

-   SharePoint Online (Plan 2) powoduje konflikt z usługą SharePoint Online (Plan 1).
-   Exchange Online (Plan 2) powoduje konflikt z usługą Exchange Online (Plan 1).

Aby rozwiązać ten konflikt, należy wyłączyć, dwóch planów. Można wyłączyć licencja E1, który jest bezpośrednio przypisane do użytkownika. Lub, musisz zmodyfikować całej grupie przypisania licencji i wyłączyć plany licencji E3. Alternatywnie można zdecydować usunąć licencję E1 od użytkownika, jeśli jest nadmiarowy w ramach licencji E3.

Decyzja o tym, jak rozwiązać konflikt licencje produktów zawsze należy do administratora. Usługa Azure AD nie automatycznie rozwiązać konflikty licencji.

**Program PowerShell:** poleceń cmdlet programu PowerShell Zgłoś ten błąd jako _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Inne produkty są zależne od tej licencji

**Problem:** jednego z produktów, które jest określone w grupie zawiera plan usługi, który musi być włączona dla innego planu usług, w innego produktu do funkcji. Ten błąd występuje podczas próby usunięcia podstawowego planu usługi Azure AD. Na przykład może to nastąpić po usunięciu użytkownika z grupy.

Aby rozwiązać ten problem, należy się upewnić, że wymagany plan jest przypisany do użytkowników za pomocą innej metody lub że zależne usługi zostały wyłączone dla tych użytkowników. Po tym można poprawnie usunąć licencji grupy z tych użytkowników.

**Program PowerShell:** poleceń cmdlet programu PowerShell Zgłoś ten błąd jako _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Lokalizacja użycia jest niedozwolona.

**Problem:** niektóre usługi firmy Microsoft nie są dostępne we wszystkich lokalizacjach w związku z lokalnymi przepisami i regulacjami. Zanim będzie można przypisać licencję do użytkownika, należy określić **lokalizacji użytkowania** właściwości dla użytkownika. Możesz określić lokalizację, w obszarze **użytkownika** > **profilu** > **ustawienia** sekcji w witrynie Azure portal.

Gdy usługa Azure AD próbuje przypisać licencję grupy do użytkownika, którego lokalizacja użycia nie jest obsługiwane, nie powiedzie się i rejestruje błąd użytkownika.

Aby rozwiązać ten problem, należy usunąć użytkowników z nieobsługiwanych lokalizacji z licencjonowanego grupy. Alternatywnie Jeśli bieżące wartości lokalizacji użycia nie stanowią lokalizacji rzeczywistego użytkownika, można zmodyfikować je tak, aby poprawnie przypisywania licencji następnym (jeśli jest obsługiwany nowej lokalizacji).

**Program PowerShell:** poleceń cmdlet programu PowerShell Zgłoś ten błąd jako _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> W przypadku usługi Azure AD spowoduje przypisanie licencji grupy, wszyscy użytkownicy bez lokalizacji określonego użycia dziedziczą lokalizację katalogu. Zaleca się, czy Administratorzy ustawić poprawne użycie wartości lokalizacji na użytkowników przed rozpoczęciem korzystania z licencjonowaniem opartym na grupy do wykonania z lokalnymi przepisami i regulacjami.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Co się stanie, jeśli istnieje więcej niż jedna licencja na produkt w grupie?

Więcej niż jedna licencja na produkt można przypisać do grupy. Na przykład Office 365 Enterprise E3 i rozwiązania Enterprise Mobility + Security można przypisać do grupy, aby łatwo włączyć dołączane usług oferowanych użytkownikom końcowym.

Usługa Azure AD próbuje przypisać wszystkie licencje, które są określone w grupie, aby każdy użytkownik. Jeśli usługi Azure AD, nie można przypisać jednego z produktów z powodu problemów z logiki biznesowej, go nie albo przypisać inne licencje w tej grupie. Przykładem jest, jeśli nie ma wystarczającą liczbę licencji dla wszystkich lub jeśli występują konflikty z innymi usługami, które są włączone dla użytkownika.

Widoczne dla użytkowników, którzy nie zostaną przypisane i sprawdzić, które produkty ma wpływ ten problem.

## <a name="how-do-you-manage-licenses-for-products-with-prerequisites"></a>Jak zarządzać licencje na produkty z wymagań wstępnych

Niektóre produkty Online firmy Microsoft, może być właścicielem *dodatki*. Dodatki wymaga planu usługi wymagań wstępnych być włączone dla użytkownika lub grupy, zanim mogą mieć przypisanych licencji. Z licencjonowaniem opartym na grupach, system wymaga zarówno wymagań wstępnych, jak i dodatku planów usług w tej samej grupie. Ma to zagwarantować, że użytkownicy, którzy są dodawane do grupy można w pełni działającego produkt. Rozważmy poniższy przykład:

Microsoft Workplace Analytics jest produktem dodatku. Zawiera on plan pojedynczą usługę o takiej samej nazwie. Firma Microsoft można przypisać tylko tego planu usługi do użytkownika lub grupy, gdy jest przypisany jeden z następujących wymagań wstępnych:
- Usługa Exchange Online (Plan 1) 
- Usługa Exchange Online (Plan 2)

Jeśli firma Microsoft próbuje przypisać tym produktem w własnej grupie, portalu zwraca błąd. Wybranie powiadomienia o błędzie zawiera następujące informacje:

![Brak grupy, wymagań wstępnych](./media/licensing-groups-resolve-problems/group-prerequisite-required.png)

Gdy wybierzemy szczegółowe informacje, przedstawia następujący komunikat o błędzie:

>Operacja licencji nie powiodło się. Upewnij się, że grupa ma niezbędnych usług przed dodaniem lub usunięciem usługi zależnej. **Usługa Microsoft Workplace Analytics wymaga usługi Exchange Online (Plan 2), można także włączone.**

Aby przypisać tej licencji dodatku do grupy, Upewniamy się, że grupa zawiera również w ramach planu usług wymagań wstępnych. Na przykład możemy zaktualizować istniejącą grupę, która już zawiera pełną wersję usługi Office 365 E3, a następnie Dodaj produkt dodatek do niego.

Jest również możliwość utworzenia grupy autonomicznego, która zawiera tylko minimalne wymagane produkty, aby ułatwić dodatek pracy. Może służyć licencji dla produktu dodatek tylko wybranych użytkowników. W tym przykładzie firma Microsoft ma przypisane następujące produkty w tej samej grupie:
- Usługa Office 365 Enterprise E3 z tylko w ramach usługi Exchange Online (Plan 2) planu usług włączone
- Microsoft Workplace Analytics

![Dołączone do grupy, wstępnie wymaganego składnika](./media/licensing-groups-resolve-problems/group-addon-with-prerequisite.png)

Od teraz dodanych do tej grupy użytkowników korzystać jedną licencję produktu E3 i jedna licencja produktu Workplace Analytics. W tym samym czasie Ci użytkownicy mogą należeć do innej grupy, co umożliwia im pełnego produktu E3 i nadal zużywają tylko jedną licencję takiego produktu.

> [!TIP]
> Można utworzyć wiele grup dla każdego planu usługi wymagań wstępnych. Na przykład, jeśli używasz pakietu Office 365 Enterprise E1 i Office 365 Enterprise E3 dla użytkowników, możesz utworzyć dwie grupy licencji firmy Microsoft Workplace Analytics: jedną, która używa E1 jako warunek wstępny, a druga używa E3. Dzięki temu można rozpowszechnić dodatek do planu E1 i E3 użytkowników bez używania dodatkowe licencje.

## <a name="license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online"></a>Przypisanie licencji kończy się niepowodzeniem dyskretnie dla użytkownika z powodu adresy serwerów proxy zduplikowane w programie Exchange Online

Jeśli używasz usługi Exchange Online, niektórzy użytkownicy w Twojej dzierżawie może niepoprawnie skonfigurowany z taką samą wartość adresu serwera proxy. Gdy Licencjonowanie na podstawie grupy próbuje przypisać licencję do takiego użytkownika, nie powiedzie się i nie rejestruje błąd. Błąd, aby zarejestrować błąd, w tym wystąpieniu jest to ograniczenie w wersji zapoznawczej ta funkcja i użyjemy, aby rozwiązać problem przed *ogólnie*.

> [!TIP]
> Jeśli zauważysz, że niektórzy użytkownicy nie otrzymała licencję i nie ma błędów rejestrowane dla tych użytkowników, najpierw sprawdzić, jeśli mają one adres serwera proxy duplikatów.
> Aby sprawdzić, czy adres serwera proxy duplikatów, wykonaj następujące polecenie cmdlet programu PowerShell dla usługi Exchange Online:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> Aby uzyskać więcej informacji na temat tego problemu, zobacz [w usłudze Exchange Online komunikat o błędzie "adres serwera Proxy jest już używana"](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online). Artykuł zawiera również informacje na [sposób nawiązywania połączeń do usługi Exchange Online przy użyciu zdalnego programu PowerShell](https://technet.microsoft.com/library/jj984289.aspx).

Po rozwiązaniu problemów adres serwera proxy dla użytkowników, których dotyczy problem, upewnij się wymusić przetwarzania licencji w grupie, aby upewnić się, że można teraz stosować licencji.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Jak wymusić przetwarzania licencji w grupie, aby naprawić błędy?

Zależności od tego, jakie kroki zostały wykonane w celu naprawić błędy może być konieczne ręcznie wyzwolić przetwarzania grupy, aby zaktualizować stan użytkownika.

Na przykład jeśli zwolnić niektórych licencji, usuwając bezpośrednich przypisań licencji użytkowników należy do wyzwolenia przetwarzania grupy, które nie jest w pełni licencji wszystkich elementów członkowskich użytkownika. Ponownego przetworzenia grupy, przejdź do okienka grupy, otwórz **licencji**, a następnie wybierz pozycję **ponownie przetworzyć** przycisk na pasku narzędzi.

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat innych scenariusze dotyczące zarządzania licencjami za pomocą grup, zobacz następujące tematy:

* [Przypisywanie licencji do grupy w usłudze Azure Active Directory](licensing-groups-assign.md)
* [Co to jest oparte na grupach Licencjonowanie w usłudze Azure Active Directory?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Jak przeprowadzić migrację użytkowników z licencjami indywidualnymi do licencjonowania opartego na grupy w usłudze Azure Active Directory](licensing-groups-migrate-users.md)
* [Usługa Azure Active Directory na podstawie grupy licencjonowania dodatkowe scenariusze](licensing-group-advanced.md)
