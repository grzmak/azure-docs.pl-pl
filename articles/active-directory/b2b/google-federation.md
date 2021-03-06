---
title: Dodaj Google jako dostawcy tożsamości dla usługi Azure Active Directory B2B | Dokumentacja firmy Microsoft
description: Sfederuj za pomocą usługi Google, aby umożliwić użytkowników-gości do logowania do aplikacji usługi Azure AD przy użyciu konta Gmail
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: 396fb947a95ebc634ab0dea24d20f35126bc006e
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389449"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>Dodaj Google jako dostawcy tożsamości dla użytkowników-gości B2B

Przez skonfigurowanie Federacji za pomocą usługi Google, możesz zezwolić zaproszonych użytkowników do logowania się na udostępnionych aplikacji i zasobów przy użyciu własnych kont Google, bez konieczności tworzenia kont usługi Azure AD lub Accounts firmy Microsoft (MSA).  
> [!NOTE]
> Usługi Google użytkowników-gości musi Zaloguj się przy użyciu łącza, które zawiera na przykład kontekstu dzierżawy `https://myapps.microsoft.com/?tenantid=<tenant id>`. Bezpośrednie linki do aplikacji i zasobów również działać tak długo, jak należą do kontekstu dzierżawy. Użytkownicy-goście, są obecnie nie można zalogować się przy użyciu punktów końcowych, które mają nie kontekstu dzierżawy. Na przykład za pomocą `https://myapps.microsoft.com`, `https://portal.azure.com`, lub endpoint wspólnego zespoły spowoduje wystąpienie błędu.
 
## <a name="what-is-the-experience-for-the-google-user"></a>Co to jest środowisko użytkownika usługi Google?
Podczas wysyłania zaproszenia do użytkownika usługi Google Gmail użytkownik-Gość powinien uzyskać dostęp do swoje aplikacje udostępnione lub zasobów za pomocą łącza, które obejmuje kontekstu dzierżawy. Ich środowisko pracy różni się zależnie od tego, czy są już przeprowadzono logowanie do usługi Google:
  - Jeśli użytkownik-Gość nie jest zalogowany do usługi Google, ich monit logowanie do usługi Google.
  - Jeśli użytkownik-Gość jest już zalogowany do usługi Google, ich wyświetlony monit o wybranie konta które mają być użyte. Musi wybrać konto, którego używasz, aby zaprosić ich.

Jeśli użytkownik-Gość widzi błąd "za długi nagłówek", użytkownik podejmie próbę czyszczenie ich plików cookie lub mogą otworzyć okno prywatnych ani incognito i spróbuj zalogować się ponownie.

![Zaloguj się za pomocą konta Google](media/google-federation/google-sign-in.png)

## <a name="step-1-configure-a-google-developer-project"></a>Krok 1: Konfigurowanie projektu dla deweloperów Google
Najpierw utwórz nowy projekt w konsoli deweloperów Google, aby otrzymać klient, identyfikator i klucz tajny klienta, który można później dodać do usługi Azure AD. 
1. Przejdź do interfejsów API Google na https://console.developers.google.comi zaloguj się przy użyciu konta Google. Firma Microsoft zaleca używanie udostępnionych zespołu konto Google.
2. Utwórz nowy projekt: na pulpicie nawigacyjnym wybierz **Tworzenie projektu**, a następnie wybierz pozycję **Utwórz**. Na stronie nowy projekt wprowadź **Nazwa projektu**, a następnie wybierz pozycję **Utwórz**.
   
   ![Nowy projekt Google](media/google-federation/google-new-project.png)

3. Upewnij się, że jest zaznaczona opcja nowy projekt w menu projektu. Następnie otwórz menu w lewym górnym rogu i wybierz **interfejsów API i usługi** > **poświadczenia**.

   ![Poświadczenia interfejsu API Google](media/google-federation/google-api.png)
 
4. Wybierz **ekran zgody Oauth** kartę, a następnie wprowadź **nazwa produktu pokazywana użytkownikom**. (Pozostaw inne ustawienia). Wybierz pozycję **Zapisz**.

   ![Ekran zgody Google OAuth](media/google-federation/google-oauth-consent-screen.png)

5. Wybierz **poświadczenia** kartę. W **Utwórz poświadczenia** menu, wybierz **identyfikator klienta OAuth**.

   ![Poświadczenia interfejsu API Google](media/google-federation/google-api-credentials.png)

6. W obszarze **typ aplikacji**, wybierz **aplikacji sieci Web**, a następnie w obszarze **identyfikatory URI przekierowania autoryzowanych**, wprowadź następujące identyfikatory URI:
   - `https://login.microsoftonline.com` 
   - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>(gdzie `<directory id>` jest Twój identyfikator katalogu)
   
    > [!NOTE]
    > Aby znaleźć swój identyfikator katalogu, przejdź do https://portal.azure.com, a następnie w obszarze **usługi Azure Active Directory**, wybierz **właściwości** i skopiuj **identyfikator katalogu**.

   ![Utwórz identyfikator klienta OAuth](media/google-federation/google-create-oauth-client-id.png)

7. Wybierz pozycję **Utwórz**. Skopiuj identyfikator klienta i klucz tajny klienta, która będzie używana podczas dodawania dostawcy tożsamości w portalu usługi Azure AD.

   ![OAuth klienta identyfikator i klucz tajny klienta](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>Krok 2: Konfigurowanie federacyjnego Google w usłudze Azure AD 
Teraz będzie ustawisz klienta Google identyfikator i klucz tajny klienta, wprowadzając ją w portalu usługi Azure AD lub przy użyciu programu PowerShell. Należy przetestować konfiguracji Federacji Google zapraszając samodzielnie przy użyciu adresu Gmail i podjęcie próby, aby zrealizować zaproszenia za pomocą konta Google, które otrzymało zaproszenie. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>W celu skonfigurowania Federacji Google w portalu usługi Azure AD 
1. Przejdź do witryny [Azure Portal](https://portal.azure.com). W lewym okienku wybierz pozycję **Azure Active Directory**. 
2. Wybierz **relacje w organizacji**.
3. Wybierz **dostawców tożsamości**, a następnie kliknij przycisk **Google** przycisku.
4. Wprowadź nazwę. Następnie wprowadź identyfikator klienta i klucz tajny klienta, który został uzyskany wcześniej. Wybierz pozycję **Zapisz**. 

   ![Dodawanie dostawcy tożsamości Google](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>W celu skonfigurowania Federacji Google przy użyciu programu PowerShell
1. Zainstaluj najnowszą wersję programu Azure AD PowerShell dla modułu programu Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Uruchom następujące polecenie: `Connect-AzureAD`.
3. W wierszu logowania Zaloguj się przy użyciu zarządzanego konta administratora globalnego.  
4. Uruchom następujące polecenie: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > Użyj identyfikatora klienta i wpisu tajnego z aplikacji utworzonej w klienta "krok 1: Konfigurowanie projektu programu Google developer." Aby uzyskać więcej informacji, zobacz [New AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview) artykułu. 
 
## <a name="how-do-i-remove-google-federation"></a>Jak usunąć Federacji Google?
Możesz usunąć konfigurację Federacji Google. Jeśli tak zrobisz, Google użytkowników-gości, którzy już wykorzystana zaproszenia nie będą mogli logować, ale możesz udzielić ich dostępu do zasobów ponownie przez usunięcie ich z katalogu i ponownie zapraszając ich. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>Do usunięcia federacyjny Google w portalu usługi Azure AD: 
1. Przejdź do witryny [Azure Portal](https://portal.azure.com). W lewym okienku wybierz pozycję **Azure Active Directory**. 
2. Wybierz **relacje w organizacji**.
3. Wybierz **dostawców tożsamości**, a następnie kliknij przycisk **Google** przycisku.
4. Wybierz **Google**, a następnie wybierz pozycję **Usuń**. 
   
   ![Usunięto społecznościowego dostawcy tożsamości](media/google-federation/google-social-identity-providers.png)

1. Wybierz **tak** aby potwierdzić usunięcie. 

### <a name="to-delete-google-federation-by-using-powershell"></a>Do usunięcia federacyjny Google przy użyciu programu PowerShell: 
1. Zainstaluj najnowszą wersję programu Azure AD PowerShell dla modułu programu Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Uruchom polecenie `Connect-AzureAD`.  
4. Podczas logowania w wierszu polecenia Zaloguj się przy użyciu zarządzanego konta administratora globalnego.  
5. Wprowadź następujące polecenie:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Aby uzyskać więcej informacji, zobacz [AzureADMSIdentityProvider Usuń](https://docs.microsoft.com/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview). 