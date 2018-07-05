---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Partie 1 : Fichier -> Nouveau projet | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 1 couvre la vue d’ensemble et fichier/nouveau projet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 8c346e88277b6cf43676f7c6b58f9679a591d634
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388209"
---
<a name="part-1-file--new-project"></a>Partie 1 : Fichier -> Nouveau projet
====================
par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 1 couvre la vue d’ensemble et fichier/nouveau projet.


## <a id="_Toc260221666"></a>  Vue d’ensemble

Ce didacticiel est une introduction à Web Forms ASP.NET. Nous allons commencer lentement, afin de l’expérience de développement de niveau web pour débutants OK.

L’application que nous allons créer est un magasin en ligne simple.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Les visiteurs peuvent parcourir les produits par catégorie :

![](tailspin-spyworks-part-1/_static/image2.jpg)

Ils peuvent afficher un produit unique et l’ajouter à leur panier d’achat :

![](tailspin-spyworks-part-1/_static/image3.jpg)

Ils peuvent consulter leur panier, supprimer des éléments qu’ils ne voulez plus de :

![](tailspin-spyworks-part-1/_static/image4.jpg)

Continuer à extraire les invite à

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Après avoir commandé, ils voient un écran de confirmation simple :

![](tailspin-spyworks-part-1/_static/image7.jpg)


Nous allons commencer en créant un nouveau projet Web Forms ASP.NET dans Visual Studio 2010, et nous ajouterons progressivement des fonctionnalités pour créer une application fonctionnelle complète. Tout au long du processus, nous traiterons accès de base de données, les affichages de listes et de la grille, pages de mise à jour de données, validation des données, à l’aide de pages maîtres pour la mise en page cohérente, AJAX, validation, l’appartenance utilisateur et bien plus encore.

Vous pouvez suivre la procédure étape par étape, ou vous pouvez télécharger l’application terminée à partir de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Vous pouvez utiliser Visual Studio 2010 ou le gratuit Visual Web Developer 2010 à partir de [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Pour générer l’application, vous pouvez utiliser SQL Server ou gratuit SQL Server Express pour héberger la base de données.

## <a id="_Toc260221667"></a>  Fichier / nouveau projet

Nous allons commencer en sélectionnant le nouveau projet dans le menu fichier dans Visual Studio. Ceci fait apparaître la boîte de dialogue Nouveau projet.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Nous allons sélectionner Visual C# / modèles Web regrouper sur la gauche, puis choisissez le modèle « ASP.NET Web Application » dans la colonne centrale. Nommez votre projet TailspinSpyworks et appuyez sur le bouton OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Cela crée notre projet. Jetons un œil au niveau des dossiers qui sont inclus dans notre application dans l’Explorateur de solutions sur le côté droit.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La Solution vide n’est pas complètement vide, il ajoute une structure de dossiers de base :

![](tailspin-spyworks-part-1/_static/image1.png)

Notez les conventions implémentées par le modèle de projet ASP.NET 4 par défaut.

- Le dossier « Compte » implémente une interface utilisateur de base pour ASP. Sous-système de l’appartenance du NET.
- Le dossier « Scripts » sert le référentiel pour les fichiers de JavaScript côté client et les fichiers .js de jQuery core sont disponibles par défaut.
- Le dossier « Styles » est utilisé pour organiser notre site web des éléments visuels (feuilles de Style CSS)

Lorsque vous appuyez sur F5 pour exécuter notre application et de restituer la page default.aspx, nous voyons ce qui suit.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Notre première amélioration d’application sera pour remplacer le fichier Style.css à partir du modèle de Web Forms par défaut avec les classes CSS et les fichiers d’image associée qui restituera l’asthetics visual que nous souhaitons pour notre application Tailspin Spyworks.

Après cela, notre page default.aspx affiche sous cette forme.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Notez que les liens de l’image dans la partie supérieure droite de la page et les éléments de menu qui ont été ajoutés à la page maître. Uniquement les liens de « connexion » et « Compte » pointent vers les pages qui existent (générés par le modèle par défaut) et le reste des pages, nous allons implémenter que nous construisons notre application.

Nous allons également déplacer la Page maître dans le répertoire de Styles. Bien que ce soit uniquement une préférence il peut rendre les choses un peu plus facile si nous décidons pour que notre application « personnalisable » à l’avenir.

Une fois cela que nous devons modifier la page maître références dans tous les fichiers .aspx générés par la valeur par défaut des pages Web Forms ASP.NET.

> [!div class="step-by-step"]
> [Next](tailspin-spyworks-part-2.md)
