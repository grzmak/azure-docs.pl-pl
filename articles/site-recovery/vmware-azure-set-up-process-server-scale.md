---
title: Konfigurowanie serwera przetwarzania na platformie Azure dla maszyny Wirtualnej VMware i serwera fizycznego powrotu po awarii przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft
description: W tym artykule opisano sposób konfigurowania serwera przetwarzania na platformie Azure, powrót po awarii maszyn wirtualnych platformy Azure do programu VMware.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 5e0e4646b14a5bff5c40d0817ab8ecccf1046ea9
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077769"
---
# <a name="set-up-additional-process-servers-for-scalability"></a>Konfigurowanie dodatkowych serwerów przetwarzania w przypadku skalowalności

Domyślnie, Jeśli replikujesz maszyny wirtualne VMware lub serwery fizyczne do platformy Azure za pomocą [Site Recovery](site-recovery-overview.md), serwer przetwarzania jest zainstalowany na komputerze z serwerem konfiguracji i służy do koordynowania innych transfer danych między Site Recovery i infrastruktury lokalnej. Aby zwiększyć wydajność i skalowanie w poziomie wdrożenie replikacji, możesz dodać autonomicznego dodatkowych serwerów przetwarzania. W tym artykule opisano, jak to zrobić.

## <a name="before-you-start"></a>Przed rozpoczęciem

### <a name="capacity-planning"></a>Planowanie pojemności

Upewnij się, że zostały wykonane [planowania pojemności](site-recovery-plan-capacity-vmware.md) potrzeby replikacji oprogramowania VMware. Pomaga to identyfikować jak i kiedy należy wdrażać dodatkowych serwerów przetwarzania.

### <a name="sizing-requirements"></a>Wymagania w zakresie rozmiaru 

Sprawdź wymagania w zakresie rozmiaru podsumowane w tabeli. Ogólnie rzecz biorąc Jeśli musisz skalować wdrożenie do ponad 200 maszyn źródłowych lub masz łącznie codziennie współczynnik więcej niż 2 TB do zmian danych, potrzebujesz dodatkowych serwerów przetwarzania do obsługi natężenia ruchu.

| **Dodatkowym serwerze przetwarzania** | **Rozmiar dysku w pamięci podręcznej** | **Współczynnik zmian danych** | **Chronione maszyny** |
| --- | --- | --- | --- |
|4 Vcpu (2 sockets * 2 rdzenie \@ 2,5 GHz), 8 GB pamięci RAM |300 GB |250 GB lub mniej |Replikowanie maszyn 85 lub mniej. |
|8 wirtualnych procesorów CPU (2 sockets * 4 rdzenie \@ 2,5 GHz), 12 GB pamięci RAM |600 GB |250 GB do 1 TB |Replikacja między maszynami 85 150. |
|12 procesorów wirtualnych Vcpu (2 sockets * 6 rdzeni \@ 2,5 GHz) 24 GB pamięci RAM |1 TB |1 TB do 2 TB |Replikacja między maszynami 150 225. |

W przypadku, gdy każda chronione maszyny źródłowej skonfigurowano 3 dyskami 100 GB.

### <a name="prerequisites"></a>Wymagania wstępne

Wymagania wstępne dotyczące dodatkowym serwerze przetwarzania są podsumowane w poniższej tabeli.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]


## <a name="download-installation-file"></a>Pobierz plik instalacyjny

Pobierz plik instalacyjny na serwer przetwarzania w następujący sposób:

1. Zaloguj się do witryny Azure portal i przejdź do magazynu usługi Recovery Services.
2. Otwórz **infrastruktura usługi Site Recovery** > **VMWare i maszyn fizycznych** > **serwery konfiguracji** (w ramach programu VMware i fizycznych Maszyny).
3. Wybierz serwer konfiguracji, aby przejść do szczegółów serwera. Następnie kliknij przycisk **+ serwer przetwarzania**.
4. W **serwer Dodaj przetwarzania** >  **wybierz, w której chcesz wdrożyć serwer przetwarzania**, wybierz opcję **Deploy Scale-out serwera przetwarzania w środowisku lokalnym**.

  ![Dodaj stronę serwerów](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Kliknij przycisk **pobrać z witryny Microsoft Azure Recovery ujednoliconej konfiguracji**. Spowoduje to pobranie najnowszej wersji pliku instalacyjnego.

  > [!WARNING]
  Wersja instalacji serwera przetwarzania powinna być taka sama jak, lub starszej niż, wersja serwera konfiguracji zostały uruchomione. Prosty sposób, aby zapewnić zgodność wersji jest użycie tego samego Instalatora, który ostatnio używane do instalowania lub aktualizowania serwera konfiguracji.

## <a name="install-from-the-ui"></a>Zainstaluj w interfejsie użytkownika

Należy zainstalować w następujący sposób. Po skonfigurowaniu serwera, należy przeprowadzić migrację maszyn źródłowych, które z niej korzystać.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Instalowanie przy użyciu wiersza polecenia

Zainstaluj, uruchamiając następujące polecenie:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Gdzie parametry wiersza polecenia są następujące:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Na przykład:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Tworzenie pliku ustawień serwera proxy

Jeśli potrzebujesz skonfigurować serwer proxy, parametr ProxySettingsFilePath przyjmuje plik jako dane wejściowe. Można utworzyć pliku w następujący sposób i przekaż go jako parametr wejściowy ProxySettingsFilePath.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Kolejne kroki
Dowiedz się więcej o [Zarządzanie przetwarzania ustawień serwera](vmware-azure-manage-process-server.md)
