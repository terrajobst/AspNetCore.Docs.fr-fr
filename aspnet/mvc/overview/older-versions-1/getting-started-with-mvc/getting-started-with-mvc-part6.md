---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Ajout d’une méthode de création et créer la vue | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: cf0d721b551c38e8c38e35f82b73ee1b14cd068f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840964"
---
<a name="adding-a-create-method-and-create-view"></a>Ajout d’une méthode de création et créer la vue
====================
par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Dans cette section, nous allons implémenter la prise en charge nécessaire pour permettre aux utilisateurs de créer de nouveaux films dans notre base de données. Pour cela, nous allons implémenter l’action de l’URL de films/Create.

Implémentation de l’URL de films/Créer est un processus en deux étapes. Lorsqu’un utilisateur visite tout d’abord l’URL de films/Créer nous voulons leur montrer un formulaire HTML qui peut remplir à entrer un nouveau film. Ensuite, lorsque l’utilisateur soumet le formulaire et les publications, les données dans le serveur, nous voulons récupérer le contenu publié et enregistrez-le dans notre base de données.

Nous allons implémenter ces deux étapes dans les deux méthodes Create() au sein de notre classe MoviesController. Une méthode affichera le &lt;formulaire&gt; que l’utilisateur doit renseigner pour créer un nouveau film. La deuxième méthode gère le traitement des données publiées lorsque l’utilisateur soumet le &lt;formulaire&gt; sur le serveur et enregistrer un nouveau film dans notre base de données.

Voici le code nous ajouterons à notre classe MoviesController pour implémenter cela :

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Le code ci-dessus contient tout le code qu’il nous faudrait au sein de notre contrôleur.

Nous allons maintenant implémenter le modèle Create View que nous allons utiliser pour afficher un formulaire à l’utilisateur. Nous allons cliquez avec le bouton droit dans la première méthode de création et sélectionnez « Ajouter une vue « pour créer le modèle de vue pour notre formulaire de film.

Nous allons sélectionner nous fera passer le modèle de vue une « séquence » en tant que classe de données d’affichage, et indiquer que nous voulons « structurer » un modèle « Créer ».

[![Ajouter une vue](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Après avoir cliqué sur le bouton Ajouter, modèle de vue \Movies\Create.aspx sera créé pour vous. Étant donné que nous avons sélectionné « Créer » dans la liste déroulante « Afficher le contenu », la boîte de dialogue Ajouter une vue « été généré automatiquement » du contenu par défaut pour nous. La génération de modèles automatique créé HTML &lt;formulaire&gt;, un emplacement pour l’erreur de validation des messages pour accéder et dans la mesure où la structure connaît des films, il créé étiquette et les champs pour chaque propriété de la classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Étant donné que notre base de données fournit automatiquement un ID d’un film, nous allons supprimer ces champs de ce modèle de référence. ID à partir de notre Create View. Supprimez les 7 lignes après &lt;légende&gt;champs&lt;/legend&gt; comme ils indiquent le champ ID que nous ne voulons pas.

Nous allons maintenant créer un nouveau film et l’ajouter à la base de données. Nous allons faire en exécutant à nouveau l’application et visitez le « / Movies » URL et cliquez sur la « créer » lien pour ajouter un nouveau film.

[![Créer - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Lorsque nous cliquons sur le bouton Créer, nous allons y publier (par HTTP POST) les données sur ce formulaire à la méthode /Movies/Create que nous venons de créer. Comme lorsque le système a pris le paramètre « numTimes » et « name » en dehors de l’URL et automatiquement les mappés à des paramètres sur une méthode précédemment, le système sera automatiquement prendre les champs de formulaire à partir d’une publication et les mapper à un objet. Dans ce cas, les valeurs des champs au format HTML tels que « ReleaseDate » et « Title » seront automatiquement placées dans les propriétés appropriées d’une nouvelle instance d’un film.

Nous allons examiner la deuxième méthode de création de notre MoviesController à nouveau. Remarquez comment il prend un objet « Vidéo » en tant qu’argument :

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Cet objet de film a été ensuite transmis vers la version de [HttpPost] de notre méthode d’action de création, et nous avons enregistré dans la base de données et puis l’utilisateur est redirigé vers la méthode d’action Index() qui affiche le résultat enregistré dans la liste de films :

[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Nous ne la vérification si notre films sont corrects, cependant, ainsi que la base de données ne nous permettre d’enregistrer un film avec aucun titre. Il serait intéressant que nous pourrions l’utilisateur qui a généré une erreur avant de la base de données. Nous allons faire ce qui suit en ajoutant la prise en charge de la validation à notre application.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part5.md)
> [Suivant](getting-started-with-mvc-part7.md)
