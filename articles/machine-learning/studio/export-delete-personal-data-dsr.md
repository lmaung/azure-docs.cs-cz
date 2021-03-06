---
title: Exportovat a odstraňovat data z Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: V rámci produktu data uložená pomocí Azure Machine Learning Studio je k dispozici pro exportu a odstranění na webu Azure portal a také prostřednictvím ověřeného rozhraní REST API. Telemetrická data, je přístupný prostřednictvím portálu Azure o ochraně osobních údajů. V tomto článku se dozvíte, jak.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 05/25/2018
ms.openlocfilehash: 49f045e8da5094f583c01c514bf92f6bf3fe1c09
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453354"
---
# <a name="export-and-delete-in-product-user-data-from-azure-machine-learning-studio"></a>Exportovat a odstranit data v rámci produktu uživatelů z Azure Machine Learning Studio

Můžete odstranit nebo exportu v rámci produktu data uložená pomocí Azure Machine Learning Studio pomocí webu Azure portal, rozhraní Studio, PowerShell a ověřil rozhraní REST API. Tento článek vysvětluje, jak. 

Telemetrická data, je přístupný prostřednictvím portálu Azure o ochraně osobních údajů. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-collect"></a>Jaké druhy uživatelská data shromažďuje nástroj Studio?

Uživatelská data se skládá z informací o uživatelích oprávnění pro přístup k telemetrii záznamy interakcí uživatelů ve službě a pracovní prostory pro tuto službu.

Existují dva druhy údajů uživatele ve službě Machine Learning Studio:
- **Data osobní účet:** ID účtu a e-mailové adresy přidružené k účtu.
- **Zákaznická data:** Data, která jste nahráli k analýze.

## <a name="studio-account-types-and-how-data-is-stored"></a>Typy účtů Studio a způsob uložení dat

Existují tři typy účtů ve službě Machine Learning Studio. Druh účtu, který máte Určuje, jak vaše data jsou uložená a jak můžete odstranit nebo ho exportovat.

- A **pracovního prostoru hosta** je anonymní, bezplatný účet. Zaregistrovat bez zadání pověření, například e-mailová adresa nebo heslo.
    -  Data se vyprazdňují po vypršení platnosti pracovního prostoru hosta.
    - Uživatelé typu Host můžete exportovat zákaznická data prostřednictvím balíčku uživatelského rozhraní, rozhraní REST API nebo Powershellu.
- A **bezplatný pracovní prostor** je bezplatný účet, přihlaste se na přihlašovací údaje - e-mailovou adresu a heslo účtu u Microsoftu.
    - Můžete exportovat a odstranit osobní a zákaznická data, která se vztahují data žádosti PSÚ (práv).
    - Můžete exportovat zákaznická data prostřednictvím balíčku uživatelského rozhraní, rozhraní REST API nebo Powershellu.
    - Bezplatné pracovní prostory bez použití účtů služby Azure AD, telemetrická data je možné exportovat pomocí portálu pro ochranu osobních údajů.
    - Když odstraníte pracovní prostor, odstraňte všechna osobní data.
- A **standardní pracovní prostor** je placený účet přistupujete s přihlašovacími údaji.
    - Můžete exportovat a odstranit osobní a zákaznická data, která jsou v souladu s žádosti PSÚ.
    - Data můžete přistupovat prostřednictvím portálu Azure o ochraně osobních údajů
    - Můžete exportovat osobní a zákaznická data prostřednictvím uživatelského rozhraní, rozhraní REST API nebo Powershellu balíčku
    - Vaše data na webu Azure Portal můžete odstranit.

## <a name="delete"></a>Odstranit data pracovního prostoru v nástroji Studio 

### <a name="delete-individual-assets"></a>Odstranit jednotlivé prostředky

Uživatelé, mohou odstranit prostředky v pracovním prostoru tak, že je vyberete a pak vyberete tlačítko Odstranit.

![Odstranění prostředků v nástroji Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Odstranit celý pracovní prostor

Uživatelé mohou také odstranit jejich celý pracovní prostor:
- Placené pracovního prostoru: Odstraňte prostřednictvím webu Azure portal.
- Bezplatný pracovní prostor: Použijte tlačítko Odstranit v **nastavení** podokně.

![Odstranit bezplatný pracovní prostor v Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-data-with-powershell"></a>Export dat Studio pomocí Powershellu
Exportovat všechny informace do formátu přenosného z Azure Machine Learning Studio pomocí příkazů pomocí Powershellu. Informace najdete v tématu [modul PowerShell pro Azure Machine Learning Studio](powershell-module.md) článku.

## <a name="next-steps"></a>Další postup

Webové služby a plán závazku fakturace dokumentaci najdete v tématu [REST API služby Azure Machine Learning Studio odkaz](https://docs.microsoft.com/rest/api/machinelearning/). 
