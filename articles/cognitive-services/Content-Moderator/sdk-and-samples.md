---
title: Zestawy SDK i przykłady — Content Moderator, Python, Java, Node.js i .NET
titlesuffix: Azure Cognitive Services
description: Pobierz zestawy SDK i przykłady dla usług Content Moderator
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: a57f6a312b00d7ec3d927c6fda319f1de8663c9c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220454"
---
# <a name="content-moderator-sdks-and-samples"></a>Zestawy SDK i przykłady dla usług Content Moderator

## <a name="sdks-for-python-java-nodejs-and-net"></a>Zestawy SDK dla języków Python, Java oraz platform Node.js i .NET

- [Zestaw SDK Content Moderator dla języka Python](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [Zestaw SDK Content Moderator dla języka Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [Zestaw SDK Content Moderator dla platformy Node.js](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [Zestaw SDK Content Moderator dla platformy .Net](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>Przykłady zestawów SDK dla platformy .NET

Poniższa lista zawiera hiperlinki do przykładów kodu utworzonych przy użyciu zestawu SDK Azure Content Moderator dla platformy .NET.

- **Biblioteka pomocnika**: [tworzenie klienta usług Content Moderator do użytku w innych przykładach](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Zobacz [Szybki start](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Moderowanie 
- **Moderowanie obrazów**: [ocena obrazu pod względem zawartości erotycznej i przeznaczonej dla osób dorosłych, tekstu i twarzy](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Zobacz [Szybki start](image-moderation-quickstart-dotnet.md).
- **Obrazy niestandardowe**: [moderowanie przy użyciu niestandardowych list obrazów](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Zobacz [Szybki start](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Istnieje maksymalny limit wynoszący **5 list obrazów**, a poszczególne listy **nie mogą przekraczać 10 000 obrazów**.
>

- **Moderowanie tekstu**: [sprawdzanie tekstu w celu wykrycia wulgaryzmów i danych osobowych](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Zobacz [Szybki start](text-moderation-quickstart-dotnet.md).
- **Terminy niestandardowe**: [moderowanie przy użyciu niestandardowych list terminów](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Zobacz [Szybki start](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Istnieje maksymalny limit wynoszący **5 list terminów**, a poszczególne listy **nie mogą przekraczać 10 000 terminów**.
>

- **Moderowanie filmów wideo**: [skanowanie wideo w celu wykrycia zawartości erotycznej i dla dorosłych oraz uzyskiwanie wyników](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Zobacz [Szybki start](video-moderation-api.md).

### <a name="review"></a>Przegląd
- **Zadania obrazów**: [uruchamianie zadania moderowania, które skanuje, a następnie tworzy przeglądy](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Zobacz [Szybki start](moderation-jobs-quickstart-dotnet.md).
- **Przeglądy obrazów**: [tworzenie przeglądów wymagających interakcji z człowiekiem](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Zobacz [Szybki start](moderation-reviews-quickstart-dotnet.md).
- **Przeglądy wideo**: [tworzenie przeglądów wideo wymagających interakcji z człowiekiem](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Zobacz [Szybki start](video-reviews-quickstart-dotnet.md)
- **Przeglądy transkrypcji wideo**: [tworzenie przeglądów transkrypcji wideo wymagających interakcji z człowiekiem](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Zobacz [Szybki Start](video-reviews-quickstart-dotnet.md)

Zobacz wszystkie przykłady dla platformy .NET wśród [przykładów użycia usług Content Moderator na platformie .NET w witrynie GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

## <a name="rest-api-samples-in-c"></a>Przykłady interfejsu API REST w języku C#

- [Moderowanie obrazów](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Moderowanie tekstu](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Moderowanie filmów wideo](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Przeglądy obrazów](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [Zadania obrazów](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

Aby uzyskać przewodniki dotyczące tych przykładów, zapoznaj się [seminarium internetowym na żądanie](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html).

## <a name="tutorials"></a>Samouczki
- [Moderowanie katalogu handlu elektronicznego](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Zobacz [samouczek](ecommerce-retail-catalog-moderation.md).
- [Moderowanie zawartości witryny Facebook](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Zobacz [samouczek](facebook-post-moderation.md).
- [Rozwiązanie do moderowania i przeglądów wideo i transkrypcji](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Zobacz [samouczek](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>Seminaria internetowe na żądanie
- [Wspomagane maszynowo moderowanie zawartości na dużą skalę w usługach Content Moderator](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
