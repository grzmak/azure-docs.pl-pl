---
title: Tagowanie części mowy — interfejs API analizy językowej
description: Dowiedz się, jak znakowanie część mowy w interfejsu API analizy językowej identyfikuje kategorii lub część mowy wystąpień poszczególnych wyrazów tekstu.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 09/27/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: a01fcea4ae6c8950d578bacefc2f064586d7306b
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238528"
---
# <a name="part-of-speech-tagging"></a>Tagowanie części mowy

> [!IMPORTANT]
> Wersja zapoznawcza analizy językowej została zlikwidowana 9 sierpnia 2018 r. Firma Microsoft zaleca używanie [moduły analizy tekstu w usłudze Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) do przetwarzania tekstu i analizy.

## <a name="background-and-motivation"></a>Tło i motywację

Tekst został rozdzielony na zdania i tokeny, następnym krokiem analizy po do identyfikowania kategorii lub części mowy każdego wyrazu.
Należą do kategorii, takich jak *rzeczownik* (zazwyczaj reprezentujący osób, miejsc, rzeczy, pomysły, itp.) i *zlecenie* (zazwyczaj reprezentujących akcje, zmiany stanu itd. Dla niektórych słów części mowy jest jednoznaczna (na przykład *quagmire* jest tylko rzeczownik), ale w przypadku wielu innych jest trudno stwierdzić.
*Tabela* można to miejsce, w którym znajdują się (lub 2-D układ liczb), ale możesz również "Tabela dyskusję na temat".

## <a name="list-of-part-of-speech-tags"></a>Lista tagów części mowy

| Tag | Opis | Przykład słów |
|-----|-------------|---------------|
| $ | Dolar | $ |
| \`\` | cudzysłów otwierający | \` \`\` |
| '' | zamykającego znaku cudzysłowu | ' '' |
| ( | Nawias otwierający | ( [ { |
| ) | Nawias zamykający | ) ] } |
| ,  | Przecinkami | ,  |
| -- | kreska | -- |
| . | terminator zdania | . ! ? |
| : | dwukropek lub wielokropka | : ; ... |
| DW | razem i koordynowania działań | a, ale lub jeszcze|
| CIĄGŁE DOSTARCZANIE | cyfry kardynalną | dziewięć 1980 20 96 " |
| DT | determiner |wszystkie zarówno żadnego z tych celów|
| NA PRZYKŁAD | egzystencjalna miejsca | istnieje |
| ŚRODOWISKA .NET FRAMEWORK | obce słowa | enfant terrible hoi polloi je ne sais quoi |
| INDIE | preposition lub nadrzędne połączeniu| wewnątrz w przypadku po czy |
| JJ | przymiotnik lub liczb porządkowych | dziewiąty wielomodalnymi dość execrable |
| JJR | przymiotnik porównawczych | lepiej, szybciej tańsze |
| JJS | przymiotnik superlative | najlepsze najszybszych najtańszej |
| LS | Znacznik elementów listy | (b) 1 2 A B A. B. |
| MD | Pomocnik modalne | można może są będzie można może powinien programu |
| NN | rzeczownik, typowe, liczbę pojedynczą i urządzeń pamięci masowej | buta pieniądze ziemniaczanej |
| NNP | rzeczownik, odpowiednie, liczbę pojedynczą | Kennedy Roosevelt Chicago Weehauken |
| NNPS | rzeczownik, odpowiednie, w liczbie mnogiej | Krzewów Springfields |
| NNS | rzeczownik, wspólnego, w liczbie mnogiej | elementy myszy pola |
| (czas pacyficzny) | wstępne determiner | wszystkie zarówno połowie wiele dość takich się, że to |
| PUNKTU SPRZEDAŻY | genitive znacznika | "w |
| ZASADY REPLIKACJI HASEŁ | zrozumieć osobistych | ona on ją I będziemy ich możesz |
| ZASADY REPLIKACJI HASEŁ$ | zrozumieć dzierżawczego | Przekształca jego jego Moje naszych ich usługi |
| RB | Parametr | klinicznie tylko |
| RBR | Parametr porównawczych | dalsze gloomier grander graver większą grimmer trudniejsze poważniejszych lepszy wynik dotyczący większych wyższe jednak większe nowsze sprawniej dłuższy mniej doskonale mniejszym lonelier dłużej głośność małe więcej... |
| SPZ | Parametr superlative | najlepsze największych bluntest Najwcześniejsza położony pierwszy najdalszego najtrudniejsze heartiest najwyższy największych najmniej mniej większość najbliższym najgorszy drugi najbardziej restrykcyjne metody |
| JEDNOSTKI UZALEŻNIONEJ | Cząstka | Wyłącz na górę się o |
| SYMBOLI | Symbol | % & |
| DO | "do" jako znacznik preposition lub infinitive | na |
| NIESTETY | interjection | Niestety hura howdy hello |
| VB | zlecenie, formularza podstawowego | Nadaj bieżąco Przypisz |
| VBD | zlecenie, przeszłym | udostępniła przypisane poleciał |
| VBG | zlecenie, participle obecne lub gerund | zapewniając upływa przypisywanie |
| VBN | zlecenie, participle w przeszłości | biorąc pod uwagę przypisane przesyłane |
| VBP | zlecenie, teraźniejszego, nie 3rd osoby pojedynczej | Nadaj bieżąco Przypisz |
| VBZ | zlecenie, teraźniejszego, 3 osoby pojedynczej | zapewnia przypisuje prostej |
| WDT | Determiner pytania "Wh" | jakie który |
| WP | Zrozumieć pytania "Wh" | kto którego |
| WP$ | Pytania "Wh" zrozumieć dzierżawczego | którego |
| WRB | Parametr pytania "Wh" | jak jednak zawsze wtedy, gdy gdzie |

## <a name="specification"></a>Specyfikacja

Jak w przypadku tokenizacji, możemy polegać na specyfikacji z [Treebank partnerów](https://catalog.ldc.upenn.edu/ldc99t42).
