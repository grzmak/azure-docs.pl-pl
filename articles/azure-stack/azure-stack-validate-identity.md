---
title: Weryfikowanie tożsamości platformy Azure dla usługi Azure Stack | Dokumentacja firmy Microsoft
description: Użyj sprawdzania gotowości stosu Azure do weryfikowania tożsamości platformy Azure.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 9c7ac89d1f12e8ec033b201f2c2dd845c11486e2
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077821"
---
# <a name="validate-azure-identity"></a>Weryfikowanie tożsamości platformy Azure 
Aby sprawdzić, czy usługi Azure Active Directory (Azure AD) jest gotowa do użycia z usługą Azure Stack, należy użyć narzędzia narzędzie do sprawdzania gotowości usługi Azure Stack (AzsReadinessChecker). Weryfikowanie rozwiązań tożsamość w systemie Azure, przed rozpoczęciem wdrażania usługi Azure Stack.  

Narzędzie sprawdzania gotowości weryfikuje:
 - Azure Active Directory (Azure AD) jako dostawcy tożsamości dla usługi Azure Stack.
 - Konto usługi Azure AD, którego chcesz użyć mogą logować się jako administrator globalny usługi Azure Active Directory. 

Sprawdzanie poprawności gwarantuje, że środowisko jest gotowe dla usługi Azure Stack do przechowywania informacji o użytkownikach, aplikacje, grupy i jednostki usługi z usługi Azure Stack w usłudze Azure AD.

## <a name="get-the-readiness-checker-tool"></a>Pobierz narzędzie sprawdzania gotowości
Pobierz najnowszą wersję narzędzia narzędzie do sprawdzania gotowości usługi Azure Stack (AzsReadinessChecker) z [galerii programu PowerShell](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Wymagania wstępne
Należy spełnić następujące wymagania wstępne.

**Komputer, na którym działa narzędzie:**
 - System Windows 10 lub Windows Server 2016 z łącznością z Internetem.
 - Program PowerShell 5.1 lub nowszej. Aby sprawdzić swoją wersję, uruchom następujące polecenie programu PowerShell, a następnie przejrzyj *głównych* wersji i *pomocnicza* wersji:  

   > `$PSVersionTable.PSVersion`
 - Konfigurowanie [programu PowerShell dla usługi Azure Stack](azure-stack-powershell-install.md). 
 - Najnowszą wersję [Microsoft Azure Stack gotowości Checker](https://aka.ms/AzsReadinessChecker) narzędzia.

**Środowisko usługi Active Directory platformy Azure:**
 - Określ konto usługi Azure AD będzie używać usługi Azure Stack i upewnij się, że administrator usługi Azure Active Directory globalnego.
 - Określ nazwę Twojej dzierżawy usługi Azure AD. Nazwa dzierżawy musi być *głównej* nazwy domeny dla usługi Azure Active Directory. Na przykład *contoso.onmicrosoft.com*. 
 - Identyfikowanie AzureEnvironement użyjesz: *AzureCloud*, *AzureGermanCloud*, lub *AzureChinaCloud*.

## <a name="validate-azure-identity"></a>Weryfikowanie tożsamości platformy Azure 
1. Na komputerze, który spełnia wymagania wstępne Otwórz administracyjny wiersz polecenia PowerShell, a następnie uruchom następujące polecenie, aby zainstalować AzsReadinessChecker:  

   > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. W wierszu polecenia programu PowerShell, uruchom następujące polecenie, aby ustawić *$serviceAdminCredential* jako Administrator usługi dla swojej dzierżawy usługi Azure AD.  Zastąp *serviceadmin@contoso.onmicrosoft.com* przy użyciu swojego konta i dzierżawy. 
   > `$serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant"` 

3. W wierszu polecenia programu PowerShell uruchom następujące polecenie, aby rozpocząć sprawdzanie poprawności usługi Azure AD. 
   - Określ wartość dla AzureEnvironment jako *AzureCloud*, *AzureGermanCloud*, lub *AzureChinaCloud*.  
   - Określ nazwę dzierżawy katalogu Active Azure zastąpić *contoso.onmicrosoft.com*. 

   > `Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AADDirectoryTenantName contoso.onmicrosoft.com`
4. Po uruchomieniu narzędzia, przejrzyj dane wyjściowe. Upewnij się, ma stan **OK** zarówno dla logowania i wymagania dotyczące instalacji. Pomyślnej weryfikacji wygląda podobnie do następującego: 
 
![Sprawdzanie poprawności na przebieg](./media/azure-stack-validate-identity/validation.png)


## <a name="report-and-log-file"></a>Raport i plik dziennika
Uruchamia każdego czasu sprawdzania poprawności, rejestruje ono wyniki do **AzsReadinessChecker.log** i **AzsReadinessCheckerReport.json**. Wyświetla lokalizację tych plików z wynikami weryfikacji w programie PowerShell.

Te pliki mogą pomóc współdzielony stan sprawdzania poprawności, zanim wdrożyć usługę Azure Stack lub badania problemów weryfikacji.  Oba pliki są zachowywane wyniki każdego sprawdzenie poprawności kolejne. Raport zawiera potwierdzenie zespołu wdrażania konfiguracji tożsamości. Plik dziennika może pomóc zespołowi pomocy technicznej lub wdrożenia badać problemy ze sprawdzaniem poprawności. 

Domyślnie oba pliki są zapisywane w *C:\Users\<username > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Użyj **- OutputPath** ***&lt;ścieżki&gt;*** parametru na końcu wiersza polecenia do określenia lokalizacji inny raport.   
 - Użyj **- CleanReport** parametru na końcu polecenia uruchomienia, aby wyczyścić informacje z *AzsReadinessCheckerReport.json*.  temat poprzednie uruchomienia tego narzędzia. 

Aby uzyskać więcej informacji [raportu sprawdzania poprawności w usłudze Azure Stack](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Błędy sprawdzania poprawności
Jeśli sprawdzenie poprawności zakończy się niepowodzeniem, w oknie programu PowerShell są wyświetlane szczegóły dotyczące błędu. Narzędzie rejestruje także informacje o AzsReadinessChecker.log.

Poniższe przykłady zapewnić wskazówki dotyczące typowych błędów sprawdzania poprawności.

### <a name="expired-or-temporary-password"></a>Hasło wygasło lub tymczasowego 
 
![wygasło hasło](./media/azure-stack-validate-identity/expired-password.png)
**Przyczyna** — konta nie można zalogować, ponieważ hasło jest wygasły lub jest tymczasowe.     

**Rozpoznawanie** — w programie PowerShell uruchom następujące polecenie, a następnie postępuj zgodnie z monitami, aby zresetować hasło.  
> `Login-AzureRMAccount`

Alternatywnie, zaloguj się do https://portal.azure.com zgodnie z konta i użytkownik musi zmienić hasło.
### <a name="unknown-user-type"></a>Typ Nieznany użytkownik 
 
![Nieznany użytkownik](./media/azure-stack-validate-identity/unknown-user.png)
**Przyczyna** — konto nie może logować się do określonej usługi Azure Active Directory (AADDirectoryTenantName). W tym przykładzie *AzureChinaCloud* jest określony jako *AzureEnvironment*.

**Rozpoznawanie** — upewnij się, że konto jest nieprawidłowa dla określonego środowiska platformy Azure. W programie PowerShell, uruchom następujące polecenie, aby zweryfikować konto jest prawidłowe dla określonego środowiska: Login-AzureRmAccount-EnvironmentName AzureChinaCloud 
### <a name="account-is-not-an-administrator"></a>Konto nie jest administratorem 
 
![nie administrator](./media/azure-stack-validate-identity/not-admin.png)

**Przyczyna** — mimo że to konto można pomyślnie zalogować się, konto nie jest administratorem usługi Azure Active Directory (AADDirectoryTenantName).  

**Rozpoznawanie** — Zaloguj się do https://portal.azure.com jako konto, przejdź do **usługi Azure Active Directory** > **użytkowników** > *wybierz użytkownika*  >  **Roli w katalogu**, a następnie upewnij się, użytkownik jest **administratora globalnego**.  Jeśli konto jest użytkownik, przejdź do strony **usługi Azure Active Directory** > **domena niestandardowa** nazwy i upewnij się, że nazwa podana dla *AADDirectoryTenantName* jest oznaczone jako podstawowej nazwy domeny dla tego katalogu.  W tym przykładzie jest *contoso.onmicrosoft.com*. 

Usługa Azure Stack wymaga, że nazwa domeny jest nazwa domeny głównej.

## <a name="next-steps"></a>Następne kroki
[Sprawdź poprawność rejestracji platformy Azure](azure-stack-validate-registration.md)  
[Wyświetl raport gotowości](azure-stack-validation-report.md)  
[Zagadnienia dotyczące integracji usługi Azure Stack ogólne](azure-stack-datacenter-integration.md)  

