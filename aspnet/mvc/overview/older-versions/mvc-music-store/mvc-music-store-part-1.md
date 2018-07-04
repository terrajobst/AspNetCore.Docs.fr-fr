---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Partie 1 : Vue d’ensemble et fichier -> Nouveau projet | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 1 traite de vue d’ensemble et fichier -> Nouveau projet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: c03b62db2227c167c68ca5cf8174e4322658d39d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377887"
---
<a name="part-1-overview-and-file-new-project"></a>Partie 1 : Vue d’ensemble et fichier -> Nouveau projet
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 1 traite de vue d’ensemble et fichier -&gt;nouveau projet.


## <a name="overview"></a>Vue d'ensemble

Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Web Developer pour le développement web. Nous allons commencer lentement, afin de l’expérience de développement de niveau web pour débutants OK.

L’application que nous allons créer est un magasin de musique simple. Il existe trois parties principales à l’application : shopping, extraction et administration.

![](mvc-music-store-part-1/_static/image1.jpg)

Les visiteurs peuvent parcourir les Albums par Genre :

![](mvc-music-store-part-1/_static/image2.jpg)

Ils peuvent afficher un album unique et l’ajouter à leur panier d’achat :

![](mvc-music-store-part-1/_static/image3.jpg)

Ils peuvent consulter leur panier, supprimer des éléments qu’ils ne voulez plus de :

![](mvc-music-store-part-1/_static/image4.jpg)

Procéder à la validation est alors invités à se connecter ou s’inscrire pour un compte d’utilisateur.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Après avoir créé un compte, il peut effectuer l’ordre en renseignant les informations d’expédition et de paiement. Pour simplifier les choses, nous exécutons une promotion incroyable : tout est gratuit s’il a entré le code de promotion « Gratuit » !

![](mvc-music-store-part-1/_static/image5.jpg)

Après avoir commandé, ils voient un écran de confirmation simple :

![](mvc-music-store-part-1/_static/image6.jpg)

En plus de pages faceing de client, nous allons également générer une section de l’administrateur qui affiche une liste d’albums à partir de laquelle les administrateurs peuvent créer, modifier et supprimer des albums :

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Fichier -&gt; nouveau projet

### <a name="installing-the-software"></a>Installation du logiciel

Ce didacticiel commence par créer un nouveau projet ASP.NET MVC 3 à l’aide de la gratuit Visual Web Developer 2010 Express (qui est gratuit), puis nous allons ajouter progressivement des fonctionnalités pour créer une application fonctionnelle complète. Tout au long du processus, nous traiterons accès de base de données, les scénarios de validation de formulaire, validation des données, à l’aide de pages maîtres pour la mise en page cohérente, à l’aide d’AJAX pour les mises à jour de la page et la validation, la connexion de l’utilisateur et bien plus encore.

Vous pouvez suivre la procédure étape par étape, ou vous pouvez télécharger l’application terminée à partir de [MVC-musique-Store](https://github.com/evilDave/MVC-Music-Store).

Vous pouvez utiliser Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (une version gratuite de Visual Studio 2010) pour générer l’application. Nous allons utiliser SQL Server Compact (également gratuit) pour héberger la base de données. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous.


- [Configuration requise de visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0 -], y compris la prise en charge runtime et outils


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Création d’un projet ASP.NET MVC 3

Nous allons commencer en sélectionnant « Nouveau projet » dans le menu fichier dans Visual Web Developer. Ceci fait apparaître la boîte de dialogue Nouveau projet.

![](mvc-music-store-part-1/_static/image5.png)

Nous allons sélectionner le Visual c# -&gt; modèles Web regrouper sur la gauche, puis choisissez le modèle « Application Web ASP.NET MVC 3 » dans la colonne centrale. Nommez votre projet MvcMusicStore et appuyez sur le bouton OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Ceci affichera une boîte de dialogue secondaire qui permet de définir des paramètres spécifiques de MVC pour notre projet. Sélectionnez les éléments suivants :

Modèle de projet - Sélectionnez vide

Moteur d’affichage : sélectionnez Razor

Utiliser des balises sémantiques HTML5 - activée

Vérifiez que vos paramètres sont indiquées ci-dessous, puis appuyez sur le bouton OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Cela crée notre projet. Jetons un œil au niveau des dossiers qui ont été ajoutés à notre application dans l’Explorateur de solutions sur le côté droit.

![](mvc-music-store-part-1/_static/image10.jpg)

Le modèle vide MVC 3 n’est pas complètement vide, il ajoute une structure de dossiers de base :

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC utilise certaines conventions d’affectation de noms de base pour les noms de dossier :

| **Dossier** | **Fonction** |
| --- | --- |
| **/ Contrôleurs** | Contrôleurs de répondent à d’entrée à partir du navigateur, de décider quoi faire avec elle et retourner une réponse à l’utilisateur. |
| **/Views** | Vues de contiennent nos modèles d’interface utilisateur |
| **/ Modèles** | Les modèles contiennent et manipulent des données |
| **/ Contenu** | Ce dossier conserve nos images, CSS et tout autre contenu statique |
| **/ Scripts** | Ce dossier conserve les fichiers JavaScript |

Ces dossiers sont inclus même dans une application ASP.NET MVC vide, car l’infrastructure ASP.NET MVC par défaut utilise une approche « convention sur configuration » et émet des hypothèses par défaut selon les conventions d’affectation de noms de dossier. Par exemple, contrôleurs recherchent les vues dans le dossier vues par défaut sans avoir à spécifier explicitement ceci dans votre code. Continue à utiliser les conventions par défaut réduit la quantité de code que vous avez besoin pour écrire, et peut également faciliter à d’autres développeurs à comprendre votre projet. Nous expliquerons ces conventions plus que nous construisons notre application.

> [!div class="step-by-step"]
> [Next](mvc-music-store-part-2.md)
