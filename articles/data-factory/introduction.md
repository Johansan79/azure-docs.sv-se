---
title: Introduktion till Azure Data Factory | Microsoft Docs
description: "Lär dig mer om den molnbaserade dataintegreringstjänsten Azure Data Factory som samordnar och automatiserar förflyttning och omvandling av data."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2017
ms.author: shlo
ms.openlocfilehash: b797ee3ef270ff3420ff9e7f4aa8032641714d7a
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/13/2017
---
# <a name="introduction-to-azure-data-factory"></a>introduktion till Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – allmänt tillgänglig](v1/data-factory-introduction.md)
> * [Version 2 – förhandsversion](introduction.md)

När det gäller stordata, lagras råa, oordnade data ofta i relationella, icke-relationella och andra lagringssystem. Men i sig självt så har rådata inte rätt kontext eller mening för att ge analytiker, dataforskare och beslutsfattare meningsfulla insikter. 

Stordata kräver en tjänst som kan samordna och operationalisera processer för att förfina dessa enorma lager av rådata till handlingsbara affärsinsikter. Azure Data Factory är en hanterad molntjänst som skapats för dessa komplexa, hybrida, ETL- (extract-transform-load), ELT- (extract-load-transform) och dataintegreringsprojekt.

Tänk dig till exempel ett spelföretag som samlar in petabyte med spelloggar från spel i molnet. Företaget vill analysera dessa loggar för att få insikter om kunders preferenser, demografi och användningsbeteende. Det vill också identifiera möjligheter till merförsäljning och korsförsäljning, utveckla intressanta nya funktioner, driva affärstillväxten och ge en bättre kundupplevelse.

När företaget ska analysera loggarna måste de använda referensdata, till exempel kundinformation, spelinformation och marknadsföringskampanjinformation som finns i ett lokalt datalager. Företaget vill använda dessa data från det lokala datalagret och kombinera dem med ytterligare loggdata som de har i ett molndatalager. 

För att utvinna insikter så hoppas de kunna bearbeta dessa sammanslagna data med hjälp av ett Spark-kluster i molnet (Azure HDInsight) och publicera omvandlade data till ett molninformationslager, till exempel Azure SQL Data Warehouse, för att enkelt kunna skapa en rapport från dem. De vill automatisera det här arbetsflödet och övervaka och hantera det enligt ett dagligt schema. De vill också köra det när filer landar i en bloblagerbehållare.

Azure Data Factory är en plattform som löser den här typen av datascenarier. Det är en *molnbaserad dataintegreringstjänst som gör att du kan skapa datadrivna arbetsflöden i molnet för att samordna och automatisera dataförflyttning och dataomvandling*. Med Azure Data Factory kan du skapa och schemalägga datadrivna arbetsflöden (kallas pipelines) som kan mata in data från olika datalager. Den kan bearbeta och transformera data med hjälp av beräkningstjänster, till exempel Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics och Azure Machine Learning. 

Dessutom kan du publicera utdata till datalager som Azure SQL Data Warehouse för Business Intelligence (BI)-program kan använda. Slutligen kan rådata ordnas, via Azure Data Factory, i meningsfulla datalager och datasjöar för att ge bättre beslutsunderlag.

![Toppnivåvy över Data Factory](media/introduction/big-picture.png)

> [!NOTE]
> Den här artikeln gäller för version 2 av Data Factory, som för närvarande är en förhandsversion. Om du använder version 1 av Data Factory-tjänsten, som är allmänt tillgänglig, läser du [Introduktion till Data Factory version 1](v1/data-factory-introduction.md).

## <a name="how-does-it-work"></a>Hur fungerar det?
Pipelines (datadrivna arbetsflöden) i Azure Data Factory utför vanligen följande fyra steg:

![Datadrivet arbetsflöde i fyra steg](media/introduction/four-steps-of-a-workflow.png)

### <a name="connect-and-collect"></a>Ansluta och samla in

Företag har olika typer av data som befinner sig på olika platser, lokalt, i molnet, strukterade, ostrukturerade och delvis strukturerade och alla dessa data anländer i olika intervall och med olika hastighet. 

Det första steget när det gäller att skapa ett informationsproduktionssystem är att ansluta till alla nödvändiga data- och bearbetningskällor, exempelvis SaaS-tjänster, databaser, filresurser och FTP-webbtjänster. Nästa steg är att flytta data efter behov till en central plats för senare bearbetning.

Utan Data Factory måste företag skapa egna dataöverföringskomponenter eller skriva anpassade tjänster för att integrera dessa data- och bearbetningskällor. Det är dyrt och svårt att integrera och underhålla sådana system. De saknar dessutom ofta övervakning, varningar och de kontroller i företagsklass som en helt hanterad tjänst kan erbjuda.

Med Data Factory kan du använda [kopieringsaktiviteten](copy-activity-overview.md) i en datapipeline för att flytta data från datalager lokalt och molnet till ett centralt datalager i molnet där du kan analysera dem. Du kan till exempel samla in data i en Azure Data Lake Store och sedan omvandla dessa data med en Azure Data Lake Analytics-databearbetningstjänst. Eller så kan du samla in data i en Azure Blob Storage och sedan omvandla de med ett Azure HDInsight Hadoop-kluster.

### <a name="transform-and-enrich"></a>Omvandla och berika
När data presenteras i ett centraliserat datalager i molnet, kan du bearbeta eller omvandla dem med beräkningstjänster som HDInsight Hadoop, Spark, Data Lake Analytics och Machine Learning. Du kan producera omvandlade data på ett tillförlitligt sätt enligt ett anpassningsbart schema och fylla på med betrodda data i produktionsmiljöer.

### <a name="publish"></a>Publicera
Efter att rådata har förfinats till en form som företaget kan använda, läser du in data i Azure Data Warehouse, Azure SQL Database, Azure CosmosDB eller den analysmotor som ditt företags användare kan peka till från sina business intelligence-verktyg.

### <a name="monitor"></a>Övervaka
När du har skapat och distribuerat din pipeline för dataintegrering och fått affärsvärde från förfinade data, kan du övervaka schemalagda aktiviteter och pipelines för att se hur många som lyckats respektive misslyckats. Azure Data Factory har inbyggt stöd för pipelineövervakning via Azure Monitor, API, PowerShell, Microsoft Operations Management Suite och hälsopaneler på Azure Portal.

## <a name="whats-different-in-version-2"></a>Vad är nytt i version 2?
Azure Data Factory version 2 bygger vidare på den ursprungliga Azure Data Factory dataöverförings- och omvandlingstjänsten och utökar den med en bredare uppsättning molninriktade dataintegreringsscenarier. Azure Data Factory version 2 har följande funktioner:

- Kontrollflöde och skala
- Distribuera och kör SQL Server Integration Services-paket (SSIS) i Azure

Efter lanseringen av version 1 upptäckte vi att kunderna behöver skapa komplexa hybridlösningar för dataintegrering som kräver både dataförflyttning och -bearbetning i molnet, lokalt och i virtuella datorer i molnet. De här kraven skapade ett behov av att kunna överföra och bearbeta data inom säkra virtuella nätverksmiljöer och att kunna skala ut med bearbetning på begäran.

Allteftersom datapipelines har blivit en viktig del i företags analysstrategier, har vi sett hur dessa viktiga dataaktiviteter kräver flexibel schemaläggning för att klara ökade databelastningar och händelseaktiverade körningar. Eftersom de här åtgärderna blir allt mer komplexa så ökar också kraven på att tjänsten ska ha stöd för vanliga arbetsflödesparadigmer, inklusive förgrening, loopning och villkorsstyrd bearbetning.

Med version 2 kan du också migrera befintliga SSIS-paket till molnet. Du kan lyfta och skifta SSIS som en Azure-tjänst som hanteras inom ADF, med hjälp av den nya funktionen Intgration Runtimes (IR). Med en SSIS IR i version 2 kan du köra, hantera, övervaka och skapa SSIS-paket i molnet.

### <a name="control-flow-and-scale"></a>Kontrollflöde och skala 
För att kunna använda olika integreringsflöden och mönster i ett modernt informationslager har Data Factory en ny flexibel datapipelinemodell som inte längre är knuten till Time Series-data. I den här versionen kan du anpassa villkor och branchning i kontrollflödet för en datapipeline och explicit skicka parametrar inom och mellan dessa flöden.

Nu har du friheten att anpassa alla flödesstilar som krävs för dataintegrering som kan skickas på begäran eller regelbundet enligt ett schema. Några vanliga flöden som nu är aktiverade och som tidigare inte var möjliga är:   

- Kontrollflöde:
    - Kedjesammansättning av aktiviteter i sekvens i en pipeline
    - Branchning av aktiviteter inom en pipeline
    - Parametrar
        - Parametrar kan definieras på pipelinenivå och argument kan skickas när du anropar din pipeline på begäran eller via en utlösare.
        - Aktiviteter kan använda argumenten som skickas till pipelinen.
    - Skicka anpassade tillstånd
        - Aktivitetsutdata inklusive tillstånd kan användas av en efterföljande aktivitet i pipelinen.
    - Loopningsbehållare
        - For-each 
- Utlösningsbaserade flöden
    - Pipelines kan utlösas på begäran eller vid en tidpunkt.
- Deltaflöden
    - Använd parametrar och definiera ditt övre vattenmärke för deltakopiering vid förflyttning av dimensions- eller referenstabeller från en relationslagringsplats som finns lokalt eller i molnet för att läsa in data i sjön. 

Mer information finns i [branchnings- och länkningsaktiviteter i en Data Factory-pipeline](tutorial-control-flow.md).

### <a name="deploy-ssis-packages-to-azure"></a>Distribuera SSIS-paket till Azure 
Om du vill flytta SSIS-arbetsbelastningar kan du skapa en Data Factory version 2 och etablera en Azure-SSIS IR (Integration Runtime). Azure-SSIS IR är ett helt hanterat kluster av virtuella Azure-datorer (noder) dedikerade för att köra dina SSIS-paket i molnet. Stegvisa instruktioner finns i guiden [distribuera SQL Server Integration Services-paket till Azure](tutorial-deploy-ssis-packages-azure.md). 
 

### <a name="sdks"></a>SDK:er
Om du är en avancerade användare och behöver ett programmeringsgränssnitt har version 2 en omfattande uppsättning SDK:er som du kan använda för att skapa, hantera och övervaka pipelines i din favoritutvecklingsmiljö (IDE).

- *.NET SDK*: .NET SDK har uppdaterats för version 2. 
- *PowerShell*: PowerShell-cmdletarna har uppdaterats för version 2. Cmdletar för version 2 har **DataFactoryV2** i namnet. Till exempel: Get-AzureRmDataFactoryV2. 
- *Python SDK*: Den här SDK:n är ny i version 2.
- *REST API*: REST API har uppdaterats för version 2.  

SDK:erna som har uppdaterats för version 2 är inte bakåtkompatibla med version 1-klienter. 

### <a name="monitoring"></a>Övervakning
Version 2 har för närvarande enbart stöd för övervakning av datafabriker med hjälp av SDK:er. Portalen har inte stöd för övervakning av datafabriker med version 2 än. 

## <a name="load-the-data-into-a-lake"></a>Läsa in data i en sjö
Data Factory har över 30 anslutningar som gör att du kan läsa in data från hybridmiljöer och heterogena miljöer till Azure. Information om de senaste prestandaresultaten från interna tester och justeringsförslag finns i [Prestanda- och justeringsguiden](copy-activity-performance.md). 

Dessutom kan har vi nyligen aktiverat hög tillgänglighet och skalbarhet för en lokal Integration Runtime som du installerar i ett privat nätverk. Detta löser krav från nivå 1 storbolaget på bättre tillgänglighet och skalbarhet.

## <a name="top-level-concepts-in-version-2"></a>Toppnivåbegrepp i version 2
En Azure-prenumeration kan ha en eller flera Azure Data Factory-instanser (eller datafabriker). Azure Data Factory består av fyra nyckelkomponenter. De här komponenterna samverkar för att tillhandahålla en plattform där du kan skapa datadrivna arbetsflöden med steg för att flytta och omvandla data.

### <a name="pipeline"></a>Pipeline
En datafabrik kan ha en eller flera pipelines. En pipeline är en logisk gruppering aktiviteter för att utföra en arbetsprocess. Aktiviteterna i en pipeline utför en uppgift tillsammans. En pipeline kan till exempel innehålla en grupp med aktiviteter som matar in data från en Azure-blob och sedan kör en Hive-fråga på ett HDInsight-kluster för att partitionera data. 

Fördelen med detta är att pipelinen låter dig hantera aktiviteter som en uppsättning istället för enskilt. Aktiviteter i en pipeline kan sammanlänkas för att köras sekventiellt eller så kan de köras fristående och parallellt.

### <a name="activity"></a>Aktivitet
Aktiviteter representerar ett bearbetningssteg i en pipeline. Du kan till exempel använda en kopieringsaktivitet för att kopiera data från ett datalager till ett annat. På samma sätt kan du använda en Hive-aktivitet som kör en Hive-fråga på ett Azure HDInsight-kluster för att transformera eller analysera dina data. Data Factory stöder tre typer av aktiviteter: dataförflyttning, datatransformering och kontroll.

### <a name="datasets"></a>Datauppsättningar
Datauppsättningar representerar datastrukturer i datalager som pekar på eller refererar till de data som du vill använda i dina aktiviteter som indata eller utdata. 

### <a name="linked-services"></a>Länkade tjänster
Länkade tjänster liknar anslutningssträngar som definierar den anslutningsinformation som behövs för att Data Factory ska kunna ansluta till externa resurser. Man kan se det som att datamängden representerar strukturen för data och den länkade tjänsten definierar anslutningen till datakällan. Till exempel anger en länkad Azure Storage-tjänst en anslutningssträng för att ansluta till ett Azure Storage-konto. Och en Azure Blob-datauppsättning anger vilken blobbehållare och mapp som innehåller data.

Länkade tjänster används för två syften i Data Factory:

- Att representera ett **datalager** som inkluderar, men inte begränsas till, en lokal SQL Server-databas, Oracle-databas, filresurs eller ett Azure Blob Storage-konto. En lista över datalager som stöds finns i artikeln om [kopieringsaktiviteter](copy-activity-overview.md).

- Så här visar du en **beräkningsresurs** som kan vara värd för körningen av en aktivitet. HDInsightHive-aktiviteten körs till exempel på ett HDInsight Hadoop-kluster. En lista över transformeringsaktiviteter och beräkningsmiljöer som stöds finns i artikeln om [omvandling av data](transform-data.md).

### <a name="triggers"></a>Utlösare
Utlösare representerar en bearbetningsenhet som avgör när en pipelinekörning måste startas. Det finns olika typer av utlösare för olika typer av händelser. I förhandsvisningen, stöder vi wall-clock scheduler-utlösaren. 


### <a name="pipeline-runs"></a>Pipelinekörningar
En pipelinekörning är en instans av en pipelinekörning. Pipelinekörningar initieras vanligen genom att skicka argumenten till de parametrar som definierats i pipelines. Argumenten kan skickas manuellt eller i en utlösardefinition.

### <a name="parameters"></a>Parametrar
Parametrar är nyckel/värde-par i en skrivskyddad konfiguration.  Parametrar har definierats i pipelinen. Argumenten för de definierade parametrarna skickas vid körning från körningskontexten som skapats av en utlösare eller en pipeline som körs manuellt. Aktiviteter i pipelinen använder parametervärdena.

En datauppsättning är en starkt typifierad parameter och en återanvändbar/refererbar entitet. En aktivitet kan referera till datauppsättningar och kan använda egenskaperna som definierats i definitionen för datauppsättningen.

En länkad tjänst är också en starkt typifierad parameter som innehåller anslutningsinformationen till antingen ett datalager eller en beräkningsmiljö. Det är också en återanvändningsbar/refererbar entitet.

### <a name="control-flow"></a>Kontrollflöde
Kontrollflöde är en orkestrering av pipelineaktiviteter som innefattar kedjesammansättning av aktiviteter i en sekvens, branchning definiering av parametrar på pipelinenivå och argument som skickas vid anrop till pipelinen på begäran eller från en utlösare. Det innefattar även att skicka anpassade tillstånd och loopbehållare, d.v.s. for-each-iteratorer.


Mer information om Data Factory-begrepp finns i följande artiklar:

- [Datamängder och länkade tjänster](concepts-datasets-linked-services.md)
- [Pipelines och aktiviteter](concepts-pipelines-activities.md)
- [Integration runtime](concepts-integration-runtime.md)

## <a name="supported-regions"></a>Regioner som stöds

För närvarande kan du skapa datafabriker i regionerna USA, västra, USA, östra 2 och Europa, västra. Dock kan en datafabrik ha åtkomst till datalager och beräkna tjänster i andra Azure-regioner för att flytta data mellan datalager eller bearbeta data med hjälp av beräkningstjänster.

Azure Data Factory lagrar inte själv några data. Du kan använda den för att skapa datadrivna arbetsflöden som samordnar flödet av data mellan datalager som stöds och bearbetning av data med hjälp av beräkningstjänster i andra regioner eller i en lokal miljö. Du kan också övervaka och hantera arbetsflöden med både program- och användargränssnittsmetoder.

Även om Data Factory bara är tillgängligt i regionerna USA, östra, USA, östra 2, och Europa, västra, så finns tjänsten som driver dataförflyttning i Data Factory tillgänglig globalt i flera regioner. Om ett datalager finns bakom en brandvägg kan en gateway för datahantering som installerats i din lokala miljö flytta data i stället.

Exempelvis kan vi anta att dina beräkningsmiljöer, som t.ex. Azure HDInsight-kluster och Azure Machine Learning, körs utanför regionen Europa, västra. Du kan skapa och använda en Azure Data Factory-instans i norra Europa och använda den för att schemalägga jobb i dina beräkningsmiljöer i Västeuropa. Det tar några millisekunder för Data Factory att utlösa jobbet i din beräkningsmiljö, men den tid det tar för att köra jobbet ändras inte.

## <a name="next-steps"></a>Nästa steg
Lär dig skapa en datafabrik med stegvisa instruktioner i följande snabbstarter: [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST-API](quickstart-create-data-factory-rest-api.md) och Azure Portal. 
