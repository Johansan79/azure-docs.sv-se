---
title: "Skapa en tilldelning av principer för att identifiera icke-kompatibla resurser i Azure-miljön | Microsoft Docs"
description: "Den här artikeln vägleder dig genom stegen för att skapa en principdefinition för att identifiera resurser som icke-kompatibla."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 85136ff2783b21472ef02aee15f8ec5844a00c12
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Skapa en tilldelning av principer för att identifiera icke-kompatibla resurser i Azure-miljön
Det första steget i att förstå efterlevnad i Azure är viktigt att veta där du stå med aktuella resurserna. Denna Snabbstart vägleder dig genom processen att skapa en tilldelning av principer för att identifiera virtuella datorer som inte använder hanterade diskar.

I slutet av den här processen kommer har du har identifierat virtuella datorer som inte använder hanterade diskar och är därför *icke-kompatibla*.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="opt-in-to-azure-policy"></a>Välja att Azure-princip

Azure princip är nu tillgänglig som förhandsversion och du behöver registrera att begära åtkomst.

1. Gå till Azure-princip på https://aka.ms/getpolicy och välj **registrera dig** i den vänstra rutan.

   ![Sök efter princip](media/assign-policy-definition/sign-up.png)

2. Delta i Azure-principen genom att välja prenumerationer i den **prenumeration** listan som du vill arbeta med. Välj sedan **registrera**.

   ![Om du vill använda Azure-princip](media/assign-policy-definition/preview-opt-in.png)

   Din förfrågan godkänns automatiskt för förhandsgranskning. Vänta i upp till 30 minuter för systemet för att bearbeta din registrering.

## <a name="create-a-policy-assignment"></a>Skapa en principtilldelning

I den här snabbstarten vi skapa en principtilldelning och tilldela den *Audit virtuella datorer utan diskar hanteras* principdefinitionen.

1. Välj **tilldelningar** till vänster på sidan för principer för Azure.
2. Välj **tilldela principen** från början av den **tilldelningar** fönstret.

   ![Tilldela en principdefinition](media/assign-policy-definition/select-assign-policy.png)

3. På den **tilldela principen** klickar du på ![princip definition knappen](media/assign-policy-definition/definitions-button.png) bredvid **princip** fältet för att öppna listan över tillgängliga definitionerna.

   ![Öppna tillgängliga principdefinitioner](media/assign-policy-definition/open-policy-definitions.png)

   Princip för Azure medföljer redan inbyggd i principdefinitioner som du kan använda. Du ser inbyggda principdefinitioner som:

   - Framtvinga taggen och dess värde
   - Tillämpa taggen och dess värde
   - Kräv SQL Server-Version 12.0

4. Sök igenom definitionerna för att hitta den *Audit virtuella datorer som inte använder hanterade diskar* definition. Klicka på principen och på **tilldela**.

   ![Hitta rätt principdefinitionen](media/assign-policy-definition/select-available-definition.png)

5. Ange en bildskärm **namn** för tilldelning av principer. I det här fallet ska vi använda *Audit virtuella datorer som inte använder hanterade diskar*. Du kan också lägga till en valfri **beskrivning**. Beskrivningen innehåller information om hur den här principtilldelning identifierar alla virtuella datorer som skapats i denna miljö som inte använder hanterade diskar.
6. Ändra prisnivån till **Standard** så att principen tillämpas med befintliga resurser.

   Det finns två prisnivåer i Azure princip – *lediga* och *Standard*. Med den kostnadsfria nivån kan du bara tvinga principer för framtida resurser, medan med Standard, du kan även tillämpa dem på befintliga resurser för att bättre förstå din kompatibilitetstillstånd. Eftersom vi finns i begränsad förhandsgranskningen vi ännu inte har startats prisnivå modellen, så du får en faktura för att välja *Standard*. Du kan läsa mer om prissättning, titta på: [priser för Azure princip](https://azure.microsoft.com/pricing/details/azure-policy/).

7. Välj den **omfång** som principen ska tillämpas på.  Ett omfång avgör vilka resurser eller gruppering av resurser hämtar principtilldelningen tillämpas på. Det kan röra sig om en prenumeration till resursgrupper.
8. Välj den prenumeration (eller resursgrupp) du tidigare har registrerat med när du har valt att Azure-principen. I det här exemplet använder vi den här prenumerationen - **Azure Analytics kapacitet Dev**, men alternativen varierar.

   ![Hitta rätt principdefinitionen](media/assign-policy-definition/assign-policy.png)

9. Välj **tilldela**.

Du är nu redo att identifiera icke-kompatibla resurser för att förstå kompatibilitetsstatusen för din miljö.

## <a name="identify-non-compliant-resources"></a>Identifiera icke-kompatibla resurser

Välj **kompatibilitet** på den vänstra rutan och söka efter principtilldelning som du skapade.

![För principefterlevnad](media/assign-policy-definition/policy-compliance.png)

Om det finns några befintliga resurser som inte är kompatibla med den nya tilldelningen, de kommer att visas under den **icke-kompatibel resurser** fliken.

Om ett villkor utvärderas i din befintliga resurser och den kommer ut true för några av dem, är resurserna markerade som ej kompatibla med principen. Här är en tabell med hur olika åtgärder som vi har tillgängliga idag fungerar med utvärderingsresultat villkor och kompatibilitetsstatusen för dina resurser.

|Resurs  |Om villkoret i principen är  |Åtgärden i principen   |Kompatibilitetsstatus  |
|-----------|---------|---------|---------|
|Finns     |True     |Neka     |Icke-kompatibel |
|Finns     |False    |Neka     |Uppfyller kraven     |
|Finns     |True     |Lägg till   |Icke-kompatibel |
|Finns     |False    |Lägg till   |Uppfyller kraven     |
|Finns     |True     |Granska    |Icke-kompatibel |
|Finns     |False    |Granska    |Icke-kompatibel |

## <a name="clean-up-resources"></a>Rensa resurser

Andra guider i den här samlingen bygger på denna Snabbstart. Om du vill fortsätta att arbeta med efterföljande självstudiekurser inte rensa upp de resurser som skapats i denna Snabbstart. Om du inte planerar att fortsätta kan du använda stegen nedan för att ta bort alla resurser som har skapats i den här snabbstarten i Azure-portalen.
1. Välj **tilldelningar** i det vänstra fönstret.
2. Sök efter tilldelningen som du nyss skapade.

   ![Ta bort en tilldelning](media/assign-policy-definition/delete-assignment.png)

3.  Välj **tar bort tilldelningen**.

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten tilldelat du en principdefinition ett scope för att se till att alla resurser i att scope är kompatibla och identifiera vilka som inte är.

Mer information om tilldelning av principer för att säkerställa att **framtida** resurser som skapas är kompatibla, även i fortsättningen vägledning för:

> [!div class="nextstepaction"]
> [Skapa och hantera principer](./create-manage-policy.md)
