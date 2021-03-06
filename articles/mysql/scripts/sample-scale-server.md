---
title: "Azure CLI-exempel för att skala en Azure-databas för MySQL-server | Microsoft Docs"
description: "Det här exempelskriptet CLI skalas Azure-databas för MySQL-server till en annan prestandanivå efter frågar mätvärdena."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/02/2017
ms.openlocfilehash: e37e706c3c12b87cc4b49315589582ae7ab8b015
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Övervaka och skala en Azure-databas för MySQL-server med Azure CLI
Det här exempelskriptet CLI skalar en Azure-databas för MySQL-server till en annan prestandanivå efter frågar mätvärdena.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt kräver i den här artikeln att du använder Azure CLI version 2.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript
I det här exempelskriptet ändrar du markerade rader om du vill anpassa admin användarnamn och lösenord. Ersätt prenumerations-ID som används i az övervakaren kommandon med din egen prenumerations-id.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Rensa distribution
Efter skriptexempel har körts kan du ta bort resursgruppen och alla resurser som är associerade med den genom att köra följande kommando:[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Skriptet förklaring
Det här skriptet använder följande kommandon. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| **Kommandot** | **Anteckningar** |
|---|---|
| [Skapa AZ grupp](/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ mysql-server](/cli/azure/mysql/server#create) | Skapar en MySQL-server som är värd för databaserna. |
| [AZ övervakaren mått lista](/cli/azure/monitor/metrics#list) | Visa en lista med måttet för resurser. |
| [ta bort grupp AZ](/cli/azure/group#delete) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg
- Läs mer om Azure CLI: [Azure CLI dokumentationen](/cli/azure/overview).
- Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för MySQL](../sample-scripts-azure-cli.md)
- Läs mer om att skala [tjänstnivåer](../concepts-service-tiers.md) och [Compute enheter och lagringsenheter](../concepts-compute-unit-and-storage.md).
