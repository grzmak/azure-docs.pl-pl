---
title: Odwołanie V3.0 interfejs API tłumaczenia tekstu w usłudze Translator
titlesuffix: Azure Cognitive Services
description: Dokumentacja dotycząca V3.0 interfejs API tekstu usługi Translator.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 243ee16f8de8add8283581c8c03a37594797864b
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430038"
---
# <a name="translator-text-api-v30"></a>Interfejs API tekstu usługi Translator w wersji 3.0

## <a name="whats-new"></a>Co nowego?

Interfejs API tekstu usługi Translator w wersji 3 zapewnia nowoczesnych opartych na formacie JSON internetowego interfejsu API. Zwiększa użyteczność i wydajność dzięki konsolidacji istniejących funkcji w mniejszą liczbę operacji i udostępnia nowe funkcje.

 * Transliterację konwersji tekstu w jednym języku jeden skrypt inny skrypt.
 * Tłumaczenie na wiele języków, w jednym żądaniu.
 * Wykrywanie języka, tłumaczenia i transliterację w jednym żądaniu.
 * Słownik, który wyszukiwanie alternatywnych tłumaczeń okresu, można znaleźć kopii tłumaczenia i przykłady pokazujące terminy używane w kontekście.
 * Bardziej szczegółowy wyniki wykrywanie języka.

## <a name="base-urls"></a>Podstawowych adresach URL

Tekst interfejsu API w wersji 3.0 jest dostępna w następujących chmury:

| Opis | Region | Podstawowy adres URL                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Globalny | API.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Authentication

Subskrybowanie do interfejsu API tłumaczenia tekstu w usługach Microsoft Cognitive Services i używają klucz subskrypcji (dostępne w witrynie Azure portal) do uwierzytelniania. 

Najprostszym sposobem jest przekazać klucz tajny platformy Azure do usługi Translator, przy użyciu nagłówka żądania `Ocp-Apim-Subscription-Key`.

Alternatywą jest do użycia tego klucza tajnego do uzyskania tokenu autoryzacji z usługi tokenu. Następnie należy przekazać token autoryzacji za pomocą usługi Translator `Authorization` nagłówek żądania. Aby uzyskać token autoryzacji, należy `POST` żądania do następującego adresu URL:

| Środowisko     | Adres URL usługi uwierzytelniania                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Oto przykład żądania do uzyskania tokenu danego klucza tajnego:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Żądania zakończonego powodzeniem zwraca token dostępu zakodowany jako zwykły tekst w treści odpowiedzi. Prawidłowy token jest przekazywany do usługi Translator jako token elementu nośnego w autoryzacji.

```
Authorization: Bearer <Base64-access_token>
```

Token uwierzytelniania jest ważny przez 10 minut. Token powinien być ponownie używane, gdy wielu wywołań interfejsów API usługi Translator. Jednak jeśli program sprawia, że żądania do interfejsu API usługi Translator przez dłuższy czas, następnie program musi żądać nowy token dostępu w regularnych odstępach czasu (np. co 8 minut).

Aby podsumować, żądanie klienta interfejsu API usługi Translator będzie obejmować jeden nagłówek autoryzacji z poniższej tabeli:

<table width="100%">
  <th width="30%">Nagłówki</th>
  <th>Opis</th>
  <tr>
    <td>OCP-Apim-Subscription-Key</td>
    <td>*Używane z subskrypcją usług Cognitive Services, jeśli przekazujesz klucz tajny*.<br/>Wartość jest platformy Azure klucz tajny dla Twojej subskrypcji do interfejsu API tłumaczenia tekstu.</td>
  </tr>
  <tr>
    <td>Autoryzacja</td>
    <td>*Jeśli przekazujesz tokenu uwierzytelniania za pomocą subskrypcji usług Cognitive Services.*<br/>Wartość tokenu elementu nośnego: "Bearer <token>".</td>
  </tr>
</table> 

## <a name="errors"></a>Błędy

Odpowiedzi błędu standardowego jest obiekt JSON z pary nazwa/wartość o nazwie `error`. Wartość jest także obiekt JSON z właściwościami:

  * `code`Kod błędu zdefiniowany przez serwer.

  * `message`: Ciąg, zapewniając czytelny dla człowieka reprezentację błędu.

Na przykład klient z bezpłatnej subskrypcji próbnej będzie komunikat o błędzie po wyczerpaniu bezpłatny limit przydziału:

```
{
  "error": {
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
Kod błędu to łączenie liczb 6-cyfrowym, 3-cyfrowy kod stanu HTTP następuje 3-cyfrowy numer do dalszego kategoryzowanie błędu. Typowe kody błędów są:

| Kod | Opis |
|:----|:-----|
| 400000| Dane wejściowe żądania jest nieprawidłowa.|
| 400001| Parametr "scope" jest nieprawidłowy.|
| 400002| Parametr "category" jest nieprawidłowy.|
| 400003| Specyfikator języka jest brakujący lub nieprawidłowy.|
| 400004| Specyfikatora skryptu docelowego ("do"skrypt") jest nieprawidłowy lub.|
| 400005| Wprowadzany tekst jest brakujący lub nieprawidłowy.|
| 400006| Kombinacja języka i skrypt nie jest prawidłowy.|
| 400018| Specyfikatora źródła skryptu ("od"skrypt") jest nieprawidłowy lub.|
| 400019| Jeden określony język nie jest obsługiwane.|
| 400020| Jeden z elementów w tablicy, której tekst wejściowy jest nieprawidłowy.|
| 400021| Parametr wersji interfejsu API jest brakujący lub nieprawidłowy.|
| 400023| Jeden z pary określony język jest nieprawidłowy.|
| 400035| Język źródłowy ("od" pole) jest nieprawidłowy.|
| 400036| Języka docelowego ("do" pole) jest nieprawidłowy lub niedostępny.|
| 400042| Jedną z opcji określone ("Opcje" pole) jest nieprawidłowy.|
| 400043| Identyfikator śledzenia klienta (ClientTraceId pola lub Nagłówek X-ClientTranceId) Brak lub jest nieprawidłowy.|
| 400050| Wprowadzany tekst jest zbyt długa.|
| 400064| Parametr "translacji" Brak lub jest nieprawidłowa.|
| 400070| Liczba docelowych skryptów (parametr ToScript) jest niezgodny z liczba docelowych języki (parametr).|
| 400071| Wartość nie jest prawidłowa dla TextType.|
| 400072| Tablica wprowadzania tekstu ma za dużo elementów.|
| 400073| Parametr skryptu jest nieprawidłowy.|
| 400074| Treść żądania nie jest prawidłowym plikiem JSON.|
| 400075| Kombinacja pary i kategorii języka jest nieprawidłowa.|
| 400077| Rozmiar maksymalny żądania został przekroczony.|
| 400079| Nie ma żądanego translacji od i do języka systemu niestandardowych.|
| 401000| Żądanie nie ma uprawnień, ponieważ poświadczenia są brakujący lub nieprawidłowy.|
| 401015| "Są podane poświadczenia dla interfejsu API rozpoznawania mowy. To żądanie wymaga poświadczeń dla interfejsu API tłumaczenia tekstu. Użyj subskrypcji interfejsu API tłumaczenia tekstu."|
| 403000| Operacja nie jest dozwolona.|
| 403001| Operacja jest niedozwolona, ponieważ subskrypcja przekroczyła limit przydziału bezpłatnych.|
| 405000| Metoda żądania nie jest obsługiwana dla żądanego zasobu.|
| 415000| Nagłówek Content-Type jest brakujący lub nieprawidłowy.|
| 429000, 429001, 429002| Serwer odrzucił żądanie, ponieważ klient wysyła zbyt wiele żądań. Zmniejsz częstotliwość żądań, aby uniknąć ograniczenia przepustowości.|
| 500000| Wystąpił nieoczekiwany błąd. Jeśli błąd będzie się powtarzać, zgłoś to za pomocą daty/godziny wystąpienia błędu, poproś identyfikator odpowiedzi nagłówek X-RequestId i identyfikator klienta z nagłówek żądania X-ClientTraceId.|
| 503000| Usługa jest tymczasowo niedostępna. Spróbuj ponownie. Jeśli błąd będzie się powtarzać, zgłoś to za pomocą daty/godziny wystąpienia błędu, poproś identyfikator odpowiedzi nagłówek X-RequestId i identyfikator klienta z nagłówek żądania X-ClientTraceId.|

