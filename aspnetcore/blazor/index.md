---
title: Présentation de ASP.NET Core éblouissant
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 10/15/2019
uid: blazor/index
ms.openlocfilehash: abf631b5e1cf762eaef4bd85a6b85802c9899291
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391153"
---
# <a name="introduction-to-aspnet-core-blazor"></a><span data-ttu-id="4058c-103">Présentation de ASP.NET Core éblouissant</span><span class="sxs-lookup"><span data-stu-id="4058c-103">Introduction to ASP.NET Core Blazor</span></span>

<span data-ttu-id="4058c-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4058c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4058c-105">*Bienvenue dans Blazor !*</span><span class="sxs-lookup"><span data-stu-id="4058c-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="4058c-106">Blazor est une infrastructure pour la création d’interfaces utilisateur d’applications web interactives côté client avec .NET :</span><span class="sxs-lookup"><span data-stu-id="4058c-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="4058c-107">Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4058c-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="4058c-108">Partagez la logique d’application côté serveur et côté client écrite dans .NET.</span><span class="sxs-lookup"><span data-stu-id="4058c-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="4058c-109">Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="4058c-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="4058c-110">L’utilisation de .NET dans le développement web côté client offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="4058c-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="4058c-111">Écriture de code dans C# plutôt que dans JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4058c-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="4058c-112">Tirez parti de l’écosystème .NET existant des bibliothèques .NET.</span><span class="sxs-lookup"><span data-stu-id="4058c-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="4058c-113">Partagez la logique de l’application sur le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="4058c-113">Share app logic across server and client.</span></span>
* <span data-ttu-id="4058c-114">Bénéficiez des performances, de la fiabilité et de la sécurité de .NET.</span><span class="sxs-lookup"><span data-stu-id="4058c-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="4058c-115">Restez productif grâce à Visual Studio sur Windows, Linux et macOS.</span><span class="sxs-lookup"><span data-stu-id="4058c-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="4058c-116">Développez avec un ensemble commun de langages, de frameworks et d’outils stables, riches en fonctionnalités et faciles à utiliser.</span><span class="sxs-lookup"><span data-stu-id="4058c-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="4058c-117">Composants</span><span class="sxs-lookup"><span data-stu-id="4058c-117">Components</span></span>

<span data-ttu-id="4058c-118">Les applications Blazor sont basées sur des *composants*.</span><span class="sxs-lookup"><span data-stu-id="4058c-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="4058c-119">Dans Blazor, un composant est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données.</span><span class="sxs-lookup"><span data-stu-id="4058c-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="4058c-120">Les composants sont des classes .NET intégrées dans des assemblys .NET qui :</span><span class="sxs-lookup"><span data-stu-id="4058c-120">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="4058c-121">Définissent la logique de rendu de l’interface utilisateur flexible.</span><span class="sxs-lookup"><span data-stu-id="4058c-121">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="4058c-122">Gèrent les événements de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4058c-122">Handle user events.</span></span>
* <span data-ttu-id="4058c-123">Peuvent être imbriqués et réutilisés.</span><span class="sxs-lookup"><span data-stu-id="4058c-123">Can be nested and reused.</span></span>
* <span data-ttu-id="4058c-124">Peuvent être partagés et distribués en tant que [bibliothèques de classes Razor](xref:razor-pages/ui-class) ou [packages NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="4058c-124">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="4058c-125">La classe de composant est généralement écrite sous la forme d’une page de balisage [Razor](xref:mvc/views/razor) avec une extension de fichier *.razor*.</span><span class="sxs-lookup"><span data-stu-id="4058c-125">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a *.razor* file extension.</span></span> <span data-ttu-id="4058c-126">Les composants dans Blazor sont appelés *composants Razor*.</span><span class="sxs-lookup"><span data-stu-id="4058c-126">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="4058c-127">Razor est une syntaxe pour la combinaison de balisage HTML et de code C# conçu pour la productivité des développeurs.</span><span class="sxs-lookup"><span data-stu-id="4058c-127">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="4058c-128">Razor permet de basculer entre le balisage HTML et C# dans le même fichier avec le support d’[IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="4058c-128">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="4058c-129">Razor Pages et MVC utilisent également Razor.</span><span class="sxs-lookup"><span data-stu-id="4058c-129">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="4058c-130">Contrairement à Razor Pages et à MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour la logique et la composition de l’interface utilisateur côté client.</span><span class="sxs-lookup"><span data-stu-id="4058c-130">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="4058c-131">Le balisage Razor suivant montre un composant (*Dialog.razor*), qui peut être imbriqué dans un autre composant :</span><span class="sxs-lookup"><span data-stu-id="4058c-131">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

<span data-ttu-id="4058c-132">Le contenu du corps (`ChildContent`) et le titre (`Title`) de la boîte de dialogue sont fournis par le composant qui utilise ce composant dans son interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4058c-132">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="4058c-133">`OnYes` est une méthode C# déclenchée par l’événement `onclick` du bouton.</span><span class="sxs-lookup"><span data-stu-id="4058c-133">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="4058c-134">Blazor utilise des balises HTML naturelles pour la composition de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4058c-134">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="4058c-135">Les éléments HTML spécifient des composants et les attributs d’une balise passent les valeurs aux propriétés d’un composant.</span><span class="sxs-lookup"><span data-stu-id="4058c-135">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="4058c-136">Dans l’exemple suivant, le composant `Index` utilise le composant `Dialog`.</span><span class="sxs-lookup"><span data-stu-id="4058c-136">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="4058c-137">`ChildContent` et `Title` sont définis par les attributs et le contenu de l’élément `<Dialog>`.</span><span class="sxs-lookup"><span data-stu-id="4058c-137">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="4058c-138">*Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="4058c-138">*Index.razor*:</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="4058c-139">La boîte de dialogue est affichée lorsque le parent (*Index.razor*) est accessible dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="4058c-139">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Composant de boîte de dialogue affiché dans le navigateur](index/_static/dialog.png)

<span data-ttu-id="4058c-141">Quand ce composant est utilisé dans l’application, IntelliSense dans [Visual Studio](/visualstudio/ide/using-intellisense)et [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) accélère le développement avec l’achèvement de la syntaxe et des paramètres.</span><span class="sxs-lookup"><span data-stu-id="4058c-141">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="4058c-142">Les composants s’affichent dans une représentation en mémoire du DOM (Document Object Model) du navigateur appelé *arborescence de rendu*, utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="4058c-142">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="4058c-143">WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="4058c-143">Blazor WebAssembly</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="4058c-144">Le webassembly éblouissant est une infrastructure d’application à page unique pour la création d’applications Web interactives côté client avec .NET.</span><span class="sxs-lookup"><span data-stu-id="4058c-144">Blazor WebAssembly is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="4058c-145">Le webassembly éblouissant utilise des normes Web ouvertes sans plug-ins ou code transpilation et fonctionne dans tous les navigateurs Web modernes, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="4058c-145">Blazor WebAssembly uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="4058c-146">L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](https://webassembly.org) (abrégé en *wasm*).</span><span class="sxs-lookup"><span data-stu-id="4058c-146">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="4058c-147">WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale.</span><span class="sxs-lookup"><span data-stu-id="4058c-147">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="4058c-148">WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in.</span><span class="sxs-lookup"><span data-stu-id="4058c-148">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="4058c-149">Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur via JavaScript, appelé *interopérabilité JavaScript* (ou *interop JavaScript*).</span><span class="sxs-lookup"><span data-stu-id="4058c-149">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="4058c-150">Le code .NET exécuté via WebAssembly dans le navigateur s’exécute dans le bac à sable JavaScript du navigateur avec les protections offertes par le bac à sable contre les actions malveillantes sur l’ordinateur client.</span><span class="sxs-lookup"><span data-stu-id="4058c-150">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

![Le webassembly éblouissant exécute du code .NET dans le navigateur avec webassembly.](index/_static/blazor-webassembly.png)

<span data-ttu-id="4058c-152">Quand une application de webassembly éblouissant est générée et exécutée dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="4058c-152">When a Blazor WebAssembly app is built and run in a browser:</span></span>

* <span data-ttu-id="4058c-153">Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.</span><span class="sxs-lookup"><span data-stu-id="4058c-153">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="4058c-154">Les assemblys et le runtime .NET sont téléchargés dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="4058c-154">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="4058c-155">Le webassembly éblouissant amorce le Runtime .NET et configure le runtime pour charger les assemblys de l’application.</span><span class="sxs-lookup"><span data-stu-id="4058c-155">Blazor WebAssembly bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="4058c-156">Le runtime webassembly éblouissant utilise l’interopérabilité JavaScript pour gérer les appels de l’API de manipulation et du navigateur DOM.</span><span class="sxs-lookup"><span data-stu-id="4058c-156">The Blazor WebAssembly runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="4058c-157">La taille de l’application publiée, sa *taille de charge utile*, est un facteur de performance essentiel pour qu’une application soit facile à utiliser.</span><span class="sxs-lookup"><span data-stu-id="4058c-157">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="4058c-158">Le téléchargement d’une application volumineuse dans un navigateur prend un certain temps, ce qui nuit à l’expérience utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4058c-158">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> <span data-ttu-id="4058c-159">Le webassembly éblouissant optimise la taille de la charge utile pour réduire les temps de téléchargement :</span><span class="sxs-lookup"><span data-stu-id="4058c-159">Blazor WebAssembly optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="4058c-160">Du code inutilisé est extrait de l’application lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="4058c-160">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="4058c-161">Réponses HTTP compressées.</span><span class="sxs-lookup"><span data-stu-id="4058c-161">HTTP responses are compressed.</span></span>
* <span data-ttu-id="4058c-162">Le runtime .NET et les assemblys sont mis en cache dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="4058c-162">The .NET runtime and assemblies are cached in the browser.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="4058c-163">Serveur Blazor</span><span class="sxs-lookup"><span data-stu-id="4058c-163">Blazor Server</span></span>

<span data-ttu-id="4058c-164">Blazor dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="4058c-164">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="4058c-165">Le serveur éblouissant fournit la prise en charge de l’hébergement des composants Razor sur le serveur dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4058c-165">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="4058c-166">Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="4058c-166">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="4058c-167">Le runtime gère l’envoi des événements d’interface utilisateur du navigateur au serveur et applique les mises à jour de l’interface utilisateur renvoyées par le serveur dans le navigateur après avoir exécuté les composants.</span><span class="sxs-lookup"><span data-stu-id="4058c-167">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="4058c-168">La connexion utilisée par le serveur éblouissant pour communiquer avec le navigateur est également utilisée pour gérer les appels Interop JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4058c-168">The connection used by Blazor Server to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Le serveur éblouissant exécute du code .NET sur le serveur et interagit avec le Document Object Model sur le client via une connexion Signalr.](index/_static/blazor-server.png)

## <a name="javascript-interop"></a><span data-ttu-id="4058c-170">Interopérabilité de JavaScript</span><span class="sxs-lookup"><span data-stu-id="4058c-170">JavaScript interop</span></span>

<span data-ttu-id="4058c-171">Pour les applications qui nécessitent des bibliothèques JavaScript tierces et l’accès à des API de navigateur, les composants interagissent avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4058c-171">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="4058c-172">Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4058c-172">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="4058c-173">Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#.</span><span class="sxs-lookup"><span data-stu-id="4058c-173">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="4058c-174">Pour plus d'informations, consultez <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="4058c-174">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="4058c-175">Partage de code et .NET Standard</span><span class="sxs-lookup"><span data-stu-id="4058c-175">Code sharing and .NET Standard</span></span>

<span data-ttu-id="4058c-176">Blazor implémente également [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="4058c-176">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="4058c-177">.NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET.</span><span class="sxs-lookup"><span data-stu-id="4058c-177">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="4058c-178">Vous pouvez partager les bibliothèques de classes .NET Standard entre les différentes plateformes .NET, comme Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.</span><span class="sxs-lookup"><span data-stu-id="4058c-178">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="4058c-179">Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket et le threading) lèvent une <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="4058c-179">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4058c-180">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4058c-180">Additional resources</span></span>

* [<span data-ttu-id="4058c-181">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="4058c-181">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [<span data-ttu-id="4058c-182">Guide C#</span><span class="sxs-lookup"><span data-stu-id="4058c-182">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="4058c-183">HTML</span><span class="sxs-lookup"><span data-stu-id="4058c-183">HTML</span></span>](https://www.w3.org/html/)
