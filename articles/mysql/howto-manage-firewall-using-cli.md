---
title: Tworzenie i zarządzanie nimi — Azure Database dla MySQL reguł zapory przy użyciu wiersza polecenia platformy Azure
description: W tym artykule opisano sposób tworzenia i zarządzania usługi Azure Database dla MySQL reguł zapory przy użyciu wiersza polecenia platformy Azure jest wiersza polecenia.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 45df284d29ea2d5eb799697b22deeab03cb66622
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956668"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Tworzenie i zarządzanie nimi — Azure Database dla MySQL reguł zapory przy użyciu wiersza polecenia platformy Azure
Reguły zapory na poziomie serwera umożliwiają administratorom zarządzanie dostępem do usługi Azure Database dla serwera MySQL z określonego adresu IP lub zakres adresów IP. Przy użyciu wygodne poleceń interfejsu wiersza polecenia platformy Azure, możesz utworzyć, zaktualizować, Usuń listę i Pokaż reguły zapory, aby zarządzać serwerem. Aby uzyskać omówienie — Azure Database dla MySQL zapór, zobacz [— Azure Database for reguły zapory serwera MySQL](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Wymagania wstępne
* [Zainstaluj interfejs wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).
* [— Azure Database for MySQL serwera i bazy danych](quickstart-create-mysql-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Polecenia reguły zapory:
**Az mysql server-reguły zapory** polecenia jest używany z wiersza polecenia platformy Azure do tworzenie, usuwanie, listy, wyświetlanie i aktualizowanie reguł zapory.

Polecenia:
- **Utwórz**: Tworzenie reguły zapory serwera MySQL na platformie Azure.
- **Usuń**: Usuń regułę zapory serwera MySQL na platformie Azure.
- **Lista**: lista reguł zapory serwera MySQL na platformie Azure.
- **Pokaż**: wyświetlenie szczegółów dotyczących serwera usługi Azure MySQL reguły zapory.
- **Aktualizuj**: aktualizowanie reguły zapory serwera MySQL na platformie Azure.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Logowanie do platformy Azure i wyświetlać listę usługi Azure Database dla serwerów MySQL
Bezpiecznie łączyć z wiersza polecenia platformy Azure przy użyciu konta platformy Azure przy użyciu **az login** polecenia.

1. Z wiersza polecenia Uruchom następujące polecenie:
```azurecli
az login
```
To polecenie generuje kod do użycia w następnym kroku.

2. Użyj przeglądarki sieci web, aby otworzyć stronę [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin), a następnie wprowadź kod.

3. Po wyświetleniu monitu zaloguj się przy użyciu swoich poświadczeń platformy Azure.

4. Po autoryzacji Twoje dane logowania listę subskrypcji jest drukowany w konsoli. Skopiuj identyfikator odpowiedniej subskrypcji, aby ustawić bieżącą subskrypcję do użycia. Użyj [zestaw konta az](/cli/azure/account#az-account-set) polecenia.
   ```azurecli-interactive
   az account set --subscription <your subscription id>
   ```

5. Lista baz danych Azure dla serwerów MySQL dla Twojej subskrypcji i grupie zasobów, w razie wątpliwości nazw. Użyj [az mysql server list](/cli/azure/mysql/server#az-mysql-server-list) polecenia.

   ```azurecli-interactive
   az mysql server list --resource-group myresourcegroup
   ```

   Należy pamiętać, atrybut nazwy na liście, które należy określić serwera MySQL, aby pracować nad. Jeśli to konieczne, Potwierdź szczegóły dla tego serwera, a przy użyciu nazwy atrybutu, aby upewnić się, że jest prawidłowa. Użyj [az mysql server show](/cli/azure/mysql/server#az-mysql-server-show) polecenia.

   ```azurecli-interactive
   az mysql server show --resource-group myresourcegroup --name mydemoserver
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lista reguł zapory w usłudze Azure Database dla serwera MySQL 
Za pomocą nazwy serwera i nazwy grupy zasobów, Utwórz listę istniejących reguł zapory serwera na serwerze. Użyj [az mysql server zapory listy](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-list) polecenia.  Należy zauważyć, że nazwa serwera jest określony w **— serwer** Przełącz a nie w **— nazwa** przełącznika. 
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Dane wyjściowe wyświetla listę reguł, jeśli formatowania w formacie JSON (domyślnie). Możesz użyć **— tabela wyjściowa** przełącznika w danych wyjściowych wyników w bardziej czytelnym formacie tabeli.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Tworzenie reguły zapory w usłudze Azure Database dla serwera MySQL
Za pomocą usługi Azure MySQL nazwę serwera i nazwę grupy zasobów Utwórz nową regułę zapory na serwerze. Użyj [tworzenie zapory serwera mysql az](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create) polecenia. Podaj nazwę reguły, a także ustawiony początkowy adres IP i końcowy adres IP (w celu zapewnienia dostępu do zakresu adresów IP) dla tej reguły.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Aby zezwolić na dostęp dla pojedynczego adresu IP, należy podać ten sam adres IP jako początkowy adres IP i końcowy adres IP, jak w poniższym przykładzie.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Aby umożliwić aplikacjom z adresów IP platformy Azure, nawiązać połączenia z usługi Azure Database for MySQL server, podaj adres IP 0.0.0.0 jako początkowy adres IP i końcowy adres IP, jak w poniższym przykładzie.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mysql --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Ta opcja konfiguruje zaporę w celu zezwalania na wszystkie połączenia z platformy Azure, w tym połączenia z subskrypcji innych klientów. W przypadku wybrania tej opcji upewnij się, że uprawnienia logowania i użytkownika zezwalają na dostęp tylko uprawnionym użytkownikom.
> 

W razie powodzenia tworzone polecenia, które dane wyjściowe Wyświetla szczegóły reguły zapory, które zostały utworzone w formacie JSON (domyślnie). W przypadku awarii dane wyjściowe zawierają tekst komunikatu o błędzie zamiast tego.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Aktualizuj reguły zapory w usłudze Azure Database dla serwera MySQL 
Za pomocą usługi Azure MySQL nazwę serwera i nazwę grupy zasobów, zaktualizuj istniejącą regułę zapory na serwerze. Użyj [aktualizacji zapory serwera mysql az](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-update) polecenia. Podaj nazwę istniejącej reguły zapory jako dane wejściowe, a także początek adresów IP i końcowy atrybutów adresu IP do aktualizacji.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
W razie powodzenia dane wyjściowe polecenia Wyświetla szczegóły reguły zapory, które zostały zaktualizowane w formacie JSON (domyślnie). W przypadku awarii dane wyjściowe zawierają tekst komunikatu o błędzie zamiast tego.

> [!NOTE]
> Jeśli nie ma reguły zapory, tworzona jest reguła za pomocą polecenia update.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Pokaż zapory szczegóły reguły w usłudze Azure Database dla serwera MySQL
Za pomocą usługi Azure MySQL nazwę serwera i nazwę grupy zasobów, wyświetlić istniejące zapory szczegóły reguły z serwera. Użyj [az mysql server zapory show](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-show) polecenia. Podaj nazwę istniejącej reguły zapory jako dane wejściowe.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
W razie powodzenia dane wyjściowe polecenia Wyświetla szczegóły reguły zapory, które zostały określone w formacie JSON (domyślnie). W przypadku awarii dane wyjściowe zawierają tekst komunikatu o błędzie zamiast tego.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Usuwanie reguły zapory w usłudze Azure Database dla serwera MySQL
Za pomocą usługi Azure MySQL nazwę serwera i nazwę grupy zasobów, usuń istniejącą regułę zapory z serwera. Użyj [Usuń zapory serwera mysql az](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-delete) polecenia. Podaj nazwę istniejącej reguły zapory.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
W razie powodzenia nie ma żadnych danych wyjściowych. W przypadku awarii Wyświetla tekst komunikatu o błędzie.

## <a name="next-steps"></a>Kolejne kroki
- Dowiedzieć się więcej o [— Azure Database for reguły zapory serwera MySQL](./concepts-firewall-rules.md).
- [Tworzenie i zarządzanie nimi — Azure Database dla MySQL reguł zapory przy użyciu witryny Azure portal](./howto-manage-firewall-using-portal.md).
