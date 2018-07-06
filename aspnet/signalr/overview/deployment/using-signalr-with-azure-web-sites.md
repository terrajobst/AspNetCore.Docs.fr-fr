---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: À l’aide de SignalR avec Web Apps dans Azure App Service | Microsoft Docs
author: pfletcher
description: Ce document décrit comment configurer une application de SignalR qui s’exécute sur Microsoft Azure. Versions des logiciels utilisaient dans le didacticiel Visual Studio 2013 ou vis...
ms.author: aspnetcontent
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 03db2debe6bf5935ced926ae8278df8b9d47c86a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822164"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>À l’aide de SignalR avec Web Apps dans Azure App Service
====================
par [Patrick Fletcher](https://github.com/pfletcher)

> Ce document décrit comment configurer une application de SignalR qui s’exécute sur Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) ou Visual Studio 2012
> - .NET 4.5
> - SignalR version 2
> - Azure SDK 2.3 pour Visual Studio 2013 ou 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), ou le [forums Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Sommaire

- [Introduction](#introduction)
- [Déploiement d’une application Web de SignalR sur Azure App Service](#deploying)
- [L’activation du protocole WebSocket sur Azure App Service](#websocket)
- [À l’aide de l’infrastructure d’intégration du Cache Redis Azure](#backplane)
- [Étapes suivantes](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introduction

ASP.NET SignalR peut servir à récupérer un nouveau niveau d’interactivité entre serveurs et web ou les clients .NET. Quand ils sont hébergés dans Azure, des applications SignalR peuvent tirer parti de haut niveau de disponibilité, évolutives et performantes environnement en cours d’exécution dans le cloud fournit.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Déploiement d’une application Web de SignalR sur Azure App Service

SignalR n’ajoute pas les complications particuliers pour déployer une application dans Azure et un déploiement à un serveur local. Une application qui utilise SignalR peut être hébergée dans Azure sans modification de configuration ou d’autres paramètres (Cependant, pour la prise en charge du protocole WebSocket, consultez [l’activation du protocole WebSocket sur Azure App Service](#websocket) ci-dessous.) Pour ce didacticiel, vous allez déployer l’application créée dans le [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) vers Azure.

**Composants requis**

- Visual Studio 2013. Si vous n’avez pas Visual Studio, Visual Studio 2013 Express pour le Web est inclus dans l’installation du SDK Azure.
- [Azure SDK 2.3 pour Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou [Azure SDK 2.3 pour Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Pour suivre ce didacticiel, vous aurez besoin un abonnement Azure. Vous pouvez [activer vos avantages d’abonné MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), ou [souscrire un abonnement d’évaluation](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Déploiement d’une application web de SignalR vers Azure

1. Terminer la [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), ou téléchargez le projet terminé sur [galerie de Code](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. Dans Visual Studio, sélectionnez **Build**, **publier le SignalR Chat**.
3. Dans la boîte de dialogue « Publier le site Web », sélectionnez « Sites Web Windows Azure ».

    ![Sélectionnez les Sites Web Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Si vous n’êtes pas connecté à votre compte Microsoft, cliquez sur **se connecter...**  dans la boîte de dialogue « Sélectionner un Site Web existant », puis connectez-vous.

    ![Sélectionnez le Site Web existant](using-signalr-with-azure-web-sites/_static/image2.png)    ![Se connecter à Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Dans la boîte de dialogue « Sélectionner un Site Web existant », cliquez sur **New**.

    ![Nouveau site Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. Dans la boîte de dialogue « Créer un site sur Windows Azure », entrez un nom d’application unique. Dans la liste déroulante région, sélectionnez la région la plus proche pour vous. Cliquez sur **Créer**.

    ![Créer un site sur Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Dans la boîte de dialogue « Publier le site Web », cliquez sur **publier**.

    ![Publier le site](using-signalr-with-azure-web-sites/_static/image6.png)
8. Lorsque l’application a terminé la publication, l’application de conversation de SignalR hébergée dans Azure App Service Web Apps s’ouvre dans un navigateur.

    ![Site dans un navigateur](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>L’activation du protocole WebSocket sur Azure App Service Web Apps

WebSocket doit être explicitement activée dans votre application web à utiliser dans une application SignalR ; Sinon, les autres protocoles seront utilisés (voir [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports) pour plus d’informations).

Pour pouvoir utiliser les WebSockets sur Azure App Service Web Apps, vous devez l’activer dans la section de configuration de l’application web. Pour ce faire, ouvrez votre application web dans le [portail de gestion](https://manage.windowsazure.com/)et sélectionnez Configurer.

![Onglet Configurer](using-signalr-with-azure-web-sites/_static/image8.png)

En haut de la page de configuration, assurez-vous que le .NET 4.5 est utilisée pour votre application web.

![Paramètre de .NET framework version 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

Dans la page de configuration, dans le **WebSockets** , sélectionnez **sur**.

![Paramètre de protocole WebSocket : sur](using-signalr-with-azure-web-sites/_static/image10.png)

Au bas de la page de Configuration, sélectionnez **enregistrer** pour enregistrer vos modifications.

![Enregistrer les paramètres](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>À l’aide de l’infrastructure d’intégration du Cache Redis Azure

Si vous utilisez plusieurs instances de votre application web, et les utilisateurs de ces instances doivent interagir avec eux (afin que, par exemple, les messages de conversation créées dans une seule instance peuvent atteindre les utilisateurs connectés à d’autres instances), le [du Cache Redis Azure fond de panier](../performance/scaleout-with-redis.md) doit être implémentée dans votre application.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les applications Web dans Azure App Service, consultez [vue d’ensemble des applications Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
