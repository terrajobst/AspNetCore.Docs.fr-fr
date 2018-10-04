---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: À l’aide de Web API 2 avec Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel vous apprend aux principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end. Ce didacticiel utilise Entity Framework 6 pour la disposition de données...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: d65c0ea35ec766ef9d9093c6502230f9de72a3f3
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795208"
---
<a name="using-web-api-2-with-entity-framework-6"></a>À l’aide de Web API 2 avec Entity Framework 6
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

> Ce didacticiel vous apprend aux principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end. Le didacticiel utilise Entity Framework 6 de la couche de données et Knockout.js pour l’application de JavaScript côté client. Le didacticiel montre également comment déployer l’application dans Azure App Service Web Apps.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
> - Web API 2.1
> - Visual Studio 2013 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
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

Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ? Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas déjà un compte, vous disposez des options suivantes :

- [Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

## <a name="create-the-project"></a>Créer le projet

Ouvrez Visual Studio. À partir de la **fichier** menu, sélectionnez **New**, puis sélectionnez **projet**. (Ou cliquez sur **nouveau projet** sur la page de démarrage.)

Dans le **nouveau projet** boîte de dialogue, cliquez sur **Web** dans le volet gauche et **Application Web ASP.NET** dans le volet central. Nommez le projet BookService et cliquez sur **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **API Web** modèle.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Si vous souhaitez héberger le projet dans un Azure App Service, laissez le **hôte dans le cloud** case cochée.

Cliquez sur **OK** pour créer le projet.

## <a name="configure-azure-settings-optional"></a>Configurer les paramètres Azure (facultatifs)

Si vous avez laissé le **hôte dans le Cloud** option activée, Visual Studio vous invite à vous connecter à Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Après que vous être connecté à Azure, Visual Studio vous invite à configurer l’application web. Entrez un nom pour le site, sélectionnez votre abonnement Azure, puis une région géographique. Sous **serveur de base de données**, sélectionnez **créer un serveur**. Entrez un nom d’utilisateur administrateur et le mot de passe.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Next](part-2.md)
