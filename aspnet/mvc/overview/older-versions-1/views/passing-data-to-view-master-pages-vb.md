---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Passage de données pour afficher les Pages maîtres (VB) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est d’expliquer comment vous pouvez passer des données à partir d’un contrôleur à une page maître de vue. Nous examinons les deux stratégies pour passer des données à une vue m...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 2daab1e8596035c1a70fb0f86ba752837d468ef2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822242"
---
<a name="passing-data-to-view-master-pages-vb"></a>Passage de données à afficher les Pages maîtres (VB)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> L’objectif de ce didacticiel est d’expliquer comment vous pouvez passer des données à partir d’un contrôleur à une page maître de vue. Nous allons examiner deux stratégies pour passer des données à une page maître de vue. Tout d’abord, nous abordons une solution facile qui résulte dans une application qui est difficile à gérer. Ensuite, nous examinons une bien meilleure solution qui nécessite un peu plus de travail initiale, mais les résultats dans une application beaucoup plus facile à gérer.


## <a name="passing-data-to-view-master-pages"></a>Passage de données à des Pages maîtres de vue

L’objectif de ce didacticiel est d’expliquer comment vous pouvez passer des données à partir d’un contrôleur à une page maître de vue. Nous allons examiner deux stratégies pour passer des données à une page maître de vue. Tout d’abord, nous abordons une solution facile qui résulte dans une application qui est difficile à gérer. Ensuite, nous examinons une bien meilleure solution qui nécessite un peu plus de travail initiale, mais les résultats dans une application beaucoup plus facile à gérer.

### <a name="the-problem"></a>Le problème

Imaginez que vous générez une application de base de données de films et que vous souhaitez afficher la liste des catégories de films sur chaque page dans votre application (voir Figure 1). En outre, imaginez que la liste des catégories de films est stockée dans une table de base de données. Dans ce cas, il serait judicieux pour récupérer les catégories à partir de la base de données et de restituer la liste des catégories de films dans une page maître de vue.


[![Affichage des catégories de films dans une page maître de vue](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figure 01**: affichage des catégories de films dans une page maître de vue ([cliquez pour afficher l’image en taille réelle](passing-data-to-view-master-pages-vb/_static/image3.png))


Voici le problème. Comment pour récupérer la liste des catégories de film dans la page maître ? Il est tentant d’appeler directement les méthodes de vos classes de modèle dans la page maître. En d’autres termes, il est tentant d’inclure le code de récupération des données à partir de la droite de la base de données dans votre page maître. Toutefois, en ignorant vos contrôleurs MVC pour accéder à la base de données risque de violer la séparation claire des préoccupations qui est un des principaux avantages de la création d’une application MVC.

Dans une application MVC, vous souhaitez que toutes les interactions entre les vues MVC et votre modèle MVC d’être gérés par vos contrôleurs MVC. Cette séparation des préoccupations des résultats dans une application plus facile à gérer, adaptable et testable.

Dans une application MVC, toutes les données passées à une vue, y compris une page maître de vue, doivent être passées à une vue par une action de contrôleur. En outre, les données doivent être passées en tirant parti des données d’affichage. Dans le reste de ce didacticiel, j’examine les deux méthodes de transmission des données de la vue à une page maître de vue.

### <a name="the-simple-solution"></a>La Solution Simple

Commençons par la solution la plus simple pour passer des données de la vue à partir d’un contrôleur à une page maître de vue. La solution la plus simple consiste à transmettre les données d’affichage pour la page maître dans chaque action du contrôleur.

Envisagez le contrôleur dans le Listing 1. Il expose deux actions nommées `Index()` et `Details()`. Le `Index()` méthode d’action retourne chaque film dans la table de base de données de films. Le `Details()` méthode d’action retourne chaque film dans une catégorie particulière de film.

**Liste 1 : `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Notez que les deux le `Index()` et le `Details()` actions ajoutent deux éléments pour afficher les données. Le `Index()` action ajoute deux clés : catégories et des films. La clé de catégories représente la liste des catégories de film affiché par la page maître de vue. La clé de films représente la liste de films affichées par la page de vue d’Index.

Le `Details()` action ajoute également deux clés nommée catégories et des films. La clé de catégories, représente une fois encore, la liste des catégories de film affiché par la page maître de vue. La clé de films représente la liste de films dans une catégorie particulière, affiché par la page de vue de détails (voir Figure 2).


[![La vue Détails](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figure 02**: afficher les détails ([cliquez pour afficher l’image en taille réelle](passing-data-to-view-master-pages-vb/_static/image6.png))


La vue Index est contenue dans le Listing 2. Il itère simplement la liste de films représenté par l’élément de films dans les données d’affichage.

**Listing 2 : `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

La page maître de vue est contenue dans le Listing 3. La page maître en mode effectue une itération et restitue toutes les catégories de film représentés par l’élément de catégories à partir des données de la vue.

**Liste 3 : `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Toutes les données est transmis à la vue et la page maître en mode Afficher les données. C’est la méthode correcte pour passer des données à la page maître.

Par conséquent, quel est le problème avec cette solution ? Le problème est que cette solution enfreint le principe DRY (ne vous répétez pas). Chaque action de contrôleur doit ajouter la même liste des catégories de films pour afficher les données. Des doublons de code dans votre application, votre application beaucoup plus difficile à gérer, de s’adapter et de modifier.

### <a name="the-good-solution"></a>La bonne Solution.

Dans cette section, nous examinons une solution alternative et préférable, pour passer des données à partir d’une action de contrôleur à une page maître de vue. Au lieu d’ajouter les catégories de films pour la page maître dans chaque action du contrôleur, nous ajoutons les catégories de films à afficher les données qu’une seule fois. Toutes les données d’affichage utilisées par la page maître de vue est ajoutée dans une Application controller.

La classe ApplicationController est contenue dans la liste 4.

La classe ApplicationController est contenue dans la liste 4.

**Liste 4 – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Voici trois choses que vous devriez remarquer que sur le contrôleur de l’Application sur la liste 4. Tout d’abord, notez que la classe hérite de la classe de base System.Web.Mvc.Controller. Le contrôleur de l’Application est une classe de contrôleur.

En second lieu, notez que la classe de contrôleur d’Application est une classe MustInherit. Une classe MustInherit est une classe qui doit être implémentée par une classe concrète. Étant donné que le contrôleur de l’Application est une classe MustInherit, vous ne peut pas pas appeler les méthodes définies dans la classe directement. Si vous essayez d’appeler directement de la classe d’Application vous obtiendrez un message d’erreur ressource est introuvable.

Troisièmement, notez que le contrôleur de l’Application contient un constructeur qui ajoute la liste des catégories de films pour afficher les données. Chaque classe de contrôleur qui hérite de contrôleur d’Application appelle automatiquement constructeur du contrôleur de l’Application. Chaque fois que vous appelez une action sur n’importe quel contrôleur qui hérite de l’Application controller, les catégories de films est automatiquement inclus dans les données d’affichage.

Le contrôleur de films sur la liste 5 hérite le contrôleur de l’Application.

**Liste 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Le contrôleur de films, tout comme le contrôleur Home abordé dans la section précédente, expose deux méthodes d’action nommées `Index()` et `Details()`. Notez que la liste des catégories de film affiché par la page maître de vue n’est pas ajouté à afficher des données dans le `Index()` ou `Details()` (méthode). Étant donné que le contrôleur de films hérite de contrôleur d’Application, la liste des catégories de films est ajoutée pour afficher les données automatiquement.

Notez que cette solution à l’ajout de données d’affichage pour une page maître de vue ne viole pas le principe DRY (ne vous répétez pas). Le code pour l’ajout de la liste des catégories de films pour afficher des données est contenu dans un seul emplacement : le constructeur pour le contrôleur de l’Application.

### <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons abordé les deux approches pour passer des données de la vue à partir d’un contrôleur à une page maître de vue. Tout d’abord, nous avons examiné une simple, mais difficile à maintenir l’approche. Dans la première section, nous avons abordé comment vous pouvez ajouter des données d’affichage pour une page maître de vue dans chaque chaque action du contrôleur dans votre application. Nous avons conclu que c’était une approche incorrecte, car elle enfreint le principe DRY (ne vous répétez pas).

Ensuite, nous avons examiné une bien meilleure stratégie pour l’ajout de données requises par une page maître de vue pour afficher les données. Au lieu d’ajouter les données d’affichage dans chaque action du contrôleur, nous avons ajouté les données d’affichage qu’une seule fois au sein d’un contrôleur d’Application. De cette façon, vous pouvez éviter code en double lors du passage des données à une page maître de vue dans une application ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](creating-page-layouts-with-view-master-pages-vb.md)
