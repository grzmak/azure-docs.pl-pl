---
title: Tłumaczenie mowy — informacje
description: Omówienie funkcji tłumaczenia mowy
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: erhopf
ms.openlocfilehash: f3c22589270ccee2c60ca5ea5b15047d9c81fabe
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165441"
---
# <a name="about-the-speech-translation-api"></a>Interfejs API tłumaczenia mowy — informacje

Interfejs API mowy usługi Microsoft umożliwia dodanie end-to-end, w czasie rzeczywistym, wielu języków tłumaczenia mowy do aplikacji, narzędzi i urządzeń. Tego samego interfejsu API może służyć do tłumaczenia mowy do rozpoznawania mowy i rozpoznawania mowy na tekst.

Za pomocą interfejsu API mowy usługi Translator firmy Microsoft aplikacje klienckie przesyłanie strumieniowe audio mowy do usługi i ponownie otrzymywać strumień wyników. Te wyniki obejmują rozpoznany tekst w języku źródła i ich tłumaczeniem w języku docelowym. Tymczasowe tłumaczenia mogą być dostarczone do czasu ukończenia wypowiedź, co ostateczne tłumaczenie jest dostarczane.

Opcjonalnie syntetyzowany dźwiękową wersję ostateczne tłumaczenie można przygotować, umożliwiając true tłumaczenia mowy do rozpoznawania mowy.

Interfejs API tłumaczenia mowy używa protokołu Websocket, aby zapewnić komunikację pełnodupleksową kanał między klientem a serwerem. Ale nie trzeba radzenia sobie z Websocket; zestaw SDK rozpoznawania mowy, obsługuje za Ciebie.

Interfejs API tłumaczenia mowy wykorzystuje te same technologie, które stanowią podstawę różnych produktów i usług. Ta usługa jest już używana przez tysiące firm na całym świecie w swoich aplikacji i przepływów pracy.

## <a name="about-the-technology"></a>Technologię

Podstawowy aparat tłumaczenia firmy Microsoft są dwa różne podejścia: statystycznego tłumaczenia maszynowego (SMT) i neuronowego tłumaczenia maszynowego (NMT). Drugim, sztucznej inteligencji podejścia wykorzystujące sieci neuronowych, jest bardziej nowoczesnego podejścia do tłumaczenia maszynowego. NMT oferuje lepsze tłumaczenie — nie tylko dokładniejsze, ale również bardziej fluent i fizycznych. Główną przyczyną tej płynności jest fakt, że technologia NMT używa do tłumaczenia słów pełnego kontekstu zdania.

Już dziś Microsoft została zmigrowana na NMT dla najbardziej popularnych języków, wykorzystujące SMT tylko dla rzadziej używanych języków. Wszystkie [języki dostępne do tłumaczenia mowy do mowy](language-support.md#speech-translation) są obsługiwane przez NMT. Tłumaczenie mowy na tekst mogą używać SMT lub NMT, w zależności od pary języka. Jeśli język docelowy jest obsługiwany przez technologię NMT, pełne tłumaczenie jest zarządzane przez technologię NMT. Jeśli język docelowy nie jest obsługiwany przez NMT, tłumaczenie jest hybrydowa NMT i SMT, przy użyciu języka angielskiego jako "pivot" między dwa języki.

Różnice między modelami są wewnętrzne w aparacie tłumaczenia. Użytkownicy końcowi Zwróć uwagę, tylko jakości tłumaczenia ulepszone, szczególnie w przypadku chińskim, japońskim i arabskim.

> [!NOTE]
> Chcesz dowiedzieć się więcej o technologia aparatu tłumaczenia firmy Microsoft? Zobacz [tłumaczenia maszynowego](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Kolejne kroki

* [Pobierz subskrypcję usługi mowy w wersji próbnej](https://azure.microsoft.com/try/cognitive-services/)
* [Zobacz, jak tłumaczenie mowy w języku C#](how-to-translate-speech-csharp.md)
* [Zobacz, jak tłumaczenie mowy w języku C++](how-to-translate-speech-cpp.md)
* [Zobacz, jak tłumaczenie mowy w języku Java](how-to-translate-speech-java.md)
