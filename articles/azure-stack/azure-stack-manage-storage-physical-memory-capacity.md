---
title: Zarządzanie pojemnością pamięci fizycznej dla usługi Azure Stack | Dokumentacja firmy Microsoft
description: Monitorowanie i zarządzanie miejsce do magazynowania dostępne dla usługi Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: Thomas.Roettinger
ms.openlocfilehash: a914d20f61b5b632e792ca29f6c201964db4a203
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452143"
---
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Zarządzanie pojemnością pamięci fizycznej dla usługi Azure Stack

*Dotyczy: zintegrowane systemy usługi Azure Stack*

Aby zwiększyć pojemność całkowitej dostępnej pamięci dla usługi Azure Stack, możesz dodać więcej pamięci. W usłudze Azure Stack serwer fizyczny jest również nazywany *węzła jednostki skalowania*. Wszystkie węzły jednostki skalowania, które są elementami członkowskimi jednostki skalowania pojedynczej musi mieć tej samej ilości pamięci.

> [!note]  
> Przed kontynuowaniem zapoznaj się z dokumentacją producenta sprzętu, aby sprawdzić, czy producent obsługuje uaktualnienie pamięci fizycznej. Umowy dotyczącej pomocy technicznej producenta OEM sprzęt dostawcy może wymagać, że dostawca wykonać umieszczania stojak serwerów fizycznych i aktualizacji oprogramowania układowego urządzenia.

Poniższy diagram przepływu przedstawia ogólny proces dodawania pamięci dla każdego węzła jednostki skalowania.

![Zwiększ ilość pamięci do każdego węzła jednostki skalowania](media\azure-stack-manage-storage-physical-capacity\process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Zwiększ ilość pamięci do istniejącego węzła
W poniższych krokach przedstawiono ogólny przegląd, Dodaj pamięci procesu. 

> [!Warning]  
Nie wykonuj następujące czynności bez odwołujące się do dokumentacji dostarczonego przez producenta OEM.

> [!Warning]  
Musi być zamknięty całej jednostki skalowania, ponieważ uaktualnienie stopniowe pamięci nie jest obsługiwana.

1. Zatrzymaj usługę Azure Stack wykonując kroki opisane w [uruchamianie i zatrzymywanie usługi Azure Stack](azure-stack-start-and-stop.md) artykułu.
2. Uaktualnij pamięci na każdym komputerze fizycznym, korzystając z dokumentacji producenta sprzętu.
3. Uruchom usługę Azure Stack przy użyciu kroków w [uruchamianie i zatrzymywanie usługi Azure Stack](azure-stack-start-and-stop.md) artykułu.

## <a name="next-steps"></a>Kolejne kroki

 - Aby dowiedzieć się, jak zarządzać kontami magazynu w usłudze Azure Stack, aby znaleźć, odzyskiwania i odzyskać pojemność magazynu na podstawie potrzeb biznesowych, zobacz [Zarządzanie kontami magazynu w usłudze Azure Stack](azure-stack-manage-storage-accounts.md).
 - Aby dowiedzieć się, operator chmury Azure Stack, monitoruje i zarządza się pojemności ich wdrożenia usługi Azure Stack, zobacz [Zarządzanie pojemnością magazynu dla usługi Azure Stack](azure-stack-manage-storage-shares.md). 
