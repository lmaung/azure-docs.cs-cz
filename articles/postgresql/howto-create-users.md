---
title: Vytvoření uživatelů ve službě Azure Database pro PostgreSQL server
description: Tento článek popisuje, jak můžete vytvořit nové uživatelské účty pro interakci s serveru Azure Database for PostgreSQL.
author: jasonwhowell
ms.author: jasonh
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/16/2018
ms.openlocfilehash: 8b1bf6f1eccefb9235751c9e113c90566dfdff79
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540821"
---
# <a name="create-users-in-azure-database-for-postgresql-server"></a>Vytvoření uživatelů ve službě Azure Database pro PostgreSQL server 
Tento článek popisuje, jak vytvořit uživatele v serveru Azure Database for PostgreSQL.

## <a name="the-server-admin-account"></a>Účet správce serveru
Při prvním vytvoření Azure Database for PostgreSQL, můžete zadat uživatelské jméno správce serveru a heslo. Další informace, můžete postupovat podle [rychlý Start](quickstart-create-server-database-portal.md) zobrazíte podrobné přístup. Uživatelské jméno správce serveru je vlastní název, můžete vyhledat uživatelské jméno správce zvolený server z webu Azure portal.

Serveru Azure Database for PostgreSQL se vytvoří s 3 výchozí role definované. Tyto role můžete zobrazit spuštěním příkazu: `SELECT rolname FROM pg_roles;`
- azure_pg_admin
- azure_superuser
- uživatel s rolí správce vašeho serveru

Uživatel s rolí správce vašeho serveru je členem azure_pg_admin role. Účet správce serveru ale není součástí azure_superuser role. Protože tato služba je spravovaná služba PaaS, jenom Microsoft je součástí role superuživatele. 

Modul PostgreSQL využívá oprávnění k řízení přístupu k databázové objekty, jak je popsáno v [dokumentaci k produktu PostgreSQL](https://www.postgresql.org/docs/current/static/sql-createrole.html). Ve službě Azure Database for PostgreSQL je uživatel správce serveru poskytuje tato oprávnění: PŘIHLÁŠENÍ, NOSUPERUSER, DĚDÍ CREATEDB, CREATEROLE, NOREPLICATION

Uživatelský účet správce serveru slouží k vytvoření dalších uživatelů a udělení těchto uživatelů do azure_pg_admin role. Kromě toho účet správce serveru slouží k vytvoření méně privilegovaných uživatelů a rolí, které mají přístup k jednotlivým databázím a schémata.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>Jak vytvořit další uživatele ve službě Azure Database for PostgreSQL
1. Získejte název uživatelské informace a Správce připojení.
   Pokud se chcete připojit ke svému databázovému serveru, potřebujete úplný název serveru a přihlašovací údaje správce. Můžete snadno vyhledat název serveru a přihlašovací údaje ze serveru **přehled** stránky nebo **vlastnosti** stránky na webu Azure Portal. 

2. Použijte účet správce a heslo pro připojení k vašemu databázovému serveru. Použijte váš upřednostňovaný klientském nástroji, jako pgAdmin nebo psql.
   Pokud si nejste jistí, jak se připojit, přečtěte si téma [rychlý start](./quickstart-create-server-database-portal.md)

3. Upravit a spuštěním následujícího kódu SQL. Nahraďte nové uživatelské jméno pro hodnotu zástupného symbolu < new_user > a heslo zástupný symbol nahraďte silné heslo. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>Postup pro vytváření uživatelů databáze ve službě Azure Database for PostgreSQL

1. Získejte název uživatelské informace a Správce připojení.
   Pokud se chcete připojit ke svému databázovému serveru, potřebujete úplný název serveru a přihlašovací údaje správce. Můžete snadno vyhledat název serveru a přihlašovací údaje ze serveru **přehled** stránky nebo **vlastnosti** stránky na webu Azure Portal. 

2. Použijte účet správce a heslo pro připojení k vašemu databázovému serveru. Použijte váš upřednostňovaný klientském nástroji, jako pgAdmin nebo psql.

3. Upravit a spuštěním následujícího kódu SQL. Nahraďte hodnotu zástupného symbolu `<db_user>` zamýšlený novým uživatelským jménem a zástupnou hodnotu `<newdb>` nahraďte vlastním názvem databáze. Nahraďte zástupný symbol heslo silné heslo. 

   Tato syntaxe kódu sql vytvoří novou databázi s názvem testdb, pro účely tohoto příkladu. Vytvoří nového uživatele ve službě PostgreSQL a připojení uděluje oprávnění pro novou databázi pro tohoto uživatele. 

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE <newdb> TO <db_user>;
   ```

4. Pomocí účtu správce, budete muset udělit další oprávnění k zabezpečení objektů v databázi. Odkazovat [PostgreSQL dokumentaci](https://www.postgresql.org/docs/current/static/ddl-priv.html) další podrobnosti o databázové role a oprávnění. Příklad: 
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Přihlaste se k serveru zadáním určené databázi pomocí nové uživatelské jméno a heslo. Tento příklad ukazuje příkazového řádku psql. Pomocí tohoto příkazu budete vyzváni k zadání hesla pro uživatelské jméno. Nahraďte vlastní název serveru, název databáze a uživatelské jméno.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Další postup
Otevření brány firewall pro IP adresy počítačů novým uživatelům povolit jim připojení: [Vytvoření a správě Azure Database for postgresql – pravidla brány firewall pomocí webu Azure portal](howto-manage-firewall-using-portal.md) nebo [rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md).

Další informace týkající se správy uživatelských účtů, najdete v dokumentaci k produktu PostgreSQL [databázové role a oprávnění](https://www.postgresql.org/docs/current/static/user-manag.html), [udělení syntaxe](https://www.postgresql.org/docs/current/static/sql-grant.html), a [oprávnění](https://www.postgresql.org/docs/current/static/ddl-priv.html).
