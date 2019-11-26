---
title: ASP.NET Core Blazor des modèles d’hébergement
author: guardrex
description: Découvrez les modèles d’hébergement Blazor webassembly et Blazor Server.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: a017737eacd93ac776afe7ee8024eed602d7edcc
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317226"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="d5b76-103">ASP.NET Core Blazor des modèles d’hébergement</span><span class="sxs-lookup"><span data-stu-id="d5b76-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="d5b76-104">Par [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d5b76-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="d5b76-105"> est un Framework Web conçu pour s’exécuter côté client dans le navigateur sur un Runtime .NET basé sur [Webassembly](https://webassembly.org/)( *Blazor webassembly*) ou côté serveur dans ASP.net Core ( *serveurBlazor* ).</span><span class="sxs-lookup"><span data-stu-id="d5b76-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="d5b76-106">Quel que soit le modèle d’hébergement, les modèles d’application et de composant *sont les mêmes*.</span><span class="sxs-lookup"><span data-stu-id="d5b76-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="d5b76-107">Pour créer un projet pour les modèles d’hébergement décrits dans cet article, consultez <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="d5b76-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="d5b76-108"> webassembly</span><span class="sxs-lookup"><span data-stu-id="d5b76-108"> WebAssembly</span></span>

<span data-ttu-id="d5b76-109">Le modèle d’hébergement principal pour Blazor s’exécute côté client dans le navigateur sur webassembly.</span><span class="sxs-lookup"><span data-stu-id="d5b76-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="d5b76-110">L’application Blazor, ses dépendances et le Runtime .NET sont téléchargés dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="d5b76-111">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="d5b76-112">Les mises à jour de l’interface utilisateur et la gestion des événements se produisent dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="d5b76-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="d5b76-113">Les ressources de l’application sont déployées en tant que fichiers statiques sur un serveur Web ou un service qui prend en charge le contenu statique sur les clients.</span><span class="sxs-lookup"><span data-stu-id="d5b76-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! Opérationnel. NO-LOC (éblouissant)]<span data-ttu-id="d5b76-114"> webassembly : [ ! Opérationnel. NO-LOC (éblouissant)] l’application s’exécute sur un thread d’interface utilisateur dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="d5b76-115">Pour créer une application Blazor à l’aide du modèle d’hébergement côté client, utilisez le modèle d' **applicationBlazor Webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="d5b76-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="d5b76-116">Après avoir sélectionné le modèle d' **applicationBlazor Webassembly** , vous avez la possibilité de configurer l’application pour utiliser un serveur principal ASP.net core en activant la case à cocher **ASP.net Core hébergé** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="d5b76-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="d5b76-117">L’application ASP.NET Core sert l’application Blazor aux clients.</span><span class="sxs-lookup"><span data-stu-id="d5b76-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="d5b76-118">L’application Blazor webassembly peut interagir avec le serveur sur le réseau à l’aide d’appels d’API Web ou de [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="d5b76-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="d5b76-119">Les modèles incluent le script *éblouissant. webassembly. js* qui gère les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d5b76-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="d5b76-120">Téléchargement du Runtime .NET, de l’application et des dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="d5b76-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="d5b76-121">Initialisation du runtime pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="d5b76-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="d5b76-122">Le modèle d’hébergement webassembly Blazor offre plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="d5b76-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="d5b76-123">Il n’existe aucune dépendance côté serveur .NET.</span><span class="sxs-lookup"><span data-stu-id="d5b76-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="d5b76-124">L’application fonctionne entièrement après son téléchargement sur le client.</span><span class="sxs-lookup"><span data-stu-id="d5b76-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="d5b76-125">Les ressources et les fonctionnalités du client sont pleinement exploitées.</span><span class="sxs-lookup"><span data-stu-id="d5b76-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="d5b76-126">Le travail est déchargé du serveur vers le client.</span><span class="sxs-lookup"><span data-stu-id="d5b76-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="d5b76-127">Un serveur Web ASP.NET Core n’est pas requis pour héberger l’application.</span><span class="sxs-lookup"><span data-stu-id="d5b76-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="d5b76-128">Les scénarios de déploiement sans serveur sont possibles (par exemple, pour servir l’application à partir d’un CDN).</span><span class="sxs-lookup"><span data-stu-id="d5b76-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="d5b76-129">Il existe des inconvénients à Blazor l’hébergement webassembly :</span><span class="sxs-lookup"><span data-stu-id="d5b76-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="d5b76-130">L’application est limitée aux fonctionnalités du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="d5b76-131">Le matériel et le logiciel client compatibles (par exemple, la prise en charge de webassembly) sont requis.</span><span class="sxs-lookup"><span data-stu-id="d5b76-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="d5b76-132">La taille du téléchargement est supérieure, et les applications prennent plus de temps à se charger.</span><span class="sxs-lookup"><span data-stu-id="d5b76-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="d5b76-133">Le Runtime .NET et la prise en charge des outils sont moins matures.</span><span class="sxs-lookup"><span data-stu-id="d5b76-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="d5b76-134">Par exemple, des limitations existent dans la prise en charge de [.NET standard](/dotnet/standard/net-standard) et le débogage.</span><span class="sxs-lookup"><span data-stu-id="d5b76-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="d5b76-135">Serveur de Blazor</span><span class="sxs-lookup"><span data-stu-id="d5b76-135">Blazor Server</span></span>

<span data-ttu-id="d5b76-136">Avec le modèle d’hébergement Blazor Server, l’application est exécutée sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5b76-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="d5b76-137">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés via une connexion [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="d5b76-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Le navigateur interagit avec l’application (hébergée à l’intérieur d’une application ASP.NET Core) sur le serveur via un [ ! Opérationnel. Connexion NO-LOC (Signalr)].](hosting-models/_static/blazor-server.png)

<span data-ttu-id="d5b76-139">Pour créer une application Blazor à l’aide du modèle d’hébergement Blazor Server, utilisez le modèle d' **application ASP.NET CoreBlazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="d5b76-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="d5b76-140">L’application ASP.NET Core héberge l’application Blazor Server et crée le point de terminaison SignalR où les clients se connectent.</span><span class="sxs-lookup"><span data-stu-id="d5b76-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="d5b76-141">L’application ASP.NET Core fait référence à la classe de `Startup` de l’application à ajouter :</span><span class="sxs-lookup"><span data-stu-id="d5b76-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="d5b76-142">Services côté serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-142">Server-side services.</span></span>
* <span data-ttu-id="d5b76-143">L’application vers le pipeline de traitement des demandes.</span><span class="sxs-lookup"><span data-stu-id="d5b76-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="d5b76-144">Le script *éblouissant. Server. js*&dagger; établit la connexion client.</span><span class="sxs-lookup"><span data-stu-id="d5b76-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="d5b76-145">Il est de la responsabilité de l’application de conserver et de restaurer l’état de l’application en fonction des besoins (par exemple, en cas de perte de connexion réseau).</span><span class="sxs-lookup"><span data-stu-id="d5b76-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="d5b76-146">Le modèle d’hébergement Blazor Server offre plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="d5b76-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="d5b76-147">La taille du téléchargement est beaucoup plus petite qu’une application Blazor webassembly, et l’application est chargée beaucoup plus rapidement.</span><span class="sxs-lookup"><span data-stu-id="d5b76-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="d5b76-148">L’application tire pleinement parti des fonctionnalités du serveur, notamment de l’utilisation de toutes les API compatibles avec .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5b76-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="d5b76-149">.NET Core sur le serveur est utilisé pour exécuter l’application, de sorte que les outils .NET existants, tels que le débogage, fonctionnent comme prévu.</span><span class="sxs-lookup"><span data-stu-id="d5b76-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="d5b76-150">Les clients légers sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="d5b76-150">Thin clients are supported.</span></span> <span data-ttu-id="d5b76-151">Par exemple, les applications Blazor Server fonctionnent avec des navigateurs qui ne prennent pas en charge webassembly et sur des appareils à ressources restreintes.</span><span class="sxs-lookup"><span data-stu-id="d5b76-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="d5b76-152">Le .NET/C# base de code de l’application, y compris le code du composant de l’application, n’est pas pris en charge par les clients.</span><span class="sxs-lookup"><span data-stu-id="d5b76-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="d5b76-153">Blazor Hébergement de serveur présente des inconvénients :</span><span class="sxs-lookup"><span data-stu-id="d5b76-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="d5b76-154">Une latence plus élevée existe généralement.</span><span class="sxs-lookup"><span data-stu-id="d5b76-154">Higher latency usually exists.</span></span> <span data-ttu-id="d5b76-155">Chaque interaction utilisateur implique un tronçon réseau.</span><span class="sxs-lookup"><span data-stu-id="d5b76-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="d5b76-156">Il n’existe aucune prise en charge hors connexion.</span><span class="sxs-lookup"><span data-stu-id="d5b76-156">There's no offline support.</span></span> <span data-ttu-id="d5b76-157">Si la connexion cliente échoue, l’application cesse de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="d5b76-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="d5b76-158">L’évolutivité est difficile pour les applications avec de nombreux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="d5b76-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="d5b76-159">Le serveur doit gérer plusieurs connexions clientes et gérer l’état du client.</span><span class="sxs-lookup"><span data-stu-id="d5b76-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="d5b76-160">Un serveur de ASP.NET Core est requis pour servir l’application.</span><span class="sxs-lookup"><span data-stu-id="d5b76-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="d5b76-161">Les scénarios de déploiement sans serveur ne sont pas possibles (par exemple, pour servir l’application à partir d’un CDN).</span><span class="sxs-lookup"><span data-stu-id="d5b76-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="d5b76-162">&dagger;le script *éblouissant. Server. js* est pris en charge à partir d’une ressource incorporée dans le ASP.net Core Framework partagé.</span><span class="sxs-lookup"><span data-stu-id="d5b76-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="d5b76-163">Comparaison avec l’interface utilisateur du rendu serveur</span><span class="sxs-lookup"><span data-stu-id="d5b76-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="d5b76-164">L’une des façons de comprendre les applications Blazor Server consiste à comprendre la différence entre les modèles traditionnels pour le rendu de l’interface utilisateur dans les applications de ASP.NET Core à l’aide de vues Razor ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d5b76-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="d5b76-165">Les deux modèles utilisent le langage Razor pour décrire le contenu HTML, mais ils diffèrent considérablement dans le mode de rendu du balisage.</span><span class="sxs-lookup"><span data-stu-id="d5b76-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="d5b76-166">Quand une page Razor ou une vue est restituée, chaque ligne de code Razor émet du code HTML sous forme de texte.</span><span class="sxs-lookup"><span data-stu-id="d5b76-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="d5b76-167">Après le rendu, le serveur supprime l’instance de la page ou de la vue, y compris tout état produit.</span><span class="sxs-lookup"><span data-stu-id="d5b76-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="d5b76-168">Lorsqu’une autre demande pour la page se produit, par exemple lorsque la validation du serveur échoue et que le résumé des validations s’affiche :</span><span class="sxs-lookup"><span data-stu-id="d5b76-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="d5b76-169">La page entière est de nouveau restituée au texte HTML.</span><span class="sxs-lookup"><span data-stu-id="d5b76-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="d5b76-170">La page est envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="d5b76-170">The page is sent to the client.</span></span>

<span data-ttu-id="d5b76-171">Une application Blazor est composée d’éléments réutilisables de l’interface utilisateur appelée *composants*.</span><span class="sxs-lookup"><span data-stu-id="d5b76-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="d5b76-172">Un composant contient C# du code, un balisage et d’autres composants.</span><span class="sxs-lookup"><span data-stu-id="d5b76-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="d5b76-173">Quand un composant est rendu, Blazor produit un graphique des composants inclus similaires à un Document Object Model XML ou XML (DOM).</span><span class="sxs-lookup"><span data-stu-id="d5b76-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="d5b76-174">Ce graphique comprend l’état des composants contenus dans les propriétés et les champs.</span><span class="sxs-lookup"><span data-stu-id="d5b76-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="d5b76-175"> évalue le graphique du composant pour produire une représentation binaire de la balise.</span><span class="sxs-lookup"><span data-stu-id="d5b76-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="d5b76-176">Le format binaire peut être :</span><span class="sxs-lookup"><span data-stu-id="d5b76-176">The binary format can be:</span></span>

* <span data-ttu-id="d5b76-177">Converti en texte HTML (lors du prérendu).</span><span class="sxs-lookup"><span data-stu-id="d5b76-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="d5b76-178">Utilisé pour mettre à jour efficacement le balisage pendant le rendu normal.</span><span class="sxs-lookup"><span data-stu-id="d5b76-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="d5b76-179">Une mise à jour de l’interface utilisateur dans Blazor est déclenchée par :</span><span class="sxs-lookup"><span data-stu-id="d5b76-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="d5b76-180">Intervention de l’utilisateur, telle que la sélection d’un bouton.</span><span class="sxs-lookup"><span data-stu-id="d5b76-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="d5b76-181">Déclencheurs d’application, tels qu’un minuteur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="d5b76-182">Le graphique est rerendu et *une différence d’interface utilisateur* (différence) est calculée.</span><span class="sxs-lookup"><span data-stu-id="d5b76-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="d5b76-183">Cette différence est le plus petit ensemble de modifications DOM requis pour mettre à jour l’interface utilisateur sur le client.</span><span class="sxs-lookup"><span data-stu-id="d5b76-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="d5b76-184">Le diff est envoyé au client dans un format binaire et appliqué par le navigateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="d5b76-185">Un composant est supprimé une fois que l’utilisateur l’a quittée sur le client.</span><span class="sxs-lookup"><span data-stu-id="d5b76-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="d5b76-186">Lorsqu’un utilisateur interagit avec un composant, l’état du composant (services, ressources) doit être conservé dans la mémoire du serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="d5b76-187">Étant donné que l’état de nombreux composants peut être géré simultanément par le serveur, l’épuisement de la mémoire est un problème qui doit être résolu.</span><span class="sxs-lookup"><span data-stu-id="d5b76-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="d5b76-188">Pour obtenir des conseils sur la création d’une application Blazor Server afin de garantir la meilleure utilisation de la mémoire du serveur, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="d5b76-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="circuits"></a><span data-ttu-id="d5b76-189">Électriques</span><span class="sxs-lookup"><span data-stu-id="d5b76-189">Circuits</span></span>

<span data-ttu-id="d5b76-190">Une application Blazor Server s’appuie sur [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="d5b76-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="d5b76-191">Chaque client communique avec le serveur sur une ou plusieurs connexions SignalR appelées *circuit*.</span><span class="sxs-lookup"><span data-stu-id="d5b76-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="d5b76-192">Un circuit est l’abstraction de Blazorsur SignalR connexions qui peuvent tolérer des interruptions réseau temporaires.</span><span class="sxs-lookup"><span data-stu-id="d5b76-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="d5b76-193">Lorsqu’un client Blazor constate que la connexion SignalR est déconnectée, il tente de se reconnecter au serveur à l’aide d’une nouvelle connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="d5b76-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="d5b76-194">Chaque écran de navigateur (onglet ou IFRAME) qui est connecté à une application Blazor Server utilise une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="d5b76-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="d5b76-195">Il s’agit encore d’une autre distinction importante par rapport aux applications classiques affichées par le serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="d5b76-196">Dans une application affichée sur un serveur, l’ouverture de la même application dans plusieurs écrans de navigateur n’est généralement pas convertie en demandes de ressources supplémentaires sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="d5b76-197">Dans une application Blazor Server, chaque écran du navigateur requiert un circuit distinct et des instances distinctes de l’état du composant à gérer par le serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="d5b76-198"> envisage de fermer un onglet de navigateur ou de naviguer vers une URL externe un arrêt *normal* .</span><span class="sxs-lookup"><span data-stu-id="d5b76-198"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="d5b76-199">En cas de résiliation appropriée, le circuit et les ressources associées sont immédiatement libérés.</span><span class="sxs-lookup"><span data-stu-id="d5b76-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="d5b76-200">Un client peut également se déconnecter de manière non appropriée, par exemple en raison d’une interruption du réseau.</span><span class="sxs-lookup"><span data-stu-id="d5b76-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="d5b76-201"> Server stocke les circuits déconnectés pour un intervalle configurable afin de permettre au client de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="d5b76-201"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="d5b76-202">Pour plus d’informations, consultez la section [reconnexion au même serveur](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="d5b76-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="d5b76-203">Latence de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="d5b76-203">UI Latency</span></span>

<span data-ttu-id="d5b76-204">La latence de l’interface utilisateur est le temps qu’il faut après une action initiée jusqu’au moment où l’interface utilisateur est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="d5b76-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="d5b76-205">Des valeurs plus petites pour la latence de l’interface utilisateur sont impératives pour qu’une application soit réactive à un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="d5b76-206">Dans une application Blazor Server, chaque action est envoyée au serveur, traitée et une comparaison d’interface utilisateur est renvoyée.</span><span class="sxs-lookup"><span data-stu-id="d5b76-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="d5b76-207">Par conséquent, la latence de l’interface utilisateur est la somme de la latence du réseau et de la latence du serveur lors du traitement de l’action.</span><span class="sxs-lookup"><span data-stu-id="d5b76-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="d5b76-208">Pour une application métier limitée à un réseau d’entreprise privé, l’effet sur les perceptions des utilisateurs de la latence en raison de la latence du réseau est généralement inperceptible.</span><span class="sxs-lookup"><span data-stu-id="d5b76-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="d5b76-209">Pour une application déployée sur Internet, la latence peut devenir perceptible pour les utilisateurs, en particulier si les utilisateurs sont largement répartis géographiquement.</span><span class="sxs-lookup"><span data-stu-id="d5b76-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="d5b76-210">L’utilisation de la mémoire peut également contribuer à la latence de l’application.</span><span class="sxs-lookup"><span data-stu-id="d5b76-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="d5b76-211">L’augmentation de l’utilisation de la mémoire permet de garbage collection fréquentes ou la pagination de la mémoire sur le disque, qui dégradent les performances des applications et augmentent la latence de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="d5b76-212">Pour plus d'informations, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="d5b76-212">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="d5b76-213"> applications serveur doivent être optimisées pour réduire la latence de l’interface utilisateur en réduisant la latence du réseau et l’utilisation de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="d5b76-213"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="d5b76-214">Pour une approche de la mesure de la latence du réseau, consultez <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="d5b76-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="d5b76-215">Pour plus d’informations sur les SignalR et les Blazor, consultez :</span><span class="sxs-lookup"><span data-stu-id="d5b76-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="d5b76-216">Connexion au serveur</span><span class="sxs-lookup"><span data-stu-id="d5b76-216">Connection to the server</span></span>

Blazor<span data-ttu-id="d5b76-217"> applications serveur requièrent une connexion SignalR active au serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-217"> Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="d5b76-218">Si la connexion est perdue, l’application tente de se reconnecter au serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="d5b76-219">Tant que l’état du client est toujours en mémoire, la session client reprend sans perte d’État.</span><span class="sxs-lookup"><span data-stu-id="d5b76-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="d5b76-220">Reconnexion au même serveur</span><span class="sxs-lookup"><span data-stu-id="d5b76-220">Reconnection to the same server</span></span>

<span data-ttu-id="d5b76-221">Une application Blazor Server effectue un prérendu en réponse à la première demande du client, qui configure l’état de l’interface utilisateur sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-221">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="d5b76-222">Lorsque le client tente de créer une connexion SignalR, le client doit se reconnecter au même serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-222">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> Blazor<span data-ttu-id="d5b76-223"> applications serveur qui utilisent plusieurs serveurs principaux doivent implémenter des *sessions rémanentes* pour les connexions SignalR.</span><span class="sxs-lookup"><span data-stu-id="d5b76-223"> Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="d5b76-224">Nous vous recommandons d’utiliser le [service Azure SignalR](/azure/azure-signalr) pour les applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d5b76-224">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="d5b76-225">Le service permet la mise à l’échelle d’une application Blazor Server vers un grand nombre de connexions SignalR simultanées.</span><span class="sxs-lookup"><span data-stu-id="d5b76-225">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="d5b76-226">Les sessions rémanentes sont activées pour le service Azure SignalR en définissant l’option de `ServerStickyMode` ou la valeur de configuration du service sur `Required`.</span><span class="sxs-lookup"><span data-stu-id="d5b76-226">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="d5b76-227">Pour plus d'informations, consultez <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d5b76-227">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="d5b76-228">Lorsque vous utilisez IIS, les sessions rémanentes sont activées avec Application Request Routing.</span><span class="sxs-lookup"><span data-stu-id="d5b76-228">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="d5b76-229">Pour plus d’informations, consultez [équilibrage de charge http à l’aide de application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="d5b76-229">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="d5b76-230">Refléter l’état de la connexion dans l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="d5b76-230">Reflect the connection state in the UI</span></span>

<span data-ttu-id="d5b76-231">Lorsque le client détecte que la connexion a été perdue, une interface utilisateur par défaut est affichée à l’utilisateur pendant que le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="d5b76-231">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="d5b76-232">En cas d’échec de la reconnexion, l’utilisateur a la possibilité de réessayer.</span><span class="sxs-lookup"><span data-stu-id="d5b76-232">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="d5b76-233">Pour personnaliser l’interface utilisateur, définissez un élément avec un `id` de `components-reconnect-modal` dans la `<body>` de la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d5b76-233">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```html
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="d5b76-234">Le tableau suivant décrit les classes CSS appliquées à l’élément `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="d5b76-234">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="d5b76-235">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="d5b76-235">CSS class</span></span>                       | <span data-ttu-id="d5b76-236">Indique&hellip;</span><span class="sxs-lookup"><span data-stu-id="d5b76-236">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="d5b76-237">Connexion perdue.</span><span class="sxs-lookup"><span data-stu-id="d5b76-237">A lost connection.</span></span> <span data-ttu-id="d5b76-238">Le client tente de se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="d5b76-238">The client is attempting to reconnect.</span></span> <span data-ttu-id="d5b76-239">Affichez le modal.</span><span class="sxs-lookup"><span data-stu-id="d5b76-239">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="d5b76-240">Une connexion active est rétablie sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-240">An active connection is re-established to the server.</span></span> <span data-ttu-id="d5b76-241">Masquez le modal.</span><span class="sxs-lookup"><span data-stu-id="d5b76-241">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="d5b76-242">Échec de la reconnexion, probablement en raison d’une défaillance du réseau.</span><span class="sxs-lookup"><span data-stu-id="d5b76-242">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="d5b76-243">Pour tenter une reconnexion, appelez `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="d5b76-243">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="d5b76-244">Reconnexion refusée.</span><span class="sxs-lookup"><span data-stu-id="d5b76-244">Reconnection rejected.</span></span> <span data-ttu-id="d5b76-245">Le serveur a été atteint mais a refusé la connexion, et l’état de l’utilisateur sur le serveur est perdu.</span><span class="sxs-lookup"><span data-stu-id="d5b76-245">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="d5b76-246">Pour recharger l’application, appelez `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="d5b76-246">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="d5b76-247">Cet état de connexion peut se produire dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="d5b76-247">This connection state may result when:</span></span><ul><li><span data-ttu-id="d5b76-248">Un blocage dans le circuit côté serveur se produit.</span><span class="sxs-lookup"><span data-stu-id="d5b76-248">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="d5b76-249">Le client est déconnecté suffisamment longtemps pour que le serveur supprime l’état de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-249">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="d5b76-250">Les instances des composants avec lesquels l’utilisateur interagit sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="d5b76-250">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="d5b76-251">Le serveur est redémarré ou le processus de travail de l’application est recyclé.</span><span class="sxs-lookup"><span data-stu-id="d5b76-251">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="d5b76-252">Reconnexion avec état après le prérendu</span><span class="sxs-lookup"><span data-stu-id="d5b76-252">Stateful reconnection after prerendering</span></span>

Blazor<span data-ttu-id="d5b76-253"> applications serveur sont configurées par défaut pour prérestituer l’interface utilisateur sur le serveur avant que la connexion cliente au serveur soit établie.</span><span class="sxs-lookup"><span data-stu-id="d5b76-253"> Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="d5b76-254">Elle est configurée dans la page Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d5b76-254">This is set up in the *_Host.cshtml* Razor page:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

<span data-ttu-id="d5b76-255">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="d5b76-255">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="d5b76-256">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="d5b76-256">Is prerendered into the page.</span></span>
* <span data-ttu-id="d5b76-257">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer un Blazor application à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-257">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="d5b76-258">Description</span><span class="sxs-lookup"><span data-stu-id="d5b76-258">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="d5b76-259">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d5b76-259">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="d5b76-260">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="d5b76-260">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="d5b76-261">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d5b76-261">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="d5b76-262">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="d5b76-262">Output from the component isn't included.</span></span> <span data-ttu-id="d5b76-263">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="d5b76-263">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="d5b76-264">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="d5b76-264">Renders the component into static HTML.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="d5b76-265">Description</span><span class="sxs-lookup"><span data-stu-id="d5b76-265">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="d5b76-266">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d5b76-266">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="d5b76-267">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="d5b76-267">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="d5b76-268">Les paramètres ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="d5b76-268">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="d5b76-269">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d5b76-269">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="d5b76-270">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="d5b76-270">Output from the component isn't included.</span></span> <span data-ttu-id="d5b76-271">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="d5b76-271">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="d5b76-272">Les paramètres ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="d5b76-272">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="d5b76-273">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="d5b76-273">Renders the component into static HTML.</span></span> <span data-ttu-id="d5b76-274">Les paramètres sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="d5b76-274">Parameters are supported.</span></span> |

::: moniker-end

<span data-ttu-id="d5b76-275">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="d5b76-275">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="d5b76-276">Lorsque `RenderMode` est `ServerPrerendered`, le composant est initialement restitué de manière statique dans le cadre de la page.</span><span class="sxs-lookup"><span data-stu-id="d5b76-276">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="d5b76-277">Une fois que le navigateur a établi une connexion au serveur, le composant est *à nouveau*rendu et le composant est maintenant interactif.</span><span class="sxs-lookup"><span data-stu-id="d5b76-277">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="d5b76-278">Si une [méthode de cycle de vie](xref:blazor/components#lifecycle-methods) pour l’initialisation du composant (`OnInitialized{Async}`) est présente, la méthode est exécutée *deux fois*:</span><span class="sxs-lookup"><span data-stu-id="d5b76-278">If a [lifecycle method](xref:blazor/components#lifecycle-methods) for initializing the component (`OnInitialized{Async}`) is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="d5b76-279">Lorsque le composant est prérendu statiquement.</span><span class="sxs-lookup"><span data-stu-id="d5b76-279">When the component is prerendered statically.</span></span>
* <span data-ttu-id="d5b76-280">Après l’établissement de la connexion au serveur.</span><span class="sxs-lookup"><span data-stu-id="d5b76-280">After the server connection has been established.</span></span>

<span data-ttu-id="d5b76-281">Cela peut entraîner une modification notable des données affichées dans l’interface utilisateur lorsque le composant est finalement restitué.</span><span class="sxs-lookup"><span data-stu-id="d5b76-281">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="d5b76-282">Pour éviter le double rendu du scénario dans une application Blazor Server :</span><span class="sxs-lookup"><span data-stu-id="d5b76-282">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="d5b76-283">Transmettez un identificateur qui peut être utilisé pour mettre en cache l’État pendant le prérendu et pour récupérer l’état après le redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="d5b76-283">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="d5b76-284">Utilisez l’identificateur pendant le prérendu pour enregistrer l’état du composant.</span><span class="sxs-lookup"><span data-stu-id="d5b76-284">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="d5b76-285">Utilisez l’identificateur après le prérendu pour récupérer l’État mis en cache.</span><span class="sxs-lookup"><span data-stu-id="d5b76-285">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="d5b76-286">Le code suivant illustre une `WeatherForecastService` mise à jour dans une application de serveur Blazor basée sur un modèle qui évite le double rendu :</span><span class="sxs-lookup"><span data-stu-id="d5b76-286">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
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
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="d5b76-287">Restituer des composants interactifs avec état à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="d5b76-287">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="d5b76-288">Les composants interactifs avec état peuvent être ajoutés à une page ou à une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="d5b76-288">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="d5b76-289">Lors du rendu de la page ou de la vue :</span><span class="sxs-lookup"><span data-stu-id="d5b76-289">When the page or view renders:</span></span>

* <span data-ttu-id="d5b76-290">Le composant est prérendu avec la page ou la vue.</span><span class="sxs-lookup"><span data-stu-id="d5b76-290">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="d5b76-291">L’état initial du composant utilisé pour le prérendu est perdu.</span><span class="sxs-lookup"><span data-stu-id="d5b76-291">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="d5b76-292">Un nouvel état de composant est créé lorsque la connexion SignalR est établie.</span><span class="sxs-lookup"><span data-stu-id="d5b76-292">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="d5b76-293">La page Razor suivante affiche un composant `Counter` :</span><span class="sxs-lookup"><span data-stu-id="d5b76-293">The following Razor page renders a `Counter` component:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="d5b76-294">Rendre des composants non interactifs à partir de pages et de vues Razor</span><span class="sxs-lookup"><span data-stu-id="d5b76-294">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="d5b76-295">Dans la page Razor suivante, le composant `MyComponent` est restitué statiquement avec une valeur initiale spécifiée à l’aide d’un formulaire :</span><span class="sxs-lookup"><span data-stu-id="d5b76-295">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>

::: moniker range=">= aspnetcore-3.1"

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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

<span data-ttu-id="d5b76-296">Étant donné que `MyComponent` est rendu statiquement, le composant ne peut pas être interactif.</span><span class="sxs-lookup"><span data-stu-id="d5b76-296">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="d5b76-297">Détecter quand l’application est prérendu</span><span class="sxs-lookup"><span data-stu-id="d5b76-297">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="d5b76-298">Configurer le client SignalR pour les applications Blazor Server</span><span class="sxs-lookup"><span data-stu-id="d5b76-298">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="d5b76-299">Parfois, vous devez configurer le client SignalR utilisé par les applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d5b76-299">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="d5b76-300">Par exemple, vous pouvez configurer la journalisation sur le client SignalR pour diagnostiquer un problème de connexion.</span><span class="sxs-lookup"><span data-stu-id="d5b76-300">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="d5b76-301">Pour configurer le client SignalR dans le fichier *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d5b76-301">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="d5b76-302">Ajoutez un attribut `autostart="false"` à la balise `<script>` pour le script *éblouissant. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="d5b76-302">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="d5b76-303">Appelez `Blazor.start` et transmettez un objet de configuration qui spécifie le générateur de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d5b76-303">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="d5b76-304">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d5b76-304">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
