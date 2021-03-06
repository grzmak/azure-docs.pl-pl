---
title: Zarządzanie dostępem do zarządzania platformy Azure przy użyciu dostępu warunkowego w usłudze Azure Active Directory
description: Więcej informacji na temat przy użyciu dostępu warunkowego w usłudze Azure AD w celu zarządzania dostępem do zarządzania platformy Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 083cb4eb84746f4a61b51f3573a0bf66110fe1ee
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435052"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Zarządzanie dostępem do zarządzania platformy Azure przy użyciu dostępu warunkowego

Dostęp warunkowy w usłudze Azure Active Directory (Azure AD) steruje dostępem do aplikacji na podstawie określonych warunków, które określisz w chmurze. Aby zezwolić na dostęp, należy utworzyć zasady dostępu warunkowego, które zezwalania lub blokowania dostępu opartego na czy są spełnione wymagania zasad. 

Zazwyczaj używasz dostępu warunkowego w celu kontrolowania dostępu do aplikacji w chmurze. Możesz również skonfigurować zasady, do kontrolowania dostępu do zarządzania platformy Azure na podstawie określonych warunków (np. ryzyko logowania, lokalizacji lub urządzeń) oraz do wymuszania wymagań, takich jak uwierzytelnianie wieloskładnikowe.

Aby utworzyć zasady zarządzania systemu Azure, musisz wybrać **Microsoft Azure Management** w obszarze **aplikacje w chmurze** podczas wybierania aplikacji, dla której chcesz zastosować zasady.

![Dostęp warunkowy do zarządzania platformy Azure](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Zasady, które możesz utworzyć ma zastosowanie do wszystkich punktów końcowych zarządzania platformy Azure, łącznie z klasycznej witryny Azure portal, witryny Azure portal, dostawca usługi Azure Resource Manager, klasyczne, interfejsów API zarządzania usługami i programu Azure PowerShell.

> [!CAUTION]
> Upewnij się, że rozumiesz, jak dostęp warunkowy działa przed rozpoczęciem konfigurowania zasad do zarządzania dostępem do zarządzania platformy Azure. Upewnij się, że nie utworzono warunki, które mogą blokować dostęp do portalu.

Aby uzyskać więcej informacji na temat sposobu konfigurowania i korzystania z dostępu warunkowego, zobacz [dostępu warunkowego w usłudze Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).