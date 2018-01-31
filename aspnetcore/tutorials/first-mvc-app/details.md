---
title: "Examen des méthodes Details et Delete"
author: rick-anderson
description: "La méthode et l’affichage du contrôleur Details dans une application ASP.NET Core MVC de base."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 4a0004fc79f8e1d334e3acb96b28b2954d19f0a1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="35ec6-103">Examen des méthodes Details et Delete</span><span class="sxs-lookup"><span data-stu-id="35ec6-103">Examining the Details and Delete methods</span></span>

<span data-ttu-id="35ec6-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="35ec6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="35ec6-105">Ouvrez le contrôleur vidéo et examinez la méthode `Details` :</span><span class="sxs-lookup"><span data-stu-id="35ec6-105">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="35ec6-106">Le moteur de génération de modèles automatique MVC qui a créé cette méthode d’action ajoute un commentaire présentant une requête HTTP qui appelle la méthode.</span><span class="sxs-lookup"><span data-stu-id="35ec6-106">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="35ec6-107">Dans le cas présent, il s’agit d’une requête GET avec trois segments d’URL : le contrôleur `Movies`, la méthode `Details` et une valeur `id`.</span><span class="sxs-lookup"><span data-stu-id="35ec6-107">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="35ec6-108">N’oubliez pas que ces segments sont définis dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="35ec6-108">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="35ec6-109">EF facilite la recherche de données à l’aide de la méthode `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="35ec6-109">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="35ec6-110">Une fonctionnalité de sécurité importante intégrée à la méthode réside dans le fait que le code vérifie que la méthode de recherche a trouvé un film avant de tenter toute opération que ce soit avec lui.</span><span class="sxs-lookup"><span data-stu-id="35ec6-110">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="35ec6-111">Par exemple, un pirate informatique pourrait induire des erreurs dans le site en modifiant l’URL créée par les liens, en remplaçant `http://localhost:xxxx/Movies/Details/1` par quelque chose comme `http://localhost:xxxx/Movies/Details/12345` (ou une autre valeur qui ne représente pas un film réel).</span><span class="sxs-lookup"><span data-stu-id="35ec6-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="35ec6-112">Si vous n’avez pas recherché un film non valide, l’application lève une exception.</span><span class="sxs-lookup"><span data-stu-id="35ec6-112">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="35ec6-113">Examinez les méthodes `Delete` et `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="35ec6-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="35ec6-114">Notez que la méthode `HTTP GET Delete` ne supprime pas le film spécifié, mais retourne une vue du film où vous pouvez soumettre (HttpPost) la suppression.</span><span class="sxs-lookup"><span data-stu-id="35ec6-114">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="35ec6-115">L’exécution d’une opération de suppression en réponse à une requête GET (ou encore l’exécution d’une opération de modification, d’une opération de création ou de toute autre opération qui modifie des données) génère une faille de sécurité.</span><span class="sxs-lookup"><span data-stu-id="35ec6-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="35ec6-116">La méthode `[HttpPost]` qui supprime les données est nommée `DeleteConfirmed` pour donner à la méthode HTTP POST une signature ou un nom unique.</span><span class="sxs-lookup"><span data-stu-id="35ec6-116">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="35ec6-117">Les signatures des deux méthodes sont illustrées ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="35ec6-117">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="35ec6-118">Le Common Language Runtime (CLR) nécessite des méthodes surchargées pour avoir une signature à paramètre unique (même nom de méthode, mais liste de paramètres différentes).</span><span class="sxs-lookup"><span data-stu-id="35ec6-118">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="35ec6-119">Toutefois, vous avez ici besoin de deux méthodes `Delete` (une pour GET et une pour POST) ayant toutes les deux la même signature de paramètre.</span><span class="sxs-lookup"><span data-stu-id="35ec6-119">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="35ec6-120">(Elles doivent toutes les deux accepter un entier unique comme paramètre.)</span><span class="sxs-lookup"><span data-stu-id="35ec6-120">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="35ec6-121">Il existe deux approches pour ce problème. L’une consiste à attribuer aux méthodes des noms différents.</span><span class="sxs-lookup"><span data-stu-id="35ec6-121">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="35ec6-122">C’est ce qu’a fait le mécanisme de génération de modèles automatique dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="35ec6-122">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="35ec6-123">Toutefois, elle présente un petit problème : ASP.NET mappe des segments d’une URL à des méthodes d’action par nom. Si vous renommez une méthode, il est probable que le routage ne pourra pas trouver cette méthode.</span><span class="sxs-lookup"><span data-stu-id="35ec6-123">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="35ec6-124">La solution consiste à faire ce que vous voyez dans l’exemple, c’est-à-dire à ajouter l’attribut `ActionName("Delete")` à la méthode `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="35ec6-124">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="35ec6-125">Cet attribut effectue un mappage du système de routage afin qu’une URL qui inclut /Delete/ pour une requête POST trouve la méthode `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="35ec6-125">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="35ec6-126">Pour contourner le problème des méthodes qui ont des noms et des signatures identiques, vous pouvez également modifier artificiellement la signature de la méthode POST pour inclure un paramètre supplémentaire (inutilisé).</span><span class="sxs-lookup"><span data-stu-id="35ec6-126">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="35ec6-127">C’est ce que nous avons fait dans une publication précédente quand nous avons ajouté le paramètre `notUsed`.</span><span class="sxs-lookup"><span data-stu-id="35ec6-127">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="35ec6-128">Vous pouvez faire de même ici pour la méthode `[HttpPost] Delete` :</span><span class="sxs-lookup"><span data-stu-id="35ec6-128">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="35ec6-129">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="35ec6-129">Publish to Azure</span></span>

<span data-ttu-id="35ec6-130">Consultez la page [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour savoir comment publier cette application sur Azure à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="35ec6-130">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="35ec6-131">Il est également possible de la publier en [ligne de commande](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="35ec6-131">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="35ec6-132">Nous vous remercions d’avoir effectué cette introduction à ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="35ec6-132">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="35ec6-133">Vos commentaires nous intéressent.</span><span class="sxs-lookup"><span data-stu-id="35ec6-133">We appreciate any comments you leave.</span></span> <span data-ttu-id="35ec6-134">La rubrique [Bien démarrer avec MVC et EF Core](xref:data/ef-mvc/intro) est un excellent moyen de poursuivre après ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="35ec6-134">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="35ec6-135">Précédent</span><span class="sxs-lookup"><span data-stu-id="35ec6-135">Previous</span></span>](validation.md)
