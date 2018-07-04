---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Partie 4 : Accès aux données et des modèles | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 4 couvre l’accès aux données et modèles.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402036"
---
<a name="part-4-models-and-data-access"></a>Partie 4 : Modèles et accès aux données
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.
> 
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. Partie 4 couvre l’accès aux données et modèles.


Jusqu’ici, nous avons simplement été passant « données factices » à partir de nos contrôleurs à nos modèles de vue. Nous sommes maintenant prêts à raccorder une véritable base de données. Dans ce didacticiel présente l’utilisation de SQL Server Compact Edition (souvent appelée SQL CE) en tant que notre moteur de base de données. SQL CE est une base de données de fichier gratuite et intégrée, en ne nécessitant pas installation ni configuration, ce qui facilite réellement la pour un développement local.

## <a name="database-access-with-entity-framework-code-first"></a>Accès de base de données avec le Code First Entity Framework

Nous allons utiliser la prise en charge Entity Framework (EF) qui est inclus dans les projets ASP.NET MVC 3 pour interroger et mettre à jour de la base de données. EF est un objet flexible relationnel de mappage des données (ORM) API qui permet aux développeurs d’interroger et mise à jour des données stockées dans une base de données d’une manière orientée objet.

Entity Framework version 4 prend en charge un paradigme de développement appelé code first. Code first vous permet de créer un objet de modèle en écrivant des classes simples (appelé aussi POCO à partir de « brut » objets CLR) et pouvez même créer la base de données à la volée à partir de vos classes.

### <a name="changes-to-our-model-classes"></a>Modifications apportées à nos Classes de modèle

Dans ce didacticiel, nous va exploiter la fonctionnalité de création de base de données dans Entity Framework. Avant cela, cependant, nous allons apporter quelques modifications mineures à nos classes de modèle à ajouter dans certaines choses que nous utiliserons plus tard.

#### <a name="adding-the-artist-model-classes"></a>Ajout des Classes de modèle artiste

Notre Albums sera associés à des artistes, donc nous allons ajouter une classe de modèle simple pour décrire un artiste. Ajoutez une nouvelle classe dans le dossier de modèles nommé Artist.cs en utilisant le code indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>La mise à jour nos Classes de modèle

Mettre à jour la classe Album comme indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Ensuite, vérifiez les mises à jour suivantes à la classe de Genre.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Ajout de l’application\_dossier de données

Nous allons ajouter une application\_répertoire de données à notre projet pour stocker nos fichiers de base de données SQL Server Express. Application\_données sont un répertoire spécial dans ASP.NET qui possède déjà les autorisations d’accès de sécurité correct pour l’accès de base de données. Dans le menu projet, sélectionnez Ajouter le dossier ASP.NET, puis application\_données.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Création d’une chaîne de connexion dans le fichier web.config

Nous allons ajouter quelques lignes au fichier de configuration du site Web afin qu’Entity Framework sache comment vous connecter à notre base de données. Double-cliquez sur le fichier Web.config situé à la racine du projet.

![](mvc-music-store-part-4/_static/image2.png)

Faites défiler vers le bas de ce fichier et ajoutez un &lt;connectionStrings&gt; section directement au-dessus de la dernière ligne, comme indiqué ci-dessous.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Ajout d’une classe de contexte

Cliquez sur le dossier de modèles et ajoutez une nouvelle classe nommée MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Cette classe sera représentent le contexte de base de données Entity Framework et sera gérer notre créer, lire, mettre à jour et supprimer les opérations pour nous. Le code de cette classe est indiqué ci-dessous.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Et voilà, il n’existe aucune autre configuration, des interfaces spéciales, etc. En étendant la classe de base DbContext, notre classe MusicStoreEntities est en mesure de gérer nos opérations de base de données pour nous. Maintenant que nous avons raccordé, nous allons ajouter quelques propriétés supplémentaires pour nos classes de modèle pour tirer parti de certaines des informations supplémentaires dans notre base de données.

### <a name="adding-our-store-catalog-data"></a>Ajout de nos données de catalogue du magasin

Nous tireront parti d’une fonctionnalité dans Entity Framework qui ajoute des données de « valeur initiale » à une base de données nouvellement créé. Cela est prédéfinie notre catalogue du magasin avec une liste de Genres, les artistes et les Albums. Le téléchargement du MvcMusicStore-Assets.zip - inclus nos fichiers de conception de site utilisés précédemment dans ce didacticiel - dispose d’un fichier de classe avec ces données de valeur initiale, situés dans un dossier nommé Code.

Dans le Code / dossier Modèles, recherchez le fichier SampleData.cs et déposez-le dans le dossier de modèles dans notre projet, comme indiqué ci-dessous.

![](mvc-music-store-part-4/_static/image4.png)

Nous devons maintenant ajouter une ligne de code pour indiquer à Entity Framework relatives à cette classe SampleData. Double-cliquez sur le fichier Global.asax à la racine du projet pour l’ouvrir et ajoutez la ligne suivante vers le haut l’Application\_Start (méthode).

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

À ce stade, nous avons terminé le travail nécessaire pour configurer notre projet Entity Framework.

## <a name="querying-the-database"></a>Interrogation de la base de données

Maintenant nous allons mettre à jour notre StoreController afin qu’au lieu d’utiliser « factice de données » il appelle à la place dans notre base de données pour interroger toutes ses informations. Nous allons commencer par déclarer un champ sur le **StoreController** devant contenir une instance de la classe MusicStoreEntities, nommée storeDB :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>La mise à jour de l’Index de Store pour interroger la base de données

La classe MusicStoreEntities est gérée par Entity Framework et expose une propriété de collection pour chaque table dans notre base de données. Nous allons mettre à jour action sur l’Index de notre StoreController pour récupérer tous les Genres dans notre base de données. Précédemment cela, nous avons coder en dur les données de chaîne. Nous pouvons maintenant à la place simplement utiliser le contexte Entity Framework Generes collection :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Aucune modification ne doit se produire pour notre modèle de vue dans la mesure où nous renvoyons toujours le même StoreIndexViewModel retourné avant - nous avons retourne simplement les données en direct à partir de notre base de données maintenant.

Lorsque nous exécuter à nouveau le projet et accédez à l’URL « / Store », nous verrons maintenant une liste de tous les Genres dans notre base de données :

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>La mise à jour Store Parcourir et détails à utiliser des données actives

Avec/Store/Parcourir ? genre =*[certains genre]* méthode d’action, nous recherchons un Genre par nom. Nous nous attendons uniquement un seul résultat, étant donné que nous ne devrions pas jamais avoir deux entrées pour le nom du même Genre, et donc nous pouvons utiliser le. Extension de Single() dans LINQ pour interroger l’objet de Genre approprié comme suit (ne tapez pas cela encore) :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

La méthode unique prend une expression Lambda en tant que paramètre, qui spécifie que nous voulons un seul objet de Genre de telle sorte que son nom correspond à la valeur que nous avons défini. Dans le cas ci-dessus, nous chargeons un objet de Genre unique avec une valeur de nom correspondant Disco.

Nous allons tirer parti d’une fonctionnalité Entity Framework qui nous permet d’indiquer d’autres entités connexes que nous voulons également chargés lors de l’objet de Genre est récupéré. Cette fonctionnalité est appelée mise en forme du résultat de requête et nous permet de réduire le nombre de fois où que nous devons accéder à la base de données pour récupérer toutes les informations que nous avons besoin. Nous souhaitons Prérécupérer des Albums de Genre nous récupérons, donc nous allons mettre à jour notre requête à inclure à partir de Genres.Include("Albums") pour indiquer que nous voulons également les albums connexes. Cela est plus efficace, dans la mesure où il récupérera notre Genre et l’Album des données dans une requête de base de données unique.

Avec les explications disparaître, voici à quoi ressemble notre action de contrôleur Parcourir mis à jour :

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Nous pouvons maintenant mettre à jour le Store parcourir la vue pour afficher les albums qui sont disponibles dans chaque Genre. Ouvrez le modèle de vue (/Views/Store/Browse.cshtml trouvé dans) et ajoutez une liste à puces d’Albums, comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Notre application en cours d’exécution et en accédant à/Store/Parcourir ? genre = montre de Jazz que nos résultats sont maintenant chargés à partir de la base de données, affichant tous les albums dans notre Genre sélectionné.

![](mvc-music-store-part-4/_static/image2.jpg)

Nous allons rendre les mêmes remplacez notre /Store/détails / [id] URL, puis remplacer nos données factices par une requête de base de données qui charge un Album dont l’ID correspond à la valeur du paramètre.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Notre application en cours d’exécution et en accédant à /Store/Details/1 montre que nos résultats sont maintenant chargés à partir de la base de données.

![](mvc-music-store-part-4/_static/image5.png)

Maintenant que notre page de détails de Store est configuré pour afficher un album par l’ID de l’Album, mettons à jour le **Parcourir** vue à lier à la vue de détails. Nous allons utiliser Html.ActionLink, exactement comme nous l’avons fait pour créer un lien à partir de l’Index de Store pour parcourir de Store à la fin de la section précédente. La source complète pour le mode de navigation apparaît ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Nous sommes maintenant en mesure d’accéder à partir de notre page Store à une page de Genre, qui répertorie les albums disponibles, et en cliquant sur un album nous pouvons afficher les détails de cet album.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-3.md)
> [Suivant](mvc-music-store-part-5.md)
