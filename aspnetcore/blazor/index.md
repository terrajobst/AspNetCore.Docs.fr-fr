---
title: Introduction à Blazor dans ASP.NET Core
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614571"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="87613-103">Introduction à Blazor</span><span class="sxs-lookup"><span data-stu-id="87613-103">Introduction to Blazor</span></span>

<span data-ttu-id="87613-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="87613-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="87613-105">Composants Razor</span><span class="sxs-lookup"><span data-stu-id="87613-105">Razor Components</span></span>

<span data-ttu-id="87613-106">Le modèle d’hébergement côté serveur de Blazor, *Razor Components*, constitue un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET :</span><span class="sxs-lookup"><span data-stu-id="87613-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="87613-107">Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87613-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="87613-108">Partagez les logiques d’applications côté serveur et côté client écrites avec .NET.</span><span class="sxs-lookup"><span data-stu-id="87613-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="87613-109">Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="87613-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="87613-110">Razor Components prend en charge les principales installations que nécessitent la plupart des applications, y compris :</span><span class="sxs-lookup"><span data-stu-id="87613-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="87613-111">Paramètres</span><span class="sxs-lookup"><span data-stu-id="87613-111">Parameters</span></span>
* <span data-ttu-id="87613-112">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="87613-112">Event handling</span></span>
* <span data-ttu-id="87613-113">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="87613-113">Data binding</span></span>
* <span data-ttu-id="87613-114">Routage</span><span class="sxs-lookup"><span data-stu-id="87613-114">Routing</span></span>
* <span data-ttu-id="87613-115">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="87613-115">Dependency injection</span></span>
* <span data-ttu-id="87613-116">Dispositions</span><span class="sxs-lookup"><span data-stu-id="87613-116">Layouts</span></span>
* <span data-ttu-id="87613-117">Création de modèles</span><span class="sxs-lookup"><span data-stu-id="87613-117">Templating</span></span>
* <span data-ttu-id="87613-118">Valeurs en cascade</span><span class="sxs-lookup"><span data-stu-id="87613-118">Cascading values</span></span>

<span data-ttu-id="87613-119">Razor Components dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="87613-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="87613-120">ASP.NET Core Razor Components dans .NET Core 3.0 ajoute la prise en charge de l’hébergement de Razor Components sur le serveur dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87613-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="87613-121">Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="87613-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="87613-122">L’exécution :</span><span class="sxs-lookup"><span data-stu-id="87613-122">The runtime:</span></span>

* <span data-ttu-id="87613-123">Gère l’envoi des événements d’interface utilisateur depuis le navigateur vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="87613-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="87613-124">Applique les mises à jour de l’interface utilisateur envoyées par le serveur au navigateur après avoir exécuté les composants.</span><span class="sxs-lookup"><span data-stu-id="87613-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="87613-125">La connexion utilisée par Razor Components pour communiquer avec le navigateur est aussi utilisée pour gérer les appels d’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87613-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="87613-127">Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="87613-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="87613-128">Composants</span><span class="sxs-lookup"><span data-stu-id="87613-128">Components</span></span>

<span data-ttu-id="87613-129">Un *composant Razor* est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données.</span><span class="sxs-lookup"><span data-stu-id="87613-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="87613-130">Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible.</span><span class="sxs-lookup"><span data-stu-id="87613-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="87613-131">Les composants peuvent être imbriqués et réutilisés.</span><span class="sxs-lookup"><span data-stu-id="87613-131">Components can be nested and reused.</span></span>

<span data-ttu-id="87613-132">Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="87613-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="87613-133">La classe est normalement écrite sous la forme d’une page de balisage Razor avec une extension de fichier *.razor* (Razor Components) ou d’une page de balisage Razor avec une extension de fichier *.cshtml* (Blazor).</span><span class="sxs-lookup"><span data-stu-id="87613-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="87613-134">[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#.</span><span class="sxs-lookup"><span data-stu-id="87613-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="87613-135">Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="87613-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="87613-136">Razor Pages et les vues MVC utilisent également Razor.</span><span class="sxs-lookup"><span data-stu-id="87613-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="87613-137">Contrairement à Razor Pages et aux vues MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour gérer la composition de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="87613-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="87613-138">Razor Components peut être utilisé spécifiquement pour la composition et la logique d’interface utilisateur côté client.</span><span class="sxs-lookup"><span data-stu-id="87613-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="87613-139">Le balisage suivant est un exemple de composant de boîte de dialogue personnalisée :</span><span class="sxs-lookup"><span data-stu-id="87613-139">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="87613-140">Quand ce composant est utilisé ailleurs dans l’application, IntelliSense accélère le développement avec la complétion de syntaxe et de paramètres.</span><span class="sxs-lookup"><span data-stu-id="87613-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="87613-141">Les composants s’affichent dans une représentation en mémoire du navigateur DOM appelé *arborescence d’affichage*. Elle peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="87613-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="87613-142">Interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="87613-142">JavaScript interop</span></span>

<span data-ttu-id="87613-143">Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, les composants interagissent avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87613-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="87613-144">Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87613-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="87613-145">Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#.</span><span class="sxs-lookup"><span data-stu-id="87613-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="87613-146">Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="87613-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="87613-147">Partage de code et .NET Standard</span><span class="sxs-lookup"><span data-stu-id="87613-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="87613-148">Les applications peuvent référencer et utiliser les bibliothèques [.NET Standard](/dotnet/standard/net-standard) existantes.</span><span class="sxs-lookup"><span data-stu-id="87613-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="87613-149">.NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET.</span><span class="sxs-lookup"><span data-stu-id="87613-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="87613-150">Blazor implémente également .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="87613-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="87613-151">Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket, le threading et d’autres fonctionnalités) lèvent <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="87613-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="87613-152">Les bibliothèques de classes .NET Standard peuvent être partagées entre les différentes plateformes .NET, notamment Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.</span><span class="sxs-lookup"><span data-stu-id="87613-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="87613-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="87613-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="87613-154">Blazor est une infrastructure d’application monopage pour la création d’applications web interactives côté client avec .NET.</span><span class="sxs-lookup"><span data-stu-id="87613-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="87613-155">Blazor utilise les normes Web ouvertes sans plug-ins ou transpilation de code.</span><span class="sxs-lookup"><span data-stu-id="87613-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="87613-156">Blazor fonctionne dans tous les navigateurs web modernes, notamment les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="87613-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="87613-157">L’utilisation de .NET dans le navigateur pour le développement web côté client offre de nombreux avantages :</span><span class="sxs-lookup"><span data-stu-id="87613-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="87613-158">**Langage C#** : Écriture de code dans C# plutôt que dans JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87613-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="87613-159">**Écosystème .NET** : Tirez parti de l’écosystème existant des bibliothèques .NET.</span><span class="sxs-lookup"><span data-stu-id="87613-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="87613-160">**Développement de l’empilement complet** : Partager la logique côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="87613-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="87613-161">**Rapidité et scalabilité** : .NET a été conçu pour la performance, la fiabilité et la sécurité.</span><span class="sxs-lookup"><span data-stu-id="87613-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="87613-162">**Outils de pointe** : Restez productif grâce à Visual Studio sur Windows, Linux et macOS.</span><span class="sxs-lookup"><span data-stu-id="87613-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="87613-163">**Stabilité et cohérence** :  Conçu sur un ensemble de langages, d’infrastructures et des outils stables, aux caractéristiques riches et faciles d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="87613-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="87613-164">L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](http://webassembly.org) (abrégé en *wasm*).</span><span class="sxs-lookup"><span data-stu-id="87613-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="87613-165">WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in.</span><span class="sxs-lookup"><span data-stu-id="87613-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="87613-166">WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale.</span><span class="sxs-lookup"><span data-stu-id="87613-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="87613-167">Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur par le biais de l’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87613-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="87613-168">Parallèlement, le code .NET exécuté via WebAssembly s’exécute dans le même bac à sable approuvé JavaScript, afin de prévenir toute action malveillante sur l’ordinateur client.</span><span class="sxs-lookup"><span data-stu-id="87613-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor exécute le code .NET dans le navigateur avec WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="87613-170">Quand une application Blazor est créée et s’exécute dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="87613-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="87613-171">Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.</span><span class="sxs-lookup"><span data-stu-id="87613-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="87613-172">Les assemblys et le runtime .NET sont téléchargés dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="87613-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="87613-173">Blazor démarre le runtime .NET et le configure pour charger les assemblys de l’application.</span><span class="sxs-lookup"><span data-stu-id="87613-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="87613-174">La manipulation du modèle DOM et les appels d’API de navigateur sont gérés par le runtime Blazor par le biais de l’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87613-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="87613-175">Blazor prend en charge les principales installations que nécessitent la plupart des applications, y compris :</span><span class="sxs-lookup"><span data-stu-id="87613-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="87613-176">Paramètres</span><span class="sxs-lookup"><span data-stu-id="87613-176">Parameters</span></span>
* <span data-ttu-id="87613-177">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="87613-177">Event handling</span></span>
* <span data-ttu-id="87613-178">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="87613-178">Data binding</span></span>
* <span data-ttu-id="87613-179">Routage</span><span class="sxs-lookup"><span data-stu-id="87613-179">Routing</span></span>
* <span data-ttu-id="87613-180">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="87613-180">Dependency injection</span></span>
* <span data-ttu-id="87613-181">Dispositions</span><span class="sxs-lookup"><span data-stu-id="87613-181">Layouts</span></span>
* <span data-ttu-id="87613-182">Création de modèles</span><span class="sxs-lookup"><span data-stu-id="87613-182">Templating</span></span>
* <span data-ttu-id="87613-183">Valeurs en cascade</span><span class="sxs-lookup"><span data-stu-id="87613-183">Cascading values</span></span>

<span data-ttu-id="87613-184">Pour réduire la taille du code inutilisé extrait de l’application téléchargée lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="87613-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="87613-185">Blazor côté client est un modèle d’hébergement côté client.</span><span class="sxs-lookup"><span data-stu-id="87613-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="87613-186">Étant donné que Blazor sépare la logique de rendu d’un composant de la façon dont les mises à jour des interfaces utilisateurs sont appliquées, vous disposez d’une flexibilité dans la manière dont Blazor peut être hébergé.</span><span class="sxs-lookup"><span data-stu-id="87613-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="87613-187">Utilisez ASP.NET Core [Razor Components](#razor-components) pour héberger Razor Components sur le serveur dans une application ASP.NET Core où les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="87613-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="87613-188">Pour plus d'informations, consultez <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="87613-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="87613-189">La taille de la charge utile est un facteur de performance essentiel pour qu’une application soit facile d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="87613-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="87613-190">Blazor optimise la taille de la charge utile pour réduire les temps de téléchargement :</span><span class="sxs-lookup"><span data-stu-id="87613-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="87613-191">Les parties inutilisées des assemblys .NET sont supprimées pendant le processus de création.</span><span class="sxs-lookup"><span data-stu-id="87613-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="87613-192">Réponses HTTP compressées.</span><span class="sxs-lookup"><span data-stu-id="87613-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="87613-193">Le runtime .NET et les assemblys sont mis en cache dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="87613-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="87613-194">[Razor Components](#razor-components) fournit une taille de charge utile plus petite que Blazor en conservant les assemblys .NET, l’assembly de l’application et le runtime côté serveur.</span><span class="sxs-lookup"><span data-stu-id="87613-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="87613-195">Les applications Razor Components fournissent uniquement les fichiers de balisage et les ressources statiques aux clients.</span><span class="sxs-lookup"><span data-stu-id="87613-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87613-196">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="87613-196">Additional resources</span></span>

* [<span data-ttu-id="87613-197">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="87613-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="87613-198">Guide C#</span><span class="sxs-lookup"><span data-stu-id="87613-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="87613-199">HTML</span><span class="sxs-lookup"><span data-stu-id="87613-199">HTML</span></span>](https://www.w3.org/html/)
