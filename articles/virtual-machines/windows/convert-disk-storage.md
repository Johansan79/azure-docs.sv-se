---
title: "Konvertera Azure hanterade diskar lagring från standard till premium, och vice versa | Microsoft Docs"
description: "Så här konverterar du Azure hanterade diskar från standard till premium och vice versa med hjälp av Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 993aea9f3b6fe4dabdc6500e5cf9d49ec594f135
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/09/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Konvertera Azure hanterade diskar lagring från standard till premium, och vice versa

Hanterade diskar erbjuder två alternativ för lagring: [Premium](premium-storage.md) (SSD-baserad) och [Standard](standard-storage.md) (HDD-baserat). På så sätt kan du enkelt växla mellan de två alternativen med minimal avbrottstid baserat på dina prestandabehov. Den här funktionen är inte tillgänglig för ohanterade diskar. Men du kan enkelt [konvertera till hanterade diskar](convert-unmanaged-to-managed-disks.md) enkelt växla mellan de två alternativen.

Den här artikeln visar hur du konverterar hanterade diskar från standard till premium och vice versa med hjälp av Azure PowerShell. Om du behöver installera eller uppgradera den, se [installera och konfigurera Azure PowerShell](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Innan du börjar

* Konverteringen kräver en omstart av den virtuella datorn, så Schemalägg migrering av lagringsenheterna diskar under en befintlig underhållsperiod. 
* Om du använder ohanterade diskar först [konvertera till hanterade diskar](convert-unmanaged-to-managed-disks.md) för att växla mellan två alternativ för lagring med hjälp av den här artikeln. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Konvertera alla hanterade diskar på en virtuell dator från standard till premium, och vice versa

I följande exempel visar vi hur du växla alla diskar på en virtuell dator från standard till premium-lagring. Du använder premiumdiskar hanteras genom den virtuella datorn måste använda en [VM-storlek](sizes.md) som har stöd för premium-lagring. Det här exemplet växlar också till en storlek som har stöd för premium-lagring.

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Konverterar en hanterad disk från standard till premium, och vice versa

För din arbetsbelastning för utveckling och testning, kanske du vill ha blandning av standard och premium diskar för att minska dina kostnader. Du kan göra det genom att uppgradera till premium-lagring på diskar som kräver bättre prestanda. I följande exempel visar vi hur du växlar en enda disk av en virtuell dator från standard till premium-lagring och vice versa. Du använder premiumdiskar hanteras genom den virtuella datorn måste använda en [VM-storlek](sizes.md) som har stöd för premium-lagring. Det här exemplet växlar också till en storlek som har stöd för premium-lagring.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get the ARM resource to get name and resource group of the VM
$vmResource = Get-AzureRmResource -ResourceId $disk.diskId
$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Nästa steg

Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).

