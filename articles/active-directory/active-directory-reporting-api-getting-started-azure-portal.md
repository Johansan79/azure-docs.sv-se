---
title: "Komma igång med Azure AD reporting API | Microsoft Docs"
description: "Hur du kommer igång med Azure Active Directory reporting API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c94a52b8a34100f22b627e291cb0becd3501fd55
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a>Komma igång med Azure Active Directory reporting API

Azure Active Directory innehåller en mängd olika rapporter. Data för de här rapporterna kan vara användbara för dina program, till exempel SIEM-system, gransknings- och business intelligence-verktyg. Azure AD reporting API: er ger programmässig åtkomst till data via en uppsättning REST-baserade API: er. Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.

Den här artikeln ger dig den information du behöver att komma igång med Azure AD reporting API: er.
I nästa avsnitt du hittar mer information om hur du använder revisionen och logga in API: er. 

Vanliga frågor och svar, Läs vår [vanliga frågor och svar](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq). Problem, ta [filen ett supportärende](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)

## <a name="learning-map"></a>Inlärningskarta
1. **Förbereda** -innan du kan testa din API samples, måste du slutföra de [krav för att få åtkomst till Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).
2. **Utforska** -få en första intryck reporting API:
   
   * [Använda exemplen för granskning API](active-directory-reporting-api-audit-samples.md) 
   * [Använda exemplen för rapporten inloggningsaktivitet API](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Anpassa** -skapa din egen lösning: 
   
   * [Med hjälp av audit API-referens](active-directory-reporting-api-audit-reference.md) 
   * [Med hjälp av rapporten API-referens inloggningsaktivitet](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Nästa steg
Om du är nyfiken att se alla tillgängliga Azure AD Graph API-slutpunkter kan använda den här länken: [https://graph.windows.net/tenant-name/activities/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).

