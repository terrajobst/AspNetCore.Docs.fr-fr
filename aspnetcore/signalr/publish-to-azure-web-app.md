---
title: Publier une application ASP.NET Core SignalR sur Azure Web App
author: bradygaster
description: Publier une application ASP.NET Core SignalR sur Azure Web App
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 1c472711a86edae8dc6e207734aa54e48c02d47d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087705"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publier une application ASP.NET Core SignalR sur Azure Web App

[Azure Web App](/azure/app-service/app-service-web-overview) est une plateforme de services de [Cloud computing de Microsoft](https://azure.microsoft.com/) pour l’hébergement d’applications web, y compris ASP.NET Core.

> [!NOTE]
> Cet article fait référence à la publication d’une application ASP.NET Core SignalR à partir de Visual Studio. Visitez [service SignalR pour Azure](https://azure.microsoft.com/services/signalr-service) pour plus d’informations sur l’utilisation de SignalR sur Azure.

## <a name="publish-the-app"></a>Publier l'application

Visual Studio fournit des outils intégrés pour la publication sur Azure Web App. Un utilisateur de Visual Studio Code peut utiliser les commandes [Azure CLI](/cli/azure) pour publier des applications sur Azure. Cet article couvre la publication avec les outils dans Visual Studio. Pour publier une application à l’aide d’Azure CLI, consultez [publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet).

Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Publier**. Vérifiez que **créer** est archivé le **choisir une cible de publication** boîte de dialogue, puis sélectionnez **publier**.

![Choix de publication cible](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Entrez les informations suivantes dans la boîte de dialogue **Créer un App Service** et sélectionnez **Créer**.

| Élément | Description |
| ---- | ----------- |
| **Nom de l’application** | Un nom unique de l’application. |
| **Abonnement** | L’abonnement Azure qui utilise l’application. |
| **Groupe de ressources** | Le groupe de ressources liées à laquelle appartient l’application.  |
| **Plan d’hébergement** | Le plan de tarification pour l’application web. |

![Créer App Service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio effectue les tâches suivantes :

* Crée un profil de publication contenant les paramètres de publication.
* Crée ou utilise une *Azure Web App* existante avec les détails fournis.
* Publie l’application.
* Lance un navigateur, avec l’application web publiée chargée.

Notez que le format de l’URL pour l’application est *{nom de l’application}.azurewebsites .net*. Par exemple, une application nommée `SignalRChattR` possède une URL qui ressemble à `https://signalrchattr.azurewebsites.net`.

Si une erreur HTTP 502.2 se produit, consultez [préversion déployer ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/index) pour résoudre le problème.

## <a name="configure-signalr-web-app"></a>Configurer l’application web de SignalR

Les applications ASP.NET Core SignalR qui sont publiées comme une application Web Azure doit avoir [affinité ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) activé. [WebSockets](xref:fundamentals/websockets) doit être activée pour permettre le transport WebSocket de la fonction.

Dans le portail Azure, accédez à **paramètres de l’application** pour votre application web. Définissez **WebSockets** à **sur**et vérifiez **affinité ARR** est **sur**.

![Paramètres de l’application Web Azure dans le portail Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 Les WebSockets et autres transports [sont limités selon le Plan App Service](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Ressources connexes

* [Publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet)
* [Publier une application ASP.NET Core sur Azure avec Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Héberger et déployer des applications ASP.NET Core Preview sur Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
