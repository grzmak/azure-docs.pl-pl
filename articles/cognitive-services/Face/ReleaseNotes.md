---
title: Informacje o wersji — Usługa interfejsu API rozpoznawania twarzy
titleSuffix: Azure Cognitive Services
description: Informacje o wersji dla usługi interfejsu API rozpoznawania twarzy zawierają historię zmian wersji w różnych wersjach.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6fd3d33d40b0ed142127e46dd7c9173de39947c7
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121995"
---
# <a name="face-api-release-notes"></a>Informacje o wersji interfejsu API rozpoznawania twarzy

Ten artykuł odnosi się do usługi interfejsu API rozpoznawania twarzy w wersji 1.0.

### <a name="release-changes-in-may-2018"></a>Zmiany wersji w maju 2018 r.

* Ulepszone `gender` atrybut ulepszona znacznie, a także `age`, `glasses`, `facialHair`, `hair`, `makeup` atrybutów. Korzystać z nich w ramach [twarzy — wykrywanie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametru. 

* Zwiększona wejściowego pliku limit rozmiaru obrazu z 4 MB do 6 MB w [twarzy — wykrywanie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [osoba grupie — dodawanie Twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) i [osoba LargePersonGroup — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Zmiany wersji marca 2018 r.

* Dodano kontener milionowej skali: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) i [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Więcej szczegółów w [sposób użycia funkcji na dużą skalę](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Zwiększono [twarzy — ustalenie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` parametr [1, 5] do [1, 100] i domyślne do 10.

### <a name="release-changes-in-may-2017"></a>Zmiany wersji maja 2017 r.

* Dodano `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure`, i `noise` atrybutów w [twarzy — wykrywanie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametru.

* Obsługiwane 10K osób w grupie i [twarzy — ustalenie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Obsługiwane dzielenia na strony w [osoba grupie — lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) z opcjonalnymi parametrami: `start` i `top`.

* Obsługiwane współbieżności podczas dodawania/usuwania twarzy wobec różnych liście twarzy i różnych osób w grupie.

### <a name="release-changes-in-march-2017"></a>Zmiany wersji marca 2017 r.
* Dodano `emotion` atrybutu w [twarzy — wykrywanie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametru.

* Stały, nie można ponownie wykryte krój z prostokątowi zwróconemu z [twarzy — wykrywanie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) jako `targetFace` w [FaceList — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) i [grupie osoba — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Ustalony rozmiar wykrywalny twarzy, aby upewnić się, że jest on dokładnie od 36 x 36 do 4096 x 4096 pikseli.

### <a name="release-changes-in-november-2016"></a>Zmiany wersji listopada 2016 r.
* Dodaje standardowy magazyn twarzy subskrypcji na przechowywanie dodatkowych twarzy w przypadku korzystania z [grupie osoba — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) lub [FaceList — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) potrzeby identyfikacji lub dopasowywania podobieństw. Opłata za przechowywane obrazy wynosi 0,5 $ za 1000 twarzy i ten kurs jest naliczana codziennie. Subskrypcje w ramach warstwy bezpłatna w dalszym ciągu ograniczone do 1000, łączna liczba osób.

### <a name="release-changes-in-october-2016"></a>Zmiany wersji października 2016 r.
* Zmienić komunikat o błędzie z więcej niż jeden twarzy targetFace z "Istnieją więcej niż jeden twarzy na obrazie" do "Ma więcej niż jeden twarzy na obrazie" w [FaceList — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) i [grupie osoba — Dodaj twarzy](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Zmiany wersji lipca 2016 r.
* Obsługiwane twarzy do uwierzytelniania obiektu osoba w [twarzy — weryfikowanie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Dodano opcjonalne `mode` parametr, umożliwiając wybór dwa tryby pracy: `matchPerson` i `matchFace` w [twarzy — Znajdź podobne](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) i wartość domyślna to `matchPerson`.

* Dodano opcjonalne `confidenceThreshold` parametr dla użytkownika ustawić próg czy jednej powierzchni należy do obiektu osoba w [twarzy — ustalenie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Dodano opcjonalne `start` i `top` parametrów w [grupie — lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) umożliwia użytkownikowi określenie punktu początkowego i łączną liczbę grup osób, do listy.

### <a name="v10-changes-from-v0"></a>Zmiany w wersji 1.0 z V0
* Zaktualizowano usługę główny punkt końcowy z ```https://westus.api.cognitive.microsoft.com/face/v0/``` do ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Zmiany zostały wprowadzone do: [twarzy — wykrywanie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [twarzy — ustalenie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [twarzy — Znajdź podobne](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) i [twarzy — grupa](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* Zaktualizowano rozmiar minimalny wykrywalny twarzy do 36 x 36 pikseli. Mniejsze niż 36 x 36 pikseli twarzy nie zostanie wykryty.

* Przestarzałe dane grupie i osoby V0 twarzy. Te dane nie są dostępne za pomocą usługi Face w wersji 1.0.

* Przestarzałe V0 punkt końcowy interfejsu API rozpoznawania twarzy w dniu 30 czerwca 2016 r.
