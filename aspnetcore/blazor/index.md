---
title: Introduction à Blazor dans ASP.NET Core
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982995"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="b98af-103">Introduction à Blazor</span><span class="sxs-lookup"><span data-stu-id="b98af-103">Introduction to Blazor</span></span>

<span data-ttu-id="b98af-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b98af-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b98af-105">Bienvenue dans Blazor !</span><span class="sxs-lookup"><span data-stu-id="b98af-105">Welcome to Blazor!</span></span>

<span data-ttu-id="b98af-106">Créez une interface utilisateur web interactive côté client à l’aide de .NET :</span><span class="sxs-lookup"><span data-stu-id="b98af-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="b98af-107">Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b98af-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="b98af-108">Partagez la logique d’application côté serveur et côté client écrite avec .NET.</span><span class="sxs-lookup"><span data-stu-id="b98af-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="b98af-109">Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="b98af-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="b98af-110">Blazor prend en charge les principaux scénarios que nécessitent la plupart des applications :</span><span class="sxs-lookup"><span data-stu-id="b98af-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="b98af-111">Paramètres</span><span class="sxs-lookup"><span data-stu-id="b98af-111">Parameters</span></span>
* <span data-ttu-id="b98af-112">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="b98af-112">Event handling</span></span>
* <span data-ttu-id="b98af-113">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="b98af-113">Data binding</span></span>
* <span data-ttu-id="b98af-114">Routage</span><span class="sxs-lookup"><span data-stu-id="b98af-114">Routing</span></span>
* <span data-ttu-id="b98af-115">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="b98af-115">Dependency injection</span></span>
* <span data-ttu-id="b98af-116">Dispositions</span><span class="sxs-lookup"><span data-stu-id="b98af-116">Layouts</span></span>
* <span data-ttu-id="b98af-117">Modèles</span><span class="sxs-lookup"><span data-stu-id="b98af-117">Templates</span></span>
* <span data-ttu-id="b98af-118">Valeurs en cascade</span><span class="sxs-lookup"><span data-stu-id="b98af-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="b98af-119">Composants</span><span class="sxs-lookup"><span data-stu-id="b98af-119">Components</span></span>

<span data-ttu-id="b98af-120">Dans Blazor, un *composant* est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données.</span><span class="sxs-lookup"><span data-stu-id="b98af-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="b98af-121">Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible.</span><span class="sxs-lookup"><span data-stu-id="b98af-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="b98af-122">Les composants peuvent être imbriqués et réutilisés.</span><span class="sxs-lookup"><span data-stu-id="b98af-122">Components can be nested and reused.</span></span>

<span data-ttu-id="b98af-123">Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="b98af-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="b98af-124">La classe de composant est généralement écrite sous la forme d’une page de balisage Razor avec une extension de fichier *.razor*.</span><span class="sxs-lookup"><span data-stu-id="b98af-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="b98af-125">Les composants dans Blazor sont parfois appelés composants Razor.</span><span class="sxs-lookup"><span data-stu-id="b98af-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="b98af-126">[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#.</span><span class="sxs-lookup"><span data-stu-id="b98af-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="b98af-127">Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="b98af-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="b98af-128">Razor Pages et les vues MVC utilisent également Razor.</span><span class="sxs-lookup"><span data-stu-id="b98af-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="b98af-129">Contrairement à Razor Pages et aux vues MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour gérer la composition de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b98af-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="b98af-130">Les composants Razor peuvent être utilisés spécifiquement pour la composition et la logique d’interface utilisateur côté client.</span><span class="sxs-lookup"><span data-stu-id="b98af-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="b98af-131">Le balisage suivant est un exemple de composant de boîte de dialogue personnalisée :</span><span class="sxs-lookup"><span data-stu-id="b98af-131">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="b98af-132">Quand ce composant est utilisé ailleurs dans l’application, IntelliSense dans [Visual Studio](https://visualstudio.microsoft.com/vs/) accélère le développement avec la complétion de syntaxe et de paramètres.</span><span class="sxs-lookup"><span data-stu-id="b98af-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="b98af-133">Les composants s’affichent dans une représentation en mémoire du navigateur DOM appelé *arborescence d’affichage*. Elle peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="b98af-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="b98af-134">Blazor côté serveur</span><span class="sxs-lookup"><span data-stu-id="b98af-134">Blazor server-side</span></span>

<span data-ttu-id="b98af-135">Blazor dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="b98af-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="b98af-136">Blazor côté serveur prend en charge l’hébergement des composants Razor sur le serveur dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b98af-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="b98af-137">Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="b98af-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="b98af-138">L’exécution :</span><span class="sxs-lookup"><span data-stu-id="b98af-138">The runtime:</span></span>

* <span data-ttu-id="b98af-139">Gère l’envoi des événements d’interface utilisateur depuis le navigateur vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="b98af-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="b98af-140">Applique les mises à jour de l’interface utilisateur envoyées par le serveur au navigateur après avoir exécuté les composants.</span><span class="sxs-lookup"><span data-stu-id="b98af-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="b98af-141">La connexion utilisée par Blazor côté serveur pour communiquer avec le navigateur est aussi utilisée pour gérer les appels d’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b98af-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor côté serveur exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR.](index/_static/blazor-server-side.png)

<span data-ttu-id="b98af-143">Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="b98af-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="b98af-144">Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="b98af-144">Blazor client-side</span></span>

<span data-ttu-id="b98af-145">Blazor côté client est un framework d’application monopage pour la création d’applications web interactives côté client avec .NET.</span><span class="sxs-lookup"><span data-stu-id="b98af-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="b98af-146">Blazor côté client utilise des standards web ouverts sans transpilation de plug-ins ou de code.</span><span class="sxs-lookup"><span data-stu-id="b98af-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="b98af-147">Blazor côté client fonctionne dans tous les navigateurs web modernes, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="b98af-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="b98af-148">L’utilisation de .NET dans le navigateur pour le développement web côté client offre de nombreux avantages :</span><span class="sxs-lookup"><span data-stu-id="b98af-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="b98af-149">**Langage C#** : Écriture de code dans C# plutôt que dans JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b98af-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="b98af-150">**Écosystème .NET** : Tirez parti de l’écosystème existant des bibliothèques .NET.</span><span class="sxs-lookup"><span data-stu-id="b98af-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="b98af-151">**Développement de l’empilement complet** : Partager la logique côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="b98af-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="b98af-152">**Rapidité et scalabilité** : .NET a été conçu pour la performance, la fiabilité et la sécurité.</span><span class="sxs-lookup"><span data-stu-id="b98af-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="b98af-153">**Outils de pointe** : Restez productif grâce à Visual Studio sur Windows, Linux et macOS.</span><span class="sxs-lookup"><span data-stu-id="b98af-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="b98af-154">**Stabilité et cohérence** :  Développez avec un ensemble commun de langages, de frameworks et d’outils stables, riches en fonctionnalités et faciles à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b98af-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="b98af-155">L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](http://webassembly.org) (abrégé en *wasm*).</span><span class="sxs-lookup"><span data-stu-id="b98af-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="b98af-156">WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in.</span><span class="sxs-lookup"><span data-stu-id="b98af-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="b98af-157">WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale.</span><span class="sxs-lookup"><span data-stu-id="b98af-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="b98af-158">Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur par le biais de l’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b98af-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="b98af-159">Parallèlement, le code .NET exécuté via WebAssembly s’exécute dans le même bac à sable approuvé JavaScript, afin de prévenir toute action malveillante sur l’ordinateur client.</span><span class="sxs-lookup"><span data-stu-id="b98af-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor côté client exécute du code .NET dans le navigateur avec WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="b98af-161">Quand une application Blazor côté client est créée et exécutée dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="b98af-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="b98af-162">Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.</span><span class="sxs-lookup"><span data-stu-id="b98af-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="b98af-163">Les assemblys et le runtime .NET sont téléchargés dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b98af-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="b98af-164">Blazor côté client amorce le runtime .NET et configure le runtime pour charger les assemblys de l’application.</span><span class="sxs-lookup"><span data-stu-id="b98af-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="b98af-165">La manipulation d’objets DOM (Document Object Model) et les appels d’API du navigateur sont gérés par le runtime Blazor côté client avec l’interopérabilité de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b98af-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="b98af-166">Pour réduire la taille du code inutilisé extrait de l’application téléchargée lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="b98af-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="b98af-167">Blazor côté client est un modèle d’hébergement côté client.</span><span class="sxs-lookup"><span data-stu-id="b98af-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="b98af-168">Étant donné que Blazor sépare la logique de rendu d’un composant de la façon dont les mises à jour des interfaces utilisateurs sont appliquées, vous disposez d’une flexibilité dans la manière dont Blazor peut être hébergé.</span><span class="sxs-lookup"><span data-stu-id="b98af-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="b98af-169">Utilisez [Blazor côté serveur](#blazor-server-side) pour héberger Blazor sur le serveur dans une application ASP.NET Core où les mises à jour de l’interface utilisateur sont gérées via une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="b98af-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="b98af-170">Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="b98af-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="b98af-171">La taille de la charge utile est un facteur de performance essentiel pour qu’une application soit facile d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="b98af-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="b98af-172">Blazor côté client optimise la taille de la charge utile pour réduire les temps de téléchargement :</span><span class="sxs-lookup"><span data-stu-id="b98af-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="b98af-173">Les parties inutilisées des assemblys .NET sont supprimées pendant le processus de création.</span><span class="sxs-lookup"><span data-stu-id="b98af-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="b98af-174">Réponses HTTP compressées.</span><span class="sxs-lookup"><span data-stu-id="b98af-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="b98af-175">Le runtime .NET et les assemblys sont mis en cache dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b98af-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="b98af-176">[Blazor côté serveur](#blazor-server-side) offre une plus petite taille de charge utile que Blazor côté client en conservant les assemblys .NET, l’assembly de l’application et le runtime côté serveur.</span><span class="sxs-lookup"><span data-stu-id="b98af-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="b98af-177">Les applications Blazor côté serveur servent uniquement les fichiers de balisage et les ressources statiques aux clients.</span><span class="sxs-lookup"><span data-stu-id="b98af-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="b98af-178">Interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="b98af-178">JavaScript interop</span></span>

<span data-ttu-id="b98af-179">Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, les composants interagissent avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b98af-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="b98af-180">Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b98af-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="b98af-181">Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#.</span><span class="sxs-lookup"><span data-stu-id="b98af-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="b98af-182">Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="b98af-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="b98af-183">Partage de code et .NET Standard</span><span class="sxs-lookup"><span data-stu-id="b98af-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="b98af-184">Les applications peuvent référencer et utiliser les bibliothèques [.NET Standard](/dotnet/standard/net-standard) existantes.</span><span class="sxs-lookup"><span data-stu-id="b98af-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="b98af-185">.NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET.</span><span class="sxs-lookup"><span data-stu-id="b98af-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="b98af-186">Blazor implémente également .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="b98af-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="b98af-187">Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket, le threading et d’autres fonctionnalités) lèvent <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="b98af-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="b98af-188">Vous pouvez partager les bibliothèques de classes .NET Standard entre les différentes plateformes .NET, comme Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.</span><span class="sxs-lookup"><span data-stu-id="b98af-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b98af-189">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b98af-189">Additional resources</span></span>

* [<span data-ttu-id="b98af-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="b98af-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="b98af-191">Guide C#</span><span class="sxs-lookup"><span data-stu-id="b98af-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="b98af-192">HTML</span><span class="sxs-lookup"><span data-stu-id="b98af-192">HTML</span></span>](https://www.w3.org/html/)
