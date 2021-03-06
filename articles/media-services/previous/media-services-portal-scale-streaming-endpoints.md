---
title: Škálování streamování koncové body pomocí webu Azure portal | Dokumentace Microsoftu
description: Tento kurz vás provede kroky pro škálování koncových bodů streamování pomocí webu Azure portal.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: b7abf7601dda8680ac439b94bfd2a6b9f995f0f7
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000775"
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Škálování koncových bodů streamování pomocí webu Azure Portal
## <a name="overview"></a>Přehled

> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Koncové body streamování **Premium** jsou vhodné pro pokročilé úlohy a poskytují vyhrazenou a škálovatelnou kapacitu šířky pásma. Zákazníci, kteří mají koncový bod streamování **Premium**, ve výchozím nastavení získají jednu jednotku streamování (SU). Koncový bod streamování je možné škálovat přidáním jednotek streamování. Každá jednotka streamování poskytuje aplikaci další kapacitu šířky pásma. Další informace o datových proudů typy koncových bodů a konfigurace CDN, najdete v článku [přehled koncových bodů streamování](media-services-streaming-endpoints-overview.md) tématu.
 
Toto téma ukazuje, jak škálovat koncový bod streamování.

Podrobné informace o cenách najdete v článku [Ceny služby Azure Media Services](https://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Škálování koncových bodů streamování

Chcete-li změnit počet jednotek streamování, postupujte takto:

1. Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.
2. V **nastavení** okně **koncové body streamování**.
3. Klikněte na koncový bod streamování, kterou chcete škálovat. 

    > [!NOTE] 
    > Je možné škálovat pouze **Premium** koncové body streamování.

4. Přesuňte posuvník k určení počtu jednotek streamování.

    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Další postup
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

