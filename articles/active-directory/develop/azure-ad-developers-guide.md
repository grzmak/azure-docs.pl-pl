---
title: W wersji 1.0 — informacje | Azure
description: Ten artykuł zawiera omówienie logowania firmy Microsoft roboczych i kont służbowych za pomocą usługi Azure Active Directory w wersji 1.0 w punkcie końcowym i platformy.
services: active-directory
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1efaafabbe82fccab79e64fdee8ce19590e6b6e7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964142"
---
# <a name="about-v10"></a>Informacje w wersji 1.0

Azure Active Directory (Azure AD) to usługa zarządzania tożsamościami w chmurze, która umożliwia deweloperom tworzenie aplikacji zapewniających bezpieczne logowanie użytkowników mających konta służbowe Microsoft. Usługa Azure AD obsługuje zarówno deweloperów tworzących aplikacje biznesowe dla pojedynczej dzierżawy, jak i deweloperów chcących tworzyć aplikacje wielodostępne. Oprócz podstawowego logowania usługa Azure AD umożliwia również aplikacjom wywoływanie interfejsów API firmy Microsoft, takich jak [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/overview), a także niestandardowych interfejsów API opartych na platformie Azure AD. W tej dokumentacji pokazano, jak dodać obsługę usługi Azure AD do własnej aplikacji, używając standardowych w branży protokołów, takich jak OAuth 2.0 i OpenID Connect.

> [!NOTE]
> Większość zawartości na tej stronie koncentruje się na v1.0 punktu końcowego i platformy, która obsługuje tylko firma Microsoft kont służbowych. Jeśli chcesz zarejestrować konta użytkownika lub osobistych kont Microsoft, zapoznaj się z informacjami na [punktu końcowego v2.0 i platforma](active-directory-appmodel-v2-overview.md). Punkt końcowy w wersji 2.0 oferuje ujednolicone środowisko deweloperów dla aplikacji, które chcesz zarejestrować na wszystkich tożsamości firmy Microsoft.

| | |
| --- | --- |
|[Informacje podstawowe o uwierzytelnianiu](authentication-scenarios.md) | Wprowadzenie do uwierzytelniania za pomocą usługi Azure AD. |
|[Typy aplikacji](app-types.md) | Przegląd scenariuszy uwierzytelniania obsługiwanych przez usługę Azure AD. |
| | |

## <a name="get-started"></a>Rozpoczęcie pracy

V1.0 przewodników Szybki Start i samouczków opisano tworzenie aplikacji na jego preferowanej platformie przy użyciu usługi Azure AD Authentication Library (ADAL) zestawu SDK. Zobacz **v1.0 przewodników Szybki Start** i **v1.0 samouczki** w [platforma tożsamości firmy Microsoft (Azure Active Directory dla deweloperów)](index.yml) na rozpoczęcie pracy.

## <a name="how-to-guides"></a>Przewodniki z instrukcjami

Zobacz **v1.0 prowadzi instrukcje** Aby uzyskać szczegółowe informacje i wskazówki dotyczące typowych zadań w usłudze Azure AD.

## <a name="reference-topics"></a>Tematy informacyjne

Następujące artykuły zawierają szczegółowe informacje o interfejsach API, komunikatach protokołów i terminach używanych w usłudze Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Biblioteki uwierzytelniania (ADAL)](active-directory-authentication-libraries.md)   | Omówienie bibliotek i zestawów SDK dostarczanych przez usługę Azure AD. |
| [Przykłady kodu](sample-v1-code.md)                                  | Lista wszystkich przykładów kodu usługi Azure AD. |
| [Słownik](developer-glossary.md)                                      | Terminologia i definicje słów używanych w tej dokumentacji. |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
