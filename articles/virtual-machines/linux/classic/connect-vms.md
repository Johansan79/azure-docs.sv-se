---
title: "Ansluta virtuella Linux-datorer i en molnbaserad tjänst | Microsoft Docs"
description: "Ansluta Linux virtuella datorer som skapats med den klassiska distributionsmodellen till en Azure-molntjänst eller virtuella nätverk."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 2fd23055-6b34-4ef0-88a8-fc19e32fb3c9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: e222645509640b104410f87e4bcd22834c8d9ec1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Ansluta Linux virtuella datorer som skapats med den klassiska distributionsmodellen med virtuella nätverk eller moln-tjänsten
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker den klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.

Linux virtuella datorer som skapats med den klassiska distributionsmodellen placeras alltid i en molntjänst. Molntjänsten fungerar som en behållare och ger ett unikt offentliga DNS-namn, en offentlig IP-adress och en uppsättning slutpunkter för att få åtkomst till den virtuella datorn via Internet. Molntjänsten kan vara i ett virtuellt nätverk, men som är inte ett krav. Du kan också [ansluta virtuella Windows-datorer med virtuella nätverk eller molnet](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Om en molnbaserad tjänst som inte är i ett virtuellt nätverk, kallas en *fristående* Molntjänsten. De virtuella datorerna i en fristående molntjänst kommunicera med andra virtuella datorer med hjälp av andra virtuella datorer har offentliga DNS-namn och trafiken som skickas via Internet. Om en tjänst i molnet är i ett virtuellt nätverk, virtuella datorer i den molntjänst som kan kommunicera med alla andra virtuella datorer i det virtuella nätverket utan att skicka all trafik via Internet.

Om du placerar dina virtuella datorer i samma molntjänst för fristående, kan du fortfarande använda belastningsutjämning och tillgänglighetsuppsättningar. Mer information finns i [virtuella datorer för belastningsutjämning](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) och [hantera tillgängligheten för virtuella datorer](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Du kan inte sortera de virtuella datorerna på undernät eller ansluta en fristående molnbaserad tjänst till ditt lokala nätverk. Här är ett exempel:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Nästa steg
När du skapar en virtuell dator är det en bra idé att [lägger till en datadisk](attach-disk.md) så att dina tjänster och arbetsbelastningar har en plats att lagra data.
