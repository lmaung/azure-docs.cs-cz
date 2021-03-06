---
title: Vytvářet a spravovat Azure IoT Central aplikace jako odběratel CSP | Dokumentace Microsoftu
description: Jako zprostředkovatel kryptografických služeb jak vytvořit aplikaci Azure IoT Central jménem vašich zákazníků.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 10/29/2018
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 73c3c57df215a66d914f5ea75475f74eff05a1f0
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200288"
---
# <a name="as-a-csp-create-and-manage-an-azure-iot-central-application-on-behalf-of-your-customer"></a>Jako zprostředkovatel kryptografických služeb vytvářet a spravovat Azure IoT Central aplikace jménem vašich zákazníků 

Program Microsoft Cloud Solution Provider (CSP) je program Microsoft Reseller. Jeho cílem je naše distribuční partneři s programem univerzálnímu dál prodávat všem komerčním Online službám Microsoftu. Další informace o [programu Cloud Solution Provider](https://partner.microsoft.com/cloud-solution-provider).

Jako zprostředkovatel kryptografických služeb, můžete vytvářet a spravovat aplikace Microsoft Azure IoT Central jménem vašich zákazníků prostřednictvím [Microsoft Partner Center](https://partnercenter.microsoft.com/partner/home). Když Azure IoT Central aplikací jsou vytvářeny jménem zákazníky CSP, stejně jako s jinými CSP spravovaných službách Azure, CSP spravují fakturaci pro zákazníky. Poplatek za Azure IoT Central se zobrazí váš celkový účet na webu Microsoft Partner Center.

Abyste mohli začít, přihlásit se ke svému účtu na portálu pro partnery společnosti Microsoft a vyberte zákazníka, pro kterého chcete vytvořit aplikaci Azure IoT Central. V levém navigačním podokně přejděte na Správa služeb pro odběratele

![Microsoft Partner Center, názorů zákazníků](media/howto-create-application-asCSP/image1.png)

Azure IoT Central je uveden jako služba pro správu k dispozici. Klikněte na Azure IoT Central odkaz na stránku k vytvoření nové aplikace nebo spravovat existující aplikace pro tohoto zákazníka.

![K dispozici pro správu Azure IoT Central](media/howto-create-application-asCSP/image2.png)

Budete přesměrováni na stránku Azure IoT Central aplikace správce. Azure IoT Central udržuje kontext, pochází z webu Microsoft Partner Center a že jste ke správě daného zákazníka. Uvidíte toto potvrzení v záhlaví stránky Správce aplikací. Z tohoto místa můžete buď přejít do stávající aplikace, kterou jste vytvořili dříve pro tohoto zákazníka ke správě nebo vytvořte novou aplikaci pro zákazníka.

![Vytvoření správce pro poskytovatele CSP](media/howto-create-application-asCSP/image3.png)

Chcete-li vytvořit aplikaci Azure IoT Central, klikněte na tlačítko **novou aplikaci** dlaždici. Tím se načtou na stránku pro vytvoření aplikace. Musíte vyplnit všechna pole na této stránce a pak zvolte **vytvořit**. Můžete najít další informace o každé z níže uvedených polí.

![Vytvoření stránky aplikace pro zprostředkovatele kryptografických služeb](media/howto-create-application-asCSP/image4.png)

![Vytvoření stránky aplikace pro zprostředkovatele kryptografických služeb](media/howto-create-application-asCSP/image4-1.png)

## <a name="payment-plan"></a>Plán plateb

Aplikace s průběžnými platbami můžete vytvořit pouze jako odběratel CSP. K prezentaci zákazníkovi Azure IoT Central, můžete vytvořit zkušební verze aplikace samostatně. Další informace o aplikacích zkušební verze a s průběžnými platbami na [Azure IoT Central stránce s cenami](https://azure.microsoft.com/pricing/details/iot-central/).

## <a name="application-name"></a>Název aplikace

Název aplikace se zobrazí na **Správce aplikací** stránky a v každé aplikaci Azure IoT Central. Můžete zvolit libovolný název aplikace Azure IoT Central. Zvolte název, který dává smysl a ostatním uživatelům ve vaší organizaci.

## <a name="application-url"></a>Adresa URL aplikace

Adresa URL aplikace je odkaz na vaši aplikaci. Můžete uložit záložku v prohlížeči nebo sdílet s ostatními.

Pokud zadáte název vaší aplikace, adresa URL vaší aplikace se automaticky generuje. Pokud dáváte přednost, můžete vybrat jinou adresu URL pro aplikaci. Každá Azure IoT Central adresa URL musí být jedinečný v rámci Azure IoT Central. Pokud zvolíte adresa URL již byla přijata se zobrazí chybová zpráva.

## <a name="directory"></a>Adresář

Protože Azure IoT Central se má kontext, který pochází spravovat zákazník, který jste vybrali v portálu pro partnery společnosti Microsoft, se zobrazí pouze tenanta Azure Active Directory pro tohoto zákazníka v poli adresáře. 

Klient služby Azure Active Directory obsahuje identity uživatelů, pověření a dalších informací v organizaci. Více předplatných Azure lze přidružit jednoho tenanta Azure Active Directory.

Další informace najdete v tématu [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Předplatné Azure

Předplatné Azure umožňuje vytvářet instance služby Azure. Azure IoT Central automaticky zjistí Všechna předplatná Azure zákazníka, ke kterému máte přístup a zobrazí je v rozevírací seznam na **vytvořit aplikaci** stránky. Zvolte předplatné Azure a vytvořte novou aplikaci Centrální IoT Azure.

Pokud nemáte předplatné Azure, které můžete vytvořit na webu Microsoft Partner Center. Po vytvoření předplatného Azure se vraťte na stránku **Create Application** (Vytvořit aplikaci). Vaše nové předplatné se zobrazí v rozevírací nabídce **Azure Subscription** (Předplatné Azure).

Další informace najdete v tématu [předplatná Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Oblast

Vyberte oblast, ve kterém chcete vytvořit aplikaci Azure IoT Central. Obvykle měli byste vybrat oblast, které je nejblíže fyzicky svoje zařízení, abyste získali optimální výkon.

Další informace najdete v tématu [oblastí Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#azure-regions).

Zobrazí se oblasti Azure IoT Central je k dispozici na [dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/regions/services/) stránky.

> [!Note]
> Po zvolení oblasti už později nebude možné přesunout aplikaci do jiné oblasti.

## <a name="application-template"></a>Šablona aplikace

Můžete vybrat některou ze šablon dostupné aplikace pro novou aplikaci Azure IoT Central. Šablona aplikace může obsahovat předdefinované položky, jako jsou šablony zařízení a řídicí panely, které vám pomůžou začít.

| Šablona aplikace | Popis |
| -------------------- | ----------- |
| Custom application (Vlastní aplikace)   | Vytvoří prázdnou aplikaci, kterou můžete naplnit vlastními šablonami zařízení a zařízeními. |
| Sample Contoso (Ukázka Contoso)       | Vytvoří aplikaci, která obsahuje zařízení šablony pro jednoduchou připojené zařízení. Pomocí této šablony můžete začít zkoumat Azure IoT Central. |
| Sample Devkits (Ukázka Devkits)       | Vytvoří aplikaci s připravenými šablonami zařízení pro připojení zařízení MXChip nebo Raspberry Pi. Tuto šablonu použijte, pokud jste vývojář zařízení experimentovat s kódem na jedno z těchto zařízení. |

## <a name="next-steps"></a>Další postup

Teď, když jste se naučili, jak vytvořit aplikaci Azure IoT Central, propojený, je zde navrhované další krok:

> [!div class="nextstepaction"]
> [Spravovat aplikaci](howto-administer.md)