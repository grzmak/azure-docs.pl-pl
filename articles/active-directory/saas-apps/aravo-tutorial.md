---
title: 'Samouczek: Integracji Azure Active Directory z Aravo | Dokumentacja firmy Microsoft'
description: Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i Aravo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 224939d8-2c9c-4561-968d-62722f5ab5ed
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: f9ada220a82a2cf9347f02960eeef9c211f37c67
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226157"
---
# <a name="tutorial-azure-active-directory-integration-with-aravo"></a>Samouczek: Integracji Azure Active Directory z Aravo

Z tego samouczka dowiesz się integrowanie Aravo z usługi Azure Active Directory (Azure AD).

Integracja z usługą Azure AD Aravo zapewnia następujące korzyści:

- Można kontrolować w usłudze Azure AD, który ma dostęp do Aravo
- Umożliwia użytkownikom automatycznie pobrać zalogowane do Aravo (logowanie jednokrotne) przy użyciu ich kont usługi Azure AD
- Możesz zarządzać kont w jednej centralnej lokalizacji - portalu Azure

Jeśli chcesz dowiedzieć się więcej informacji o integracji aplikacji SaaS w usłudze Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z Aravo, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Aravo jednokrotnego włączone subskrypcji

> [!NOTE]
> Aby przetestować kroki opisane w tym samouczku, zaleca się używania środowiska produkcyjnego.

Aby przetestować kroki opisane w tym samouczku, należy wykonać te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać miesięczna wersja próbna [tutaj](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Opis scenariusza
W tym samouczku można przetestować usługę Azure AD rejestracji jednokrotnej w środowisku testowym. Scenariusz opisany w tym samouczku składa się z dwóch głównych elementów:

1. Dodawanie Aravo z galerii
2. Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne

## <a name="adding-aravo-from-the-gallery"></a>Dodawanie Aravo z galerii
Aby skonfigurować integrację usługi Azure AD Aravo, należy dodać Aravo z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Aravo z galerii, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, na panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Usługa Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw**. Następnie przejdź do **wszystkie aplikacje**.

    ![Aplikacje][2]
    
3. Aby dodać nową aplikację, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Aplikacje][3]

4. W polu wyszukiwania wpisz **Aravo**.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/aravo-tutorial/tutorial_aravo_search.png)

5. W panelu wyników wybierz **Aravo**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/aravo-tutorial/tutorial_aravo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie usługi Azure AD logowanie jednokrotne
W tej sekcji możesz skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Aravo na podstawie użytkownika testowego, nazywany "Britta Simona".

Dla rejestracji jednokrotnej do pracy usługi Azure AD musi wiedzieć, użytkownik odpowiednika w Aravo jest dla użytkownika, w usłudze Azure AD. Innymi słowy link relację między użytkownikiem usługi Azure AD i danemu użytkownikowi w Aravo musi się.

W Aravo, należy przypisać wartość **nazwy użytkownika** w usłudze Azure AD jako wartość **Username** do ustanawiania relacji łącza.

Aby skonfigurować i przetestować usługi Azure AD rejestracji jednokrotnej z Aravo, należy wykonać poniższe bloki konstrukcyjne:

1. **[Konfigurowanie usługi Azure AD rejestracji jednokrotnej](#configuring-azure-ad-single-sign-on)**  — aby umożliwić użytkownikom korzystać z tej funkcji.
2. **[Tworzenie użytkownika testowego usługi Azure AD](#creating-an-azure-ad-test-user)**  — do przetestowania usługi Azure AD rejestracji jednokrotnej z Simona Britta.
3. **[Tworzenie użytkownika testowego Aravo](#creating-an-aravo-test-user)**  — w celu zapewnienia odpowiednikiem Simona Britta Aravo połączonego z usługi Azure AD reprezentację użytkownika.
4. **[Przypisanie użytkownika testowego usługi Azure AD](#assigning-the-azure-ad-test-user)**  — aby umożliwić Simona Britta do użycia usługi Azure AD rejestracji jednokrotnej.
5. **[Testowanie rejestracji jednokrotnej](#testing-single-sign-on)**  — Aby sprawdzić, czy konfiguracja działa.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurowanie usługi Azure AD rejestracji jednokrotnej

W tej sekcji można włączyć usługi Azure AD rejestracji jednokrotnej w portalu Azure i skonfigurować logowanie jednokrotne w aplikacji Aravo.

**Aby skonfigurować usługi Azure AD rejestracji jednokrotnej z Aravo, wykonaj następujące czynności:**

1. W portalu Azure na **Aravo** strona integracji aplikacji, kliknij przycisk **logowanie jednokrotne**.

    ![Konfigurowanie rejestracji jednokrotnej][4]

2. Na **logowanie jednokrotne** okno dialogowe, wybierz opcję **tryb** jako **na języku SAML logowania jednokrotnego** Aby włączyć logowanie jednokrotne.
 
    ![Konfigurowanie rejestracji jednokrotnej](./media/aravo-tutorial/tutorial_aravo_samlbase.png)

3. Na **Aravo domeny i adres URL** sekcji, wykonaj następujące czynności:

    ![Konfigurowanie rejestracji jednokrotnej](./media/aravo-tutorial/tutorial_aravo_url.png)

    a. W **identyfikator** tekstowym, wpisz adres URL, używając następującego wzorca: `https://<companyname>.aravo.com`

    b. W **adres URL odpowiedzi** tekstowym, wpisz adres URL, używając następującego wzorca: `https://<companyname>.aravo.com/aems/login.do`

    > [!NOTE] 
    > Wartości te nie są prawdziwe. Rzeczywisty identyfikator i adres URL odpowiedzi, należy zaktualizować te wartości. Skontaktuj się z [zespołem pomocy technicznej Aravo](http://www.aravo.com/about-us/contact/) uzyskać te wartości.
 
4. Na **certyfikat podpisywania SAML** kliknij **certyfikatu (Base64)** , a następnie zapisz plik certyfikatu na tym komputerze.

    ![Konfigurowanie rejestracji jednokrotnej](./media/aravo-tutorial/tutorial_aravo_certificate.png) 

5. Kliknij przycisk **zapisać** przycisku.

    ![Konfigurowanie rejestracji jednokrotnej](./media/aravo-tutorial/tutorial_general_400.png)

6. Na **konfiguracji Aravo** , kliknij przycisk **skonfigurować Aravo** otworzyć **Konfigurowanie logowania jednokrotnego** okna. Kopiuj **Sign-Out adres URL, identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** z **sekcji krótkimi opisami.**

    ![Konfigurowanie rejestracji jednokrotnej](./media/aravo-tutorial/tutorial_aravo_configure.png) 

7. Do konfigurowania rejestracji jednokrotnej na **Aravo** stronie, musisz wysłać pobrany **certyfikatu (Base64)**, **Sign-Out adres URL, identyfikator jednostki SAML i SAML pojedynczy znak na adres URL usługi** Aby [zespołem pomocy technicznej Aravo](http://www.aravo.com/about-us/contact/). 


> [!TIP]
> Teraz możesz przeczytać zwięzły wersji tych instrukcji wewnątrz [portalu Azure](https://portal.azure.com), podczas konfigurowania aplikacji!  Po dodaniu tej aplikacji z **usługi Active Directory > aplikacje dla przedsiębiorstw** po prostu kliknij **rejestracji jednokrotnej** karcie i dostęp do dokumentacji osadzonych za pomocą **konfiguracji** sekcji u dołu. Więcej o funkcji dokumentacji osadzonego w tym miejscu: [dokumentacji osadzonych usługi Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD
Celem tej sekcji jest tworzenie użytkownika testowego w portalu Azure o nazwie Simona Britta.

![Tworzenie użytkowników usługi Azure AD][100]

**Aby utworzyć użytkownika testowego w usłudze Azure AD, wykonaj następujące czynności:**

1. W **portalu Azure**, w lewym okienku nawigacji, kliknij polecenie **usługi Azure Active Directory** ikony.

    ![Tworzenie użytkownika testowego usługi Azure AD](./media/aravo-tutorial/create_aaduser_01.png) 

2. Aby wyświetlić listę użytkowników, przejdź do **użytkowników i grup** i kliknij przycisk **wszyscy użytkownicy**.
    
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/aravo-tutorial/create_aaduser_02.png) 

3. Aby otworzyć **użytkownika** okna dialogowego, kliknij przycisk **Dodaj** górnej części okna dialogowego.
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/aravo-tutorial/create_aaduser_03.png) 

4. Na **użytkownika** okna dialogowego strony, należy wykonać następujące czynności:
 
    ![Tworzenie użytkownika testowego usługi Azure AD](./media/aravo-tutorial/create_aaduser_04.png) 

    a. W **nazwa** pole tekstowe, typ **BrittaSimon**.

    b. W **nazwy użytkownika** pole tekstowe, typ **adres e-mail** z BrittaSimon.

    c. Wybierz **Pokaż hasło** i zanotuj wartość **hasło**.

    d. Kliknij przycisk **Utwórz**.
 
### <a name="creating-an-aravo-test-user"></a>Tworzenie użytkownika testowego Aravo

Celem tej sekcji jest utworzenie użytkownika o nazwie Simona Britta w Aravo. Praca z [zespołem pomocy technicznej Aravo](http://www.aravo.com/about-us/contact/) Aby dodać użytkowników w ramach konta Aravo.

### <a name="assigning-the-azure-ad-test-user"></a>Przypisanie użytkownika testowego usługi Azure AD

W tej sekcji można włączyć Simona Britta do używania Azure logowania jednokrotnego za udzielanie dostępu Aravo.

![Przypisz użytkownika][200] 

**Aby przypisać Simona Britta Aravo, wykonaj następujące czynności:**

1. W portalu Azure Otwórz widok aplikacji, a następnie przejdź do widoku katalogu i przejdź do **aplikacje dla przedsiębiorstw** kliknięcie **wszystkie aplikacje**.

    ![Przypisz użytkownika][201] 

2. Na liście aplikacji zaznacz **Aravo**.

    ![Konfigurowanie rejestracji jednokrotnej](./media/aravo-tutorial/tutorial_aravo_app.png) 

3. W menu po lewej stronie kliknij **użytkowników i grup**.

    ![Przypisz użytkownika][202] 

4. Kliknij przycisk **Dodaj** przycisku. Następnie wybierz **użytkowników i grup** na **Dodaj przydziału** okna dialogowego.

    ![Przypisz użytkownika][203]

5. Na **użytkowników i grup** okno dialogowe, wybierz opcję **Simona Britta** na liście Użytkownicy.

6. Kliknij przycisk **wybierz** znajdującego się na **użytkowników i grup** okna dialogowego.

7. Kliknij przycisk **przypisać** znajdującego się na **Dodaj przydziału** okna dialogowego.
    
### <a name="testing-single-sign-on"></a>Testowanie rejestracji jednokrotnej

Celem tej sekcji służy do testowania usługi Microsoft Azure AD rejestracji jednokrotnej konfigurację za pomocą panelu dostępu.

Po kliknięciu kafelka Aravo w panelu dostępu użytkownik powinien pobrać automatycznie zalogowane do aplikacji Aravo.

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/aravo-tutorial/tutorial_general_01.png
[2]: ./media/aravo-tutorial/tutorial_general_02.png
[3]: ./media/aravo-tutorial/tutorial_general_03.png
[4]: ./media/aravo-tutorial/tutorial_general_04.png

[100]: ./media/aravo-tutorial/tutorial_general_100.png

[200]: ./media/aravo-tutorial/tutorial_general_200.png
[201]: ./media/aravo-tutorial/tutorial_general_201.png
[202]: ./media/aravo-tutorial/tutorial_general_202.png
[203]: ./media/aravo-tutorial/tutorial_general_203.png

