---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Partie 1 : Vue d’ensemble et création du projet | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0e4021402e8deccd2395f23b6b512679b5e9d281
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826592"
---
<a name="part-1-overview-and-creating-the-project"></a>Partie 1 : Vue d’ensemble et création du projet
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework est une infrastructure de mappage objet/relationnel. Il mappe les objets de domaine dans votre code aux entités dans une base de données relationnelle. La plupart du temps, vous n’avez pas à vous soucier de la couche de base de données, car Entity Framework s’occupe de celui-ci pour vous. Votre code manipule les objets et les modifications sont rendues persistantes dans une base de données.

## <a name="about-the-tutorial"></a>À propos du didacticiel

Dans ce didacticiel, vous allez créer une application simple de magasin. Il existe deux parties principales à l’application. Les utilisateurs normaux peuvent afficher les produits et créer des commandes :

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Les administrateurs peuvent créer, supprimer ou modifier des produits :

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Vous allez apprendre des compétences

Voici ce que vous allez apprendre :

- Comment utiliser Entity Framework avec l’API Web ASP.NET.
- Comment utiliser knockout.js pour créer un interface utilisateur du client dynamique.
- Comment utiliser l’authentification par formulaire avec l’API Web pour authentifier les utilisateurs.

Bien que ce didacticiel est autonome, vous souhaiterez probablement lire avant de commencer les didacticiels suivants :

- [Votre première API Web ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Création d’une API Web qui prend en charge les opérations CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Une certaine connaissance [ASP.NET MVC](../../../../mvc/index.md) s’avère également utile.

## <a name="overview"></a>Vue d'ensemble

À un niveau élevé, voici l’architecture de l’application :

- ASP.NET MVC génère des pages HTML pour le client.
- API Web ASP.NET expose les opérations CRUD sur les données (products et orders).
- Entity Framework traduit les modèles c# utilisés par l’API Web en entités de base de données.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Le diagramme suivant illustre la façon dont les objets de domaine sont représentées de différentes couches de l’application : la couche de base de données, le modèle objet et enfin le format de câble, qui est utilisé pour transmettre des données au client via HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Vous pouvez créer le projet du didacticiel à l’aide de Visual Web Developer Express ou la version complète de Visual Studio.

À partir de la **Démarrer** , cliquez sur **nouveau projet**.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet « ProductStore » et cliquez sur **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **Application Internet** et cliquez sur **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Le modèle « Application Internet » crée une application ASP.NET MVC prenant en charge l’authentification par formulaire. Si vous exécutez l’application maintenant, il possède déjà certaines fonctionnalités :

- Nouveaux utilisateurs peuvent inscrire en cliquant sur le lien « S’inscrire » dans le coin supérieur droit.
- Les utilisateurs inscrits peuvent se connecter en cliquant sur le lien « Se connecter ».

Informations d’appartenance sont conservées dans une base de données qui est créé automatiquement. Pour plus d’informations sur l’authentification par formulaire dans ASP.NET MVC, consultez [procédure pas à pas : à l’aide de l’authentification par formulaire dans ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Mettre à jour le fichier CSS

Cette étape est cosmétique, mais cela rendra les pages de rendu telles que les captures d’écran précédentes.

Dans l’Explorateur de solutions, développez le dossier de contenu et d’ouvrir le fichier Site.css. Ajoutez les styles CSS suivants :

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Next](using-web-api-with-entity-framework-part-2.md)
