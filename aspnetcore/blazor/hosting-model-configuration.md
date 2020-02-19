---
title: Configuration du modèle d’hébergement ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur la configuration du modèle d’hébergement Blazor, notamment sur l’intégration des composants Razor dans les applications Razor Pages et MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: bd44643877e45c5b48b0972bcc2f637fbc5d98f2
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447033"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="a1163-103">Configuration du modèle d’hébergement ASP.NET Core éblouissant</span><span class="sxs-lookup"><span data-stu-id="a1163-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="a1163-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a1163-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a1163-105">Cet article traite de l’hébergement de la configuration du modèle.</span><span class="sxs-lookup"><span data-stu-id="a1163-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="a1163-106">Serveur Blazor</span><span class="sxs-lookup"><span data-stu-id="a1163-106">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="a1163-107">Refléter l’état de la connexion dans l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="a1163-107">Reflect the connection state in the UI</span></span>

<span data-ttu-id="a1163-108">Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="a1163-108">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="a1163-109">En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.</span><span class="sxs-lookup"><span data-stu-id="a1163-109">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="a1163-110">Pour personnaliser l’interface utilisateur, définissez un élément avec un `id` de `components-reconnect-modal` dans la `<body>` de la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a1163-110">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="a1163-111">Le tableau suivant décrit les classes CSS appliquées à l’élément `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="a1163-111">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="a1163-112">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="a1163-112">CSS class</span></span>                       | <span data-ttu-id="a1163-113">Indique&hellip;</span><span class="sxs-lookup"><span data-stu-id="a1163-113">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="a1163-114">Connexion perdue.</span><span class="sxs-lookup"><span data-stu-id="a1163-114">A lost connection.</span></span> <span data-ttu-id="a1163-115">Le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="a1163-115">The client is attempting to reconnect.</span></span> <span data-ttu-id="a1163-116">Affichez le modal.</span><span class="sxs-lookup"><span data-stu-id="a1163-116">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="a1163-117">Une connexion active est rétablie sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="a1163-117">An active connection is re-established to the server.</span></span> <span data-ttu-id="a1163-118">Masquez le modal.</span><span class="sxs-lookup"><span data-stu-id="a1163-118">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="a1163-119">Échec de la reconnexion, probablement en raison d’une défaillance du réseau.</span><span class="sxs-lookup"><span data-stu-id="a1163-119">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="a1163-120">Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="a1163-120">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="a1163-121">Reconnexion refusée.</span><span class="sxs-lookup"><span data-stu-id="a1163-121">Reconnection rejected.</span></span> <span data-ttu-id="a1163-122">Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur est perdu.</span><span class="sxs-lookup"><span data-stu-id="a1163-122">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="a1163-123">Pour recharger l’application, appelez `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="a1163-123">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="a1163-124">Cet état de connexion peut se produire dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="a1163-124">This connection state may result when:</span></span><ul><li><span data-ttu-id="a1163-125">Un blocage dans le circuit côté serveur se produit.</span><span class="sxs-lookup"><span data-stu-id="a1163-125">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="a1163-126">Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a1163-126">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="a1163-127">Les instances des composants avec lesquels l’utilisateur interagit sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="a1163-127">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="a1163-128">Le serveur est redémarré ou le processus de travail de l’application est recyclé.</span><span class="sxs-lookup"><span data-stu-id="a1163-128">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="a1163-129">Mode d’affichage</span><span class="sxs-lookup"><span data-stu-id="a1163-129">Render mode</span></span>

<span data-ttu-id="a1163-130">Les applications serveur éblouissantes sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie.</span><span class="sxs-lookup"><span data-stu-id="a1163-130">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="a1163-131">Elle est configurée dans la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a1163-131">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="a1163-132">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="a1163-132">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="a1163-133">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="a1163-133">Is prerendered into the page.</span></span>
* <span data-ttu-id="a1163-134">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a1163-134">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="a1163-135">Description</span><span class="sxs-lookup"><span data-stu-id="a1163-135">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="a1163-136">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="a1163-136">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="a1163-137">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="a1163-137">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="a1163-138">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="a1163-138">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="a1163-139">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="a1163-139">Output from the component isn't included.</span></span> <span data-ttu-id="a1163-140">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="a1163-140">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="a1163-141">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="a1163-141">Renders the component into static HTML.</span></span> |

<span data-ttu-id="a1163-142">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="a1163-142">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="a1163-143">Restituer des composants interactifs avec état à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="a1163-143">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="a1163-144">Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="a1163-144">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="a1163-145">Lors du rendu de la page ou de la vue :</span><span class="sxs-lookup"><span data-stu-id="a1163-145">When the page or view renders:</span></span>

* <span data-ttu-id="a1163-146">Le composant est prérendu avec la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="a1163-146">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="a1163-147">L’état initial du composant utilisé pour le prérendu est perdu.</span><span class="sxs-lookup"><span data-stu-id="a1163-147">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="a1163-148">Un nouvel état de composant est créé lorsque la connexion SignalR est établie.</span><span class="sxs-lookup"><span data-stu-id="a1163-148">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="a1163-149">La page Razor suivante affiche un composant `Counter` :</span><span class="sxs-lookup"><span data-stu-id="a1163-149">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="a1163-150">Rendre des composants non interactifs à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="a1163-150">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="a1163-151">Dans la page Razor suivante, le composant `Counter` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :</span><span class="sxs-lookup"><span data-stu-id="a1163-151">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="a1163-152">Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.</span><span class="sxs-lookup"><span data-stu-id="a1163-152">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="a1163-153">Configurer le client SignalR pour les applications Blazor Server</span><span class="sxs-lookup"><span data-stu-id="a1163-153">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="a1163-154">Parfois, vous devez configurer le client SignalR utilisé par les applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="a1163-154">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="a1163-155">Par exemple, vous pouvez configurer la journalisation sur le client SignalR pour diagnostiquer un problème de connexion.</span><span class="sxs-lookup"><span data-stu-id="a1163-155">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="a1163-156">Pour configurer le client SignalR dans le fichier *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a1163-156">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="a1163-157">Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="a1163-157">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="a1163-158">Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a1163-158">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
