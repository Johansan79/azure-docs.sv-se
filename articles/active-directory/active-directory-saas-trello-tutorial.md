---
title: "Självstudier: Azure Active Directory-integrering med Trello | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Trello."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 598387b6066612c6c4a4c92cba5ba03e03a55203
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Självstudier: Azure Active Directory-integrering med Trello

I kursen får lära du att integrera Trello med Azure Active Directory (AD Azure).

Integrera Trello med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Trello
- Du kan aktivera användarna att automatiskt hämta loggat in på Trello (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Trello, behöver du följande:

- En Azure AD-prenumeration
- En Trello enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Trello från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-trello-from-the-gallery"></a>Att lägga till Trello från galleriet
Du måste lägga till Trello från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Trello i Azure AD.

**Utför följande steg för att lägga till Trello från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **Trello**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_search.png)

5. Välj i resultatpanelen **Trello**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Trello baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i Trello motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Trello upprättas.

I Trello, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.

Om du vill konfigurera och testa Azure AD enkel inloggning med Trello, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Trello](#creating-a-trello-test-user)**  – du har en motsvarighet för Britta Simon i Trello som är kopplad till Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Trello program.

>[!NOTE]
    >Du bör få den  **\<enterprise\>**  slug från Trello. Om du inte har värdet slug Kontakta [Trello supportteamet](mailto:support@trello.com) att hämta slug för ditt företag.
    > 

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Trello:**

1. I Azure-portalen på den **Trello** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_samlbase.png)

3. På den **Trello domän och URL: er** om du vill konfigurera programmet i **IDP initierade läge**, utför följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_url.png)

    I den **Reply URL** textruta Skriv en URL med följande mönster:`https://trello.com/auth/saml/consume/<enterprise>`

4. Om du vill konfigurera programmet i **SP initierade läge**, utför följande steg:

  ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_url1.png)

    a. Klicka på den **visa avancerade inställningar för URL: en**.

    b. I den **logga URL** textruta Skriv en URL med följande mönster:`https://trello.com/auth/saml/login/<enterprise>`

  c. I den **identifierare** textruta, ange följande URL:`https://trello.com/auth/saml/metadata`

5. Trello program förväntar SAML intyg som innehåller specifika attribut. Konfigurera följande attribut för det här programmet. Du kan hantera värden för attributen från den **”användarattribut”** av programmet. Följande skärmbild visar ett exempel för det här.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_attribute.png)

6. På den **SAML-token attribut** dialogrutan för varje rad som visas i tabellen nedan, utför följande steg:
 
    | Attributnamn | Attributvärde |
    | --- | --- |
    | User.Email | User.Mail |
    | User.FirstName | User.givenName |
    | User.LastName | User.surname |

    a. Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_officespace_05.png)

    b. I den **namn** textruta ange attributets namn visas för den raden. 

    c. Från den **värdet** listan, ange det attributvärde som visas för den raden.
    
    d. Klicka på **OK**. 
 
7. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_certificate.png) 

8. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_general_400.png)

6. På den **Trello Configuration** klickar du på **konfigurera Trello** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_configure.png) 

9. För att få SSO konfigurerats för ditt program, gå till [Trello företagskonfiguration för SSO](https://trello.com/sso-configuration) att skicka [Trello supportteamet](mailto:support@trello.com) den **SAML inloggning tjänst-URL för enkel** och bifoga den **certifikat (Base64)**.

> [!TIP]
> Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned. Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_01.png) 

2. Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_02.png) 

3. Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-trello-test-user"></a>Skapa en testanvändare Trello

I det här avsnittet skapar du en användare som kallas Britta Simon i Trello. I det här avsnittet skapar du en användare som kallas Britta Simon i Trello. Trello stöder just-in-time-etablering och ett nytt konto skapas första gången du loggar in från Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Trello.

![Tilldela användare][200] 

**Om du vill tilldela Trello Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **Trello**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_app.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Trello på åtkomstpanelen du bör få automatiskt loggat in på ditt Trello program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trello-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png

