---
title: "Skapa en modell för Azure Machine Learning (förhandsversion) | Microsoft Docs"
description: "I den här kompletta självstudien får du lära dig att använda Azure Machine Learning (förhandsversion) från slutpunkt till slutpunkt. Det här är del två som beskriver experimentering."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 11/06/2017
ms.openlocfilehash: b723cc23ccbda6c5d39627682116cc314dcf2c0e
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/20/2017
---
# <a name="classify-iris-part-2-build-a-model"></a>Klassificera Iris del 2: Skapa en modell
Azure Machine Learning (förhandsversion) är en integrerad, avancerad lösning för datavetenskap och analys som datatekniker kan använda för att förbereda data, utveckla experiment och distribuera modeller i molnskala.

Den här självstudien är del två i en serie med tre delar. I den här delen av självstudien använder du Azure Machine Learning (förhandsversion) för att:

> [!div class="checklist"]
> * Använda Azure Machine Learning Workbench.
> * Öppna skript och granska kod.
> * Köra skript i en lokal miljö.
> * Granska körningshistorik.
> * Köra skript i en lokal Docker-miljö.
> * Köra skript i ett lokalt Azure CLI-fönster.
> * Köra skript i en fjärransluten Docker-miljö.
> * Köra skript i en Azure HDInsight-molnmiljö.

Den här självstudien använder den tidlösa [Iris-datauppsättningen](https://en.wikipedia.org/wiki/Iris_flower_data_set). Skärmbilderna är Windows-specifika, men upplevelsen är nästan identisk i Mac OS.

## <a name="prerequisites"></a>Krav
Slutför del ett i den här självstudieserien. Gå igenom [självstudien Förbered data](tutorial-classifying-iris-part-1.md), där du får skapa Azure Machine Learning-resurser och installera Azure Machine Learning Workbench, innan du börjar med stegen i den här självstudien.

Du kan också experimentera med skript som körs med en lokal Docker-behållare. Om du vill göra det behöver du ha en installerad och startad Docker-motor (Community Edition räcker) lokalt på din Windows- eller Mac OS-dator. Läs [Installationsinstruktioner för Docker](https://docs.docker.com/engine/installation/) för mer information om hur du installerar Docker.

Om du vill experimentera med att skicka skript som ska köras i en Docker-behållare på en fjärransluten virtuell dator i Azure eller ett Azure HDInsight Spark-kluster kan du följa [instruktionerna för att skapa en Ubuntu-baserad Azure Data Science Virtual Machine eller ett HDInsight-kluster](how-to-create-dsvm-hdi.md).

## <a name="review-irissklearnpy-and-the-configuration-files"></a>Granska iris_sklearn.py och konfigurationsfilerna
1. Öppna Azure Machine Learning Workbench och öppna sedan projektet **myIris** som du skapade i den förra delen av självstudieserien.

2. När projektet är öppet väljer du knappen **Filer** (mappikonen) längst till vänster i fönstret för att öppna fillistan i projektmappen.

3. Välj filen **iris_sklearn.py**. Python-koden öppnas i en ny textredigeringsflik i Workbench.

   ![Öppna en fil](media/tutorial-classifying-iris/open_iris_sklearn.png)

   >[!NOTE]
   >Koden du ser kanske inte är exakt samma som föregående kod eftersom det här exempelprojektet uppdateras ofta.

4. Granska Python-skriptkoden så att du bekantar dig med kodningsstilen. Skriptet utför följande uppgifter:

   - Dataförberedningspaketet **iris.dprep** läses in för att skapa en [pandas-DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

        >[!NOTE]
        >Använd dataförberedningspaket `iris.dprep` som finns med i exempelprojektet. Det ska vara likadant som filen `iris-1.dprep` du skapade i del 1 av självstudien.

   - Slumpvis utvalda funktioner läggs till som gör det svårare att lösa problemet. Slumpmässigheten behövs eftersom Iris är en liten datamängd som enkelt kan klassificeras med nästan 100 % noggrannhet.

   - Machine Learning-biblioteket [scikit-learn](http://scikit-learn.org/stable/index.html) används till att skapa en Logistic Regression-modell. 

   - Serialiserar modellen genom att infoga biblioteket [pickle](https://docs.python.org/2/library/pickle.html) till en fil i mappen `outputs`. Skriptet läser sedan in den och deserialiserar tillbaka den i minnet.

   - Den deserialiserade modellen används till att göra en förutsägelse för en ny post. 

   - Två diagram ritas upp med hjälp av biblioteket [matplotlib](https://matplotlib.org/), en felmatris och en ROC-kurva (receiver operating characteristic) med flera klasser, och de sparas sedan i mappen `outputs`.

   - Objektet `run_logger` används kontinuerligt för att lagra regulariseringshastigheten och modellnoggrannheten i loggarna. Loggarna visas automatiskt i körhistoriken.


## <a name="execute-irissklearnpy-script-in-a-local-environment"></a>Köra skriptet iris_sklearn.py i en lokal miljö

Nu ska vi förbereda oss för att köra skriptet **iris_sklearn.py** för första gången. För skriptet krävs paketen **scikit-learn** och **matplotlib**. Paketet **scikit-learn** är redan installerat av Azure Machine Learning Workbench. Du behöver däremot installera **matplotlib**. 

1. Klicka på menyn **Arkiv** i Azure Machine Learning Workbench och välj **Öppna kommandotolken** för att öppna kommandotolken. Kommandoradsfönstret refereras till som *CLI-fönstret för Azure Machine Learning Workbench*, eller bara *CLI-fönstret*.

2. Skriv följande kommando i CLI-fönstret för att installera Python-paketet **matplotlib**. Det bör vara färdigt på mindre än en minut.

   ```azurecli
   pip install matplotlib
   ```

   >[!NOTE]
   >Om du hoppar över det tidigare kommandot `pip install` körs koden i `iris_sklearn.py`. Om du bara kör `iris_sklearn.py` så producerar inte koden felmatrisen eller ROC-kurvan för flera klasser i visualiseringen av historiken.

3. Gå tillbaka till Workbench-fönstret. 

4. Öppna den nedrullningsbara menyn bredvid ikonen **Spara** i verktygsfältet högst upp i fliken **iris_sklearn.py** och välj sedan **Kör konfiguration**. Välj **lokal** som körningsmiljö och ange sedan `iris_sklearn.py` som skriptet att köra.

5. Gå sedan till den högra sidan av verktygsfältet och ange `0.01` i fältet **Argument**. 

   ![Kör kontroll](media/tutorial-classifying-iris/run_control.png)

6. Klicka på knappen **Kör**. Ett jobb schemaläggs omedelbart. Jobbet står med i fönstret **Jobb** till höger i Workbench-fönstret. 

7. Efter en liten stund ändras statusen för jobbet från **Skickar** till **Körs**, och sedan till **Slutfört**.

   ![Kör sklearn](media/tutorial-classifying-iris/run_sklearn.png)

8. Välj **Slutfört** i jobbstatustexten i fönstret **Jobb**. Ett popup-fönster öppnas med standardutdata (stdout) från skriptkörningen. Om du vill stänga stdout-texten väljer du knappen **Stäng** (**x**) längst upp till höger i popup-fönstret.

9. I samma jobbstatus i fönstret **Jobb** väljer du den blå texten **iris_sklearn.py [n]** (_n_ är körningsnumret) precis ovanför statusen **Slutförd** och starttiden. Fönstret **Körningsegenskaper** öppnas och visar följande information för den specifika körningen:
   - Information om **Körningsegenskaper**
   - **Utdatafiler**
   - Eventuella **Visualiseringar**
   - **Loggar** 

   När körningen har slutförts visas följande resultat i popup-fönstret:

   >[!NOTE]
   >Eftersom vi introducerade vissa slumpmässigheter i inlärningsuppsättningen kommer de exakta resultaten att variera något från de resultat som visas här.

   ```text
   Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
   
   Iris dataset shape: (150, 5)
   Regularization rate is 0.01
   LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
          penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
          verbose=0, warm_start=False)
   Accuracy is 0.6792452830188679
   
   ==========================================
   Serialize and deserialize using the outputs folder.
   
   Export the model to model.pkl
   Import the model from model.pkl
   New sample: [[3.0, 3.6, 1.3, 0.25]]
   Predicted class is ['Iris-setosa']
   Plotting confusion matrix...
   Confusion matrix in text:
   [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
   Confusion matrix plotted.
   Plotting ROC curve....
   ROC curve plotted.
   Confusion matrix and ROC curve plotted. See them in Run History details pane.
   ```

10. Stäng fliken **Körningsegenskaper** och återgå sedan till fliken **iris_sklearn.py**. 

11. Kör skriptet flera gånger. 

    Ange en serie med olika numeriska värden i fältet **Argument**, från `0.001` till `10`. Välj **Kör** för att köra koden några gånger till. De argumentvärden du ändrar varje gång skickas till Logistic Regression-algoritmen i koden, vilket ger olika resultat varje gång.

## <a name="review-the-run-history-in-detail"></a>Granska körningshistoriken i detalj
I Azure Machine Learning Workbench sparas varje skriptkörning som en post i körningshistoriken. Du kan visa körningshistoriken för ett visst skript genom att öppna vyn **Körningar**.

1. Välj knappen **Körningar** (klockikonen) i det vänstra verktygsfältet för att öppna listan över **Körningar**. Välj **iris_sklearn.py** för att se **Instrumentpanelen för körningar** för `iris_sklearn.py`.

   ![Körningsvy](media/tutorial-classifying-iris/run_view.png)

2. Fliken med **Instrumentpanelen för körningar** öppnas. Granska statistiken som sparats från de många körningarna. Diagram återges längst upp på fliken. Varje körning har ett på varandra efterföljande nummer och körningsinformation visas i tabellen längst ned på skärmen.

   ![Instrumentpanelen för körningar](media/tutorial-classifying-iris/run_dashboard.png)

3. Filtrera tabellen och välj sedan något av diagrammen för att visa status, längd, noggrannhet och regulariseringshastighet för varje körning. 

4. Markera två eller tre körningar i tabellen **Körningar** och välj knappen **Jämför**, så öppnas ett detaljerat jämförelsefönster. Granska jämförelsen. Välj bakåtknappen **Körningslista** uppe till vänster i **Jämförelsefönstret** så att du återgår till **Instrumentpanelen för körningar**.

5. Välj en enskild körning så att detaljvyn visas. Observera att statistiken för den valda körningen visas i avsnittet **Körningsegenskaper**. Filerna som skrivits till utdatamappen visas i avsnittet **Utdata** och du kan ladda ned filerna därifrån.

   ![Körningsinformation](media/tutorial-classifying-iris/run_details.png)

   De två diagrammen, felmatrisen och ROC-kurvan för flera klasser, återges i avsnittet **Visualisering**. Du hittar även alla loggfiler i avsnittet **Loggar**.

## <a name="execute-scripts-in-the-local-docker-environment"></a>Köra skript i den lokala Docker-miljön

Med Machine Learning kan du enkelt konfigurera ytterligare körningsmiljöer, till exempel Docker och köra skriptet i dessa miljöer. 

>[!IMPORTANT]
>Du måste ha Docker-motorn installerad lokalt och startad för att utföra det här steget. Läs installationsinstruktionerna för Docker för mer information.

1. Välj **mappikonen** i det vänstra fönstret så att du öppnar listan med **Filer** för projektet. Expandera mappen `aml_config`. 

2. Det finns flera förkonfigurerade miljöer, som **docker-python**, **docker-spark** och **lokalt**. 

   Alla miljöer har två filer, som `docker-python.compute` och `docker-python.runconfig`. Öppna varje fil för att se att vissa alternativ kan konfigureras i textredigeraren.  

   Välj **Stäng** (**x**) på flikarna i eventuella öppna textredigerare för att frigöra utrymme.

3. Kör skriptet **iris_sklearn.py** med miljön **docker-python**: 

   - Välj **klockikonen** i det vänstra verktygsfältet för att öppna fönstret **Körningar**. Välj **Alla körningar**. 
   - Längst upp på fliken **Alla körningar** väljer du **docker-python** som målmiljö istället för standardvärdet **lokalt**. 
   - Gå sedan till höger sida och välj **iris_sklearn.py** som skriptet att köra. 
   - Lämna fältet **Argument** tomt eftersom skriptet anger ett standardvärde. 
   - Klicka på knappen **Kör**.

4. Observera att ett nytt jobb startar. Det visas i fönstret **Jobb** till höger i Workbench-fönstret.

   När du kör mot Docker för första gången tar körningen några minuter mer att slutföras. 

   Azure Machine Learning Workbench skapar en ny docker-fil i bakgrunden. 
   Den nya docker-filen refererar till basavbildningen för Docker, som anges i filen `docker.compute`, Python-paket med beroenden som anges i filen `conda_dependencies.yml`. 
   
   Docker-motorn har följande uppgifter:

    - Laddar ned basavbildningen från Azure.
    - Installerar Python-paketet som anges i filen `conda_dependencies.yml`.
    - Startar en Docker-behållare.
    - Kopior eller referenser, beroende på körningskonfigurationen, den lokala kopian av projektmappen.      
    - Kör skriptet `iris_sklearn.py`.

   I slutändan bör du se exakt samma resultat som när du använder **lokalt** som mål.

5. Nu ska vi prova Spark. Docker-basavbildningen innehåller en förinstallerad och förkonfigurerad Spark-instans. Du kan köra ett PySpark-skript i den på grund av den instansen. Det här är ett enkelt sätt att utveckla och testa ditt eget Spark-program utan att behöva ägna tid åt att installera och konfigurera Spark själv. 

   Öppna filen `iris_spark.py`. Skriptet läser in datafilen `iris.csv` och sedan används Logistic Regression-algoritmen från Spark Machine Learning-biblioteket till att klassificera Iris-datauppsättningen. Ändra nu körningsmiljön till **docker-spark**, skriptet till **iris_spark.py** och kör sedan skriptet igen. Den här processen tar lite längre tid eftersom en Spark-session måste skapas och startas i Docker-behållaren. Du kan också se att stdout inte är samma som stdout för `iris_spark.py`.

6. Gör några fler körningar och prova olika argument. 

7. Öppna filen `iris_spark.py` för att se Logistic Regression-modellen som skapats med Spark Machine Learning-biblioteket. 

8. Interagera med fönstret **Jobb**, öppna listan med körningshistorik och visa detaljer om dina körningar i olika körningsmiljöer.

## <a name="execute-scripts-in-the-azure-machine-learning-cli-window"></a>Kör skript i Azure Machine Learning CLI-fönstret

1. Öppna kommandoradsfönstret i Azure Machine Learning Workbench, välj menyn **Arkiv** och välj **Öppna kommandotolken**. Kommandotolken startar i projektmappen med prompten `C:\Temp\myIris\>`.

   >[!IMPORTANT]
   >Du måste använda kommandotolken (som öppnas från Workbench) för att utföra följande steg.

2. Använd kommandotolken för att logga in på Azure. 

   Workbench-appen och CLI använder oberoende cacheminnen för inloggningsuppgifter vid autentisering mot Azure-resurser. Du behöver bara göra det här en gång, tills token i cacheminnet upphör att gälla. Kommandot **az account list** returnerar listan med prenumerationer som är tillgängliga för din inloggning. Om det finns fler än en använder du ID-värdet från prenumerationen som du vill använda. Ange den prenumerationen som standardkonto med kommandot **az account set -s** och ange värdet som är prenumerations-ID. Bekräfta sedan inställningen med kontokommandot **show**.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. När du är autentiserad och har ställt in den aktuella Azure-prenumerationskontexten skriver du följande kommandon i CLI-fönstret för att installera **matplotlib**, och skicka sedan Python-skriptet som ett experiment att köra.

   ```azurecli
   REM you don't need to run this command if you have installed matplotlib locally from the previous steps
   pip install matplotlib
   
   REM kicks off an execution of the iris_sklearn.py file against the local compute context
   az ml experiment submit -c local .\iris_sklearn.py
   ```

4. Granska utdata. Utdata och resultat är samma som när du tidigare använde Workbench för att köra skriptet. 

5. Kör samma skript igen med hjälp av Docker-körningsmiljön om du har Docker installerat på datorn.

   ```azurecli
   REM executes iris_sklearn.py in the local Docker container Python environment
   az ml experiment submit -c docker-python .\iris_sklearn.py 0.01
   
   REM executes iris_spark.py in the local Docker container Spark environment
   az ml experiment submit -c docker-spark .\iris_spark.py 0.1
   ```
6. Välj **mappikonen** i fönstret till vänster i Workbench så att du visar listan med projektfiler, och öppna Python-skriptet **run.py**. 

   Det här skriptet är användbart när du vill loopa över olika regulariseringshastigheter. Kör experimentet flera gånger med dessa hastigheter. Det här skriptet startar ett `iris_sklearn.py`-jobb med en regulariseringshastighet på `10.0` (löjligt hög hastighet). Skriptet halverar sedan hastigheten i följande körning och så vidare tills hastigheten är så långsam som `0.005`. 

   ```python
   # run.py
   import os
   
   reg = 10
   while reg > 0.005:
       os.system('az ml experiment submit -c local ./iris_sklearn.py {}'.format(reg))
       reg = reg / 2
   ```

   Kör följande kommandon för att öppna skriptet **run.py** från kommandotolken:

   ```cmd
   REM submits iris_sklearn.py multiple times with different regularization rates
   python run.py
   ```

   När `run.py` har slutförts visas ett diagram i körningshistoriken i Workbench.

## <a name="execute-in-a-docker-container-on-a-remote-machine"></a>Köra i en Docker-behållare på en fjärrdator
Om du vill köra skriptet i en Docker-behållare på en fjärrdator med Linux måste du ha SSH-åtkomst (användarnamn och lösenord) till fjärrdatorn. Dessutom måste fjärrdatorn ha en Docker-motor installerad och igång. Det enklaste sättet att ordna en sådan Linux-dator är att skapa en Ubuntu-baserad DSVM (Data Science Virtual Machine) i Azure. Lär dig [att skapa en Ubuntu DSVM som ska användas i Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal).

>[!NOTE] 
>CentOS-baserade DSVM:er stöds *inte*.

1. När den virtuella datorn har skapats kan du ansluta den som en körningsmiljö om du genererar ett par `.runconfig`- och `.compute`-filer. Använd följande kommando för att generera filerna. Vi ger den nya miljön namnet `myvm`.
 
   ```azurecli
   REM creates an myvm compute target
   az ml computetarget attach --name myvm --address <IP address> --username <username> --password <password> --type remotedocker
   ```
   
   >[!NOTE]
   >IP-adresserna kan även vara offentligt adresserbara FQDN (fullständigt kvalificerade domännamn), till exempel `vm-name.southcentralus.cloudapp.azure.com`. Det är en bra idé att lägga till FQDN för din DSVM och använda dem här i stället för IP-adresser. Det här är en bra idé eftersom du kanske vill stänga av den virtuella datorn senare för att spara på kostnaden. Nästa gång du startar den virtuella datorn kan IP-adresserna ha ändrats.

   Kör sedan följande kommando för att skapa Docker-avbildningen i den virtuella datorn, så att den blir redo att köra skripten:
   
   ```azurecli
   REM prepares the myvm compute target
   az ml experiment prepare -c myvm
   ```
   >[!NOTE]
   >Du kan även ändra värdet för `PrepareEnvironment` i `myvm.runconfig` från standardvärdet `false` till `true`. Den här förändringen förbereder automatiskt Docker-behållaren vid den första körningen.

2. Redigera den genererade filen `myvm.runconfig` under `aml_config` och ändra ramverket från standardvärdet `PySpark` till `Python`:

   ```yaml
   "Framework": "Python"
   ```
   >[!NOTE]
   >Det bör också fungera att lämna ramverks-inställningen som PySpark. Det är däremot mindre effektivt om du inte verkligen behöver en Spark-session för att köra Python-skriptet.

3. Kör samma kommando som du gjorde förut i CLI-fönstret, förutom att du använder _myvm_ som mål den här gången:
   ```azurecli
   REM executes iris_sklearn.py in a remote Docker container
   az ml experiment submit -c myvm .\iris_sklearn.py
   ```
   Kommandot körs som om du använder en `docker-python`-miljö, förutom att körningen sker på den virtuella fjärrdatorn med Linux. Samma utdata visas i CLI-fönstret.

4. Nu ska vi prova att använda Spark i behållaren. Öppna Utforskaren. Du kan också göra detta från CLI-fönstret om du är bekant med grundläggande kommandon för filmanipulering. Skapa en kopia av filen `myvm.runconfig` och ge den namnet `myvm-spark.runconfig`. Redigera den nya filen och ändra inställningen `Framework` från `Python` till `PySpark`:
   ```yaml
   "Framework": "PySpark"
   ```
   Gör inga ändringar i filen `myvm.compute`. Samma Docker-avbildning på samma virtuella dator används vid Spark-körningen. I den nya filen `myvm-spark.runconfig` pekar fältet `target` på samma fil `myvm.compute` via namnet `myvm`.

5. Ange följande kommando för att köra skriptet i Spark-instansen i den fjärranslutna Docker-behållaren:
   ```azureli
   REM executes iris_spark.py in a Spark instance on a remote Docker container
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="execute-script-in-an-hdinsight-cluster"></a>Köra skript i ett HDInsight-kluster
Du kan också köra skriptet i ett faktiskt HDInsight Spark-kluster. Lär dig [att skapa HDInsight Spark-kluster som ska användas i Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal).

>[Obs!] HDInsight-klustret måste använda Azure Blob som primär lagring. Användning av Azure Data Lake-lagring stöds inte ännu.

1. Om du har tillgång till ett Spark for Azure HDInsight-kluster kan du generera ett kommando för konfiguration av HDInsight-körningen på det sätt som visas här. Ange HDInsight-klusternamnet samt ditt användarnamn och lösenord för HDInsight som parametrar. Ange följande kommando:

   ```azurecli
   REM creates a compute target that points to a HDInsight cluster
   az ml computetarget attach --name myhdi --address <cluster head node FQDN> --username <username> --password <password> --type cluster

   REM prepares the HDInsight cluster
   az ml experiment prepare -c myhdi
   ```

   Klusterhuvudnodens FQDN är vanligtvis `<cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >`username` är klustrets SSH-användarnamn. Standardvärdet är `sshuser` om du inte ändrat det under HDInsight-konfigurationen. Värdet är inte `admin`, som är den andra användaren som skapades under konfigurationen för att ge åtkomst till klustrets administrationswebbplats. 

2. Kör följande kommando så körs skriptet i HDInsight-klustret:

   ```azurecli
   REM executes iris_spark on the HDInsight cluster
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >När du kör mot ett fjärranslutet HDInsight-kluster kan du även visa körningsinformation om YARN-jobbet (Yet Another Resource Negotiator) på `https://<cluster_name>.azurehdinsight.net/yarnui` med användarkontot `admin`.


## <a name="next-steps"></a>Nästa steg
I den här andra delen av självstudieserien i tre delar har du lärt dig att använda Azure Machine Learning för att:
> [!div class="checklist"]
> * Använda Azure Machine Learning Workbench.
> * Öppna skript och granska kod.
> * Köra skript i en lokal miljö.
> * Granska körningshistorik.
> * Köra skript i en lokal Docker-miljö.
> * Köra skript i ett lokalt Azure CLI-fönster.
> * Köra skript i en fjärransluten Docker-miljö.
> * Köra skript i en HDInsight-molnmiljö.

Du är nu redo att gå vidare till den tredje delen i serien. Nu när du har skapat Logistic Regression-modellen ska vi distribuera den som en realtidswebbtjänst.

> [!div class="nextstepaction"]
> [Distribuera en modell](tutorial-classifying-iris-part-3.md)
