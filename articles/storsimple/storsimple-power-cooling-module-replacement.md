---
title: "Ersätta en PCM på StorSimple-enheten | Microsoft Docs"
description: "Beskriver hur du tar bort och ersätter ström och kylning modul (PCM) på din StorSimple-enhet"
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 24a158cb-0b79-4908-bb5a-431e48760f6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 7031aff9d4797e99e6523a65ded7495c88aff282
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/07/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Ersätta en ström och kylning modulen på din StorSimple-enhet
> [!NOTE]
> Den klassiska portalen för StorSimple är föråldrad. Din StorSimple-enhetshanterare flyttas automatiskt till den nya Azure portalen enligt utfasningen schemat. Du får ett e-postmeddelande och portalmeddelandet för flyttningen. Det här dokumentet kommer också att dragits tillbaka snart. Om du vill visa versionen av den här artikeln för den nya Azure portalen, gå till [ersätta en ström och kylning modulen på StorSimple-enheten](storsimple-8000-power-cooling-module-replacement.md). Frågor om flyttningen, se [vanliga frågor och svar: flyttar till Azure-portalen](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Översikt
Ström och kylning modul (PCM) i Microsoft Azure StorSimple-enheten består av en strömförsörjning och kylfläktar som styrs från den primära servern och EBOD höljen. Det finns endast en modell för PCM som är certifierad för varje enhet. Primär höljet är certifierad för en 764 W PCM och EBOD höljet är certifierad för en 580 W PCM. Även om PCMs för primära höljet och EBOD höljet är olika, är ersättning proceduren identiska.

Den här självstudiekursen beskrivs hur du:

* Ta bort en PCM
* Installera en ersättning PCM

> [!IMPORTANT]
> Innan du tar bort och ersätter en PCM granska säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="before-you-replace-a-pcm"></a>Innan du ersätter en PCM
Tänk på följande viktiga problem innan du ersätter din PCM:

* Om PCM-strömförsörjning misslyckas, lämna felaktiga installerat modulen, men ta bort strömsladd. Fläkten fortsätter att ta emot ström från höljet och fortsätta att tillhandahålla rätt kylning. Om fläkten misslyckas, måste PCM bytas omedelbart.
* Innan du tar bort PCM koppla kraften från PCM genom att inaktivera den huvudsakliga växeln (om sådan finns) eller genom att ta bort strömsladd fysiskt. Detta ger en varning om att datorn att stängas power är nära förestående.
* Kontrollera att den andra PCM fungerar för fortsatt systemfunktioner innan du ersätter felaktiga PCM. En felaktig PCM måste ersättas med en fullt fungerande PCM så snart som möjligt.
* PCM modulen ersättning tar bara några minuter att slutföra, men den måste slutföras inom 10 minuter på att ta bort misslyckade PCM för att förhindra överhettning.
* Observera att ersättning 764 W PCM moduler skickas från fabriken inte innehåller batterimodulen säkerhetskopiering. Du måste ta bort batteriet från din felaktiga PCM och infoga det i modulen innan du utför ersättningen ersättning. Mer information finns i så här [ta bort och infoga en reserv batteri modul](storsimple-battery-replacement.md).

## <a name="remove-a-pcm"></a>Ta bort en PCM
Följ dessa instruktioner när du är redo att ta bort en ström och kylning modul (PCM) från Microsoft Azure StorSimple-enhet.

> [!NOTE]
> Innan du tar bort din PCM kontrollerar du att rätt ersätter (764 W för primära höljet) eller 580 W för EBOD höljet.
> 
> 

#### <a name="to-remove-a-pcm"></a>Ta bort en PCM
1. I den klassiska Azure-portalen klickar du på **enheter** > **Underhåll** > **maskinvarustatus**. Kontrollera statusen för komponenterna PCM under **delade komponenter** att identifiera vilket PCM misslyckades:
   
   * Om en strömförsörjning i PCM 0 har misslyckats, status för **strömförsörjning i PCM 0** blir röd.
   * Om en strömförsörjning i PCM 1 har misslyckats, status för **strömförsörjning i PCM 1** blir röd.
   * Om fläkten i PCM 1 har inte statusen för antingen **kylning 0 för PCM 0** eller **kylning 1 för PCM 0** blir röd.
2. Leta upp den misslyckade PCM på baksidan av primära höljet. Om du kör en 8600-modell, identifiera primära höljet genom att titta på System enhet ID-numret visas på Kontrollpanelen LED-skärmen. Standard enhets-ID som visas på det primära höljet är **00**, medan standardvärdet enhets-ID som visas på höljet EBOD är **01**. I följande diagram och tabell förklarar frontpanel LED-skärmen.
   
    ![System-ID på OPS frontpanel](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Bild 1** framför panelen på enheten  
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |Ljud av |
   | 2 |System power |
   | 3 |Modul-fel |
   | 4 |Logiska fel |
   | 5 |Enhet ID visas |
3. Övervakning indikatorn indikatorer på baksidan primära höljet kan också användas för att identifiera den felande PCM. Finns i följande diagram och tabell för att förstå hur du använder indikatorer för att hitta den felande PCM. Till exempel om det Indikator som motsvarar den **fläkt misslyckas** är upplysta, fläkten misslyckades. Likaså om den Indikator motsvarar **AC misslyckas** är upplysta, strömförsörjningen misslyckades. 
   
    ![Bakplan för enheten PCM övervakning indikatorn indikatorer](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Bild 2** tillbaka of PCM med indikator indikatorer
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |AC strömavbrott |
   | 2 |Fläkt fel |
   | 3 |Batteri fel |
   | 4 |PCM OK |
   | 5 |DC strömavbrott |
   | 6 |Batteri felfri |
4. Referera till följande diagram av StorSimple-enhet för att hitta modulen misslyckade PCM. PCM 0 är till vänster och PCM 1 till höger. Tabellen nedan beskrivs modulerna.
   
     ![Bakplan av primära Höljesmoduler](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Bild 3** baksidan av enheten med plugin-program 
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Styrenhet 0 |
   | 4 |Kontrollant 1 |
5. Stäng av den felaktiga PCM och koppla bort Ange strömsladd. Du kan nu ta bort PCM.
6. Förstå själva låset och sidan av PCM referensen mellan USB och pekfingret och klämma dem tillsammans för att öppna referensen.
   
    ![Öppna PCM referensen](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Bild 4** öppna PCM-referens
7. Krama referensen och ta bort PCM.
   
    ![Ta bort PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Bild 5** tar bort PCM

## <a name="install-a-replacement-pcm"></a>Installera en ersättning PCM
Följ instruktionerna för att installera en PCM i din StorSimple-enhet. Se till att du har infogat säkerhetskopiering batterimodulen innan du installerar ersättning PCM (gäller endast 764 W PCMs). Mer information finns i så här [ta bort och infoga en reserv batteri modul](storsimple-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>Så här installerar du en PCM
1. Kontrollera att du har rätt ersättning PCM för denna enhet. Primär höljet måste en 764 W PCM och EBOD höljet måste en 580 W PCM. Du bör inte försöka använda den 580 W PCM i primära höljet eller 764 W-PCM i EBOD hölje. Följande bild visar var du vill identifiera den här informationen på den etikett som fästs på PCM.
   
    ![Enheten PCM etikett](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Bild 6** PCM etikett
2. Sök efter skada på höljet var särskilt uppmärksam på anslutningarna. 
   
   > [!NOTE]
   > **Installera inte modulen om någon koppling stift är böjda.**
   > 
   > 
3. Dra modulen till höljet med PCM-referensen i öppet läge.
   
    ![Installerar PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Bild 7** installerar PCM
4. Stäng manuellt PCM-referensen. Du bör höra en klickning som snabbt tillkallar handtaget låstypen. 
   
   > [!NOTE]
   > För att säkerställa att utöva connector PIN-koder tug du försiktigt på referensen utan att släppa låset. Om PCM bilder ut, innebär det att låset stängdes innan kopplingarna ägnar åt.
   > 
   > 
5. Anslut strömkablarna strömkällan och PCM.
6. Skydda belastningen befrielse balar. 
7. Aktivera PCM.
8. Kontrollera att ersättningen lyckades: i den klassiska Azure-portalen för din StorSimple Manager-tjänsten går du till **enheter** > **Underhåll** > **maskinvarustatus**. Under **delade komponenter**, status för PCM vara grön. 
   
   > [!NOTE]
   > Det kan ta några minuter för ersättning PCM helt initieras.
   > 
   > 

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).

