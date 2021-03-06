---
title: Nastavení souborů cookie Proxy aplikace – Azure Active Directory | Dokumentace Microsoftu
description: Azure Active Directory (Azure AD) obsahuje přístupu a relace soubory cookie pro přístup k místním aplikacím přes Proxy aplikací. V tomto článku zjistíte, jak používat a nakonfigurujte nastavení souborů cookie.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: celested
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60fc8cb8be39b2ffc217641464a991d8d2f3b997
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674289"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>Nastavení souborů cookie pro přístup k místním aplikacím v Azure Active Directory

Azure Active Directory (Azure AD) obsahuje přístupu a relace soubory cookie pro přístup k místním aplikacím přes Proxy aplikací. Zjistěte, jak používat nastavení souborů cookie Proxy aplikací. 

## <a name="what-are-the-cookie-settings"></a>Co jsou nastavení souborů cookie?

[Proxy aplikací](application-proxy.md) používá následující nastavení souborů cookie přístupu a relace.

| Nastavení souborů cookie | Výchozí | Popis | Doporučení |
| -------------- | ------- | ----------- | --------------- |
| Používat soubory cookie HTTP-Only | **Ne** | **Ano** umožňuje Proxy aplikací, které mají být zahrnuty HTTPOnly příznak hlavičky HTTP odpovědi. Tento příznak poskytuje další bezpečnostní výhody, třeba zabraňuje klientským skriptování (CSS) kopírování nebo upravit soubory cookie.<br></br><br></br>Předtím, než jsme nepodporuje nastavení jen HTTP, Proxy aplikací zašifrované a soubory cookie přenosu přes zabezpečený kanál SSL pro ochranu před úpravy. | Použití **Ano** kvůli další bezpečnostní výhody.<br></br><br></br>Použití **ne** pro klienty nebo Uživatelští agenti, které vyžadují přístup k souboru cookie relace. Například použít **ne** pro protokol RDP nebo MTSC klienta, která se připojuje k serveru služby Brána vzdálené plochy prostřednictvím Proxy aplikací.|
| Použít zabezpečený soubor cookie | **Ne** | **Ano** umožňuje Proxy aplikací, které chcete zahrnout Secure příznak v hlavičkách odpovědi protokolu HTTP. Bezpečné soubory cookie vylepšuje zabezpečení tím, že soubory cookie přenosu přes zabezpečený kanál TLS jako je například HTTPS. To zabrání tomu, aby soubory cookie pozorovaného neoprávněnými osobami kvůli přenosu souboru cookie ve formátu prostého textu. | Použití **Ano** kvůli další bezpečnostní výhody.|
| Použít trvalý soubor cookie | **Ne** | **Ano** umožňuje Proxy aplikace a jeho ukládání souborů cookie přístup při zavření prohlížeče, nevyprší platnost. Stálost trvá až do vypršení platnosti přístupového tokenu, nebo dokud uživatel ručně neodstraní trvalé soubory cookie. | Použití **ne** kvůli bezpečnostní riziko spojené s udržováním ověřovaný.<br></br><br></br>Doporučujeme používat pouze **Ano** pro starší aplikace, které nelze sdílení souborů cookie mezi procesy. Je lepší aktualizovat vaši aplikaci obsluhování sdílení souborů cookie mezi procesy namísto použití trvalé soubory cookie. Například můžete potřebovat trvalé soubory cookie umožňuje uživatelům otevírat dokumenty Office v zobrazení Průzkumníka z webu služby SharePoint. Bez trvalé soubory cookie tato operace může selhat, pokud soubory cookie přístupu nejsou sdíleny mezi prohlížečem, Průzkumníka procesů a procesem Office. |

## <a name="set-the-cookie-settings---azure-portal"></a>Nastavení souborů cookie – nastavení webu Azure portal
Nastavení souborů cookie pomocí webu Azure portal:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). 
2. Přejděte do **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace**.
3. Vyberte aplikaci, pro kterou chcete povolit nastavení souborů cookie.
4. Klikněte na tlačítko **Proxy aplikací**.
5. V části **další nastavení**, nastavte nastavení souborů cookie na **Ano** nebo **ne**.
6. Klikněte na tlačítko **Uložit** změny. 

## <a name="view-current-cookie-settings---powershell"></a>Zobrazit aktuální nastavení souborů cookie – PowerShell

Pokud chcete zobrazit aktuální nastavení souborů cookie pro aplikace, použijte tento příkaz Powershellu:  

```PowerShell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## <a name="set-cookie-settings---powershell"></a>Nastavení souborů cookie – PowerShell

V následující příkazy Powershellu ```<ObjectId>``` je ID objektu aplikace. 

**Http-Only Cookie** 

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Secure Cookie**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Trvalé soubory cookie**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```
