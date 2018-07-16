---
title: Créer une API web avec ASP.NET Core et Visual Studio pour Mac
author: rick-anderson
description: Créer une API web avec ASP.NET Core MVC et Visual Studio pour Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 4caa6d9057de8d0e821c4abefe22985f43ff95ad
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38156138"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="82ca1-103">Créer une API web avec ASP.NET Core et Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="82ca1-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="82ca1-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="82ca1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="82ca1-105">Dans ce didacticiel, créez une API web pour la gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="82ca1-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="82ca1-106">L’interface utilisateur n’est pas construite.</span><span class="sxs-lookup"><span data-stu-id="82ca1-106">The UI isn't constructed.</span></span>

<span data-ttu-id="82ca1-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="82ca1-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="82ca1-108">macOS : API web avec Visual Studio pour Mac (ce didacticiel)</span><span class="sxs-lookup"><span data-stu-id="82ca1-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="82ca1-109">Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="82ca1-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="82ca1-110">macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="82ca1-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="82ca1-111">Pour obtenir un exemple qui utilise une base de données persistante, consultez [Présentation d’ASP.NET Core MVC sur macOS ou Linux](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="82ca1-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82ca1-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="82ca1-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="82ca1-113">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="82ca1-113">Create the project</span></span>

<span data-ttu-id="82ca1-114">Dans Visual Studio, sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

<span data-ttu-id="82ca1-116">Sélectionnez **Application .NET Core** > **API web ASP.NET Core** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)

<span data-ttu-id="82ca1-118">Entrez *TodoApi* comme **Nom de projet**, puis cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="82ca1-120">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="82ca1-120">Launch the app</span></span>

<span data-ttu-id="82ca1-121">Dans Visual Studio, sélectionnez **Exécuter** > **Démarrer avec débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="82ca1-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="82ca1-122">Visual Studio lance un navigateur et accède à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="82ca1-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="82ca1-123">Vous obtenez une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="82ca1-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="82ca1-124">Remplacez l’URL par `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="82ca1-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="82ca1-125">Les données `ValuesController` s’affichent :</span><span class="sxs-lookup"><span data-stu-id="82ca1-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="82ca1-126">Ajouter la prise en charge d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="82ca1-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="82ca1-127">Installez le fournisseur de base de données [Entity Framework Core InMemory](/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="82ca1-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="82ca1-128">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="82ca1-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="82ca1-129">Dans le menu **Projet**, choisissez **Ajouter des packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="82ca1-130">Vous pouvez aussi cliquer avec le bouton droit sur **Dépendances**, puis sélectionner **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="82ca1-131">Entrez `EntityFrameworkCore.InMemory` dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="82ca1-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="82ca1-132">Sélectionnez `Microsoft.EntityFrameworkCore.InMemory`, puis **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="82ca1-133">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="82ca1-133">Add a model class</span></span>

<span data-ttu-id="82ca1-134">Un modèle est un objet qui représente les données de votre application.</span><span class="sxs-lookup"><span data-stu-id="82ca1-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="82ca1-135">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="82ca1-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="82ca1-136">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="82ca1-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="82ca1-137">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="82ca1-138">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="82ca1-138">Name the folder *Models*.</span></span>

![nouveau dossier](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="82ca1-140">Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="82ca1-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="82ca1-141">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="82ca1-142">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="82ca1-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="82ca1-143">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="82ca1-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="82ca1-144">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="82ca1-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="82ca1-145">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="82ca1-145">Create the database context</span></span>

<span data-ttu-id="82ca1-146">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="82ca1-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="82ca1-147">Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="82ca1-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="82ca1-148">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="82ca1-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="82ca1-149">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="82ca1-149">Add a controller</span></span>

<span data-ttu-id="82ca1-150">Dans l’Explorateur de solutions, dans le dossier *Contrôleurs*, ajoutez la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="82ca1-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="82ca1-151">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="82ca1-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="82ca1-152">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="82ca1-152">Launch the app</span></span>

<span data-ttu-id="82ca1-153">Dans Visual Studio, sélectionnez **Exécuter** > **Démarrer avec débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="82ca1-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="82ca1-154">Visual Studio lance un navigateur et accède à `http://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="82ca1-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="82ca1-155">Vous obtenez une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="82ca1-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="82ca1-156">Remplacez l’URL par `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="82ca1-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="82ca1-157">Les données `ValuesController` s’affichent :</span><span class="sxs-lookup"><span data-stu-id="82ca1-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="82ca1-158">Accédez au contrôleur `Todo` à l’adresse `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="82ca1-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="82ca1-159">Le code JSON suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="82ca1-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="82ca1-160">Implémenter les autres opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="82ca1-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="82ca1-161">Nous allons ajouter les méthodes `Create`, `Update` et `Delete` au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="82ca1-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="82ca1-162">Comme ces méthodes sont des variations sur un même thème, je vais simplement montrer le code et mettre en évidence les principales différences.</span><span class="sxs-lookup"><span data-stu-id="82ca1-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="82ca1-163">Générez le projet après avoir ajouté ou modifié du code.</span><span class="sxs-lookup"><span data-stu-id="82ca1-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="82ca1-164">Créer</span><span class="sxs-lookup"><span data-stu-id="82ca1-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="82ca1-165">La méthode précédente répond à HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="82ca1-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="82ca1-166">L’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indique à MVC qu’il faut obtenir la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="82ca1-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="82ca1-167">La méthode précédente répond à HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="82ca1-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="82ca1-168">MVC obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="82ca1-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="82ca1-169">La méthode `CreatedAtRoute` retourne une réponse 201.</span><span class="sxs-lookup"><span data-stu-id="82ca1-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="82ca1-170">Il s’agit de la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="82ca1-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="82ca1-171">`CreatedAtRoute` ajoute également un en-tête Location à la réponse.</span><span class="sxs-lookup"><span data-stu-id="82ca1-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="82ca1-172">L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="82ca1-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="82ca1-173">Consultez [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="82ca1-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="82ca1-174">Utiliser Postman pour envoyer une requête Create</span><span class="sxs-lookup"><span data-stu-id="82ca1-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="82ca1-175">Démarrez l’application (**Exécuter** > **Démarrer avec débogage**).</span><span class="sxs-lookup"><span data-stu-id="82ca1-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="82ca1-176">Ouvrez Postman.</span><span class="sxs-lookup"><span data-stu-id="82ca1-176">Open Postman.</span></span>

![Console Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="82ca1-178">Mettez à jour le numéro de port dans l’URL localhost.</span><span class="sxs-lookup"><span data-stu-id="82ca1-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="82ca1-179">Définissez la méthode HTTP sur *POST*.</span><span class="sxs-lookup"><span data-stu-id="82ca1-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="82ca1-180">Cliquez sur l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="82ca1-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="82ca1-181">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="82ca1-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="82ca1-182">Définissez le type sur *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="82ca1-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="82ca1-183">Entrez un corps de requête avec une tâche similaire au code JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="82ca1-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="82ca1-184">Cliquez sur le bouton **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="82ca1-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="82ca1-185">Si vous ne recevez pas de réponse après avoir cliqué sur **Send** (Envoyer), désactivez l’option **SSL certification verification** (Vérification de la certification SSL).</span><span class="sxs-lookup"><span data-stu-id="82ca1-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="82ca1-186">Celle-ci se trouve sous **File** (Fichier) > **Settings** (Paramètres).</span><span class="sxs-lookup"><span data-stu-id="82ca1-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="82ca1-187">Cliquez à nouveau sur le bouton **Send** (Envoyer) une fois le paramètre désactivé.</span><span class="sxs-lookup"><span data-stu-id="82ca1-187">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="82ca1-188">Cliquez sur l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse), puis copiez la valeur de l’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="82ca1-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="82ca1-190">Vous pouvez utiliser l’URI d’en-tête Location pour accéder à la ressource que vous avez créée.</span><span class="sxs-lookup"><span data-stu-id="82ca1-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="82ca1-191">La méthode `Create` retourne [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="82ca1-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="82ca1-192">Le premier paramètre passé à `CreatedAtRoute` représente la route nommée à utiliser pour générer l’URL.</span><span class="sxs-lookup"><span data-stu-id="82ca1-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="82ca1-193">Rappelez la méthode `GetById` qui a créé la route nommée `"GetTodo"` :</span><span class="sxs-lookup"><span data-stu-id="82ca1-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="82ca1-194">Mise à jour</span><span class="sxs-lookup"><span data-stu-id="82ca1-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

<span data-ttu-id="82ca1-195">`Update` est similaire à `Create`, mais utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="82ca1-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="82ca1-196">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="82ca1-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="82ca1-197">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les différences.</span><span class="sxs-lookup"><span data-stu-id="82ca1-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="82ca1-198">Pour prendre en charge les mises à jour partielles, utilisez HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="82ca1-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="82ca1-200">Supprimer</span><span class="sxs-lookup"><span data-stu-id="82ca1-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="82ca1-201">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="82ca1-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
