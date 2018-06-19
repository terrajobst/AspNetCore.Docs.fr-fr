---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Partie 4 : Ajout d’une vue Admin | Documents Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879528"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="b8dfe-102">Partie 4 : Ajout d’une vue de l’administrateur</span><span class="sxs-lookup"><span data-stu-id="b8dfe-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="b8dfe-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b8dfe-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b8dfe-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="b8dfe-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="b8dfe-105">Ajouter une vue de l’administrateur</span><span class="sxs-lookup"><span data-stu-id="b8dfe-105">Add an Admin View</span></span>

<span data-ttu-id="b8dfe-106">Nous allons maintenant nous tourner vers le côté client et ajouter une page qui peut utiliser des données à partir du contrôleur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="b8dfe-107">La page permettra aux utilisateurs de créer, modifier ou supprimer des produits, en envoyant les requêtes AJAX au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="b8dfe-108">Dans l’Explorateur de solutions, développez le dossier contrôleurs et d’ouvrir le fichier HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="b8dfe-109">Ce fichier contient un contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-109">This file contains an MVC controller.</span></span> <span data-ttu-id="b8dfe-110">Ajoutez une méthode nommée `Admin`:</span><span class="sxs-lookup"><span data-stu-id="b8dfe-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="b8dfe-111">Le **HttpRouteUrl** méthode crée l’URI à l’API web, et nous enregistrez-le dans le sac d’affichage pour une date ultérieure.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="b8dfe-112">Ensuite, placez le curseur de texte dans le `Admin` méthode d’action, puis avec le bouton droit et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="b8dfe-113">Cela affiche la **ajouter une vue** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="b8dfe-114">Dans le **ajouter une vue** boîte de dialogue, nom de la vue « Admin ».</span><span class="sxs-lookup"><span data-stu-id="b8dfe-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="b8dfe-115">Activez la case à cocher **créer une vue fortement typée**.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="b8dfe-116">Sous **classe de modèle**, sélectionnez « Product (ProductStore.Models) ».</span><span class="sxs-lookup"><span data-stu-id="b8dfe-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="b8dfe-117">Laissez toutes les autres options en tant que leurs valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="b8dfe-118">En cliquant sur **ajouter** ajoute un fichier nommé Admin.cshtml sous Views/Home.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="b8dfe-119">Ouvrez ce fichier et ajouter le code HTML suivant.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="b8dfe-120">Ce code HTML définit la structure de la page, mais aucun fonctionnalités ne sont encore associés.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="b8dfe-121">Créer un lien vers la Page d’administration</span><span class="sxs-lookup"><span data-stu-id="b8dfe-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="b8dfe-122">Dans l’Explorateur de solutions, développez le dossier Views, puis le dossier Shared.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="b8dfe-123">Ouvrez le fichier nommé \_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="b8dfe-124">Recherchez le **ul** élément avec l’id = « menu » et un lien d’action pour l’affichage de l’administrateur :</span><span class="sxs-lookup"><span data-stu-id="b8dfe-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="b8dfe-125">Dans l’exemple de projet, j’ai apporté quelques autres modifications esthétique, par exemple en remplaçant la chaîne « Votre logo ici ».</span><span class="sxs-lookup"><span data-stu-id="b8dfe-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="b8dfe-126">Elles n’affectent pas la fonctionnalité de l’application.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="b8dfe-127">Vous pouvez télécharger le projet et comparer les fichiers.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="b8dfe-128">Exécutez l’application et cliquez sur le lien « Admin » qui apparaît en haut de la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="b8dfe-129">La page d’administration doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="b8dfe-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="b8dfe-130">Droit à présent, la page ne fait rien.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="b8dfe-131">Dans la section suivante, nous allons utiliser Knockout.js pour créer une interface utilisateur dynamique.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="b8dfe-132">Ajouter une autorisation</span><span class="sxs-lookup"><span data-stu-id="b8dfe-132">Add Authorization</span></span>

<span data-ttu-id="b8dfe-133">La page d’administration est actuellement accessible à toute personne visitant le site.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="b8dfe-134">Nous allons modifier cette option pour restreindre l’accès aux administrateurs.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="b8dfe-135">Commencez par ajouter un rôle « Administrateur » et un utilisateur administrateur.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="b8dfe-136">Dans l’Explorateur de solutions, développez le dossier de filtres et ouvrir le fichier nommé InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="b8dfe-137">Recherchez le `SimpleMembershipInitializer` constructeur.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="b8dfe-138">Après l’appel à **WebSecurity.InitializeDatabaseConnection**, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b8dfe-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="b8dfe-139">Il s’agit d’un moyen simple et rapide pour ajouter le rôle « Administrateur » et créez un utilisateur pour le rôle.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="b8dfe-140">Dans l’Explorateur de solutions, développez le dossier Controllers et ouvrez le fichier HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="b8dfe-141">Ajouter le **Authorize** attribut le `Admin` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b8dfe-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="b8dfe-142">Ouvrez le fichier AdminController.cs et ajoutez le **Authorize** à l’ensemble d’attributs `AdminController` classe.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="b8dfe-143">MVC et les API Web définissent **Authorize** attributs, dans différents espaces de noms.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="b8dfe-144">MVC utilise **System.Web.Mvc.AuthorizeAttribute**, tandis que l’API Web utilise **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="b8dfe-145">Seuls les administrateurs peuvent désormais afficher la page d’administration.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="b8dfe-146">En outre, si vous envoyez une demande HTTP au contrôleur Admin, la demande doit contenir un cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="b8dfe-147">Si ce n’est pas le cas, le serveur envoie une réponse HTTP 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="b8dfe-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="b8dfe-148">Vous pouvez le voir dans Fiddler en envoyant une demande GET pour `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="b8dfe-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b8dfe-149">[Précédent](using-web-api-with-entity-framework-part-3.md)
> [Suivant](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="b8dfe-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
