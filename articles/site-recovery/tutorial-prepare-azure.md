---
title: "Skapa resurser för användning med Azure Site Recovery | Microsoft Docs"
description: "Lär dig hur du förbereder Azure för replikering av lokala datorer med hjälp av Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 321e304f-b29e-49e4-aa64-453878490ea7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 2fa7e731a05e19697603058829f130074bb5b522
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/01/2017
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Förbered Azure-resurser för replikering av lokala datorer

Den [Azure Site Recovery](site-recovery-overview.md) tjänsten bidrar till din affärskontinuitet och haveriberedskap (BCDR) strategi genom att hålla dina appar och köra tillgängliga under planerade och oplanerade avbrott. Site Recovery samordnar haveriberedskap för lokala datorer och virtuella Azure-datorer (VM), inklusive replikering, redundans och återställning hanterar.

Den här kursen visar hur du förbereder Azure komponenter när du vill replikera lokala virtuella datorer och fysiska servrar till Azure. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Verifiera ditt konto har behörighet för replikering
> * Skapa ett Azure-lagringskonto.
> * Ange ett Azure-nätverk. När virtuella Azure-datorer skapas efter växling vid fel, är de anslutna till den här Azure-nätverk.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) innan du börjar.

## <a name="log-in-to-azure"></a>Logga in på Azure

Logga in på Azure Portal på http://portal.azure.com.

## <a name="verify-account-permissions"></a>Kontrollera behörigheter

Om du just har skapat din kostnadsfria Azure-konto är du administratör för din prenumeration. Om du inte är administratör för prenumerationen kan du arbeta med administratören att tilldela de behörigheter som du behöver. Om du vill aktivera replikering för en ny virtuell dator, måste du ha:

- Behörighet att skapa en virtuell dator i den valda resursgruppen
- Behörighet att skapa en virtuell dator i det valda virtuella nätverket
- Behörighet att skriva till det valda lagringskontot

Virtual Machine-deltagare inbyggda rollen har dessa behörigheter. Du måste också behörighet att hantera Azure Site Recovery-åtgärder. Site Recovery-deltagare rollen har alla behörigheter som krävs för att hantera Site Recovery-åtgärder i Recovery Services-valvet.

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

Bilder av replikerade datorer lagras i Azure storage. Virtuella Azure-datorer skapas från lagring när du växla över från lokal till Azure.

1. I den [Azure-portalen](https://portal.azure.com) -menyn klickar du på **ny** -> **lagring** -> **lagringskonto**.
2. Ange ett namn för lagringskontot. För dessa självstudier använder vi namnet **contosovmsacct1910171607**. Namnet måste vara unikt i Azure och vara mellan 3 och 24 tecken, witn siffror och gemener.
3. Använd den **Resource Manager** distributionsmodell.
4. Välj **generella** > **Standard**.
5. Välj standard **RA-GRS** för lagring redundans.
6. Välj den prenumeration som du vill skapa det nya lagringskontot i.
7. Ange en ny resursgrupp. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. För dessa självstudier använder vi namnet **ContosoRG**.
8. Välj den geografiska platsen för ditt lagringskonto. Lagringskontot måste vara i samma region som Recovery Services-valvet. För dessa självstudier använder vi platsen **Västeuropa**.

   ![Skapa storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Skapa lagringskontot genom att klicka på **Skapa**.

## <a name="create-a-vault"></a>Skapa ett valv

1. I Azure portal menyn klickar du på **ny** > **hantering och övervakning** >
    **säkerhetskopiering och återställning av**.
2. I **Namn** anger du ett eget namn som identifierar valvet. Den här kursen använder vi **ContosoVMVault**.
3. Välj den befintliga resursgruppen med namnet **contosoRG**.
4. Ange Azure-regionen **Västeuropa**.
5. För att snabbt komma åt valvet från instrumentpanelen, klickar du på **fäst på instrumentpanelen** > **skapa**.

   ![Nytt valv](./media/tutorial-prepare-azure/new-vault-settings.png)

   Det nya valvet visas på den **instrumentpanelen** > **alla resurser**, och på primära **Recovery Services-valv** sidan.

## <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk

När virtuella Azure-datorer skapas från lagring efter växling vid fel, är de anslutna till nätverket.

1. I den [Azure-portalen](https://portal.azure.com) -menyn klickar du på **ny** > **nätverk** >
    **virtuellt nätverk**
2. Lämna **Resource Manager** markerad som distributionsmodell. Resource Manager är prioriterade distributionsmodell.
   - Ange ett nätverksnamn. Namnet måste vara unikt i Azure-resursgrupp. Vi använder namnet **ContosoASRnet**
   - Använd den befintliga resursgruppen **contosoRG**.
   - Ange adressintervallet-10.0.0.0/24.
   - Den här kursen behöver vi ett undernät.
   - Välj den prenumeration som ska skapas i nätverket.
   - Välj platsen **Västeuropa**. Nätverket måste finnas i samma region som Recovery Services-valvet.
3. Klicka på **Skapa**.

   ![Skapa nätverk](media/tutorial-prepare-azure/create-network.png)

   Det virtuella nätverket tar några sekunder att skapa. När den har skapats kan se du den i instrumentpanelen för Azure-portalen.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Förbereda lokal VMware-infrastruktur för katastrofåterställning till Azure](tutorial-prepare-on-premises-vmware.md)
