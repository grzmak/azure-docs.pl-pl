---
title: Urządzenie raspberry Pi w chmurze (Node.js) - łączenie urządzenia Raspberry Pi dla usługi Azure IoT Hub | Dokumentacja firmy Microsoft
description: Dowiedz się, jak skonfigurować i połączyć urządzenia Raspberry Pi do usługi Azure IoT Hub dla urządzenia Raspberry Pi w celu wysyłania danych do platformy w chmurze w ramach tego samouczka.
author: rangv
manager: ''
keywords: Usługa Azure iot urządzenia raspberry pi, usługi iot hub dla urządzenia raspberry pi, urządzenia raspberry pi wysyłania danych do chmury, urządzenia raspberry pi w chmurze
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 348186bcb29b272b7e6512ce42221d54d6b388d9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161819"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Łączenie urządzenia Raspberry Pi do usługi Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

W tym samouczku rozpoczniesz od podstawy pracy z urządzeniem Raspberry Pi, w którym działa Raspbian uczenia. Następnie dowiesz się, jak bezproblemowe łączenie urządzeń do chmury przy użyciu [usługi Azure IoT Hub](about-iot-hub.md). Aby uzyskać przykłady Windows 10 IoT Core, przejdź do [Centrum deweloperów Windows](http://www.windowsondevices.com/).

Nie masz jeszcze zestawu? Spróbuj [symulatora w trybie online urządzenia Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md). Lub Kup nowy zestaw [tutaj](https://azure.microsoft.com/develop/iot/starter-kits).


## <a name="what-you-do"></a>Co należy zrobić

* Tworzenie Centrum IoT.
* Zarejestruj urządzenie Pi w usłudze IoT hub.
* Konfigurowanie urządzenia Raspberry Pi.
* Uruchom przykładową aplikację na Pi do wysyłania danych czujnika do usługi IoT hub.

## <a name="what-you-learn"></a>Omawiane zagadnienia

* Jak utworzyć Centrum Azure IoT hub i uzyskać nowe parametry połączenia urządzenia.
* Jak połączyć Pi z czujnikiem BME280.
* Jak zbierać dane z czujników po uruchomieniu aplikacji przykładowej na Pi.
* Jak wysyłać danych czujnika do usługi IoT hub.

## <a name="what-you-need"></a>Co jest potrzebne

![Co jest potrzebne](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* Raspberry Pi 2 lub Raspberry Pi 3 tablicy.
* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Monitor, USB klawiatury i myszy, który nawiązuje połączenie z Pi.
* Mac lub komputerze z systemem Windows lub Linux.
* Połączenie internetowe.
* 16 GB lub nowszej karcie microSD.
* USB-karty lub microSD na kartach SD Nagraj obraz systemu operacyjnego na karcie microSD.
* Podaj potęgą 2 i 5 wolt za pomocą kabla USB wczesnych metr 6.

Opcjonalne są następujące elementy:

* Zmontowanych Adafruit BME280 temperatury, wykorzystanie i wilgotności czujnika.
* Breadboard.
* Przewodów jumper 6 F/M.
* Rozproszona LED 10 mm.

> [!NOTE] 
> Jeśli nie masz opcjonalne elementy, można użyć danych z symulowanych czujników.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a>Konfigurowanie urządzenia Raspberry Pi

### <a name="install-the-raspbian-operating-system-for-pi"></a>Zainstaluj system operacyjny Raspbian pi

Przygotuj karcie microSD instalacji obrazu Raspbian.

1. Pobierz Raspbian.

   a. [Pobierz Raspbian Stretch](http://downloads.raspberrypi.org/raspbian/images/raspbian-2017-07-05/) (plik zip).

   > [!WARNING]
   > Użyj powyższe łącze, aby pobrać `raspbian-2017-07-5` obraz pliku zip. Najnowszą wersję obrazów Raspbian ma znane problemy z węzłem Pi dwużyłowa, co może prowadzić do awarii w następnych krokach.
 
   b. Wyodrębnij obraz Raspbian do folderu na komputerze.

2. Zainstaluj Raspbian karcie microSD.

   a. [Pobierz i zainstaluj narzędzie nagrywarka karty Etcher SD](https://etcher.io/).

   b. Uruchom Etcher, a następnie wybierz obraz Raspbian, który został wyodrębniony w kroku 1.

   c. Wybierz dysk karty microSD. Etcher już być może wybrano poprawny dysk.

   d. Kliknij przycisk Flash, aby zainstalować Raspbian na karcie microSD.

   e. Karta microSD należy usunąć z komputera po zakończeniu instalacji. Jest bezpiecznie usunąć karcie microSD bezpośrednio, ponieważ Etcher automatycznie wysuwa lub odinstalowuje karcie microSD po jego ukończeniu.

   f. Wstaw karcie microSD do Pi.

### <a name="enable-ssh-and-i2c"></a>Włączanie protokołu SSH i I2C

1. Pi nawiązać monitora, klawiatury i myszy. 

2. Rozpocznij Pi, a następnie zaloguj się w Raspbian przy użyciu `pi` jako nazwy użytkownika i `raspberry` jako hasło.

3. Kliknij ikonę Raspberry > **preferencje** > **Raspberry Pi konfiguracji**.

   ![W menu Preferencje Raspbian](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

4. Na **interfejsów** kartę, należy ustawić **I2C** i **SSH** do **Włącz**, a następnie kliknij przycisk **OK**. Jeśli nie masz fizycznego czujniki i chcesz użyć danych z symulowanych czujników, ten krok jest opcjonalny.

   ![Włącz I2C i ustawieniami SSH na urządzeniu Raspberry Pi](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
> Aby włączyć protokół SSH i I2C, można znaleźć więcej dokumentów odwołania na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) i [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-the-sensor-to-pi"></a>Łączenie z czujnika do Pi

Użyj przewodów breadboard i jumper nawiązać DIODĘ i BME280 Pi w następujący sposób. Jeśli nie masz czujnik [pominąć tę sekcję](#connect-pi-to-the-network).

![Połączenie urządzenia Raspberry Pi i czujnik](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

Czujnik BME280 może zbierać dane temperatury i wilgotności. Dioda LED miga, gdy urządzenie wysyła komunikat do chmury. 

Czujnik numery PIN można użyć następujących połączeń:

| Uruchom (czujnik & LED)     | Końcowy (tablica)            | Kolor kabel   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 5G)             | 3, 3V PWR (Pin 1)       | Kabel biały   |
| GND (Pin 7G)             | GND (Pin 6)            | Brązowy kabel   |
| SDI (Pin 10G)            | I2C1 SDA (Pin 3)       | Czerwony kabel     |
| SCK (Pin 8 K)             | I2C1 SCL (Pin 5)       | Kabel pomarańczowy  |
| VDD LED (Pin 18F)        | Interfejs GPIO 24 (Pin 18)       | Kabel biały   |
| GND LED (Pin 17F)        | GND (Pin 20)           | Czarnego przewodu   |

Kliknij, aby wyświetlić [Raspberry Pi 2 i 3 mapowania kodu pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) użytkownikowi.

Po pomyślnym nawiązaniu BME280 urządzenia Raspberry Pi, należy go jak poniżej obrazu.

![Pi połączonych i BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi się z siecią

Włącz Pi przy użyciu micro kabla USB i zasilacz. Użyj kabla Ethernet do łączenia Pi sieci przewodowej lub postępuj zgodnie z [instrukcji z urządzeniem Raspberry Pi podstawę](https://www.raspberrypi.org/learning/software-guide/wifi/) do Pi nawiązać połączenie z sieci bezprzewodowej. Po Twojej Pi została pomyślnie podłączona do sieci, należy pamiętać o [adres IP Twojego Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Podłączone do sieci przewodowej](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Upewnij się, że Pi jest podłączony do sieci z komputera. Na przykład jeśli komputer jest połączony z siecią bezprzewodową, gdy Pi jest połączony z siecią przewodową, możesz nie zobaczyć adres IP w danych wyjściowych devdisco.

## <a name="run-a-sample-application-on-pi"></a>Uruchamianie przykładowej aplikacji na Pi

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Sklonuj przykładową aplikację i zainstaluj wstępnie wymagane pakiety

1. Połącz z urządzenia Raspberry Pi przy użyciu jednego z następujących klientów SSH z komputera hosta:
   
   **Użytkownicy Windows**
  
   a. Pobierz i zainstaluj [PuTTY](http://www.putty.org/) for Windows. 

   b. Skopiuj adres IP w sekcji Pi do nazwy hosta (lub adres IP), a następnie wybierz typ połączenia SSH.
   
   ![Programu puTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac i użytkownicy systemu Ubuntu**
   
   Użyj wbudowanego klienta SSH w systemie Ubuntu lub z systemem macOS. Może być konieczne uruchomienie `ssh pi@<ip address of pi>` połączyć Pi za pośrednictwem protokołu SSH.

   > [!NOTE] 
   > Domyślna nazwa użytkownika to `pi` , a hasło to `raspberry`.

2. Do Twojej Pi, należy zainstalować środowisko Node.js i NPM.
   
   Najpierw sprawdź wersję środowiska Node.js. 
   
   ```bash
   node -v
   ```

   Jeśli wersja jest niższa niż 4.x, czy na Twoje Pi jest nie Node.js, zainstaluj najnowszą wersję.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

3. Klonowanie przykładowej aplikacji.

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

4. Zainstaluj wszystkie pakiety dla przykładu. Instalacja obejmuje zestaw SDK urządzeń Azure IoT, czujnik BME280 biblioteki i biblioteki dołączenie Pi.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   >Może upłynąć kilka minut na zakończenie tego procesu instalacji, w zależności od połączenia sieciowego.

### <a name="configure-the-sample-application"></a>Konfigurowanie przykładowej aplikacji

1. Otwórz plik konfiguracji, uruchamiając następujące polecenia:

   ```bash
   nano config.json
   ```

   ![Plik konfiguracji](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Istnieją dwa elementy w tym pliku, które można skonfigurować. Pierwsza z nich jest `interval`, która definiuje interwał (w milisekundach) między komunikaty wysyłane do chmury. Drugim jest `simulatedData`, który jest wartość logiczna wskazująca, czy używać danych z czujników symulowanych lub nie.

   Jeśli użytkownik **nie ma czujnika**ustaw `simulatedData` wartość `true` się do aplikacji przykładowej, tworzenia i używania danych z symulowanych czujników.

2. Zapisz i zamknij go, wpisując formantu O > Wprowadź > Control X.

### <a name="run-the-sample-application"></a>Uruchamianie przykładowej aplikacji

Uruchamianie przykładowej aplikacji, uruchamiając następujące polecenie:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   > Upewnij się, możesz kopiowania i wklejania parametry połączenia urządzenia w apostrofy.


Powinny być widoczne następujące dane wyjściowe, dane czujników i komunikaty, które są wysyłane do usługi IoT hub.

![Dane wyjściowe — dane czujników wysyłanych z urządzenia Raspberry Pi do Centrum IoT hub](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Kolejne kroki

Po uruchomieniu aplikacji przykładowej, zbieranie danych z czujników i wysyłać je do Centrum IoT hub. Aby wyświetlić wiadomości wysłanych do usługi IoT hub lub wysyłania wiadomości do urządzenia Raspberry Pi urządzenia Raspberry Pi, zobacz [rozszerzenie użycia usługi Azure IoT Toolkit dla programu Visual Studio Code do wysyłania i odbierania komunikatów między urządzeniem i Centrum IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
