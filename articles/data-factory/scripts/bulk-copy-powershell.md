---
title: "PowerShell-skript: kopiera data i grupp med hjälp av Azure Data Factory | Microsoft Docs"
description: "Detta PowerShell-skript visar hur du använder Azure Data Factory för att kopiera data från ett dataarkiv som källa till ett dataarkiv som mål i grupp."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: spelluru
ms.openlocfilehash: c8044d70a7c793d865884223081433e49f014f7b
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>PowerShell script - kopiera flera tabeller i grupp med hjälp av Azure Data Factory

Det här exempelskriptet PowerShell kopierar data från flera tabeller i en Azure SQL database till ett Azure SQL data warehouse.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

Se [Självstudier: masskopiera](../tutorial-bulk-copy.md#prerequisites) förutsättningarna för att köra det här exemplet.

## <a name="sample-script"></a>Exempelskript

> [!IMPORTANT]
> Det här skriptet skapar JSON-filer som definierar Data Factory-enheter (länkade tjänsten dataset och pipeline) på din hårddisk i mappen c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/bulk-copy-from-sql-databse-to-sql-data-warehouse/bulk-copy-from-sql-database-to-sql-data-warehouse.ps1 "Bulk copy from Azure SQL Database => Azure SQL Data Warehouse")]

## <a name="clean-up-deployment"></a>Rensa distribution

Du kan använda följande kommando för att ta bort resursgruppen och alla resurser som är associerade med den när du har kört exempelskript:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Ta bort datafabriken från resursgruppen genom att köra följande kommando: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon: 

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Ange AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Skapa en datafabrik. |
| [Ange AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Skapar en länkad tjänst i datafabriken. En länkad tjänst länkar en datalagret eller beräkning till en data factory. |
| [Ange AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Skapar en datamängd i datafabriken. En datauppsättning representerar o för en aktivitet i en pipeline. | 
| [Ange AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Skapar en pipeline i datafabriken. En pipeline innehåller en eller flera aktiviteter som utför en viss åtgärd. I den här pipelinen kopieras en kopieringsaktiviteten data från en plats till en annan plats i ett Azure Blob Storage. |
| [Anropa AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Skapar en körning för pipeline. Med andra ord körs pipeline. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Hämtar information om körning av aktiviteten (aktivitet kör) i pipelinen. 
| [Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |
|||

## <a name="next-steps"></a>Nästa steg

Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/).

Ytterligare Azure Data Factory PowerShell skriptexempel finns i den [Azure Data Factory PowerShell-skript](../samples-powershell.md).