---
title: "Självstudier: Azure Active Directory-integrering med Zscaler | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zscaler."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 73c81691b68ee820e1d905a17b4f2ab6b6ceb5fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Självstudier: Azure Active Directory-integrering med Zscaler

I kursen får lära du att integrera Zscaler med Azure Active Directory (AD Azure).

Integrera Zscaler med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Zscaler
- Du kan aktivera användarna att automatiskt hämta loggat in på Zscaler (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Zscaler, behöver du följande:

- En Azure AD-prenumeration
- En Zscaler enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Zscaler från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-zscaler-from-the-gallery"></a>Att lägga till Zscaler från galleriet
Du måste lägga till Zscaler från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zscaler i Azure AD.

**Utför följande steg för att lägga till Zscaler från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **Zscaler**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_search.png)

5. Välj i resultatpanelen **Zscaler**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i Zscaler motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zscaler upprättas.

I Zscaler, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.

Om du vill konfigurera och testa Azure AD enkel inloggning med Zscaler, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Konfigurera proxyinställningar](#configuring-proxy-settings)**  - om du vill konfigurera proxyinställningarna i Internet Explorer
3. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
4. **[Skapa en testanvändare Zscaler](#creating-a-zscaler-test-user)**  – du har en motsvarighet för Britta Simon i Zscaler som är kopplad till Azure AD-representation av användaren.
5. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
6. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Zscaler program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zscaler:**

1. I Azure-portalen på den **Zscaler** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. På den **Zscaler domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_url.png)

    I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.zsclaer.net`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med det faktiska inloggnings-URL. Kontakta [Zscaler klienten supportteamet](https://www.zscaler.com/company/contact) att hämta det här värdet. 

4. På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_general_400.png)

6. På den **Zscaler Configuration** klickar du på **konfigurera Zscaler** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_configure.png) 

7. I en annan webbläsarfönster loggar du in på webbplatsen ZScaler företag som administratör.

8. Klicka på menyn högst upp **Administration**.
   
    ![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")

9. Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.   
            
    ![Hantera användare och autentisering](./media/active-directory-saas-zscaler-tutorial/ic800207.png "hantera användare och autentisering")

10. I den **Välj autentiseringsalternativ för din organisation** avsnittet, utför följande steg:   
                
    ![Autentisering](./media/active-directory-saas-zscaler-tutorial/ic800208.png "autentisering")
   
    a. Välj **autentisera med SAML enkel inloggning**.

    b. Klicka på **konfigureras SAML enkel inloggning**.

11. På den **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg och klicka sedan på **klar**

    ![Enkel inloggning](./media/active-directory-saas-zscaler-tutorial/ic800209.png "enkel inloggning")
    
    a. Klistra in den **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat från Azure-portalen i den **URL för SAML-portalen som användare om autentisering skickas** textruta.
    
    b. I den **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.
    
    c. Om du vill överföra din hämtat certifikat klickar du på **Zscaler pem**.
    
    d. Välj **aktivera etablering av SAML-automatiskt**.

12. På den **konfigurera användarautentisering** dialogrutan utför följande steg:

    ![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")
    
    a. Klicka på **Spara**.

    b. Klicka på **aktivera nu**.

## <a name="configuring-proxy-settings"></a>Konfigurera proxyinställningar
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Konfigurera proxyinställningarna i Internet Explorer

1. Starta **Internet Explorer**.

2. Välj **Internetalternativ** från den **verktyg** menyn för att öppna den **Internetalternativ** dialogrutan.   
    
     ![Internet-alternativ](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet-alternativ")

3. Klicka på den **anslutningar** fliken.   
  
     ![Anslutningar](./media/active-directory-saas-zscaler-tutorial/ic769493.png "anslutningar")

4. Klicka på **LAN-inställningar** att öppna den **LAN-inställningar** dialogrutan.

5. Utför följande steg i avsnittet Proxy-server:   
   
    ![Proxyserver](./media/active-directory-saas-zscaler-tutorial/ic769494.png "proxyserver")

    a. Välj **använder en proxyserver för nätverket**.

    b. Ange i textrutan adress **gateway.zscaler.net**.

    c. Ange i textrutan Port **80**.

    d. Välj **Använd ingen proxyserver för lokala adresser**.

    e. Klicka på **OK** att stänga den **inställningar för lokalt nätverk (LAN)** dialogrutan.

6. Klicka på **OK** att stänga den **Internetalternativ** dialogrutan.

> [!TIP]
> Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned. Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_01.png) 

2. Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_02.png) 

3. Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-zscaler-test-user"></a>Skapa en testanvändare Zscaler

Om du vill aktivera Azure AD-användare kan logga in på ZScaler etableras de för ZScaler.  
När det gäller ZScaler är etablering en manuell aktivitet.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Utför följande steg för att konfigurera användaretablering:

1. Logga in på ditt **Zscaler** klient.

2. Klicka på **Administration**.   
   
    ![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")

3. Klicka på **Användarhantering**.   
        
     ![Lägg till](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Lägg till")

4. I den **användare** klickar du på **Lägg till**.
      
    ![Lägg till](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Lägg till")

5. Utför följande steg i avsnittet Lägg till användare:
        
    ![Lägg till användare](./media/active-directory-saas-zscaler-tutorial/ic781038.png "lägga till användare")
   
    a. Typ av **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och **avdelning** av en giltig AAD-konto som du vill etablera.

    b. Klicka på **Spara**.

> [!NOTE]
> Du kan använda något annat ZScaler användarens konto skapas verktyg eller API: er som tillhandahålls av ZScaler att etablera AAD-användarkonton.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Zscaler.

![Tilldela användare][200] 

**Om du vill tilldela Zscaler Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **Zscaler**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_app.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen Zscaler på åtkomstpanelen du bör få automatiskt loggat in på ditt Zscaler program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_203.png

