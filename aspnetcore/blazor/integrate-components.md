---
title: Intégrer des composants ASP.NET Core Razor dans des applications Razor Pages et MVC
author: guardrex
description: En savoir plus sur les scénarios de liaison de données pour les composants et les éléments DOM dans Blazor applications.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453295"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="bcaf9-103">Intégrer des composants ASP.NET Core Razor dans des applications Razor Pages et MVC</span><span class="sxs-lookup"><span data-stu-id="bcaf9-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="bcaf9-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="bcaf9-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="bcaf9-105">Les composants Razor peuvent être intégrés dans des applications Razor Pages et MVC.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="bcaf9-106">Lorsque la page ou la vue est restituée, les composants peuvent être prérendus en même temps.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="bcaf9-107">Préparer l’application pour utiliser des composants dans les pages et les vues</span><span class="sxs-lookup"><span data-stu-id="bcaf9-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="bcaf9-108">Une Razor Pages ou une application MVC existante peut intégrer des composants Razor dans des pages et des vues :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="bcaf9-109">Dans le fichier de disposition de l’application ( *_Layout. cshtml*) :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="bcaf9-110">Ajoutez la balise `<base>` suivante à l’élément `<head>` :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="bcaf9-111">La valeur `href` (le *chemin d’accès de base*de l’application) dans l’exemple précédent suppose que l’application se trouve dans le chemin d’URL racine (`/`).</span><span class="sxs-lookup"><span data-stu-id="bcaf9-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="bcaf9-112">Si l’application est une sous-application, suivez les instructions de la section *chemin d’accès* à la base de l’application de l’article <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="bcaf9-113">Le fichier *_Layout. cshtml* se trouve dans le dossier *pages/Shared* d’une application Razor pages ou d’un dossier *Views/Shared* dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="bcaf9-114">Ajoutez une balise `<script>` pour le script *éblouissant. Server. js* immédiatement avant la balise de fermeture `</body>` :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="bcaf9-115">L’infrastructure ajoute le script *éblouissant. Server. js* à l’application.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="bcaf9-116">Il n’est pas nécessaire d’ajouter manuellement le script à l’application.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="bcaf9-117">Ajoutez un fichier *_Imports. Razor* au dossier racine du projet avec le contenu suivant (modifiez le dernier espace de noms, `MyAppNamespace`, en lui attribuant l’espace de noms de l’application) :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="bcaf9-118">Dans `Startup.ConfigureServices`, inscrivez le service du serveur éblouissant :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="bcaf9-119">Dans `Startup.Configure`, ajoutez le point de terminaison du Hub éblouissant à `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="bcaf9-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="bcaf9-120">Intégrer des composants dans n’importe quelle page ou vue.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-120">Integrate components into any page or view.</span></span> <span data-ttu-id="bcaf9-121">Pour plus d’informations, consultez la section [rendre les composants à partir d’une page ou d’une vue](#render-components-from-a-page-or-view) .</span><span class="sxs-lookup"><span data-stu-id="bcaf9-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="bcaf9-122">Utiliser des composants routables dans une application Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bcaf9-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="bcaf9-123">*Cette section concerne l’ajout de composants qui sont directement routables à partir des demandes des utilisateurs.*</span><span class="sxs-lookup"><span data-stu-id="bcaf9-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="bcaf9-124">Pour prendre en charge les composants Razor routables dans les applications Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="bcaf9-125">Suivez les instructions de la section [préparer l’application à utiliser les composants des pages et des vues](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="bcaf9-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="bcaf9-126">Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-126">Add an *App.razor* file to the project root with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="bcaf9-127">Ajoutez un fichier *_Host. cshtml* au dossier *pages* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="bcaf9-128">Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="bcaf9-129">Ajoutez un itinéraire de priorité basse pour la page *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="bcaf9-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="bcaf9-130">Ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-130">Add routable components to the app.</span></span> <span data-ttu-id="bcaf9-131">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="bcaf9-132">Pour plus d’informations sur les espaces de noms, consultez la section [espaces de noms de composants](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="bcaf9-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="bcaf9-133">Utiliser des composants routables dans une application MVC</span><span class="sxs-lookup"><span data-stu-id="bcaf9-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="bcaf9-134">*Cette section concerne l’ajout de composants qui sont directement routables à partir des demandes des utilisateurs.*</span><span class="sxs-lookup"><span data-stu-id="bcaf9-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="bcaf9-135">Pour prendre en charge les composants Razor routables dans les applications MVC :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="bcaf9-136">Suivez les instructions de la section [préparer l’application à utiliser les composants des pages et des vues](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="bcaf9-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="bcaf9-137">Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="bcaf9-138">Ajoutez un fichier *_Host. cshtml* au dossier *views/de démarrage* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="bcaf9-139">Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="bcaf9-140">Ajoutez une action au contrôleur d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="bcaf9-141">Ajoutez un itinéraire de faible priorité pour l’action de contrôleur qui retourne la vue *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="bcaf9-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="bcaf9-142">Créez un dossier *pages* et ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="bcaf9-143">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="bcaf9-144">Pour plus d’informations sur les espaces de noms, consultez la section [espaces de noms de composants](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="bcaf9-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="bcaf9-145">Espaces de noms de composants</span><span class="sxs-lookup"><span data-stu-id="bcaf9-145">Component namespaces</span></span>

<span data-ttu-id="bcaf9-146">Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms qui représente le dossier à la page/la vue ou au fichier *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="bcaf9-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="bcaf9-147">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-147">In the following example:</span></span>

* <span data-ttu-id="bcaf9-148">Remplacez `MyAppNamespace` par l’espace de noms de l’application.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="bcaf9-149">Si un dossier nommé *Components* n’est pas utilisé pour contenir les composants, remplacez `Components` par le dossier dans lequel se trouvent les composants.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="bcaf9-150">Le fichier *_ViewImports. cshtml* se trouve dans le dossier *pages* d’une application Razor pages ou du dossier *views* d’une application MVC.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="bcaf9-151">Pour plus d’informations, consultez <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="bcaf9-152">Rendre les composants à partir d’une page ou d’une vue</span><span class="sxs-lookup"><span data-stu-id="bcaf9-152">Render components from a page or view</span></span>

<span data-ttu-id="bcaf9-153">*Cette section se rapporte à l’ajout de composants à des pages ou à des vues, où les composants ne sont pas directement routés à partir des demandes de l’utilisateur.*</span><span class="sxs-lookup"><span data-stu-id="bcaf9-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="bcaf9-154">Pour afficher un composant à partir d’une page ou d’une vue, utilisez le tag Helper `Component` :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-154">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="bcaf9-155">Le type de paramètre doit être sérialisable JSON, ce qui signifie généralement que le type doit avoir un constructeur par défaut et des propriétés définissables.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-155">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="bcaf9-156">Par exemple, vous pouvez spécifier une valeur pour `IncrementAmount`, car le type de `IncrementAmount` est un `int`, qui est un type primitif pris en charge par le sérialiseur JSON.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-156">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="bcaf9-157">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-157">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="bcaf9-158">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-158">Is prerendered into the page.</span></span>
* <span data-ttu-id="bcaf9-159">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-159">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="bcaf9-160">Description</span><span class="sxs-lookup"><span data-stu-id="bcaf9-160">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="bcaf9-161">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-161">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="bcaf9-162">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-162">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="bcaf9-163">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-163">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="bcaf9-164">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-164">Output from the component isn't included.</span></span> <span data-ttu-id="bcaf9-165">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="bcaf9-166">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-166">Renders the component into static HTML.</span></span> |

<span data-ttu-id="bcaf9-167">Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-167">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="bcaf9-168">Les composants ne peuvent pas utiliser les scénarios spécifiques aux vues et aux pages, tels que les vues partielles et les sections.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-168">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="bcaf9-169">Pour utiliser la logique de la vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-169">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="bcaf9-170">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="bcaf9-170">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="bcaf9-171">Pour plus d’informations sur la façon dont les composants sont rendus, l’état des composants et le tag Helper `Component`, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="bcaf9-171">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
