---
title: "Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur du skapar och hanterar Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI-kommandoraden."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 66d192287eeaaaa82c0f61f8aa13b8bf7bf8cd47
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI
Brandväggsregler på servernivå kan administratörer hantera åtkomst till en Azure-databas för MySQL-Server från en specifik IP-adress eller ett intervall med IP-adresser. Med hjälp av lämplig Azure CLI-kommandona, kan du skapa, uppdatera, ta bort, lista, och visa brandväggsregler för att hantera servern. En översikt över Azure-databas för MySQL brandväggar, se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Krav
* [Installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Installera Azure Python SDK för PostgreSQL och MySQL-tjänster.
* Installera Azure CLI-komponenten för PostgreSQL och MySQL-tjänster.
* Skapa en Azure-databas för MySQL-servern.

## <a name="firewall-rule-commands"></a>Brandväggen regeln kommandon:
Den **az mysql-brandväggsregel** kommandot används från Azure CLI för att skapa, ta bort, lista, visa och uppdatera brandväggsreglerna.

Kommandon:
- **Skapa**: skapa en brandväggsregel för Azure MySQL-server.
- **ta bort**: ta bort en brandväggsregel för Azure MySQL-server.
- **listan**: visa en lista med brandväggsregler för Azure MySQL-server.
- **Visa**: Visa detaljer för en server i Azure MySQL brandväggsregel.
- **Uppdatera**: uppdatera en Azure MySQL brandväggsregel.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Logga in på Azure och visa en lista med din Azure-databas för MySQL-servrar
På ett säkert sätt ansluta Azure CLI med Azure-konto med hjälp av den **az inloggningen** kommando.

1. Kör följande kommando från kommandoraden:
```azurecli
az login
```
Detta kommando en kod som ska användas i nästa steg.

2. Använda en webbläsare för att öppna sidan [https://aka.ms/devicelogin](https://aka.ms/devicelogin), och ange sedan koden.

3. Logga in med dina autentiseringsuppgifter för Azure när du ombeds göra det.

4. När inloggningen är auktoriserad ut en lista över prenumerationer i konsolen. Kopiera ID för den önskade prenumerationen för att ange den aktuella prenumerationen ska användas.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Visa en lista med Azure-databaser för MySQL-servrar för din prenumeration och resurs-grupp om du är osäker på namnen.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Observera namnattributet i listan som du måste ange MySQL-servern att fungera på. Om det behövs, bekräfta informationen för servern och använder attributet för att kontrollera att den är korrekt:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lista brandväggsregler på Azure-databas för MySQL-Server 
Med hjälp av namnet på servern och resursgruppens namn, lista över befintliga serverbrandväggsreglerna på servern. Observera att server name-attribut har angetts i den **--server** växla och inte i den **--namnet** växla.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
Utdata innehåller reglerna, om sådant finns, i JSON-format (som standard). Du kan använda den **--utdatatabell** växla till skriver resultatet i ett mer lättläst tabellformat.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Skapa en brandväggsregel på Azure-databas för MySQL-Server
Skapa en ny brandväggsregel på servern med namnet på Azure MySQL-servern och resursgruppens namn. Ange ett namn för regeln som start-IP och sluta IP (för att ge åtkomst till ett intervall med IP-adresser) för regeln.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Ange samma IP-adress som den första IP- och slut-IP som i följande exempel för att tillåta åtkomst för en enda IP-adress.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Kommandoutdata visas vid lyckades, information om brandväggsregeln som du har skapat i JSON-format (som standard). Om det finns ett fel, visar utdata text för felmeddelande i stället.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Uppdatera en brandväggsregel på Azure-databas för MySQL-server 
Med Azure MySQL-servernamnet och resursgruppens namn kan uppdatera en befintlig brandväggsregel på servern. Ange namnet på befintlig brandväggsregel som indata, samt starta IP- och IP-attribut som ska uppdateras.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Kommandoutdata visas vid lyckades, information om brandväggsregeln som du har uppdaterat i JSON-format (som standard). Om det finns ett fel, visar utdata text för felmeddelande i stället.

> [!NOTE]
> Om brandväggsregeln inte finns, skapas regeln av kommandot update.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Visa brandväggen regeldetaljer på Azure-databas för MySQL-servern
Använda Azure MySQL-servernamnet och resursgruppens namn, visa befintliga brandväggen Regelinformation från servern. Ange namnet på befintlig brandväggsregel som indata.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Kommandoutdata visas vid lyckades, information om brandväggsregeln som du har angett i JSON-format (som standard). Om det finns ett fel, visar utdata text för felmeddelande i stället.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Ta bort en brandväggsregel på Azure-databas för MySQL-Server
Använda Azure MySQL-servernamnet och resursgruppens namn, ta bort en befintlig brandväggsregel från servern. Ange namnet på befintlig brandväggsregel.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Det är klart, inga utdata. Text för felmeddelande visas vid ett fel.

## <a name="next-steps"></a>Nästa steg
- Mer information om [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md).
- [Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure portal](./howto-manage-firewall-using-portal.md).
