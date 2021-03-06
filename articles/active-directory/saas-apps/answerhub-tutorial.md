---
title: 'Samouczek: Integracja usługi Azure Active Directory z AnswerHub | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługi Azure Active Directory i AnswerHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 447a3911bc1f021fb1ca2658716de1910b5379b6
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044086"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Samouczek: Integracja usługi Azure Active Directory z AnswerHub

W tym samouczku dowiesz się, jak zintegrować AnswerHub w usłudze Azure Active Directory (Azure AD).

Integrowanie AnswerHub z usługą Azure AD zapewnia następujące korzyści:

- Możesz kontrolować w usłudze Azure AD, kto ma dostęp do AnswerHub
- Umożliwia użytkownikom automatyczne pobieranie zalogowanych do AnswerHub (logowanie jednokrotne) przy użyciu konta usługi Azure AD
- Możesz zarządzać konta w jednej centralnej lokalizacji — witryny Azure portal

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD za pomocą AnswerHub, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- AnswerHub logowanie jednokrotne włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, chyba że jest to konieczne.
- Jeśli nie masz środowisko usługi Azure AD w wersji próbnej, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W ramach tego samouczka można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych bloków konstrukcyjnych:

1. Dodawanie AnswerHub z galerii
2. Konfigurowanie i testowania usługi Azure AD logowanie jednokrotne

## <a name="adding-answerhub-from-the-gallery"></a>Dodawanie AnswerHub z galerii
Aby skonfigurować integrację AnswerHub w usłudze Azure AD, należy dodać AnswerHub z galerii z listą zarządzanych aplikacji SaaS.

**Aby dodać AnswerHub z galerii, wykonaj następujące czynności:**

1. W  **[witryny Azure portal](https://portal.azure.com)**, w panelu nawigacyjnym po lewej stronie kliknij pozycję **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
3. Aby dodać nową aplikację, kliknij **nową aplikację** przycisk u góry okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **AnswerHub**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/answerhub-tutorial/tutorial_answerhub_search.png)

5. W panelu wyników wybierz **AnswerHub**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowania usługi Azure AD logowanie jednokrotne
W tej sekcji służy do konfigurowania i testowanie usługi Azure AD logowanie jednokrotne za pomocą AnswerHub w oparciu o użytkownika testu o nazwie "Britta Simon".

Dla logowania jednokrotnego do pracy usługi Azure AD musi znać użytkownika odpowiednika w AnswerHub do użytkownika w usłudze Azure AD. Innymi słowy relację łącza między użytkownika usługi Azure AD i powiązanego użytkownika w AnswerHub musi można ustanowić.

W AnswerHub, należy przypisać wartość **nazwa_użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łączy.

Aby skonfigurować i testowanie usługi Azure AD logowanie jednokrotne za pomocą AnswerHub, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD logowania jednokrotnego](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD logowanie jednokrotne za pomocą Britta Simon.
3. **[Tworzenie użytkownika testowego AnswerHub](#creating-an-answerhub-test-user)**  — aby odpowiednikiem Britta Simon w AnswerHub połączonego z usługi Azure AD reprezentacja użytkownika.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — Aby włączyć Britta Simon korzystać z usługi Azure AD logowania jednokrotnego.
5. **[Testowanie logowania jednokrotnego](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD logowania jednokrotnego

W tej sekcji możesz włączyć usługi Azure AD logowania jednokrotnego w witrynie Azure portal i konfigurowanie logowania jednokrotnego w aplikacji AnswerHub.

**Aby skonfigurować usługę Azure AD logowanie jednokrotne z AnswerHub, wykonaj następujące czynności:**

1. W witrynie Azure portal na **AnswerHub** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie logowania jednokrotnego][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **opartej na SAML logowania jednokrotnego** włączyć logowanie jednokrotne.
 
    ![Konfigurowanie logowania jednokrotnego](./media/answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. Na **AnswerHub domena i adresy URL** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/answerhub-tutorial/tutorial_answerhub_url.png)

    a. W **adres URL logowania** pole tekstowe, wpisz adres URL przy użyciu następującego wzorca: `https://<company>.answerhub.com`

    b. W **identyfikator** pole tekstowe, wpisz adres URL przy użyciu następującego wzorca: `https://<company>.answerhub.com`

    > [!NOTE] 
    > Te wartości są prawdziwe. Zaktualizuj te wartości przy użyciu rzeczywistego adresu URL logowania jednokrotnego i identyfikator. Skontaktuj się z pomocą [zespołem pomocy technicznej klienta AnswerHub](mailto:success@answerhub.com) do uzyskania tych wartości. 
 
4. Na **certyfikat podpisywania SAML** kliknij **Certificate(Base64)** , a następnie zapisz plik certyfikatu na komputerze.

    ![Konfigurowanie logowania jednokrotnego](./media/answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Kliknij przycisk **Zapisz** przycisku.

    ![Konfigurowanie logowania jednokrotnego](./media/answerhub-tutorial/tutorial_general_400.png)

6. Na **konfiguracji AnswerHub** , kliknij przycisk **skonfigurować AnswerHub** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **adres URL wylogowania i SAML pojedynczego logowania jednokrotnego usługi adresu URL** z **krótki przewodnik po sekcji.**

    ![Konfigurowanie logowania jednokrotnego](./media/answerhub-tutorial/tutorial_answerhub_configure.png) 

7. W oknie przeglądarki internetowej innej Zaloguj się do witryny firmy AnswerHub, jako administrator.
   
    >[!NOTE]
    >Jeśli potrzebujesz pomocy przy konfigurowaniu AnswerHub, skontaktuj się z pomocą [zespołem pomocy technicznej firmy AnswerHub](mailto:success@answerhub.com.).
   
8. Przejdź do **administracji**.

9. Kliknij przycisk **użytkowników i grup** kartę.

10. W okienku nawigacji po lewej stronie w **społecznościowych ustawienia** kliknij **konfiguracji SAML**.

11. Kliknij przycisk **konfiguracji dostawcy tożsamości** kartę.

12. Na **konfiguracji dostawcy tożsamości** karcie, wykonaj następujące czynności:

     ![Ustawienia języka SAML](./media/answerhub-tutorial/ic785172.png "ustawienia języka SAML")  
  
     a. W **adres URL logowania dostawcy tożsamości** pola tekstowego, Wklej **SAML pojedynczego logowania jednokrotnego usługi adresu URL** skopiowanej w witrynie Azure portal.
  
     b. W **adres URL wylogowania dostawcy tożsamości** pola tekstowego, Wklej **adres URL wylogowania** wartości, które zostały skopiowane z witryny Azure portal.
     
     c. W **Format identyfikatora nazwy tożsamości** polu tekstowym wprowadź nazwę użytkownika, identyfikator wartość takie same jak w witrynie Azure portal w **atrybutów użytkownika** sekcji.
  
     d. Kliknij przycisk **klucze i certyfikaty**.

13. Na karcie klucze i certyfikaty wykonaj następujące czynności:
    
     ![Klucze i certyfikaty](./media/answerhub-tutorial/ic785173.png "klucze i certyfikaty")  
 
     a. Otwórz Twój certyfikat zakodowany base-64, który został pobrany z witryny Azure portal w programie Notatnik, skopiuj zawartość go do Schowka, a następnie wklej go do **klucz publiczny tożsamości (na x 509 Format)** pole tekstowe.
  
     b. Kliknij pozycję **Zapisz**.

14. Na **konfiguracji dostawcy tożsamości** kliknij pozycję **Zapisz**.

> [!TIP]
> Teraz mogą odczytywać zwięzłe wersji tych instrukcji wewnątrz [witryny Azure portal](https://portal.azure.com), podczas gdy konfigurujesz aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij pozycję **logowania jednokrotnego** karty i uzyskać dostęp do osadzonych dokumentacji za pośrednictwem  **Konfiguracja** sekcji u dołu. Możesz dowiedzieć się więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacja embedded usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w witrynie Azure portal, o nazwie Britta Simon.

![Utwórz użytkownika usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **witryny Azure portal**, w okienku nawigacji po lewej stronie kliknij **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/answerhub-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/answerhub-tutorial/create_aaduser_02.png) 

3. Aby otworzyć **użytkownika** okno dialogowe, kliknij przycisk **Dodaj** u góry okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/answerhub-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, wykonaj następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/answerhub-tutorial/create_aaduser_04.png) 

    a. W **nazwa** polu tekstowym wpisz **BrittaSimon**.

    b. W **nazwa_użytkownika** polu tekstowym wpisz **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-an-answerhub-test-user"></a>Tworzenie użytkownika testowego AnswerHub

Aby umożliwić użytkownikom usługi Azure AD, zaloguj się do AnswerHub, musi być obsługiwana w AnswerHub.  
W przypadku AnswerHub Inicjowanie obsługi administracyjnej jest zadanie ręczne.

**Aby udostępnić konto użytkownika, wykonaj następujące czynności:**

1. Zaloguj się do Twojej **AnswerHub** witryny firmy jako administrator.

2. Przejdź do **administracji**.

3. Kliknij przycisk **użytkownikami i grupami** kartę.

4. W okienku nawigacji po lewej stronie w **Zarządzanie użytkownikami** kliknij **Tworzenie lub importowanie użytkowników**.
   
   ![Użytkownicy i grupy](./media/answerhub-tutorial/ic785175.png "użytkownikami i grupami")

5. Typ **adres E-mail**, **Username** i **hasło** prawidłowego konta usługi Azure Active Directory chcesz aprowizować do powiązanych pól tekstowych, a następnie kliknij przycisk  **Zapisz**.

>[!NOTE]
>Można użyć jakichkolwiek innych AnswerHub użytkownika konta tworzenie narzędzi lub interfejsów API dostarczonych przez AnswerHub do aprowizacji kont użytkowników usługi AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji możesz włączyć Britta Simon do używania usługi Azure logowanie jednokrotne za udzielanie dostępu do AnswerHub.

![Przypisz użytkownika][200] 

**Aby przypisać Britta Simon AnswerHub, wykonaj następujące czynności:**

1. W witrynie Azure portal Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji wybierz **AnswerHub**.

    ![Konfigurowanie logowania jednokrotnego](./media/answerhub-tutorial/tutorial_answerhub_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz pozycję **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Britta Simon** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdujący się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdujący się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji służy do testowania konfiguracji usługi Azure AD pojedynczego logowania jednokrotnego przy użyciu panelu dostępu.

Po kliknięciu kafelka AnswerHub w panelu dostępu, użytkownik powinien uzyskać automatycznie zalogowanych do aplikacji AnswerHub.
Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Lista samouczków dotyczących integrowania aplikacji SaaS w usłudze Azure Active Directory](tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/answerhub-tutorial/tutorial_general_01.png
[2]: ./media/answerhub-tutorial/tutorial_general_02.png
[3]: ./media/answerhub-tutorial/tutorial_general_03.png
[4]: ./media/answerhub-tutorial/tutorial_general_04.png

[100]: ./media/answerhub-tutorial/tutorial_general_100.png

[200]: ./media/answerhub-tutorial/tutorial_general_200.png
[201]: ./media/answerhub-tutorial/tutorial_general_201.png
[202]: ./media/answerhub-tutorial/tutorial_general_202.png
[203]: ./media/answerhub-tutorial/tutorial_general_203.png

