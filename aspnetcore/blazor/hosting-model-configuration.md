---
title: Configuration du modèle d’hébergement ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur la configuration du modèle d’hébergement Blazor, notamment sur l’intégration des composants Razor dans les applications Razor Pages et MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215059"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="f1950-103">Configuration du modèle d’hébergement ASP.NET Core éblouissant</span><span class="sxs-lookup"><span data-stu-id="f1950-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="f1950-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f1950-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f1950-105">Cet article traite de l’hébergement de la configuration du modèle.</span><span class="sxs-lookup"><span data-stu-id="f1950-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="f1950-106">Serveur Blazor</span><span class="sxs-lookup"><span data-stu-id="f1950-106">Blazor Server</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="f1950-107">Intégrer des composants Razor dans des applications Razor Pages et MVC</span><span class="sxs-lookup"><span data-stu-id="f1950-107">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="f1950-108">Utiliser des composants dans les pages et les vues</span><span class="sxs-lookup"><span data-stu-id="f1950-108">Use components in pages and views</span></span>

<span data-ttu-id="f1950-109">Une Razor Pages ou une application MVC existante peut intégrer des composants Razor dans des pages et des vues :</span><span class="sxs-lookup"><span data-stu-id="f1950-109">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="f1950-110">Dans le fichier de disposition de l’application ( *_Layout. cshtml*) :</span><span class="sxs-lookup"><span data-stu-id="f1950-110">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="f1950-111">Ajoutez la balise `<base>` suivante à l’élément `<head>` :</span><span class="sxs-lookup"><span data-stu-id="f1950-111">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="f1950-112">La valeur `href` (le *chemin d’accès de base*de l’application) dans l’exemple précédent suppose que l’application se trouve dans le chemin d’URL racine (`/`).</span><span class="sxs-lookup"><span data-stu-id="f1950-112">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="f1950-113">Si l’application est une sous-application, suivez les instructions de la section *chemin d’accès* à la base de l’application de l’article <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="f1950-113">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="f1950-114">Le fichier *_Layout. cshtml* se trouve dans le dossier *pages/Shared* d’une application Razor pages ou d’un dossier *Views/Shared* dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="f1950-114">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="f1950-115">Ajoutez une balise `<script>` pour le script *éblouissant. Server. js* juste avant la balise de fermeture `</body>` :</span><span class="sxs-lookup"><span data-stu-id="f1950-115">Add a `<script>` tag for the *blazor.server.js* script right before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="f1950-116">L’infrastructure ajoute le script *éblouissant. Server. js* à l’application.</span><span class="sxs-lookup"><span data-stu-id="f1950-116">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="f1950-117">Il n’est pas nécessaire d’ajouter manuellement le script à l’application.</span><span class="sxs-lookup"><span data-stu-id="f1950-117">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="f1950-118">Ajoutez un fichier *_Imports. Razor* au dossier racine du projet avec le contenu suivant (modifiez le dernier espace de noms, `MyAppNamespace`, en lui attribuant l’espace de noms de l’application) :</span><span class="sxs-lookup"><span data-stu-id="f1950-118">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="f1950-119">Dans `Startup.ConfigureServices`, inscrivez le service du serveur éblouissant :</span><span class="sxs-lookup"><span data-stu-id="f1950-119">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="f1950-120">Dans `Startup.Configure`, ajoutez le point de terminaison du Hub éblouissant à `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="f1950-120">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="f1950-121">Intégrer des composants dans n’importe quelle page ou vue.</span><span class="sxs-lookup"><span data-stu-id="f1950-121">Integrate components into any page or view.</span></span> <span data-ttu-id="f1950-122">Pour plus d’informations, consultez la section *intégrer des composants dans Razor pages et applications MVC* de l’article <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="f1950-122">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="f1950-123">Utiliser des composants routables dans une application Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f1950-123">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="f1950-124">Pour prendre en charge les composants Razor routables dans les applications Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="f1950-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="f1950-125">Suivez les instructions de la section [utiliser des composants dans les pages et les vues](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="f1950-125">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="f1950-126">Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="f1950-126">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="f1950-127">Ajoutez un fichier *_Host. cshtml* au dossier *pages* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="f1950-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="f1950-128">Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="f1950-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="f1950-129">Ajoutez un itinéraire de priorité basse pour la page *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f1950-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="f1950-130">Ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="f1950-130">Add routable components to the app.</span></span> <span data-ttu-id="f1950-131">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f1950-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="f1950-132">Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms représentant le dossier au fichier *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="f1950-132">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f1950-133">Pour plus d’informations, consultez <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="f1950-133">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="f1950-134">Utiliser des composants routables dans une application MVC</span><span class="sxs-lookup"><span data-stu-id="f1950-134">Use routable components in an MVC app</span></span>

<span data-ttu-id="f1950-135">Pour prendre en charge les composants Razor routables dans les applications MVC :</span><span class="sxs-lookup"><span data-stu-id="f1950-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="f1950-136">Suivez les instructions de la section [utiliser des composants dans les pages et les vues](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="f1950-136">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="f1950-137">Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="f1950-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="f1950-138">Ajoutez un fichier *_Host. cshtml* au dossier *views/de démarrage* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="f1950-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="f1950-139">Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="f1950-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="f1950-140">Ajoutez une action au contrôleur d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="f1950-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="f1950-141">Ajoutez un itinéraire de faible priorité pour l’action de contrôleur qui retourne la vue *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f1950-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="f1950-142">Créez un dossier *pages* et ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="f1950-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="f1950-143">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f1950-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="f1950-144">Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms représentant le dossier au fichier *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="f1950-144">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f1950-145">Pour plus d’informations, consultez <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="f1950-145">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="f1950-146">Refléter l’état de la connexion dans l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="f1950-146">Reflect the connection state in the UI</span></span>

<span data-ttu-id="f1950-147">Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="f1950-147">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="f1950-148">En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.</span><span class="sxs-lookup"><span data-stu-id="f1950-148">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="f1950-149">Pour personnaliser l’interface utilisateur, définissez un élément avec un `id` de `components-reconnect-modal` dans la `<body>` de la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="f1950-149">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="f1950-150">Le tableau suivant décrit les classes CSS appliquées à l’élément `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="f1950-150">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="f1950-151">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="f1950-151">CSS class</span></span>                       | <span data-ttu-id="f1950-152">Indique&hellip;</span><span class="sxs-lookup"><span data-stu-id="f1950-152">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="f1950-153">Connexion perdue.</span><span class="sxs-lookup"><span data-stu-id="f1950-153">A lost connection.</span></span> <span data-ttu-id="f1950-154">Le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="f1950-154">The client is attempting to reconnect.</span></span> <span data-ttu-id="f1950-155">Affichez le modal.</span><span class="sxs-lookup"><span data-stu-id="f1950-155">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="f1950-156">Une connexion active est rétablie sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f1950-156">An active connection is re-established to the server.</span></span> <span data-ttu-id="f1950-157">Masquez le modal.</span><span class="sxs-lookup"><span data-stu-id="f1950-157">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="f1950-158">Échec de la reconnexion, probablement en raison d’une défaillance du réseau.</span><span class="sxs-lookup"><span data-stu-id="f1950-158">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="f1950-159">Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="f1950-159">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="f1950-160">Reconnexion refusée.</span><span class="sxs-lookup"><span data-stu-id="f1950-160">Reconnection rejected.</span></span> <span data-ttu-id="f1950-161">Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur est perdu.</span><span class="sxs-lookup"><span data-stu-id="f1950-161">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="f1950-162">Pour recharger l’application, appelez `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="f1950-162">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="f1950-163">Cet état de connexion peut se produire dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="f1950-163">This connection state may result when:</span></span><ul><li><span data-ttu-id="f1950-164">Un blocage dans le circuit côté serveur se produit.</span><span class="sxs-lookup"><span data-stu-id="f1950-164">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="f1950-165">Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f1950-165">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="f1950-166">Les instances des composants avec lesquels l’utilisateur interagit sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="f1950-166">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="f1950-167">Le serveur est redémarré ou le processus de travail de l’application est recyclé.</span><span class="sxs-lookup"><span data-stu-id="f1950-167">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="f1950-168">Mode d’affichage</span><span class="sxs-lookup"><span data-stu-id="f1950-168">Render mode</span></span>

<span data-ttu-id="f1950-169">Les applications serveur éblouissantes sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie.</span><span class="sxs-lookup"><span data-stu-id="f1950-169">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="f1950-170">Elle est configurée dans la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="f1950-170">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="f1950-171">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="f1950-171">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="f1950-172">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="f1950-172">Is prerendered into the page.</span></span>
* <span data-ttu-id="f1950-173">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f1950-173">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="f1950-174">Description</span><span class="sxs-lookup"><span data-stu-id="f1950-174">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="f1950-175">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="f1950-175">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="f1950-176">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="f1950-176">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="f1950-177">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="f1950-177">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="f1950-178">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="f1950-178">Output from the component isn't included.</span></span> <span data-ttu-id="f1950-179">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="f1950-179">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="f1950-180">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="f1950-180">Renders the component into static HTML.</span></span> |

<span data-ttu-id="f1950-181">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="f1950-181">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="f1950-182">Restituer des composants interactifs avec état à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="f1950-182">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="f1950-183">Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="f1950-183">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="f1950-184">Lors du rendu de la page ou de la vue :</span><span class="sxs-lookup"><span data-stu-id="f1950-184">When the page or view renders:</span></span>

* <span data-ttu-id="f1950-185">Le composant est prérendu avec la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="f1950-185">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="f1950-186">L’état initial du composant utilisé pour le prérendu est perdu.</span><span class="sxs-lookup"><span data-stu-id="f1950-186">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="f1950-187">Un nouvel état de composant est créé lorsque la connexion SignalR est établie.</span><span class="sxs-lookup"><span data-stu-id="f1950-187">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="f1950-188">La page Razor suivante affiche un composant `Counter` :</span><span class="sxs-lookup"><span data-stu-id="f1950-188">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="f1950-189">Rendre des composants non interactifs à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="f1950-189">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="f1950-190">Dans la page Razor suivante, le composant `Counter` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :</span><span class="sxs-lookup"><span data-stu-id="f1950-190">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="f1950-191">Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.</span><span class="sxs-lookup"><span data-stu-id="f1950-191">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="f1950-192">Configurer le client SignalR pour les applications Blazor Server</span><span class="sxs-lookup"><span data-stu-id="f1950-192">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="f1950-193">Parfois, vous devez configurer le client SignalR utilisé par les applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="f1950-193">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="f1950-194">Par exemple, vous pouvez configurer la journalisation sur le client SignalR pour diagnostiquer un problème de connexion.</span><span class="sxs-lookup"><span data-stu-id="f1950-194">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="f1950-195">Pour configurer le client SignalR dans le fichier *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="f1950-195">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="f1950-196">Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="f1950-196">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="f1950-197">Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur de SignalR.</span><span class="sxs-lookup"><span data-stu-id="f1950-197">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
