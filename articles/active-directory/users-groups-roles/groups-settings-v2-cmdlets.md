---
title: Przykłady programu PowerShell do zarządzania grupami w usłudze Azure Active Directory | Dokumentacja firmy Microsoft
description: Ta strona zawiera przykłady programu PowerShell ułatwiające zarządzanie grupami w usłudze Azure Active Directory
keywords: Usługa Azure AD i Azure Active Directory, programu PowerShell, grupy, grupa zarządzania
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/07/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: d6fb5a97ef573a35f335875beddc7752f580bec1
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296648"
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Usługi Active Directory w wersji 2 poleceń cmdlet platformy Azure do zarządzania grupami
> [!div class="op_single_selector"]
> * [Azure Portal](../fundamentals/active-directory-groups-create-azure-portal.md)
> * [Program PowerShell](groups-settings-v2-cmdlets.md)
>
>

Ten artykuł zawiera przykładowe sposoby zarządzania grupami w usłudze Azure Active Directory (Azure AD) przy użyciu programu PowerShell.  Również jak można skonfigurować przy użyciu modułu Azure AD PowerShell. Najpierw musisz [pobieranie modułu programu PowerShell usługi Azure AD](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>Instalowanie modułu Azure AD PowerShell
Aby zainstalować moduł programu Azure AD PowerShell, użyj następujących poleceń:

    PS C:\Windows\system32> install-module azuread
    PS C:\Windows\system32> import-module azuread

Aby sprawdzić, czy moduł jest gotowy do użycia, użyj następującego polecenia:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Teraz można uruchomić przy użyciu poleceń cmdlet w module. Pełny opis poleceń cmdlet w module usługi Azure AD, można znaleźć w dokumentacji online dla [usługi Azure Active Directory PowerShell w wersji 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connect-to-the-directory"></a>Łączenie się z katalogiem
Przed rozpoczęciem, Zarządzanie grupami przy użyciu poleceń cmdlet programu PowerShell usługi Azure AD, musisz połączyć sesji programu PowerShell do katalogu, w którym chcesz zarządzać. Użyj następującego polecenia:

    PS C:\Windows\system32> Connect-AzureAD

Polecenie cmdlet wyświetli monit o podanie poświadczeń, których chcesz użyć do dostępu do katalogu. W tym przykładzie używamy karen@drumkit.onmicrosoft.com uzyskanie dostępu do katalogu demonstracyjnych. Polecenie cmdlet zwraca potwierdzenie, aby pokazać, że sesja została pomyślnie połączono z katalogiem:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Teraz można uruchomić przy użyciu poleceń cmdlet programu Azure AD do zarządzania grupami w Twoim katalogu.

## <a name="retrieve-groups"></a>Pobieranie grup
Aby pobrać istniejących grup z katalogu, użyj polecenia cmdlet Get-AzureADGroups. 

Aby pobrać wszystkie grupy w katalogu, użyj polecenia cmdlet bez parametrów:

    PS C:\Windows\system32> get-azureadgroup

Polecenie cmdlet zwraca wszystkie grupy w połączonym katalogu.

Można pobrać określonej grupy, dla którego należy określić identyfikator obiektu grupy, można użyć parametru - identyfikator obiektu:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Polecenie cmdlet zwraca teraz grupy, w których objectID pasuje do wartości parametru, który wprowadzono:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Można wyszukać określoną grupę za pomocą parametru - filter. Ten parametr przyjmuje klauzuli filtru ODATA i zwraca wszystkie grupy, które są zgodne z filtrem, jak w poniższym przykładzie:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> Polecenia cmdlet programu PowerShell usługi Azure AD implementuje standardowe zapytania OData. Aby uzyskać więcej informacji, zobacz **$filter** w [opcje zapytania systemu OData przy użyciu punktu końcowego OData](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="create-groups"></a>Tworzenie grup
Aby utworzyć nową grupę w katalogu, należy użyć polecenia cmdlet New-AzureADGroup. To polecenie cmdlet tworzy nową grupę zabezpieczeń o nazwie "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="update-groups"></a>Grupy aktualizacji
Aby zaktualizować istniejącą grupę, użyj polecenia cmdlet Set-AzureADGroup. W tym przykładzie Zmieniamy Właściwość DisplayName grupy "Administratorzy usługi Intune". Po pierwsze są znajdowane grupy za pomocą polecenia cmdlet Get-AzureADGroup są i filtrować przy użyciu atrybutu DisplayName:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Następnie Zmieniamy właściwość Description na nową wartość "Administratorzy usługi Intune urządzenie":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Teraz Jeśli możemy znaleźć grupy widzimy, że właściwość Description zostanie zaktualizowany w celu odzwierciedlenia nowej wartości:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="delete-groups"></a>Usuń grupy
Można usunąć grup z katalogu, użyj polecenia cmdlet Remove-AzureADGroup w następujący sposób:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="manage-group-membership"></a>Zarządzaj członkostwem w grupach 
### <a name="add-members"></a>Dodaj członków
Aby dodać nowych członków do grupy, należy użyć polecenia cmdlet Add-AzureADGroupMember. To polecenie dodaje członka do grupy Administratorzy usługi Intune, która była używana w poprzednim przykładzie:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr — identyfikator obiektu jest identyfikator obiektu grupy, do której chcesz dodać element członkowski, a RefObjectId — jest identyfikator obiektu użytkownika, którego chcesz dodać jako członka do grupy.

### <a name="get-members"></a>Pobieranie członków
Aby uzyskać członków istniejącej grupy, użyj polecenia cmdlet Get-AzureADGroupMember, jak w poniższym przykładzie:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

### <a name="remove-members"></a>Usuń członków
Aby usunąć element członkowski, który wcześniej dodane do grupy, użyj polecenia cmdlet Remove-AzureADGroupMember, jak to pokazano poniżej:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

### <a name="verify-members"></a>Sprawdź elementy członkowskie
Aby sprawdzić członkostwa w grupach użytkownika, użyj polecenia cmdlet Select AzureADGroupIdsUserIsMemberOf. To polecenie cmdlet przyjmuje jako parametry, identyfikator obiektu użytkownika, do których chcesz sprawdzić członkostwa w grupach i lista grup, do których chcesz sprawdzić członkostwa. Lista grup musi być podany w postaci złożone zmienną typu "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", więc możemy najpierw utworzyć zmienną z tym typem:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Następnie należy podać wartości identyfikatory GroupID sprawdzić w atrybucie "identyfikatory", GroupID tej zmiennej złożonych:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Teraz Jeśli chcemy sprawdzić członkostwa w grupach użytkownika z 72cd4bbd-2594-40a2-935c-016f3cfeeeea ObjectID dla grup w $g powinniśmy skorzystać:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Wartość zwracana jest lista grup, których członkiem jest ten użytkownik. Można także zastosować tę metodę w celu sprawdzania członkostwa kontakty, grupy lub jednostki usługi dla danej listy grup, za pomocą AzureADGroupIdsContactIsMemberOf Select, Select AzureADGroupIdsGroupIsMemberOf lub Wybierz AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="disable-group-creation-by-your-users"></a>Wyłącz tworzenie grupy użytkowników
Aby uniemożliwić użytkownikom niebędącym administratorami utworzenie grup zabezpieczeń. Domyślne zachowanie w Microsoft Online Directory usługi MSODS () jest umożliwienie użytkownikom bez uprawnień administratora na tworzenie grup, czy Samodzielne Zarządzanie grupami samoobsługi jest włączona. Ustawienie SSGM kontroluje zachowanie tylko w panelu dostępu Moje aplikacje. 

Aby wyłączyć tworzenie grupy dla użytkowników bez uprawnień administratora:

1. Sprawdź, czy użytkownicy niebędący administratorami mogą tworzyć grupy:
   
  ````
  PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
  ````
  
2. Jeśli zostanie zwrócona `UsersPermissionToCreateGroupsEnabled : True`, a następnie użytkownicy niebędący administratorami mogą tworzyć grupy. Aby wyłączyć tę funkcję:
  
  ```` 
  Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
  ````
  
## <a name="manage-owners-of-groups"></a>Zarządzenie właścicielami grup
Aby dodać właścicieli do grupy, użyj polecenia cmdlet Add-AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametr — identyfikator obiektu jest identyfikator obiektu grupy, do której chcemy dodać właściciela, a RefObjectId — jest identyfikator obiektu użytkownika, którego chcesz dodać jako właściciela grupy.

Aby pobrać właścicieli grupy, użyj polecenia cmdlet Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Polecenie cmdlet zwraca listę wszystkich właścicieli dla określonej grupy:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Jeśli chcesz usunąć właściciela z grupy, użyj polecenia cmdlet Remove-AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Aliasy zarezerwowane 
Po utworzeniu grupy pewność, że punkty końcowe umożliwiają użytkownikom końcowym określić mailNickname lub alias ma być używany jako część adresu e-mail grupy. Grupy z następujących aliasów e-mail o wysokim poziomie uprawnień można tworzyć tylko przez administratora globalnego usługi Azure AD. 
  
* nadużyć 
* Administrator 
* administrator 
* hostmaster 
* majordomo 
* Postmaster 
* Główny 
* Zabezpieczanie 
* security 
* Administrator protokołu SSL 
* webmastera 

## <a name="next-steps"></a>Kolejne kroki
Można znaleźć więcej dokumentacji usługi Azure Active Directory PowerShell w [poleceń cmdlet usługi Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Zarządzanie dostępem do zasobów za pomocą grup usługi Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
