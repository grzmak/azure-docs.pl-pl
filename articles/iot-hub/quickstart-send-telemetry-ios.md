---
title: Wysyłanie danych telemetrycznych do usługi Azure IoT Hub — Szybki start | Microsoft Docs
description: W tym przewodniku Szybki start uruchomisz przykładową aplikację systemu iOS wysyłającą symulowane dane telemetryczne do centrum IoT oraz odczytującą dane telemetryczne z centrum IoT na potrzeby przetwarzania w chmurze.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/20/2018
ms.author: kgremban
ms.openlocfilehash: aecb9a1819060e0da6338e8e16bf681fad42dd22
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161921"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-ios"></a>Szybki start: wysyłanie danych telemetrycznych z urządzenia do centrum IoT (iOS)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub to usługa platformy Azure, która umożliwia pozyskiwanie dużych ilości danych telemetrycznych z urządzeń IoT do chmury w celu magazynowania lub przetwarzania. W tym artykule opisano wysyłanie danych telemetrycznych z aplikacji urządzenia symulowanego do usługi IoT Hub, a następnie wyświetlanie tych danych z poziomu aplikacji zaplecza. 

W tym artykule używana jest wstępnie napisana aplikacja języka Swift wysyłająca dane telemetryczne oraz narzędzie interfejsu wiersza polecenia odczytujące dane telemetryczne z usługi IoT Hub. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne

- Pobierz przykładowy kod z [przykładów dla platformy Azure](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip) 
- Najnowsza wersja środowiska [XCode](https://developer.apple.com/xcode/) korzystająca z najnowszej wersji zestawu SDK systemu iOS. Ten przewodnik Szybki start przetestowano przy użyciu środowiska XCode 9.3 i systemu iOS 11.3.
- Najnowsza wersja menedżera [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).

## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT Hub

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Rejestrowanie urządzenia

Zanim urządzenie będzie mogło nawiązać połączenie, należy je najpierw zarejestrować w centrum IoT. W tym przewodniku Szybki start urządzenie symulowane jest rejestrowane przy użyciu interfejsu wiersza polecenia platformy Azure.

1. Dodaj rozszerzenie interfejsu wiersza polecenia usługi IoT Hub i utwórz tożsamość urządzenia. Zastąp ciąg `{YourIoTHubName}` nazwą centrum IoT:

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   az iot hub device-identity create --hub-name {YourIoTHubName} --device-id myiOSdevice
   ```

    Jeśli wybierzesz inną nazwę dla swojego urządzenia, zaktualizuj nazwę urządzenia w przykładowych aplikacjach przed ich uruchomieniem.

1. Uruchom następujące polecenie, aby uzyskać _parametry połączenia urządzenia_ dla urządzenia, które właśnie zostało zarejestrowane:

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id myiOSdevice --output table
   ```

   Zanotuj parametry połączenia urządzenia, które wyglądają następująco: `Hostname=...=`. Użyjesz tej wartości w dalszej części tego artykułu.

## <a name="send-simulated-telemetry"></a>Wysyłanie symulowanych danych telemetrycznych

Przykładowa aplikacja jest uruchamiana na urządzeniu z systemem iOS, które łączy się z punktem końcowym specyficznym dla urządzenia w centrum IoT i wysyła symulowane dane telemetryczne dotyczące temperatury oraz wilgotności. 

### <a name="install-cocoapods"></a>Instalowanie zasobników CocoaPods

Zasobniki CocoaPods zarządzają zależnościami dla projektów systemu iOS korzystających z bibliotek innych firm.

W oknie terminalu przejdź do folderu Azure-IoT-Samples-iOS pobranego w ramach wymagań wstępnych. Następnie przejdź do przykładowego projektu:

```sh
cd quickstart/sample-device
```

Upewnij się, że środowisko XCode jest zamknięte, a następnie uruchom następujące polecenie, aby zainstalować zasobniki CocoaPods zadeklarowane w pliku **podfile**:

```sh
pod install
```

Oprócz instalacji zasobników wymaganych przez projekt polecenie instalacji tworzy także plik obszaru roboczego środowiska XCode, który jest już skonfigurowany do używania zasobników na potrzeby zależności. 

### <a name="run-the-sample-application"></a>Uruchamianie przykładowej aplikacji 

1. Otwórz przykładowy obszar roboczy w środowisku XCode.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. Rozwiń projekt **MQTT Client Sample**, a następnie rozwiń folder o takiej samej nazwie.  
3. Otwórz plik **ViewController.swift** do edycji w środowisku XCode. 
4. Wyszukaj zmienną **connectionString** i zaktualizuj wartość zanotowanymi wcześniej parametrami połączenia urządzenia.
5. Zapisz zmiany. 
6. Uruchom projekt w emulatorze urządzenia za pomocą przycisku **Skompiluj i uruchom** lub kombinacji klawiszy **Command + R**. 

   ![Uruchamianie projektu](media/quickstart-send-telemetry-ios/run-sample.png)

7. Po otwarciu emulatora wybierz pozycję **Start** w przykładowej aplikacji.

Poniższy zrzut ekranu przedstawia przykładowe dane wyjściowe w momencie wysyłania przez aplikację symulowanych danych telemetrycznych do centrum IoT:

   ![Uruchamianie urządzenia symulowanego](media/quickstart-send-telemetry-ios/view-d2c.png)

## <a name="read-the-telemetry-from-your-hub"></a>Odczytywanie danych telemetrycznych z centrum

Przykładowa aplikacja uruchomiona w emulatorze środowiska XCode wyświetla dane dotyczące komunikatów wysłanych z urządzenia. Dane możesz wyświetlić także za pośrednictwem centrum IoT, gdy są odbierane. Rozszerzenie interfejsu wiersza polecenia usługi IoT Hub może połączyć się z punktem końcowym **Zdarzenia** po stronie usługi w usłudze IoT Hub. Rozszerzenie odbiera komunikaty z urządzenia do chmury wysyłane z urządzenia symulowanego. Aplikacja zaplecza usługi IoT Hub zwykle działa w chmurze, aby odbierać i przetwarzać komunikaty urządzenie-chmura.

Uruchom następujące polecenia interfejsu wiersza polecenia platformy Azure, zastępując ciąg `{YourIoTHubName}` nazwą centrum IoT Hub:

```azurecli-interactive
az iot hub monitor-events --device-id myiOSdevice --hub-name {YourIoTHubName}
```

Poniższy zrzut ekranu przedstawia dane wyjściowe w momencie odbierania przez rozszerzenie danych telemetrycznych wysyłanych przez urządzenie symulowane do centrum:

Poniższy zrzut ekranu przedstawia typ danych telemetrycznych wyświetlanych w oknie terminalu:

![Wyświetlanie danych telemetrycznych](media/quickstart-send-telemetry-ios/view-telemetry.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Następne kroki

W tym artykule opisano konfigurowanie centrum IoT, rejestrowanie urządzenia, wysyłanie symulowanych danych telemetrycznych do centrum z urządzenia z systemem iOS oraz odczytywanie danych telemetrycznych z centrum. 

Aby dowiedzieć się, jak kontrolować urządzenie symulowane z poziomu aplikacji zaplecza, przejdź do następnego przewodnika Szybki start.

> [!div class="nextstepaction"]
> [Szybki start: kontrolowanie urządzenia podłączonego do centrum IoT](quickstart-control-device-node.md)

<!-- Links -->
[lnk-process-d2c-tutorial]: tutorial-routing.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
