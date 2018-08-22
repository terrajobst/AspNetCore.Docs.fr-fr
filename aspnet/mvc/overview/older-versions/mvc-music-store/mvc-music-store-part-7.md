---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Partie 7 : Appartenance et autorisation | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 7 couvre l’appartenance et l’autorisation.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 44205f0ef59e00ad9fb1c45fdc0ba8934b5804cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830147"
---
<a name="part-7-membership-and-authorization"></a>Partie 7 : Appartenance et autorisation
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 7 couvre l’appartenance et l’autorisation.


Notre contrôleur Store Manager est actuellement accessible à toute personne visitant notre site. Nous allons modifier cette option pour restreindre les autorisations aux administrateurs du site.

## <a name="adding-the-accountcontroller-and-views"></a>Ajout du contrôle AccountController et vues

Une différence entre le modèle d’Application Web de ASP.NET MVC 3 complet et le modèle MVC 3 Application Web ASP.NET vide est que le modèle vide n’inclut pas un contrôleur de compte. Nous allons ajouter un contrôleur de compte en copiant plusieurs fichiers à partir d’une nouvelle application ASP.NET MVC créée à partir du modèle d’Application Web de ASP.NET MVC 3 complet.

Créez une application ASP.NET MVC en utilisant le modèle d’Application Web de ASP.NET MVC 3 complète et copiez les fichiers suivants dans les mêmes répertoires dans notre projet :

1. AccountController.cs de copie dans le répertoire Controllers
2. AccountModels de copie dans le répertoire de modèles
3. Créer un répertoire de compte dans le répertoire Views et copie toutes les quatre vues dans

Modifier l’espace de noms pour les classes de contrôleur et le modèle afin qu’ils commencent par MvcMusicStore. La classe AccountController doit utiliser l’espace de noms MvcMusicStore.Controllers, et la classe AccountModels doit utiliser l’espace de noms MvcMusicStore.Models.

*Remarque : Ces fichiers sont également disponibles dans le téléchargement de MvcMusicStore-Assets.zip à partir duquel nous copié les fichiers de conception de notre site au début du didacticiel. Les fichiers d’appartenance sont situés dans le répertoire de Code.*

La solution de mise à jour doit se présenter comme suit :

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Ajout d’un utilisateur administratif avec le site de Configuration ASP.NET

Avant que nous avons besoin d’autorisation dans notre site Web, nous allons devoir créer un utilisateur avec accès. Le moyen le plus simple de créer un utilisateur consiste à utiliser le site Web de Configuration ASP.NET intégré.

Lancer le site Web ASP.NET Configuration en cliquant sur Suivant l’icône dans l’Explorateur de solutions.

![](mvc-music-store-part-7/_static/image2.png)

Cette opération lance un site Web de configuration. Cliquez sur l’onglet sécurité sur l’écran d’accueil, puis cliquez sur le lien « Activer les rôles » dans le centre de l’écran.

![](mvc-music-store-part-7/_static/image3.png)

Cliquez sur le lien « Créer ou gérer des rôles ».

![](mvc-music-store-part-7/_static/image4.png)

Entrez le nom de rôle « Administrateur » et appuyez sur le bouton Ajouter un rôle.

![](mvc-music-store-part-7/_static/image5.png)

Cliquez sur le bouton précédent, puis cliquez sur le lien d’utilisateur de créer sur le côté gauche.

![](mvc-music-store-part-7/_static/image6.png)

Renseignez les champs des informations sur la gauche en utilisant les informations suivantes :

| **Champ** | **Valeur** |
| --- | --- |
| **Nom d'utilisateur** | Administrateur |
| **Mot de passe** | password123 ! |
| **Confirmer le mot de passe** | password123 ! |
| **Message électronique** | (n’importe quelle adresse de messagerie fonctionneront) |
| **Question de sécurité** | (comme vous le souhaitez) |
| **Réponse de sécurité** | (comme vous le souhaitez) |

*Remarque : Vous pouvez évidemment utiliser n’importe quel mot de passe que vous souhaitez. Les paramètres de sécurité de mot de passe par défaut exigent un mot de passe qui est de 7 caractères et qui contient un caractère non alphanumérique.*

Sélectionnez le rôle d’administrateur pour cet utilisateur, puis cliquez sur le bouton Créer un utilisateur.

![](mvc-music-store-part-7/_static/image7.png)

À ce stade, vous devez voir un message indiquant que l’utilisateur a été créé avec succès.

![](mvc-music-store-part-7/_static/image8.png)

Vous pouvez maintenant fermer la fenêtre du navigateur.

## <a name="role-based-authorization"></a>Autorisation basée sur un rôle

Nous pouvons désormais restreindre l’accès à StoreManagerController à l’aide de l’attribut [Authorize], qui spécifie que l’utilisateur doit être dans le rôle d’administrateur pour accéder à toute action de contrôleur dans la classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Remarque : L’attribut [Authorize] peut être placé sur les méthodes d’action spécifique, ainsi qu’au niveau de la classe de contrôleur.*

Accédez maintenant à /StoreManager fait apparaître une boîte de dialogue Ouverture de session :

![](mvc-music-store-part-7/_static/image9.png)

Après l’ouverture de session avec notre nouveau compte d’administrateur, nous sommes en mesure d’accéder à l’écran Modifier Album comme avant.

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-6.md)
> [Suivant](mvc-music-store-part-8.md)
