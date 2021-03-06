---
title: Rozwiązywanie problemów z błędami interfejsu API raportowania usługi Azure Active Directory | Dokumentacja firmy Microsoft
description: Umożliwia rozwiązanie problemu dotyczącego błędów podczas wywoływania usługi Azure Active Directory interfejsy API raportowania usługi.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: a333e72312b3916b2f6ffb0703de0ff1655d6685
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057370"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Rozwiązywanie problemów z błędami interfejsu API raportowania usługi Azure Active Directory

W tym artykule wymieniono typowe komunikaty o błędach, które mogą występować podczas uzyskiwania dostępu do raportów aktywności przy użyciu interfejsu API programu Graph MS i kroki do ich rozwiązania.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 HTTP wewnętrzny błąd serwera podczas uzyskiwania dostępu do punktu końcowego programu Microsoft Graph w wersji 2

Obecnie obsługujemy punktu końcowego v2 programu Microsoft Graph — upewnij się, że dostęp do dzienników aktywności, przy użyciu punktu końcowego v1 programu Microsoft Graph.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Błąd: Nie można pobrać ról użytkownika z programu Graph usługi AD

Może wystąpić ten komunikat o błędzie podczas próby dostępu do logowania za pomocą Eksploratora programu Graph. Upewnij się, że użytkownik jest zalogowany na koncie przy użyciu zarówno przycisków logowania w Interfejsie użytkownika programu Graph Explorer, jak pokazano na poniższej ilustracji. 

![Graph Explorer](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Błąd: Nie można wykonać sprawdzenia licencji premium z programu Graph usługi AD 

Jeśli napotkasz ten komunikat o błędzie podczas próby uzyskania dostępu do logowania za pomocą Eksploratora programu Graph, wybierz **zmodyfikować uprawnienia** poniżej swoje konto w lewym okienku nawigacji, a następnie wybierz **Tasks.ReadWrite** i **Directory.Read.All**. 

![Modyfikowanie uprawnień interfejsu użytkownika](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Błąd: Dzierżawy jest B2C ani dzierżawa nie ma licencji premium

Uzyskiwanie dostępu do raportów logowania wymaga usługi Azure Active Directory — wersja premium 1 (P1) licencji. Jeśli widzisz ten komunikat o błędzie podczas uzyskiwania dostępu do logowania, upewnij się, że dzierżawy ma licencję z licencją Azure AD P1.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Błąd: Użytkownik nie jest dozwolone role 

Jeśli widzisz ten komunikat o błędzie podczas próby uzyskania dostępu do dzienników inspekcji lub logowania za pomocą interfejsu API upewnij się, że Twoje konto jest częścią **Czytelnik zabezpieczeń** lub **czytelnika raportów** roli w usłudze Azure Active Directory Dzierżawca. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Błąd: Aplikacja Brak uprawnień Czytaj dane katalogu usługi AAD 

Wykonaj poniższe kroki w [wymagania wstępne dotyczące dostępu do usługi Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md) aby upewnić się, aplikacja jest uruchomiona przy użyciu odpowiedniego zestawu uprawnień. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Błąd: Aplikacja Brak uprawnień "Odczytywać wszystkie dane dziennika inspekcji" MSGraph interfejsu API

Wykonaj poniższe kroki w [wymagania wstępne dotyczące dostępu do usługi Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md) aby upewnić się, aplikacja jest uruchomiona przy użyciu odpowiedniego zestawu uprawnień. 

## <a name="next-steps"></a>Następne kroki

[Użyj inspekcji, dokumentacja interfejsu API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit)
[użycia odwołania raportu interfejsu API logowań](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)