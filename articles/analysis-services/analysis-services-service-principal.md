---
title: Automatyzowanie zadań usług Azure Analysis Services przy użyciu jednostki usługi | Dokumentacja firmy Microsoft
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4e27c21b3b8d84b658b4cad10e05f8fa0b057268
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427319"
---
# <a name="automation-with-service-principals"></a>Automatyzacja przy użyciu jednostki usługi

Jednostki usług to zasoby aplikacji usługi Azure Active Directory tworzone w ramach dzierżawy w celu przeprowadzania nienadzorowanych operacji na poziomie zasobu lub usługi. Są one unikatowego typu *tożsamość użytkownika* przy użyciu Identyfikatora aplikacji i hasło lub certyfikat. Jednostka usługi ma tylko uprawnienia niezbędne do wykonywania zadań określone przez role i uprawnienia, dla których jest przypisany. 

W usługach Analysis Services jednostki usługi są używane za pomocą usługi Azure Automation, PowerShell w trybie nienadzorowanym, niestandardowych aplikacji klienckich i aplikacji sieci web do automatyzacji typowych zadań. Na przykład serwery aprowizacji, wdrażanie modeli, odświeżanie danych, skalowanie w górę/w dół, a wstrzymywanie i wznawianie wszystkich można zautomatyzować za pomocą jednostki usługi. Uprawnienia są przypisywane do jednostek usług za pośrednictwem członkostwo w roli, podobnie jak regularne konta usługi Azure AD głównej nazwy użytkownika.

## <a name="create-service-principals"></a>Tworzenie jednostek usługi
 
Nazwy główne usług mogą być tworzone w witrynie Azure portal lub przy użyciu programu PowerShell. Aby dowiedzieć się więcej, zobacz:

[Tworzenie jednostki usługi — witryna Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Tworzenie jednostki usługi — PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Store zasoby poświadczeń i certyfikatów w usłudze Azure Automation

Poświadczenia nazwy głównej usługi i certyfikaty mogą być przechowywane bezpiecznie w usłudze Azure Automation runbook operacji. Aby dowiedzieć się więcej, zobacz:

[Zasoby poświadczeń w usłudze Azure Automation](../automation/automation-credentials.md)   
[Zasoby certyfikatów w usłudze Azure Automation](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Dodawanie jednostki usługi do roli administratora serwera

Zanim użyjesz nazwy głównej usługi dla operacji zarządzania serwerem usług Analysis Services, należy dodać go do roli Administratorzy serwera. Aby dowiedzieć się więcej, zobacz [Dodawanie jednostki usługi do roli administratora serwera](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Nazwy główne usług w parametrach połączenia

Identyfikator aplikacji nazwy głównej usługi i hasło lub certyfikat, które mogą być używane w ciągów połączenia, bardzo podobne do nazwy UPN.

### <a name="powershell"></a>PowerShell

W przypadku używania nazwy głównej usługi dla zasobu operacje zarządzania za pomocą [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) moduł, użyj `Login-AzureRmAccount` polecenia cmdlet. W przypadku używania nazwy głównej usługi dla operacji serwera przy użyciu [SQLServer](https://www.powershellgallery.com/packages/SqlServer) moduł, użyj `Add-AzureAnalysisServicesAccount` polecenia cmdlet. 

W poniższym przykładzie appID i hasło są używane do wykonywania operacji odświeżania bazy danych modelu:

```PowerShell
Param (

        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Add-AzureAnalysisServicesAccount -Credential $Credential -ServicePrincipal -TenantId $TenantId -RolloutEnvironment "westcentralus.asazure.windows.net"

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full"
```

### <a name="amo-and-adomd"></a>AMO i ADOMD 

Podczas nawiązywania połączenia z aplikacji sieci web i aplikacje klienckie [AMO i ADOMD biblioteki klienckie](analysis-services-data-providers.md) wersji 15.0.2 i wyższych pakiety do zainstalowania z pakietu NuGet obsługuje nazwy główne usług w parametrach połączenia przy użyciu następującej składni: `app:AppID` i hasło lub `cert:thumbprint`. 

W poniższym przykładzie `appID` i `password` są używane do wykonywania operacji odświeżania bazy danych modelu:

```C#
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>Kolejne kroki
[Zaloguj się przy użyciu programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Dodawanie jednostki usługi do roli administratora serwera](analysis-services-addservprinc-admins.md)   