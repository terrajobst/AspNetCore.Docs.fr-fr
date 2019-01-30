---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutoriel : Générer des vues pour EF Database First avec une application ASP.NET MVC'
description: Ce didacticiel se concentre sur l’utilisation de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7a56c0f9197a99427bcde6103ebc69d245e8ce63
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236417"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Tutoriel : Générer des vues pour EF Database First avec une application ASP.NET MVC

À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur l’utilisation de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter une structure
> * Ajouter des liens vers des vues
> * Afficher les vues de l’étudiant
> * Afficher les vues de l’inscription

## <a name="prerequisite"></a>Prérequis

* [Créer des modèles d’application et les données du web](creating-the-web-application.md)

## <a name="add-scaffold"></a>Ajouter une structure

Vous êtes prêt à générer du code qui fournira les opérations de données standard pour les classes de modèle. Vous ajoutez le code en ajoutant un élément d’une structure. Il existe de nombreuses options pour le type de la structure que vous pouvez ajouter ; Dans ce didacticiel, la structure inclura un contrôleur et des vues qui correspondent aux modèles Student et d’inscription, que vous avez créé dans la section précédente.

Pour maintenir la cohérence dans votre projet, vous allez ajouter le nouveau contrôleur à l’objet de **contrôleurs** dossier. Avec le bouton droit le **contrôleurs** dossier, puis sélectionnez **ajouter** > **nouvel élément structuré**.

Sélectionnez le **contrôleur MVC 5 avec vues, utilisant Entity Framework** option. Cette option génère le contrôleur et les vues pour la mise à jour, suppression, création et affichage des données dans votre modèle.

![Ajoutez un contrôleur mvc](generating-views/_static/image2.png)

Sélectionnez **étudiant (ContosoSite.Models)** pour la classe de modèle et sélectionnez le **ContosoUniversityDataEntities (ContosoSite.Models)** pour la classe de contexte. Conservez le nom de contrôleur en tant que **StudentsController**.

Cliquez sur **Ajouter**.

Si vous recevez une erreur, il peut être, car vous n’avez pas généré le projet dans la section précédente. Dans ce cas, essayez de générer le projet et puis ajoutez de nouveau l’élément généré automatiquement.

Une fois le processus de génération de code terminé, vous verrez un nouveau contrôleur et les vues dans votre projet **contrôleurs** et **vues** > **étudiants** dossiers .


Effectuez de nouveau les mêmes étapes, mais ajouter une structure pour le **inscription** classe. Une fois terminé, vous avez un **EnrollmentsController.cs** fichier et un dossier sous **vues** nommé **les inscriptions** avec les vues Create, Delete, détails, Edit et Index.

## <a name="add-links-to-new-views"></a>Ajouter des liens vers des vues

Pour le rendre plus facile pour vous permettent d’accéder à vos nouvelles vues, vous pouvez ajouter deux liens hypertexte dans les vues d’Index pour les étudiants et les inscriptions. Ouvrez le fichier à **vues** > **domestique** > *Index.cshtml*, qui est la page d’accueil de votre site. Ajoutez le code suivant sous le jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Pour la méthode ActionLink, le premier paramètre est le texte à afficher dans le lien. Le deuxième paramètre est l’action et le troisième paramètre est le nom du contrôleur. Par exemple, le premier lien pointe vers l’action de l’Index dans StudentsController. Le lien hypertexte est construit à partir de ces valeurs. Le premier lien finalement mène à la **Index.cshtml** fichier inclus dans le **vues/étudiants** dossier.

## <a name="display-student-views"></a>Afficher les vues de l’étudiant

Vous allez vérifier que le code ajouté à votre projet correctement affiche une liste des étudiants et permet aux utilisateurs de modifier, créer ou supprimer les enregistrements d’étudiants dans la base de données.

Avec le bouton droit le **vues** > **accueil** > *Index.cshtml* et sélectionnez **afficher dans le navigateur**. Sur la page d’accueil application, sélectionnez **liste d’étudiants**.

![](generating-views/_static/image6.png)

Sur le **Index** page, notez la liste des étudiants et des liens pour modifier ces données. Sélectionnez le **créer un nouveau** lier et fournir des valeurs pour un nouvel étudiant. Cliquez sur **créer**et notez le nouvel étudiant est ajouté à votre liste.

Sur le **Index** page, sélectionnez le **modifier** lien et modifier certains des valeurs d’un étudiant. Cliquez sur **enregistrer**et notez l’enregistrement d’étudiant a été modifié.

Enfin, sélectionnez le **supprimer** lier et confirmer que vous souhaitez supprimer l’enregistrement en cliquant sur le **supprimer** bouton.

Sans écrire de code, vous avez ajouté des vues qui effectuent des opérations courantes sur les données dans la table Student.

Vous avez peut-être remarqué que l’étiquette de texte pour un champ est basé sur la propriété de base de données (tel que **LastName**) qui n’est pas nécessairement ce que vous voulez afficher sur la page web. Par exemple, vous pouvez préférer l’étiquette à être **nom**. Vous pouvez résoudre ce affichage problème ultérieurement dans ce didacticiel.

## <a name="display-enrollment-views"></a>Afficher les vues de l’inscription

Votre base de données inclut une relation un-à-plusieurs entre les tables Student et d’inscription et une relation un-à-plusieurs entre les tables de la Course et Enrollment. Les vues pour l’inscription gèrent correctement ces relations. Accédez à la page d’accueil de votre site, puis sélectionnez le **liste des inscriptions** lien, puis le **créer un nouveau** lien.

La vue affiche un formulaire pour la création d’un nouvel enregistrement d’inscription. En particulier, notez que le formulaire contient un **CourseID** liste déroulante et un **StudentID** liste déroulante. Les deux sont remplies avec les valeurs des tables connexes.

En outre, la validation de valeurs fournies est automatiquement appliquée en fonction du type de données du champ. **Niveau** nécessite un nombre, un message d’erreur s’affiche si vous essayez de fournir une valeur incompatible : *Le niveau de champ doit être un nombre.*

Vous avez vérifié que les vues générées automatiquement permettent aux utilisateurs de travailler avec les données dans la base de données. Dans le didacticiel suivant de cette série, vous mettez à jour de la base de données et effectuez les modifications correspondantes dans l’application web.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Structure ajoutée
> * Liens ajoutés à des vues
> * Vues affichées étudiant
> * Vues affichées d’inscription

Passez au didacticiel suivant pour apprendre à modifier la base de données.
> [!div class="nextstepaction"]
> [Modifier la base de données](changing-the-database.md)