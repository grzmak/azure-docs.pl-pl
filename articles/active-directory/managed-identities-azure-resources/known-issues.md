---
title: Często zadawane pytania i znane problemy związane z zarządzanych tożsamości dla zasobów platformy Azure
description: Znane problemy związane z zarządzanych tożsamości dla zasobów platformy Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: 2a759aea4288af2e90335b47244408d6a537e24b
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44295585"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>Często zadawane pytania i znane problemy związane z zarządzanych tożsamości dla zasobów platformy Azure

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Często zadawane pytania

> [!NOTE]
> Zarządzane tożsamości dla zasobów platformy Azure to nowa nazwa usługi, znana wcześniej jako tożsamość usługi zarządzanej (MSI).

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Zarządzanych tożsamości dla zasobów platformy Azure działa z usługami w chmurze platformy Azure?

Nie, nie ma żadnych planów, aby obsługiwać zarządzanych tożsamości na potrzeby zasobów platformy Azure w usługach Azure Cloud Services.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Zarządzanych tożsamości dla zasobów platformy Azure działa z Active Directory Authentication Library (ADAL) lub biblioteka Microsoft Authentication Library (MSAL)?

Dla zasobów platformy Azure nie jest jeszcze zintegrowana z biblioteką ADAL lub biblioteki MSAL, nie, zarządzanych tożsamości. Aby uzyskać więcej informacji na temat pozyskiwania token dla zarządzanych tożsamości dla zasobów platformy Azure przy użyciu punktu końcowego REST, zobacz [jak uzyskiwanie tokenu dostępu, za pomocą tożsamości zarządzanych zasobów platformy Azure na Maszynie wirtualnej platformy Azure ](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>Co to jest granicy zabezpieczeń z zarządzanych tożsamości dla zasobów platformy Azure?

Granicy zabezpieczeń tożsamości jest zasób, do której jest dołączony do. Na przykład granice zabezpieczeń dla maszyny wirtualnej z zarządzanych tożsamości dla zasobów platformy Azure jest włączone, maszyna wirtualna. Każdy kod uruchomiony na tej maszynie Wirtualnej jest w stanie wywołać zarządzanych tożsamości dla zasobów platformy Azure z punktu końcowego i żądania tokenów. Jest podobnie środowisko z innymi zasobami, które obsługują zarządzanych tożsamości dla zasobów platformy Azure.

### <a name="should-i-use-the-managed-identities-for-azure-resources-vm-imds-endpoint-or-the-vm-extension-endpoint"></a>Dla punktu końcowego maszyny Wirtualnej IMDS zasobów platformy Azure lub punktu końcowego z rozszerzenia maszyny Wirtualnej należy używać zarządzanych tożsamości?

Korzystając z zarządzanych tożsamości dla zasobów platformy Azure z maszynami wirtualnymi, firma Microsoft zachęca do endpoint IMDS zasobów platformy Azure przy użyciu zarządzanych tożsamości. Azure Instance Metadata Service jest punkt końcowy REST dostępne dla wszystkich maszyn wirtualnych IaaS utworzone za pomocą usługi Azure Resource Manager. Korzyści z używania zarządzanych tożsamości dla zasobów platformy Azure za pośrednictwem IMDS, należą:

1. Wszystkie systemy operacyjne obsługiwane modelu IaaS platformy Azure mogą używać zarządzanych tożsamości dla zasobów platformy Azure za pośrednictwem IMDS. 
2. Nie musisz zainstalować rozszerzenie na maszynie Wirtualnej, aby umożliwić zarządzanych tożsamości dla zasobów platformy Azure. 
3. Certyfikaty używane przez zarządzanych tożsamości dla zasobów platformy Azure nie są już dostępne na maszynie wirtualnej. 
4. Punkt końcowy IMDS jest dobrze znanego nierutowalny adresu IP, dostępne tylko z poziomu maszyny Wirtualnej. 

Zarządzanych tożsamości dla rozszerzenia maszyny Wirtualnej zasoby platformy Azure jest nadal dostępne do użycia oprogramowania; jednak pory firma Microsoft będzie domyślnie przy użyciu punktu końcowego IMDS. Zarządzanych tożsamości dla rozszerzenia maszyny Wirtualnej zasoby platformy Azure zostaną wycofane w styczniu 2019 r. 

Aby uzyskać więcej informacji na temat usługi Azure Instance Metadata Service, zobacz [IMDS dokumentacji](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Co to są obsługiwane dystrybucje systemu Linux?

Wszystkie dystrybucje systemu Linux obsługiwane przez IaaS platformy Azure może służyć z zarządzanych tożsamości dla zasobów platformy Azure za pośrednictwem punktu końcowego IMDS. 

Uwaga: Zarządzanych tożsamości dla zasobów platformy Azure rozszerzenia maszyny Wirtualnej (zaplanowane do wycofania z użycia w styczniu 2019) obsługuje tylko poniższe dystrybucje systemu Linux:
- Stabilny systemu CoreOS
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Inne dystrybucje systemu Linux nie są obecnie obsługiwane, a rozszerzenie może zakończyć się niepowodzeniem w nieobsługiwanych dystrybucjach.

To rozszerzenie działa na 6,9 CentOS. Jednak ze względu na brak obsługi systemu w 6,9, rozszerzenie nie automatycznie ponownego uruchomienia jeśli wystąpiła awaria lub zatrzymana. Ponownego uruchomienia po ponownym uruchomieniu maszyny Wirtualnej. Aby ręcznie uruchomić ponownie rozszerzenie, zobacz [jak jest ponownym zarządzanych tożsamości dla rozszerzenia zasobów platformy Azure?](#how-do-you-restart-the-managed-identities-for-Azure-resources-extension)

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>Jak ponownym zarządzanych tożsamości dla rozszerzenia zasobów platformy Azure?
W systemie Windows i niektóre wersje systemu Linux Jeśli rozszerzenie zostanie zatrzymana, następujące polecenie cmdlet może służyć do ręcznie uruchomić ponownie:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Gdzie: 
- Rozszerzenie nazwy i typu dla Windows: ManagedIdentityExtensionForWindows
- Rozszerzenie nazwy i typu dla systemu Linux jest: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Znane problemy

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Skrypt automatyzacji" kończy się niepowodzeniem podczas próby eksport schematu dla zarządzanych tożsamości dla rozszerzenia zasobów platformy Azure

Po włączeniu zarządzanych tożsamości dla zasobów platformy Azure na maszynie Wirtualnej następujący błąd jest wyświetlany, gdy próba użycia funkcji "Skrypt automatyzacji" dla maszyny Wirtualnej lub jej grupy zasobów:

![Błąd eksportowania zarządzanych tożsamości dla zasobów platformy Azure, skrypt automatyzacji](./media/msi-known-issues/automation-script-export-error.png)

Zarządzanych tożsamości dla zasobów platformy Azure, które rozszerzenia maszyny Wirtualnej (zaplanowane do wycofania z użycia w styczniu 2019) jest obecnie nie obsługuje możliwość eksportowania jego schematu do szablonu grupy zasobów. W rezultacie w wygenerowany szablon nie są wyświetlane parametry konfiguracji, aby umożliwić zarządzanych tożsamości dla zasobów platformy Azure w zasobie. Poniższe sekcje mogą być dodawane ręcznie, wykonując na potrzeby przykładów w [Konfigurowanie zarządzanych tożsamości dla zasobów platformy Azure na Maszynie wirtualnej platformy Azure przy użyciu szablonów](qs-configure-template-windows-vm.md).

Gdy funkcja eksportu schematu stają się dostępne dla zarządzanych tożsamości dla rozszerzenia maszyny Wirtualnej zasoby platformy Azure (planowana do wycofania z użycia w styczniu 2019), będzie ono wyświetlane w [eksportowanie grupy zasobów zawierające rozszerzeń maszyn wirtualnych](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Blok konfiguracji nie są wyświetlane w witrynie Azure portal

Jeśli nie ma bloku konfiguracji maszyny Wirtualnej na maszynie Wirtualnej, następnie zarządzanych tożsamości dla zasobów platformy Azure nie zostało włączone w portalu w Twoim regionie jeszcze.  Sprawdź ponownie później.  Można również włączyć zarządzanych tożsamości dla zasobów platformy Azure dla maszyny Wirtualnej przy użyciu [PowerShell](qs-configure-powershell-windows-vm.md) lub [wiersza polecenia platformy Azure](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Nie można przypisać dostępu do maszyn wirtualnych w bloku kontrola dostępu (IAM)

Jeśli **maszyny wirtualnej** nie jest wyświetlany w witrynie Azure portal jako wyborem dla **Przypisz dostęp do** w **kontrola dostępu (IAM)** > **Dodaj uprawnienia**, a następnie zarządzanych tożsamości dla zasobów platformy Azure nie został włączony w portalu w Twoim regionie jeszcze. Sprawdź ponownie później.  Nadal można wybrać tożsamości dla maszyny Wirtualnej na potrzeby przypisania roli przez wyszukiwanie zarządzanych tożsamości dla zasobów platformy Azure nazwy głównej usługi.  Wprowadź nazwę maszyny Wirtualnej w **wybierz** pola i nazwę główną usługi, zostanie wyświetlony w wynikach wyszukiwania.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Maszyna wirtualna nie została uruchomiona po jest przenoszony z grupy zasobów lub subskrypcji

Jeśli przenosisz Maszynę wirtualną w stanie uruchomienia, nadal działa podczas przenoszenia. Jednak po przeniesieniu, jeśli maszyna wirtualna zostanie zatrzymana i uruchomiona ponownie, zakończy się niepowodzeniem do uruchomienia. Ten problem występuje, ponieważ maszyna wirtualna nie aktualizuje odwołanie do zarządzanych tożsamości dla tożsamości zasobów platformy Azure i wskaż w starej grupy zasobów w dalszym ciągu.

**Obejście problemu** 
 
Wyzwalanie aktualizacji na maszynie Wirtualnej, dzięki czemu go uzyskać prawidłowe wartości dla zarządzanych tożsamości dla zasobów platformy Azure. Możesz tworzyć zmiany właściwości maszyny Wirtualnej, można zaktualizować odwołania do zarządzanych tożsamości dla tożsamości zasobów platformy Azure. Na przykład można ustawić nową wartość tagu na maszynie Wirtualnej za pomocą następującego polecenia:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
To polecenie ustawia nowy tag "fixVM" o wartości 1 na maszynie Wirtualnej. 
 
Przez ustawienie tej właściwości, maszyna wirtualna zostaje zaktualizowana o poprawne zarządzanych tożsamości dla identyfikatora URI zasobu zasobów platformy Azure, a następnie powinno być możliwe do uruchomienia maszyny Wirtualnej. 
 
Gdy maszyna wirtualna jest uruchomiona, można usunąć tagu za pomocą następującego polecenia:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-identities"></a>Znane problemy związane z tożsamości przypisanych przez użytkownika

- przydziały tożsamości przypisanych przez użytkownika są dostępne tylko dla maszyny Wirtualnej i zestawu skalowania maszyn wirtualnych. Ważne: przypisania tożsamości przypisanych przez użytkownika zmieni się w ciągu najbliższych miesięcy.
- Zduplikowane tożsamości przypisanych przez użytkownika na tym samym VM/VMSS, spowoduje, że VM/VMSS nie powiedzie się. Dotyczy to również tożsamości, które są dodawane z inną wielkością liter. np. MyUserAssignedIdentity i myuserassignedidentity. 
- Inicjowanie obsługi rozszerzenia maszyny Wirtualnej (zaplanowane do wycofania z użycia w 2019 styczeń) do maszyny Wirtualnej może zakończyć się niepowodzeniem z powodu błędów wyszukiwania DNS. Uruchom ponownie maszynę Wirtualną i spróbuj ponownie. 
- Dodawanie tożsamości przypisanych przez użytkownika "nieistniejącej" spowoduje, że maszyna wirtualna może się nie powieść. 
- Tworzenie tożsamości przypisanych przez użytkownika przy użyciu znaków specjalnych (np. podkreślenie) w nazwie, nie jest obsługiwane.
- nazwy tożsamości przypisanych przez użytkownika są ograniczone do 24 znaków w scenariuszu typu end to end. tożsamości przypisanych przez użytkownika z nazwami dłuższe niż 24 znaki zakończy się niepowodzeniem do przypisania.
- W przypadku korzystania z tożsamości zarządzanej rozszerzenia maszyny wirtualnej (zaplanowane do wycofania z użycia w styczniu 2019) obsługiwany limit jest 32 przypisanych do użytkowników zarządzanych tożsamości. Bez rozszerzenia tożsamości zarządzanej maszyny wirtualnej i obsługiwany limit to 512.  
- Podczas dodawania drugiego tożsamości przypisanych przez użytkownika, identyfikator ClientID, który mogą być niedostępne do żądania tokenów dla rozszerzenia maszyny Wirtualnej. Środki zaradcze Uruchom ponownie zarządzanych tożsamości dla rozszerzenia maszyny Wirtualnej zasoby platformy Azure przy użyciu poniższych poleceń powłoki bash dwa:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Gdy maszyna wirtualna ma tożsamości przypisanych przez użytkownika, ale Brak tożsamości przypisanych przez system, portalu, do którego będą wyświetlane w interfejsie użytkownika zarządzanych tożsamości dla zasobów platformy Azure jako wyłączone. Aby włączyć tożsamości przypisanych przez system, należy użyć szablonu usługi Azure Resource Manager, interfejsu wiersza polecenia platformy Azure lub zestawu SDK.
