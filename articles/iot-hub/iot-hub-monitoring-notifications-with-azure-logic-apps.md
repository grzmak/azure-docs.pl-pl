---
title: Zdalne monitorowanie IoT i powiadomień przy użyciu usługi Azure Logic Apps | Dokumentacja firmy Microsoft
description: Używać usługi Azure Logic Apps na potrzeby IoT temperatury monitorowania w Centrum IoT i automatycznie wysyłać powiadomienia e-mail do skrzynki pocztowej dla wszelkich wykrytych anomalii.
author: rangv
manager: ''
keywords: powiadomienia dotyczące monitorowania, iot, iot monitorowania temperatury iot
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: adda4e948c11f84517b1e8dd01e6cfe42155e1ca
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409445"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Zdalne monitorowanie IoT i powiadomień za pomocą usługi Azure Logic Apps, łącząc usługę IoT hub i skrzynki pocztowej

![Diagram end-to-end](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Usługa Azure Logic Apps umożliwia automatyzowanie procesów w postaci serii kroków. Aplikacja logiki można połączyć z różnych usług i protokołów. Zaczyna się od wyzwalacza takich jak "Gdy konto zostanie dodane", a przy użyciu kombinacji akcji, jak "wysyłania powiadomienia wypychanego". Ta funkcja sprawia, że aplikacje logiki to idealne rozwiązanie IoT dla IoT monitorowania, takich jak pozostając alert anomalii wśród innych scenariuszy użycia.

## <a name="what-you-learn"></a>Omawiane zagadnienia

Dowiesz się, jak utworzyć aplikację logiki, która łączy Centrum IoT hub i skrzynki pocztowej do monitorowania temperatury i powiadomienia. Gdy temperatura przekracza 30 C, aplikacja kliencka oznacza `temperatureAlert = "true"` w komunikacie, wysyła do Centrum IoT hub. Komunikat wyzwala aplikację logiki, aby wysłać Ci wiadomość e-mail z powiadomieniem.

## <a name="what-you-do"></a>Co należy zrobić

* Utwórz przestrzeń nazw magistrali usług i Dodaj kolejkę do niego.
* Dodaj punkt końcowy i reguły routingu do usługi IoT hub.
* Tworzenie, konfigurowanie i testowanie aplikacji logiki.

## <a name="what-you-need"></a>Co jest potrzebne

* Samouczek [skonfigurować na twoim urządzeniu](iot-hub-raspberry-pi-kit-node-get-started.md) ukończone, która obejmuje następujące wymagania:
  * Aktywna subskrypcja platformy Azure.
  * Usługi Azure IoT hub w ramach Twojej subskrypcji.
  * Aplikacja kliencka, która wysyła komunikaty do usługi Azure IoT hub.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Utwórz przestrzeń nazw magistrali usług i Dodaj kolejkę do niego

### <a name="create-a-service-bus-namespace"></a>Utwórz przestrzeń nazw magistrali usług

1. Na [witryny Azure portal](https://portal.azure.com/), kliknij przycisk **Utwórz zasób** > **integracji dla przedsiębiorstw** > **usługi Service Bus**.
1. Podaj następujące informacje:

   **Nazwa**: Nazwa usługi service bus.

   **Warstwa cenowa**: kliknij **podstawowe** > **wybierz**. Warstwa podstawowa jest wystarczająca na potrzeby tego samouczka.

   **Grupa zasobów**: Użyj tej samej grupie zasobów, która korzysta z usługi IoT hub.

   **Lokalizacja**: Użyj tej samej lokalizacji, która korzysta z usługi IoT hub.
1. Kliknij pozycję **Utwórz**.

   ![Utwórz przestrzeń nazw magistrali usług w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Dodaj do kolejki usługi service bus

1. Otwórz przestrzeń nazw magistrali usług, a następnie kliknij przycisk **+ kolejka**.
1. Wprowadź nazwę kolejki, a następnie kliknij przycisk **Utwórz**.
1. Otwórz kolejki usługi service bus, a następnie kliknij przycisk **zasady dostępu współdzielonego** > **+ Dodaj**.
1. Wprowadź nazwę zasady wyboru **Zarządzaj**, a następnie kliknij przycisk **Utwórz**.

   ![Dodawanie kolejki usługi service bus w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a>Dodawanie punktu końcowego i reguły routingu do Centrum IoT hub

### <a name="add-an-endpoint"></a>Dodawanie punktu końcowego

1. Otwórz Centrum IoT hub, kliknij pozycję punkty końcowe > + Dodaj.
1. Wprowadź następujące informacje:

   **Nazwa**: Nazwa punktu końcowego.

   **Typ punktu końcowego**: Wybierz **kolejki usługi Service Bus**.

   **Przestrzeń nazw usługi Service Bus**: wybierz utworzoną przestrzeń nazw.

   **Kolejki usługi Service Bus**: Wybierz kolejkę został utworzony.
1. Kliknij przycisk **OK**.

   ![Dodawanie punktu końcowego do Centrum IoT hub w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Dodawanie reguły rozsyłania

1. W usłudze IoT hub kliknij **trasy** > **+ Dodaj**.
1. Wprowadź następujące informacje:

   **Nazwa**: Nazwa reguły routingu.

   **Źródło danych**: Wybierz **DeviceMessages**.

   **Punkt końcowy**: Wybierz punkt końcowy został utworzony.

   **Ciąg zapytania**: wprowadź `temperatureAlert = "true"`.
1. Kliknij pozycję **Zapisz**.

   ![Dodaj regułę routingu w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Tworzenie i konfigurowanie aplikacji logiki

### <a name="create-a-logic-app"></a>Tworzenie aplikacji logiki

1. W [witryny Azure portal](https://portal.azure.com/), kliknij przycisk **Utwórz zasób** > **integracji dla przedsiębiorstw** > **aplikacji logiki**.
1. Wprowadź następujące informacje:

   **Nazwa**: Nazwa aplikacji logiki.

   **Grupa zasobów**: Użyj tej samej grupie zasobów, która korzysta z usługi IoT hub.

   **Lokalizacja**: Użyj tej samej lokalizacji, która korzysta z usługi IoT hub.
1. Kliknij pozycję **Utwórz**.

### <a name="configure-the-logic-app"></a>Konfigurowanie aplikacji logiki

1. Otwórz aplikację logiki, która zostanie otwarty w Projektancie aplikacji logiki.
1. W Projektancie aplikacji logiki, kliknij przycisk **pusta aplikacja logiki**.

   ![Uruchom przy użyciu pustej aplikacji logiki w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Kliknij przycisk **Service Bus**.

   ![Wybierz usługi Service Bus, aby rozpocząć tworzenie aplikacji logiki w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Kliknij przycisk **usługi Service Bus — po nadejściu jeden lub więcej wiadomości w kolejce (Automatyczne zakończenie)**.
1. Tworzenie połączenia usługi service bus.
   1. Wprowadź nazwę połączenia.
   1. Kliknij przestrzeń nazw magistrali usług > zasady usługi service bus > **Utwórz**.

      ![Tworzenie połączenia usługi service bus dla twojej aplikacji logiki w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Kliknij przycisk **Kontynuuj** po utworzeniu połączenia usługi service bus.
   1. Wybierz kolejkę, który został utworzony, a następnie wprowadź `175` dla **maksymalna liczba komunikatów**

      ![Określ liczbę maksymalną komunikatu dla połączenia usługi service bus w aplikacji logiki](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Kliknij przycisk "Zapisz" przycisk, aby zapisać zmiany.

1. Utwórz połączenie usługi SMTP.
   1. Kliknij przycisk **nowy krok** > **Dodaj akcję**.
   1. Typ `SMTP`, kliknij przycisk **SMTP** usługi w wynikach wyszukiwania, a następnie kliknij przycisk **SMTP — Wyślij wiadomość E-mail**.

      ![Utwórz połączenie SMTP w Twojej aplikacji logiki w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Wprowadzić dane SMTP skrzynki pocztowej, a następnie kliknij przycisk **Utwórz**.

      ![Wprowadź informacje o połączeniu SMTP w Twojej aplikacji logiki w witrynie Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Uzyskaj informacje SMTP dla [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), i [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).
   1. Wprowadź swój adres e-mail dla **z** i **do**, i `High temperature detected` dla **podmiotu** i **treści**.
   1. Kliknij pozycję **Zapisz**.

Aplikacja logiki jest w stanie, gdy zapisujesz go.

## <a name="test-the-logic-app"></a>Testowanie aplikacji logiki

1. Uruchom aplikację kliencką, która wdrażania na urządzenia z systemem w [ESP8266 połączyć z usługą Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. Zwiększ temperatury środowisko wokół SensorTag się powyżej 30 C. Na przykład jasny świeczka wokół usługi Sensor tag.
1. Otrzymasz wiadomość e-mail z powiadomieniem wysyłanych przez aplikację logiki.

   > [!NOTE]
   > Dostawcy usługi poczty e-mail może być konieczne Sprawdź tożsamość nadawcy, aby upewnić się, że chodzi o WAS, który umożliwia wysłanie wiadomości e-mail.

## <a name="next-steps"></a>Kolejne kroki

Pomyślnie utworzono aplikację logiki, która łączy Centrum IoT hub i skrzynki pocztowej do monitorowania temperatury i powiadomienia.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
