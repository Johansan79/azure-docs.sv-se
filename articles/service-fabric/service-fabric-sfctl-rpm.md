---
title: Azure Service Fabric CLI - sfctl rpm | Microsoft Docs
description: Beskriver Service Fabric CLI sfctl rpm-kommandon.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/26/2017
ms.author: ryanwi
ms.openlocfilehash: f032af4714ad458fa6ad6fb0741f689d44f4098b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-rpm"></a>sfctl rpm
Fråga efter och skicka kommandon till reparera manager-tjänsten.

## <a name="commands"></a>Kommandon
|Kommando|Beskrivning|
| --- | --- |
|    Godkänn kraft| Tvingar godkännande av angivna reparationsuppgiften.|
|    ta bort       | Tar bort en slutförd reparationsuppgiften.|
|    lista         | Hämtar en lista över reparera aktiviteter som matchar de angivna filtren.|

## <a name="sfctl-rpm-delete"></a>ta bort sfctl rpm
Tar bort en slutförd reparationsuppgiften.

Detta API stöder Service Fabric-plattform. Det är inte avsedd att användas direkt från din kod. 

### <a name="arguments"></a>Argument
|Argumentet|Beskrivning|
| --- | --- |
|    --aktivitet-id [krävs]| ID för slutförda reparationsuppgiften som ska tas bort.|
|    --version           | Det aktuella versionsnumret för reparationsuppgiften. Om inte är noll, sedan lyckas begäran endast om det här värdet matchar den faktiska aktuella versionen av reparationsuppgiften. Om noll, utförs ingen kontroll av version.|

### <a name="global-arguments"></a>Globala argument
|Argumentet|Beskrivning|
| --- | --- |
|    – Felsökning             | Öka loggning detaljnivå om du vill visa alla debug-loggar.|
|    --hjälp -h           | Visa den här hjälpmeddelandet och avsluta.|
|    --utdata -o         | Format för utdata.  Tillåtna värden: json jsonc, tabell, TVs.  Standard: json.
|    --fråga             | JMESPath frågesträngen. Se http://jmespath.org/ för mer information och exempel.|
|    -verbose           | Öka loggning detaljnivå. Använd--debug för fullständig felsökningsloggar.|


## <a name="sfctl-rpm-list"></a>sfctl rpm-lista
Hämtar en lista över reparera aktiviteter som matchar de angivna filtren.

Detta API stöder Service Fabric-plattform. Det är inte avsedd att användas direkt från din kod. 

### <a name="arguments"></a>Argument
|Argumentet|Beskrivning|
| --- | --- |
|    --utföraren-filter| Namnet på den reparera utföraren vars påstått aktiviteter som ska tas med i listan.|
|    --tillstånd-filter   | Ett logiskt eller av följande värden som anger vilken uppgift tillstånd ska inkluderas i resultatlistan. -1 - skapade - 2 - anspråk - 4 - förbereder - 8 - godkända - 16 - verkställande - 32 - återställning - 64 - slutfördes.|
|    --aktivitetsfilter-id | Det reparera uppgift ID-prefixet som ska matchas.|

### <a name="global-arguments"></a>Globala argument
|Argumentet|Beskrivning|
| --- | --- |
|    – Felsökning          | Öka loggning detaljnivå om du vill visa alla debug-loggar.|
|    --hjälp -h        | Visa den här hjälpmeddelandet och avsluta.|
|    --utdata -o      | Format för utdata.  Tillåtna värden: json jsonc, tabell, TVs.  Standard| JSON.|
|    --fråga          | JMESPath frågesträngen. Se http://jmespath.org/ för mer information och exempel.|
|    -verbose        | Öka loggning detaljnivå. Använd--debug för fullständig felsökningsloggar.|

## <a name="next-steps"></a>Nästa steg
- [Ställ in](service-fabric-cli.md) Service Fabric CLI.
- Lär dig att använda Service Fabric CLI med hjälp av den [exempel på skript](/azure/service-fabric/scripts/sfctl-upgrade-application).