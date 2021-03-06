---
title: "Migrera datorer efter bedömning med Azure migrera | Microsoft Docs"
description: "Beskriver hur du hämtar rekommendationer för att migrera datorer när du har kört en bedömning med tjänsten Azure migrera."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 809c6e85-0928-45e2-a7c7-6824d860e134
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 730111411a080a3134ad69fee8bfceead4d27fa4
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2017
---
# <a name="migrate-machines-after-assessment"></a>Migrera datorer efter utvärdering


[Azure migrera](migrate-overview.md) utvärderar lokala datorer för att kontrollera om de är lämplig för migrering till Azure och ger storlek och kostnad beräkningar för att köra datorn i Azure. För närvarande utvärderar Azure migrera endast datorer för migrering. Själva migreringen utförs för närvarande med andra Azure-tjänster.

Den här artikeln beskriver hur du får förslag på en Migreringsverktyget när du har kört en utvärdering av migreringen.

## <a name="migration-tool-suggestion"></a>Migrering verktyget förslag

Du måste installera agenter på lokala datorer för att få förslag om Migreringsverktyg.  

1. Skapa ett Azure migrera projekt, identifiera lokala datorer och skapa en utvärdering av migreringen. [Läs mer](tutorial-assessment-vmware.md).
2. Hämta och installera Azure migrera-agenterna på varje lokal dator som du vill se en rekommenderad migreringsmetod. [Den här proceduren](how-to-create-group-machine-dependencies.md#prepare-machines-for-dependency-mapping) att installera agenter.
2. Identifiera dina lokala datorer som är lämpliga för lift och SKIFT-migrering. Dessa är de virtuella datorer som inte kräver inga ändringar av appar som körs på dem och som kan migreras som är.
3. Vi rekommenderar för lift och SKIFT migrering med hjälp av Azure Site Recovery. [Läs mer](../site-recovery/tutorial-migrate-on-premises-to-azure.md). Alternativt kan du använda 3 part verktyg som stöder migrering till Azure.
4. Om du har lokala datorer som inte är lämpligt för en lift och SKIFT-migrering, till exempel om du vill migrera specifik app i stället för en hel virtuell dator kan använda du andra Migreringsverktyg. Till exempel föreslår vi den [Azure databasen migreringstjänst](https://azure.microsoft.com/campaigns/database-migration/) om du vill migrera lokala databaser sådana en SQL Server, MySQL och Oracle till Azure.


## <a name="review-suggested-migration-methods"></a>Granska föreslagna metoder

1. Innan du får en föreslagna migreringsmetod måste du skapa ett Azure migrera projekt, identifiera lokala datorer och köra en utvärdering av migreringen. [Läs mer](tutorial-assessment-vmware.md).
2. När bedömningen har skapats kan du visa den i projektet > **översikt** > **instrumentpanelen**. Klicka på **Readiness Assessment**

    ![Bedömning beredskap](./media/tutorial-assessment-vmware/assessment-report.png)  

3. I **förslag verktyget**, granska förslag för verktyg som du kan använda för migrering.

    ![Föreslagna verktyget](./media/tutorial-assessment-vmware/assessment-suitability.png) 




## <a name="next-steps"></a>Nästa steg

[Lär dig mer](concepts-assessment-calculation.md) om hur bedömningar beräknas.
