---
title: "Hantera Underhåll meddelanden för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Visa aviseringar för underhåll för Linux virtuella datorer som körs i Azure och starta självbetjäning underhåll."
services: virtual-machines-linux
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: zivr
ms.openlocfilehash: b31955e19883f9fe2e7ed6cf7f5076eaf52577c0
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/09/2017
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Hantering av planerat underhåll meddelanden för Linux virtuella datorer

Azure utför regelbundet uppdateringar för att förbättra tillförlitligheten, prestanda och säkerhet för infrastrukturen för värd för virtuella datorer. Uppdateringar är ändringar som korrigering värdmiljön eller uppgradera och inaktivering av maskinvara. Flesta av de här uppdateringarna utförs utan någon inverkan på de virtuella datorerna. Det finns dock fall där uppdateringar har konsekvenser:

- Om underhåll som inte kräver en omstart, använder Azure migrering på plats för att pausa den virtuella datorn när värden har uppdaterats.

- Om en omstart krävs för underhåll, får du ett meddelande om vid underhåll är planerade. I dessa fall måste ges ett tidsfönster där du kan starta underhållet själv när det passar dig.


Planerat underhåll som kräver en omstart kommer i waves. Varje wave har ett annat område (regioner).

- En våg börjar med ett meddelande till kunder. Som standard skickas meddelandet till prenumerationen ägare och Medägare. Du kan lägga till fler meddelanden alternativ som e-post, SMS och Webhooks, och mottagare av meddelanden. 
- Ett fönster med självbetjäning ställs strax efter meddelandet. Du hittar vilka av dina virtuella datorer ingår i den här wave och starta underhållsläge med proaktiv Omdistributionen under den här perioden. 
- Följande fönstret självbetjäning börjar schemalagda underhållsperiod. Just nu är Azure schemalägger och gäller det nödvändiga underhållet för den virtuella datorn.  

Målet med att ha två windows är att ge tillräckligt med tid för att starta underhåll och starta om den virtuella datorn samtidigt som du vet när Azure startar automatiskt underhåll.

Du kan använda Azure CLI, PowerShell, REST-API och Azure-portalen för att fråga efter underhållsperioderna för dina virtuella datorer och starta självbetjäning underhåll.

 > [!NOTE]
 > Om du försöker starta underhåll och misslyckas Azure markerar den virtuella datorn som **hoppas över** och det är inte starta om datorn under den schemalagda underhållsperioden. I stället kontaktas du i ett senare tillfälle med ett nytt schema. 

## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>Virtuella datorer med hjälp av CLI Underhåll schemalagt för att hitta

Planerat underhållsinformation kan visas med hjälp av [get till azure vm-instansvyn](/cli/azure/vm?view=azure-cli-latest#az_vm_get_instance_view).
 
Underhållsinformation returneras bara om det är planerat underhåll. Om det finns inget Underhåll schemalagda som påverkar den virtuella datorn, kommer kommandot inte att returnerar underhållsinformation om. 

```azure-cli
az vm get-instance-view  - g rgName  -n vmName 
```

Följande värden returneras under MaintenanceRedeployStatus: 

| Värde | Beskrivning   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Anger om du kan starta Underhåll på den virtuella datorn just nu ||
| PreMaintenanceWindowStartTime         | I början av självbetjäning underhållsfönstret när du kan initiera Underhåll på den virtuella datorn ||
| PreMaintenanceWindowEndTime           | Slutet av självbetjäning underhållsfönstret när du kan initiera Underhåll på den virtuella datorn ||
| MaintenanceWindowStartTime            | I början av det schemalagda underhållsfönstret när du kan initiera Underhåll på den virtuella datorn ||
| MaintenanceWindowEndTime              | Slutet av den schemalagda underhållsperioden när du kan initiera Underhåll på den virtuella datorn ||
| LastOperationResultCode               | Resultatet av det senaste försöket att starta Underhåll på den virtuella datorn ||




## <a name="start-maintenance-on-your-vm-using-cli"></a>Starta Underhåll på den virtuella datorn med hjälp av CLI

Följande anrop initierar Underhåll på en virtuell dator om `IsCustomerInitiatedMaintenanceAllowed` har angetts till true.

```azure-cli
az vm perform-maintenance rgName vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="classic-deployments"></a>Klassiska distributioner

Om du fortfarande har äldre virtuella datorer som har kan distribuerats med den klassiska distributionsmodellen du använda CLI 1.0 frågan för virtuella datorer och initiera underhåll.

Kontrollera att du är i rätt läge för att arbeta med klassiska Virtuella genom att skriva:

```
azure config mode asm
```

Att hämta status för underhåll av en virtuell dator med namnet *myVM*, typ:

```
azure vm show myVM 
``` 

Om du vill starta Underhåll på du klassiska virtuella datorn med namnet *myVM* i den *myService* service och *myDeployment* distribution, typ:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```


## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR


**F: Varför behöver du starta om min virtuella datorer nu?**

**S:** Även merparten av uppdateringar och uppgraderingar till Windows Azure-plattformen inte påverkar tillgängligheten för virtuell dator, så finns det fall där vi inte kan undvika startas om virtuella datorer som finns i Azure. Vi har ackumulerade flera ändringar som kräver att starta om våra servrar vilket innebär att virtuella datorer startas om.

**F: om jag följer rekommendationer för hög tillgänglighet med hjälp av en Tillgänglighetsuppsättning kan jag säker?**

**S:**virtuella datorer som distribueras i en tillgänglighetsgrupp eller skalningsuppsättningar i virtuella datorer har begreppet av uppdateringen domäner (UD). När du utför underhåll, Azure godkänner UD begränsningen och kommer inte att starta om virtuella datorer från olika UD (inom samma tillgänglighetsuppsättning).  Azure väntar även minst 30 minuter innan du flyttar till nästa grupp av virtuella datorer. 

Mer information om hög tillgänglighet finns i Hantera tillgängligheten för Windows-datorer i Azure eller hantera tillgängligheten för Linux virtuella datorer i Azure.

**F: Jag har katastrofåterställning som angetts i en annan region. Kan jag säker?**

**S:** varje Azure-region paras ihop med en annan region inom samma geografiska (till exempel USA, Europa eller Asien). Planerade Azure-uppdateringar distribueras en i taget till kopplade regioner för att minimera nedtid och risk för programavbrott. Under planerat underhåll schemalägga Azure ett liknande fönster för användarna om att starta underhåll, men det schemalagda underhållsfönstret kommer att skilja mellan parad regioner.  

Mer information om Azure-regioner avse regioner och tillgänglighet för virtuella datorer i Azure.  Du kan se en fullständig lista över regionala par här.

**F: hur jag Håll dig informerad om planerat underhåll?**

**S:** ett planerat underhåll wave startas genom att ange ett schema till en eller flera Azure-regioner. Strax efter ett e-postmeddelande skickas till prenumerationen ägare (en e-post per prenumeration). Ytterligare kanaler och mottagare för denna avisering kan konfigureras med hjälp av aktiviteten loggen aviseringar. Om du distribuerar en virtuell dator till en region där planerat underhåll redan schemalagts kommer inte att ta emot meddelandet men i stället måste du kontrollera underhållstillstånd av den virtuella datorn.

**F: inte visas någon uppgift om planerat underhåll i portalen, Powershell eller CLI, fel?**

**S:** Information som rör planerat underhåll som är tillgänglig under ett planerat underhåll wave endast för de virtuella datorerna som kommer att påverkas av den. Med andra ord, om du ser inte data, kan det vara att Underhåll wave har redan slutförts (eller inte startats) eller att den virtuella datorn finns redan i en uppdaterad server.

**F: ska jag börja Underhåll på min virtuella dator?**

**S:** i allmänhet arbetsbelastningar som har distribuerats i en tjänst i molnet, tillgänglighetsuppsättning eller skaluppsättning för virtuella datorer är känsligt för planerat underhåll.  Endast en enskild uppdateringsdomän påverkas vid en given tidpunkt under planerat underhåll. Tänk på att ordningen för update-domäner som påverkas inte nödvändigtvis sker sekventiellt.

Du kanske vill starta underhåll själv i följande fall:
- Programmet körs i en enskild virtuell dator och du måste tillämpa alla Underhåll kontorstid
- Du behöver samordna samband med Underhåll som en del av din SERVICENIVÅAVTAL
- Du behöver mer än 30 minuter mellan varje VM-omstart även inom en tillgänglighet ange.
- Vill du ta bort hela programmet (flera nivåer, flera domäner för uppdatering) för att slutföra underhållet snabbare.

**F: finns det ett sätt att veta exakt när påverkas min virtuella dator?**

**S:** när schemat kan vi definierar ett tidsfönster på flera dagar. Dock är exakt Sekvenseringen av servrar (och virtuella datorer) i det här fönstret okänd. Kunder som vill veta exakt tid för sina virtuella datorer kan använda schemalagda händelser och fråga i den virtuella datorn och får ett meddelande om 10 minuter innan en VM-omstart.
  
**F: hur lång tid tar det att datorn startas om min virtuella dator?**

**S:** beroende på storleken på den virtuella datorn, omstart kan ta flera minuter. Observera att om du använder molntjänster, skala, eller anger tillgänglighetsuppsättning får du 30 minuter mellan varje grupp av virtuella datorer (UD). 

**F: Vad kommer att upplevelsen för molntjänster och skaluppsättningar för Service Fabric?**

**S:** när dessa plattformar som påverkas av planerat underhåll, kunder som använder dessa plattformar anses vara säkert anges som endast virtuella datorer i en enda uppgradera domän (UD) påverkas vid en given tidpunkt.  

**F: Jag har fått ett e-postmeddelande om hur du inaktiverar maskinvara, är det samma som planerat underhåll?**

**S:** inaktivering av maskinvaran är en planerad underhållshändelse, har vi ännu inte publicerats så användningsfallet till den nya upplevelsen.  Planerar vi för kunder att förvirrade om de tar emot två liknande e-postmeddelanden om waves för två olika planerat underhåll.

**F: visas inte någon underhållsinformation på min virtuella datorer. Vad som gick fel?**

**S:** det finns flera orsaker till varför det inte uppstår några underhållsinformation på dina virtuella datorer:
1.  Du använder en prenumeration som markerats som Microsoft internt.
2.  Din virtuella dator är inte schemalagda för underhåll. Det kan vara att Underhåll wave har slutförts, avbröts eller ändras så att dina virtuella datorer som inte längre påverkas av den.
3.  Du har inte kolumnen 'Underhåll' lagts till i din VM listvy. När vi har lagt till den här kolumnen till standardvy kan kunder som konfigurerats för att se kolumner som inte är standard måste manuellt lägga till den **Underhåll** kolumn till deras VM-listvy.

**F: den virtuella datorn är Underhåll schemalagt för en andra gång. Varför?**

**S:** det finns flera användningsområden där du kan se din virtuella dator Underhåll schemalagt för när du redan har slutfört din Underhåll Omdistributionen:
1.  Vi har avbrutits wave för underhåll och startas om den med en annan nyttolast. Det kan bero på att vi har upptäckt fel nyttolasten och vi behöver distribuera en ytterligare nyttolast.
2.  Den virtuella datorn har *service lagats* till en annan nod på grund av ett maskinvarufel
3.  Du har valt för att stoppa (frigöra) och starta om den virtuella datorn
4.  Du har **automatisk avstängning** aktiverat för den virtuella datorn


## <a name="next-steps"></a>Nästa steg

Lär dig hur du kan registrera dig för underhållshändelser från inom den virtuella datorn med hjälp av [schemalagda händelser](scheduled-events.md).