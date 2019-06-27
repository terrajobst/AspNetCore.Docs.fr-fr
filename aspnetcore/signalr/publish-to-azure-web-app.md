---
title: Publier une ASP.NET Core application SignalR dans Azure App Service
author: bradygaster
description: Découvrez comment publier une application ASP.NET Core SignalR sur Azure App Service.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406113"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Publier une ASP.NET Core application SignalR dans Azure App Service

Par [Brady Gaster](https://twitter.com/bradygaster)

[Azure App Service](/azure/app-service/app-service-web-overview) est un [informatique en nuage Microsoft](https://azure.microsoft.com/) service de plateforme pour l’hébergement d’applications web, y compris ASP.NET Core.

> [!NOTE]
> Cet article fait référence à la publication d’une application ASP.NET Core SignalR à partir de Visual Studio. Pour plus d’informations, consultez [service SignalR pour Azure](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Publier l'application

Cet article couvre la publication avec les outils dans Visual Studio. Visual Studio Code les utilisateurs peuvent utiliser [Azure CLI](/cli/azure) commandes pour publier des applications sur Azure. Pour plus d’informations, consultez [publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet).

1. Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Publier**.

1. Vérifiez que **App Service** et **créer** sont sélectionnés dans le **choisir une cible de publication** boîte de dialogue.

1. Sélectionnez **créer un profil** à partir de la **publier** bouton Déplacer vers le bas.

   Entrez les informations décrites dans le tableau suivant dans le **créer App Service** boîte de dialogue et sélectionnez **créer**.

   | Élément               | Description |
   | ------------------ | ----------- |
   | **Name**           | Nom unique de l’application. |
   | **Abonnement**   | Abonnement Azure qui utilise l’application. |
   | **Groupe de ressources** | Groupe de ressources liées à laquelle appartient l’application. |
   | **Plan d’hébergement**   | Plan de tarification pour l’application web. |

1. Sélectionnez le **Service Azure SignalR** dans le **dépendances** > **ajouter** liste déroulante :

   ![Zone de dépendances montrant la sélection du Service Azure SignalR dans la liste déroulante Ajouter](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. Dans le **Service Azure SignalR** boîte de dialogue, sélectionnez **créer une nouvelle instance de Service Azure SignalR**.

1. Fournir un **nom**, **groupe de ressources**, et **emplacement**. Retour à la **Service Azure SignalR** boîte de dialogue et sélectionnez **ajouter**.

Visual Studio effectue les tâches suivantes :

* Crée un profil de publication contenant les paramètres de publication.
* Crée un *Azure Web App* avec les détails fournis.
* Publie l’application.
* Lance un navigateur, ce qui charge l’application web.

Le format d’URL de l’application est `{APP SERVICE NAME}.azurewebsites.net`. Par exemple, une application nommée `SignalRChatApp` possède une URL de `https://signalrchatapp.azurewebsites.net`.

Si HTTP *502.2 - passerelle incorrecte* erreur se produit lors du déploiement d’une application qui cible une version préliminaire de version de .NET Core, consultez [préversion déployer ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) pour résoudre le problème.

## <a name="configure-the-app-in-azure-app-service"></a>Configurer l’application dans Azure App Service

> [!NOTE]
> *Cette section s’applique uniquement aux applications n’utilisent ne pas le Service Azure SignalR.*
>
> Si l’application utilise le Service Azure SignalR, le Service d’application ne nécessite pas la configuration de l’affinité de l’Application Request Routing (ARR) et Web Sockets décrites dans cette section. Les clients connectent à leurs Sockets Web au Service Azure SignalR, pas directement à l’application.

Pour les applications hébergées sans le Service Azure SignalR, activer :

* [Affinité ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) pour acheminer les demandes à partir d’un utilisateur à la même instance d’App Service. Le paramètre par défaut est **sur**.
* [Web Sockets](xref:fundamentals/websockets) pour permettre le transport WebSocket de la fonction. Le paramètre par défaut est **hors**.

1. Dans le portail Azure, accédez à l’application web dans **App Services**.
1. Ouvrez **Configuration** > **paramètres généraux**.
1. Définissez **Web sockets** à **sur**.
1. Vérifiez que **affinité ARR** a la valeur **sur**.

## <a name="app-service-plan-limits"></a>Plan App Service limite

Web Sockets et les autres transports sont limitées selon le Plan App Service sélectionné. Pour plus d’informations, consultez le *Azure Cloud Services limite* et *limites App Service* sections de la [abonnement Azure et limites de service, quotas et contraintes](/azure/azure-subscription-service-limits#app-service-limits) article.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Qu’est le Service Azure SignalR ?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet)
* [Héberger et déployer des applications ASP.NET Core Preview sur Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
