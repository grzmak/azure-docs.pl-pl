---
title: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — kierowanie ruchu przez wirtualne urządzenie sieciowe | Microsoft Docs
description: Przykładowy skrypt interfejsu wiersza polecenia platformy Azure — kierowanie ruchu przez wirtualne urządzenie sieciowe w ramach zapory.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: 62077f45d96e96a7fef35cf025740849d2b99445
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42054964"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Kierowanie ruchu przez wirtualne urządzenie sieciowe

Ten przykładowy skrypt tworzy sieć wirtualną z podsieciami frontonu i zaplecza. Tworzy również maszynę wirtualną z włączonym przekazywaniem adresu IP, aby przekierowywać ruch między dwiema podsieciami. Po uruchomieniu skryptu można wdrażać oprogramowanie sieciowe, takie jak aplikacja zapory, na maszynie wirtualnej.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Przykładowy skrypt


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszynę wirtualną i wszystkie powiązane zasoby.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt zawiera następujące polecenia służące do tworzenia grupy zasobów, sieci wirtualnej i sieciowych grup zabezpieczeń. Każde polecenie w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Tworzy sieć wirtualną i podsieć frontonu platformy Azure. |
| [az network subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Tworzy zaplecza i sieć obwodową podsieci. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Tworzy publiczny adres IP, dostęp do maszyny Wirtualnej z Internetu. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Tworzy interfejs sieci wirtualnej i włącza przekazywanie adresów IP. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Tworzy sieciową grupę zabezpieczeń. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Tworzy reguły sieciowych grup zabezpieczeń, które zezwalają na użycie portów HTTP i HTTPS dla ruchu przychodzącego do maszyny wirtualnej. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Tworzy skojarzenie sieciowych grup zabezpieczeń i tabel tras z podsieciami. |
| [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create)| Tworzy tabelę tras dla wszystkich tras. |
| [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create)| Tworzy trasy, aby kierować ruchem między podsieciami i Internetem za pośrednictwem maszyny Wirtualnej. |
| [az vm create](/cli/azure/vm#az_vm_create) | Tworzy maszynę wirtualną i dołącza do niej kartę sieciową. To polecenie określa również obraz maszyny wirtualnej do użycia oraz poświadczenia administracyjne. |
| [az group delete](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów i wszystkie zasoby, które zawiera. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia platformy Azure, zobacz [dokumentację interfejsu wiersza polecenia platformy Azure](/cli/azure).

Więcej przykładowych skryptów interfejsu wiersza polecenia platformy sieci można znaleźć w [dokumentacji platformy Azure, omówienie sieci](../cli-samples.md)