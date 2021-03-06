---
title: Zrozumienie, jak role są używane w jednostkach na podstawie wzorca
titleSuffix: Azure Cognitive Services
description: Role są podtypy nazwanych, kontekstowych podmiotu używana tylko we wzorcach. Na przykład Kup wypowiedź biletu z nowego Jorku do Londynu, zarówno w Nowym Jorku, jak i w Londynie są miast, ale każda ma inne znaczenie w zdaniu. Nowy Jork jest miasto źródła i Londyn jest miasta docelowego.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 5fda0ac590e5faeaa8b6ec44a7d649d2c0122eeb
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49352989"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Role jednostki we wzorcach są podtypy kontekstowych
Role są podtypy nazwanych, kontekstowych podmiotu używana tylko w [wzorców](luis-concept-patterns.md).

Na przykład w wypowiedź `buy a ticket from New York to London`, zarówno w Nowym Jorku, jak i w Londynie są miast, ale każda ma inne znaczenie w zdaniu. Nowy Jork jest miasto źródła i Londyn jest miasta docelowego. 

Role nadaj nazwę różnic:

|Jednostka|Rola|Przeznaczenie|
|--|--|--|
|Lokalizacja|źródło|Jeżeli płaszczyzny pozostawia z|
|Lokalizacja|miejsce docelowe|gdzie wyładowuje płaszczyzna|
|Wstępnie utworzone datetimeV2|na|Data zakończenia|
|Wstępnie utworzone datetimeV2|z|Data rozpoczęcia|

## <a name="how-are-roles-used-in-patterns"></a>Jak role są używane we wzorcach?
W polu wypowiedź szablonu wzorca role są używane w ramach wypowiedź: 

|Wzorzec z rolami jednostki|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Składnia roli we wzorcach
Jednostki i rola są ujęte w nawiasach, `{}`. Jednostka i rola są rozdzielone średnikiem. 

## <a name="roles-versus-hierarchical-entities"></a>Role i hierarchiczne jednostek
Jednostki hierarchiczną zapewnia te same informacje kontekstowe jako role, ale tylko wypowiedzi w **intencji**. Podobnie, role zawierają te same informacje kontekstowe jako hierarchiczna jednostki, ale tylko w **wzorców**.

|Learning kontekstowych|Używane w|
|--|--|
|Hierarchiczna jednostek|Intencji|
|role|Wzorce|

## <a name="roles-with-prebuilt-entities"></a>Role z wstępnie utworzonych jednostek

Za pomocą ról wstępnie utworzonych jednostek oferowanie znaczenie różnych wystąpień wstępnie utworzone jednostki w ramach wypowiedź. 

### <a name="roles-with-datetimev2"></a>Role z datetimeV2

Wstępnie utworzone jednostki, datetimeV2, wykonuje świetnie zrozumienia szeroką gamę różnych w daty i godziny w wypowiedzi. Można określić daty i zakresy dat inaczej niż wstępnie utworzone jednostki domyślnej interpretacji. 

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się, jak dodać [ról](luis-how-to-add-entities.md#add-role-to-pattern-based-entity)
