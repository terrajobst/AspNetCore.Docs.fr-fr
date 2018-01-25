---
title: "Migration à partir de l’API Web ASP.NET"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 39224d1b2b0a54b043aa279659ff38134c5af7b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="71b7d-102">Migration à partir de l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="71b7d-102">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="71b7d-103">Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="71b7d-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="71b7d-104">API Web est des services HTTP qui atteignent un large éventail de clients, y compris les navigateurs et périphériques mobiles.</span><span class="sxs-lookup"><span data-stu-id="71b7d-104">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="71b7d-105">Base d’ASP.NET MVC prend en charge les API Web offre un moyen unique et homogène de la création d’applications web de génération.</span><span class="sxs-lookup"><span data-stu-id="71b7d-105">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="71b7d-106">Dans cet article, nous allons montrer les étapes requises pour migrer une implémentation de l’API Web à partir de l’API Web ASP.NET vers ASP.NET MVC de base.</span><span class="sxs-lookup"><span data-stu-id="71b7d-106">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="71b7d-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71b7d-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="71b7d-108">Projet d’API Web ASP.NET révision</span><span class="sxs-lookup"><span data-stu-id="71b7d-108">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="71b7d-109">Cet article utilise l’exemple de projet *ProductsApp*, créé dans l’article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) comme point de départ.</span><span class="sxs-lookup"><span data-stu-id="71b7d-109">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="71b7d-110">Dans ce projet, un projet d’API Web ASP.NET simple est configuré comme suit.</span><span class="sxs-lookup"><span data-stu-id="71b7d-110">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="71b7d-111">Dans *Global.asax.cs*, un appel est fait à `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="71b7d-111">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="71b7d-112">`WebApiConfig`est défini dans *App_Start*, et a simplement une ligne statique `Register` méthode :</span><span class="sxs-lookup"><span data-stu-id="71b7d-112">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="71b7d-113">Cette classe configure [attribut routage](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’il n’est pas réellement utilisé dans le projet.</span><span class="sxs-lookup"><span data-stu-id="71b7d-113">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="71b7d-114">Il configure également la table de routage qui est utilisée par l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71b7d-114">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="71b7d-115">Dans ce cas, API Web ASP.NET s’attend à recevoir les URL au format */api/ {controller} / {id}*, avec *{id}* est facultatif.</span><span class="sxs-lookup"><span data-stu-id="71b7d-115">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="71b7d-116">Le *ProductsApp* projet comprend qu’un seul contrôleur simple, qui hérite de `ApiController` et expose deux méthodes :</span><span class="sxs-lookup"><span data-stu-id="71b7d-116">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="71b7d-117">Enfin, le modèle, *produit*, utilisée par le *ProductsApp*, est une classe simple :</span><span class="sxs-lookup"><span data-stu-id="71b7d-117">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="71b7d-118">Maintenant que nous avons un projet simple à partir duquel commencer, nous pouvons montrent comment migrer ce projet d’API Web pour ASP.NET MVC de base.</span><span class="sxs-lookup"><span data-stu-id="71b7d-118">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="71b7d-119">Créer le projet de Destination</span><span class="sxs-lookup"><span data-stu-id="71b7d-119">Create the Destination Project</span></span>

<span data-ttu-id="71b7d-120">À l’aide de Visual Studio, créez une solution vide et nommez-la *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="71b7d-120">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="71b7d-121">Ajouter existant *ProductsApp* de projet à ce dernier, puis ajouter un nouveau projet d’Application Web ASP.NET Core à la solution.</span><span class="sxs-lookup"><span data-stu-id="71b7d-121">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="71b7d-122">Nommez le nouveau projet *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="71b7d-122">Name the new project *ProductsCore*.</span></span>

![Boîte de dialogue Nouveau projet ouvrir avec des modèles Web](webapi/_static/add-web-project.png)

<span data-ttu-id="71b7d-124">Ensuite, choisissez le modèle de projet d’API Web.</span><span class="sxs-lookup"><span data-stu-id="71b7d-124">Next, choose the Web API project template.</span></span> <span data-ttu-id="71b7d-125">Nous allons migrer le *ProductsApp* le contenu pour ce nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="71b7d-125">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Boîte de dialogue nouvelle Application Web avec le modèle de projet d’API Web sélectionné dans la liste des modèles ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="71b7d-127">Supprimer le `Project_Readme.html` le fichier à partir du nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="71b7d-127">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="71b7d-128">Votre solution doit maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="71b7d-128">Your solution should now look like this:</span></span>

![Solution d’application ouverte dans l’Explorateur de solutions affichant les fichiers et dossiers des projets ProductsApp et ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="71b7d-130">Migration de Configuration</span><span class="sxs-lookup"><span data-stu-id="71b7d-130">Migrate Configuration</span></span>

<span data-ttu-id="71b7d-131">ASP.NET Core n’utilise plus *Global.asax*, *web.config*, ou *App_Start* dossiers.</span><span class="sxs-lookup"><span data-stu-id="71b7d-131">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="71b7d-132">Au lieu de cela, toutes les tâches de démarrage sont effectuées en *Startup.cs* à la racine du projet (consultez [démarrage de l’Application](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="71b7d-132">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="71b7d-133">Dans ASP.NET MVC de base, le routage basé sur l’attribut est à présent inclus par défaut lorsque `UseMvc()` est appelé ; et, il s’agit de l’approche recommandée pour la configuration des itinéraires de l’API Web (et comment le projet de démarrage des API Web gère le routage).</span><span class="sxs-lookup"><span data-stu-id="71b7d-133">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="71b7d-134">En supposant que vous souhaitez utiliser le routage d’attributs dans votre projet à l’avenir, aucune configuration supplémentaire n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="71b7d-134">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="71b7d-135">Simplement appliquer les attributs en fonction des besoins de vos contrôleurs et vos actions, comme dans l’exemple `ValuesController` classe qui est inclus dans le projet de démarrage des API Web :</span><span class="sxs-lookup"><span data-stu-id="71b7d-135">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="71b7d-136">Notez la présence de *[controller]* sur la ligne 8.</span><span class="sxs-lookup"><span data-stu-id="71b7d-136">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="71b7d-137">Le routage basé sur l’attribut maintenant prend en charge certains jetons, tels que *[controller]* et *[action]*.</span><span class="sxs-lookup"><span data-stu-id="71b7d-137">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="71b7d-138">Ces jetons sont remplacés lors de l’exécution avec le nom du contrôleur ou action, respectivement, à laquelle l’attribut a été appliqué.</span><span class="sxs-lookup"><span data-stu-id="71b7d-138">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="71b7d-139">Ceci permet de réduire le nombre de chaînes magiques dans le projet, et il garantit que les itinéraires restent synchronisés avec leurs contrôleurs correspondants et leurs actions lorsque refactorisations de changement de nom automatique sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="71b7d-139">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="71b7d-140">Pour migrer le contrôleur d’API de produits, nous devons d’abord copier *ProductsController* vers le nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="71b7d-140">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="71b7d-141">Insérez ensuite simplement l’attribut d’itinéraire sur le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="71b7d-141">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="71b7d-142">Vous devez également ajouter le `[HttpGet]` d’attributs pour les deux méthodes, étant donné que les deux doivent être appelés via HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="71b7d-142">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="71b7d-143">Inclure l’attente d’un paramètre « id » dans l’attribut pour `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="71b7d-143">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="71b7d-144">À ce stade, le routage est correcte ; Toutefois, nous ne pouvons pas encore le tester.</span><span class="sxs-lookup"><span data-stu-id="71b7d-144">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="71b7d-145">Des modifications supplémentaires doivent être effectuées avant *ProductsController* peut être compilé.</span><span class="sxs-lookup"><span data-stu-id="71b7d-145">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="71b7d-146">Migrer des modèles et des contrôleurs</span><span class="sxs-lookup"><span data-stu-id="71b7d-146">Migrate Models and Controllers</span></span>

<span data-ttu-id="71b7d-147">La dernière étape du processus de migration pour ce projet d’API Web simple consiste à copier sur les contrôleurs et les modèles qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="71b7d-147">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="71b7d-148">Dans ce cas, il suffit de copier *Controllers/ProductsController.cs* à partir du projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="71b7d-148">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="71b7d-149">Ensuite, copiez la totalité du dossier des modèles de projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="71b7d-149">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="71b7d-150">Ajuster les espaces de noms pour le nouveau nom de projet (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="71b7d-150">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="71b7d-151">À ce stade, vous pouvez générer l’application, et vous trouverez un nombre d’erreurs de compilation.</span><span class="sxs-lookup"><span data-stu-id="71b7d-151">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="71b7d-152">Il doivent généralement être comprises dans les catégories suivantes :</span><span class="sxs-lookup"><span data-stu-id="71b7d-152">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="71b7d-153">*ApiController* n’existe pas</span><span class="sxs-lookup"><span data-stu-id="71b7d-153">*ApiController* does not exist</span></span>

* <span data-ttu-id="71b7d-154">*System.Web.Http* espace de noms n’existe pas</span><span class="sxs-lookup"><span data-stu-id="71b7d-154">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="71b7d-155">*IHttpActionResult* n’existe pas</span><span class="sxs-lookup"><span data-stu-id="71b7d-155">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="71b7d-156">Heureusement, il s’agit très faciles à corriger :</span><span class="sxs-lookup"><span data-stu-id="71b7d-156">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="71b7d-157">Modification *ApiController* à *contrôleur* (il se pouvez que vous deviez ajouter *à l’aide de Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="71b7d-157">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="71b7d-158">Supprimer tout à l’aide d’instruction faisant référence à *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="71b7d-158">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="71b7d-159">Modifier toute méthode retournant *IHttpActionResult* pour renvoyer un *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="71b7d-159">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="71b7d-160">Une fois ces modifications ont été apportées et inutilisés à l’aide d’instructions supprimés, migrées *ProductsController* classe ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="71b7d-160">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="71b7d-161">Vous devez maintenant être en mesure d’exécuter le projet migré et accédez à */api/produits*; et, vous devez voir la liste complète des 3 produits.</span><span class="sxs-lookup"><span data-stu-id="71b7d-161">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="71b7d-162">Accédez à */api/products/1* et vous devez voir le premier produit.</span><span class="sxs-lookup"><span data-stu-id="71b7d-162">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="71b7d-163">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="71b7d-163">Summary</span></span>

<span data-ttu-id="71b7d-164">Migration d’un projet d’API Web ASP.NET simple vers ASP.NET MVC de base est assez simple, grâce à la prise en charge intégrée pour les API Web dans ASP.NET MVC de base.</span><span class="sxs-lookup"><span data-stu-id="71b7d-164">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="71b7d-165">Le principal que doivent faire migrer tous les projets API Web ASP.NET sont itinéraires, les contrôleurs et les modèles, ainsi que des mises à jour pour les types utilisés par les contrôleurs et les actions.</span><span class="sxs-lookup"><span data-stu-id="71b7d-165">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
