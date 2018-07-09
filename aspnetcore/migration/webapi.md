---
title: Migrer à partir de l’API Web ASP.NET vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer d’une implémentation de l’API Web à partir de l’API Web ASP.NET vers ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894190"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="c64c5-103">Migrer à partir de l’API Web ASP.NET vers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c64c5-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="c64c5-104">Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c64c5-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c64c5-105">API Web sont des services HTTP qui atteignent une large gamme de clients, y compris les navigateurs et appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="c64c5-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="c64c5-106">ASP.NET Core MVC prend en charge pour la création d’API Web offre un moyen unique et homogène de la création d’applications web.</span><span class="sxs-lookup"><span data-stu-id="c64c5-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="c64c5-107">Dans cet article, nous montrons les étapes nécessaires pour migrer d’une implémentation de l’API Web à partir de l’API Web ASP.NET vers ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c64c5-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="c64c5-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c64c5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="c64c5-109">Projet d’API Web ASP.NET de révision</span><span class="sxs-lookup"><span data-stu-id="c64c5-109">Review ASP.NET Web API project</span></span>

<span data-ttu-id="c64c5-110">Cet article utilise l’exemple de projet *ProductsApp*, créé dans l’article [mise en route avec ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) comme point de départ.</span><span class="sxs-lookup"><span data-stu-id="c64c5-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="c64c5-111">Dans ce projet, un projet d’API Web ASP.NET simple est configuré comme suit.</span><span class="sxs-lookup"><span data-stu-id="c64c5-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="c64c5-112">Dans *Global.asax.cs*, un appel est effectué pour `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="c64c5-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="c64c5-113">`WebApiConfig` est défini dans *App_Start*, et a simplement une ligne statique `Register` méthode :</span><span class="sxs-lookup"><span data-stu-id="c64c5-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

<span data-ttu-id="c64c5-114">Cette classe configure [routage par attributs](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’il n’est pas réellement utilisé dans le projet.</span><span class="sxs-lookup"><span data-stu-id="c64c5-114">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="c64c5-115">Il configure également la table de routage, qui est utilisée par l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c64c5-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="c64c5-116">Dans ce cas, les API Web ASP.NET s’attendent des URL pour correspondre au format */api/ {controller} / {id}*, avec *{id}* est facultatif.</span><span class="sxs-lookup"><span data-stu-id="c64c5-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="c64c5-117">Le *ProductsApp* projet comprend qu’un seul contrôleur simple, qui hérite de `ApiController` et expose deux méthodes :</span><span class="sxs-lookup"><span data-stu-id="c64c5-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="c64c5-118">Enfin, le modèle, *produit*, utilisé par le *ProductsApp*, est une classe simple :</span><span class="sxs-lookup"><span data-stu-id="c64c5-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="c64c5-119">Maintenant que nous avons un projet simple à partir duquel commencer, nous pouvons illustrer comment migrer ce projet d’API Web vers ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c64c5-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="c64c5-120">Créer le projet de Destination</span><span class="sxs-lookup"><span data-stu-id="c64c5-120">Create the Destination Project</span></span>

<span data-ttu-id="c64c5-121">À l’aide de Visual Studio, créez une solution vide et nommez-la *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="c64c5-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="c64c5-122">Ajouter l’existant *ProductsApp* de projet à ce dernier, puis ajouter un nouveau projet d’Application Web ASP.NET Core à la solution.</span><span class="sxs-lookup"><span data-stu-id="c64c5-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="c64c5-123">Nommez le nouveau projet *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="c64c5-123">Name the new project *ProductsCore*.</span></span>

![Boîte de dialogue Nouveau projet ouvrir aux modèles Web](webapi/_static/add-web-project.png)

<span data-ttu-id="c64c5-125">Ensuite, choisissez le modèle de projet API Web.</span><span class="sxs-lookup"><span data-stu-id="c64c5-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="c64c5-126">Nous allons migrer le *ProductsApp* le contenu pour ce nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="c64c5-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Boîte de dialogue Nouveau Application Web avec le modèle de projet API Web sélectionné dans la liste des modèles ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="c64c5-128">Supprimer le `Project_Readme.html` fichier à partir du nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="c64c5-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="c64c5-129">Votre solution doit maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="c64c5-129">Your solution should now look like this:</span></span>

![Solution d’application ouverte dans l’Explorateur de solutions affichant les fichiers et dossiers des projets ProductsApp et ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="c64c5-131">Migrer la Configuration</span><span class="sxs-lookup"><span data-stu-id="c64c5-131">Migrate Configuration</span></span>

<span data-ttu-id="c64c5-132">ASP.NET Core n’utilise plus *Global.asax*, *web.config*, ou *App_Start* dossiers.</span><span class="sxs-lookup"><span data-stu-id="c64c5-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="c64c5-133">Au lieu de cela, toutes les tâches de démarrage sont effectuent dans *Startup.cs* à la racine du projet (consultez [démarrage de l’Application](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="c64c5-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="c64c5-134">Dans ASP.NET Core MVC, le routage basé sur l’attribut est désormais inclus par défaut lorsque `UseMvc()` est appelée ; et, il s’agit de l’approche recommandée pour la configuration des itinéraires de l’API Web (et comment le projet de démarrage des API Web gère le routage).</span><span class="sxs-lookup"><span data-stu-id="c64c5-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="c64c5-135">En supposant que vous souhaitez utiliser le routage par attributs dans votre projet à l’avenir, aucune configuration supplémentaire est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c64c5-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="c64c5-136">Simplement appliquer les attributs en fonction des besoins de vos contrôleurs et vos actions, comme dans l’exemple `ValuesController` classe qui est inclus dans le projet de démarrage des API Web :</span><span class="sxs-lookup"><span data-stu-id="c64c5-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="c64c5-137">Notez la présence de *[controller]* sur la ligne 8.</span><span class="sxs-lookup"><span data-stu-id="c64c5-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="c64c5-138">Le routage basé sur l’attribut maintenant prend en charge certains jetons, tels que *[controller]* et *[action]*.</span><span class="sxs-lookup"><span data-stu-id="c64c5-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="c64c5-139">Ces jetons sont remplacés lors de l’exécution avec le nom du contrôleur ou de l’action, respectivement, à laquelle l’attribut a été appliqué.</span><span class="sxs-lookup"><span data-stu-id="c64c5-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="c64c5-140">Ceci permet de réduire le nombre de chaînes magiques dans le projet, et elle garantit que les itinéraires restent synchronisés avec leurs contrôleurs correspondants et les actions lorsque les refactorisations de changement de nom automatique sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="c64c5-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="c64c5-141">Pour migrer le contrôleur d’API de produits, nous devons tout d’abord copier *ProductsController* vers le nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="c64c5-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="c64c5-142">Puis simplement inclure l’attribut d’itinéraire sur le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="c64c5-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="c64c5-143">Vous devez également ajouter le `[HttpGet]` d’attribut pour les deux méthodes, dans la mesure où ils sont tous deux doivent être appelées par le biais de HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="c64c5-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="c64c5-144">Inclure l’attente d’un paramètre « id » dans l’attribut pour `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="c64c5-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="c64c5-145">Routage à ce stade, est configuré correctement ; Toutefois, nous ne pouvons pas encore le tester.</span><span class="sxs-lookup"><span data-stu-id="c64c5-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="c64c5-146">Modifications supplémentaires doivent être effectuées avant *ProductsController* compilera.</span><span class="sxs-lookup"><span data-stu-id="c64c5-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="c64c5-147">Migrer des modèles et des contrôleurs</span><span class="sxs-lookup"><span data-stu-id="c64c5-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="c64c5-148">La dernière étape du processus de migration pour ce projet d’API Web simple consiste à copier sur les contrôleurs et tous les modèles qu’ils utilisent.</span><span class="sxs-lookup"><span data-stu-id="c64c5-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="c64c5-149">Dans ce cas, il suffit de copier *Controllers/ProductsController.cs* du projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="c64c5-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="c64c5-150">Ensuite, copiez la totalité du dossier des modèles à partir du projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="c64c5-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="c64c5-151">Ajuster les espaces de noms pour correspondre au nouveau nom de projet (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="c64c5-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="c64c5-152">À ce stade, vous pouvez générer l’application, et vous trouverez un nombre d’erreurs de compilation.</span><span class="sxs-lookup"><span data-stu-id="c64c5-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="c64c5-153">Il doivent généralement être comprises dans les catégories suivantes :</span><span class="sxs-lookup"><span data-stu-id="c64c5-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="c64c5-154">*ApiController* n’existe pas</span><span class="sxs-lookup"><span data-stu-id="c64c5-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="c64c5-155">*System.Web.Http* espace de noms n’existe pas</span><span class="sxs-lookup"><span data-stu-id="c64c5-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="c64c5-156">*IHttpActionResult* n’existe pas</span><span class="sxs-lookup"><span data-stu-id="c64c5-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="c64c5-157">Heureusement, il s’agit très faciles à corriger :</span><span class="sxs-lookup"><span data-stu-id="c64c5-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="c64c5-158">Modification *ApiController* à *contrôleur* (vous devrez peut-être ajouter *à l’aide de Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="c64c5-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="c64c5-159">Supprimer l’un à l’aide d’instruction qui fait référence à *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="c64c5-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="c64c5-160">Modifier toute méthode retournant *IHttpActionResult* pour retourner un *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="c64c5-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="c64c5-161">Une fois que ces modifications ont été apportées et inutilisées à l’aide d’instructions supprimé, migrées *ProductsController* classe ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="c64c5-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="c64c5-162">Vous devez maintenant être en mesure d’exécuter le projet migré et accédez à */api/produits*; et, vous devez voir la liste complète des 3 produits.</span><span class="sxs-lookup"><span data-stu-id="c64c5-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="c64c5-163">Accédez à */api/products/1* et vous devriez voir le premier produit.</span><span class="sxs-lookup"><span data-stu-id="c64c5-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a><span data-ttu-id="c64c5-164">Correctif de compatibilité ASP.NET 4.x Web API 2</span><span class="sxs-lookup"><span data-stu-id="c64c5-164">ASP.NET 4.x Web API 2 compatibility shim</span></span>

<span data-ttu-id="c64c5-165">Est un outil utile lors de la migration d’ASP.NET Web API projets vers ASP.NET Core la [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="c64c5-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="c64c5-166">Le shim de compatibilité étend ASP.NET Core pour autoriser un nombre de différentes conventions d’API Web 2 à utiliser.</span><span class="sxs-lookup"><span data-stu-id="c64c5-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="c64c5-167">L’exemple porté précédemment dans ce document est assez basique pour que le shim de compatibilité n’était pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c64c5-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="c64c5-168">Pour les grands projets, le shim de compatibilité peut être utile pour temporairement combler le fossé de API entre ASP.NET Core et ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="c64c5-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="c64c5-169">Le shim de compatibilité d’API Web est destiné à être utilisé en tant que mesure temporaire afin de faciliter la migration de grands projets d’API Web vers ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c64c5-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="c64c5-170">Au fil du temps, les projets doivent être mis à jour pour utiliser les modèles ASP.NET Core au lieu d’utiliser le shim de compatibilité.</span><span class="sxs-lookup"><span data-stu-id="c64c5-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="c64c5-171">Les fonctionnalités de compatibilité incluses dans `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluent :</span><span class="sxs-lookup"><span data-stu-id="c64c5-171">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="c64c5-172">Ajoute un `ApiController` tapez afin que les types de base des contrôleurs n’aient à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="c64c5-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="c64c5-173">Active la liaison de modèle de style Web API.</span><span class="sxs-lookup"><span data-stu-id="c64c5-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="c64c5-174">Fonctions de liaison du modèle MVC ASP.NET Core même vers MVC 5, par défaut.</span><span class="sxs-lookup"><span data-stu-id="c64c5-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="c64c5-175">Les modifications de shim de compatibilité liaison de données pour être plus similaire aux conventions de liaison de modèle API Web 2.</span><span class="sxs-lookup"><span data-stu-id="c64c5-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="c64c5-176">Par exemple, les types complexes sont liés automatiquement à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="c64c5-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="c64c5-177">Étend la liaison de modèle afin que les actions de contrôleur peuvent prendre des paramètres de type `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="c64c5-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="c64c5-178">Ajoute des formateurs de messages autorisant des actions pour retourner des résultats de type `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="c64c5-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="c64c5-179">Ajoute des méthodes de réponse supplémentaires qui ont utilisés pour traiter les réponses Web API 2 actions :</span><span class="sxs-lookup"><span data-stu-id="c64c5-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="c64c5-180">Générateurs de HttpResponseMessage :</span><span class="sxs-lookup"><span data-stu-id="c64c5-180">HttpResponseMessage generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="c64c5-181">Méthodes de résultat d’action :</span><span class="sxs-lookup"><span data-stu-id="c64c5-181">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="c64c5-182">Ajoute une instance de `IContentNegotiator` au conteneur d’injection de dépendances de l’application et les types liés à la négociation à partir du contenu est [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponible.</span><span class="sxs-lookup"><span data-stu-id="c64c5-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="c64c5-183">Cela inclut les types tels que `DefaultContentNegotiator`, `MediaTypeFormatter`, etc..</span><span class="sxs-lookup"><span data-stu-id="c64c5-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="c64c5-184">Pour utiliser le shim de compatibilité, vous devez :</span><span class="sxs-lookup"><span data-stu-id="c64c5-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="c64c5-185">Installer le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="c64c5-185">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="c64c5-186">Inscrire les services du shim de compatibilité avec le conteneur d’injection de dépendances de l’application en appelant `services.AddMvc().AddWebApiConventions()` dans l’application `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="c64c5-186">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="c64c5-187">Définir des itinéraires spécifiques de l’API Web à l’aide de `MapWebApiRoute` sur le `IRouteBuilder` dans l’application `IApplicationBuilder.UseMvc` appeler.</span><span class="sxs-lookup"><span data-stu-id="c64c5-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="c64c5-188">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="c64c5-188">Summary</span></span>

<span data-ttu-id="c64c5-189">Migration d’un projet d’API Web ASP.NET simple à ASP.NET Core MVC est assez simple, grâce à la prise en charge intégrée pour les API Web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c64c5-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="c64c5-190">Les éléments principaux que doivent faire migrer chaque projet d’API Web ASP.NET sont des itinéraires, contrôleurs et modèles, ainsi que des mises à jour pour les types utilisés par les contrôleurs et les actions.</span><span class="sxs-lookup"><span data-stu-id="c64c5-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
