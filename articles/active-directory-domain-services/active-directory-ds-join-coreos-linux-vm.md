---
title: 'Azure Active Directory Domain Services: Připojení virtuálního počítače s CoreOS Linux ke spravované doméně | Dokumentace Microsoftu'
description: Připojení virtuálního počítače s CoreOS Linux do Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 3948b0e1445aef5b9030e5e40f4bd4ec7ea1bf51
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175753"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Připojení virtuálního počítače s CoreOS Linux ke spravované doméně
V tomto článku se dozvíte, jak propojit virtuální počítač s Linuxem CoreOS v Azure k spravované doméně služby Azure AD Domain Services.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Před zahájením
K provádění úkolů uvedených v tomto článku, budete potřebovat:
1. Platný **předplatného Azure**.
2. **Adresář Azure AD** – buď synchronizaci s místním adresářem nebo výhradně cloudový adresář.
3. **Azure AD Domain Services** musí být povolené pro adresář Azure AD. Pokud jste neudělali, postupujte podle všechny úkoly popsané v [příručce Začínáme](active-directory-ds-getting-started.md).
4. Ujistěte se, že jste nakonfigurovali IP adres spravované domény jako servery DNS pro virtuální síť. Další informace najdete v tématu [postup aktualizace nastavení DNS pro virtuální síť Azure](active-directory-ds-getting-started-dns.md)
5. Dokončete kroky potřebné k [synchronizace hesel do spravované domény služby Azure AD Domain Services](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-coreos-linux-virtual-machine"></a>Zřízení virtuálního počítače s Linuxem CoreOS
Zřízení virtuálního počítače s CoreOS v Azure pomocí kteréhokoli z následujících metod:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Tento článek používá **CoreOS Linux (stabilní verze)** image virtuálního počítače v Azure.

> [!IMPORTANT]
> * Nasazení virtuálního počítače do **stejné virtuální síti, ve kterém jste povolili službu Azure AD Domain Services**.
> * Vyberte **jinou podsíť** než ten, ve kterém jste povolili službu Azure AD Domain Services.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Vzdálené připojení na nově zřízeného virtuálního počítač s Linuxem
Zřízení virtuálního počítače CoreOS v Azure. Další úlohou je vzdáleně připojit k virtuálnímu počítači pomocí účtu místního správce vytvořené při zřizování virtuálních počítačů.

Postupujte podle pokynů v článku [jak se přihlásit k virtuálnímu počítači s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Konfigurace souboru hostitelů na virtuálním počítači s Linuxem
V terminálu SSH upravte soubor/etc/hosts a aktualizovat IP adresu vašeho počítače a název hostitele.

```
sudo vi /etc/hosts
```

V souboru hostitele zadejte následující hodnotu:

```
127.0.0.1 contoso-coreos.contoso100.com contoso-coreos
```
"Contoso100.com" následuje název domény DNS vaší spravované domény. "contoso-coreos" je název hostitele virtuálního počítače CoreOS, ke které se připojujete ke spravované doméně.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Konfigurace služby SSSD na virtuálním počítači s Linuxem
Dále, aktualizujte v konfiguračním souboru SSSD ("/ etc/sssd/sssd.conf") tak, aby odpovídala následující ukázce:

 ```
 [sssd]
 config_file_version = 2
 services = nss, pam
 domains = CONTOSO100.COM

 [domain/CONTOSO100.COM]
 id_provider = ad
 auth_provider = ad
 chpass_provider = ad

 ldap_uri = ldap://contoso100.com
 ldap_search_base = dc=contoso100,dc=com
 ldap_schema = rfc2307bis
 ldap_sasl_mech = GSSAPI
 ldap_user_object_class = user
 ldap_group_object_class = group
 ldap_user_home_directory = unixHomeDirectory
 ldap_user_principal = userPrincipalName
 ldap_account_expire_policy = ad
 ldap_force_upper_case_realm = true
 fallback_homedir = /home/%d/%u

 krb5_server = contoso100.com
 krb5_realm = CONTOSO100.COM
 ```
Nahraďte "CONTOSO100. COM "s názvem domény DNS vaší spravované domény. Zajistěte, aby že v případě velké v souboru conf zadáte název domény.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Připojení virtuálního počítače Linux ke spravované doméně
Teď, když na virtuálním počítači s Linuxem nainstalované požadované balíčky, dalším krokem je připojení virtuálního počítače do spravované domény.

```
sudo adcli join -D CONTOSO100.COM -U bob@CONTOSO100.COM -K /etc/krb5.keytab -H contoso-coreos.contoso100.com -N coreos
```


> [!NOTE]
> **Řešení potíží:** Pokud *adcli* nemůže najít vaši spravovanou doménu:
  * Ujistěte se, že doménu je dostupný z virtuálního počítače (zkuste příkaz ping).
  * Zkontrolujte, že virtuální počítač skutečně byla nasazena do stejné virtuální síti, ve kterém je spravovaná doména k dispozici.
  * Zaškrtněte, pokud chcete zobrazit, když jste aktualizovali nastavení serveru DNS pro virtuální síť tak, aby odkazoval na řadiče domény spravované domény.
>

Spusťte službu SSSD. V terminálu SSH zadejte následující příkaz:
  ```
  sudo systemctl start sssd.service
  ```


## <a name="verify-domain-join"></a>Ověřte připojení k doméně
Ověřte, zda je počítač byl úspěšně připojen ke spravované doméně. Připojení k doméně virtuální počítač s CoreOS pomocí jiné připojení SSH. Použít účet uživatele domény a pak zaškrtněte, pokud chcete zobrazit, pokud je uživatelský účet správně přeložit.

1. Vaše SSH terminálu zadejte následující příkaz pro připojení k doméně připojený CoreOS virtuálnímu počítači pomocí SSH. Použít účet domény, který patří ke spravované doméně (například "bob@CONTOSO100.COM" v tomto případě.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-coreos.contoso100.com
    ```

2. V terminálu SSH zadejte následující příkaz zobrazíte, pokud domovský adresář byl správně inicializován.
    ```
    pwd
    ```

3. V terminálu SSH zadejte následující příkaz, pokud chcete zobrazit, pokud členství ve skupinách jsou řešeny správně.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Řešení potíží s připojení k doméně
Odkazovat [připojení k doméně Poradce při potížích s](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshoot-joining-a-domain) článku.

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Připojte se k virtuálnímu počítači s Windows serverem do spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Jak se přihlásit k virtuálnímu počítači s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
