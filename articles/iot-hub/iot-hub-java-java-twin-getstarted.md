---
title: Rozpoczynanie pracy z usługą Azure IoT Hub bliźniaczych reprezentacji urządzeń (Java) | Dokumentacja firmy Microsoft
description: Jak używać usługi Azure IoT Hub bliźniaczych reprezentacji urządzeń Dodawanie tagów, a następnie użyć zapytania usługi IoT Hub. Urządzenia usługi Azure IoT SDK dla języka Java umożliwia wdrożenie aplikacji urządzenia i usługi Azure IoT SDK dla języka Java do zaimplementowania app service, który dodaje znaczniki i uruchamia zapytanie usługi IoT Hub.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: b8884cafbf250b9d7a88219b5647addafee9904a
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186904"
---
# <a name="get-started-with-device-twins-java"></a>Rozpoczynanie pracy z bliźniaczych reprezentacji urządzeń (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

W tym samouczku utworzysz dwie aplikacje konsolowe Java:

* **Dodaj tagów query**, aplikacji zaplecza Java, która dodaje znaczniki i zapytań bliźniaczych reprezentacji urządzeń.
* **Symulowane urządzenie**, aplikacji urządzenia Java, która łączy się z Twojego Centrum IoT i raportowanie stanu łączności za pomocą zgłaszanych właściwości.

> [!NOTE]
> Artykuł [Azure IoT SDKs](iot-hub-devguide-sdks.md) informacje dotyczące zestawów SDK usługi Azure IoT, w której można tworzyć aplikacje zarówno w przypadku urządzeń, jak i zaplecza.

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Najnowszy zestaw [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktywne konto platformy Azure. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto](http://azure.microsoft.com/pricing/free-trial/) w zaledwie kilka minut.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>Tworzenie aplikacji usługi

W tej sekcji utworzysz aplikacji Java, która dodaje metadanymi lokalizacji, zgodnie z tagu bliźniaczej reprezentacji urządzenia w usłudze IoT Hub skojarzone z **myDeviceId**. Aplikacja najpierw zapytań usługi IoT hub dla urządzeń znajdujących się w Stanach Zjednoczonych, a następnie w przypadku urządzeń, którzy raportują połączenia sieci komórkowej.

1. Na komputerze deweloperskim, Utwórz pusty folder o nazwie `iot-java-twin-getstarted`.

1. W `iot-java-twin-getstarted` folderu, Utwórz projekt narzędzia Maven o nazwie **Dodaj tagów query** przy użyciu następującego polecenia w wierszu polecenia. Zwróć uwagę, że jest to jedno długie polecenie:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. W wierszu polecenia przejdź do `add-tags-query` folderu.

1. Za pomocą edytora tekstów otwórz `pom.xml` w pliku `add-tags-query` folderze i dodaj następującą zależność do **zależności** węzła. Ta zależność umożliwia użycie **iot-service-client** pakietu w aplikacji do komunikowania się z Centrum IoT:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Możesz sprawdzić dostępność najnowszej wersji **iot-service-client** przy użyciu [funkcji wyszukiwania narzędzia Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Dodaj następujący kod **kompilacji** węzła po **zależności** węzła. Ta konfiguracja powoduje, że narzędzie Maven na potrzeby tworzenia aplikacji Java 1.8:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Zapisz i Zamknij `pom.xml` pliku.

1. Za pomocą edytora tekstów otwórz `add-tags-query\src\main\java\com\mycompany\app\App.java` pliku.

1. Dodaj do pliku następujące instrukcje **importowania**:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Dodaj następujące zmienne na poziomie klasy do klasy **App**. Zastąp `{youriothubconnectionstring}` parametrami połączenia Centrum IoT zanotowanym w *Tworzenie Centrum IoT* sekcji:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Aktualizacja **głównego** podpis metody, aby uwzględnić następujące `throws` klauzuli:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Dodaj następujący kod do **głównego** metodę w celu utworzenia **DeviceTwin** i **DeviceTwinDevice** obiektów. **DeviceTwin** obiektu obsługuje komunikację z Centrum IoT hub. **DeviceTwinDevice** obiekt reprezentuje bliźniaczej reprezentacji urządzenia za pomocą jej właściwości i tagów:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Dodaj następujący kod `try/catch` za pomocą bloku **głównego** metody:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. Aby zaktualizować **region** i **zakładu produkcyjnego** tagów bliźniaczych reprezentacji urządzeń w bliźniak urządzenia, Dodaj następujący kod w `try` bloku:

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. Do wykonywania zapytań bliźniaczych reprezentacji urządzeń w usłudze IoT hub, Dodaj następujący kod do `try` blok po kodzie dodanym w poprzednim kroku. Kod działa na dwóch zapytań. Każda kwerenda zwraca maksymalnie 100 urządzeń:

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. Zapisz i Zamknij `add-tags-query\src\main\java\com\mycompany\app\App.java` pliku

1. Tworzenie **Dodaj tagów query** aplikacji i poprawić błędy. W wierszu polecenia przejdź do `add-tags-query` folderu i uruchom następujące polecenie:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Tworzenie aplikacji urządzenia

W tej sekcji utworzysz aplikację konsolową Java, która ustawia wartość zgłaszanych właściwości, które są wysyłane do usługi IoT Hub.

1. W `iot-java-twin-getstarted` folderu, Utwórz projekt narzędzia Maven o nazwie **symulowane urządzenie** przy użyciu następującego polecenia w wierszu polecenia. Zwróć uwagę, że jest to jedno długie polecenie:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. W wierszu polecenia przejdź do `simulated-device` folderu.

1. Za pomocą edytora tekstów otwórz `pom.xml` w pliku `simulated-device` folderze i dodaj następujące zależności do **zależności** węzła. Ta zależność umożliwia użycie **iot-device-client** pakietu w aplikacji do komunikowania się z Centrum IoT:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Możesz sprawdzić dostępność najnowszej wersji **iot-device-client** przy użyciu [funkcji wyszukiwania narzędzia Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Dodaj następujący kod **kompilacji** węzła po **zależności** węzła. Ta konfiguracja powoduje, że narzędzie Maven na potrzeby tworzenia aplikacji Java 1.8:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Zapisz i Zamknij `pom.xml` pliku.

1. Za pomocą edytora tekstów otwórz `simulated-device\src\main\java\com\mycompany\app\App.java` pliku.

1. Dodaj do pliku następujące instrukcje **importowania**:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Dodaj następujące zmienne na poziomie klasy do klasy **App**. Zastępowanie `{youriothubname}` nazwą Centrum IoT hub, a `{yourdevicekey}` urządzenie wartością klucza wygenerowanego w *tworzenie tożsamości urządzenia* sekcji:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Ta przykładowa aplikacja używa zmiennej **protocol** podczas tworzenia wystąpienia obiektu **DeviceClient**. 

1. Dodaj następujący kod do **głównego** metody:
    * Utwórz klienta urządzenia do komunikowania się z usługą IoT Hub.
    * Tworzenie **urządzenia** obiekt, aby zapisać właściwości bliźniaczych reprezentacji urządzeń.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object to store the device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
    };
    ```

1. Dodaj następujący kod do **głównego** metodę w celu utworzenia **connectivityType** zgłaszane właściwości i wysyłać je do usługi IoT Hub:

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Dodaj następujący kod na końcu **głównego** metody. Oczekiwanie na **Enter** klucz umożliwia czasu dla usługi IoT Hub zgłosić stan operacji bliźniaczej reprezentacji urządzenia:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Zapisz i Zamknij `simulated-device\src\main\java\com\mycompany\app\App.java` pliku.

1. Tworzenie **symulowane urządzenie** aplikacji i poprawić błędy. W wierszu polecenia przejdź do `simulated-device` folderu i uruchom następujące polecenie:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Uruchamianie aplikacji

Teraz można przystąpić do uruchomienia aplikacji konsoli.

1. W wierszu polecenia w `add-tags-query` folder, uruchom następujące polecenie, aby uruchomić **Dodaj tagów query** usługi app Service:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikacja usługi IoT Hub dla środowiska Java do zaktualizuj wartości tagów i uruchom zapytania urządzeń](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Możesz zobaczyć **zakładu produkcyjnego** i **region** tagi dodane do bliźniaczej reprezentacji urządzenia. Pierwsze zapytanie zwraca urządzenia, ale druga nie.

1. W wierszu polecenia w `simulated-device` folder, uruchom następujące polecenie, aby dodać **connectivityType** zgłaszane właściwości do bliźniaczej reprezentacji urządzenia:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Dodaje klienta urządzenia ** connectivityType ** zgłaszane właściwości](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. W wierszu polecenia w `add-tags-query` folder, uruchom następujące polecenie, aby uruchomić **Dodaj tagów query** po raz drugi usługi app service:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikacja usługi IoT Hub dla środowiska Java do zaktualizuj wartości tagów i uruchom zapytania urządzeń](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Teraz Twoje urządzenie zostało wysłane **connectivityType** właściwość do usługi IoT Hub, drugie zapytanie zwraca urządzenie.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku opisano konfigurowanie nowego centrum IoT Hub w witrynie Azure Portal, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Dodano metadane urządzenia jako tagi z aplikacji zaplecza, a aplikacja powstała z jednego urządzenia do raportu informacje o łączności urządzenia w bliźniaku urządzenia. Przedstawiono również sposób zbadać informacji bliźniaczej reprezentacji urządzenia przy użyciu języka zapytań usługi IoT Hub podobnego do SQL.

Użyj następujących zasobów, aby dowiedzieć się, jak:

* Wysyłanie danych telemetrycznych z urządzeń przy użyciu [Rozpoczynanie pracy z usługą IoT Hub](quickstart-send-telemetry-java.md) samouczka.
* Formant urządzenia interakcyjne (takich jak włączenie wentylator z aplikacji kontrolowanej przez użytkownika) [używanie metod bezpośrednich](quickstart-control-device-java.md) samouczka.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
