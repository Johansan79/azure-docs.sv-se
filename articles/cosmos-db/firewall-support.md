---
title: "Åtkomstkontroll för Azure DB Cosmos-brandväggsstöd & IP | Microsoft Docs"
description: "Lär dig hur du använder IP-principer för åtkomstkontroll för brandväggsstöd för på Azure Cosmos DB databasen konton."
keywords: "IP-åtkomstkontroll, brandväggsstöd"
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: ankshah
ms.openlocfilehash: 1ceaa834ff68d5dca4abce561f9185e89af582af
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/20/2017
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure DB Cosmos-brandväggsstöd
Om du vill skydda data som lagras i Azure DB som Cosmos-databaskonto har Azure Cosmos DB fanns stöd för en hemlighet baserat [auktoriseringsmodellen](https://msdn.microsoft.com/library/azure/dn783368.aspx) som använder en stark hashbaserad meddelandeautentiseringskod (HMAC). Förutom den hemliga baserat auktoriseringsmodellen stöder nu Azure Cosmos DB drivs IP-baserade åtkomstkontroller för inkommande brandväggsstöd-princip. Den här modellen liknar brandväggsregler vid traditionella system och ger en extra nivå av säkerhet till databaskontot Azure Cosmos DB. Med den här modellen kan du nu konfigurera en Azure Cosmos DB konto om du vill att endast nås från en godkänd uppsättning datorer och/eller molntjänster. Åtkomst till Azure Cosmos DB resurser från dessa godkända uppsättningar av datorer och tjänster kräver fortfarande anroparen presentera en giltig auktoriserings-token.

## <a name="ip-access-control-overview"></a>IP-åtkomstkontroll: översikt
Ett konto för Azure DB som Cosmos-databasen är tillgänglig från offentliga internet som standard så länge begäran åtföljs av en giltig auktoriserings-token. Om du vill konfigurera IP-policy-baserad åtkomstkontroll, måste användaren ange en uppsättning IP-adresser och IP-adressintervall i CIDR-formuläret ska ingå som listan över tillåtna klientens IP-adresser för en viss databas-konto. När den här konfigurationen används blockeras alla begäranden från datorer utanför den här listan över tillåtna av servern.  Anslutningen processchema för IP-baserad åtkomstkontroll beskrivs i följande diagram:

![Diagram över anslutningsprocessen för IP-baserad åtkomstkontroll](./media/firewall-support/firewall-support-flow.png)

## <a name="connections-from-cloud-services"></a>Anslutningar från molntjänster
I Azure är cloud services ett vanligt sätt som värd för tjänsten mellannivålogik med hjälp av Azure Cosmos DB. Om du vill aktivera åtkomst till ett konto för Azure DB som Cosmos-databasen från en molnbaserad tjänst, offentliga IP-adressen för Molntjänsten måste läggas till listan över tillåtna IP-adresser som är associerade med din Azure Cosmos DB databaskonto av [konfigurera IP-åtkomst Kontrollera principen](#configure-ip-policy).  Detta säkerställer att alla rollinstanser av molntjänster har åtkomst till ditt konto för Azure DB som Cosmos-databasen. Du kan hämta IP-adresserna för dina molntjänster i Azure-portalen som visas i följande skärmbild:

![Skärmbild som visar den offentliga IP-adressen för en molnbaserad tjänst som visas i Azure-portalen](./media/firewall-support/public-ip-addresses.png)

När du skalar upp din molntjänst genom att lägga till ytterligare rollen instanser har de nya instanserna automatiskt åtkomst till Azure Cosmos DB databaskontot eftersom de ingår i samma molntjänst.

## <a name="connections-from-virtual-machines"></a>Anslutningar från virtuella datorer
[Virtuella datorer](https://azure.microsoft.com/services/virtual-machines/) eller [skalningsuppsättningar i virtuella](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kan också användas som värd för mellannivån tjänster med hjälp av Azure Cosmos DB.  Om du vill konfigurera Azure DB som Cosmos-databaskonto för att tillåta åtkomst från virtuella datorer, offentliga IP-adresser för virtuell dator och/eller virtuella måste skaluppsättning konfigureras som en av de tillåtna IP-adresserna för dina Azure Cosmos DB databaskonto genom [konfigurera de IP-principer för åtkomstkontroll](#configure-ip-policy). Du kan hämta IP-adresser för virtuella datorer i Azure-portalen som visas i följande skärmbild.

![Skärmbild som visar en offentlig IP-adress för en virtuell dator visas i Azure-portalen](./media/firewall-support/public-ip-addresses-dns.png)

När du lägger till ytterligare virtuella datorinstanser i gruppen finns de automatiskt åtkomst till ditt konto för Azure DB som Cosmos-databasen.

## <a name="connections-from-the-internet"></a>Anslutningar från internet
När du använder ett konto för Azure Cosmos-DB-databas från en dator på internet kan läggas klientens IP-adress eller IP-adressintervall för datorn till listan över tillåtna IP-adress för databaskontot Azure Cosmos DB. 

## <a name="connections-from-azure-paas-service"></a>Anslutningar från Azure PaaS-tjänsten 
Azure Functions används i Azure, PaaS-tjänster som Azure Stream analytics, tillsammans med Azure Cosmos DB. Om du vill aktivera åtkomst till Azure Cosmos DB databaskonto från dessa typer av tjänster vars IP-adress inte är tillgänglig, IP-adress 0.0.0.0 måste läggas till listan över tillåtna IP-adresser som är kopplat till din Azure Cosmos DB databaskonto genom [konfigurerar de IP-principer för åtkomstkontroll](#configure-ip-policy).  Detta säkerställer att Azure PaaS-tjänster har åtkomst till en Azure DB som Cosmos-konto som har den här regeln. 

 ## <a id="configure-ip-policy"></a>Konfigurera de IP-principer för åtkomstkontroll
De IP-principer för åtkomstkontroll kan anges i Azure-portalen eller programmässigt via [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), eller [REST API](/rest/api/documentdb/) genom att uppdatera `ipRangeFilter`egenskapen. IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg. Exempel: ”13.91.6.132,13.91.6.1/24”. När du uppdaterar din databaskonto via dessa metoder, måste du fylla i alla egenskaper för att förhindra återställs till standardinställningarna.

> [!NOTE]
> Genom att aktivera en IP-principer för åtkomstkontroll för din Azure Cosmos DB databaskonto tillåtna all åtkomst till din Azure Cosmos DB databaskonto från datorer utanför den konfigurerade listan över IP-adressintervall blockeras. Tack vare den här modellen blockeras surfning plan för åtgärden från portalen också för att kontrollera integriteten för åtkomstkontroll.

För att förenkla utvecklingen hjälper Azure-portalen dig att identifiera och lägga till IP-Adressen för klientdatorn i listan över tillåtna, så att appar som körs på datorn kan komma åt Azure Cosmos DB-konto. Klientens IP-adress här har identifierats som visas av portalen. Det kan vara klientens IP-adress för datorn, men det kan också vara IP-adressen för din nätverks-gateway. Glöm inte att ta bort det innan du fortsätter till produktionen.

Om du vill ange de IP-principer för åtkomstkontroll i Azure portal, navigerar du till bladet Azure DB som Cosmos-konto, klickar du på **brandväggen** i navigeringsmenyn, klicka på **på** 

![Skärmbild som visar hur du öppnar bladet brandväggen i Azure-portalen](./media/firewall-support/azure-portal-firewall.png)

Ange om Azure-portalen kan få åtkomst till kontot och lägga till andra adresser och intervall som är lämpligt, och klicka sedan i den nya rutan **spara**.  

> [!NOTE]
> När du aktiverar en IP-principer för åtkomstkontroll som du behöver lägga till IP-adressen för Azure-portalen att upprätthålla åtkomsten. Portalen IP-adresser är:
> |Region|IP-adress|
> |------|----------|
> |Alla regioner utom de som anges nedan|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
> |Tyskland|51.4.229.218|
> |Kina|139.217.8.252|
> |USA-förvaltad region|52.244.48.71|
>

![Skärmbild som visar en hur du konfigurerar inställningar för brandväggen i Azure-portalen](./media/firewall-support/azure-portal-firewall-configure.png)

## <a name="troubleshooting-the-ip-access-control-policy"></a>Felsökning av de IP-principer för åtkomstkontroll
### <a name="portal-operations"></a>Portalen åtgärder
Genom att aktivera en IP-principer för åtkomstkontroll för din Azure Cosmos DB databaskonto tillåtna all åtkomst till din Azure Cosmos DB databaskonto från datorer utanför den konfigurerade listan över IP-adressintervall blockeras. Om du vill aktivera portal plan dataåtgärder som surfning samlingar och fråga dokument måste du därför uttryckligen tillåta Azure portal med hjälp av den **brandväggen** blad i portalen. 

![Skärmbild som visar en så att aktivera åtkomst till Azure-portalen](./media/firewall-support/azure-portal-access-firewall.png)

### <a name="sdk--rest-api"></a>SDK & Rest API
För säkerhetsskäl åtkomst via SDK eller REST-API från datorer som inte finns på listan över tillåtna returneras ett allmänt 404 gick inte att hitta svar utan ytterligare information. Kontrollera IP-Adressen tillåten lista som konfigurerats för ditt konto för Azure DB som Cosmos-databasen för att säkerställa rätt principkonfigurationen tillämpas på ditt konto för Azure DB som Cosmos-databasen.

## <a name="next-steps"></a>Nästa steg
Information om nätverk prestandatips finns [prestandatips](performance-tips.md).

