---
title: 'Usłudze Azure Databricks: Typowe pytania i pomagać | Dokumentacja firmy Microsoft'
description: Uzyskaj odpowiedzi na często zadawane pytania oraz informacje dotyczące rozwiązywania problemów dotyczących usługi Azure Databricks.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: c3ba235c60480c38a21ee3264c54b4a4dcdea340
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434605"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Często zadawane pytania dotyczące usługi Azure Databricks

W tym artykule wymieniono najpopularniejsze zapytania, które mogą być związane z usługą Azure Databricks. Zawiera również listę niektórych typowych problemów, które może być podczas korzystania z usługi Databricks. Aby uzyskać więcej informacji, zobacz [co to jest Azure Databricks](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Czy można używać własnych kluczy w lokalnych szyfrowania? 
W bieżącej wersji za pomocą własnych kluczy z usługi Azure Key Vault nie jest obsługiwana. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Czy można używać sieci wirtualnych platformy Azure, za pomocą usługi Databricks?
Nowej sieci wirtualnej jest tworzony w ramach inicjowania obsługi usługi Databricks. W tej wersji nie można użyć własnych sieci wirtualnej platformy Azure.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Jak dostęp do usługi Azure Data Lake Store z notesu? 

Wykonaj następujące kroki:
1. W usłudze Azure Active Directory (Azure AD) aprowizować nazwy głównej usługi i zapisz jego kluczem.
1. Przypisz uprawnienia niezbędne do nazwy głównej usługi w Data Lake Store.
1. Aby uzyskać dostęp do pliku w Data Lake Store, użyj poświadczenia nazwy głównej usługi w notesie.

Aby uzyskać więcej informacji, zobacz [Użyj Data Lake Store z usługą Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html).

## <a name="fix-common-problems"></a>Rozwiązywanie typowych problemów

Poniżej przedstawiono kilka problemów, które mogą wystąpić przy użyciu usługi Databricks.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problem: Ta subskrypcja nie jest zarejestrowana do korzystania z przestrzeni nazw "Microsoft.Databricks"

#### <a name="error-message"></a>Komunikat o błędzie

"Ta subskrypcja nie jest zarejestrowany do korzystania z przestrzeni nazw"Microsoft.Databricks". Zobacz https://aka.ms/rps-not-found na temat rejestrowania subskrypcji. (Code: MissingSubscriptionRegistration) "

#### <a name="solution"></a>Rozwiązanie

1. Przejdź do witryny [Azure Portal](https://portal.azure.com).
1. Wybierz **subskrypcje**, subskrypcji, którego używasz, a następnie **dostawców zasobów**. 
1. Na liście dostawców zasobów przed **Microsoft.Databricks**, wybierz opcję **zarejestrować**. W subskrypcji, aby zarejestrować dostawcę zasobów, musi mieć rolę współautora lub właściciela.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problem: Twoje konto {email} nie ma roli współautora lub właściciela zasobu obszaru roboczego usługi Databricks w witrynie Azure portal

#### <a name="error-message"></a>Komunikat o błędzie

"Twoje konto {email} nie ma roli współautora lub właściciela zasobu obszaru roboczego usługi Databricks w witrynie Azure portal. Ten błąd może również wystąpić, jeśli jesteś użytkownikiem-gościem w dzierżawie. Poproś administratora o udzielenie dostępu lub dodanie Cię jako użytkownika bezpośrednio w obszarze roboczym usługi Databricks." 

#### <a name="solution"></a>Rozwiązanie

Poniżej przedstawiono kilka rozwiązań tego problemu:

* Aby zainicjować dzierżawy, użytkownik zalogowany się jako zwykły użytkownik dzierżawy, a nie jako użytkownik-Gość. Musisz także roli współautora dla zasobu obszaru roboczego usługi Databricks. Można udzielić użytkownikowi dostępu z **kontrola dostępu (IAM)** kartę w obszarze roboczym usługi Databricks w witrynie Azure portal.

* Ten błąd może również wystąpić, jeśli nazwa domeny poczty e-mail jest przypisany do wielu katalogów w usłudze Azure AD. Aby obejść ten problem, należy utworzyć nowego użytkownika w katalogu, który zawiera subskrypcję za pomocą obszaru roboczego usługi Databricks.

    a. W witrynie Azure portal przejdź do usługi Azure AD. Wybierz **użytkowników i grup** > **Dodawanie użytkownika**.

    b. Dodaj użytkownika z `@<tenant_name>.onmicrosoft.com` e-mail zamiast `@<your_domain>` wiadomości e-mail. Możesz znaleźć tę opcję w **domen niestandardowych**, w ramach usługi Azure AD w witrynie Azure portal.
    
    c. Udzielisz temu użytkownikowi nowe **Współautor** rolę zasobu obszaru roboczego usługi Databricks.
    
    d. Zaloguj się do witryny Azure portal z nowym użytkownikiem, a następnie znajdź obszar roboczy usługi Databricks.
    
    e. Uruchom obszar roboczy usługi Databricks jako ten użytkownik.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problem: Twoje konto {email} nie została zarejestrowana w usłudze Databricks 

#### <a name="solution"></a>Rozwiązanie

Jeśli obszar roboczy nie został utworzony, a użytkownik zostanie dodany jako użytkownik, skontaktuj się z osobą, która utworzyła obszaru roboczego. Mieć osoba dodał Cię przy użyciu konsoli administracyjnej usługi Azure Databricks. Aby uzyskać instrukcje, zobacz [Dodawanie użytkowników i zarządzanie nimi](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Jeśli utworzono obszar roboczy, a nadal otrzymujesz ten błąd, spróbuj wybrać **Inicjuj obszar roboczy** ponownie w witrynie Azure portal.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Problem: Błąd uruchamiania dostawcy chmury podczas konfigurowania klastra (PublicIPCountLimitReached)

#### <a name="error-message"></a>Komunikat o błędzie

"Niepowodzenie uruchomienia dostawcy w chmurze: Wystąpił błąd dostawcy chmury podczas konfigurowania klastra. Aby uzyskać więcej informacji zobacz Przewodnik po usłudze Databricks. Kod błędu systemu Azure: PublicIPCountLimitReached. Komunikat o błędzie platformy Azure: nie można utworzyć więcej niż 60 publiczne adresy IP dla tej subskrypcji w tym regionie. "

#### <a name="solution"></a>Rozwiązanie

Klastry usługi Databricks używają jeden publiczny adres IP w każdym węźle. Jeśli Twoja subskrypcja ma już wszystkie jego publiczne adresy IP, wykonaj następujące czynności [zażądać zwiększenia limitu przydziału](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Wybierz **przydziału** jako **typ problemu**, i **sieć: ARM** jako **typ limitu przydziału**. W **szczegóły**, zażądać zwiększenia limitu przydziału publicznego adresu IP. Na przykład jeśli limit jest obecnie 60 i chcesz utworzyć klaster z węzłami 100, zwiększenia limitu do 160.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Problem: Drugi typ błąd uruchamiania dostawcy chmury podczas konfigurowania klastra (MissingSubscriptionRegistration)

#### <a name="error-message"></a>Komunikat o błędzie

"Niepowodzenie uruchomienia dostawcy w chmurze: Wystąpił błąd dostawcy chmury podczas konfigurowania klastra. Aby uzyskać więcej informacji zobacz Przewodnik po usłudze Databricks.
Kod błędu systemu Azure: komunikat o błędzie platformy MissingSubscriptionRegistration Azure: subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw "Microsoft.Compute". Zobacz https://aka.ms/rps-not-found na temat rejestrowania subskrypcji. "

#### <a name="solution"></a>Rozwiązanie

1. Przejdź do witryny [Azure Portal](https://portal.azure.com).
1. Wybierz **subskrypcje**, subskrypcji, którego używasz, a następnie **dostawców zasobów**. 
1. Na liście dostawców zasobów przed **Microsoft.Compute**, wybierz opcję **zarejestrować**. W subskrypcji, aby zarejestrować dostawcę zasobów, musi mieć rolę współautora lub właściciela.

Aby uzyskać szczegółowe instrukcje, zobacz [dostawcy zasobów i ich typy](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Problem: Usługa Azure Databricks wymaga uprawnień dostępu do zasobów w organizacji, którzy mogą udzielić tylko administrator.

#### <a name="background"></a>Tło

Usługa Azure Databricks jest zintegrowana z usługą Azure AD. Dzięki temu można ustawić uprawnień w ramach usługi Azure Databricks (na przykład w notesach lub klastry), określając użytkowników z usługi Azure AD. Usługi Azure Databricks można było wyświetlić listę nazw użytkowników z usługi Azure AD wymaga uprawnień do odczytu do tych informacji. Wymaga to zgodę. Jeśli zgody nie jest już dostępna, zostanie wyświetlony błąd.

#### <a name="solution"></a>Rozwiązanie

Zaloguj się jako administrator globalny do witryny Azure portal. Usługi Azure Active Directory, przejdź do **ustawienia użytkownika** kartę i upewnij się, że **użytkownicy mogą zezwalać aplikacjom uzyskiwanie dostępu do danych firmy w ich imieniu** ustawiono **tak**.

## <a name="next-steps"></a>Kolejne kroki

- [Szybki Start: Rozpoczynanie pracy z usługą Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Co to jest Azure Databricks?](what-is-azure-databricks.md)

