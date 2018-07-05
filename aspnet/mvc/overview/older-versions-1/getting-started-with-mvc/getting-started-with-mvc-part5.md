---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: L’accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 556eed77cbd9e81c0a2a1334bb0a8ee56abafd34
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814999"
---
<a name="accessing-your-models-data-from-a-controller"></a>L’accès aux données de votre modèle à partir d’un contrôleur
====================
par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Dans cette section, nous allons créer une nouvelle classe MoviesController, et écrire du code qui Récupère nos données de film et l’affiche dans le navigateur à l’aide d’un modèle de vue.

Cliquez avec le bouton droit sur le dossier Controllers et effectuer une nouvelle MoviesController.

[![Ajouter un contrôleur](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Cela créera un nouveau fichier « MoviesController.cs » en dessous de notre dossier \Controllers au sein de notre projet. Nous allons mettre à jour le MovieController pour récupérer la liste de films à partir de notre base de données nouvellement rempli.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Nous exécutons une requête LINQ, afin que nous récupérons uniquement films publiées après l’été 1984. Nous avons besoin d’un modèle de vue pour afficher cette liste de films de retour, par conséquent, avec le bouton droit dans la méthode et sélectionnez Ajouter une vue pour la créer.

Dans la boîte de dialogue Ajouter une vue, nous allons indiquer que nous passons une liste&lt;Movies.Models.Movie&gt; à notre modèle de vue. Contrairement aux fois précédentes nous utilisé la boîte de dialogue Ajouter une vue et que vous avez choisi de créer un modèle « Vide », cette fois, que nous allons indiquer que nous voulons Visual Studio automatiquement « structurer » un modèle de vue pour nous avec du contenu par défaut. Pour cela, nous allons sélectionner l’élément « List » dans le menu « contenu de liste déroulante de l’affichage.

N’oubliez pas, lorsque vous avez créé une nouvelle classe, vous aurez besoin pour compiler votre application pour qu’elle s’affiche dans la boîte de dialogue Vue ajouter.

![Ajouter une vue](getting-started-with-mvc-part5/_static/image3.png)

Cliquez sur Ajouter et le système génère automatiquement le code pour nous qui affiche notre liste de films pour une vue. Il s’agit d’un bon moment pour modifier le &lt;h2&gt; titre à quelque chose comme « My Movie List » comme nous l’avons fait précédemment avec la vue de Hello World.

[![Films - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Exécutez votre application et visitez /Movies dans la barre d’adresses. Maintenant, nous avons récupéré des données à partir de la base de données à l’aide d’une requête de base à l’intérieur du contrôleur et renvoyé les données à une vue qui connaît les films. Cette vue puis tourne via la liste de films et crée une table de données pour nous.

[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Nous n’implémentation modifier, détails et supprimer des fonctionnalités avec cette application -, ce qui nous évite les liens par défaut que le modèle de structure créé pour nous. Ouvrez le fichier /Movies/Index.aspx et les supprimer.

Voici le code source pour ce que notre modèle de vue mis à jour doit ressembler à une fois que nous apporter ces modifications :

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Il consiste à créer des liens que nous ne devons, donc nous allons les supprimer pour cet exemple. Nous conserverons notre créer un nouveau lien, car il s’agit suivant ! Voici à quoi ressemble notre application avec cette colonne supprimée.

[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Nous disposons désormais d’une simple liste de nos données de film. Toutefois, si nous cliquons sur le lien « Créer nouveau », nous obtenons une erreur car il n’est pas connecté ! Nous allons implémenter une méthode d’Action de créer et activer un utilisateur à entrer de nouveaux films dans notre base de données.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part4.md)
> [Suivant](getting-started-with-mvc-part6.md)
