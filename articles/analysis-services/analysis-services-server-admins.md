---
title: "Hantera server-administratörer i Azure Analysis Services | Microsoft Docs"
description: "Lär dig hur du hanterar server-administratörer för en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 6a42baf9fd880264e4130c009f27935a4743de21
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/02/2017
---
# <a name="manage-server-administrators"></a>Hantera server-administratörer
Server-administratörer måste vara en giltig användare eller grupp i Azure Active Directory (Azure AD) för klienten som servern finns. Du kan använda **Analysis Services-administratörer** i bladet kontroll för din server i Azure-portalen eller serveregenskaper i SSMS att hantera serveradministratörer. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Lägg till server-administratörer med Azure-portalen
1. I bladet kontroll för servern klickar du på **Analysis Services-administratörer**.
2. I den  **\<servername >-Analysis Services-administratörer** bladet, klickar du på **Lägg till**.
3. I den **lägga till serveradministratörer** bladet välj användarkonton från din Azure AD eller bjuda in externa användare efter e-postadress.

    ![Server-administratörer i Azure-portalen](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Lägg till server-administratörer med SSMS
1. Högerklicka på servern > **egenskaper**.
2. I **Analysis Server-egenskaper**, klickar du på **säkerhet**.
3. Klicka på **Lägg till**, och sedan ange den e-postadressen för en användare eller grupp i din Azure AD.
   
    ![Lägg till server-administratörer i SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Nästa steg 
[Autentisering och användarbehörigheter](analysis-services-manage-users.md)  
[Hantera databasroller och användare](analysis-services-database-users.md)  
[Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md)  

