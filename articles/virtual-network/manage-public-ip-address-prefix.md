---
title: Vytvoření, změna nebo odstranění Azure předpony veřejné IP adresy
titlesuffix: Azure Virtual Network
description: Zjistěte, jak vytvořit, změnit nebo odstranit předponu veřejné IP adresy.
services: virtual-network
documentationcenter: na
author: anavinahar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: anavin
ms.openlocfilehash: ece6a6efa2f4424fb1c9d7f5a7e12a4e707faf45
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56649301"
---
# <a name="create-change-or-delete-a-public-ip-address-prefix"></a>Vytvoření, změna nebo odstranění předponu veřejné IP adresy

Další informace o veřejných předpona IP adresy a jak vytvořit, změnit a toku nějaký tok odstranit. Předponu veřejné IP adresy je souvislý rozsah adres na základě počtu veřejných IP adres, které zadáte. Adresy jsou přiřazené k vašemu předplatnému. Při vytváření prostředku veřejné IP adresy můžete přiřadit statickou veřejnou IP adresu z předpony a přidružte adresu virtuálních počítačů, nástroje pro vyrovnávání zatížení nebo jiným prostředkům, umožňující připojení k Internetu. Pokud nejste obeznámeni s předpony veřejných IP adres, přečtěte si téma [přehled předpony veřejných IP adres](public-ip-address-prefix.md)

## <a name="before-you-begin"></a>Před zahájením

> [!IMPORTANT]
> Předpony veřejných IP je ve verzi public preview v oblasti omezená. Je možné [zjistěte, co to znamená, že bude ve verzi preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Předpony veřejných IP je v tuto chvíli k dispozici: Střed USA – Západ, USA – Západ, USA – západ 2, střed USA, Severní Evropa, západní Evropa a jihovýchodní Asie. Aktualizovaný seznam oblastí, najdete v tématu [aktualizace Azure](https://azure.microsoft.com/updates/?product=virtual-network).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Před dokončením kroků v jakékoli části tohoto článku, proveďte následující úkoly:

- Pokud ještě nemáte účet Azure, zaregistrujte si [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud používáte portál, otevřete https://portal.azure.coma přihlaste se pomocí svého účtu Azure.
- Pokud používáte příkazy prostředí PowerShell k dokončení úkolů v tomto článku, buď spusťte příkazy [Azure Cloud Shell](https://shell.azure.com/powershell), nebo pomocí prostředí PowerShell z vašeho počítače. Azure Cloud Shell je bezplatné interaktivní prostředí, které můžete použít k provedení kroků v tomto článku. Má předinstalované obecné nástroje Azure, které jsou nakonfigurované pro použití s vaším účtem. Tento kurz vyžaduje modul Azure PowerShell verze 1.0.0 nebo novějším. Nainstalovanou verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable Az`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-az-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Connect-AzAccount` pro vytvoření připojení k Azure.
- Pokud k dokončení úkolů v tomto článku pomocí příkazů rozhraní příkazového řádku Azure (CLI), buď spusťte příkazy [Azure Cloud Shell](https://shell.azure.com/bash), nebo pomocí rozhraní příkazového řádku z vašeho počítače. Tento kurz vyžaduje použití Azure CLI verze 2.0.41 nebo novější. Nainstalovanou verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). Pokud používáte Azure CLI místně, musíte také spustit `az login` vytvořit připojení k Azure.

Účet přihlásit nebo připojit k Azure, musíte být přiřazeni k [Přispěvatel sítě](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolí nebo [vlastní roli](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) přiřazené příslušné akce uvedené v [oprávnění ](#permissions).

Předpony veřejných IP adres mají poplatek. Podrobnosti najdete v tématu [ceny](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="create-a-public-ip-address-prefix"></a>Vytvoření veřejného předpona IP adresy

1. V horní, v levém horním rohu portálu, vyberte **+ vytvořit prostředek**.
2. Zadejte *předpona veřejné ip adresy* v *Hledat na Marketplace* pole. Když **předpona veřejné IP adresy** se zobrazí ve výsledcích hledání vyberte ji.
3. V části **předpona veřejné IP adresy**vyberte **vytvořit**.
4. Zadejte nebo vyberte hodnoty pro následující nastavení v části **předpona vytvoření veřejné IP adresy**a pak vyberte **vytvořit**:

   |Nastavení|Povinné?|Podrobnosti|
   |---|---|---|
   |Předplatné|Ano|Musí existovat ve stejném [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) jako prostředek, který chcete přidružit k veřejnou IP adresu.|
   |Skupina prostředků|Ano|Může existovat ve stejné nebo různé [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) jako prostředek, který chcete přidružit k veřejnou IP adresu.|
   |Název|Ano|Název musí být jedinečný v rámci skupiny prostředků, kterou vyberete.|
   |Oblast|Ano|Musí existovat ve stejném [oblasti](https://azure.microsoft.com/regions)jako veřejné IP adresy budete přiřazovat adresy z rozsahu. Předpona je aktuálně je ve verzi preview v střed USA – Západ, USA – Západ, USA – západ 2, střed USA, Severní Evropa, západní Evropa a jihovýchodní Asie.|
   |Velikost předpony|Ano| Velikost předpony, které potřebujete. A/28 nebo 16 IP adres je výchozí nastavení.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Vytvoření az network public-ip předpona](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-create)|
|PowerShell|[New-AzPublicIpPrefix](/powershell/module/az.network/new-azpublicipprefix)|

## <a name="create-a-static-public-ip-address-from-a-prefix"></a>Vytvoření statické veřejné IP adresy z předpony
Jakmile vytvoříte předponu, je nutné vytvořit statické IP adresy z předpony. Pokud to chcete udělat, postupujte podle následujících kroků.

1. Do pole, které obsahuje text *vyhledat prostředky* v horní části stránky na webu Azure portal, zadejte *předpona veřejné ip adresy*. Když **předpony veřejných IP adres** nezobrazí ve výsledcích hledání, vyberte ji.
2. Vyberte předpona, která chcete k vytvoření veřejné IP adresy z.
3. Když se objeví v seznamu výsledků hledání, vyberte ho a klikněte na **+ přidat IP adresu** v oddílu Přehled. V případě, že se to nezobrazí, ujistěte se, že používáte správné odkaz pro verzi preview: https://aka.ms/publicipprefixportal
4. Zadejte nebo vyberte hodnoty pro následující nastavení v části **vytvoření veřejné IP adresy**. Protože předponu pro standardní SKU, protokoly IPv4 a statické, potřebujete jenom zadejte následující informace:

   |Nastavení|Povinné?|Podrobnosti|
   |---|---|---|
   |Název|Ano|Název veřejné IP adresy musí být jedinečný v rámci skupiny prostředků, kterou vyberete.|
   |Časový limit nečinnosti (minuty)|Ne|Kolik minut nechat připojení TCP nebo HTTP otevřené bez nutnosti spoléhat se na klientských počítačích k odesílání zpráv keep-alive. |
   |Popisek názvu DNS|Ne|Musí být jedinečný v rámci oblasti Azure vytvořit název v (v rámci všech předplatných a všechny zákazníky). Azure automaticky zaregistruje názvem a IP adresou ve své službě DNS, abyste se mohli připojit k prostředku s názvem. Azure připojí, jako výchozí podsíť *location.cloudapp.azure.com* (Pokud je umístění je vyberete) k názvu zadáte, chcete-li vytvořit plně kvalifikovaný název DNS. Další informace najdete v tématu [použití Azure DNS pomocí Azure veřejnou IP adresu](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|

## <a name="view-or-delete-a-prefix"></a>Zobrazit nebo odstranit předponu

1. Do pole, které obsahuje text *vyhledat prostředky* v horní části stránky na webu Azure portal, zadejte *předpona veřejné ip adresy*. Když **předpony veřejných IP adres** nezobrazí ve výsledcích hledání, vyberte ji.
2. Vyberte název veřejné předpona IP adresy, které chcete zobrazit, změnit nastavení, nebo odstranit ze seznamu.
3. Proveďte jeden z následujících možností podle toho, jestli chcete zobrazit, odstranit nebo změnit předpona veřejné IP adresy.
   - **Zobrazení**: **Přehled** část ukazuje nastavení pro veřejné předponu adresy IP, například předpona klíče.
   - **Odstranit**: Chcete-li odstranit předpona veřejné IP adresy, vyberte **odstranit** v **přehled** oddílu. Pokud adresy v rozsahu předpony jsou přidružené k veřejné IP adresy prostředků, musíte nejprve odstranit prostředky veřejné adresy IP adresy. Zobrazit [odstranit veřejnou IP adresu](virtual-network-public-ip-address.md#view-change-settings-for-or-delete-a-public-ip-address).

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ network public-ip předponu seznamu](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-list) na seznamu veřejné IP adresy [az network public-ip předponu zobrazit](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-show) zobrazíte nastavení. [az network public-ip předponu aktualizace](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-update) aktualizovat; [az network public-ip předponu odstranit](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-delete) odstranit|
|PowerShell|[Get-AzPublicIpPrefix](/powershell/module/az.network/get-azpublicipprefix) načíst objekt veřejné IP adresy a zobrazte její nastavení [Set-AzPublicIpPrefix](/powershell/module/az.network/set-azpublicipprefix) aktualizovat nastavení. [Odebrat AzPublicIpPrefix](/powershell/module/az.network/remove-azpublicipprefix) odstranit|

## <a name="permissions"></a>Oprávnění

K provádění úloh na předpony veřejných IP adres, musí mít váš účet přiřazenou k [Přispěvatel sítě](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolí nebo [vlastní](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) role, která je přiřazena příslušné akce uvedené v následující tabulce:

| Akce                                                            | Název                                                           |
| ---------                                                         | -------------                                                  |
| Microsoft.Network/publicIPPrefixes/read                           | Přečtěte si předponu veřejné IP adresy                                |
| Microsoft.Network/publicIPPrefixes/write                          | Vytvořit nebo aktualizovat předponu veřejné IP adresy                    |
| Microsoft.Network/publicIPPrefixes/delete                         | Odstranit předponu veřejné IP adresy                              |
|Microsoft.Network/publicIPPrefixes/join/action                     | Vytvoření veřejné IP adresy z předpony |

## <a name="next-steps"></a>Další postup

- Další informace o scénářích a výhody použití [veřejných předpon adres IP](public-ip-address-prefix.md)