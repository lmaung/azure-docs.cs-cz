---
title: Co je Azure Stack? | Dokumenty Microsoft
description: Azure Stack umožňuje provozovat služby Azure ve vašem datovém centru.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/25/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.lastreviewed: 02/25/2019
ms.openlocfilehash: 65894ccd9514bce1d429b336f8bd5e6674048e65
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820259"
---
# <a name="what-is-azure-stack"></a>Co je Azure Stack?

Microsoft Azure Stack je hybridní Cloudová platforma, která vám umožní poskytovat služby Azure ve vašem datovém centru. Tato platforma je navržen pro podporu vyvíjející obchodní požadavky. Azure Stack můžete povolit nové scénáře pro moderní aplikace, jako je například hraničních a odpojených prostředích, nebo splňovat určité požadavky na zabezpečení a dodržování předpisů.

Azure Stack se teď nabízí dvě možnosti nasazení podle svých potřeb.


## <a name="azure-stack-development-kit"></a>Vývojová sada pro Azure Stack

Microsoft [Azure Stack Development Kit (ASDK)](./asdk/asdk-what-is.md) je jedním uzlem nasazení služby Azure Stack, který můžete použít k vyhodnocení a další informace o službě Azure Stack.  Můžete také použít ASDK jako prostředí pro vývojáře k vytváření aplikací pomocí rozhraní API a nástrojů, který je konzistentní s Azure.

>[!Note]
>ASDK není určena pro použití jako v provozním prostředí.

ASDK má následující omezení:

* ASDK je přidružen jedné služby Azure Active Directory (Azure AD) nebo zprostředkovatele identity služby Active Directory Federation Services (AD FS). Můžete vytvořit více uživatelů v tomto adresáři a přiřaďte předplatná každému uživateli.
* Protože komponenty služby Azure Stack jsou nasazené na jeden hostitelský počítač, se omezené fyzické prostředky dostupné pro prostředky tenanta. Tato konfigurace není určen pro škálování nebo pro účely vyhodnocení výkonu.
* Scénáře pro sítě jsou omezené z důvodu jednom hostiteli a síťovou kartu požadavky na nasazení.

## <a name="azure-stack-integrated-systems"></a>Integrované systémy pro službu Azure Stack
Azure Stack nabízí integrované systémy Díky partnerství Microsoftu a [hardwarových partnerů](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), vytváření řešení, které nabízí cloud tempem inovace a výpočetnímu zjednodušení správy. Azure Stack se nabízí jako integrované hardwaru a softwaru system, takže máte flexibility a kontroly, které potřebujete, spolu se schopností na inovace v cloudu. Systémy ve službě Azure Stack integrované v rozsahu od 4 až 16 uzlů a společně podporuje hardwarových partnerů a Microsoft.  Integrované systémy Azure Stack slouží k vytvoření nové scénáře a nasazovat nové řešení pro vaše produkční úlohy.

## <a name="next-steps"></a>Další postup

[Klíčové funkce a koncepty](azure-stack-key-features.md)

[Azure Stack: Rozšíření Azure (pdf)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
