---
title: Migrowanie z API mowy usługi Translator z usługą mowy
titleSuffix: Azure Cognitive Services
description: W tym temacie umożliwia migrację aplikacji z interfejsu API tłumaczenia mowy do usługi rozpoznawania mowy.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: b50cdc6978519c0ec9da447c324237c00577d9fd
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885276"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migrowanie z API mowy usługi Translator z usługą mowy

Skorzystaj z tego artykułu do migracji aplikacji z interfejsu API mowy usługi Translator firmy Microsoft do [usługa rozpoznawania mowy](index.yml). Ten przewodnik przedstawia różnice między interfejsu API tłumaczenia mowy i usługa rozpoznawania mowy i sugeruje Strategie migracji aplikacji.

> [!NOTE]
> Klucz interfejsu API tłumaczenia mowy subskrypcji nie zostanie zaakceptowane przez usługę rozpoznawania mowy. Należy rozpocząć nową subskrypcję usługi mowy.

## <a name="comparison-of-features"></a>Porównanie funkcji

| Cecha                                           | Interfejs API tłumaczenia mowy w usłudze Translator                                  | Usługa rozpoznawania mowy | Szczegóły                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tłumaczenie tekstu                               | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tłumaczenie na mowę                             | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Globalny punkt końcowy                                   | : heavy_check_mark:                                              | : heavy_minus_sign:                 | Usługa rozpoznawania mowy nie oferuje obecnie globalny punkt końcowy. Globalny punkt końcowy może automatycznie kierować ruch do najbliższego regionalnych punktu końcowego, zmniejsza opóźnienie w aplikacji.                                                    |
| Regionalne punktów końcowych                                | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Limit czasu połączenia                             | 90 minut                                               | Bez ograniczeń za pomocą zestawu SDK. 10 minut z połączeniem websocket                                                                                                                                                                                                                                                                                   |
| Klucz uwierzytelniania w nagłówku                                | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Wiele języków przetłumaczyć w pojedyncze żądanie | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Dostępne są zestawy SDK                                    | : heavy_minus_sign:                                              | : heavy_check_mark:                 | Zobacz [dokumentacji usługi mowy](index.yml) dla dostępnych zestawów SDK.                                                                                                                                                    |
| Połączeń protokołu Websocket                             | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Języki interfejsu API                                     | : heavy_check_mark:                                              | : heavy_minus_sign:                 | Usługa rozpoznawania mowy obsługuje tego samego zakresu opisanego w językach [Dokumentacja języków interfejsu API usługi Translator](../translator-speech/languages-reference.md) artykułu. |
| Filtr wulgaryzmów i znacznika                       | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . WAV/PCM jako dane wejściowe                                 | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Inne typy plików jako dane wejściowe                         | : heavy_minus_sign:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Wyniki częściowe                                   | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Informacje o chronometrażu                                       | : heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                 |
| Identyfikator korelacji                                    | : heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Niestandardowe modele mowy                              | : heavy_minus_sign:                                              | : heavy_check_mark:                 | Usługa rozpoznawania mowy oferuje niestandardowe modele mowy dopasowane umożliwiają dostosowywanie rozpoznawania mowy do słownictwa unikatowy w Twojej organizacji.                                                                                                                                           |
| Modele tłumaczenia niestandardowych                         | : heavy_minus_sign:                                              | : heavy_check_mark:                 | Subskrybowanie interfejsu API tłumaczenia tekstu firmy Microsoft pozwala na użycie [niestandardowe w usłudze Translator](https://www.microsoft.com/translator/business/customization/) (obecnie w wersji zapoznawczej) można skorzystać z własnych danych w poszukiwaniu tłumaczeń bardziej precyzyjne operacje.                                                 |

## <a name="migration-strategies"></a>Strategie migracji

Jeśli Ty lub Twoja organizacja ma aplikacje w rozwoju i produkcji, które używają interfejsu API tłumaczenia mowy, należy zaktualizować je do korzystania z usługi rozpoznawania mowy. Zobacz [usługa rozpoznawania mowy](index.yml) dokumentacji dostępne zestawy SDK, przykłady kodu i samouczkom. Poniżej przedstawiono niektóre kwestie należy wziąć pod uwagę podczas migracji:

* Usługa rozpoznawania mowy nie oferuje obecnie globalny punkt końcowy. Należy określić, jeśli aplikacja będzie działać wydajnie za pomocą pojedynczego punktu końcowego regionalne dla wszystkich jego ruchu. Jeśli go nie będzie konieczne będzie umożliwia używanie funkcji geolokalizacji określić najbardziej efektywny sposób punktu końcowego.

* Jeśli aplikacja używa długotrwałe połączeń i nie można używać dostępnych zestawów SDK, można użyć połączenia protokołu websocket i zarządzać limit czasu 10 minut przez ponowne połączenie się w odpowiednim czasie.

* Jeśli aplikacja korzysta z interfejsu API tłumaczenia tekstu i interfejsu API tłumaczenia mowy, aby umożliwić modele tłumaczenia niestandardowych, można dodać identyfikatorów "Kategoria" bezpośrednio przy użyciu usługi mowy.

* Usługa rozpoznawania mowy może wykonać tłumaczenia na języki pojedyncze żądanie, w przeciwieństwie do interfejsu API tłumaczenia mowy.

## <a name="next-steps"></a>Kolejne kroki

* [Wypróbuj bezpłatnie usługę rozpoznawania mowy](get-started.md)
* [Szybki Start: Rozpoznawanie mowy w aplikacji platformy uniwersalnej systemu Windows przy użyciu zestawu SDK rozpoznawania mowy](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Zobacz także

* [Co to jest usługa mowy](overview.md)
* [Dokumentacja usługi rozpoznawania mowy i zestawu SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
