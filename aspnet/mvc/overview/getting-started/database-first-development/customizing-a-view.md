---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First avec ASP.NET MVC : personnaliser un affichage | Microsoft Docs'
author: Rick-Anderson
description: À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: f66e097d53514ab3842e04cd545ca626c652478a
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021208"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF Database First avec ASP.NET MVC : personnalisation d’une vue
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.
> 
> Cette partie de la série se concentre sur la modification des vues générés automatiquement pour améliorer la présentation.


## <a name="add-enrolled-courses-to-student-details"></a>Ajouter des cours inscrits pour les détails de l’étudiant

Le code généré fournit un bon point de départ pour votre application, mais elle n’en fournit pas nécessairement toutes les fonctionnalités dont vous avez besoin dans votre application. Vous pouvez personnaliser le code pour répondre aux besoins particuliers de votre application. Actuellement, votre application n’affiche pas les cours inscrits de l’étudiant sélectionné. Dans cette section, vous allez ajouter les cours inscrits pour chaque étudiant à la **détails** vue de l’étudiant.

Ouvrez **Students/Details.cshtml**et en dessous de la dernière &lt;/dl&gt; onglet, mais avant la fermeture &lt;/div&gt; , ajoutez le code suivant.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Ce code crée une table qui affiche une ligne pour chaque enregistrement dans la table de l’inscription de l’étudiant sélectionné. Le **affichage** méthode effectue le rendu HTML pour l’objet qui représente l’expression (modelItem). Vous utilisez la méthode d’affichage (plutôt que de simplement en l’incorporant la valeur de propriété dans le code) pour vous assurer que la valeur est mise en forme correctement en fonction de son type et le modèle pour ce type. Dans cet exemple, chaque expression retourne une propriété unique de l’enregistrement actif dans la boucle, et les valeurs sont des types primitifs qui sont rendus sous forme de texte.

Accédez de nouveau à la vue Index des étudiants et sélectionnez **détails** pour l’une des étudiants. Vous verrez que les cours inscrits ont été incluses dans la vue.

![étudiant avec l’inscription](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Précédent](changing-the-database.md)
> [Suivant](enhancing-data-validation.md)
