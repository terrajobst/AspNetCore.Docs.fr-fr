---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First avec ASP.NET MVC : personnaliser un affichage | Microsoft Docs'
author: tfitzmac
description: À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7359b8daddc74e375675d73126d7d76b288e853d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817186"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="b28db-104">EF Database First avec ASP.NET MVC : personnalisation d’une vue</span><span class="sxs-lookup"><span data-stu-id="b28db-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="b28db-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b28db-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b28db-106">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="b28db-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b28db-107">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="b28db-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b28db-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="b28db-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="b28db-109">Cette partie de la série se concentre sur la modification des vues générés automatiquement pour améliorer la présentation.</span><span class="sxs-lookup"><span data-stu-id="b28db-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="b28db-110">Ajouter des cours inscrits pour les détails de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="b28db-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="b28db-111">Le code généré fournit un bon point de départ pour votre application, mais elle n’en fournit pas nécessairement toutes les fonctionnalités dont vous avez besoin dans votre application.</span><span class="sxs-lookup"><span data-stu-id="b28db-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="b28db-112">Vous pouvez personnaliser le code pour répondre aux besoins particuliers de votre application.</span><span class="sxs-lookup"><span data-stu-id="b28db-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="b28db-113">Actuellement, votre application n’affiche pas les cours inscrits de l’étudiant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b28db-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="b28db-114">Dans cette section, vous allez ajouter les cours inscrits pour chaque étudiant à la **détails** vue de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="b28db-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="b28db-115">Ouvrez **Students/Details.cshtml**et en dessous de la dernière &lt;/dl&gt; onglet, mais avant la fermeture &lt;/div&gt; , ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="b28db-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="b28db-116">Ce code crée une table qui affiche une ligne pour chaque enregistrement dans la table de l’inscription de l’étudiant sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b28db-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="b28db-117">Le **affichage** méthode effectue le rendu HTML pour l’objet qui représente l’expression (modelItem).</span><span class="sxs-lookup"><span data-stu-id="b28db-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="b28db-118">Vous utilisez la méthode d’affichage (plutôt que de simplement en l’incorporant la valeur de propriété dans le code) pour vous assurer que la valeur est mise en forme correctement en fonction de son type et le modèle pour ce type.</span><span class="sxs-lookup"><span data-stu-id="b28db-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="b28db-119">Dans cet exemple, chaque expression retourne une propriété unique de l’enregistrement actif dans la boucle, et les valeurs sont des types primitifs qui sont rendus sous forme de texte.</span><span class="sxs-lookup"><span data-stu-id="b28db-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="b28db-120">Accédez de nouveau à la vue Index des étudiants et sélectionnez **détails** pour l’une des étudiants.</span><span class="sxs-lookup"><span data-stu-id="b28db-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="b28db-121">Vous verrez que les cours inscrits ont été incluses dans la vue.</span><span class="sxs-lookup"><span data-stu-id="b28db-121">You will see the enrolled courses have been included in the view.</span></span>

![étudiant avec l’inscription](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b28db-123">[Précédent](changing-the-database.md)
> [Suivant](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="b28db-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
