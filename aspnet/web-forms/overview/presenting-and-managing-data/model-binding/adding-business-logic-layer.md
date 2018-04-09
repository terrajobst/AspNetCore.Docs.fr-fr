---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Couche de logique métier ajout à un projet qui utilise la liaison de modèle et les web forms | Documents Microsoft
author: tfitzmac
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle permet une interaction de données plus droites-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="24785-104">Couche de logique métier ajout à un projet qui utilise la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="24785-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="24785-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24785-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="24785-106">Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="24785-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="24785-107">Liaison de modèle permet une interaction de données plus simple que vous traitez des données des objets de source (comme ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="24785-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="24785-108">Cette série commence par la partie introductive et déplace vers des concepts plus avancés dans des didacticiels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="24785-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="24785-109">Ce didacticiel montre comment utiliser la liaison de modèle avec une couche de logique métier.</span><span class="sxs-lookup"><span data-stu-id="24785-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="24785-110">Vous allez définir le membre OnCallingDataMethods pour spécifier qu’un objet autre que la page actuelle est utilisé pour appeler les méthodes de données.</span><span class="sxs-lookup"><span data-stu-id="24785-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="24785-111">Ce didacticiel s’appuie sur le projet créé dans le [antérieures](retrieving-data.md) parties de la série.</span><span class="sxs-lookup"><span data-stu-id="24785-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="24785-112">Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet en c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="24785-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="24785-113">Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="24785-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="24785-114">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2013 indiqué dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="24785-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="24785-115">Vous allez générer</span><span class="sxs-lookup"><span data-stu-id="24785-115">What you'll build</span></span>

<span data-ttu-id="24785-116">Liaison de modèle vous permet de placer votre code d’interaction de données dans le fichier code-behind d’une page web ou dans une classe de logique métier distincts.</span><span class="sxs-lookup"><span data-stu-id="24785-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="24785-117">Les didacticiels précédents ont montré comment utiliser les fichiers code-behind pour le code d’interaction de données.</span><span class="sxs-lookup"><span data-stu-id="24785-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="24785-118">Cette approche fonctionne pour les sites de petite taille, mais il peut se produire du code répétition et difficulté supérieure lors de la maintenance d’un site de grande taille.</span><span class="sxs-lookup"><span data-stu-id="24785-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="24785-119">Il peut également être très difficile à tester par programmation du code qui se trouve dans les fichiers code-behind, car il n’existe aucune couche d’abstraction.</span><span class="sxs-lookup"><span data-stu-id="24785-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="24785-120">Pour centraliser le code d’interaction de données, vous pouvez créer une couche de logique métier qui contient l’ensemble de la logique permettant d’interagir avec des données.</span><span class="sxs-lookup"><span data-stu-id="24785-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="24785-121">Appelez ensuite la couche de logique métier à partir de vos pages web.</span><span class="sxs-lookup"><span data-stu-id="24785-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="24785-122">Ce didacticiel montre comment déplacer tout le code que vous avez écrit dans les didacticiels précédents dans une couche de logique métier et ensuite utiliser ce code à partir des pages.</span><span class="sxs-lookup"><span data-stu-id="24785-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="24785-123">Dans ce didacticiel, vous effectuerez les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="24785-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="24785-124">Déplacer le code à partir des fichiers de code-behind pour une couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="24785-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="24785-125">Modifier vos contrôles liés aux données pour appeler les méthodes dans la couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="24785-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="24785-126">Créer une couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="24785-126">Create business logic layer</span></span>

<span data-ttu-id="24785-127">Maintenant, vous allez créer la classe qui est appelée à partir des pages web.</span><span class="sxs-lookup"><span data-stu-id="24785-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="24785-128">Les méthodes de cette classe similaire aux méthodes que vous avez utilisée dans les didacticiels précédents et incluent les attributs de fournisseur de valeur.</span><span class="sxs-lookup"><span data-stu-id="24785-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="24785-129">Tout d’abord, ajoutez un nouveau dossier appelé **BLL**.</span><span class="sxs-lookup"><span data-stu-id="24785-129">First, add a new folder called **BLL**.</span></span>

![Ajouter un dossier](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="24785-131">Dans le dossier de la couche BLL, créez une nouvelle classe nommée **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="24785-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="24785-132">Il contient toutes les opérations de données permettant d’accéder dans les fichiers code-behind.</span><span class="sxs-lookup"><span data-stu-id="24785-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="24785-133">Les méthodes sont presque les mêmes que les méthodes dans le fichier code-behind, mais incluent des modifications.</span><span class="sxs-lookup"><span data-stu-id="24785-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="24785-134">Le changement le plus important de noter est que vous exécutez n’est plus le code à partir d’une instance de **Page** classe.</span><span class="sxs-lookup"><span data-stu-id="24785-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="24785-135">La classe de Page contient le **TryUpdateModel** (méthode) et le **ModelState** propriété.</span><span class="sxs-lookup"><span data-stu-id="24785-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="24785-136">Lorsque ce code est déplacé vers une couche de logique métier, vous n’avez plus une instance de la classe de Page pour appeler ces membres.</span><span class="sxs-lookup"><span data-stu-id="24785-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="24785-137">Pour contourner ce problème, vous devez ajouter un **ModelMethodContext** paramètre à une méthode qui accède à TryUpdateModel ou ModelState.</span><span class="sxs-lookup"><span data-stu-id="24785-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="24785-138">Ce paramètre ModelMethodContext vous permet d’appeler TryUpdateModel ou récupérer ModelState.</span><span class="sxs-lookup"><span data-stu-id="24785-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="24785-139">Vous n’avez pas besoin modifier n’importe où dans la page web pour prendre en compte ce nouveau paramètre.</span><span class="sxs-lookup"><span data-stu-id="24785-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="24785-140">Remplacez le code dans SchoolBL.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="24785-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="24785-141">Passez en revue les pages existantes pour récupérer des données à partir de la couche de logique métier</span><span class="sxs-lookup"><span data-stu-id="24785-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="24785-142">Enfin, vous allez convertir les pages Students.aspx, AddStudent.aspx et Courses.aspx à partir de l’utilisation des requêtes dans le fichier code-behind à l’aide de la couche de logique métier.</span><span class="sxs-lookup"><span data-stu-id="24785-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="24785-143">Dans les fichiers code-behind pour les étudiants, AddStudent et Courses, supprimez ou commentez les méthodes de requête suivantes :</span><span class="sxs-lookup"><span data-stu-id="24785-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="24785-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="24785-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="24785-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="24785-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="24785-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="24785-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="24785-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="24785-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="24785-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="24785-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="24785-149">Vous ne devez maintenant disposer d’aucun code dans le fichier code-behind relative aux opérations de données.</span><span class="sxs-lookup"><span data-stu-id="24785-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="24785-150">Le **OnCallingDataMethods** Gestionnaire d’événements vous permet de spécifier un objet à utiliser pour les méthodes de données.</span><span class="sxs-lookup"><span data-stu-id="24785-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="24785-151">Dans Students.aspx, ajoutez une valeur pour ce gestionnaire d’événements et modifier les noms des méthodes de données pour les noms des méthodes dans la classe de logique métier.</span><span class="sxs-lookup"><span data-stu-id="24785-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="24785-152">Dans le fichier code-behind pour Students.aspx, définissez le Gestionnaire d’événements pour l’événement CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="24785-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="24785-153">Dans ce gestionnaire d’événements, vous spécifiez la classe de logique métier pour les opérations de données.</span><span class="sxs-lookup"><span data-stu-id="24785-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="24785-154">Dans AddStudent.aspx, apporter des modifications similaires.</span><span class="sxs-lookup"><span data-stu-id="24785-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="24785-155">Dans Courses.aspx, apporter des modifications similaires.</span><span class="sxs-lookup"><span data-stu-id="24785-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="24785-156">Exécutez l’application et remarquez que toutes les pages de fonction comme c’était précédemment.</span><span class="sxs-lookup"><span data-stu-id="24785-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="24785-157">Également, la logique de validation fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="24785-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="24785-158">Conclusion</span><span class="sxs-lookup"><span data-stu-id="24785-158">Conclusion</span></span>

<span data-ttu-id="24785-159">Dans ce didacticiel, vous ré-structurée de votre application d’utiliser une couche d’accès aux données et la couche de logique métier.</span><span class="sxs-lookup"><span data-stu-id="24785-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="24785-160">Vous avez spécifié que les contrôles de données utilisent un objet qui n’est pas la page actuelle pour les opérations de données.</span><span class="sxs-lookup"><span data-stu-id="24785-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="24785-161">Précédent</span><span class="sxs-lookup"><span data-stu-id="24785-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
