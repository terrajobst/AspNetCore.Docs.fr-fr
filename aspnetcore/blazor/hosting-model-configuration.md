---
title: Configuration du modèle d’hébergement ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur la configuration du modèle d’hébergement Blazor, notamment sur l’intégration des composants Razor dans les applications Razor Pages et MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 1f71ac63bbe9dc9d56cfca2ded19a5b863be828f
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306434"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="778d6-103">Configuration du modèle d’hébergement ASP.NET Core éblouissant</span><span class="sxs-lookup"><span data-stu-id="778d6-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="778d6-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="778d6-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="778d6-105">Cet article traite de l’hébergement de la configuration du modèle.</span><span class="sxs-lookup"><span data-stu-id="778d6-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="778d6-106">WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="778d6-106">Blazor WebAssembly</span></span>

<span data-ttu-id="778d6-107">À partir de la version ASP.NET Core 3,2 Preview 3, le webassembly éblouissant prend en charge la configuration à partir de :</span><span class="sxs-lookup"><span data-stu-id="778d6-107">As of the ASP.NET Core 3.2 Preview 3 release, Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="778d6-108">*wwwroot/appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="778d6-108">*wwwroot/appsettings.json*</span></span>
* <span data-ttu-id="778d6-109">*wwwroot/appSettings. {ENVIRONMENT}. JSON*</span><span class="sxs-lookup"><span data-stu-id="778d6-109">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>

<span data-ttu-id="778d6-110">Dans une application hébergée sur éblouissant, l' [environnement d’exécution](xref:fundamentals/environments) est identique à la valeur de l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="778d6-110">In a Blazor Hosted app, the [runtime environment](xref:fundamentals/environments) is the same as the server app's value.</span></span>

<span data-ttu-id="778d6-111">Lors de l’exécution locale de l’application, l’environnement est par défaut développement.</span><span class="sxs-lookup"><span data-stu-id="778d6-111">When running the app locally, the environment defaults to Development.</span></span> <span data-ttu-id="778d6-112">Lorsque l’application est publiée, l’environnement prend par défaut la valeur production.</span><span class="sxs-lookup"><span data-stu-id="778d6-112">When the app is published, the environment defaults to Production.</span></span> <span data-ttu-id="778d6-113">Pour plus d’informations, notamment sur la configuration de l’environnement, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="778d6-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

> [!WARNING]
> <span data-ttu-id="778d6-114">La configuration dans une application de webassembly éblouissante est visible pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="778d6-114">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="778d6-115">**Ne stockez pas les secrets ou les informations d’identification de l’application dans la configuration.**</span><span class="sxs-lookup"><span data-stu-id="778d6-115">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="778d6-116">Les fichiers de configuration sont mis en cache pour une utilisation hors connexion.</span><span class="sxs-lookup"><span data-stu-id="778d6-116">Configuration files are cached for offline use.</span></span> <span data-ttu-id="778d6-117">Avec les [applications Web progressives (PWA)](xref:blazor/progressive-web-app), vous pouvez uniquement mettre à jour les fichiers de configuration lors de la création d’un déploiement.</span><span class="sxs-lookup"><span data-stu-id="778d6-117">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="778d6-118">La modification des fichiers de configuration entre les déploiements n’a aucun effet, car :</span><span class="sxs-lookup"><span data-stu-id="778d6-118">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="778d6-119">Les utilisateurs ont des versions mises en cache des fichiers qu’ils continuent d’utiliser.</span><span class="sxs-lookup"><span data-stu-id="778d6-119">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="778d6-120">Les fichiers *Service-Worker. js* et *Service-Worker-Assets. js* de PWA doivent être reconstruits lors de la compilation, qui signalent à l’application à la prochaine visite en ligne de l’utilisateur que l’application a été redéployée.</span><span class="sxs-lookup"><span data-stu-id="778d6-120">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="778d6-121">Pour plus d’informations sur la façon dont les mises à jour en arrière-plan sont gérées par PWA, consultez <xref:blazor/progressive-web-app#background-updates>.</span><span class="sxs-lookup"><span data-stu-id="778d6-121">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="778d6-122">Serveur Blazor</span><span class="sxs-lookup"><span data-stu-id="778d6-122">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="778d6-123">Refléter l’état de la connexion dans l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="778d6-123">Reflect the connection state in the UI</span></span>

<span data-ttu-id="778d6-124">Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="778d6-124">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="778d6-125">En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.</span><span class="sxs-lookup"><span data-stu-id="778d6-125">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="778d6-126">Pour personnaliser l’interface utilisateur, définissez un élément avec un `id` de `components-reconnect-modal` dans la `<body>` de la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="778d6-126">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="778d6-127">Le tableau suivant décrit les classes CSS appliquées à l’élément `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="778d6-127">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="778d6-128">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="778d6-128">CSS class</span></span>                       | <span data-ttu-id="778d6-129">Indique&hellip;</span><span class="sxs-lookup"><span data-stu-id="778d6-129">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="778d6-130">Connexion perdue.</span><span class="sxs-lookup"><span data-stu-id="778d6-130">A lost connection.</span></span> <span data-ttu-id="778d6-131">Le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="778d6-131">The client is attempting to reconnect.</span></span> <span data-ttu-id="778d6-132">Affichez le modal.</span><span class="sxs-lookup"><span data-stu-id="778d6-132">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="778d6-133">Une connexion active est rétablie sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="778d6-133">An active connection is re-established to the server.</span></span> <span data-ttu-id="778d6-134">Masquez le modal.</span><span class="sxs-lookup"><span data-stu-id="778d6-134">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="778d6-135">Échec de la reconnexion, probablement en raison d’une défaillance du réseau.</span><span class="sxs-lookup"><span data-stu-id="778d6-135">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="778d6-136">Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="778d6-136">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="778d6-137">Reconnexion refusée.</span><span class="sxs-lookup"><span data-stu-id="778d6-137">Reconnection rejected.</span></span> <span data-ttu-id="778d6-138">Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur est perdu.</span><span class="sxs-lookup"><span data-stu-id="778d6-138">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="778d6-139">Pour recharger l’application, appelez `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="778d6-139">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="778d6-140">Cet état de connexion peut se produire dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="778d6-140">This connection state may result when:</span></span><ul><li><span data-ttu-id="778d6-141">Un blocage dans le circuit côté serveur se produit.</span><span class="sxs-lookup"><span data-stu-id="778d6-141">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="778d6-142">Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="778d6-142">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="778d6-143">Les instances des composants avec lesquels l’utilisateur interagit sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="778d6-143">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="778d6-144">Le serveur est redémarré ou le processus de travail de l’application est recyclé.</span><span class="sxs-lookup"><span data-stu-id="778d6-144">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="778d6-145">Mode d’affichage</span><span class="sxs-lookup"><span data-stu-id="778d6-145">Render mode</span></span>

<span data-ttu-id="778d6-146">Les applications serveur éblouissantes sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie.</span><span class="sxs-lookup"><span data-stu-id="778d6-146">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="778d6-147">Elle est configurée dans la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="778d6-147">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="778d6-148">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="778d6-148">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="778d6-149">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="778d6-149">Is prerendered into the page.</span></span>
* <span data-ttu-id="778d6-150">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="778d6-150">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="778d6-151">Description</span><span class="sxs-lookup"><span data-stu-id="778d6-151">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="778d6-152">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="778d6-152">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="778d6-153">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="778d6-153">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="778d6-154">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="778d6-154">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="778d6-155">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="778d6-155">Output from the component isn't included.</span></span> <span data-ttu-id="778d6-156">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="778d6-156">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="778d6-157">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="778d6-157">Renders the component into static HTML.</span></span> |

<span data-ttu-id="778d6-158">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="778d6-158">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="778d6-159">Restituer des composants interactifs avec état à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="778d6-159">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="778d6-160">Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="778d6-160">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="778d6-161">Lors du rendu de la page ou de la vue :</span><span class="sxs-lookup"><span data-stu-id="778d6-161">When the page or view renders:</span></span>

* <span data-ttu-id="778d6-162">Le composant est prérendu avec la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="778d6-162">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="778d6-163">L’état initial du composant utilisé pour le prérendu est perdu.</span><span class="sxs-lookup"><span data-stu-id="778d6-163">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="778d6-164">Un nouvel état de composant est créé lorsque la connexion SignalR est établie.</span><span class="sxs-lookup"><span data-stu-id="778d6-164">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="778d6-165">La page Razor suivante affiche un composant `Counter` :</span><span class="sxs-lookup"><span data-stu-id="778d6-165">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="778d6-166">Rendre des composants non interactifs à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="778d6-166">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="778d6-167">Dans la page Razor suivante, le composant `Counter` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :</span><span class="sxs-lookup"><span data-stu-id="778d6-167">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="778d6-168">Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.</span><span class="sxs-lookup"><span data-stu-id="778d6-168">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="778d6-169">Configurer le client SignalR pour les applications Blazor Server</span><span class="sxs-lookup"><span data-stu-id="778d6-169">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="778d6-170">Parfois, vous devez configurer le client SignalR utilisé par les applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="778d6-170">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="778d6-171">Par exemple, vous pouvez configurer la journalisation sur le client SignalR pour diagnostiquer un problème de connexion.</span><span class="sxs-lookup"><span data-stu-id="778d6-171">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="778d6-172">Pour configurer le client SignalR dans le fichier *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="778d6-172">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="778d6-173">Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="778d6-173">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="778d6-174">Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur de SignalR.</span><span class="sxs-lookup"><span data-stu-id="778d6-174">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
