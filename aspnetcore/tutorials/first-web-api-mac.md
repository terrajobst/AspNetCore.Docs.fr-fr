---
title: "Créer une API web avec ASP.NET Core et Visual Studio pour Mac"
author: rick-anderson
description: "Créer une API web avec ASP.NET Core MVC et Visual Studio pour Mac"
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: b0e1a331fe3229119f4669fa336b6af4822785bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="f1dc0-103">Créer une API web avec ASP.NET Core MVC et Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="f1dc0-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="f1dc0-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f1dc0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f1dc0-105">Dans ce didacticiel, créez une API web pour la gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f1dc0-106">L’interface utilisateur n’est pas construite.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-106">The UI isn't constructed.</span></span>

<span data-ttu-id="f1dc0-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f1dc0-108">macOS : API web avec Visual Studio pour Mac (ce didacticiel)</span><span class="sxs-lookup"><span data-stu-id="f1dc0-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="f1dc0-109">Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f1dc0-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="f1dc0-110">macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f1dc0-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="f1dc0-111">Pour obtenir un exemple qui utilise une base de données persistante, consultez [Introduction à ASP.NET Core MVC sur Mac ou Linux](xref:tutorials/first-mvc-app-xplat/index).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1dc0-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="f1dc0-112">Prerequisites</span></span>

<span data-ttu-id="f1dc0-113">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-113">Install the following:</span></span>

- <span data-ttu-id="f1dc0-114">[SDK .NET Core 2.0.0 ](https://www.microsoft.com/net/core) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="f1dc0-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="f1dc0-115">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="f1dc0-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="f1dc0-116">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="f1dc0-116">Create the project</span></span>

<span data-ttu-id="f1dc0-117">Dans Visual Studio, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-117">From Visual Studio, select **File > New Solution**.</span></span>

![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

<span data-ttu-id="f1dc0-119">Sélectionnez **Application .NET Core > API web ASP.NET Core > Suivant**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)

<span data-ttu-id="f1dc0-121">Entrez **TodoApi** comme **Nom du projet**, puis sélectionnez Créer.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="f1dc0-123">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="f1dc0-123">Launch the app</span></span>

<span data-ttu-id="f1dc0-124">Dans Visual Studio, sélectionnez **Exécuter > Démarrer avec débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f1dc0-125">Visual Studio lance un navigateur et accède à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="f1dc0-126">Vous obtenez une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f1dc0-127">Remplacez l’URL par `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f1dc0-128">Les données `ValuesController` s’affichent :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f1dc0-129">Ajouter la prise en charge d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f1dc0-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="f1dc0-130">Installez le fournisseur de base de données [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="f1dc0-131">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="f1dc0-132">Dans le menu **Projet**, choisissez **Ajouter des packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="f1dc0-133">Vous pouvez aussi cliquer sur **Dépendances**, puis sélectionner **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="f1dc0-134">Entrez `EntityFrameworkCore.InMemory` dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="f1dc0-135">Sélectionnez `Microsoft.EntityFrameworkCore.InMemory`, puis **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="f1dc0-136">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="f1dc0-136">Add a model class</span></span>

<span data-ttu-id="f1dc0-137">Un modèle est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="f1dc0-138">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f1dc0-139">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-139">Add a folder named *Models*.</span></span> <span data-ttu-id="f1dc0-140">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f1dc0-141">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f1dc0-142">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-142">Name the folder *Models*.</span></span>

![nouveau dossier](first-web-api-mac/_static/folder.png)

<span data-ttu-id="f1dc0-144">Remarque : Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f1dc0-145">Ajoutez une classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="f1dc0-146">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter > Nouveau fichier > Général > Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="f1dc0-147">Nommez la classe `TodoItem`, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="f1dc0-148">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-148">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f1dc0-149">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f1dc0-150">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="f1dc0-150">Create the database context</span></span>

<span data-ttu-id="f1dc0-151">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f1dc0-152">Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f1dc0-153">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f1dc0-154">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="f1dc0-154">Add a controller</span></span>

<span data-ttu-id="f1dc0-155">Dans l’Explorateur de solutions, dans le dossier *Contrôleurs*, ajoutez la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="f1dc0-156">Remplacez le code généré par ce qui suit (et ajoutez des accolades fermantes) :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f1dc0-157">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="f1dc0-157">Launch the app</span></span>

<span data-ttu-id="f1dc0-158">Dans Visual Studio, sélectionnez **Exécuter > Démarrer avec débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f1dc0-159">Visual Studio lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi au hasard.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="f1dc0-160">Vous obtenez une erreur HTTP 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f1dc0-161">Remplacez l’URL par `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f1dc0-162">Les données `ValuesController` s’affichent :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="f1dc0-163">Accédez au contrôleur `Todo` à l’adresse `http://localhost:port/api/todo` :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f1dc0-164">Implémenter les autres opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="f1dc0-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="f1dc0-165">Nous allons ajouter les méthodes `Create`, `Update` et `Delete` au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="f1dc0-166">Comme il s’agit de variations sur un même thème, je vais simplement montrer le code et mettre en évidence les principales différences.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="f1dc0-167">Générez le projet après avoir ajouté ou modifié du code.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="f1dc0-168">Créer</span><span class="sxs-lookup"><span data-stu-id="f1dc0-168">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f1dc0-169">Il s’agit d’une méthode HTTP POST, indiquée par l’attribut [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f1dc0-170">L’attribut [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indique à MVC qu’il faut obtenir la valeur de l’élément d’action à partir du corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f1dc0-171">La méthode `CreatedAtRoute` retourne une réponse 201, qui constitue la réponse standard pour une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="f1dc0-172">`CreatedAtRoute` ajoute également un en-tête Location à la réponse.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="f1dc0-173">L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f1dc0-174">Consultez [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f1dc0-175">Utiliser Postman pour envoyer une requête Create</span><span class="sxs-lookup"><span data-stu-id="f1dc0-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="f1dc0-176">Démarrez l’application (**Exécuter > Démarrer avec débogage**).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="f1dc0-177">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-177">Start Postman.</span></span>

![Console Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="f1dc0-179">Affectez la valeur `POST` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="f1dc0-180">Sélectionnez la case d’option **Body**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="f1dc0-181">Sélectionnez la case d’option **raw**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="f1dc0-182">Sélectionnez le type JSON.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-182">Set the type to JSON</span></span>
* <span data-ttu-id="f1dc0-183">Dans l’éditeur de clé-valeur, entrez un élément d’action comme</span><span class="sxs-lookup"><span data-stu-id="f1dc0-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="f1dc0-184">Sélectionnez **Send**.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-184">Select **Send**</span></span>

* <span data-ttu-id="f1dc0-185">Sélectionnez l’onglet Headers dans le volet inférieur et copiez l’en-tête **Location** :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Onglet Headers de la console Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="f1dc0-187">Vous pouvez utiliser l’URI d’en-tête Location pour accéder à la ressource que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="f1dc0-188">Rappelez la méthode `GetById` qui a créé la route nommée `"GetTodo"` :</span><span class="sxs-lookup"><span data-stu-id="f1dc0-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="f1dc0-189">Mise à jour</span><span class="sxs-lookup"><span data-stu-id="f1dc0-189">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f1dc0-190">`Update` est similaire à `Create`, mais utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="f1dc0-191">La réponse est [204 (Pas de contenu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f1dc0-192">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les différences.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f1dc0-193">Pour prendre en charge les mises à jour partielles, utilisez HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="f1dc0-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f1dc0-195">Supprimer</span><span class="sxs-lookup"><span data-stu-id="f1dc0-195">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f1dc0-196">La réponse est [204 (Pas de contenu)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="f1dc0-198">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="f1dc0-198">Next steps</span></span>

* [<span data-ttu-id="f1dc0-199">Routage vers les actions du contrôleur</span><span class="sxs-lookup"><span data-stu-id="f1dc0-199">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="f1dc0-200">Pour plus d’informations sur le déploiement de votre API, consultez [Héberger et déployer](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="f1dc0-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="f1dc0-201">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f1dc0-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="f1dc0-202">Postman</span><span class="sxs-lookup"><span data-stu-id="f1dc0-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="f1dc0-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="f1dc0-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
