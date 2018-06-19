---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Le tri, la pagination et le filtrage des données avec la liaison de modèle et les web forms | Documents Microsoft
author: tfitzmac
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle permet une interaction de données plus droites-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885404"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="805a1-104">Le tri, la pagination et le filtrage des données avec la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="805a1-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="805a1-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="805a1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="805a1-106">Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="805a1-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="805a1-107">Liaison de modèle permet une interaction de données plus simple que vous traitez des données des objets de source (comme ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="805a1-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="805a1-108">Cette série commence par la partie introductive et déplace vers des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="805a1-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="805a1-109">Ce didacticiel montre comment ajouter le tri, la pagination et le filtrage des données via la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="805a1-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="805a1-110">Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.</span><span class="sxs-lookup"><span data-stu-id="805a1-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="805a1-111">Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="805a1-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="805a1-112">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="805a1-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="805a1-113">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 indiqué dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="805a1-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="805a1-114">Vous allez générer</span><span class="sxs-lookup"><span data-stu-id="805a1-114">What you'll build</span></span>

<span data-ttu-id="805a1-115">Dans ce didacticiel, vous effectuerez les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="805a1-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="805a1-116">Activer le tri et la pagination des données</span><span class="sxs-lookup"><span data-stu-id="805a1-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="805a1-117">Activer le filtrage des données selon une sélection par l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="805a1-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="805a1-118">Ajouter un tri</span><span class="sxs-lookup"><span data-stu-id="805a1-118">Add sorting</span></span>

<span data-ttu-id="805a1-119">Il est très facile d’activation du tri dans le GridView.</span><span class="sxs-lookup"><span data-stu-id="805a1-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="805a1-120">Dans le fichier Student.aspx, définissez simplement **AllowSorting** à **true** dans le GridView.</span><span class="sxs-lookup"><span data-stu-id="805a1-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="805a1-121">Vous n’avez pas besoin de définir un **SortExpression** valeur pour chaque colonne, comme le DataField est automatiquement utilisé.</span><span class="sxs-lookup"><span data-stu-id="805a1-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="805a1-122">Le contrôle GridView modifie la requête pour inclure le tri des données par la valeur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="805a1-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="805a1-123">Le code en surbrillance ci-dessous illustre l’ajout de que vous devez faire activer le tri.</span><span class="sxs-lookup"><span data-stu-id="805a1-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="805a1-124">Exécuter l’application web et de tester des étudiants tri en fonction des valeurs dans des colonnes différentes.</span><span class="sxs-lookup"><span data-stu-id="805a1-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![étudiants de tri](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="805a1-126">ajouter la pagination</span><span class="sxs-lookup"><span data-stu-id="805a1-126">Add paging</span></span>

<span data-ttu-id="805a1-127">Il est également très facile de l’activation de la pagination.</span><span class="sxs-lookup"><span data-stu-id="805a1-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="805a1-128">Dans le GridView, définissez la **AllowPaging** propriété **true** et définir le **PageSize** propriété pour le nombre d’enregistrements que vous souhaitez afficher sur chaque page.</span><span class="sxs-lookup"><span data-stu-id="805a1-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="805a1-129">Dans ce didacticiel, vous pouvez la définir à 4.</span><span class="sxs-lookup"><span data-stu-id="805a1-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="805a1-130">Exécuter l’application web et notez que maintenant les enregistrements sont réparties sur plusieurs pages avec pas plus de 4 enregistrements affichés sur une seule page.</span><span class="sxs-lookup"><span data-stu-id="805a1-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![ajouter la pagination](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="805a1-132">Exécution de requête différée améliore l’efficacité de l’application.</span><span class="sxs-lookup"><span data-stu-id="805a1-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="805a1-133">Au lieu de récupérer l’ensemble de données, le contrôle GridView modifie la requête pour récupérer uniquement les enregistrements de la page actuelle.</span><span class="sxs-lookup"><span data-stu-id="805a1-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="805a1-134">Filtrer les enregistrements par sélection de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="805a1-134">Filter records by user selection</span></span>

<span data-ttu-id="805a1-135">Liaison de modèle ajoute plusieurs attributs qui vous permettent de désigner la définition de la valeur d’un paramètre dans une méthode de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="805a1-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="805a1-136">Ces attributs sont dans le **System.Web.ModelBinding** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="805a1-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="805a1-137">Elles comprennent :</span><span class="sxs-lookup"><span data-stu-id="805a1-137">They include:</span></span>

- <span data-ttu-id="805a1-138">Contrôle</span><span class="sxs-lookup"><span data-stu-id="805a1-138">Control</span></span>
- <span data-ttu-id="805a1-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="805a1-139">Cookie</span></span>
- <span data-ttu-id="805a1-140">Formulaire</span><span class="sxs-lookup"><span data-stu-id="805a1-140">Form</span></span>
- <span data-ttu-id="805a1-141">Profil</span><span class="sxs-lookup"><span data-stu-id="805a1-141">Profile</span></span>
- <span data-ttu-id="805a1-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="805a1-142">QueryString</span></span>
- <span data-ttu-id="805a1-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="805a1-143">RouteData</span></span>
- <span data-ttu-id="805a1-144">Session</span><span class="sxs-lookup"><span data-stu-id="805a1-144">Session</span></span>
- <span data-ttu-id="805a1-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="805a1-145">UserProfile</span></span>
- <span data-ttu-id="805a1-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="805a1-146">ViewState</span></span>

<span data-ttu-id="805a1-147">Dans ce didacticiel, vous utiliserez une valeur d’un contrôle pour filtrer les enregistrements sont affichés dans le GridView.</span><span class="sxs-lookup"><span data-stu-id="805a1-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="805a1-148">Vous allez ajouter le **contrôle** d’attribut à la méthode de requête que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="805a1-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="805a1-149">Dans un [ultérieurement](using-query-string-values-to-retrieve-data.md) didacticiel, vous allez appliquer la **QueryString** d’attribut à un paramètre pour spécifier que la valeur du paramètre provient d’une valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="805a1-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="805a1-150">Au-dessus du contrôle ValidationSummary, ajoutez d’abord une liste déroulante de filtrage les étudiants sont affichés.</span><span class="sxs-lookup"><span data-stu-id="805a1-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="805a1-151">Dans le fichier code-behind, modifier la méthode select pour recevoir une valeur à partir du contrôle et le nom du contrôle qui fournit la valeur de la valeur du nom du paramètre.</span><span class="sxs-lookup"><span data-stu-id="805a1-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="805a1-152">Vous devez ajouter un **à l’aide de** instruction pour le **System.Web.ModelBinding** résoudre l’attribut de contrôle de l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="805a1-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="805a1-153">Le code suivant montre la méthode select nouveau travaillée pour filtrer les données retournées en fonction de la valeur de la zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="805a1-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="805a1-154">Ajout d’un attribut de contrôle avant un paramètre spécifie que la valeur de ce paramètre est fourni à partir d’un contrôle portant le même nom.</span><span class="sxs-lookup"><span data-stu-id="805a1-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="805a1-155">Exécuter l’application web et sélectionner des valeurs différentes à partir de la liste déroulante pour filtrer la liste d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="805a1-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![étudiants de filtre](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="805a1-157">Conclusion</span><span class="sxs-lookup"><span data-stu-id="805a1-157">Conclusion</span></span>

<span data-ttu-id="805a1-158">Dans ce didacticiel, vous activé le tri et la pagination des données.</span><span class="sxs-lookup"><span data-stu-id="805a1-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="805a1-159">Vous est également activé le filtrage des données par la valeur d’un contrôle.</span><span class="sxs-lookup"><span data-stu-id="805a1-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="805a1-160">Dans la prochaine [didacticiel](integrating-jquery-ui.md) vous allez améliorer l’interface utilisateur en intégrant un widget de JQuery UI dans le modèle de données dynamiques.</span><span class="sxs-lookup"><span data-stu-id="805a1-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="805a1-161">[Précédent](updating-deleting-and-creating-data.md)
> [Suivant](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="805a1-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
