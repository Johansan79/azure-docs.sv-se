---
title: "Skapa en funktion i Azure som utlöses av kömeddelanden | Microsoft Docs"
description: "Använd Azure Functions för att skapa en funktion utan server som startas av meddelanden som skickas till en Azure Storage-kö."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 3fd5a5b9d2e2eec485fd9ecc5380ad6adb9851d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Skapa en funktion som utlöses av Azure Queue Storage

Lär dig hur du skapar en funktion som utlöses när meddelanden skickas till en Azure Storage-kö.

![Visa meddelande i loggarna.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Krav

- Hämta och installera [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

- En Azure-prenumeration. Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

Därefter skapar du en funktion i den nya funktionsappen.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Skapa en funktion som utlöses av en kö

1. Expandera funktionsappen och klicka på knappen **+** bredvid **Funktioner**. Om det är den första funktionen i din funktionsapp väljer du **Anpassad funktion**. Detta visar en fullständig uppsättning med funktionsmallar.

    ![Sidan snabbstart för funktioner i Azure Portal](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. Välj en mall för **Kö-utlösare** för önskat språk och använd inställningarna som angetts i tabellen.

    ![Skapa funktionen som utlöses av lagringskön.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | Inställning | Föreslaget värde | Beskrivning |
    |---|---|---|
    | **Namnge din funktion** | Ett unikt namn i funktionsappen | Namnge funktionen som utlöses av kön. |
    | **Könamn**   | myqueue-items    | Namnet på den kö som ska anslutas till i ditt Storage-konto. |
    | **Lagringskontoanslutning** | AzureWebJobStorage | Du kan antingen använda den lagringskontoanslutning som redan används i funktionsappen eller skapa en ny.  |    

3. Klicka på **Skapa** för att skapa den nya funktionen.

Sedan ansluter du till ditt Azure Storage-konto och skapar lagringskön **myqueue-items**.

## <a name="create-the-queue"></a>Skapa kön

1. Klicka på **Integrera** i din funktion, expandera **Dokumentation** och kopiera både **kontonamnet** och **kontonyckeln**. Du kan använda dessa autentiseringsuppgifter för att ansluta till storage-konto i Azure Lagringsutforskaren. Om du redan har anslutit ditt lagringskonto går du vidare till steg 4.

    ![Hämta autentiseringsuppgifterna för att ansluta till lagringskontot.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)

1. Kör verktyget [Microsoft Azure Storage Explorer](http://storageexplorer.com/), klicka på anslutningsikonen till vänster, välj **Use a storage account name and key** (Använd ett kontonamn och en kontonyckel för lagringskontot) och klicka på **Nästa**.

    ![Kör verktyget Storage Account Explorer.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Ange **kontonamnet** och **kontonyckeln** från steg 1, klicka på **Nästa** och sedan på **Anslut**.

    ![Ange autentiseringsuppgifter för lagringskontot och anslut.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Expandera bifogade storage-konto, högerklicka på **köer**, klickar du på **Skapa kö**, typen `myqueue-items`, och tryck sedan på RETUR.

    ![Skapa en lagringskö.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Nu när du har en lagringskö kan du testa funktionen genom att lägga till ett meddelande i kön.

## <a name="test-the-function"></a>Testa funktionen

1. Tillbaka i Azure-portalen går du till din funktion, expandera den **loggar** längst ned på sidan och se till att loggen strömning inte är pausad.

1. I Lagringsutforskaren expanderar du ditt lagringskonto, **Köer** och **myqueue-items**. Klicka sedan på **Lägg till meddelande**.

    ![Lägg till ett meddelande i kön.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Skriv ditt "Hello World!"- meddelande i **Meddelandetext** och klicka på **OK**.

1. Vänta några sekunder och gå sedan tillbaka till dina funktionsloggar och kontrollera att det nya meddelandet har lästs från kön.

    ![Visa meddelande i loggarna.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Gå tillbaka till Lagringsutforskaren, klicka på **Uppdatera** och kontrollera att meddelandet har bearbetats och inte längre finns i kön.

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har skapat en funktion som körs när ett meddelande läggs till i en lagringskö.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information om Queue Storage-utlösare finns i [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md) (Azure Functions-lagringsköbindningar).
