---
title: Dodawanie użytkowników do usług AD FS Azure Stack | Dokumentacja firmy Microsoft
description: Dowiedz się, jak dodawać użytkowników do wdrożenia usług AD FS usługi Azure Stack
services: azure-stack
documentationcenter: ''
author: patricka
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.openlocfilehash: f8abacbcb05d1346931b5c2e1097660cfbd8e594
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344170"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Dodaj użytkowników usługi Azure Stack w usługach AD FS
Możesz użyć **użytkownicy usługi Active Directory i komputery** przystawki można dodać użytkowników do środowiska usługi Azure Stack, korzystając z usług AD FS jako dostawcy tożsamości.

## <a name="add-windows-server-active-directory-users"></a>Dodawanie użytkowników usługi Active Directory systemu Windows Server
> [!TIP]
> W tym przykładzie używa domyślnego azurestack.local ASDK usługi active directory. 

1.  Zaloguj się do komputera przy użyciu konta, zapewniając dostęp do narzędzi administracyjnych Windows i Otwórz nowe Microsoft Management Console (MMC).
2.  Kliknij przycisk **Plik > Dodaj lub Usuń przystawkę**.
3.  Wybierz **użytkowników usługi Active Directory i komputery** > **AzureStack.local** > **użytkowników**.
4.  Kliknij przycisk **akcji** > **nowe** > **użytkownika**.
5.  W nowy obiekt — użytkownik okna, podaj i Potwierdź hasło
6.  Kliknij przycisk **dalej** finalize wartości, a następnie kliknij przycisk Zakończ, aby utworzyć użytkownika.


## <a name="next-steps"></a>Kolejne kroki
[Tworzenie jednostek usługi](azure-stack-create-service-principals.md)