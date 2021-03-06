---
title: Jak przygotować się do zmiany adresu SSL IP — Azure
description: Jeśli Twój adres SSL IP będzie można zmienić, Dowiedz się, co należy zrobić, dzięki czemu aplikacja będzie nadal działać po zmianie.
services: app-service\web
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: cephalin
ms.openlocfilehash: e8558b4c3c7dafca8d4fff7e2aae0597a66c031d
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576546"
---
# <a name="how-to-prepare-for-an-ssl-ip-address-change"></a>Jak przygotować się do zmiany adresu SSL IP

Jeśli otrzymasz powiadomienie, że zmiana jest adres SSL IP aplikacji usługi Azure App Service, postępuj zgodnie z instrukcjami w tym artykule, aby zwolnić istniejącego adresu SSL IP i przypisać nowe.

## <a name="release-ssl-ip-addresses-and-assign-new-ones"></a>Zwolnienie adresów SSL IP i przypisać nowe

1.  Otwórz [portal Azure](https://portal.azure.com).

2.  W menu nawigacji po lewej stronie wybierz **App Services**.

3.  Wybierz aplikację usługi App Service z listy.

4.  W obszarze **ustawienia** nagłówka, kliknij przycisk **ustawienia protokołu SSL** w nawigacji po lewej stronie.

5. W sekcji wiązania SSL zaznacz rekord nazwy hosta. W wyświetlonym edytorze wybierz **SNI SSL** na **typ SSL** menu listy rozwijanej i kliknij przycisk **Dodawanie powiązania**. Gdy pojawi się komunikat o powodzeniu operacji, został wydany istniejącego adresu IP.

6.  W **powiązania SSL** ponownie wybierz ten sam rekord nazwy hosta, za pomocą certyfikatu. W edytorze, która zostanie otwarta, tym razem wybierz **SSL opartych na protokole IP** na **typ SSL** menu listy rozwijanej i kliknij przycisk **Dodawanie powiązania**. Gdy pojawi się komunikat o powodzeniu operacji, nabyciu nowego adresu IP.

7.  Jeśli rekord A (wskazuje bezpośrednio na Twój adres IP rekordu DNS) jest skonfigurowany w portalu rejestracji domeny (innych firm w usłudze DNS platformy Azure lub dostawcy DNS), zamienić istniejącego adresu IP na nowo wygenerowane. Nowy adres IP można znaleźć, postępując zgodnie z instrukcjami w następnej sekcji.

## <a name="find-the-new-ssl-ip-address-in-the-azure-portal"></a>Znajdź nowy adres SSL IP w witrynie Azure Portal

1.  Poczekaj kilka minut, a następnie otwórz [witryny Azure portal](https://portal.azure.com).

2.  W menu nawigacji po lewej stronie wybierz **App Services**.

3.  Wybierz aplikację usługi App Service z listy.

4.  W obszarze **ustawienia** nagłówka, kliknij przycisk **właściwości** w nawigacji po lewej stronie, a następnie znajdź sekcja o nazwie **wirtualny adres IP**.

5. Skopiuj adres IP, a następnie ponownie skonfiguruj swoje rekord domeny lub adresu IP mechanizmu.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule opisano sposób przygotowania do zmiany adresu IP, które zostało zainicjowane przez platformę Azure. Aby uzyskać więcej informacji na temat adresów IP w usłudze Azure App Service, zobacz [adresy protokołu SSL i SSL IP w usłudze Azure App Service](app-service-ip-addresses.md).
