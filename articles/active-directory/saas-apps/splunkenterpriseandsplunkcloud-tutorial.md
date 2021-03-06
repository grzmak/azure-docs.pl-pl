---
title: 'Samouczek: Integracja usługi Azure Active Directory z platformy Splunk Enterprise i w chmurze Splunk | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługi Azure Active Directory i platformy Splunk Enterprise i Splunk chmury.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: b1eb8f198b0d9e25a6514e19794c9b1d54a4cd3e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436087"
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Samouczek: Integracja usługi Azure Active Directory z platformy Splunk Enterprise i w chmurze Splunk

W tym samouczku dowiesz się, jak zintegrować platformę Splunk Enterprise i Splunk chmury za pomocą usługi Azure Active Directory (Azure AD).

Integrowanie platformy Splunk Enterprise i Splunk chmury z usługą Azure AD zapewnia następujące korzyści:

- Możesz kontrolować w usłudze Azure AD, kto ma dostęp do platformy Splunk Enterprise i w chmurze Splunk
- Użytkowników, aby automatycznie uzyskać zalogowane platformy Splunk Enterprise i w chmurze Splunk (logowanie jednokrotne) można włączyć za pomocą kont usługi Azure AD
- Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD przy użyciu platformy Splunk Enterprise i w chmurze Splunk, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Platformy Splunk Enterprise i w chmurze Splunk logowania jednokrotnego włączonych subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
- Jeśli nie masz środowisko usługi Azure AD w wersji próbnej, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W ramach tego samouczka można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodanie platformy Splunk Enterprise i Splunk chmury z galerii
1. Konfigurowanie i testowania usługi Azure AD logowanie jednokrotne

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Dodanie platformy Splunk Enterprise i Splunk chmury z galerii
Aby skonfigurować integrację z platformy Splunk Enterprise i Splunk chmury w usłudze Azure AD, należy dodać platformy Splunk Enterprise i Splunk chmury z galerii z listą zarządzanych aplikacji SaaS.

**Aby dodać platformy Splunk Enterprise i Splunk chmury z galerii, wykonaj następujące czynności:**

1. W  **[witryny Azure portal](https://portal.azure.com)**, w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

1. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
1. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Aplikacje][3]

1. W polu wyszukiwania wpisz **platformy Splunk Enterprise i w chmurze Splunk**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_search.png)

1. W panelu wyników wybierz **platformy Splunk Enterprise i w chmurze Splunk**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowania usługi Azure AD logowanie jednokrotne
W tej sekcji konfigurowania i testowania usługi Azure AD logowanie jednokrotne z platformy Splunk Enterprise i Splunk w chmurze na koncie użytkownika testu o nazwie "Britta Simon."

Dla logowania jednokrotnego do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika platformy Splunk Enterprise i Splunk chmura jest dla użytkownika, w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i powiązanego użytkownika platformy Splunk Enterprise i Splunk chmury musi nawiązać.

Platformy Splunk Enterprise i w chmurze Splunk przypisywanie wartości **nazwa_użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łączy.

Aby skonfigurować i testowanie usługi Azure AD logowania jednokrotnego przy użyciu platformy Splunk Enterprise i w chmurze Splunk, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
1. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
1. **[Tworzenie użytkownika testowego platformy Splunk Enterprise i w chmurze Splunk](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  — aby mają odpowiednika Britta Simon w platformy Splunk Enterprise i chmury Splunk, która jest połączona z usługi Azure AD reprezentacja użytkownika.
1. **[Przypisywanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
1. **[Testowanie logowania jednokrotnego](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD logowania jednokrotnego

W tej sekcji możesz włączyć usługi Azure AD logowania jednokrotnego w witrynie Azure portal i konfigurowanie logowania jednokrotnego w aplikacji platformy Splunk Enterprise i Splunk chmury.

**Aby skonfigurować usługę Azure AD logowanie jednokrotne z platformy Splunk Enterprise i w chmurze Splunk, wykonaj następujące czynności:**

1. W witrynie Azure portal na **platformy Splunk Enterprise i w chmurze Splunk** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie logowania jednokrotnego][4]

1. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **opartej na SAML logowania jednokrotnego** włączyć logowanie jednokrotne.
 
    ![Konfigurowanie logowania jednokrotnego](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_samlbase.png)

1. Na **platformy Splunk Enterprise i Splunk chmury domena i adresy URL** sekcji, jeśli chcesz skonfigurować aplikację w **tożsamości** zainicjowano tryb:

    ![Konfigurowanie logowania jednokrotnego](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_url.png)

    a. W **adres URL logowania** pole tekstowe, wpisz adres URL przy użyciu następującego wzorca: `https://<splunkserverUrl>/en-US/app/launcher/home`
    
    b. W **identyfikator** pole tekstowe, wpisz adres URL serwera Splunk.

    c. W **adres URL odpowiedzi** pole tekstowe, wpisz adres URL przy użyciu następującego wzorca: `https://<splunkserver>/saml/acs`

    > [!NOTE] 
    > Te wartości są prawdziwe. Rzeczywisty identyfikator, adres URL odpowiedzi i adres URL logowania, należy zaktualizować te wartości. W tym miejscu zalecamy przy użyciu unikatowej wartości ciągu w identyfikatorze. Skontaktuj się z pomocą [zespołu pomocy technicznej platformy Splunk Enterprise i Splunk Cloud Client](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) do uzyskania tych wartości. 

1. Na **certyfikat podpisywania SAML** kliknij **XML metadanych** , a następnie zapisz plik metadanych na tym komputerze.

    ![Konfigurowanie logowania jednokrotnego](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_certificate.png) 

1. Kliknij przycisk **Zapisz** przycisku.

    ![Konfigurowanie logowania jednokrotnego](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_400.png)

1. Aby skonfigurować logowanie jednokrotne na **platformy Splunk Enterprise i w chmurze Splunk** stronie, musisz wysłać pobrany **XML metadanych** do [zespołupomocytechnicznejplatformySplunkEnterpriseiwchmurzeSplunk](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).

> [!TIP]
> Teraz mogą odczytywać zwięzłe wersji tych instrukcji wewnątrz [witryny Azure portal](https://portal.azure.com), podczas gdy konfigurujesz aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij pozycję **logowania jednokrotnego** karty i uzyskać dostęp do osadzonych dokumentacji za pośrednictwem  **Konfiguracja** sekcji u dołu. Możesz dowiedzieć się więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacja embedded usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w witrynie Azure portal, o nazwie Britta Simon.

![Utwórz użytkownika usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **witryny Azure portal**, w okienku nawigacji po lewej stronie kliknij **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_01.png) 

1. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_02.png) 

1. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** u góry okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_03.png) 

1. Na **użytkownika** okna dialogowego strony, wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_04.png) 

    a. W **nazwa** polu tekstowym wpisz **BrittaSimon**.

    b. W **nazwa_użytkownika** polu tekstowym wpisz **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij pozycję **Utwórz**.
 
### <a name="creating-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Tworzenie użytkownika testowego platformy Splunk Enterprise i w chmurze Splunk

W tej sekcji utworzysz użytkownika o nazwie Britta Simon w platformy Splunk Enterprise, jak i Splunk chmury. Praca z [zespołu pomocy technicznej platformy Splunk Enterprise i w chmurze Splunk](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) Aby dodać użytkowników do platformy Splunk Enterprise i Splunk chmury.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji możesz włączyć Britta Simon do używania platformy Azure logowanie jednokrotne za udzielanie dostępu do platformy Splunk Enterprise i Splunk chmury.

![Przypisz użytkownika][200] 

**Aby przypisać Britta Simon do platformy Splunk Enterprise, jak i chmury Splunk, wykonaj następujące czynności:**

1. W witrynie Azure portal Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

1. Na liście aplikacji wybierz **platformy Splunk Enterprise i w chmurze Splunk**.

    ![Konfigurowanie logowania jednokrotnego](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_app.png) 

1. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

1. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz pozycję **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

1. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Britta Simon** na liście Użytkownicy.

1. Kliknij przycisk **wybierz** znajdujący się na **użytkowników i grup** okna dialogowego.

1. Kliknij przycisk **przypisać** znajdujący się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania usługi Azure AD SSOonfiguration, za pomocą panelu dostępu.

Po kliknięciu platformy Splunk Enterprise, a chmura Splunk kafelka w panelu dostępu, możesz należy pobrać automatycznie zalogowanych do aplikacji platformy Splunk Enterprise i Splunk chmury.

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Lista samouczków dotyczących integrowania aplikacji SaaS w usłudze Azure Active Directory](tutorial-list.md)
* [Czym jest dostęp do aplikacji i logowanie jednokrotne za pomocą usługi Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_01.png
[2]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_02.png
[3]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_03.png
[4]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_04.png

[100]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_100.png

[200]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_200.png
[201]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_201.png
[202]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_202.png
[203]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_203.png

