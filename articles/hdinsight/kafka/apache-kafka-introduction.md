---
title: "Introduktion till Apache Kafka på HDInsight - Azure | Microsoft Docs"
description: "Lär dig mer om Apache Kafka på HDInsight: vad det är, dess syfte och var du hittar exempel och kommer igång."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: c4e0d792ae8f4c17d53430f49d81d179e56b9722
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/15/2017
---
# <a name="introducing-apache-kafka-on-hdinsight"></a>Introduktion till Apache Kafka på HDInsight

[Apache Kafka](https://kafka.apache.org) är en distribuerad direktuppspelningsplattform med öppen källkod som kan användas för att skapa realtidsuppspelade datapipelines och program. Kafka tillhandahåller även funktionen för asynkron meddelandekö som liknar en meddelandekö där du kan publicera och prenumerera på namngivna dataströmmar. Kafka på HDInsight erbjuder en hanterad, maximalt skalbar och högtillgänglig tjänst i Microsoft Azure-molnet.

## <a name="why-use-kafka-on-hdinsight"></a>Varför använda Kafka på HDInsight?

Kafka på HDInsight har följande två funktioner:

* Servicenivåavtal (SLA): [SLA-information för HDInsight](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/).

* Meddelandemönster av typen publicera-prenumerera: Kafka innehåller en producent-API för publicering av poster till ett Kafka-ämne. Konsument-API används när det finns en prenumererar på ett ämne.

* Dataströmsbearbetning: Kafka används ofta med Apache Storm eller Spark för bearbetning av dataströmmen i realtid. Kafka 0.10.0.0 (HDInsight version 3.5 och 3.6) införde en strömmande API som gör att du kan skapa direktuppspelade lösningar utan Storm eller Spark.

* Vågrät skala: Kafka partitionerar dataströmmar mellan noderna i HDInsight-klustret. Konsumentprocesser kan associeras med enskilda partitioner för att ge belastningsutjämning när poster används.

* Leverans i ordning: Inom varje partition lagras posterna i strömmen i den ordning som de togs emot. Genom att associera en konsumentprocess per partition bearbetas posterna garanterat i ordning.

* Feltoleranta: Partitioner kan replikeras mellan noder för att ge feltolerans.

* Integrering med Azure Managed Disks: Managed Disks ger högre skala och genomströmning för diskar som används av de virtuella datorerna i HDInsight-klustret.

    Hanterade diskar är aktiverade som standard för Kafka på HDInsight. Antalet diskar som används per nod kan konfigureras när HDInsight skapas. För mer information om hanterade diskar, se [Hanterade Azure-diskar](../../virtual-machines/windows/managed-disks-overview.md).

    Se [Ökad skalbalhet med Kafta på HDInsight](apache-kafka-scalability.md) för mer information om att konfigurera hanterade diskar med Kafka på HDInsight.

## <a name="use-cases"></a>Användningsfall

* **Meddelanden**: Eftersom den stöder meddelandemönstret publicera-prenumerera används Kafka ofta som en asynkron meddelandekö.

* **Aktivitetsspårning**: Eftersom Kafka innehåller i ordningsbaserad loggning av poster kan den användas för att spåra och återskapa aktiviteter. Till exempel användaråtgärder på en webbplats eller i ett program.

* **Sammanställning**: Med hjälp av strömbearbetning kan du sammanställa information från olika strömmar för att kombinera och centralisera information till användningsdata.

* **Omvandling**: Med dataströmsbearbetning kan du kombinera och utöka data från flera inkommande avsnitt i ett eller flera utdataämnen.

## <a name="architecture"></a>Arkitektur

![Kafka-klusterkonfiguration](./media/apache-kafka-introduction/kafka-cluster.png)

Det här diagrammet visar en typisk Kafka-konfiguration som använder konsumentgrupper, partitionering och replikering för att erbjuda parallell läsning av händelser med feltolerans. Apache ZooKeeper är byggt för samtidiga och elastiska transaktioner med låg latens, eftersom det hanterar Kafka-klusters tillstånd. Kafka lagrar poster i *ämnen*. Poster produceras av *producenter*, och används av *konsumenter*. Producenter hämtar poster från asynkrona Kafka-*meddelandeköer*. Varje arbetsnod i HDInsight-klustret är en asynkron Kafka-meddelandekö. En partition skapas för varje konsument, vilket möjliggör parallell bearbetning av strömmade data. Replikering används för att sprida partitionerna mellan noder, vilket skyddar mot nodavbrott. En partition som är markerad med *(L)* är ledande för den angivna partitionen. Producenttrafik dirigeras till varje ledande nod med det tillstånd som hanteras av ZooKeeper.

> [!IMPORTANT]
> Kafka har ingen information om den underliggande maskinvaran (rack) i Azure-datacentret. För att kontrollera att partitionerna är rätt balanserade över den underliggande maskinvaran kan du läsa informationen om att [konfigurera hög tillgänglighet för data (Kafka)](apache-kafka-high-availability.md).

## <a name="next-steps"></a>Nästa steg

Använd följande länkar om du vill veta om hur du använder Apache Kafka på HDInsight:

* [Kom igång med Kafka på HDInsight](apache-kafka-get-started.md)

* [Använd MirrorMaker för att skapa en replik av Kafka på HDInsight](apache-kafka-mirroring.md)

* [Använda Apache Storm med Kafka på HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Använda Apache Spark med Kafka på HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Ansluta till Kafka via ett trådlöst Azure-nätverk](apache-kafka-connect-vpn-gateway.md)