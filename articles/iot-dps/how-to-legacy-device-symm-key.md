---
title: Jak aprowizować starsze urządzenia z usługi Azure IoT Hub Device Provisioning za pomocą kluczy symetrycznych | Dokumentacja firmy Microsoft
description: Jak aprowizować starsze urządzenia z urządzeniem, wystąpienie usługi aprowizacji za pomocą kluczy symetrycznych
author: wesmc7777
ms.author: wesmc
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 51fea4fa1973fbe92242f1995d892cd5b038a29b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991644"
---
# <a name="how-to-provision-legacy-devices-using-symmetric-keys"></a>Jak wykonać aprowizację starsze urządzenia przy użyciu kluczy symetrycznych


Powszechny problem z wielu starszych urządzeń jest często mają tożsamość, która składa się z jednego fragmentu informacji. Informacje o tożsamości jest zwykle adres MAC lub numeru seryjnego. Starsze urządzenia może nie mieć certyfikat, modułu TPM lub innych funkcja zabezpieczeń, który może służyć do bezpiecznego identyfikowania urządzenia. Usługi Device Provisioning dla Centrum IoT hub zawiera zaświadczenie klucza symetrycznego. Zaświadczenie klucza symetrycznego może służyć do identyfikowania urządzenia na podstawie informacje, takie jak adres MAC lub numeru seryjnego.

Jeśli możesz łatwo zainstalować [sprzętowego modułu zabezpieczeń (HSM)](concepts-security.md#hardware-security-module) i certyfikat, a następnie, może być lepszym rozwiązaniem w identyfikacji i aprowizacji urządzeń. Ponieważ takie podejście może umożliwić obejścia aktualizowania kodu wdrożony do wszystkich urządzeń, a nie musiałby klucz tajny, osadzonego w obrazie urządzenia.

W tym artykule założono, ani modułu HSM lub certyfikatu to rentowną opcją. Jednak zakłada się, że masz niektóre metody aktualizacji kodu urządzenia na potrzeby udostępniania tych urządzeń do usługi Device Provisioning. 

W tym artykule założono również, że aktualizacja urządzenia ma miejsce w bezpiecznym środowisku w celu uniemożliwienia nieupoważnionego dostępu do klucza głównego grupy lub klucza pochodnego urządzenia.

Ten artykuł stanowi opracowane głównie pod kątem stację roboczą z systemem Windows. Jednak można wykonać procedury opisane w systemie Linux. Na przykład Linux, zobacz [instrukcjami aprowizacji dla wielodostępności](how-to-provision-multitenant.md).


## <a name="overview"></a>Przegląd

Identyfikator unikatowy rejestracji zostanie zdefiniowana dla każdego urządzenia, w oparciu o informacje umożliwiające identyfikację tego urządzenia. Na przykład adres MAC lub numeru seryjnego.

Grupy rejestracji, który używa [zaświadczenie klucza symetrycznego](concepts-symmetric-key-attestation.md) zostanie utworzony przy użyciu usługi Device Provisioning Service. Klucz główny grupy będzie znajdować się w grupie rejestracji. Klucza głównego będzie służyć do tworzenia skrótu każdy identyfikator rejestracji, aby wygenerować klucz urządzenia unikatowy dla każdego urządzenia. Urządzenie będzie używało tego klucza pochodnego urządzenia za pomocą jego Identyfikatora rejestracji zaświadczania z usługą Device Provisioning i można przypisać do usługi IoT hub.

Kod urządzenia, które przedstawiono w tym artykule będzie zgodna z tego samego wzorca jako [Szybki Start: Aprowizowanie symulowanego urządzenia przy użyciu kluczy symetrycznych](quick-create-simulated-device-symm-key.md). Ten kod będzie symulowania urządzenia przy użyciu przykładu z [zestawu SDK C usługi IoT Azure](https://github.com/Azure/azure-iot-sdk-c). Potwierdza symulowanego urządzenia z grupy rejestracji zamiast rejestrację indywidualną, jak pokazano w przewodniku Szybki Start.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Wymagania wstępne

* Ukończenie [skonfigurować IoT Hub Device Provisioning Service przy użyciu witryny Azure portal](./quick-setup-auto-provision.md) Szybki Start.
* Program Visual Studio 2015 lub [Visual Studio 2017](https://www.visualstudio.com/vs/) z włączonym pakietem roboczym [„Programowanie aplikacji klasycznych w języku C++”](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/).
* Zainstalowana najnowsza wersja usługi[Git](https://git-scm.com/download/).


## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Przygotuj środowisko projektowe zestawu SDK C usługi Azure IoT

W tej sekcji należy przygotować środowisko programistyczne, używany do tworzenia [zestawu SDK C usługi IoT Azure](https://github.com/Azure/azure-iot-sdk-c). 

Zestaw SDK zawiera przykładowy kod dla symulowanego urządzenia. Tego symulowanego urządzenia, zostanie podjęta próba inicjowania obsługi podczas sekwencji rozruchu urządzenia.

1. Pobierz wersję 3.11.4 [system kompilacji CMake](https://cmake.org/download/). Sprawdź pobrane dane binarne przy użyciu odpowiedniej wartości skrótu kryptograficznego. W poniższym przykładzie użyto programu Windows PowerShell do sprawdzenia wartości skrótu kryptograficznego dla wersji dystrybucji MSI 3.11.4 x64:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```
    
    Następujące wartości skrótu dla wersji 3.11.4 zostały wymienione w witrynie narzędzia CMake w momencie pisania tego dokumentu:

    ```
    6dab016a6b82082b8bcd0f4d1e53418d6372015dd983d29367b9153f1a376435  cmake-3.11.4-Linux-x86_64.tar.gz
    72b3b82b6d2c2f3a375c0d2799c01819df8669dc55694c8b8daaf6232e873725  cmake-3.11.4-win32-x86.msi
    56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869  cmake-3.11.4-win64-x64.msi
    ```

    Ważne jest, aby wstępnie wymagane składniki (program Visual Studio oraz pakiet roboczy „Programowanie aplikacji klasycznych w języku C++”) były zainstalowane na tym komputerze **przed** uruchomieniem `CMake` instalacji. Gdy wymagania wstępne zostaną spełnione, a pobrane pliki zweryfikowane, zainstaluj system kompilacji CMake.

2. Otwórz wiersz polecenia lub powłokę Git Bash. Wykonaj następujące polecenie, aby sklonować repozytorium zestawu SDK C pakietu Azure IoT w witrynie GitHub:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Rozmiar tego repozytorium wynosi obecnie około 220 MB. Należy się spodziewać, że ukończenie operacji potrwa kilka minut.


3. Utwórz podkatalog `cmake` w katalogu głównym repozytorium Git, a następnie przejdź do tego folderu. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Uruchom następujące polecenie, które tworzy wersję zestawu SDK specyficzną dla platformy klienta deweloperskiego. Rozwiązanie programu Visual Studio dla symulowanego urządzenia zostanie wygenerowane w katalogu `cmake`. 

    ```cmd
    cmake -Duse_prov_client:BOOL=ON ..
    ```
    
    Jeśli program `cmake` nie znajdzie kompilatora języka C++, mogą występować błędy kompilacji podczas uruchamiania powyższego polecenia. Jeśli tak się stanie, spróbuj uruchomić to polecenie w [wierszu polecenia programu Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Gdy kompilacja zakończy się powodzeniem, kilka ostatnich wierszy danych wyjściowych będzie wyglądać podobnie do następujących danych wyjściowych:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-a-symmetric-key-enrollment-group"></a>Utwórz grupę rejestracji klucza symetrycznego

1. Zaloguj się do [witryny Azure portal](http://portal.azure.com), a następnie otwórz swoje wystąpienie usługi Device Provisioning.

2. Wybierz **Zarządzanie rejestracjami** kartę, a następnie kliknij przycisk **Dodaj grupę rejestracji** znajdujący się u góry strony. 

3. Na **Dodaj grupę rejestracji**, wprowadź następujące informacje i kliknij przycisk **Zapisz** przycisku.

    - **Nazwa grupy**: wprowadź **mylegacydevices**.

    - **Typ zaświadczeń**: Wybierz **klucz symetryczny**.

    - **Automatyczne generowanie kluczy**: Zaznacz to pole wyboru.

    - **Wybierz sposób przypisywania urządzeń do centrów**: Wybierz **statycznie** można przypisać do określonego koncentratora.

    - **Wybierz centra IoT Hub do tej grupy mogą być przypisane do**: Wybierz jeden z koncentratorami.

    ![Dodaj grupę rejestracji dla zaświadczenia klucza symetrycznego](./media/how-to-legacy-device-symm-key/symm-key-enrollment-group.png)

4. Po zapisaniu Twojej rejestracji **klucza podstawowego** i **klucz pomocniczy** zostanie wygenerowany i dodany do wpisu rejestracji. Grupa symetrycznego klucza rejestracji jest wyświetlana jako **mylegacydevices** w obszarze *Nazwa grupy* kolumny w *grup rejestracji* kartę. 

    Otwórz rejestracji i skopiuj wartości z wygenerowanym **klucz podstawowy**. Ten klucz jest kluczem głównym grupy.


## <a name="choose-a-unique-registration-id-for-the-device"></a>Wybierz identyfikator rejestracji dla urządzenia

Identyfikator rejestracji musi być zdefiniowany do identyfikowania poszczególnych urządzeń. Można użyć adresu MAC, numer seryjny lub żadnych unikatowych informacji z urządzenia. 

W tym przykładzie używamy kombinację adresu MAC i numer seryjny tworzących następujący ciąg identyfikatora rejestracji.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Utwórz identyfikator rejestracji dla urządzenia. Prawidłowe znaki to małe znaki alfanumeryczne i kreski ("-").


## <a name="derive-a-device-key"></a>Uzyskania klucza urządzenia 

Aby wygenerować klucz urządzenia, należy użyć klucza głównego grupy do obliczenia [HMAC SHA256](https://wikipedia.org/wiki/HMAC) identyfikatora rejestracji dla urządzenia i przekonwertować wynik w formacie Base64.

Nie dołączaj klucza głównego usługi grupy w kodzie urządzenia.


#### <a name="linux-workstations"></a>Stacje robocze systemu Linux

Jeśli używasz stacja robocza z systemem Linux umożliwia openssl wygenerowanie własnego klucza pochodnego urządzenia, jak pokazano w poniższym przykładzie.

Zastąp wartość **klucz** z **klucz podstawowy** zanotowany wcześniej.

Zastąp wartość **REG_ID** przy użyciu swojego identyfikatora rejestracji.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


#### <a name="windows-based-workstations"></a>Stacje robocze z systemem Windows

Jeśli używasz stację roboczą z systemem Windows można użyć programu PowerShell do wygenerowanie własnego klucza pochodnego urządzenia, jak pokazano w poniższym przykładzie.

Zastąp wartość **klucz** z **klucz podstawowy** zanotowany wcześniej.

Zastąp wartość **REG_ID** przy użyciu swojego identyfikatora rejestracji.

```PowerShell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```PowerShell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


Urządzenie będzie używać klucza pochodnego urządzenia za pomocą Identyfikatora rejestracji symetrycznego zaświadczenie klucza przy użyciu grup rejestracji podczas inicjowania obsługi.



## <a name="create-a-device-image-to-provision"></a>Tworzenie obrazu urządzenia w celu aprowizacji

W tej sekcji spowoduje zaktualizowanie przykładu aprowizacji o nazwie **prov\_dev\_klienta\_przykładowe** znajduje się w zestawie SDK C usługi Azure IoT konfigurowania wcześniej. 

Ten przykładowy kod symuluje sekwencji rozruchu urządzenia, która spowoduje wysłanie żądania aprowizacji wystąpienia usługi Device Provisioning Service. Sekwencji rozruchu spowoduje, że urządzenie zostało rozpoznane i przypisane do Centrum IoT, które zostały skonfigurowane w grupie rejestracji.

1. W witrynie Azure Portal wybierz kartę **Przegląd** dla swojej usługi Device Provisioning Service, a następnie zapisz wartość **_Zakres identyfikatorów_**.

    ![Wyodrębnianie informacji o punkcie końcowym usługi Device Provisioning Service z bloku portalu](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. W programie Visual Studio, otwórz **azure_iot_sdks.sln** plik rozwiązania, który został wygenerowany po uruchomieniu narzędzia CMake wcześniej. Plik rozwiązania powinien znajdować się w następującej lokalizacji:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. W oknie *Eksplorator rozwiązań* programu Visual Studio przejdź do folderu **Provision\_Samples**. Rozwiń przykładowy projekt o nazwie **prov\_dev\_client\_sample**. Rozwiń pozycję **Pliki źródłowe**, a następnie otwórz plik **prov\_dev\_client\_sample.c**.

4. Znajdź stałą `id_scope` i zastąp jej wartość wcześniej skopiowaną wartością **Zakres identyfikatorów**. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Znajdź definicję funkcji `main()` w tym samym pliku. Upewnij się, że `hsm_type` zmienna jest ustawiona na `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` jak pokazano poniżej:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. Kliknij prawym przyciskiem myszy projekt **prov\_dev\_client\_sample**, a następnie wybierz pozycję **Ustaw jako projekt startowy**. 

7. W programie Visual Studio *Eksploratora rozwiązań* okna, przejdź do **hsm\_zabezpieczeń\_klienta** projektu i rozwiń go. Rozwiń **pliki źródłowe**, a następnie otwórz **hsm\_klienta\_key.c**. 

    Znajdź deklaracji `REGISTRATION_NAME` i `SYMMETRIC_KEY_VALUE` stałe. Wprowadź następujące zmiany do pliku, a następnie zapisz plik.

    Zaktualizuj wartość `REGISTRATION_NAME` stałej z **identyfikator unikatowy rejestracji dla urządzenia**.
    
    Zaktualizuj wartość `SYMMETRIC_KEY_VALUE` stałe za pomocą usługi **pochodne klucz urządzenia**.

    ```c
    static const char* const REGISTRATION_NAME = "sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6";
    static const char* const SYMMETRIC_KEY_VALUE = "Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=";
    ```

7. Z menu programu Visual Studio wybierz pozycję **Debuguj** > **Uruchom bez debugowania**, aby uruchomić rozwiązanie. W monicie o ponowne skompilowanie projektu kliknij przycisk **Tak**, aby ponownie skompilować projekt przed uruchomieniem.

    Następujące dane wyjściowe jest przykładem symulowane urządzenie pomyślnie uruchamiania systemu oraz podłączania do aprowizacji wystąpienia usługi ma być przypisane do usługi IoT hub:

    ```cmd
    Provisioning API Version: 1.2.8

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: 
    test-docs-hub.azure-devices.net, deviceId: sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

    Press enter key to exit:
    ```

8. W portalu, przejdź do Centrum IoT hub symulowanego urządzenia został przypisany do, a następnie kliknij przycisk **urządzeń IoT** kartę. Po pomyślnej aprowizacji symulowanego Centrum, jego identyfikator urządzenia będzie widoczny na **urządzeń IoT** bloku przy użyciu *stan* jako **włączone**. Czasami trzeba kliknąć **Odśwież** znajdujący się u góry. 

    ![Urządzenie jest rejestrowane w centrum IoT](./media/how-to-legacy-device-symm-key/hub-registration.png) 



## <a name="security-concerns"></a>Kwestie dotyczące bezpieczeństwa

Należy pamiętać, że spowoduje to pozostawienie klucza pochodnego urządzenia dołączone jako część obrazu, który nie jest najlepszą zalecaną praktyką zabezpieczeń. To jest jednym z powodów Dlaczego bezpieczeństwo i łatwość użytkowania są wady i zalety. 





## <a name="next-steps"></a>Kolejne kroki

* Aby dowiedzieć się więcej Reprovisioning, zobacz [pojęcia reprovisoning urządzeń usługi IoT Hub](concepts-device-reprovision.md) 
* [Szybki Start: Aprowizowanie symulowanego urządzenia przy użyciu kluczy symetrycznych](quick-create-simulated-device-symm-key.md)
* Aby dowiedzieć się więcej anulowania zastrzeżenia, zobacz [jak anulować aprowizację urządzeń, które wcześniej zostały udostępnione do automatycznego ](how-to-unprovision-devices.md) 











