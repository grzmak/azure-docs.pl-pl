---
title: Konfiguruj parametry usługi w usłudze Azure Database for PostgreSQL
description: W tym artykule opisano jak skonfigurować parametry usługi w usłudze Azure Database for PostgreSQL przy użyciu wiersza polecenia wiersza polecenia platformy Azure.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 5520c08d2bf5dba85ece1de0bca7329286625911
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968054"
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Dostosowywanie parametrów konfiguracji serwera przy użyciu wiersza polecenia platformy Azure
Można wyświetlać, Pokaż i zaktualizuj parametry konfiguracji dla serwera Azure PostgreSQL za pomocą interfejsu wiersza polecenia (Azure CLI). Podzbiór aparatu konfiguracji jest narażony na poziomie serwera i może być modyfikowany. 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków w tym przewodniku, potrzebne są:
- Tworzenie usługi Azure Database for postgresql w warstwie serwera i bazy danych, postępując zgodnie z [Tworzenie serwera usługi Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)
- Zainstaluj [wiersza polecenia platformy Azure](/cli/azure/install-azure-cli) interfejsu wiersza polecenia na maszynie lub użyj [usługi Azure Cloud Shell](../cloud-shell/overview.md) w witrynie Azure portal przy użyciu przeglądarki.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Lista parametrów konfiguracji serwera dla usługi Azure Database dla serwera PostgreSQL
Aby wyświetlić listę wszystkich parametrów można modyfikować w serwera i ich wartości, należy uruchomić [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) polecenia.

Możesz wyświetlić listę parametrów konfiguracji serwera dla serwera **mydemoserver.postgres.database.azure.com** w ramach grupy zasobów **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Pokaż szczegóły parametrów konfiguracji serwera
Aby wyświetlić szczegółowe informacje dotyczące parametru określonej konfiguracji dla serwera, uruchom [az postgres server configuration show](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) polecenia.

Ten przykład przedstawia szczegółowe informacje o **dziennika\_min\_wiadomości** parametru konfiguracji serwera dla serwera **mydemoserver.postgres.database.azure.com** w ramach grupy zasobów **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Zmodyfikuj wartość parametru konfiguracji serwera
Można również zmodyfikować wartości parametru niektórych serwera konfiguracji, która aktualizuje podstawowej wartości konfiguracji dla aparatu serwera PostgreSQL. Aby zaktualizować konfigurację, użyj [az postgres server configuration set](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) polecenia. 

Aby zaktualizować **dziennika\_min\_wiadomości** serwera parametru konfiguracji serwera **mydemoserver.postgres.database.azure.com** w ramach grupy zasobów  **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Jeśli chcesz zresetować wartość parametru konfiguracji, po prostu chcesz Opuść opcjonalnego `--value` parametr i usługa stosuje się wartością domyślną. W powyżej przykładzie to powiadomienie będzie wyglądać:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
To polecenie resetuje **dziennika\_min\_wiadomości** konfiguracji, wartość domyślna **ostrzeżenie**. Uzyskać więcej informacji na temat konfiguracji serwera i dopuszczalne wartości, zobacz dokumentację PostgreSQL [konfiguracji serwera](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Kolejne kroki
- Aby skonfigurować i dostęp do dzienników serwera, zobacz [dzienników serwera w usłudze Azure Database for PostgreSQL](concepts-server-logs.md)
