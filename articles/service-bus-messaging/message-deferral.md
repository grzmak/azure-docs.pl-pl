---
title: Odraczanie komunikatów w usłudze Azure Service Bus | Dokumentacja firmy Microsoft
description: Odrocz dostarczenia komunikatów usługi Service Bus
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: efe04b19188d7324c3f86565610040b8eaa97c43
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855395"
---
# <a name="message-deferral"></a>Odraczanie komunikatów

Gdy kolejki lub subskrypcji klient otrzymuje komunikat informujący o tym, czy jest chce go przetworzyć, ale dla których przetwarzanie nie jest obecnie możliwe ze względu na specjalne okoliczności wewnątrz aplikacji, włączono dla niej opcję "odkładania" pobieranie komunikat do późniejszego. Komunikat pozostanie w kolejce lub subskrypcji, ale zostanie odłożony.

Opóźnienia jest funkcją utworzone specjalnie do przepływu pracy przetwarzania scenariuszy. Struktury przepływ pracy może wymagać pewnych operacji, które mają być przetwarzane w określonej kolejności i może być konieczne odłożyć przetwarzania niektórych Odebrane komunikaty do momentu zakończenia realizowania wcześniejsze pracy, która jest informowany przez inne komunikaty.

Prosty przykład ilustrujący jest przetwarzania sekwencji, w którym płatności od dostawcy zewnętrznego płatności jest wyświetlane powiadomienie w systemie przed rozpropagowane pasującego zamówienia zakupu w sklepie systemu realizacji zamówień. W takim przypadku system realizacji może odroczyć przetwarzania powiadomienia o płatności, dopóki zamówienie, z którą chcesz skojarzyć go. W scenariuszach spotkania, w której wiadomości z różnych źródeł dysku przepływu pracy do przodu, kolejność wykonywania w czasie rzeczywistym w rzeczywistości może być poprawna, ale wiadomości w czasie wykonywania odbicia wyniki może pojawić się poza kolejnością.

Ostatecznie odroczenia pomaga Zmienianie kolejności wiadomości w kolejności przyjazdu w kolejności, w którym zostaną one przetworzone, przy równoczesnym zachowaniu je bezpiecznie w magazynie komunikatów na potrzeby przetwarzania, które opóźnienia.

## <a name="message-deferral-apis"></a>Odraczanie komunikatów interfejsów API

Interfejs API jest [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) lub [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) w kliencie programu .NET Framework [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) w kliencie programu .NET Standard i **mesageReceiver.defer** lub **messageReceiver.deferSync** klienta Java. 

Odroczone wiadomości pozostaną w kolejka główna oraz wszystkie inne aktywne komunikaty (w przeciwieństwie do wiadomości utraconych wiadomości, które znajdują się w kolejki podrzędnej), ale one nie otrzyma za pomocą funkcji regularnych odbioru/ReceiveAsync. Odnalezione odroczonej wiadomości [Przeglądanie komunikatów](message-browsing.md) Jeśli aplikacja utraci śledzenie z nich.

Aby pobrać odroczony komunikat, jego właściciela jest odpowiedzialny za zapamiętywanie [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) zgodnie z jej do odracza. Żadnego odbiorcy, który zna numer sekwencyjny odroczony komunikat później może odbierać wiadomości jawnie z `Receive(sequenceNumber)`.

Nie można przetworzyć komunikatu, ponieważ określony zasób do obsługi tego komunikatu jest tymczasowo niedostępna, ale przetwarzanie komunikatu nie summarily zawiesza się, sposób umieszczania ten komunikat po stronie przez kilka minut, czy do zapamiętania  **SequenceNumber** w [zaplanowanych wiadomości](message-sequencing.md) do opublikowania w ciągu kilku minut i ponownie Pobierz odroczony komunikat po nadejściu zaplanowanych wiadomości. Jeśli program obsługi komunikatów, który jest zależny od bazy danych dla wszystkich operacji tej bazy danych jest tymczasowo niedostępny, go powinien używa opóźnienia, ale raczej zawiesić odbierania komunikatów całkowicie, aż baza danych jest ponownie dostępny.

Opóźnienie wiadomości nie ma wpływu na wygaśnięcie komunikatu, co oznacza, że odroczonej wiadomości nadal wygaśnie początkowo zaplanowanej godzinie i są przenoszone do kolejki utraconych wiadomości po odpowiednim skonfigurowaniu.

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat obsługi komunikatów usługi Service Bus, zobacz następujące tematy:

* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Wprowadzenie do kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)