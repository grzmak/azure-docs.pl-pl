---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 97e032af71947340c7e3b0af3b9d0701c972144e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843154"
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Testowanie zapytań interfejsu API Microsoft Graph z poziomu aplikacji dla systemu iOS

Aby uruchomić kod w symulatorze, naciśnij klawisz **polecenia** + **R**.

![Przetestuj aplikację w symulatorze](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Gdy wszystko będzie gotowe do testowania, wybierz pozycję **wywołania interfejsu API Microsoft Graph**. Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło.

### <a name="provide-consent-for-application-access"></a>Podanie zgody na dostęp do aplikacji
Przy pierwszym logowaniu do aplikacji, zostanie wyświetlony monit o wyrażenie zgody, aby umożliwić aplikacji dostęp do Twojego profilu i logowanie się w:

![Wyrażenie zgody na dostęp do aplikacji](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Wyświetl wyniki aplikacji
Po zalogowaniu, powinien zostać wyświetlony informacji o profilu użytkownika zwracany przez wywołanie interfejsu API Microsoft Graph w **rejestrowania** sekcji. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i delegowane uprawnienia

Interfejsu API programu Microsoft Graph wymaga **user.read** zakresu na odczytywanie profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie każda aplikacja, która jest zarejestrowana w portalu rejestracji. Inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera zaplecza, może być wymagane dodatkowe zakresy. Interfejsu API programu Microsoft Graph wymaga **Calendars.Read** zakres, aby wyświetlić listę kalendarzy użytkownika.

Aby uzyskać dostęp do kalendarzy użytkownika w kontekście aplikacji, należy dodać **Calendars.Read** delegowane uprawnienia do rejestrowania informacji o aplikacji. Następnie należy dodać **Calendars.Read** ograniczyć zakres do **acquireTokenSilent** wywołania. 

>[!NOTE]
>Użytkownik może być monitowany o dodatkowe zgody, jak zwiększyć liczbę zakresów.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
