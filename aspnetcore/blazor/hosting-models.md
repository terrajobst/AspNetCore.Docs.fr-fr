---
title: ASP.NET Core les modèles d’hébergement éblouissants
author: guardrex
description: Comprendre les modèles d’hébergement côté client et côté serveur.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 64393e826cb17550085f468f5916fca55973908f
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993388"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="254ce-103">ASP.NET Core les modèles d’hébergement éblouissants</span><span class="sxs-lookup"><span data-stu-id="254ce-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="254ce-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="254ce-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="254ce-105">Blazor est un Framework Web conçu pour s’exécuter côté client dans le navigateur sur un Runtime.net basé sur [webassembly](https://webassembly.org/) (*Blazor côté client*) ou côté serveur dans ASP.net Core (*Blazor côté serveur*).</span><span class="sxs-lookup"><span data-stu-id="254ce-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="254ce-106">Quel que soit le modèle d’hébergement, les modèles d’application et de composant *sont les mêmes*.</span><span class="sxs-lookup"><span data-stu-id="254ce-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="254ce-107">Pour créer un projet pour les modèles d’hébergement décrits dans cet article, <xref:blazor/get-started>consultez.</span><span class="sxs-lookup"><span data-stu-id="254ce-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="254ce-108">Côté client</span><span class="sxs-lookup"><span data-stu-id="254ce-108">Client-side</span></span>

<span data-ttu-id="254ce-109">Le modèle d’hébergement principal pour éblouissant s’exécute côté client dans le navigateur sur webassembly.</span><span class="sxs-lookup"><span data-stu-id="254ce-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="254ce-110">L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="254ce-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="254ce-111">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="254ce-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="254ce-112">Les mises à jour de l’interface utilisateur et la gestion des événements se produisent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="254ce-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="254ce-113">Les ressources de l’application sont déployées en tant que fichiers statiques sur un serveur Web ou un service qui prend en charge le contenu statique sur les clients.</span><span class="sxs-lookup"><span data-stu-id="254ce-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Éblouissant côté client: L’application éblouissant s’exécute sur un thread d’interface utilisateur dans le navigateur.](hosting-models/_static/client-side.png)

<span data-ttu-id="254ce-115">Pour créer une application éblouissant à l’aide du modèle d’hébergement côté client, utilisez le modèle d' **application éblouissante** webassembly ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="254ce-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="254ce-116">Après avoir sélectionné le modèle **application éblouissant** webassembly, vous avez la possibilité de configurer l’application pour utiliser un serveur principal ASP.net core en activant la case à cocher **ASP.net Core hébergée** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="254ce-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="254ce-117">L’application ASP.NET Core sert l’application éblouissant aux clients.</span><span class="sxs-lookup"><span data-stu-id="254ce-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="254ce-118">L’application Blazor côté client peut interagir avec le serveur sur le réseau à l’aide d’appels d’API Web ou [Signalr](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="254ce-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="254ce-119">Les modèles incluent le script *éblouissant. webassembly. js* qui gère les éléments suivants:</span><span class="sxs-lookup"><span data-stu-id="254ce-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="254ce-120">Téléchargement du Runtime .NET, de l’application et des dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="254ce-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="254ce-121">Initialisation du runtime pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="254ce-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="254ce-122">Le modèle d’hébergement côté client offre plusieurs avantages:</span><span class="sxs-lookup"><span data-stu-id="254ce-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="254ce-123">Il n’existe aucune dépendance côté serveur .NET.</span><span class="sxs-lookup"><span data-stu-id="254ce-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="254ce-124">L’application fonctionne entièrement après son téléchargement sur le client.</span><span class="sxs-lookup"><span data-stu-id="254ce-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="254ce-125">Les ressources et les fonctionnalités du client sont pleinement exploitées.</span><span class="sxs-lookup"><span data-stu-id="254ce-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="254ce-126">Le travail est déchargé du serveur vers le client.</span><span class="sxs-lookup"><span data-stu-id="254ce-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="254ce-127">Un serveur Web ASP.NET Core n’est pas requis pour héberger l’application.</span><span class="sxs-lookup"><span data-stu-id="254ce-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="254ce-128">Les scénarios de déploiement sans serveur sont possibles (par exemple, pour servir l’application à partir d’un CDN).</span><span class="sxs-lookup"><span data-stu-id="254ce-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="254ce-129">L’hébergement côté client présente des inconvénients:</span><span class="sxs-lookup"><span data-stu-id="254ce-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="254ce-130">L’application est limitée aux fonctionnalités du navigateur.</span><span class="sxs-lookup"><span data-stu-id="254ce-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="254ce-131">Le matériel et le logiciel client compatibles (par exemple, la prise en charge de webassembly) sont requis.</span><span class="sxs-lookup"><span data-stu-id="254ce-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="254ce-132">La taille du téléchargement est supérieure, et les applications prennent plus de temps à se charger.</span><span class="sxs-lookup"><span data-stu-id="254ce-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="254ce-133">Le Runtime .NET et la prise en charge des outils sont moins matures.</span><span class="sxs-lookup"><span data-stu-id="254ce-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="254ce-134">Par exemple, des limitations existent dans la prise en charge de [.NET standard](/dotnet/standard/net-standard) et le débogage.</span><span class="sxs-lookup"><span data-stu-id="254ce-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="254ce-135">Côté serveur</span><span class="sxs-lookup"><span data-stu-id="254ce-135">Server-side</span></span>

<span data-ttu-id="254ce-136">Avec le modèle d’hébergement côté serveur, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="254ce-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="254ce-137">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="254ce-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via une connexion Signalr.](hosting-models/_static/server-side.png)

<span data-ttu-id="254ce-139">Pour créer une application éblouissant à l’aide du modèle d’hébergement côté serveur, utilisez le modèle d' **application ASP.net Core éblouissante Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="254ce-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="254ce-140">L’application ASP.NET Core héberge l’application côté serveur et crée le point de terminaison Signalr où les clients se connectent.</span><span class="sxs-lookup"><span data-stu-id="254ce-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="254ce-141">L’application ASP.net Core fait référence à la `Startup` classe de l’application à ajouter:</span><span class="sxs-lookup"><span data-stu-id="254ce-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="254ce-142">Services côté serveur.</span><span class="sxs-lookup"><span data-stu-id="254ce-142">Server-side services.</span></span>
* <span data-ttu-id="254ce-143">L’application vers le pipeline de traitement des demandes.</span><span class="sxs-lookup"><span data-stu-id="254ce-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="254ce-144">Le script&dagger; *éblouissant. Server. js* établit la connexion client.</span><span class="sxs-lookup"><span data-stu-id="254ce-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="254ce-145">Il est de la responsabilité de l’application de conserver et de restaurer l’état de l’application en fonction des besoins (par exemple, en cas de perte de connexion réseau).</span><span class="sxs-lookup"><span data-stu-id="254ce-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="254ce-146">Le modèle d’hébergement côté serveur offre plusieurs avantages:</span><span class="sxs-lookup"><span data-stu-id="254ce-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="254ce-147">La taille du téléchargement est beaucoup plus petite qu’une application côté client et l’application est chargée beaucoup plus rapidement.</span><span class="sxs-lookup"><span data-stu-id="254ce-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="254ce-148">L’application tire pleinement parti des fonctionnalités du serveur, notamment de l’utilisation de toutes les API compatibles avec .NET Core.</span><span class="sxs-lookup"><span data-stu-id="254ce-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="254ce-149">.NET Core sur le serveur est utilisé pour exécuter l’application, de sorte que les outils .NET existants, tels que le débogage, fonctionnent comme prévu.</span><span class="sxs-lookup"><span data-stu-id="254ce-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="254ce-150">Les clients légers sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="254ce-150">Thin clients are supported.</span></span> <span data-ttu-id="254ce-151">Par exemple, les applications côté serveur fonctionnent avec des navigateurs qui ne prennent pas en charge webassembly et sur des appareils à ressources restreintes.</span><span class="sxs-lookup"><span data-stu-id="254ce-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="254ce-152">Le .NET/C# base de code de l’application, y compris le code du composant de l’application, n’est pas pris en charge par les clients.</span><span class="sxs-lookup"><span data-stu-id="254ce-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="254ce-153">L’hébergement côté serveur présente des inconvénients:</span><span class="sxs-lookup"><span data-stu-id="254ce-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="254ce-154">Une latence plus élevée existe généralement.</span><span class="sxs-lookup"><span data-stu-id="254ce-154">Higher latency usually exists.</span></span> <span data-ttu-id="254ce-155">Chaque interaction utilisateur implique un tronçon réseau.</span><span class="sxs-lookup"><span data-stu-id="254ce-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="254ce-156">Il n’existe aucune prise en charge hors connexion.</span><span class="sxs-lookup"><span data-stu-id="254ce-156">There's no offline support.</span></span> <span data-ttu-id="254ce-157">Si la connexion cliente échoue, l’application cesse de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="254ce-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="254ce-158">L’évolutivité est difficile pour les applications avec de nombreux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="254ce-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="254ce-159">Le serveur doit gérer plusieurs connexions clientes et gérer l’état du client.</span><span class="sxs-lookup"><span data-stu-id="254ce-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="254ce-160">Un serveur de ASP.NET Core est requis pour servir l’application.</span><span class="sxs-lookup"><span data-stu-id="254ce-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="254ce-161">Les scénarios de déploiement sans serveur ne sont pas possibles (par exemple, pour servir l’application à partir d’un CDN).</span><span class="sxs-lookup"><span data-stu-id="254ce-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="254ce-162">&dagger;Le script *éblouissant. Server. js* est pris en charge à partir d’une ressource incorporée dans le ASP.net Core Framework partagé.</span><span class="sxs-lookup"><span data-stu-id="254ce-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="254ce-163">Reconnexion au même serveur</span><span class="sxs-lookup"><span data-stu-id="254ce-163">Reconnection to the same server</span></span>

<span data-ttu-id="254ce-164">Les applications côté serveur éblouissantes nécessitent une connexion active Signalr au serveur.</span><span class="sxs-lookup"><span data-stu-id="254ce-164">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="254ce-165">Si la connexion est perdue, l’application tente de se reconnecter au serveur.</span><span class="sxs-lookup"><span data-stu-id="254ce-165">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="254ce-166">Tant que l’état du client est toujours en mémoire, la session client reprend sans perte d’État.</span><span class="sxs-lookup"><span data-stu-id="254ce-166">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="254ce-167">Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="254ce-167">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="254ce-168">En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.</span><span class="sxs-lookup"><span data-stu-id="254ce-168">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="254ce-169">Pour personnaliser l’interface utilisateur, définissez un élément `components-reconnect-modal` avec `id` comme dans la page Razor *_Host. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="254ce-169">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="254ce-170">Le client met à jour cet élément avec l’une des classes CSS suivantes en fonction de l’état de la connexion:</span><span class="sxs-lookup"><span data-stu-id="254ce-170">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="254ce-171">`components-reconnect-show`&ndash; Affichez l’interface utilisateur pour indiquer que la connexion a été perdue et que le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="254ce-171">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="254ce-172">`components-reconnect-hide`&ndash; Le client dispose d’une connexion active et masque l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="254ce-172">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="254ce-173">`components-reconnect-failed`&ndash; Échec de la reconnexion.</span><span class="sxs-lookup"><span data-stu-id="254ce-173">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="254ce-174">Pour tenter une nouvelle connexion, appelez `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="254ce-174">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="254ce-175">Reconnexion avec état après le prérendu</span><span class="sxs-lookup"><span data-stu-id="254ce-175">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="254ce-176">Les applications côté serveur éblouissantes sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie.</span><span class="sxs-lookup"><span data-stu-id="254ce-176">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="254ce-177">Elle est configurée dans la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="254ce-177">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="254ce-178">Le client se reconnecte au serveur avec le même État que celui utilisé pour prégénérer l’application.</span><span class="sxs-lookup"><span data-stu-id="254ce-178">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="254ce-179">Si l’état de l’application est toujours en mémoire, l’état du composant n’est pas restitué après l’établissement de la connexion Signalr.</span><span class="sxs-lookup"><span data-stu-id="254ce-179">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="254ce-180">Restituer des composants interactifs avec état à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="254ce-180">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="254ce-181">Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="254ce-181">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="254ce-182">Lorsque la page ou la vue est restituée, le composant est prérendu.</span><span class="sxs-lookup"><span data-stu-id="254ce-182">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="254ce-183">L’application se reconnecte ensuite à l’état du composant une fois que la connexion cliente est établie tant que l’État est toujours en mémoire.</span><span class="sxs-lookup"><span data-stu-id="254ce-183">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="254ce-184">Par exemple, la page Razor suivante affiche un `Counter` composant avec un nombre initial spécifié à l’aide d’un formulaire:</span><span class="sxs-lookup"><span data-stu-id="254ce-184">For example, the following Razor page renders a `Counter` component with an initial count that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="254ce-185">Détecter quand l’application est prérendu</span><span class="sxs-lookup"><span data-stu-id="254ce-185">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="254ce-186">Configurer le client Signalr pour les applications côté serveur éblouissantes</span><span class="sxs-lookup"><span data-stu-id="254ce-186">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="254ce-187">Parfois, vous devez configurer le client Signalr utilisé par les applications côté serveur éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="254ce-187">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="254ce-188">Par exemple, vous pouvez configurer la journalisation sur le client Signalr pour diagnostiquer un problème de connexion.</span><span class="sxs-lookup"><span data-stu-id="254ce-188">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="254ce-189">Pour configurer le client Signalr dans le fichier *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="254ce-189">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="254ce-190">Ajoutez un `autostart="false"` attribut à la `<script>` balise du script *éblouissant. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="254ce-190">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="254ce-191">Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur signalr.</span><span class="sxs-lookup"><span data-stu-id="254ce-191">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="254ce-192">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="254ce-192">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
