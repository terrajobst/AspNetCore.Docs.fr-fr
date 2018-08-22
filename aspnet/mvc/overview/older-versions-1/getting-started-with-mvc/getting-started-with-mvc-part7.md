---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Ajout de la Validation au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828954"
---
<a name="adding-validation-to-the-model"></a>Ajout de la Validation au modèle
====================
par [Scott Hanselman](https://github.com/shanselman)

> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Dans cette section, nous allons implémenter la prise en charge nécessaire pour activer la validation d’entrée au sein de notre application. Nous allons nous assurer que notre contenu de la base de données est toujours correct et fournir des messages d’erreur utiles aux utilisateurs finaux lorsqu’ils essayent et entrer des données de film qui ne sont pas valides. Nous allons commencer en ajoutant un peu de logique de validation à la classe Movie.

Cliquez avec le bouton droit sur le dossier de modèle et sélectionnez Ajouter une classe. Nommez votre film de la classe.

Lorsque nous avons créé le modèle d’entité film précédemment, l’IDE créé une classe de film. En fait, la partie de la classe de film peut être dans un fichier et partie dans un autre. Il s’agit d’une classe partielle. Nous allons étendre la classe Movie à partir d’un autre fichier.

Nous allons créer une classe partielle de film qui pointe vers une classe « buddy » avec certains attributs qui donnent des indicateurs de validation pour le système. Nous allons marquer le titre et le prix comme requis et également insistez pour que le prix est dans une certaine plage. Cliquez avec le bouton droit sur le dossier Modèles et sélectionnez Ajouter une classe. Nommer votre classe de film et cliquez sur le bouton OK. Voici à quoi notre partielle film classe ressemble.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Exécutez de nouveau votre application et essayez d’entrer un film avec un prix supérieures à 100. Vous obtiendrez une erreur lorsque vous avez envoyé le formulaire. L’erreur est détectée sur le côté serveur et se produit après que le formulaire est publié. Notez comment les HTML helpers intégrés d’ASP.NET MVC ont été suffisamment intelligents pour afficher le message d’erreur et conservez les valeurs pour nous dans les éléments de la zone de texte :

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Cela fonctionne très bien, mais il serait intéressant que nous pourrions l’utilisateur sur le côté client, immédiatement, avant que le serveur obtient impliqué.

Nous allons activer une validation côté client avec JavaScript.

## <a name="adding-client-side-validation"></a>Ajout d’une Validation côté Client

Étant donné que notre classe Movie a déjà certains attributs de validation, nous devons simplement ajouter quelques fichiers JavaScript à notre modèle de vue de Create.aspx et ajoutez une ligne de code pour activer la validation côté client se déroulent.

Depuis VWD atteindre notre dossier vues/film et ouvrez Create.aspx.

Ouvrez le dossier de Scripts dans l’Explorateur de solutions et faites glisser les trois scripts suivants pour dans le &lt;head&gt; balise.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Vous souhaitez que ces fichiers de script pour apparaître dans cet ordre.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

En outre, ajoutez la ligne unique ci-dessus le Html.BeginForm :

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Voici le code indiqué dans l’IDE.

[![Films - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Exécutez votre application et consultez à nouveau /Movies/Create et cliquez sur créer sans entrer de données. Les messages d’erreur apparaissent immédiatement sans la page flash que nous associons à l’envoi des données jusqu’au serveur. C’est parce que ASP.NET MVC est désormais la validation l’entrée à la fois sur le client (à l’aide de JavaScript) et sur le serveur.

[![Créer - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Cela est en ordre. Nous allons maintenant ajouter une colonne supplémentaire à la base de données.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part6.md)
> [Suivant](getting-started-with-mvc-part8.md)
