---
title: Publier un noyau ASP.NET applications SignalR à l’application Web Azure
author: rachelappel
description: Publier un noyau ASP.NET applications SignalR à l’application Web Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271916"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publier un noyau ASP.NET applications SignalR à une application Web Azure

[Application Web Azure](/azure/app-service/app-service-web-overview) est un [informatique en nuage Microsoft](https://azure.microsoft.com/) service de plateforme pour l’hébergement d’applications web, y compris ASP.NET Core.

> [!NOTE]
> Cet article fait référence à la publication d’une application ASP.NET Core SignalR à partir de Visual Studio. Visitez [service SignalR pour Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) pour plus d’informations sur l’utilisation de SignalR sur Azure.

## <a name="publish-the-app"></a>Publier l'application

Visual Studio fournit des outils intégrés pour la publication vers une application Web Azure. Visual Studio Code utilisateur peut utiliser [CLI d’Azure](/cli/azure) commandes pour publier des applications pour Azure. Cet article traite de publication utilisant les outils dans Visual Studio. Pour publier une application à l’aide de CLI d’Azure, consultez [publier une application ASP.NET Core dans Windows Azure avec les outils de ligne de commande](xref:tutorials/publish-to-azure-webapp-using-cli).

Avec le bouton droit sur le projet dans **l’Explorateur de solutions** et sélectionnez **publier**. Vérifiez que **nouvel** est activée dans le **choisir une cible de publication** boîte de dialogue, puis sélectionnez **publier**.

![Choix de publication cible](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Entrez les informations suivantes dans le **créer un Service application** boîte de dialogue et sélectionnez **créer**.

| Élément | Description |
| ---- | ----------- |
| **Nom de l’application** | Un nom unique de l’application. |
| **Abonnement** | L’abonnement Azure utilisé par l’application. |
| **Groupe de ressources** | Le groupe de ressources liées à laquelle appartient l’application.  |
| **Plan d’hébergement** | Le plan de tarification pour l’application web. |

![Créer le service d’applications](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio effectue les tâches suivantes :

* Crée un profil de publication qui contient les paramètres de publication.
* Crée ou utilise un fichier *application Web Azure* avec les détails fournis.
* Publie l’application.
* Lance un navigateur, avec l’application web publiée chargée.

Notez le format de l’URL pour l’application est *{nom de l’application} .azurewebsites .net*. Par exemple, une application nommée `SignalRChattR` possède une URL qui ressemble à `https://signalrchattr.azurewebsites.net`.

Si une erreur HTTP 502.2 se produit, consultez [préversion déployer ASP.NET Core pour le Service d’applications Azure](xref:host-and-deploy/azure-apps/index) pour résoudre le problème.

## <a name="configure-signalr-web-app"></a>Configurer l’application web de SignalR

Les applications ASP.NET Core SignalR qui sont publiées comme une application Web Azure doit avoir [ARR affinité](https://en.wikipedia.org/wiki/Application_Request_Routing) activé. [WebSockets](xref:fundamentals/websockets) doit être activée pour permettre le transport WebSocket (fonction).

Dans le portail Azure, accédez à **paramètres de l’application** pour votre application web. Définissez **WebSockets** à **sur**et vérifiez **ARR affinité** est **sur**.

![Paramètres de l’application Web Azure dans le portail Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets et autres transports [sont limitées, selon le Plan App Service](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Ressources connexes

* [Publier une application ASP.NET Core dans Windows Azure avec les outils de ligne de commande](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Publier une application ASP.NET Core dans Azure avec Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Héberger et déployer des applications ASP.NET Core aperçu sur Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
