---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: À l’aide de Web API 2 avec Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel vous apprend aux principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end. Ce didacticiel utilise Entity Framework 6 pour la disposition de données...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236521"
---
<a name="using-web-api-2-with-entity-framework-6"></a>À l’aide de Web API 2 avec Entity Framework 6
====================

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

> Ce didacticiel vous enseigne les principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end. Le didacticiel utilise Entity Framework 6 de la couche de données et Knockout.js pour l’application de JavaScript côté client. Le didacticiel montre également comment déployer l’application dans Azure App Service Web Apps.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
> - Web API 2.1
> - Visual Studio 2017 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Knockout.js](http://knockoutjs.com/) 3.1

Ce didacticiel utilise l’API Web ASP.NET 2 avec Entity Framework 6 pour créer une application web qui manipule une base de données principale. Voici une capture d’écran de l’application que vous allez créer.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

L’application utilise une conception d’application à page unique (SPA). « Single-page application » est le terme général qui désigne une application web qui charge une seule page HTML et puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages. Après le chargement de page initial, l’application s’entretient avec le serveur via les demandes AJAX. L’AJAX demande de renvoyer les données JSON, que l’application utilise pour mettre à jour de l’interface utilisateur.

AJAX n’est pas nouveau, mais aujourd'hui, il existe des infrastructures JavaScript qui le rendent plus facile créer et gérer une grande application SPA sophistiquée. Ce didacticiel utilise [Knockout.js](http://knockoutjs.com/), mais vous pouvez utiliser n’importe quel framework de client JavaScript.

Voici les principaux blocs de construction pour cette application :

- ASP.NET MVC crée la page HTML.
- API Web ASP.NET gère les requêtes AJAX et retourne les données JSON.
- Knockout.js-lie les éléments HTML pour les données JSON.
- Entity Framework communique avec la base de données.

## <a name="see-this-app-running-on-azure"></a>Consultez cette application en cours d’exécution sur Azure

Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ? Vous pouvez déployer une version complète de l’application à votre compte Azure en sélectionnant le bouton suivant.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas déjà un compte, vous disposez des options suivantes :

- [Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

## <a name="create-the-project"></a>Créer le projet

Ouvrez Visual Studio. À partir de la **fichier** menu, sélectionnez **New**, puis sélectionnez **projet**. (Ou sélectionnez **nouveau projet** sur la page de démarrage.)

Dans le **nouveau projet** boîte de dialogue, sélectionnez **Web** dans le volet gauche et **Application Web ASP.NET (.NET Framework)** dans le volet central. Nommez le projet **BookService** et sélectionnez **OK**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **API Web** modèle.

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


Sélectionnez **OK** pour créer le projet.

## <a name="configure-azure-settings-optional"></a>Configurer les paramètres Azure (facultatifs)

Après avoir créé le projet, vous pouvez choisir de déployer dans Azure App Service Web Apps à tout moment. 

1. Dans l’Explorateur de solutions, cliquez sur votre projet, puis sélectionnez **publier**.

2. Dans la fenêtre qui s’affiche, sélectionnez **Démarrer**. Le **choisir une cible de publication** boîte de dialogue s’affiche.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Sélectionnez **Créer un profil**. La boîte de dialogue **Créer App Service** s’affiche.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Acceptez les valeurs par défaut, ou entrez des valeurs différentes pour le nom de l’application, groupe de ressources, qui héberge le plan d’abonnement Azure et région géographique. 

4. Sélectionnez **créer une base de données SQL**. Le **configurer SQL Server** boîte de dialogue s’affiche. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Acceptez les valeurs par défaut ou entrez des valeurs différentes. Entrez un **Adminstrator Username** et **mot de passe administrateur** pour votre nouvelle base de données. Sélectionnez **OK** lorsque vous avez terminé. Le **créer App Service** page s’affiche de nouveau.

5. Sélectionnez **créer** pour créer votre profil. Un message s’affiche dans le coin inférieur droit, indiquant que le déploiement est en cours. Après quelques instants, le **publier** fenêtre réapparaît.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Le profil que vous avez créé pour déployer l’application est désormais disponible. 


> [!div class="step-by-step"]
> [Next](part-2.md)
