---
title: "Skapa Apache Spark machine learning-program på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur du skapar maskininlärning Apache Spark-program HDInsight Spark kluster med Jupyter-anteckningsbok"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: ec69dfb1a0d43b73efbada654175c54bf315a1e5
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/29/2017
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Skapa Apache Spark machine learning-program på Azure HDInsight

Lär dig hur du skapar ett Apache Spark maskininlärning för program med hjälp av ett Spark-kluster i HDInsight. Den här artikeln visar hur du använder Jupyter-anteckningsboken tillgänglig när klustret är att skapa och testa det här programmet. Programmet använder HVAC.csv exempeldata som är tillgänglig på alla kluster som standard.

**Krav:**

Du måste ha följande:

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Förstå datauppsättningen
Innan vi börja skapa programmet, låt oss förstå strukturen för de data som vi skapa programmet och vilken typ av analys gör vi i data. 

I den här artikeln använder vi provet **HVAC.csv** datafil som är tillgängliga i Azure Storage-konto som du har associerat med HDInsight-klustret. Inom lagringskonto, filen är på **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Ladda ned och öppna CSV-filen för att få en ögonblicksbild av data.  

![Ögonblicksbild av data som används för Spark machine learning exempel](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "ögonblicksbild av data som används för Spark machine learning-exempel")

Data visar de mål och faktiska temperatur av en byggnad som har installerats för HVAC-system. Vi antar den **System** kolumn representerar system-ID och **SystemAge** kolumn representerar antalet år HVAC-system har på plats och byggnaden.

Vi använder informationen för att förutsäga om en byggnad ska vara högre temperaturer eller colder baserat på mål temperaturen, en system-ID och system ålder.

## <a name="app"></a>Skriva ett Spark machine learning-program med hjälp av Spark MLlib
I det här programmet använder vi en Spark ML-pipeline för att utföra en klassificering för dokumentet. I pipelinen, vi dela dokumentet till ord, konvertera orden till en numerisk funktionen vector och slutligen skapar en förutsägelse modell med hjälp av funktionen angreppsmetoderna och etiketter. Utför följande steg för att skapa programmet.

1. På startsidan i [Azure-portalen](https://portal.azure.com/) klickar du på panelen för ditt Spark-kluster (om du har fäst det på startsidan). Du kan också navigera till ditt kluster under **Bläddra bland alla** > **HDInsight-kluster**.   
2. Från Spark-klusterbladet, klickar du på **Klusterinstrumentpanel** och sedan på **Jupyter Notebook**. Ange administratörsautentiseringsuppgifterna för klustret om du uppmanas att göra det.
   
   > [!NOTE]
   > Du kan också nå Jupyter Notebook för ditt kluster genom att öppna nedanstående URL i webbläsaren. Ersätt **CLUSTERNAME** med namnet på klustret:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Skapa en ny anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.
   
    ![Skapa en Jupyter-anteckningsbok Spark machine learning exempel](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "skapa en Jupyter-anteckningsbok Spark machine learning-exempel")
4. En ny anteckningsbok skapas och öppnas med namnet Untitled.pynb. Klicka på anteckningsbokens namn högst upp och ange ett trevligt namn.
   
    ![Ange ett namn för anteckningsboken Spark machine learning exempel](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "ange ett namn för anteckningsboken Spark machine learning-exempel")
5. Du behöver inte uttryckligen skapa några kontexter eftersom du har skapat anteckningsboken med hjälp av PySpark-kerneln. Spark- och Hive-kontexterna skapas automatiskt för dig när du kör den första kodcellen. Du kan starta genom att importera de typer som krävs för det här scenariot. Klistra in följande kodavsnitt i en tom cell och tryck sedan på **SKIFT + RETUR**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Du måste nu läsa in data (hvac.csv), tolkas det och använda den för att träna modellen. För det här kan du definiera en funktion som kontrollerar om faktiska byggnaden är större än temperaturen som mål. Om den faktiska temperaturen är större än byggnaden är hot, med värdet **1.0**. Om den faktiska temperaturen är mindre byggnaden är kyla, med värdet **0,0**. 
   
    Klistra in följande kodavsnitt i en tom cell och tryck på **SKIFT + RETUR**.

        # List the structure of data for better understanding. Because the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Konfigurera Spark machine learning pipeline som består av tre steg: tokenizer och hashingTF lr. Mer information om vad är en pipeline och hur det fungerar finns <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pipeline</a>.
   
    Klistra in följande kodavsnitt i en tom cell och tryck på **SKIFT + RETUR**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Anpassa pipeline för utbildning-dokumentet. Klistra in följande kodavsnitt i en tom cell och tryck på **SKIFT + RETUR**.
   
        model = pipeline.fit(training)
3. Kontrollera att dokumentet utbildning kontrollpunkt förloppet med programmet. Klistra in följande kodavsnitt i en tom cell och tryck på **SKIFT + RETUR**.
   
        training.show()
   
    Detta bör ge utdata som liknar följande:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Gå tillbaka och kontrollera utdata mot rådata CSV-filen. Den första raden i CSV-fil har till exempel data:

    ![Utdata ögonblicksbild Spark machine learning exempel](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "utdata ögonblicksbild Spark machine learning-exempel")

    Lägg märke till att den faktiska temperaturen är mindre än den target temperatur tyder på byggnaden är Kall. Därför i utdata, utbildning värdet för **etikett** i den första raden är **0,0**, vilket innebär att byggnaden är inte aktivt.

1. Förbered en datauppsättning för att köra den tränade modellen mot. Om du vill göra det, skulle vi överföra på ett Systemid och system ålder (betecknas som **SystemInfo** i utbildning utdata), och modellen skulle förutsäga om byggnad med det system-ID och system ålder skulle vara högre temperaturer (betecknas med 1.0) eller kallare (betecknas med 0,0).
   
   Klistra in följande kodavsnitt i en tom cell och tryck på **SKIFT + RETUR**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Slutligen göra förutsägelser på testdata. Klistra in följande kodavsnitt i en tom cell och tryck på **SKIFT + RETUR**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Du bör se utdata som liknar följande:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   Från den första raden i förutsägelsen finns för ett HVAC-system med ID 20 och 25 år system ålder byggnaden kommer att varm (**förutsägelse = 1.0**). Det första värdet för DenseVector (0.49999) motsvarar förutsägelser 0,0 och det andra värdet (0.5001) motsvarar förutsägelser 1.0. I utdata, även om det andra värdet är bara marginellt högre visar modellen **förutsägelse = 1.0**.
4. När du har kört appen bör du stänga ned anteckningsboken för att frigöra resurser. Du gör det genom att klicka på **Stäng och stoppa** i anteckningsbokens **Fil**-meny. Då avslutas anteckningsboken och stängs ned.

## <a name="anaconda"></a>Använd Anaconda scikit-Läs bibliotek för Spark machine learning
Apache Spark-kluster i HDInsight innehåller Anaconda-bibliotek. Detta omfattar även den **scikit-Läs** bibliotek för machine learning. Biblioteket innehåller också olika datauppsättningar som du kan använda för att skapa exempelprogrammen direkt från en Jupyter-anteckningsbok. Exempel på med scikit-Läs biblioteket, se [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll](apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-plugin för IntelliJ IDEA till att skapa och skicka Spark Scala-appar](apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-plugin för IntelliJ IDEA till att felsöka Spark-program via fjärranslutning](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för Apache Spark-klustret i Azure HDInsight](apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]:../hadoop/apache-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]:../hadoop/apache-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
