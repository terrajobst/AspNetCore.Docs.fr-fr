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
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="a2394-103">Tutoriel : Personnaliser l’affichage pour EF Database First avec une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a2394-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="a2394-104">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="a2394-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="a2394-105">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a2394-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="a2394-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a2394-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="a2394-107">Ce didacticiel se concentre sur la modification des vues générés automatiquement pour améliorer la présentation.</span><span class="sxs-lookup"><span data-stu-id="a2394-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="a2394-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2394-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a2394-109">Ajouter des cours à la page de détail pour les étudiants</span><span class="sxs-lookup"><span data-stu-id="a2394-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="a2394-110">Vérifiez que les cours sont ajoutés à la page</span><span class="sxs-lookup"><span data-stu-id="a2394-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2394-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a2394-111">Prerequisites</span></span>

* [<span data-ttu-id="a2394-112">Modifier la base de données</span><span class="sxs-lookup"><span data-stu-id="a2394-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="a2394-113">Ajouter des cours au détail de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="a2394-113">Add courses to student detail</span></span>

<span data-ttu-id="a2394-114">Le code généré fournit un bon point de départ pour votre application, mais elle n’en fournit pas nécessairement toutes les fonctionnalités dont vous avez besoin dans votre application.</span><span class="sxs-lookup"><span data-stu-id="a2394-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="a2394-115">Vous pouvez personnaliser le code pour répondre aux besoins particuliers de votre application.</span><span class="sxs-lookup"><span data-stu-id="a2394-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="a2394-116">Actuellement, votre application n’affiche pas les cours inscrits de l’étudiant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="a2394-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="a2394-117">Dans cette section, vous allez ajouter les cours inscrits pour chaque étudiant à la **détails** vue de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="a2394-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="a2394-118">Ouvrez **vues** > **étudiants** > *Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a2394-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="a2394-119">En dessous de la dernière &lt;/dl&gt; balise, mais avant la fermeture &lt;/div&gt; , ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="a2394-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="a2394-120">Ce code crée une table qui affiche une ligne pour chaque enregistrement dans la table de l’inscription de l’étudiant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="a2394-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="a2394-121">Le **affichage** méthode effectue le rendu HTML pour l’objet qui représente l’expression (modelItem).</span><span class="sxs-lookup"><span data-stu-id="a2394-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="a2394-122">Vous utilisez la méthode d’affichage (plutôt que de simplement en l’incorporant la valeur de propriété dans le code) pour vous assurer que la valeur est mise en forme correctement en fonction de son type et le modèle pour ce type.</span><span class="sxs-lookup"><span data-stu-id="a2394-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="a2394-123">Dans cet exemple, chaque expression retourne une propriété unique de l’enregistrement actif dans la boucle, et les valeurs sont des types primitifs qui sont rendus sous forme de texte.</span><span class="sxs-lookup"><span data-stu-id="a2394-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="a2394-124">Confirmer les cours sont ajoutés</span><span class="sxs-lookup"><span data-stu-id="a2394-124">Confirm courses are added</span></span>

<span data-ttu-id="a2394-125">Exécutez la solution.</span><span class="sxs-lookup"><span data-stu-id="a2394-125">Run the solution.</span></span> <span data-ttu-id="a2394-126">Cliquez sur **liste d’étudiants** et sélectionnez **détails** pour l’une des étudiants.</span><span class="sxs-lookup"><span data-stu-id="a2394-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="a2394-127">Vous verrez que les cours inscrits ont été incluses dans la vue.</span><span class="sxs-lookup"><span data-stu-id="a2394-127">You will see the enrolled courses have been included in the view.</span></span>

![étudiant avec l’inscription](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="a2394-129">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="a2394-129">Next steps</span></span>
<span data-ttu-id="a2394-130">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2394-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a2394-131">Cours ajoutés à la page de détail pour les étudiants</span><span class="sxs-lookup"><span data-stu-id="a2394-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="a2394-132">Confirmé que les cours sont ajoutés à la page</span><span class="sxs-lookup"><span data-stu-id="a2394-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="a2394-133">Passez au didacticiel suivant pour apprendre à ajouter des annotations de données pour spécifier les exigences de validation et afficher la mise en forme.</span><span class="sxs-lookup"><span data-stu-id="a2394-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a2394-134">Améliorer la validation des données</span><span class="sxs-lookup"><span data-stu-id="a2394-134">Enhance data validation</span></span>](enhancing-data-validation.md)
