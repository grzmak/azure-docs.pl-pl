---
title: Szybki start — tworzenie maszyny wirtualnej z systemem Linux w witrynie Azure Portal | Microsoft Docs
description: Z tego przewodnika Szybki start dowiesz się, jak utworzyć maszynę wirtualną z systemem Linux w witrynie Azure Portal
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: fcc9f338ad69322091199ce9d5d2d1d6f9f2165e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227286"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-the-azure-portal"></a>Szybki start: tworzenie maszyny wirtualnej z systemem Linux w witrynie Azure Portal

Maszyny wirtualne platformy Azure można utworzyć za pomocą witryny Azure Portal. Ta metoda bazuje na opartym na przeglądarce interfejsie użytkownika umożliwiającym tworzenie maszyn wirtualnych i powiązanych z nimi zasobów. Z tego przewodnika Szybki start dowiesz się, jak za pomocą witryny Azure Portal wdrożyć maszynę wirtualną platformy Azure z systemem Ubuntu. Aby następnie zobaczyć działanie maszyny wirtualnej, połączysz się z nią za pomocą protokołu SSH i zainstalujesz serwer internetowy NGINX.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-ssh-key-pair"></a>Tworzenie pary kluczy SSH

Do wykonania kroków tego przewodnika Szybki start konieczne jest posiadanie pary kluczy SSH. Jeśli masz już parę kluczy SSH, możesz pominąć ten krok.

Aby utworzyć parę kluczy SSH i zalogować się do maszyn wirtualnych systemu Linux, uruchom następujące polecenie z powłoki Bash i postępuj zgodnie z wyświetlanymi instrukcjami. Możesz na przykład użyć aplikacji [Azure Cloud Shell](../../cloud-shell/overview.md) lub [Windows Substem for Linux](/windows/wsl/install-win10). Dane wyjściowe polecenia zawierają nazwę pliku klucza publicznego. Skopiuj zawartość pliku klucza publicznego (`cat ~/.ssh/id_rsa.pub`) do schowka:

```bash
ssh-keygen -t rsa -b 2048
```

Aby uzyskać bardziej szczegółowe informacje na temat tworzenia par kluczy SSH, łącznie z użyciem programu PuTTy, zobacz [Jak używać kluczy SSH w systemie Windows](ssh-from-windows.md).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do witryny Azure Portal pod adresem http://portal.azure.com

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

1. W lewym górnym rogu okna witryny Azure Portal wybierz pozycję **Utwórz zasób**.

1. W polu wyszukiwania nad listą zasobów Microsoft Azure Marketplace wpisz **Ubuntu Server 16.04 LTS by Canonical**, wybierz odpowiednią pozycję i kliknij **Utwórz**.

1. Na karcie **Podstawowe**, w obszarze **Szczegóły projektu**, upewnij się, że wybrano poprawną subskrypcję, a następnie wybierz opcję **Utwórz nową** w obszarze **Grupa zasobów**. W wyskakującym okienku wpisz *MyResourceGroup* w polu nazwy grupy zasobów, a następnie wybierz przycisk **OK*. 

    ![Tworzenie nowej grupy zasobów dla maszyny wirtualnej](./media/quick-create-portal/project-details.png)

1. W obszarze **Szczegóły wystąpienia** wpisz *myVM* w polu **Nazwa maszyny wirtualnej** i wybierz *Wschodnie stany USA* w polu **Region**. Inne wartości pozostaw domyślne.

    ![Sekcja Szczegóły wystąpienia](./media/quick-create-portal/instance-details.png)

1. W obszarze **Konto administratora** wybierz opcję **klucz publiczny SSH**, wpisz nazwę użytkownika, a następnie wklej swój klucz publiczny w polu tekstowym. Usuń wszystkie wiodące i końcowe białe znaki z klucza publicznego.

    ![Konto administratora](./media/quick-create-portal/administrator-account.png)

1. W obszarze **Reguły portów wejściowych** > **Publiczne porty wejściowe** wybierz opcję **Zezwalaj na wybranych portach**, a następnie wybierz z listy rozwijanej pozycje **SSH (22)** i **HTTP (80)**. 

    ![Otwieranie portów dla protokołów RDP i HTTP](./media/quick-create-portal/inbound-port-rules.png)

1. Pozostaw pozostałe wartości domyślne, a następnie wybierz przycisk **Przejrzyj + utwórz** znajdujący się u dołu strony.

    
## <a name="connect-to-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną

Utwórz połączenie SSH z maszyną wirtualną.

1. Na stronie przeglądu wybierz przycisk **Połącz** dla swojej maszyny wirtualnej. 

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png)

2. Na stronie **Nawiązywanie połączenia z maszyną wirtualną** zostaw opcje domyślne, aby połączyć się za pomocą nazwy DNS przez port 22. W obszarze **Logowanie za pomocą konta lokalnego maszyny wirtualnej** jest wyświetlane polecenie połączenia. Kliknij przycisk, aby skopiować polecenie. W poniższym przykładzie pokazano, jak wygląda polecenie połączenia SSH:

    ```bash
    ssh azureuser@myvm-123abc.eastus.cloudapp.azure.com
    ```

3. Aby utworzyć połączenie, wklej polecenie połączenia SSH do powłoki, takiej jak Azure Cloud Shell lub Bash systemie Ubuntu rezydującym w systemie Windows. 

## <a name="install-web-server"></a>Instalowanie serwera sieci Web

Aby zobaczyć działanie maszyny wirtualnej, zainstaluj serwer internetowy NGINX. W sesji SSH użyj poniższych poleceń w celu zaktualizowania źródeł pakietów i zainstalowania najnowszego pakietu NGINX:

```bash
# update packages
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Po zakończeniu użyj polecenia `exit` w sesji SSH i wróć do właściwości maszyny wirtualnej w witrynie Azure Portal.


## <a name="view-the-web-server-in-action"></a>Oglądanie działającego serwera internetowego

Po zainstalowaniu serwera NGINX i otwarciu portu 80 dla maszyny wirtualnej można uzyskać dostęp do serwera sieci Web z Internetu. Otwórz przeglądarkę internetową i wpisz publiczny adres IP maszyny wirtualnej. Publiczny adres IP można znaleźć na stronie przeglądu maszyny wirtualnej lub w górnej części strony *Sieć*, na której są dodawane reguły portu wejściowego.

![Domyślna witryna serwera NGINX](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć. Aby to zrobić, wybierz grupę zasobów maszyny wirtualnej, wybierz opcję **Usuń**, a następnie potwierdź nazwę grupy zasobów, którą chcesz usunąć.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wdrożono prostą maszynę wirtualną, utworzono sieciową grupę zabezpieczeń i regułę oraz zainstalowano podstawowy serwer internetowy. Aby dowiedzieć się więcej o maszynach wirtualnych platformy Azure, przejdź do samouczka dla maszyn wirtualnych z systemem Linux.

> [!div class="nextstepaction"]
> [Samouczki dla maszyny wirtualnej platformy Azure z systemem Linux](./tutorial-manage-vm.md)
