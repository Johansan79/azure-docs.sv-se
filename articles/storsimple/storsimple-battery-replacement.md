---
title: "Ersätt batteri på Microsoft Azure StorSimple-enhet | Microsoft Docs"
description: "Beskriver hur du tar bort, ersätta och underhålla batterimodulen säkerhetskopiering på StorSimple-enheten."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3c8a6654-4826-4883-aad8-75f332347c53
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: d1646d800692d93d7dfc2e9a9c48c3671c280e02
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2017
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Ersätt batterimodulen säkerhetskopiering på din StorSimple-enhet
> [!NOTE]
> Den klassiska portalen för StorSimple är föråldrad. Din StorSimple-enhetshanterare flyttas automatiskt till den nya Azure portalen enligt utfasningen schemat. Du får ett e-postmeddelande och portalmeddelandet för flyttningen. Det här dokumentet kommer också att dragits tillbaka snart. Om du vill visa versionen av den här artikeln för den nya Azure portalen, gå till [ersätta batterimodulen säkerhetskopiering på StorSimple-enheten](storsimple-8000-battery-replacement.md). Frågor om flyttningen, se [vanliga frågor och svar: flyttar till Azure-portalen](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Översikt
Primära höljet ström och kylning modul (PCM) på Microsoft Azure StorSimple-enheten har ett extra batteri pack. Paketet innehåller power så att StorSimple-enhet kan spara data om strömavbrott AC till primära höljet. Detta batteri pack kallas den *säkerhetskopiering batterimodulen*. Säkerhetskopiering batteri-modul finns bara för primära höljet i din StorSimple-enhet (EBOD höljet inte innehåller en säkerhetskopiering batteri modul). 

Den här självstudiekursen beskrivs hur du:

* Ta bort batterimodulen säkerhetskopiering 
* Installera en ny modul som reserv batteri
* Underhåll batterimodulen säkerhetskopiering

> [!IMPORTANT]
> Innan du tar bort och ersätter en reserv batteri modul, granska säkerhetsinformation i den [introduktion till ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="remove-the-backup-battery-module"></a>Ta bort batterimodulen säkerhetskopiering
Batterimodulen säkerhetskopiering för din StorSimple-enhet är en-fältutbytbar enhet. Innan det installeras i PCM ska batterimodulen lagras i dess ursprungliga förpackning. Utför följande steg för att ta bort säkerhetskopiering batteri.

#### <a name="to-remove-the-backup-battery-module"></a>Ta bort batterimodulen säkerhetskopiering
1. I den klassiska Azure-portalen går du till **enheter** > **Underhåll** > **maskinvarustatus**. Under **delade komponenter**, se status för batteriet.
2. Identifiera PCM där batteriet misslyckades. Bild 1 visar på baksidan av StorSimple-enhet.
   
    ![Bakplan av primära Höljesmoduler](./media/storsimple-battery-replacement/IC740994.png)
   
    **Bild 1** baksidan primära enhet visar PCM och controller moduler
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Styrenhet 0 |
   | 4 |Kontrollant 1 |
   
    Baserat på nummer 3 i bild 2 visas indikatorn övervakning LETT på PCM 0 som motsvarar **batteri fel** bör upplysta.
   
    ![Bakplan av enheten PCM övervakning indikator indikatorer](./media/storsimple-battery-replacement/IC740992.png)
   
    **Bild 2** tillbaka of PCM visar övervakning indikatorn indikatorer
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |AC strömavbrott |
   | 2 |Fläkt fel |
   | 3 |Batteri fel |
   | 4 |PCM OK |
   | 5 |DC strömavbrott |
   | 6 |Batteri felfri |
3. Om du vill ta bort PCM med en misslyckad batteri, följer du stegen i [ta bort en PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).
4. Med PCM tas bort, lyfter rotera modulreferens batteri uppåt som anges i följande bild och hämtar upp till ta bort batteriet.
   
    ![Ta bort batteri från PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    **Bild 3** tar bort batteriet från PCM
5. Placera modulen i-fältutbytbar enhet paketera.
6. Returnera den felaktiga enheten till Microsoft för rätt underhåll och hantering.

## <a name="install-a-new-backup-battery-module"></a>Installera en ny modul som reserv batteri
Utför följande steg för att installera modulen ersättning batteri i PCM i primära höljet av StorSimple-enheten.

#### <a name="to-install-the-battery-module"></a>Installera batterimodulen
1. Placera batterimodulen säkerhetskopiering med rätt orientering i PCM.
2. Tryck ned modulreferens batteri ända till plats kopplingen.
3. Ersätt PCM i primära höljet genom att följa riktlinjerna i [ersätta en ström och kylning modulen på StorSimple-enheten](storsimple-power-cooling-module-replacement.md).
4. När ersättningen är klar, går du till **enheter** > **Underhåll** > **maskinvarustatus** i den klassiska Azure-portalen. Kontrollera statusen för batteri och kontrollera att installationen har lyckats. Grön status anger att batteriet är felfri.

## <a name="maintain-the-backup-battery-module"></a>Underhåll batterimodulen säkerhetskopiering
I din StorSimple-enhet ger säkerhetskopiering batterimodulen power styrenheten för under ett Strömfel går förlorade. Det gör att den virtuella StorSimple-enheten att spara kritiska data innan du stänger av på ett kontrollerat sätt. Systemet kan hantera två på varandra följande förlorade händelser med två fullständigt debiteras batterierna på PCMs.

I den klassiska Azure-portalen på **maskinvarustatus** på den **Underhåll** sidan anges om inte fungerar på batteri eller närmar sig slutet av livslängd. Batteristatus anges med **batteri i PCM 0** eller **batteri i PCM 1** under **delade komponenter**. Den här sidan visas en **DEGRADERAD** tillstånd för det närmar sig slutet av livslängd, och **misslyckades** för nått end-för-livslängd. 

> [!NOTE]
> Batteriet kan rapportera **misslyckades** när enheten behöver bara att debiteras.
> 
> 

Om den **DEGRADERAD** status visas, rekommenderar vi följande vilka åtgärder:

* Systemet kan ha uppstått nyligen strömavbrott eller batterierna kan väldigt regelbundet underhåll. Se systemet efter 12 timmar innan du fortsätter.
  
  * Om tillståndet är fortfarande **DEGRADERAD** efter 12 timmar kontinuerlig anslutning till AC ström med domänkontrollanter och PCMs körs, sedan batteriet måste ersättas. Kontrollera [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) för en ersättning säkerhetskopiering batteri modul.
  * Om tillståndet blir OK efter 12 timmar, batteriet fungerar och behövs endast en avgift för underhåll.
* Om det inte har en associerad förlust av funktionalitet och PCM är påslagen och ansluten till ett eluttag, måste batteriet bytas. [Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) beställa en ersättning säkerhetskopiering batteri modul.

> [!IMPORTANT]
> Avyttra misslyckade batteri enligt nationella och regionala regler. 
> 
> 

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).

