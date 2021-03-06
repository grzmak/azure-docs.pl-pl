---
title: Omówienie przechowywania wersji usługi LUIS
titleSuffix: Azure Cognitive Services
description: Dowiedz się, jak używać wersji do zarządzania zmianami w Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 5d78172ae441300cdc39df8b911fd7ecaa42df9f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032699"
---
# <a name="versions"></a>Wersje
Utwórz różne modele taką samą aplikację na [wersji](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>Identyfikator wersji
Identyfikator wersji zawiera znaki, cyfry lub "." i nie może być dłuższa niż 10 znaków.

## <a name="initial-version"></a>Wersja początkowa
Wersja początkowa (0,1) to domyślna wersja active. 

## <a name="active-version"></a>Wersja aktywna
Aby [Ustaw wersję](luis-how-to-manage-versions.md#set-active-version) jako aktywny oznacza jest obecnie edytować i testowane na platformie [LUIS](luis-reference-regions.md) witryny sieci Web. Ustaw wersję jako aktywny, aby uzyskać dostęp do swoich danych, wprowadzenia aktualizacji, jak również testowanie i opublikować go.

Nazwa aktualnie aktywnego wersji jest wyświetlane w panelu po lewej stronie, górna po nazwie aplikacji. 

[ ![Zmień wersję active](./media/luis-concept-version/version-in-nav-bar-inline.png) ](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Wersje i gniazda publikowania
Opublikuj etapu i produktu miejsca. Każdego miejsca może mieć inną wersję lub tej samej wersji. Jest to przydatne w celu sprawdzenia zmian między wersjami modelu za pośrednictwem punktu końcowego, który jest dostępny do botów lub inne usługi LUIS wywołanie aplikacji. 

## <a name="clone-a-version"></a>Klonowanie wersji
Klonuj wersji, aby utworzyć kopię istniejącej wersji i zapisać ją jako nową wersję. Klonuj wersji, aby użyć tej samej zawartości istniejącej wersji jako punktu wyjścia dla nowej wersji. Po klonowania wersji staje się nowej wersji **active** wersji. 

## <a name="import-and-export-a-version"></a>Importowanie i eksportowanie wersji
Można zaimportować wersji na poziomie aplikacji. Ta wersja staje się aktywny wersji i używane identyfikator wersji we właściwości "versionId" pliku aplikacji. Można również zaimportować się na poziomie wersji, do istniejącej aplikacji. Nowa wersja staje się aktywny wersji. 

Możesz wyeksportować wersji na poziomie aplikacji, lub możesz wyeksportować wersji na poziomie wersji. Jedyna różnica polega na czy wersja wyeksportowanych poziomie aplikacji jest aktualnie aktywne wersji znajduje się na poziomie wersji, możesz wybrać dowolnej wersji, aby wyeksportować na **[ustawienia](luis-how-to-manage-versions.md)** strony. 

Wyeksportowany plik nie zawiera informacji przedstawiono maszyny, ponieważ aplikacja jest retrained po ich zaimportowaniu. Wyeksportowany plik nie zawiera współpracowników — należy dodać te wstecz po wersji są importowane do nowej aplikacji.

## <a name="export-each-version-as-app-backup"></a>Eksportowanie każdej wersji jako kopia zapasowa aplikacji
Aby utworzyć kopię zapasową aplikacją usługi LUIS, należy wyeksportować poszczególnych wersji na **[ustawienia](luis-how-to-manage-versions.md)** strony.

## <a name="delete-a-version"></a>Usuwanie wersji
Wszystkie wersje, z wyjątkiem aktywnej wersji można usunąć z listy wersji na stronie Ustawienia. 

## <a name="version-availability-at-the-endpoint"></a>Dostępność wersji w punkcie końcowym
Uczony wersje nie są automatycznie dostępne w Twojej aplikacji [punktu końcowego](luis-glossary.md#endpoint). Należy najpierw [publikowania](luis-how-to-publish-app.md) lub ponownie opublikować wersję w kolejności, aby była dostępna w punkcie końcowym w aplikacji. Możesz opublikować **przemieszczania** i **produkcji**, co daje maksymalnie dwie wersje aplikacji, które są dostępne w punkcie końcowym. Jeśli potrzebujesz więcej wersji aplikacji, które są dostępne w punkcie końcowym, możesz wyeksportować wersję i ponownie zaimportować do nowej aplikacji. Nowa aplikacja ma identyfikator innej aplikacji.

## <a name="collaborators"></a>Współpracownicy
Właściciela i wszystkie [współpracowników](luis-how-to-collaborate.md) mają pełny dostęp do wszystkich wersji aplikacji.

## <a name="next-steps"></a>Kolejne kroki

Zobacz, jak dodać [versioning](luis-how-to-manage-versions.md) na stronie Ustawienia aplikacji. 

Dowiedz się, jak projektować [intencji](luis-concept-intent.md) do modelu.