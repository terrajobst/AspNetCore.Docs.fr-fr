---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Création d’une base de données | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: fff974edfaffeb164879c10b8e70bd3482c47554
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813758"
---
<a name="creating-a-database"></a>Création d’une base de données
====================
par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Dans cette section, nous allons créer une nouvelle base de SQL Express que nous allons utiliser pour stocker et récupérer de nos données de film. Dans l’IDE Visual Web Developer, sélectionnez Affichage | Explorateur de serveurs. Cliquez avec le bouton droit sur connexions de données et cliquez sur Ajouter une connexion...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Dans la boîte de dialogue Choisir la Source de données, sélectionnez Microsoft SQL Server et cliquez sur Continuer.

![](getting-started-with-mvc-part4/_static/image2.png)

Dans la boîte de dialogue Ajouter une connexion, entrez «. \SQLEXPRESS » pour le nom de votre serveur, puis entrez « Movies » comme nom pour votre nouvelle base de données.

[![Ajouter la boîte de dialogue Connexion](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Cliquez sur OK et vous demandera si vous souhaitez créer cette base de données. Sélectionnez Oui.

[![Créez des films ?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Vous avez maintenant une base de données vide dans l’Explorateur de serveurs.

![Ajouter une nouvelle Table](getting-started-with-mvc-part4/_static/image7.png)

Cliquez avec le bouton droit sur les Tables et cliquez sur Ajouter une Table. Le Concepteur de tables s’affiche. Ajouter des colonnes pour l’Id, Title, ReleaseDate, Genre et prix. Cliquez avec le bouton droit sur la colonne d’ID et cliquez sur définie la clé primaire. Voici quelles mes zones de conception ressemble.

[![Éditeur de Table de base de données](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

En outre, sélectionnez la colonne d’Id et sous propriétés de colonne ci-dessous, remplacez « Spécification d’identité » sur « Oui ».

[![IsIdentity - propriétés des colonnes](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Lorsque c’est terminé, cliquez sur l’icône Enregistrer dans la barre d’outils ou sélectionnez fichier | Enregistrer dans le menu et nommez votre table «**film**» (singulier). Nous avons une base de données et une table !

[![Choisissez le nom](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Revenez à l’Explorateur de serveurs et cliquez avec le bouton droit sur la table Movie, puis sélectionnez « Afficher des données de Table ». Entrez quelques films par conséquent, notre base de données a des données.

[![Modification de la Table de base de données](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Création d’un modèle

Maintenant, revenez à l’Explorateur de solutions sur le côté droit de l’IDE et avec le bouton droit sur le dossier de modèles et sélectionnez Ajouter | Nouvel élément.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Nous allons créer un modèle d’entité à partir de notre nouvelle base de données. Cette opération ajoute un ensemble de classes à notre projet qui facilite pour nous interroger et manipuler les données au sein de notre base de données. Sélectionnez le nœud de données sur le côté gauche de la boîte de dialogue, puis sélectionnez le modèle d’élément ADO.NET Entity Data Model. Nommez-le Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Cliquez sur le bouton « Ajouter ». Cela lance le « Assistant EDM ».

Dans la nouvelle boîte de dialogue qui s’affiche, sélectionnez Générer à partir de la base de données. Étant donné que nous avons simplement une base de données, nous devons uniquement indiquer à Entity Framework sur notre nouvelle base de données et sa table. Cliquez sur Suivant pour enregistrer notre connexion de base de données dans la configuration de notre application web. Maintenant, vérifier les Tables et les films case à cocher et cliquez sur Terminer.

[![Assistant Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Maintenant, nous pouvons voir notre nouvelle table Movie dans Entity Framework Designer et y accéder à partir du code.

[![Films - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Sur l’aire de conception, vous pouvez voir une classe « Film ». Cette classe correspond à la table « Movie » dans notre base de données, et chaque propriété qu’il contient est mappée à une colonne avec la table. Chaque instance d’une classe « Film » correspond à une ligne dans la table « Movie ».

Si vous n’aimez pas la dénomination par défaut et le mappage des conventions utilisées par Entity Framework, vous pouvez utiliser le Concepteur d’Entity Framework pour modifier ou de les personnaliser. Pour cette application, nous allons utiliser les valeurs par défaut et enregistrez simplement le fichier en tant que-est.

Maintenant, nous allons travailler avec des données réelles !

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part3.md)
> [Suivant](getting-started-with-mvc-part5.md)
