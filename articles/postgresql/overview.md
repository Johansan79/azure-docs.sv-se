---
title: "Översikt över Azure-databas för PostgreSQL relationsdatabstjänst | Microsoft Docs"
description: "Innehåller en översikt över Azure-databas för PostgreSQL relationsdatabstjänst."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 10/20/2017
ms.openlocfilehash: 9aa24dd10ef29c716c05cafeb84e0beb23d50628
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/07/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Vad är Azure-databas för PostgreSQL?

Azure PostgreSQL-databas är en relationsdatabastjänst i molnet för Microsoft som skapats för utvecklare som baseras på den grupp med öppen källkod [PostgreSQL](https://www.postgresql.org/) databasmotorn. Tjänsten är tillgänglig som förhandsversion. Azure-databas för PostgreSQL ger:

- Inbyggd hög tillgänglighet med utan extra kostnad.
- Förutsägbar prestanda, använder inklusiva betalning per användning priser.
- Skala direkt inom några sekunder.
- Skyddad för att skydda känsliga data i vila och under rörelse.
- Automatisk säkerhetskopiering och punkt-i-tid-återställning för upp till 35 dagar.
- Företagsklass säkerhet och efterlevnad.

Alla dessa funktioner kräver nästan inga administration och alla tillhandahålls utan extra kostnad. Dessa funktioner kan du fokusera på snabb utveckling och accelerera din tid till marknad, istället för tilldelning av värdefull tid och resurser för att hantera virtuella datorer och infrastruktur. Du kan dessutom fortsätta att utveckla ditt program med öppen källkod verktyg och plattform du önskar och leverera med hastighet och prestanda som ditt företag kräver utan att behöva veta nya kunskaper. 

Den här artikeln ger en introduktion till Azure-databas för PostgreSQL nyckelkoncept och funktioner som är relaterade till prestanda, skalbarhet och hanterbarhet. Se dessa Snabbstart för att komma igång:

- [Skapa en Azure-databas för PostgreSQL med hjälp av Azure portal](quickstart-create-server-database-portal.md)
- [Skapa en Azure-databas för PostgreSQL med hjälp av Azure CLI](quickstart-create-server-database-azure-cli.md)

För en uppsättning exempel som Azure CLI, se:

- [Azure CLI-exempel för Azure-databas för PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Justera prestanda och skalning inom några sekunder
Erbjuder två tjänstnivåer i förhandsgranskning, Azure-databasen för PostgreSQL-tjänsten: Basic och Standard. Varje nivå har olika prestanda och funktioner för att stödja lätta till tunga arbetsbelastningar. Du kan skapa din första app på en liten databas för några kronor i månaden och sedan justera skalan för att uppfylla behoven i din lösning. Dynamisk skalbarhet gör databasen besvarar transparent snabbt ändra resurskraven. Du betalar bara för resurserna du behöver och endast när det behövs. Se [prisnivåer](concepts-service-tiers.md) mer information.

## <a name="monitoring-and-alerting"></a>Övervakning och avisering
Hur du bestämmer dig för när ska man reglerar upp eller ner? Du kan använda inbyggda prestanda övervakning och avisering funktioner, kombinerat med klassificeringarna prestanda baserat på Compute-enheter. Använda dessa verktyg kan du bedöma snabbt effekten av Compute enheter skala upp eller ned baserat på din aktuella eller planerade prestandabehov. Se [aviseringar](howto-alert-on-metric.md) mer information.

## <a name="keep-your-app-and-business-running"></a>Håll igång din app och din verksamhet
Azures branschledande 99,99% tillgänglighet (inte tillgängligt i förhandsvisning) servicenivåavtal (SLA) tillhandahålls av ett globalt nätverk av Microsoft-hanterade Datacenter gör att din app igång 24/7. Med varje Azure-databas för PostgreSQL-server kan du dra nytta av inbyggd säkerhet, feltolerans och dataskydd som du annars skulle behöva köpa eller utforma, skapa och hantera. Med Azure-databas för PostgreSQL varje tjänstnivå erbjuder ett omfattande utbud av funktionerna för verksamhetskontinuitet och alternativ som du kan använda för att komma igång och fortsätta vara sätt. Du kan använda [point-in-time-återställning](howto-restore-server-portal.md) för att återställa en databas till ett tidigare skede, så långt tillbaka som 35 dagar. Dessutom kan datacenter som värd för dina databaser skulle få ett avbrott, du återställa databaser från geo-redundant kopior av nya säkerhetskopior.

## <a name="secure-your-data"></a>Skydda dina data
Tjänster för Azure-databas har en tradition av säkerhet för data att Azure-databas för PostgreSQL upprätthåller med funktioner som begränsar åtkomst, skydda data i vila och under rörelse och hjälper dig att övervaka aktiviteten. Besök den [Azure Säkerhetscenter](https://www.microsoft.com/TrustCenter/Security/default.aspx) information om azures plattformssäkerhet.

Azure-databasen för PostgreSQL-tjänsten använder kryptering för data i vila. Data, inklusive säkerhetskopior krypteras på disken (med undantag för temporära filer som skapas av motorn när du kör frågor). Tjänsten använder AES 256-bitars chiffer som ingår i Azure storage kryptering och nycklarna är hanteras av datorn. Lagringskryptering är alltid igång och kan inte inaktiveras.

Standard Azure-databasen för PostgreSQL-tjänsten har konfigurerats för att kräva [SSL anslutningssäkerhet](./concepts-ssl-connection-security.md) för data i rörelse över nätverket. Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man i mitten” genom att kryptera dataströmmen mellan servern och ditt program.  Alternativt kan inaktivera du kräver SSL för att ansluta till din databastjänst om klientprogrammet inte har stöd för SSL-anslutning.

## <a name="next-steps"></a>Nästa steg
- Finns det [sida med priser](https://azure.microsoft.com/pricing/details/postgresql/) för kostnadsjämförelse och Kostnadsberäknare.
- Kom igång genom att [skapa din första Azure-databas för PostgreSQL](./quickstart-create-server-database-portal.md).
- Skapa din första app i Python, PHP, Ruby, C\#, Java, Node.js: [anslutningsbibliotek](./concepts-connection-libraries.md)
