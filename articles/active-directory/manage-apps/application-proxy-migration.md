---
title: Przeprowadź uaktualnienie do serwera Proxy aplikacji usługi Azure AD | Dokumentacja firmy Microsoft
description: Wybierz, które rozwiązanie serwera proxy jest przydatna, jeśli wykonujesz uaktualnienie z programu Microsoft Forefront lub ujednoliconej bramy dostępu.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c4ecb812156eae7402065cff4dc4bae3aef1554b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365179"
---
# <a name="compare-remote-access-solutions"></a>Porównanie rozwiązań dostępu zdalnego

Usługa Azure Active Directory serwera Proxy aplikacji jest jednym z dwóch rozwiązań dostępu zdalnego, które firma Microsoft oferuje. Druga to Proxy aplikacji sieci Web, — lokalną wersją. Te dwa rozwiązania zastąpienia starszych produktów, które oferowane przez firmy Microsoft: Microsoft Forefront Threat Management bramy (TMG) i ujednolicone dostępu do bramy (UAG). Użyj w tym artykule, aby zrozumieć, jak te cztery rozwiązania wypadają w porównaniu do siebie nawzajem. Dla osób, które nadal przy użyciu przestarzałe rozwiązania serwera TMG lub UAG należy użyć w tym artykule do zaplanowania migracji do jednego serwera Proxy aplikacji. 


## <a name="feature-comparison"></a>Porównanie funkcji

Aby dowiedzieć się, jak Threat Management Gateway (TMG), Unified dostępu do bramy (UAG), serwer Proxy aplikacji sieci Web (WAP) i serwera Proxy aplikacji usługi Azure AD (AP) wypadają w porównaniu do siebie nawzajem, należy użyć tej tabeli.

| Cecha | TMG | UAG | WAP | Interfejs API |
| ------- | --- | --- | --- | --- |
| Uwierzytelnianie certyfikatów | Yes | Yes | - | - |
| Selektywne publikowanie aplikacji przeglądarki | Yes | Yes | Yes | Yes |
| Wstępnego uwierzytelniania i pojedynczego logowania jednokrotnego | Yes | Yes | Yes | Yes | 
| Warstwy 2/3 zapory | Yes | Yes | - | - |
| Funkcje serwera proxy do przodu | Yes | - | - | - |
| Funkcje sieci VPN | Yes | Yes | - | - |
| Obsługa protokołu zaawansowane | - | Yes | Tak, jeśli uruchomiony za pośrednictwem protokołu HTTP | Tak, jeśli uruchomiony za pośrednictwem protokołu HTTP lub za pośrednictwem bramy usług pulpitu zdalnego |
| Służy jako serwer proxy usług AD FS | - | Yes | Yes | - |
| Jednego portalu, aby uzyskać dostęp do aplikacji | - | Yes | - | Yes |
| Tłumaczenie łącze treści odpowiedzi | Yes | Yes | - | Yes | 
| Uwierzytelnianie przy użyciu nagłówków | - | Yes | - | Tak, z usługą PingAccess | 
| Zabezpieczenia w skali chmury | - | - | - | Yes | 
| Dostęp warunkowy | - | Yes | - | Yes |
| Brak składników w sieci obwodowej | - | - | - | Yes |
| Nie połączeń przychodzących | - | - | - | Yes |

W przypadku większości scenariuszy zaleca się aplikacja usługi Azure AD jako nowoczesnych rozwiązań. Serwer Proxy aplikacji sieci Web jest tylko preferowane w scenariuszach, które wymagają serwera proxy usług AD FS i nie można użyć domenom niestandardowym w usłudze Azure Active Directory. 

Serwer Proxy aplikacji usługi Azure AD zapewnia unikalne korzyści w porównaniu do podobne produkty, w tym:

- Rozszerzenie usługi Azure AD do zasobów lokalnych
   - Zabezpieczenia w skali chmury i ochrona
   - Funkcje, takie jak dostęp warunkowy i uwierzytelniania wieloskładnikowego można łatwo włączyć
- Nie componenet w strefą zdemilitaryzowaną
- Nie połączeń przychodzących, wymagane
- Panel dostępu jednego, użytkownicy mogą przejść do dla wszystkich swoich aplikacji, w tym usługi Office 365, Azure AD zintegrowanych aplikacji SaaS i lokalnych aplikacji sieci web. 


## <a name="next-steps"></a>Kolejne kroki

- [Użyj usługi Azure AD aplikacji, aby zapewnić bezpieczny dostęp zdalny do aplikacji lokalnych](application-proxy.md)
- [Przejście z programu Forefront TMG i UAG do serwera Proxy aplikacji](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
