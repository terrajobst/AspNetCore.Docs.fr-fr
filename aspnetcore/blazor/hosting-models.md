---
title: ASP.NET Core Blazor des modèles d’hébergement
author: guardrex
description: Découvrez les modèles d’hébergement Blazor webassembly et Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
- blazor.webassembly.js
uid: blazor/hosting-models
ms.openlocfilehash: 2c66bede9c1e31b22fd1612fead556176d6f192b
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726865"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="7ef61-103">ASP.NET Core Blazor des modèles d’hébergement</span><span class="sxs-lookup"><span data-stu-id="7ef61-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="7ef61-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7ef61-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="7ef61-105"> est un Framework Web conçu pour s’exécuter côté client dans le navigateur sur un Runtime .NET basé sur [Webassembly](https://webassembly.org/)( *Blazor webassembly*) ou côté serveur dans ASP.net Core ( *serveurBlazor* ).</span><span class="sxs-lookup"><span data-stu-id="7ef61-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="7ef61-106">Quel que soit le modèle d’hébergement, les modèles d’application et de composant *sont les mêmes*.</span><span class="sxs-lookup"><span data-stu-id="7ef61-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="7ef61-107">Pour créer un projet pour les modèles d’hébergement décrits dans cet article, consultez <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="7ef61-108"> webassembly</span><span class="sxs-lookup"><span data-stu-id="7ef61-108"> WebAssembly</span></span>

<span data-ttu-id="7ef61-109">Le modèle d’hébergement principal pour Blazor s’exécute côté client dans le navigateur sur webassembly.</span><span class="sxs-lookup"><span data-stu-id="7ef61-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="7ef61-110">L’application Blazor, ses dépendances et le Runtime .NET sont téléchargés dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="7ef61-111">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="7ef61-112">Les mises à jour de l’interface utilisateur et la gestion des événements se produisent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="7ef61-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="7ef61-113">Les ressources de l’application sont déployées en tant que fichiers statiques sur un serveur Web ou un service qui prend en charge le contenu statique sur les clients.</span><span class="sxs-lookup"><span data-stu-id="7ef61-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! Opérationnel. NO-LOC (éblouissant)]<span data-ttu-id="7ef61-114"> webassembly : [ ! Opérationnel. NO-LOC (éblouissant)] l’application s’exécute sur un thread d’interface utilisateur dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="7ef61-115">Pour créer une application Blazor à l’aide du modèle d’hébergement côté client, utilisez le modèle d' **applicationBlazor Webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="7ef61-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="7ef61-116">Après avoir sélectionné le modèle d' **applicationBlazor Webassembly** , vous avez la possibilité de configurer l’application pour utiliser un serveur principal ASP.net core en activant la case à cocher **ASP.net Core hébergé** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="7ef61-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="7ef61-117">L’application ASP.NET Core sert l’application Blazor aux clients.</span><span class="sxs-lookup"><span data-stu-id="7ef61-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="7ef61-118">L’application Blazor webassembly peut interagir avec le serveur sur le réseau à l’aide d’appels d’API Web ou de [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="7ef61-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="7ef61-119">Les modèles incluent le `blazor.webassembly.js` script qui gère :</span><span class="sxs-lookup"><span data-stu-id="7ef61-119">The templates include the `blazor.webassembly.js` script that handles:</span></span>

* <span data-ttu-id="7ef61-120">Téléchargement du Runtime .NET, de l’application et des dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="7ef61-121">Initialisation du runtime pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="7ef61-122">Le modèle d’hébergement webassembly Blazor offre plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="7ef61-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="7ef61-123">Il n’existe aucune dépendance côté serveur .NET.</span><span class="sxs-lookup"><span data-stu-id="7ef61-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="7ef61-124">L’application fonctionne entièrement après son téléchargement sur le client.</span><span class="sxs-lookup"><span data-stu-id="7ef61-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="7ef61-125">Les ressources et les fonctionnalités du client sont pleinement exploitées.</span><span class="sxs-lookup"><span data-stu-id="7ef61-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="7ef61-126">Le travail est déchargé du serveur vers le client.</span><span class="sxs-lookup"><span data-stu-id="7ef61-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="7ef61-127">Un serveur Web ASP.NET Core n’est pas requis pour héberger l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="7ef61-128">Les scénarios de déploiement sans serveur sont possibles (par exemple, pour servir l’application à partir d’un CDN).</span><span class="sxs-lookup"><span data-stu-id="7ef61-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="7ef61-129">Il existe des inconvénients à Blazor l’hébergement webassembly :</span><span class="sxs-lookup"><span data-stu-id="7ef61-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="7ef61-130">L’application est limitée aux fonctionnalités du navigateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="7ef61-131">Le matériel et le logiciel client compatibles (par exemple, la prise en charge de webassembly) sont requis.</span><span class="sxs-lookup"><span data-stu-id="7ef61-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="7ef61-132">La taille du téléchargement est supérieure, et les applications prennent plus de temps à se charger.</span><span class="sxs-lookup"><span data-stu-id="7ef61-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="7ef61-133">Le Runtime .NET et la prise en charge des outils sont moins matures.</span><span class="sxs-lookup"><span data-stu-id="7ef61-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="7ef61-134">Par exemple, des limitations existent dans la prise en charge de [.NET standard](/dotnet/standard/net-standard) et le débogage.</span><span class="sxs-lookup"><span data-stu-id="7ef61-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="7ef61-135">Serveur de Blazor</span><span class="sxs-lookup"><span data-stu-id="7ef61-135">Blazor Server</span></span>

<span data-ttu-id="7ef61-136">Avec le modèle d’hébergement Blazor Server, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ef61-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="7ef61-137">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés via une connexion [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="7ef61-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via un [ ! Opérationnel. Connexion NO-LOC (Signalr)].](hosting-models/_static/blazor-server.png)

<span data-ttu-id="7ef61-139">Pour créer une application Blazor à l’aide du modèle d’hébergement Blazor Server, utilisez le modèle d' **application ASP.NET CoreBlazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="7ef61-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="7ef61-140">L’application ASP.NET Core héberge l’application Blazor Server et crée le point de terminaison SignalR où les clients se connectent.</span><span class="sxs-lookup"><span data-stu-id="7ef61-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="7ef61-141">L’application ASP.NET Core fait référence à la classe de `Startup` de l’application à ajouter :</span><span class="sxs-lookup"><span data-stu-id="7ef61-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="7ef61-142">Services côté serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-142">Server-side services.</span></span>
* <span data-ttu-id="7ef61-143">L’application vers le pipeline de traitement des demandes.</span><span class="sxs-lookup"><span data-stu-id="7ef61-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="7ef61-144">Le script de `blazor.server.js`&dagger; établit la connexion client.</span><span class="sxs-lookup"><span data-stu-id="7ef61-144">The `blazor.server.js` script&dagger; establishes the client connection.</span></span> <span data-ttu-id="7ef61-145">Il est de la responsabilité de l’application de conserver et de restaurer l’état de l’application en fonction des besoins (par exemple, en cas de perte de connexion réseau).</span><span class="sxs-lookup"><span data-stu-id="7ef61-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="7ef61-146">Le modèle d’hébergement Blazor Server offre plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="7ef61-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="7ef61-147">La taille du téléchargement est beaucoup plus petite qu’une application Blazor webassembly, et l’application est chargée beaucoup plus rapidement.</span><span class="sxs-lookup"><span data-stu-id="7ef61-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="7ef61-148">L’application tire pleinement parti des fonctionnalités du serveur, notamment de l’utilisation de toutes les API compatibles avec .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ef61-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="7ef61-149">.NET Core sur le serveur est utilisé pour exécuter l’application, de sorte que les outils .NET existants, tels que le débogage, fonctionnent comme prévu.</span><span class="sxs-lookup"><span data-stu-id="7ef61-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="7ef61-150">Les clients légers sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="7ef61-150">Thin clients are supported.</span></span> <span data-ttu-id="7ef61-151">Par exemple, les applications Blazor Server fonctionnent avec des navigateurs qui ne prennent pas en charge webassembly et sur des appareils à ressources restreintes.</span><span class="sxs-lookup"><span data-stu-id="7ef61-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="7ef61-152">Le .NET/C# base de code de l’application, y compris le code du composant de l’application, n’est pas pris en charge par les clients.</span><span class="sxs-lookup"><span data-stu-id="7ef61-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="7ef61-153">Blazor Hébergement de serveur présente des inconvénients :</span><span class="sxs-lookup"><span data-stu-id="7ef61-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="7ef61-154">Une latence plus élevée existe généralement.</span><span class="sxs-lookup"><span data-stu-id="7ef61-154">Higher latency usually exists.</span></span> <span data-ttu-id="7ef61-155">Chaque interaction utilisateur implique un tronçon réseau.</span><span class="sxs-lookup"><span data-stu-id="7ef61-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="7ef61-156">Il n’existe aucune prise en charge hors connexion.</span><span class="sxs-lookup"><span data-stu-id="7ef61-156">There's no offline support.</span></span> <span data-ttu-id="7ef61-157">Si la connexion cliente échoue, l’application cesse de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="7ef61-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="7ef61-158">L’évolutivité est difficile pour les applications avec de nombreux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="7ef61-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="7ef61-159">Le serveur doit gérer plusieurs connexions clientes et gérer l’état du client.</span><span class="sxs-lookup"><span data-stu-id="7ef61-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="7ef61-160">Un serveur de ASP.NET Core est requis pour servir l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="7ef61-161">Les scénarios de déploiement sans serveur ne sont pas possibles (par exemple, pour servir l’application à partir d’un CDN).</span><span class="sxs-lookup"><span data-stu-id="7ef61-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="7ef61-162">&dagger;le script `blazor.server.js` est pris en charge à partir d’une ressource incorporée dans le ASP.NET Core Framework partagé.</span><span class="sxs-lookup"><span data-stu-id="7ef61-162">&dagger;The `blazor.server.js` script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="7ef61-163">Comparaison avec l’interface utilisateur du rendu serveur</span><span class="sxs-lookup"><span data-stu-id="7ef61-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="7ef61-164">L’une des façons de comprendre les applications Blazor Server consiste à comprendre la différence entre les modèles traditionnels pour le rendu de l’interface utilisateur dans les applications de ASP.NET Core à l’aide de vues Razor ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7ef61-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="7ef61-165">Les deux modèles utilisent le langage Razor pour décrire le contenu HTML, mais ils diffèrent considérablement dans le mode de rendu du balisage.</span><span class="sxs-lookup"><span data-stu-id="7ef61-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="7ef61-166">Quand une page Razor ou une vue est restituée, chaque ligne de code Razor émet du code HTML sous forme de texte.</span><span class="sxs-lookup"><span data-stu-id="7ef61-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="7ef61-167">Après le rendu, le serveur supprime l’instance de la page ou de la vue, y compris tout état produit.</span><span class="sxs-lookup"><span data-stu-id="7ef61-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="7ef61-168">Lorsqu’une autre demande pour la page se produit, par exemple lorsque la validation du serveur échoue et que le résumé des validations s’affiche :</span><span class="sxs-lookup"><span data-stu-id="7ef61-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="7ef61-169">La page entière est de nouveau restituée au texte HTML.</span><span class="sxs-lookup"><span data-stu-id="7ef61-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="7ef61-170">La page est envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="7ef61-170">The page is sent to the client.</span></span>

<span data-ttu-id="7ef61-171">Une application Blazor est composée d’éléments réutilisables de l’interface utilisateur appelée *composants*.</span><span class="sxs-lookup"><span data-stu-id="7ef61-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="7ef61-172">Un composant contient C# du code, un balisage et d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="7ef61-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="7ef61-173">Quand un composant est rendu, Blazor produit un graphique des composants inclus similaires à un Document Object Model XML ou XML (DOM).</span><span class="sxs-lookup"><span data-stu-id="7ef61-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="7ef61-174">Ce graphique comprend l’état des composants contenus dans les propriétés et les champs.</span><span class="sxs-lookup"><span data-stu-id="7ef61-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="7ef61-175"> évalue le graphique du composant pour produire une représentation binaire de la balise.</span><span class="sxs-lookup"><span data-stu-id="7ef61-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="7ef61-176">Le format binaire peut être :</span><span class="sxs-lookup"><span data-stu-id="7ef61-176">The binary format can be:</span></span>

* <span data-ttu-id="7ef61-177">Converti en texte HTML (lors du prérendu).</span><span class="sxs-lookup"><span data-stu-id="7ef61-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="7ef61-178">Utilisé pour mettre à jour efficacement le balisage pendant le rendu normal.</span><span class="sxs-lookup"><span data-stu-id="7ef61-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="7ef61-179">Une mise à jour de l’interface utilisateur dans Blazor est déclenchée par :</span><span class="sxs-lookup"><span data-stu-id="7ef61-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="7ef61-180">Intervention de l’utilisateur, telle que la sélection d’un bouton.</span><span class="sxs-lookup"><span data-stu-id="7ef61-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="7ef61-181">Déclencheurs d’application, tels qu’un minuteur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="7ef61-182">Le graphique est rerendu et *une différence d’interface utilisateur* (différence) est calculée.</span><span class="sxs-lookup"><span data-stu-id="7ef61-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="7ef61-183">Cette différence est le plus petit ensemble de modifications DOM requis pour mettre à jour l’interface utilisateur sur le client.</span><span class="sxs-lookup"><span data-stu-id="7ef61-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="7ef61-184">Le diff est envoyé au client dans un format binaire et appliqué par le navigateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="7ef61-185">Un composant est supprimé une fois que l’utilisateur l’a quittée sur le client.</span><span class="sxs-lookup"><span data-stu-id="7ef61-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="7ef61-186">Lorsqu’un utilisateur interagit avec un composant, l’état du composant (services, ressources) doit être conservé dans la mémoire du serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="7ef61-187">Étant donné que l’état de nombreux composants peut être géré simultanément par le serveur, l’épuisement de la mémoire est un problème qui doit être résolu.</span><span class="sxs-lookup"><span data-stu-id="7ef61-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="7ef61-188">Pour obtenir des conseils sur la création d’une application Blazor Server afin de garantir la meilleure utilisation de la mémoire du serveur, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="7ef61-189">Intégrer des composants Razor dans des applications Razor Pages et MVC</span><span class="sxs-lookup"><span data-stu-id="7ef61-189">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="7ef61-190">Utiliser des composants dans les pages et les vues</span><span class="sxs-lookup"><span data-stu-id="7ef61-190">Use components in pages and views</span></span>

<span data-ttu-id="7ef61-191">Une Razor Pages ou une application MVC existante peut intégrer des composants Razor dans des pages et des vues :</span><span class="sxs-lookup"><span data-stu-id="7ef61-191">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="7ef61-192">Dans le fichier de disposition de l’application ( *_Layout. cshtml*) :</span><span class="sxs-lookup"><span data-stu-id="7ef61-192">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="7ef61-193">Ajoutez la balise `<base>` suivante à l’élément `<head>` :</span><span class="sxs-lookup"><span data-stu-id="7ef61-193">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="7ef61-194">La valeur `href` (le *chemin d’accès de base*de l’application) dans l’exemple précédent suppose que l’application se trouve dans le chemin d’URL racine (`/`).</span><span class="sxs-lookup"><span data-stu-id="7ef61-194">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="7ef61-195">Si l’application est une sous-application, suivez les instructions de la section *chemin d’accès* à la base de l’application de l’article <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-195">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="7ef61-196">Le fichier *_Layout. cshtml* se trouve dans le dossier *pages/Shared* d’une application Razor pages ou d’un dossier *Views/Shared* dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="7ef61-196">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="7ef61-197">Ajoutez une balise `<script>` pour le script *éblouissant. Server. js* à l’intérieur de la balise `</body>` de fermeture :</span><span class="sxs-lookup"><span data-stu-id="7ef61-197">Add a `<script>` tag for the *blazor.server.js* script inside of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="7ef61-198">L’infrastructure ajoute le script *éblouissant. Server. js* à l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-198">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="7ef61-199">Il n’est pas nécessaire d’ajouter manuellement le script à l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-199">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="7ef61-200">Ajoutez un fichier *_Imports. Razor* au dossier racine du projet avec le contenu suivant (modifiez le dernier espace de noms, `MyAppNamespace`, en lui attribuant l’espace de noms de l’application) :</span><span class="sxs-lookup"><span data-stu-id="7ef61-200">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="7ef61-201">Dans `Startup.ConfigureServices`, ajoutez le service serveur Blazor :</span><span class="sxs-lookup"><span data-stu-id="7ef61-201">In `Startup.ConfigureServices`, add the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="7ef61-202">Dans `Startup.Configure`, ajoutez le point de terminaison Blazor Hub à `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="7ef61-202">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="7ef61-203">Intégrer des composants dans n’importe quelle page ou vue.</span><span class="sxs-lookup"><span data-stu-id="7ef61-203">Integrate components into any page or view.</span></span> <span data-ttu-id="7ef61-204">Pour plus d’informations, consultez la section *intégrer des composants dans Razor pages et applications MVC* de l’article <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-204">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="7ef61-205">Utiliser des composants routables dans une application Razor Pages</span><span class="sxs-lookup"><span data-stu-id="7ef61-205">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="7ef61-206">Pour prendre en charge les composants Razor routables dans les applications Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="7ef61-206">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="7ef61-207">Suivez les instructions de la section [utiliser des composants dans les pages et les vues](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="7ef61-207">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="7ef61-208">Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="7ef61-208">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="7ef61-209">Ajoutez un fichier *_Host. cshtml* au dossier *pages* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="7ef61-209">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="7ef61-210">Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="7ef61-210">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="7ef61-211">Ajoutez un itinéraire de priorité basse pour la page *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="7ef61-211">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="7ef61-212">Ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-212">Add routable components to the app.</span></span> <span data-ttu-id="7ef61-213">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="7ef61-213">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="7ef61-214">Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms représentant le dossier au fichier *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7ef61-214">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="7ef61-215">Pour plus d'informations, consultez <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-215">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="7ef61-216">Utiliser des composants routables dans une application MVC</span><span class="sxs-lookup"><span data-stu-id="7ef61-216">Use routable components in an MVC app</span></span>

<span data-ttu-id="7ef61-217">Pour prendre en charge les composants Razor routables dans les applications MVC :</span><span class="sxs-lookup"><span data-stu-id="7ef61-217">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="7ef61-218">Suivez les instructions de la section [utiliser des composants dans les pages et les vues](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="7ef61-218">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="7ef61-219">Ajoutez un fichier *app. Razor* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="7ef61-219">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="7ef61-220">Ajoutez un fichier *_Host. cshtml* au dossier *views/de démarrage* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="7ef61-220">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="7ef61-221">Les composants utilisent le fichier *_Layout. cshtml* partagé pour leur disposition.</span><span class="sxs-lookup"><span data-stu-id="7ef61-221">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="7ef61-222">Ajoutez une action au contrôleur d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="7ef61-222">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="7ef61-223">Ajoutez un itinéraire de faible priorité pour l’action de contrôleur qui retourne la vue *_Host. cshtml* à la configuration du point de terminaison dans `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="7ef61-223">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="7ef61-224">Créez un dossier *pages* et ajoutez des composants routables à l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-224">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="7ef61-225">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="7ef61-225">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="7ef61-226">Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms représentant le dossier au fichier *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7ef61-226">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="7ef61-227">Pour plus d'informations, consultez <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-227">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="circuits"></a><span data-ttu-id="7ef61-228">Électriques</span><span class="sxs-lookup"><span data-stu-id="7ef61-228">Circuits</span></span>

<span data-ttu-id="7ef61-229">Une application Blazor Server s’appuie sur [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="7ef61-229">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="7ef61-230">Chaque client communique avec le serveur sur une ou plusieurs connexions SignalR appelées *circuit*.</span><span class="sxs-lookup"><span data-stu-id="7ef61-230">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="7ef61-231">Un circuit est l’abstraction de Blazorsur SignalR connexions qui peuvent tolérer des interruptions réseau temporaires.</span><span class="sxs-lookup"><span data-stu-id="7ef61-231">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="7ef61-232">Lorsqu’un client Blazor constate que la connexion SignalR est déconnectée, il tente de se reconnecter au serveur à l’aide d’une nouvelle connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ef61-232">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="7ef61-233">Chaque écran de navigateur (onglet ou IFRAME) qui est connecté à une application Blazor Server utilise une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ef61-233">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="7ef61-234">Il s’agit encore d’une autre distinction importante par rapport aux applications classiques affichées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-234">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="7ef61-235">Dans une application affichée sur un serveur, l’ouverture de la même application dans plusieurs écrans de navigateur n’est généralement pas convertie en demandes de ressources supplémentaires sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-235">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="7ef61-236">Dans une application Blazor Server, chaque écran du navigateur requiert un circuit distinct et des instances distinctes de l’état du composant à gérer par le serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-236">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="7ef61-237"> envisage de fermer un onglet de navigateur ou de naviguer vers une URL externe un arrêt *normal* .</span><span class="sxs-lookup"><span data-stu-id="7ef61-237"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="7ef61-238">En cas de résiliation appropriée, le circuit et les ressources associées sont immédiatement libérés.</span><span class="sxs-lookup"><span data-stu-id="7ef61-238">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="7ef61-239">Un client peut également se déconnecter de manière non appropriée, par exemple en raison d’une interruption du réseau.</span><span class="sxs-lookup"><span data-stu-id="7ef61-239">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="7ef61-240"> Server stocke les circuits déconnectés pour un intervalle configurable afin de permettre au client de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="7ef61-240"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="7ef61-241">Pour plus d’informations, consultez la section [reconnexion au même serveur](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="7ef61-241">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="7ef61-242">Latence de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="7ef61-242">UI Latency</span></span>

<span data-ttu-id="7ef61-243">La latence de l’interface utilisateur est le temps qu’il faut après une action initiée jusqu’au moment où l’interface utilisateur est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="7ef61-243">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="7ef61-244">Des valeurs plus petites pour la latence de l’interface utilisateur sont impératives pour qu’une application soit réactive à un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-244">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="7ef61-245">Dans une application Blazor Server, chaque action est envoyée au serveur, traitée et une comparaison d’interface utilisateur est renvoyée.</span><span class="sxs-lookup"><span data-stu-id="7ef61-245">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="7ef61-246">Par conséquent, la latence de l’interface utilisateur est la somme de la latence du réseau et de la latence du serveur lors du traitement de l’action.</span><span class="sxs-lookup"><span data-stu-id="7ef61-246">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="7ef61-247">Pour une application métier limitée à un réseau d’entreprise privé, l’effet sur les perceptions des utilisateurs de la latence en raison de la latence du réseau est généralement inperceptible.</span><span class="sxs-lookup"><span data-stu-id="7ef61-247">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="7ef61-248">Pour une application déployée sur Internet, la latence peut devenir perceptible pour les utilisateurs, en particulier si les utilisateurs sont largement répartis géographiquement.</span><span class="sxs-lookup"><span data-stu-id="7ef61-248">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="7ef61-249">L’utilisation de la mémoire peut également contribuer à la latence de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-249">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="7ef61-250">L’augmentation de l’utilisation de la mémoire permet de garbage collection fréquentes ou la pagination de la mémoire sur le disque, qui dégradent les performances des applications et augmentent la latence de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-250">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="7ef61-251">Pour plus d'informations, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-251">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="7ef61-252"> applications serveur doivent être optimisées pour réduire la latence de l’interface utilisateur en réduisant la latence du réseau et l’utilisation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="7ef61-252"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="7ef61-253">Pour une approche de la mesure de la latence du réseau, consultez <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-253">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="7ef61-254">Pour plus d’informations sur les SignalR et les Blazor, consultez :</span><span class="sxs-lookup"><span data-stu-id="7ef61-254">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="7ef61-255">Connexion au serveur</span><span class="sxs-lookup"><span data-stu-id="7ef61-255">Connection to the server</span></span>

Blazor<span data-ttu-id="7ef61-256"> applications serveur requièrent une connexion SignalR active au serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-256"> Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="7ef61-257">Si la connexion est perdue, l’application tente de se reconnecter au serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-257">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="7ef61-258">Tant que l’état du client est toujours en mémoire, la session client reprend sans perte d’État.</span><span class="sxs-lookup"><span data-stu-id="7ef61-258">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="7ef61-259">Reconnexion au même serveur</span><span class="sxs-lookup"><span data-stu-id="7ef61-259">Reconnection to the same server</span></span>

<span data-ttu-id="7ef61-260">Une application Blazor Server effectue un prérendu en réponse à la première demande du client, qui configure l’état de l’interface utilisateur sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-260">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="7ef61-261">Lorsque le client tente de créer une connexion SignalR, le client doit se reconnecter au même serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-261">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> Blazor<span data-ttu-id="7ef61-262"> applications serveur qui utilisent plusieurs serveurs principaux doivent implémenter des *sessions rémanentes* pour les connexions SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ef61-262"> Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="7ef61-263">Nous vous recommandons d’utiliser le [service Azure SignalR](/azure/azure-signalr) pour les applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="7ef61-263">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="7ef61-264">Le service permet la mise à l’échelle d’une application Blazor Server vers un grand nombre de connexions SignalR simultanées.</span><span class="sxs-lookup"><span data-stu-id="7ef61-264">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="7ef61-265">Les sessions rémanentes sont activées pour le service Azure SignalR en définissant l’option de `ServerStickyMode` ou la valeur de configuration du service sur `Required`.</span><span class="sxs-lookup"><span data-stu-id="7ef61-265">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="7ef61-266">Pour plus d'informations, consultez <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="7ef61-266">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="7ef61-267">Lorsque vous utilisez IIS, les sessions rémanentes sont activées avec Application Request Routing.</span><span class="sxs-lookup"><span data-stu-id="7ef61-267">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="7ef61-268">Pour plus d’informations, consultez [équilibrage de charge http à l’aide de application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="7ef61-268">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="7ef61-269">Refléter l’état de la connexion dans l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="7ef61-269">Reflect the connection state in the UI</span></span>

<span data-ttu-id="7ef61-270">Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="7ef61-270">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="7ef61-271">En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.</span><span class="sxs-lookup"><span data-stu-id="7ef61-271">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="7ef61-272">Pour personnaliser l’interface utilisateur, définissez un élément avec un `id` de `components-reconnect-modal` dans la `<body>` de la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7ef61-272">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="7ef61-273">Le tableau suivant décrit les classes CSS appliquées à l’élément `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="7ef61-273">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="7ef61-274">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="7ef61-274">CSS class</span></span>                       | <span data-ttu-id="7ef61-275">Indique&hellip;</span><span class="sxs-lookup"><span data-stu-id="7ef61-275">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="7ef61-276">Connexion perdue.</span><span class="sxs-lookup"><span data-stu-id="7ef61-276">A lost connection.</span></span> <span data-ttu-id="7ef61-277">Le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="7ef61-277">The client is attempting to reconnect.</span></span> <span data-ttu-id="7ef61-278">Affichez le modal.</span><span class="sxs-lookup"><span data-stu-id="7ef61-278">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="7ef61-279">Une connexion active est rétablie sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-279">An active connection is re-established to the server.</span></span> <span data-ttu-id="7ef61-280">Masquez le modal.</span><span class="sxs-lookup"><span data-stu-id="7ef61-280">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="7ef61-281">Échec de la reconnexion, probablement en raison d’une défaillance du réseau.</span><span class="sxs-lookup"><span data-stu-id="7ef61-281">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="7ef61-282">Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="7ef61-282">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="7ef61-283">Reconnexion refusée.</span><span class="sxs-lookup"><span data-stu-id="7ef61-283">Reconnection rejected.</span></span> <span data-ttu-id="7ef61-284">Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur est perdu.</span><span class="sxs-lookup"><span data-stu-id="7ef61-284">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="7ef61-285">Pour recharger l’application, appelez `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="7ef61-285">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="7ef61-286">Cet état de connexion peut se produire dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="7ef61-286">This connection state may result when:</span></span><ul><li><span data-ttu-id="7ef61-287">Un blocage dans le circuit côté serveur se produit.</span><span class="sxs-lookup"><span data-stu-id="7ef61-287">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="7ef61-288">Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-288">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="7ef61-289">Les instances des composants avec lesquels l’utilisateur interagit sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="7ef61-289">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="7ef61-290">Le serveur est redémarré ou le processus de travail de l’application est recyclé.</span><span class="sxs-lookup"><span data-stu-id="7ef61-290">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="7ef61-291">Reconnexion avec état après le prérendu</span><span class="sxs-lookup"><span data-stu-id="7ef61-291">Stateful reconnection after prerendering</span></span>

Blazor<span data-ttu-id="7ef61-292"> applications serveur sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie.</span><span class="sxs-lookup"><span data-stu-id="7ef61-292"> Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="7ef61-293">Elle est configurée dans la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7ef61-293">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="7ef61-294">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="7ef61-294">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="7ef61-295">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="7ef61-295">Is prerendered into the page.</span></span>
* <span data-ttu-id="7ef61-296">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer un Blazor application à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-296">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="7ef61-297">Description</span><span class="sxs-lookup"><span data-stu-id="7ef61-297">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="7ef61-298">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="7ef61-298">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="7ef61-299">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="7ef61-299">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="7ef61-300">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="7ef61-300">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="7ef61-301">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="7ef61-301">Output from the component isn't included.</span></span> <span data-ttu-id="7ef61-302">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="7ef61-302">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="7ef61-303">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="7ef61-303">Renders the component into static HTML.</span></span> |

<span data-ttu-id="7ef61-304">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="7ef61-304">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="7ef61-305">Lorsque `RenderMode` est `ServerPrerendered`, le composant est initialement restitué de manière statique dans le cadre de la page.</span><span class="sxs-lookup"><span data-stu-id="7ef61-305">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="7ef61-306">Une fois que le navigateur a établi une connexion au serveur, le composant est *à nouveau*rendu et le composant est maintenant interactif.</span><span class="sxs-lookup"><span data-stu-id="7ef61-306">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="7ef61-307">Si la méthode de cycle de vie [OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods) pour initialiser le composant est présente, la méthode est exécutée *deux fois*:</span><span class="sxs-lookup"><span data-stu-id="7ef61-307">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="7ef61-308">Lorsque le composant est prérendu statiquement.</span><span class="sxs-lookup"><span data-stu-id="7ef61-308">When the component is prerendered statically.</span></span>
* <span data-ttu-id="7ef61-309">Après l’établissement de la connexion au serveur.</span><span class="sxs-lookup"><span data-stu-id="7ef61-309">After the server connection has been established.</span></span>

<span data-ttu-id="7ef61-310">Cela peut entraîner une modification notable des données affichées dans l’interface utilisateur lorsque le composant est finalement restitué.</span><span class="sxs-lookup"><span data-stu-id="7ef61-310">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="7ef61-311">Pour éviter le double rendu du scénario dans une application Blazor Server :</span><span class="sxs-lookup"><span data-stu-id="7ef61-311">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="7ef61-312">Transmettez un identificateur qui peut être utilisé pour mettre en cache l’État pendant le prérendu et pour récupérer l’état après le redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ef61-312">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="7ef61-313">Utilisez l’identificateur pendant le prérendu pour enregistrer l’état du composant.</span><span class="sxs-lookup"><span data-stu-id="7ef61-313">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="7ef61-314">Utilisez l’identificateur après le prérendu pour récupérer l’État mis en cache.</span><span class="sxs-lookup"><span data-stu-id="7ef61-314">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="7ef61-315">Le code suivant illustre une `WeatherForecastService` mise à jour dans une application de serveur Blazor basée sur un modèle qui évite le double rendu :</span><span class="sxs-lookup"><span data-stu-id="7ef61-315">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="7ef61-316">Restituer des composants interactifs avec état à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="7ef61-316">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="7ef61-317">Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="7ef61-317">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="7ef61-318">Lors du rendu de la page ou de la vue :</span><span class="sxs-lookup"><span data-stu-id="7ef61-318">When the page or view renders:</span></span>

* <span data-ttu-id="7ef61-319">Le composant est prérendu avec la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="7ef61-319">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="7ef61-320">L’état initial du composant utilisé pour le prérendu est perdu.</span><span class="sxs-lookup"><span data-stu-id="7ef61-320">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="7ef61-321">Un nouvel état de composant est créé lorsque la connexion SignalR est établie.</span><span class="sxs-lookup"><span data-stu-id="7ef61-321">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="7ef61-322">La page Razor suivante affiche un composant `Counter` :</span><span class="sxs-lookup"><span data-stu-id="7ef61-322">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="7ef61-323">Rendre des composants non interactifs à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="7ef61-323">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="7ef61-324">Dans la page Razor suivante, le composant `Counter` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :</span><span class="sxs-lookup"><span data-stu-id="7ef61-324">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="7ef61-325">Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.</span><span class="sxs-lookup"><span data-stu-id="7ef61-325">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="7ef61-326">Détecter quand l’application est prérendu</span><span class="sxs-lookup"><span data-stu-id="7ef61-326">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="7ef61-327">Configurer le client SignalR pour les applications Blazor Server</span><span class="sxs-lookup"><span data-stu-id="7ef61-327">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="7ef61-328">Parfois, vous devez configurer le client SignalR utilisé par les applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="7ef61-328">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="7ef61-329">Par exemple, vous pouvez configurer la journalisation sur le client SignalR pour diagnostiquer un problème de connexion.</span><span class="sxs-lookup"><span data-stu-id="7ef61-329">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="7ef61-330">Pour configurer le client SignalR dans le fichier *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7ef61-330">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="7ef61-331">Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="7ef61-331">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="7ef61-332">Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ef61-332">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="7ef61-333">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7ef61-333">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
