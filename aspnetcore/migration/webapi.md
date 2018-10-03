---
title: Migrer à partir de l’API Web ASP.NET vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer d’une implémentation de l’API web à partir de l’API Web ASP.NET 4.x vers ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: 3c4ded874de2700e1290022a535c08f30dce9490
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47861016"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="59a61-103">Migrer à partir de l’API Web ASP.NET vers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59a61-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="59a61-104">Par [Scott Addie](https://twitter.com/scott_addie) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="59a61-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="59a61-105">API Web ASP.NET 4.x est un service HTTP qui atteigne un large éventail de clients, y compris les navigateurs et appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="59a61-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="59a61-106">ASP.NET Core unifie ASP.NET du 4.x MVC et modèles de l’application API Web dans un modèle de programmation plus simple, appelé ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="59a61-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="59a61-107">Cet article illustre les étapes nécessaires pour migrer à partir de l’API Web ASP.NET 4.x vers ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="59a61-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="59a61-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="59a61-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59a61-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="59a61-109">Prerequisites</span></span>

* [<span data-ttu-id="59a61-110">SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="59a61-110">.NET Core 2.1 SDK or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="59a61-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 ou ultérieure avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="59a61-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="59a61-112">Passez en revue le projet d’API Web ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="59a61-112">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="59a61-113">En tant que point de départ, cet article utilise le *ProductsApp* projet créé dans [mise en route avec ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="59a61-113">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="59a61-114">Dans ce projet, un projet d’API Web ASP.NET 4.x simple est configuré comme suit.</span><span class="sxs-lookup"><span data-stu-id="59a61-114">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="59a61-115">Dans *Global.asax.cs*, un appel est effectué pour `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="59a61-115">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="59a61-116">`WebApiConfig` est défini dans le *App_Start* dossier.</span><span class="sxs-lookup"><span data-stu-id="59a61-116">`WebApiConfig` is defined in the *App_Start* folder.</span></span> <span data-ttu-id="59a61-117">Il a simplement une ligne statique `Register` méthode :</span><span class="sxs-lookup"><span data-stu-id="59a61-117">It has just one static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

<span data-ttu-id="59a61-118">Cette classe configure [routage par attributs](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’il n’est pas réellement utilisé dans le projet.</span><span class="sxs-lookup"><span data-stu-id="59a61-118">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="59a61-119">Il configure également la table de routage, qui est utilisée par l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59a61-119">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="59a61-120">Dans ce cas, les API Web ASP.NET 4.x attend les URL pour correspondre au format `/api/{controller}/{id}`, avec `{id}` est facultatif.</span><span class="sxs-lookup"><span data-stu-id="59a61-120">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="59a61-121">Le *ProductsApp* projet comprend un seul contrôleur.</span><span class="sxs-lookup"><span data-stu-id="59a61-121">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="59a61-122">Le contrôleur hérite `ApiController` et expose deux méthodes :</span><span class="sxs-lookup"><span data-stu-id="59a61-122">The controller inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="59a61-123">Le `Product` modèle utilisé par `ProductsController` est une classe simple :</span><span class="sxs-lookup"><span data-stu-id="59a61-123">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="59a61-124">Les sections suivantes expliquent la migration du projet API Web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="59a61-124">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="59a61-125">Créer le projet de destination</span><span class="sxs-lookup"><span data-stu-id="59a61-125">Create destination project</span></span>

<span data-ttu-id="59a61-126">Procédez comme suit dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="59a61-126">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="59a61-127">Accédez à **fichier** > **nouveau** > **projet** > **autres Types de projets**  >  **Solutions visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="59a61-127">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="59a61-128">Sélectionnez **nouvelle Solution**et nommez la solution *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="59a61-128">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="59a61-129">Cliquez sur le **OK** bouton.</span><span class="sxs-lookup"><span data-stu-id="59a61-129">Click the **OK** button.</span></span>
* <span data-ttu-id="59a61-130">Ajouter l’existant *ProductsApp* projet à la solution.</span><span class="sxs-lookup"><span data-stu-id="59a61-130">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="59a61-131">Ajouter un nouveau **Application Web ASP.NET Core** projet à la solution.</span><span class="sxs-lookup"><span data-stu-id="59a61-131">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="59a61-132">Sélectionnez le **.NET Core** ciblent framework à partir de la liste déroulante, puis sélectionnez le **API** modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="59a61-132">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="59a61-133">Nommez le projet *ProductsCore*, puis cliquez sur le **OK** bouton.</span><span class="sxs-lookup"><span data-stu-id="59a61-133">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="59a61-134">La solution contient maintenant deux projets.</span><span class="sxs-lookup"><span data-stu-id="59a61-134">The solution now contains two projects.</span></span> <span data-ttu-id="59a61-135">Les sections suivantes expliquent de migration le *ProductsApp* le contenu du projet pour le *ProductsCore* projet.</span><span class="sxs-lookup"><span data-stu-id="59a61-135">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="59a61-136">Migrer la configuration</span><span class="sxs-lookup"><span data-stu-id="59a61-136">Migrate configuration</span></span>

<span data-ttu-id="59a61-137">ASP.NET Core n’utilise pas le *App_Start* dossier ou le *Global.asax* fichier et le *web.config* fichier est ajouté au moment de la publication.</span><span class="sxs-lookup"><span data-stu-id="59a61-137">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="59a61-138">*Startup.cs* remplace *Global.asax* et se trouve dans la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="59a61-138">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="59a61-139">Le `Startup` classe gère toutes les tâches de démarrage d’application.</span><span class="sxs-lookup"><span data-stu-id="59a61-139">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="59a61-140">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="59a61-140">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="59a61-141">Dans ASP.NET Core MVC, le routage par attributs est inclus par défaut lorsque <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> est appelée `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="59a61-141">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="59a61-142">Ce qui suit `UseMvc` appeler remplace le *ProductsApp* du projet *app_start/webapiconfig.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="59a61-142">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="59a61-143">Migrer des modèles et des contrôleurs</span><span class="sxs-lookup"><span data-stu-id="59a61-143">Migrate models and controllers</span></span>

<span data-ttu-id="59a61-144">Recopiez les *ProductApp* contrôleur du projet et le modèle utilisé.</span><span class="sxs-lookup"><span data-stu-id="59a61-144">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="59a61-145">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="59a61-145">Follow these steps:</span></span>

1. <span data-ttu-id="59a61-146">Copie *Controllers/ProductsController.cs* du projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="59a61-146">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="59a61-147">Copiez l’intégralité de *modèles* dossier du projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="59a61-147">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="59a61-148">Modifier les espaces de noms des fichiers copiés pour correspondre au nouveau nom de projet (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="59a61-148">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="59a61-149">Ajuster la `using ProductsApp.Models;` instruction dans *ProductsController.cs* trop.</span><span class="sxs-lookup"><span data-stu-id="59a61-149">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="59a61-150">Génération à ce stade, les résultats de l’application dans un nombre d’erreurs de compilation.</span><span class="sxs-lookup"><span data-stu-id="59a61-150">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="59a61-151">Les erreurs se produisent, car les composants suivants n’existent pas dans ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="59a61-151">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="59a61-152">Classe `ApiController`</span><span class="sxs-lookup"><span data-stu-id="59a61-152">`ApiController` class</span></span>
* <span data-ttu-id="59a61-153">Espace de noms `System.Web.Http`</span><span class="sxs-lookup"><span data-stu-id="59a61-153">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="59a61-154">`IHttpActionResult` Interface</span><span class="sxs-lookup"><span data-stu-id="59a61-154">`IHttpActionResult` interface</span></span>

<span data-ttu-id="59a61-155">Corrigez les erreurs comme suit :</span><span class="sxs-lookup"><span data-stu-id="59a61-155">Fix the errors as follows:</span></span>

1. <span data-ttu-id="59a61-156">Modification `ApiController` à <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="59a61-156">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="59a61-157">Ajouter `using Microsoft.AspNetCore.Mvc;` pour résoudre le `ControllerBase` référence.</span><span class="sxs-lookup"><span data-stu-id="59a61-157">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="59a61-158">Supprimez `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="59a61-158">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="59a61-159">Modifier le `GetProduct` type de retour de l’action à partir de `IHttpActionResult` à `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="59a61-159">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

## <a name="configure-routing"></a><span data-ttu-id="59a61-160">Configurer le routage</span><span class="sxs-lookup"><span data-stu-id="59a61-160">Configure routing</span></span>

<span data-ttu-id="59a61-161">Configurer le routage comme suit :</span><span class="sxs-lookup"><span data-stu-id="59a61-161">Configure routing as follows:</span></span>

1. <span data-ttu-id="59a61-162">Décorer la `ProductsController` classe avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="59a61-162">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="59a61-163">L’exemple précédent [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribut configure le modèle de routage attribut du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="59a61-163">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="59a61-164">Le [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribut rend le routage d’attributs requis pour toutes les actions dans ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="59a61-164">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="59a61-165">Routage par attributs prend en charge les jetons, tels que `[controller]` et `[action]`.</span><span class="sxs-lookup"><span data-stu-id="59a61-165">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="59a61-166">Lors de l’exécution, chaque jeton est remplacé par le nom du contrôleur ou de l’action, respectivement, à laquelle l’attribut a été appliqué.</span><span class="sxs-lookup"><span data-stu-id="59a61-166">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="59a61-167">Les jetons de réduisent le nombre de chaînes magiques dans le projet.</span><span class="sxs-lookup"><span data-stu-id="59a61-167">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="59a61-168">Les jetons Vérifiez également les itinéraires restent synchronisés avec les contrôleurs de correspondants et les actions lorsque automatique renommez refactorisations sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="59a61-168">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="59a61-169">Activer les demandes HTTP Get pour le `ProductController` actions :</span><span class="sxs-lookup"><span data-stu-id="59a61-169">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="59a61-170">Appliquer le [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribut le `GetAllProducts` action.</span><span class="sxs-lookup"><span data-stu-id="59a61-170">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="59a61-171">Appliquer le `[HttpGet("{id}")]` attribut le `GetProduct` action.</span><span class="sxs-lookup"><span data-stu-id="59a61-171">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="59a61-172">Après ces modifications et la suppression d’inutilisé `using` instructions, *ProductsController.cs* fichier ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="59a61-172">After these changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="59a61-173">Exécutez le projet migré et accédez à `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="59a61-173">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="59a61-174">Une liste complète des trois produits s’affiche.</span><span class="sxs-lookup"><span data-stu-id="59a61-174">A full list of three products appears.</span></span> <span data-ttu-id="59a61-175">Accédez à `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="59a61-175">Browse to `/api/products/1`.</span></span> <span data-ttu-id="59a61-176">Le premier produit s’affiche.</span><span class="sxs-lookup"><span data-stu-id="59a61-176">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="59a61-177">Correctif de compatibilité</span><span class="sxs-lookup"><span data-stu-id="59a61-177">Compatibility shim</span></span>

<span data-ttu-id="59a61-178">Le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) bibliothèque fournit un shim de compatibilité pour déplacer les projets d’API Web ASP.NET 4.x vers ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59a61-178">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="59a61-179">Le shim de compatibilité étend ASP.NET Core pour prendre en charge un nombre de conventions d’ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="59a61-179">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="59a61-180">L’exemple porté précédemment dans ce document est assez basique pour que le shim de compatibilité était inutile.</span><span class="sxs-lookup"><span data-stu-id="59a61-180">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="59a61-181">Pour les grands projets, le shim de compatibilité peut être utile pour temporairement combler le fossé de API entre ASP.NET Core et ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="59a61-181">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="59a61-182">Le shim de compatibilité d’API Web est destiné à être utilisé en tant que mesure temporaire pour prendre en charge la migration volumineux API Web les projets ASP.NET 4.x vers ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59a61-182">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="59a61-183">Au fil du temps, les projets doivent être mis à jour pour utiliser les modèles ASP.NET Core au lieu d’utiliser le shim de compatibilité.</span><span class="sxs-lookup"><span data-stu-id="59a61-183">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="59a61-184">Les fonctionnalités de compatibilité incluses dans `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluent :</span><span class="sxs-lookup"><span data-stu-id="59a61-184">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="59a61-185">Ajoute un `ApiController` tapez afin que les types de base des contrôleurs n’aient à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="59a61-185">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="59a61-186">Active la liaison de modèle de style Web API.</span><span class="sxs-lookup"><span data-stu-id="59a61-186">Enables Web API-style model binding.</span></span> <span data-ttu-id="59a61-187">ASP.NET Core MVC de modèle des fonctions de liaison de la même façon à celui d’ASP.NET 4.x MVC 5, par défaut.</span><span class="sxs-lookup"><span data-stu-id="59a61-187">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="59a61-188">Les modifications de shim de compatibilité liaison de données pour être plus similaire aux conventions de liaison de modèle de 4.x API Web 2 ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59a61-188">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="59a61-189">Par exemple, les types complexes sont liés automatiquement à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="59a61-189">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="59a61-190">Étend la liaison de modèle afin que les actions de contrôleur peuvent prendre des paramètres de type `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="59a61-190">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="59a61-191">Ajoute des formateurs de messages autorisant des actions pour retourner des résultats de type `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="59a61-191">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="59a61-192">Ajoute des méthodes de réponse supplémentaires qui ont utilisés pour traiter les réponses Web API 2 actions :</span><span class="sxs-lookup"><span data-stu-id="59a61-192">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="59a61-193">`HttpResponseMessage` générateurs de :</span><span class="sxs-lookup"><span data-stu-id="59a61-193">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="59a61-194">Méthodes de résultat d’action :</span><span class="sxs-lookup"><span data-stu-id="59a61-194">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="59a61-195">Ajoute une instance de `IContentNegotiator` à l’application de conteneur d’injection (DI) et rend disponibles les types liés à la négociation de contenu à partir de [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="59a61-195">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="59a61-196">Exemples de ces types de `DefaultContentNegotiator` et `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="59a61-196">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="59a61-197">Pour utiliser le shim de compatibilité :</span><span class="sxs-lookup"><span data-stu-id="59a61-197">To use the compatibility shim:</span></span>

1. <span data-ttu-id="59a61-198">Installer le [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="59a61-198">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="59a61-199">Inscrire les services du shim de compatibilité avec le conteneur d’injection de dépendances de l’application en appelant `services.AddMvc().AddWebApiConventions()` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="59a61-199">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="59a61-200">Définir achemine les API spécifiques à l’aide de web `MapWebApiRoute` sur le `IRouteBuilder` dans l’application `IApplicationBuilder.UseMvc` appeler.</span><span class="sxs-lookup"><span data-stu-id="59a61-200">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59a61-201">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="59a61-201">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
