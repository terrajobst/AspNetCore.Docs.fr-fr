---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutoriel : Personnaliser l’affichage pour EF Database First avec une application ASP.NET MVC'
description: Ce didacticiel se concentre sur la modification des vues générés automatiquement pour améliorer la présentation.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236495"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Tutoriel : Personnaliser l’affichage pour EF Database First avec une application ASP.NET MVC

À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur la modification des vues générés automatiquement pour améliorer la présentation.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des cours à la page de détail pour les étudiants
> * Vérifiez que les cours sont ajoutés à la page

## <a name="prerequisites"></a>Prérequis

* [Modifier la base de données](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Ajouter des cours au détail de l’étudiant

Le code généré fournit un bon point de départ pour votre application, mais elle n’en fournit pas nécessairement toutes les fonctionnalités dont vous avez besoin dans votre application. Vous pouvez personnaliser le code pour répondre aux besoins particuliers de votre application. Actuellement, votre application n’affiche pas les cours inscrits de l’étudiant sélectionné. Dans cette section, vous allez ajouter les cours inscrits pour chaque étudiant à la **détails** vue de l’étudiant.

Ouvrez **vues** > **étudiants** > *Details.cshtml*. En dessous de la dernière &lt;/dl&gt; balise, mais avant la fermeture &lt;/div&gt; , ajoutez le code suivant.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Ce code crée une table qui affiche une ligne pour chaque enregistrement dans la table de l’inscription de l’étudiant sélectionné. Le **affichage** méthode effectue le rendu HTML pour l’objet qui représente l’expression (modelItem). Vous utilisez la méthode d’affichage (plutôt que de simplement en l’incorporant la valeur de propriété dans le code) pour vous assurer que la valeur est mise en forme correctement en fonction de son type et le modèle pour ce type. Dans cet exemple, chaque expression retourne une propriété unique de l’enregistrement actif dans la boucle, et les valeurs sont des types primitifs qui sont rendus sous forme de texte.

## <a name="confirm-courses-are-added"></a>Confirmer les cours sont ajoutés

Exécutez la solution. Cliquez sur **liste d’étudiants** et sélectionnez **détails** pour l’une des étudiants. Vous verrez que les cours inscrits ont été incluses dans la vue.

![étudiant avec l’inscription](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Cours ajoutés à la page de détail pour les étudiants
> * Confirmé que les cours sont ajoutés à la page

Passez au didacticiel suivant pour apprendre à ajouter des annotations de données pour spécifier les exigences de validation et afficher la mise en forme.
> [!div class="nextstepaction"]
> [Améliorer la validation des données](enhancing-data-validation.md)
