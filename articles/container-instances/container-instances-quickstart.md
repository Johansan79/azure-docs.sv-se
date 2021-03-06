---
title: "Snabbstart - skapa din första Azure Behållarinstanser behållare"
description: "Distribuera och kom igång med Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 160ba84d2c022ca3af2eb13a9689a282b4a6b198
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/20/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Skapa din första behållare i Azure Container Instances
Azure Behållarinstanser som gör det enkelt att skapa och hantera Docker-behållare i Azure, utan att behöva etablera virtuella datorer eller anta en högre nivå tjänst. I Snabbstart, skapa en behållare i Azure och exponera till internet med en offentlig IP-adress. Den här åtgärden utförs med ett enda kommando. Inom några sekunder visas det i webbläsaren:

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Du kan använda Azure Cloud-gränssnitt eller en lokal installation av Azure CLI för att slutföra den här snabbstarten. Om du väljer att installera och använda CLI lokalt denna Snabbstart kräver att du använder Azure CLI version 2.0.21 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure Container Instances är Azure-resurser och måste vara placerade i en Azure-resursgrupp, en logisk samling dit Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med det [az gruppen skapa] [ az-group-create] kommando.

I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Skapa en behållare

Du kan skapa en behållare genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp till den [az behållaren skapa] [ az-container-create] kommando. Du kan också göra behållaren tillgänglig på Internet med en offentlig IP-adress. I den här snabbstarten du distribuera en behållare som är värd för en liten webbprogram som skrivits i [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public --ports 80
```

Om några sekunder bör du få ett svar på din begäran. Inledningsvis behållaren finns i den **skapa** tillstånd, men det bör starta inom några sekunder. Du kan kontrollera status med hjälp av den [az behållaren visa] [ az-container-show] kommando:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Längst ned i resultatet visas behållarens etableringsstatus och IP-adress:

```json
...
"ipAddress": {
      "ip": "13.88.176.27",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

När behållaren flyttas till den **lyckades** tillstånd, kan du nå den i din webbläsare med den angivna IP-adressen.

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

## <a name="pull-the-container-logs"></a>Hämta behållarloggarna

Du kan hämta loggar för den behållare som du skapat med hjälp av [az behållaren loggar] [ az-container-logs] kommando:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Resultat:

```bash
Server running...
10.240.255.107 - - [20/Nov/2017:19:16:28 +0000] "GET / HTTP/1.1" 200 1663 "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
10.240.255.107 - - [20/Nov/2017:19:16:28 +0000] "GET / HTTP/1.1" 200 1663
10.240.255.107 - - [20/Nov/2017:19:16:28 +0000] "GET /favicon.ico HTTP/1.1" 404 19
```

## <a name="delete-the-container"></a>Ta bort behållaren

När du är klar med behållaren du kan ta bort den med hjälp av den [az behållaren bort] [ az-container-delete] kommando:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

Kontrollera att behållaren har tagits bort genom att köra den [az behållaren listan](/cli/azure/container#az_container_list) kommando:

```azurecli-interactive
az container list --resource-group myResourceGroup -o table
```

Den **minbehållare** behållaren ska inte visas i kommandots utdata. Om du har inga andra behållare i resursgruppen visas inga utdata.

## <a name="next-steps"></a>Nästa steg

Koden för den behållare som används i denna Snabbstart finns [på GitHub][app-github-repo], tillsammans med dess Dockerfile. Om du vill försöka skapa den på egen hand och distribuera den till Azure Container Instances via Azure Container Registry, går du vidare till självstudierna för Azure Container Instances.

> [!div class="nextstepaction"]
> [Azure Container Instances-självstudier](./container-instances-tutorial-prepare-app.md)

Om du vill testa alternativ för att köra behållare i ett orchestration-system på Azure, finns det [Service Fabric] [ service-fabric] eller [Azure Container Service (AKS)] [ container-service] Snabbstart.

<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
[container-service]: ../aks/kubernetes-walkthrough.md


<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
