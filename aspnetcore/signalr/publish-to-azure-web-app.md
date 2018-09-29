---
title: Publier une ASP.NET Core SignalR application Azure Web App
author: tdykstra
description: Publier une ASP.NET Core SignalR application Azure Web App
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: a6a0e44f5c67fefdac6bd26b3772c23e75f8bfc1
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454724"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publier une ASP.NET Core application SignalR dans une application Web Azure

[L’application Web Azure](/azure/app-service/app-service-web-overview) est un [informatique en nuage Microsoft](https://azure.microsoft.com/) service de plateforme pour l’hébergement d’applications web, y compris ASP.NET Core.

> [!NOTE]
> Cet article fait référence à la publication d’une application ASP.NET Core SignalR à partir de Visual Studio. Visitez [service SignalR pour Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) pour plus d’informations sur l’utilisation de SignalR sur Azure.

## <a name="publish-the-app"></a>Publier l'application

Visual Studio fournit des outils intégrés pour la publication vers une application Web Azure. Visual Studio Code utilisateur peut utiliser [Azure CLI](/cli/azure) commandes pour publier des applications sur Azure. Cet article couvre la publication avec les outils dans Visual Studio. Pour publier une application à l’aide d’Azure CLI, consultez [publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet).

Avec le bouton droit sur le projet dans **l’Explorateur de solutions** et sélectionnez **publier**. Vérifiez que **créer** est archivé le **choisir une cible de publication** boîte de dialogue, puis sélectionnez **publier**.

![Choix de publication cible](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Entrez les informations suivantes dans le **créer App Service** boîte de dialogue et sélectionnez **créer**.

| Élément | Description |
| ---- | ----------- |
| **Nom de l’application** | Un nom unique de l’application. |
| **Abonnement** | L’abonnement Azure qui utilise l’application. |
| **Groupe de ressources** | Le groupe de ressources liées à laquelle appartient l’application.  |
| **Plan d’hébergement** | Le plan de tarification pour l’application web. |

![Créer app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio effectue les tâches suivantes :

* Crée un profil de publication contenant les paramètres de publication.
* Crée ou utilise un existant *Azure Web App* avec les détails fournis.
* Publie l’application.
* Lance un navigateur, avec l’application web publiée chargée.

Notez que le format de l’URL pour l’application est *{nom de l’application} .azurewebsites .net*. Par exemple, une application nommée `SignalRChattR` possède une URL qui ressemble à `https://signalrchattr.azurewebsites.net`.

Si une erreur HTTP 502.2 se produit, consultez [préversion déployer ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/index) pour résoudre le problème.

## <a name="configure-signalr-web-app"></a>Configurer l’application web de SignalR

Les applications ASP.NET Core SignalR qui sont publiées comme une application Web Azure doit avoir [affinité ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) activé. [WebSockets](xref:fundamentals/websockets) doit être activée pour permettre le transport WebSocket de la fonction.

Dans le portail Azure, accédez à **paramètres de l’application** pour votre application web. Définissez **WebSockets** à **sur**et vérifiez **affinité ARR** est **sur**.

![Paramètres de l’application Web Azure dans le portail Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets et autres transports [sont limitées selon le Plan App Service](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Ressources connexes

* [Publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet)
* [Publier une application ASP.NET Core sur Azure avec Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Héberger et déployer des applications ASP.NET Core Preview sur Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
