---
title: "Tjänst-till-tjänst-autentisering: Java med Data Lake Store med Azure Active Directory | Microsoft Docs"
description: "Lär dig att uppnå service to service autentisering med Data Lake Store med Azure Active Directory med Java"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/29/2017
ms.author: nitinme
ms.openlocfilehash: f68239b2f4f7a2ba0617023d9397184c483a4d99
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-java"></a>Tjänst-till-tjänst-autentisering med Data Lake Store använder Java
> [!div class="op_single_selector"]
> * [Använda Java](data-lake-store-service-to-service-authenticate-java.md)
> * [Använda .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Använda Python](data-lake-store-service-to-service-authenticate-python.md)
> * [Använda REST-API](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

I den här artikeln får information du om hur du använder Java SDK för att göra service to service autentisering med Azure Data Lake Store. Slutanvändarens autentisering med Data Lake Store med hjälp av Java SDK stöds inte.

## <a name="prerequisites"></a>Krav
* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Skapa ett program för Azure Active Directory ”Web”**. Du måste ha slutfört stegen i [tjänst-till-tjänst-autentisering med Data Lake Store med Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). Den här självstudien använder Maven för bygg- och projektberoenden. Även om det är möjligt att skapa utan att använda ett build-system som Maven eller Gradle, gör de här systemen det mycket enklare att hantera beroenden.

* (Valfritt) Och IDE-liknande [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) eller [Eclipse](https://www.eclipse.org/downloads/) eller liknande.

## <a name="service-to-service-authentication"></a>Tjänst-till-tjänst-autentisering
1. Skapa ett Maven-projekt med [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) från kommandoraden eller med hjälp av en IDE. Anvisningar för hur du skapar ett Java-projekt med IntelliJ finns [här](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Anvisningar för hur du skapar ett Java-projekt med Eclipse finns [här](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).

2. Lägg till följande beroenden till din Maven **pom.xml**-fil. Lägg till följande fragment före taggen **\</project>**:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.2.3</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    Det första beroendet är för att använda Data Lake Store-SDK:n (`azure-data-lake-store-sdk`) från maven-centrallagret. Det andra beroendet är för att ange vilket loggningsramverk (`slf4j-nop`) som ska användas för programmet. Data Lake Store SDK:n använder sig av loggningsfasaden [slf4j](http://www.slf4j.org/) som låter dig välja från en rad populära ramverk för loggning som log4j, Java-loggning, logback och så vidare, eller ingen loggning alls. I det här exemplet inaktiverar vi loggning, därför använder vi **slf4j-nop** bindning. Om du vill använda andra alternativ för loggning i din app, se [här](http://www.slf4j.org/manual.html#projectDep).

3. Lägg till följande importuttryck i programmet.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

4. Använd följande kodavsnitt i Java-program för att hämta token för Active Directory webbprogrammet du skapat tidigare med ett av underklasser för `AccessTokenProvider` (i följande exempel används `ClientCredsTokenProvider`). Tokenleverantören cachelagrar autentiseringsuppgifter som används för att hämta token i minnet och förnyar automatiskt token när de håller på att gå ut. Det är möjligt att skapa egna underklasser för `AccessTokenProvider` så token hämtas av kunden koden. Nu ska vi bara använda den som angetts i SDK.

    Ersätt **FILL-IN-HERE** med de faktiska värdena för Azure Active Directory-webbappen.

        private static String clientId = "FILL-IN-HERE";
        private static String authTokenEndpoint = "FILL-IN-HERE";
        private static String clientKey = "FILL-IN-HERE";
    
        AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);   

Data Lake Store SDK:n erbjuder praktiska metoder för att hantera de säkerhetstokens som behövs för att kommunicera med Data Lake Store-kontot. Dock tvingar inte SDK:n dig att använda enbart de här metoderna. Du kan använda valfria andra metoder för att hämta token, som att använda [Azure Active Directory SDK:n](https://github.com/AzureAD/azure-activedirectory-library-for-java) eller din egna anpassade kod.

## <a name="next-steps"></a>Nästa steg
I den här artikeln beskrivs hur du använder slutanvändarens autentisering för att autentisera med Azure Data Lake Store med hjälp av Java SDK. Du kan nu se följande artiklar som talar om hur du använder Java SDK för att arbeta med Azure Data Lake Store.

* [Dataåtgärder på Data Lake Store med hjälp av Java SDK](data-lake-store-get-started-java-sdk.md)


