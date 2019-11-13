---
title: Publier une application SignalR ASP.NET Core sur Azure App Service
author: bradygaster
description: Découvrez comment publier une application SignalR ASP.NET Core sur Azure App Service.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: d03a007ca883b3d0391b848e3e92c90469ee640a
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963933"
---
# <a name="publish-an-aspnet-core-opno-locsignalr-app-to-azure-app-service"></a>Publier une application SignalR ASP.NET Core sur Azure App Service

Par [Brady Gaster](https://twitter.com/bradygaster)

[Azure App service](/azure/app-service/app-service-web-overview) est un service de plateforme [Microsoft Cloud Computing](https://azure.microsoft.com/) pour héberger des applications web, y compris ASP.net core.

> [!NOTE]
> Cet article fait référence à la publication d’une application ASP.NET Core SignalR à partir de Visual Studio. Pour plus d’informations, consultez [SignalR service pour Azure](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Publier l'application

Cet article traite de la publication à l’aide des outils de Visual Studio. Visual Studio Code utilisateurs peuvent utiliser des commandes [Azure CLI](/cli/azure) pour publier des applications sur Azure. Pour plus d’informations, consultez [publier une application ASP.net Core sur Azure à l’aide des outils en ligne de commande](/azure/app-service/app-service-web-get-started-dotnet).

1. Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Publier**.

1. Confirmez que **app service** et **créer nouveau** sont sélectionnés dans la boîte de dialogue **choisir une cible de publication** .

1. Sélectionnez **créer un profil** dans la liste déroulante du bouton **publier** .

   Entrez les informations décrites dans le tableau suivant de la boîte de dialogue **créer un app service** , puis sélectionnez **créer**.

   | Élément               | Description |
   | ------------------ | ----------- |
   | **Nom**           | Nom unique de l’application. |
   | **Abonnement**   | Abonnement Azure utilisé par l’application. |
   | **Groupe de ressources** | Groupe de ressources associées auxquelles l’application appartient. |
   | **Plan d’hébergement**   | Plan de tarification pour l’application Web. |

1. Sélectionnez le **service Azure SignalR** dans la liste déroulante **dépendances** > **Ajouter** :

   ![Zone dépendances présentant la sélection d’Azure [ ! Opérationnel. Service NO-LOC (Signalr)] dans la liste déroulante Ajouter](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. Dans la boîte de dialogue **service azure SignalR** , sélectionnez **créer une nouvelle instance Azure SignalR service**.

1. Fournissez un **nom**, un **groupe de ressources**et un **emplacement**. Revenez à la boîte de dialogue **Azure SignalR service** et sélectionnez **Ajouter**.

Visual Studio effectue les tâches suivantes :

* Crée un profil de publication contenant les paramètres de publication.
* Crée une *application Web Azure* avec les détails fournis.
* Publie l’application.
* Lance un navigateur, qui charge l’application Web.

Le format de l’URL de l’application est `{APP SERVICE NAME}.azurewebsites.net`. Par exemple, une application nommée `SignalRChatApp` a une URL de `https://signalrchatapp.azurewebsites.net`.

Si une erreur de *passerelle HTTP 502,2-incorrecte* se produit lors du déploiement d’une application qui cible une version préliminaire de .net Core, consultez [déployer ASP.net core version préliminaire dans Azure App service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) pour la résoudre.

## <a name="configure-the-app-in-azure-app-service"></a>Configurer l’application dans Azure App Service

> [!NOTE]
> *Cette section s’applique uniquement aux applications qui n’utilisent pas le service Azure SignalR.*
>
> Si l’application utilise le service Azure SignalR, la App Service ne nécessite pas la configuration de l’affinité de Application Request Routing (ARR) et des sockets Web décrits dans cette section. Les clients connectent leurs Sockets Web au service Azure SignalR, et non directement à l’application.

Pour les applications hébergées sans le service Azure SignalR, activez :

* [Affinité arr](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) pour acheminer les demandes d’un utilisateur vers la même instance de App service. La valeur par défaut est **on**.
* [WebSockets](xref:fundamentals/websockets) pour permettre au transport Web Sockets de fonctionner. La valeur par défaut est **off**.

1. Dans le Portail Azure, accédez à l’application Web dans **app services**.
1. Ouvrez **Configuration** > **paramètres généraux**.
1. Affectez la valeur **on**à **Web Sockets** .
1. Vérifiez que l' **affinité arr** est définie sur **on**.

## <a name="app-service-plan-limits"></a>Limites du plan de App Service

Les sockets Web et les autres transports sont limités en fonction du plan de App Service sélectionné. Pour plus d’informations, consultez les sections *limites de services Cloud Azure* et *limites de App service* de l’article [abonnement Azure et limites, quotas et contraintes du service](/azure/azure-subscription-service-limits#app-service-limits) .

## <a name="additional-resources"></a>Ressources supplémentaires

* [Qu’est-ce qu’Azure SignalR service ?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Publier une application ASP.NET Core sur Azure avec les outils en ligne de commande](/azure/app-service/app-service-web-get-started-dotnet)
* [Héberger et déployer ASP.NET Core des applications en version préliminaire sur Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
