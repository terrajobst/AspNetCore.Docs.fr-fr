---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Ajout d’une colonne au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817199"
---
<a name="adding-a-column-to-the-model"></a>Ajout d’une colonne au modèle
====================
par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Dans cette section, nous allons présenter comment nous pouvons apporter des modifications au schéma de notre base de données et gérer les modifications au sein de notre application.

Nous allons ajouter une colonne « Évaluation » à la table Movie. Revenez à l’IDE, puis cliquez sur l’Explorateur de base de données. Cliquez avec le bouton droit sur la table Movie et sélectionnez Ouvrir la définition de Table.

Ajouter une colonne « Rating » comme indiqué ci-dessous. Étant donné que nous n’avons pas n’importe quel contrôle d’accès maintenant, la colonne peut accepter les valeurs NULL. Cliquez sur Enregistrer.

[![Modification de Table de films](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Ensuite, revenez à l’Explorateur de solutions et ouvrez le fichier Movies.edmx (qui se trouve dans le dossier \Models). Cliquez avec le bouton droit sur l’aire de conception (la zone blanche) et sélectionnez le modèle de mise à jour à partir de la base de données.

[![Films - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Cette action lance l’Assistant de mise à jour « ». Cliquez sur l’onglet de l’actualisation dans celui-ci et cliquez sur Terminer. Notre classe de modèle de film sera ensuite être mis à jour avec la nouvelle colonne.

![Assistant de mise à jour (2)](getting-started-with-mvc-part8/_static/image5.png)

Après avoir cliqué sur Terminer, vous pouvez voir que la nouvelle colonne de classement a été ajoutée à l’entité de film dans notre modèle.

[![Entité de film](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Nous avons ajouté une colonne dans le modèle de base de données, mais ne conscient pas les vues à son sujet.

## <a name="update-views-with-model-changes"></a>Vues de mise à jour avec les modifications apportées au modèle

Il existe plusieurs façons, nous pouvons mettre à jour nos modèles de vue pour refléter la nouvelle colonne de contrôle d’accès. Étant donné que nous avons créé ces vues en les générant par le biais de la boîte de dialogue Ajouter une vue, nous pourrions supprimez-les et recréez-les à nouveau. Toutefois, généralement personnes sera déjà modifications apportées à leurs modèles de vue à partir de la génération initiale généré automatiquement et souhaitez ajouter ou supprimer des champs manuellement, tout comme nous l’avons fait avec le champ ID pour créer.

Ouvrez le modèle \Views\Movies\Index.aspx et ajoutez un &lt;th&gt;évaluation&lt;/th&gt; au début de la table Movie. J’ai ajouté mes après Genre. Puis, dans la même position de colonne, mais plus bas, ajoutez une ligne pour notre nouvelle évaluation de la sortie.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Notre modèle Index.aspx final doit ressembler à ceci :

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Nous allons ouvrir le modèle \Views\Movies\Create.aspx, puis ajoutez une étiquette et une zone de texte pour notre nouvelle propriété de contrôle d’accès :

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Notre modèle Create.aspx final ressembler à ceci et nous allons modifier le titre et la base de données secondaire de notre navigateur &lt;h2&gt; titre à quelque chose comme « Créer un film » pendant que nous sommes ici !

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Exécutez votre application et que vous avez maintenant un nouveau champ dans la base de données qui a été ajouté à la page Create. Ajouter un nouveau film - cette fois avec une évaluation égale à - et cliquez sur Créer.

[![Créer un film - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Après avoir cliqué sur Créer, vous êtes envoyé à la page d’Index où vous nouveau film est répertorié avec la nouvelle colonne de classement dans la base de données

[![Liste de films - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Ce didacticiel de base a été de vous lancer dans la création de contrôleurs, leur association avec des vues et la transmission autour des données codées en dur. Puis nous avons créé et conçu une base de données et insérer des données dans. Nous avons extrait les données de la base de données et il affiché dans un tableau HTML. Ensuite, nous avons ajouté un formulaire de création qui permettent à l’utilisateur d’ajouter des données à la base de données eux-mêmes à partir de l’Application Web. Nous avons ajouté la validation, puis apportées à la validation d’utiliser JavaScript côté client. Enfin, nous modifié la base de données pour inclure une nouvelle colonne de données, puis mis à jour de nos deux pages pour créer et afficher ces nouvelles données.

J’ai maintenant vous encourageons à passer à notre didacticiel de niveau intermédiaire «[Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)», ainsi que les nombreuses vidéos et à vos ressources [ https://asp.net/mvc ](https://asp.net/mvc) pour en savoir plus sur ASP.NET MVC !

Bonne lecture !

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) et [ @shanselman ](http://twitter.com/shanselman) sur Twitter.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part7.md)
