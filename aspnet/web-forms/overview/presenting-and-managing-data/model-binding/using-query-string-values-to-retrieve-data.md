---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: "À l’aide des valeurs de chaîne de requête pour filtrer les données avec la liaison de modèle et les web forms | Documents Microsoft"
author: tfitzmac
description: "Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle permet une interaction de données plus droites-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="c2e4a-104">À l’aide des valeurs de chaîne de requête pour filtrer les données avec la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="c2e4a-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="c2e4a-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c2e4a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c2e4a-106">Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="c2e4a-107">Liaison de modèle permet une interaction de données plus simple que vous traitez des données des objets de source (comme ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="c2e4a-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="c2e4a-108">Cette série commence par la partie introductive et déplace vers des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="c2e4a-109">Ce didacticiel montre comment passer une valeur dans la chaîne de requête et utilisez cette valeur pour récupérer des données via la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="c2e4a-110">Ce didacticiel s’appuie sur le projet créé dans le [antérieures](retrieving-data.md) parties de la série.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="c2e4a-111">Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="c2e4a-112">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="c2e4a-113">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 indiqué dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="c2e4a-114">Vous allez générer</span><span class="sxs-lookup"><span data-stu-id="c2e4a-114">What you'll build</span></span>

<span data-ttu-id="c2e4a-115">Dans ce didacticiel, vous effectuerez les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="c2e4a-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="c2e4a-116">Ajouter une nouvelle page pour afficher les cours inscrits pour un étudiant</span><span class="sxs-lookup"><span data-stu-id="c2e4a-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="c2e4a-117">Récupérer les cours inscrits pour l’étudiant sélectionné selon une valeur dans la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="c2e4a-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="c2e4a-118">Ajouter un lien hypertexte avec une valeur de chaîne de requête à partir de l’affichage de grille vers la nouvelle page</span><span class="sxs-lookup"><span data-stu-id="c2e4a-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="c2e4a-119">Les étapes décrites dans ce didacticiel sont relativement similaires à ce que vous avez fait dans la plus antérieure [didacticiel](sorting-paging-and-filtering-data.md) pour filtrer les étudiants affichées en fonction de la sélection de l’utilisateur dans une zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="c2e4a-120">Dans ce didacticiel, vous avez utilisé le **contrôle** attribut dans la méthode select pour spécifier que la valeur du paramètre provient d’un contrôle.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="c2e4a-121">Dans ce didacticiel, vous allez utiliser le **QueryString** attribut dans la méthode select pour spécifier que la valeur du paramètre provient de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="c2e4a-122">Ajouter une nouvelle page pour afficher le cours de l’un étudiant</span><span class="sxs-lookup"><span data-stu-id="c2e4a-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="c2e4a-123">Ajoutez un nouveau formulaire web qui utilise la page maître Site.master et nommez la page **cours**.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="c2e4a-124">Dans le **Courses.aspx** , ajoutez un affichage de grille pour afficher les cours pour les étudiants sélectionné.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="c2e4a-125">Définir la méthode select</span><span class="sxs-lookup"><span data-stu-id="c2e4a-125">Define the select method</span></span>

<span data-ttu-id="c2e4a-126">Dans **Courses.aspx.cs**, vous allez ajouter la méthode select avec le nom spécifié dans l’affichage de grille **SelectMethod** propriété.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="c2e4a-127">Dans cette méthode, vous allez définir la requête pour récupérer le cours de l’un étudiant et spécifier que le paramètre provient d’une valeur de chaîne de requête avec le même nom que le paramètre.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="c2e4a-128">Vous devez tout d’abord, ajoutez le code suivant **à l’aide de** instructions.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="c2e4a-129">Ensuite, ajoutez le code suivant à Courses.aspx.cs :</span><span class="sxs-lookup"><span data-stu-id="c2e4a-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="c2e4a-130">L’attribut de chaîne de requête signifie qu’une valeur de chaîne de requête nommée StudentID est automatiquement affectée au paramètre de cette méthode.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="c2e4a-131">Ajouter un lien hypertexte avec la valeur de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="c2e4a-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="c2e4a-132">Dans l’affichage de grille sur Students.aspx, vous allez ajouter un champ de lien hypertexte qui établit un lien vers votre nouvelle page de cours.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="c2e4a-133">Le lien hypertexte inclut une valeur de chaîne de requête avec l’id de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="c2e4a-134">Dans Students.aspx, ajoutez le champ suivant pour les colonnes de la vue grille juste en dessous du champ de Total des crédits.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="c2e4a-135">Exécutez l’application et remarquez que l’affichage de grille inclut désormais le lien du cours.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Ajouter un lien hypertexte](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="c2e4a-137">Lorsque vous cliquez sur un des liens, vous verrez les cours inscrits que student.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![afficher en cours](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="c2e4a-139">Conclusion</span><span class="sxs-lookup"><span data-stu-id="c2e4a-139">Conclusion</span></span>

<span data-ttu-id="c2e4a-140">Dans ce didacticiel, vous avez ajouté un lien avec une valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="c2e4a-141">Vous avez utilisé cette valeur de chaîne de requête pour la valeur du paramètre dans la méthode select.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="c2e4a-142">Dans la prochaine [didacticiel](adding-business-logic-layer.md), vous allez déplacer le code à partir des fichiers code-behind dans une couche de logique métier et une couche d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="c2e4a-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c2e4a-143">[Précédent](integrating-jquery-ui.md)
[Suivant](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="c2e4a-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
