---
title: Převést Azure spravované disky úložiště úroveň ze standard na premium a naopak | Dokumentace Microsoftu
description: Jak převést Azure spravované disky úložiště z úrovně standard na premium a naopak, pomocí rozhraní příkazového řádku Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 6b78027191d72c10b20c9d09a92c82be4a9e3e05
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650797"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Převést Azure spravované disky úložiště úroveň ze standard na premium a naopak

Spravované disky nabízí čtyři [typ disku](disks-types.md) možnosti: Ultra jednotky SSD (Solid-State Drive), Premium SSD, SSD standardy a standardních pevných discích (HDD). Umožňuje snadno přepínat mezi možnostmi s minimálními výpadky na základě vašich potřeb výkonu. To není podporováno pro nespravované disky. Ale můžete snadno [převod na managed disks](convert-unmanaged-to-managed-disks.md) snadno přepínat mezi typy disků.

V tomto článku se dozvíte, jak převést spravované disky standard na premium a naopak pomocí příkazového řádku Azure. Pokud potřebujete instalaci nebo upgrade, naleznete v tématu [instalace Azure CLI](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Před zahájením

* Převod vyžaduje restartování virtuálního počítače, proto naplánujte migraci disků úložiště během už existujícího časového období údržby. 
* Pokud používáte nespravované disky, nejprve [převod na managed disks](convert-unmanaged-to-managed-disks.md) přepínat mezi možnosti úložiště pomocí tohoto článku.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Převést všechny spravované disky virtuálního počítače ze standard na premium a naopak

Následující příklad ukazuje, jak přepnout všechny disky virtuálního počítače z úrovně standard na premium storage. Pokud chcete použít spravované disky úrovně premium, musíte použít váš virtuální počítač [velikost virtuálního počítače](sizes.md) , který podporuje storage úrovně premium. Tento příklad také přepíná na hodnotu, která podporuje službu premium storage.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Převést spravovaného disku ze standard na premium a naopak

Pro úlohy pro vývoj/testování můžete mít kombinaci disků úrovně standard a premium ke snížení nákladů. Můžete ji provést díky upgradu na premium storage, jenom disky, které vyžadují vyšší výkon. Následující příklad ukazuje, jak přepnout jeden disk virtuálního počítače ze standard na premium storage a naopak. Pokud chcete použít spravované disky úrovně premium, musíte použít váš virtuální počítač [velikost virtuálního počítače](sizes.md) , který podporuje storage úrovně premium. Tento příklad také přepíná na hodnotu, která podporuje službu premium storage.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Převést spravovaného disku ze standardní pevného disku na SSD na úrovni standard a naopak

Následující příklad ukazuje, jak přepnout jeden disk virtuálního počítače ze standardního pevný disk na SSD na úrovni standard.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-managed-disks-between-standard-and-premium-in-azure-portal"></a>Převést na spravované disky mezi úrovněmi standard a premium na webu Azure portal

Můžete převést spravované disky mezi úrovněmi standard a premium na webu Azure Portal.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Vyberte virtuální počítač ze seznamu **virtuálních počítačů** na portálu.
3. Pokud není virtuální počítač zastavený, klikněte na tlačítko **Zastavit** horní části okna Přehled virtuálních počítačů a vyčkat, než virtuální počítač zastavit.
3. V okně pro virtuální počítač, vyberte **disky** z nabídky.
4. Vyberte disk, který má být převeden.
5. Vyberte **konfigurace** z nabídky.
6. Změnit **typ účtu** z **standardní HDD** k **Premium SSD**a naopak.
7. Klikněte na tlačítko **Uložit** a zavřít okno disku.

Aktualizovat typ disku je účinné okamžité. Váš virtuální počítač můžete restartovat po převodu.

## <a name="next-steps"></a>Další postup

Pořiďte si jen pro čtení virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).

