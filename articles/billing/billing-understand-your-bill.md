---
title: "Förstå din faktura för Azure | Microsoft Docs"
description: "Lär dig hur du läst och förstått användnings- och faktura för din Azure-prenumeration"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.openlocfilehash: 668b32e99ba9a3bdf8e8f16ac51c35c609444cd9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Förstå fakturan för Microsoft Azure
Jämför fakturan med filen detaljerad daglig användning och kostnader management rapporterna i Azure-portalen för att förstå fakturan Azure.

>[!NOTE]
>Den här artikeln gäller inte för kunder med Enterprise-avtal (EA). Om du är en EA-kund [faktura dokumentationen hittar på Enterprise Portal.](https://ea.azure.com/helpdocs/understandingYourInvoice)  

En PDF-fil på fakturan och en kopia av din detaljerad daglig användning CSV Filhämtning finns [hämta ditt Azure billing faktura och dagligen användningsdata](billing-download-azure-invoice-daily-usage-date.md). 

Detaljerade villkor och beskrivningar av din faktura och detaljerad daglig användning fil finns [Förstå villkoren på fakturan Microsoft Azure](billing-understand-your-invoice.md) och [förstå villkor i Microsoft Azure detaljerad användning](billing-understand-your-usage.md). 

Mer information om de kostnad rapporterna finns [Azure-portalen kostnadshanteringen](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started).

## <a name="charges"></a>Hur gör jag till att tillägg i min faktura är korrekta?
<div style="padding-top: 56.25%; position: relative; width: 100%;">
<iframe style="position: absolute;top: 0;left: 0;right: 0;bottom: 0;" width="100%" height="100%" src="https://www.youtube.com/embed/3YegFD769Pk" frameborder="0" allowfullscreen></iframe>
</div>

Om en avgift på fakturan som du vill ha mer information om finns det ett par olika alternativ.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Alternativ 1: Granska fakturan och jämför användning och kostnader med detaljerad användning CSV-fil

CSV-fil för detaljerad användning visar dina avgifter per faktureringsperiod och daglig användning. För detaljerad användning CSV-filen, se [hämta ditt Azure billing faktura och dagligen användningsdata](https://docs.microsoft.com/en-us/azure/billing/billing-download-azure-invoice-daily-usage-date).

Avgifter för användning visas på nivån mätaren. Följande villkor betyda samma sak i både fakturan och filen detaljerad användning. Faktureringsperioden på fakturan är till exempel motsvarar den faktureringsperiod som visas i filen detaljerad användning.

 | Fakturan (PDF) | Detaljerad användning (CSV)|
 | --- | --- |
|Faktureringscykel | Faktureringsperiod |
 |Namn |Mätarkategori |
 |Typ |Mätaren underkategori |
 |Resurs |Mätarnamn |
 |Region |Mätarregion |
 |Förbrukad |Förbrukat antal |
 |Ingår |Inkluderad mängd |
 |Faktureringsbar |Överbliven kvantitet |

Den **Användningskostnader** avsnittet för fakturan innehåller det totala värdet för varje mätaren förbrukades under din faktureringsperioden. Följande skärmbild visar exempelvis en avgift för användning för Azure-Schemaläggaren.

![Avgifterna för användning av faktura](./media/billing-understand-your-bill/1.png)

Den **instruktionen** delen av din detaljerad användning CSV visas samma kostnad. Både den *förbrukad* belopp och *värdet* matchar fakturan.

![Avgifterna för användning av CSV](./media/billing-understand-your-bill/2.png)

Om du vill se en sammanställning av denna avgift dagligen, gå till den **daglig användning** avsnitt i CSV-filen. Filtrera efter ”Scheduler” *mätaren kategori* och du kan se vilka dagar mätaren användes och hur mycket har förbrukats. Den *resurs* och *resursgruppen* informationen visas också för jämförelse. Den *förbrukad* värden ska lägga till vad som ska visas på fakturan.

![Daglig användning avsnitt i CSV-filen](./media/billing-understand-your-bill/3.png)

För att få kostnaden per dag, multiplicera den *förbrukad* belopp med den *hastighet* värdet från den **instruktionen** avsnitt.

Mer information om fakturan finns [förstå fakturan Azure](billing-understand-your-invoice.md).

Mer information om varje kolumn i CSV: N, se [förstå din Azure detaljerad användning](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Alternativ 2: Granska din faktura och jämföra med användning och kostnader i Azure-portalen

Azure-portalen kan också hjälpa dig att kontrollera dina debiteringar. Azure portal tillhandahåller kostnaden management diagram för en snabb överblick över din användning och kostnader på fakturan.

Om du vill fortsätta med exemplet ovan, finns det [prenumerationssidan](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), väljer din prenumeration och välj sedan **kostnad analysis**. Därifrån kan du ange tidsperiod och visas syntax för Azure-Schemaläggaren.

![Visa kostnad analys i Azure-portalen](./media/billing-understand-your-bill/4.png)

Se dagliga arbete och omkostnader i **kostnad historik**, klickar du på raden.

![Kostnad historikvyn i Azure-portalen](./media/billing-understand-your-bill/5.png)

Läs mer i [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](billing-getting-started.md#costs).

## <a name="external"></a>Nyheter om externa serviceavgifter?
Externa tjänster (även kallat Azure Marketplace order) tillhandahålls av oberoende tjänsteleverantörer och faktureras separat. Avgifterna visas inte på fakturan Azure. Läs mer i [förstå Azure externa avgifterna](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Hur gör jag en betalning

Om du har ställt in ett kreditkort eller en bankkort som betalningsmetoden debiteras betalningen automatiskt inom 10 dagar efter faktureringsperioden tar slut. På ditt kreditkort instruktionen raden säger **MSFT Azure**.

Om du [betala av fakturering](billing-how-to-pay-by-invoice.md), skickar du betalningen till platsen som anges längst ned på fakturan. Mer hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Hur jag för att kontrollera status för en betalning kreditkort?

[Skapa ett supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) och be om statusen för din betalning. 

## <a name="tips-for-cost-management"></a>Tips för hantering av kostnad
- Uppskatta kostnader med hjälp av den [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/) och [totalkostnad för ägarskap Kalkylatorn](https://aka.ms/azure-tco-calculator), och få de [utförlig prisinformation för varje tjänst](https://azure.microsoft.com/en-us/pricing/).
- [Ställ in fakturering aviseringar](billing-set-up-alerts.md).
- [Granska din användning och kostnader regelbundet i Azure portal](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.

Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.
