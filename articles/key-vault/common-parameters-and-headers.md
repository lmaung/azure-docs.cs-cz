---
title: Běžné parametry a záhlaví
description: Parametry a hlavičky, které jsou společné pro všechny operace, které lze provést související s prostředky služby Key Vault.
services: key-vault
documentationcenter: ''
author: bryanla
manager: barbkess
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: bryanla
ms.openlocfilehash: 1ac0f54aa4dfdc9db4724629c4dbfe7a4982838f
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301226"
---
# <a name="common-parameters-and-headers"></a>Běžné parametry a záhlaví

Následující informace jsou společné pro všechny operace, které lze provést související s prostředky služby Key Vault:

- Nahraďte `{api-version}` s api-version v identifikátoru URI.
- Nahraďte `{subscription-id}` identifikátorem předplatného v identifikátoru URI
- Nahraďte `{resource-group-name}` s vybranou skupinou prostředků. Další informace najdete v tématu Použití skupin prostředků ke správě prostředků Azure.
- Nahraďte `{vault-name}` s názvem služby key vault v identifikátoru URI.
- Nastavte hlavičku Content-Type na application/json.
- Nastavte hlavičku autorizace webového tokenu JSON, který získáte z Azure Active Directory (AAD). Další informace najdete v tématu [ověřování Azure Resource Manageru](authentication-requests-and-responses.md) požadavky.

## <a name="common-error-response"></a>Běžné chybové odpovědi
Služba bude používat stavové kódy HTTP indikuje úspěch nebo selhání. Kromě toho selhání obsahovat odpovědi v následujícím formátu:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|Název elementu | Type | Popis |
|---|---|---|
| kód | řetězec | Typ chyby, ke které došlo.|
| zpráva | řetězec | Popis co způsobilo chybu. |



## <a name="see-also"></a>Viz také
 [Reference k REST API služby Azure Key Vault](/rest/api/keyvault/)
