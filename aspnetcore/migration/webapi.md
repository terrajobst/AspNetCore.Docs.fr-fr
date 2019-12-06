---
title: Migrer de API Web ASP.NET vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer une implémentation d’API Web à partir de l’API Web ASP.NET 4. x vers ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: c68cf83f427f53b110075168c6d5e4d021808782
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881145"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="f44e2-103">Migrer de API Web ASP.NET vers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f44e2-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="f44e2-104">Par [Scott Addie](https://twitter.com/scott_addie) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f44e2-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f44e2-105">Une API Web ASP.NET 4. x est un service HTTP qui atteint un large éventail de clients, y compris des navigateurs et des appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="f44e2-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="f44e2-106">ASP.NET Core unifie les modèles d’application MVC et API Web de ASP.NET 4. x en un modèle de programmation plus simple, connu sous le nom de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f44e2-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="f44e2-107">Cet article décrit les étapes nécessaires à la migration de l’API Web ASP.NET 4. x vers ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f44e2-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="f44e2-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f44e2-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f44e2-109">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="f44e2-109">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="f44e2-110">Examiner le projet d’API Web ASP.NET 4. x</span><span class="sxs-lookup"><span data-stu-id="f44e2-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="f44e2-111">En guise de point de départ, cet article utilise le projet *ProductsApp* créé dans [prise en main avec API Web ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="f44e2-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="f44e2-112">Dans ce projet, un simple projet d’API Web ASP.NET 4. x est configuré comme suit.</span><span class="sxs-lookup"><span data-stu-id="f44e2-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="f44e2-113">Dans *global.asax.cs*, un appel est effectué pour `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="f44e2-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="f44e2-114">La classe `WebApiConfig` se trouve dans le dossier *App_Start* et a une méthode `Register` statique :</span><span class="sxs-lookup"><span data-stu-id="f44e2-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="f44e2-115">Cette classe configure le [routage des attributs](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), bien qu’elle ne soit pas réellement utilisée dans le projet.</span><span class="sxs-lookup"><span data-stu-id="f44e2-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="f44e2-116">Elle configure également la table de routage, qui est utilisée par API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f44e2-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="f44e2-117">Dans ce cas, l’API Web ASP.NET 4. x s’attend à ce que les URL correspondent au format `/api/{controller}/{id}`, `{id}` étant facultatif.</span><span class="sxs-lookup"><span data-stu-id="f44e2-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="f44e2-118">Le projet *ProductsApp* comprend un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f44e2-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="f44e2-119">Le contrôleur hérite de `ApiController` et contient deux actions :</span><span class="sxs-lookup"><span data-stu-id="f44e2-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="f44e2-120">Le modèle de `Product` utilisé par `ProductsController` est une classe simple :</span><span class="sxs-lookup"><span data-stu-id="f44e2-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="f44e2-121">Les sections suivantes montrent comment migrer le projet d’API Web vers ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f44e2-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="f44e2-122">Créer un projet de destination</span><span class="sxs-lookup"><span data-stu-id="f44e2-122">Create destination project</span></span>

<span data-ttu-id="f44e2-123">Effectuez les étapes suivantes dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="f44e2-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="f44e2-124">Accédez à **fichier** > **nouveau** **projet** >  > **autres types de projets** > **solutions Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="f44e2-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="f44e2-125">Sélectionnez **solution vide**et nommez la solution *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="f44e2-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="f44e2-126">Cliquez sur le bouton **OK** .</span><span class="sxs-lookup"><span data-stu-id="f44e2-126">Click the **OK** button.</span></span>
* <span data-ttu-id="f44e2-127">Ajoutez le projet *ProductsApp* existant à la solution.</span><span class="sxs-lookup"><span data-stu-id="f44e2-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="f44e2-128">Ajoutez un nouveau **ASP.net Core projet d’application Web** à la solution.</span><span class="sxs-lookup"><span data-stu-id="f44e2-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="f44e2-129">Sélectionnez le Framework cible **.net Core** dans la liste déroulante, puis sélectionnez le modèle de projet **API** .</span><span class="sxs-lookup"><span data-stu-id="f44e2-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="f44e2-130">Nommez le projet *ProductsCore*, puis cliquez sur le bouton **OK** .</span><span class="sxs-lookup"><span data-stu-id="f44e2-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="f44e2-131">La solution contient maintenant deux projets.</span><span class="sxs-lookup"><span data-stu-id="f44e2-131">The solution now contains two projects.</span></span> <span data-ttu-id="f44e2-132">Les sections suivantes expliquent comment migrer le contenu du projet *ProductsApp* vers le projet *ProductsCore* .</span><span class="sxs-lookup"><span data-stu-id="f44e2-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="f44e2-133">Migrer la configuration</span><span class="sxs-lookup"><span data-stu-id="f44e2-133">Migrate configuration</span></span>

<span data-ttu-id="f44e2-134">ASP.NET Core n’utilise pas le dossier *App_Start* ou le fichier *global. asax* , et le fichier *Web. config* est ajouté au moment de la publication.</span><span class="sxs-lookup"><span data-stu-id="f44e2-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="f44e2-135">*Startup.cs* est le remplacement de *global. asax* et se trouve à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="f44e2-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="f44e2-136">La classe `Startup` gère toutes les tâches de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="f44e2-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="f44e2-137">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="f44e2-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="f44e2-138">Dans ASP.NET Core MVC, le routage des attributs est inclus par défaut lors de l’appel de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="f44e2-139">L’appel App_Start de `UseMvc` suivant remplace le fichier */webapiconfig.cs* du projet *ProductsApp* :</span><span class="sxs-lookup"><span data-stu-id="f44e2-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="f44e2-140">Migrer les modèles et les contrôleurs</span><span class="sxs-lookup"><span data-stu-id="f44e2-140">Migrate models and controllers</span></span>

<span data-ttu-id="f44e2-141">Copiez sur le contrôleur du projet *ProductApp* et le modèle qu’il utilise.</span><span class="sxs-lookup"><span data-stu-id="f44e2-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="f44e2-142">Suivez les étapes ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f44e2-142">Follow these steps:</span></span>

1. <span data-ttu-id="f44e2-143">Copiez les *contrôleurs/ProductsController. cs* du projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="f44e2-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="f44e2-144">Copiez le dossier *Models* entier du projet d’origine vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="f44e2-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="f44e2-145">Modifiez les espaces de noms des fichiers copiés pour qu’ils correspondent au nouveau nom du projet (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="f44e2-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="f44e2-146">Ajustez également l’instruction `using ProductsApp.Models;` dans *ProductsController.cs* .</span><span class="sxs-lookup"><span data-stu-id="f44e2-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="f44e2-147">À ce stade, la génération de l’application génère un certain nombre d’erreurs de compilation.</span><span class="sxs-lookup"><span data-stu-id="f44e2-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="f44e2-148">Les erreurs se produisent parce que les composants suivants n’existent pas dans ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="f44e2-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="f44e2-149">Classe `ApiController`</span><span class="sxs-lookup"><span data-stu-id="f44e2-149">`ApiController` class</span></span>
* <span data-ttu-id="f44e2-150">Espace de noms `System.Web.Http`</span><span class="sxs-lookup"><span data-stu-id="f44e2-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="f44e2-151">Interface `IHttpActionResult`</span><span class="sxs-lookup"><span data-stu-id="f44e2-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="f44e2-152">Corrigez les erreurs comme suit :</span><span class="sxs-lookup"><span data-stu-id="f44e2-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="f44e2-153">Modification `ApiController` à <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="f44e2-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="f44e2-154">Ajoutez `using Microsoft.AspNetCore.Mvc;` pour résoudre la référence `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="f44e2-155">Supprimez `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="f44e2-156">Remplacez le type de retour de l’action `GetProduct` `IHttpActionResult` par `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="f44e2-157">Simplifiez l’instruction `return` de l’action `GetProduct` comme suit :</span><span class="sxs-lookup"><span data-stu-id="f44e2-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="f44e2-158">Configurer le routage</span><span class="sxs-lookup"><span data-stu-id="f44e2-158">Configure routing</span></span>

<span data-ttu-id="f44e2-159">Configurez le routage comme suit :</span><span class="sxs-lookup"><span data-stu-id="f44e2-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="f44e2-160">Marquez la classe `ProductsController` avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="f44e2-160">Mark the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="f44e2-161">L’attribut [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) précédent configure le modèle de routage des attributs du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f44e2-161">The preceding [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="f44e2-162">L’attribut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) rend le routage des attributs obligatoire pour toutes les actions dans ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f44e2-162">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="f44e2-163">Le routage d’attributs prend en charge les jetons, tels que `[controller]` et `[action]`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="f44e2-164">Lors de l’exécution, chaque jeton est remplacé par le nom du contrôleur ou de l’action, respectivement, auquel l’attribut a été appliqué.</span><span class="sxs-lookup"><span data-stu-id="f44e2-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="f44e2-165">Les jetons réduisent le nombre de chaînes magiques dans le projet.</span><span class="sxs-lookup"><span data-stu-id="f44e2-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="f44e2-166">Les jetons garantissent également que les itinéraires restent synchronisés avec les contrôleurs et actions correspondants lorsque les refactorisations de renommage automatique sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="f44e2-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="f44e2-167">Définissez le mode de compatibilité du projet sur ASP.NET Core 2,2 :</span><span class="sxs-lookup"><span data-stu-id="f44e2-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="f44e2-168">Modification précédente :</span><span class="sxs-lookup"><span data-stu-id="f44e2-168">The preceding change:</span></span>

    * <span data-ttu-id="f44e2-169">Est requis pour utiliser l’attribut `[ApiController]` au niveau du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f44e2-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="f44e2-170">Opte pour l’interruption potentielle des comportements introduits dans ASP.NET Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="f44e2-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="f44e2-171">Activez les demandes HTTP d’extraction aux actions de `ProductController` :</span><span class="sxs-lookup"><span data-stu-id="f44e2-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="f44e2-172">Appliquez l’attribut [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) à l’action `GetAllProducts`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-172">Apply the [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="f44e2-173">Appliquez l’attribut `[HttpGet("{id}")]` à l’action `GetProduct`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="f44e2-174">Après les modifications précédentes et la suppression des instructions `using` inutilisées, le fichier *ProductsController.cs* ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="f44e2-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="f44e2-175">Exécutez le projet migré et accédez à `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="f44e2-176">La liste complète des trois produits s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f44e2-176">A full list of three products appears.</span></span> <span data-ttu-id="f44e2-177">Accédez à `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="f44e2-178">Le premier produit s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f44e2-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="f44e2-179">Shim de compatibilité</span><span class="sxs-lookup"><span data-stu-id="f44e2-179">Compatibility shim</span></span>

<span data-ttu-id="f44e2-180">La bibliothèque [Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) fournit un shim de compatibilité pour déplacer des projets d’API Web ASP.net 4. x vers ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="f44e2-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="f44e2-181">Le shim de compatibilité étend ASP.NET Core pour prendre en charge un certain nombre de conventions de l’API Web 2 de ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="f44e2-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="f44e2-182">L’exemple porté précédemment dans ce document est suffisamment basique pour que le shim de compatibilité soit inutile.</span><span class="sxs-lookup"><span data-stu-id="f44e2-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="f44e2-183">Pour les projets de grande taille, l’utilisation du shim de compatibilité peut être utile pour combler temporairement le fossé entre les API ASP.NET Core et ASP.NET 4. x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="f44e2-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="f44e2-184">Le shim de compatibilité de l’API Web est destiné à être utilisé comme mesure temporaire pour prendre en charge la migration de grands projets d’API Web ASP.NET 4. x vers ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f44e2-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="f44e2-185">Au fil du temps, les projets doivent être mis à jour pour utiliser des modèles de ASP.NET Core au lieu de s’appuyer sur le shim de compatibilité.</span><span class="sxs-lookup"><span data-stu-id="f44e2-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="f44e2-186">Les fonctionnalités de compatibilité incluses dans `Microsoft.AspNetCore.Mvc.WebApiCompatShim` sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="f44e2-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="f44e2-187">Ajoute un type de `ApiController` afin que les types de base des contrôleurs ne doivent pas être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="f44e2-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="f44e2-188">Active la liaison de modèle de style API Web.</span><span class="sxs-lookup"><span data-stu-id="f44e2-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="f44e2-189">ASP.NET Core liaison de modèle MVC fonctionne de façon similaire à celle de ASP.NET 4. x MVC 5, par défaut.</span><span class="sxs-lookup"><span data-stu-id="f44e2-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="f44e2-190">Le shim de compatibilité modifie la liaison de modèle pour qu’elle soit plus semblable aux conventions de liaison de modèle d’API Web 2 ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="f44e2-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="f44e2-191">Par exemple, les types complexes sont automatiquement liés à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="f44e2-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="f44e2-192">Étend la liaison de modèle afin que les actions du contrôleur puissent prendre des paramètres de type `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="f44e2-193">Ajoute des formateurs de message permettant aux actions de retourner des résultats de type `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="f44e2-194">Ajoute des méthodes de réponse supplémentaires que les actions de l’API Web 2 peuvent avoir utilisées pour répondre aux réponses :</span><span class="sxs-lookup"><span data-stu-id="f44e2-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="f44e2-195">générateurs de `HttpResponseMessage` :</span><span class="sxs-lookup"><span data-stu-id="f44e2-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="f44e2-196">Méthodes de résultat d’action :</span><span class="sxs-lookup"><span data-stu-id="f44e2-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="f44e2-197">Ajoute une instance de `IContentNegotiator` au conteneur d’injection de dépendances de l’application et met à disposition les types liés à la négociation de contenu à partir de [Microsoft. Aspnet. WebApi. client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="f44e2-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="f44e2-198">`DefaultContentNegotiator` et `MediaTypeFormatter`sont des exemples de ces types.</span><span class="sxs-lookup"><span data-stu-id="f44e2-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="f44e2-199">Pour utiliser le shim de compatibilité :</span><span class="sxs-lookup"><span data-stu-id="f44e2-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="f44e2-200">Installez le package NuGet [Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) .</span><span class="sxs-lookup"><span data-stu-id="f44e2-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="f44e2-201">Enregistrez les services du shim de compatibilité avec le conteneur DI de l’application en appelant `services.AddMvc().AddWebApiConventions()` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f44e2-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="f44e2-202">Définissez des itinéraires spécifiques à l’API Web à l’aide de `MapWebApiRoute` sur le `IRouteBuilder` dans l’appel de `IApplicationBuilder.UseMvc` de l’application.</span><span class="sxs-lookup"><span data-stu-id="f44e2-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f44e2-203">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f44e2-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
