---
title: Kodowanie niestandardowej transformacji za pomocą usługi Azure Media Services v3 | Dokumentacja firmy Microsoft
description: W tym temacie pokazano, jak kodować niestandardowe przekształcenia przy użyciu usługi Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: c3318020b54beb121ac48d993a8ce794ac8e377f
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376639"
---
# <a name="how-to-encode-with-a-custom-transform"></a>Jak kodowanie za pomocą niestandardowej transformacji

Podczas kodowania za pomocą usługi Azure Media Services, należy można szybko rozpocząć pracę z jedną z zalecanych wbudowane ustawienia wstępne, oparte na najlepszych branżowych rozwiązań, jak pokazano w [strumieniowego przesyłania plików](stream-files-tutorial-with-api.md) samouczek, można też tworzyć niestandardowe Ustawienie wstępne pod kątem określonych wymagań scenariusza lub urządzenia. 

> [!Note]
> W usłudze Azure Media Services v3 są wszystkie kodowania szybkości transmisji bitów w bitach na sekundę. Stanowi to odmianę v2 REST, które ustawienia wstępne usługi Media Encoder Standard. Na przykład szybkości transmisji bitów w wersji 2 będzie można określić jako 128, ale w wersji 3 byłoby 128000.

## <a name="download-the-sample"></a>Pobierz przykład

Sklonuj repozytorium GitHub, który zawiera pełny przykład platformy .NET Core do komputera za pomocą następującego polecenia:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
Niestandardowe ustawienia wstępnego przykładowe znajduje się w [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) folderu.

## <a name="create-a-transform-with-a-custom-preset"></a>Tworzenie przekształcenia przy użyciu niestandardowego ustawienia wstępnego 

Podczas tworzenia nowego [Przekształcanie](https://docs.microsoft.com/rest/api/media/transforms), należy określić, co chcesz, aby wygenerować jako dane wyjściowe. Wymagany parametr to obiekt **TransformOutput**, jak pokazano w poniższym kodzie. Każdy obiekt **TransformOutput** zawiera element **Preset**. Element **Preset** zawiera szczegółowe instrukcje operacji przetwarzania wideo i/lub dźwięku używanych do wygenerowania wymaganego obiektu **TransformOutput**. Następujące **TransformOutput** tworzy niestandardowe ustawienia koderów-dekoderów i warstwy danych wyjściowych.

Podczas tworzenia obiektu [Transform](https://docs.microsoft.com/rest/api/media/transforms) należy najpierw sprawdzić, czy taki obiekt już istnieje, używając metody **Get**, tak jak przedstawiono w poniższym kodzie.  W Media Services v3 **uzyskać** metody na jednostkach zwracają **null** Jeśli jednostka nie istnieje (bez uwzględniania wielkości liter Sprawdź nazwy).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Kolejne kroki

[Przesyłanie strumieniowe plików](stream-files-tutorial-with-api.md) 
