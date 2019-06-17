---
title: Introduction à Blazor dans ASP.NET Core
author: guardrex
description: Explorez ASP.NET Core Blazor, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034426"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="8ecc3-103">Introduction à Blazor</span><span class="sxs-lookup"><span data-stu-id="8ecc3-103">Introduction to Blazor</span></span>

<span data-ttu-id="8ecc3-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8ecc3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8ecc3-105">*Bienvenue dans Blazor !*</span><span class="sxs-lookup"><span data-stu-id="8ecc3-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="8ecc3-106">Blazor est une infrastructure pour la création d’interfaces utilisateur d’applications web interactives côté client avec .NET :</span><span class="sxs-lookup"><span data-stu-id="8ecc3-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="8ecc3-107">Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="8ecc3-108">Partagez la logique d’application côté serveur et côté client écrite avec .NET.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="8ecc3-109">Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="8ecc3-110">L’utilisation de .NET dans le développement web côté client offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="8ecc3-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="8ecc3-111">Écriture de code dans C# plutôt que dans JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="8ecc3-112">Tirez parti de l’écosystème .NET existant des bibliothèques .NET.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="8ecc3-113">Partagez la logique de l’application sur le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-113">Share app logic across the server and client.</span></span>
* <span data-ttu-id="8ecc3-114">Bénéficiez des performances, de la fiabilité et de la sécurité de .NET.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="8ecc3-115">Restez productif grâce à Visual Studio sur Windows, Linux et macOS.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="8ecc3-116">Développez avec un ensemble commun de langages, de frameworks et d’outils stables, riches en fonctionnalités et faciles à utiliser.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="8ecc3-117">Composants</span><span class="sxs-lookup"><span data-stu-id="8ecc3-117">Components</span></span>

<span data-ttu-id="8ecc3-118">Les applications Blazor sont basées sur des *composants*.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="8ecc3-119">Dans Blazor, un composant est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="8ecc3-120">Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-120">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="8ecc3-121">Les composants peuvent être imbriqués et réutilisés.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-121">Components can be nested and reused.</span></span>

<span data-ttu-id="8ecc3-122">Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme [packages NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="8ecc3-122">Components are .NET classes built into .NET assemblies that can be shared and distributed as [NuGet packages](/nuget/what-is-nuget).</span></span> <span data-ttu-id="8ecc3-123">La classe de composant est généralement écrite sous la forme d’une page de balisage Razor avec une extension de fichier *.razor*.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-123">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="8ecc3-124">Les composants dans Blazor sont parfois appelés *composants Razor*.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-124">Components in Blazor are sometimes referred to as *Razor components*.</span></span> <span data-ttu-id="8ecc3-125">[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C# conçu pour la productivité des développeurs.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-125">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="8ecc3-126">Razor permet de basculer entre le balisage HTML et C# dans le même fichier avec le support d’[IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="8ecc3-126">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="8ecc3-127">Razor Pages et MVC utilisent également Razor.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-127">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="8ecc3-128">Contrairement à Razor Pages et à MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour la logique et la composition de l’interface utilisateur côté client.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-128">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="8ecc3-129">Le balisage Razor suivant montre un composant (*Dialog.razor*), qui peut être imbriqué dans un autre composant :</span><span class="sxs-lookup"><span data-stu-id="8ecc3-129">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

<span data-ttu-id="8ecc3-130">Le contenu du corps (`ChildContent`) et le titre (`Title`) de la boîte de dialogue sont fournis par le composant qui utilise ce composant dans son interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-130">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="8ecc3-131">`OnYes` est une méthode C# déclenchée par l’événement `onclick` du bouton.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-131">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="8ecc3-132">Blazor utilise des balises HTML naturelles pour la composition de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-132">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="8ecc3-133">Les éléments HTML spécifient des composants et les attributs d’une balise passent les valeurs aux propriétés d’un composant.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-133">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span> <span data-ttu-id="8ecc3-134">`ChildContent` et `Title` sont définis par le composant qui utilise le composant de la boîte de dialogue (*Index.razor*) :</span><span class="sxs-lookup"><span data-stu-id="8ecc3-134">`ChildContent` and `Title` are set by the component that uses the Dialog component (*Index.razor*):</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="8ecc3-135">La boîte de dialogue est affichée lorsque le parent (*Index.razor*) est accessible dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="8ecc3-135">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Composant de boîte de dialogue affiché dans le navigateur](index/_static/dialog.png)

<span data-ttu-id="8ecc3-137">Quand ce composant est utilisé dans l’application, IntelliSense dans [Visual Studio](/visualstudio/ide/using-intellisense)et [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) accélère le développement avec l’achèvement de la syntaxe et des paramètres.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-137">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="8ecc3-138">Les composants s’affichent dans une représentation en mémoire du navigateur DOM appelée *arborescence d’affichage*. Elle est ensuite utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-138">Components render into an in-memory representation of the browser DOM called a *render tree* that's used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="8ecc3-139">Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="8ecc3-139">Blazor client-side</span></span>

<span data-ttu-id="8ecc3-140">Blazor côté client est un framework d’application monopage pour la création d’applications web interactives côté client avec .NET.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-140">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="8ecc3-141">Blazor côté client utilise les normes web ouvertes sans transpilation de plug-ins ou de codes et fonctionne dans tous les navigateurs web modernes, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-141">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="8ecc3-142">L’exécution de code .NET à l’intérieur des navigateurs web est rendue possible grâce à [WebAssembly](http://webassembly.org) (abrégé en *wasm*).</span><span class="sxs-lookup"><span data-stu-id="8ecc3-142">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="8ecc3-143">WebAssembly est un standard web ouvert pris en charge dans les navigateurs web sans plug-in.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-143">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="8ecc3-144">WebAssembly est un format bytecode compact optimisé pour un téléchargement rapide et une vitesse d’exécution maximale.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-144">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="8ecc3-145">Le code WebAssembly peut accéder à toutes les fonctionnalités du navigateur via JavaScript, appelé *interopérabilité JavaScript* (ou *interop JavaScript*).</span><span class="sxs-lookup"><span data-stu-id="8ecc3-145">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="8ecc3-146">Le code .NET exécutée via WebAssembly dans le navigateur s’exécute dans le même bac à sable approuvé que JavaScript, ce qui élimine pratiquement la possibilité pour une application d’effectuer des actions malveillantes sur l’ordinateur client.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-146">.NET code executed via WebAssembly in the browser runs in the same trusted sandbox as JavaScript, which virtually eliminates the opportunity for an app to perform malicious actions on the client machine.</span></span>

![Blazor côté client exécute du code .NET dans le navigateur avec WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="8ecc3-148">Quand une application Blazor côté client est créée et exécutée dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="8ecc3-148">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="8ecc3-149">Les fichiers de code C# et les fichiers Razor sont compilés dans des assemblys .NET.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-149">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="8ecc3-150">Les assemblys et le runtime .NET sont téléchargés dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-150">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="8ecc3-151">Blazor côté client amorce le runtime .NET et configure le runtime pour charger les assemblys de l’application.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-151">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="8ecc3-152">Le runtime Blazor côté client utilise l’interopérabilité de JavaScript pour traiter la manipulation DOM (Document Object Model) et les appels d’API du navigateur.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-152">The Blazor client-side runtime uses JavaScript interop to handle Document Object Model (DOM) manipulation and browser API calls.</span></span>

<span data-ttu-id="8ecc3-153">La taille de l’application publiée, sa *taille de charge utile*, est un facteur de performance essentiel pour qu’une application soit facile à utiliser.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-153">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="8ecc3-154">Le téléchargement d’une application volumineuse dans un navigateur prend un certain temps, ce qui nuit à l’expérience utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-154">A large app takes a relatively long time to download to a browser, which hurts the user experience.</span></span> <span data-ttu-id="8ecc3-155">Blazor côté client optimise la taille de la charge utile pour réduire les temps de téléchargement :</span><span class="sxs-lookup"><span data-stu-id="8ecc3-155">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="8ecc3-156">Du code inutilisé est extrait de l’application lorsqu’elle est publiée par l’[éditeur de liens de langage intermédiaire (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="8ecc3-156">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="8ecc3-157">Réponses HTTP compressées.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-157">HTTP responses are compressed.</span></span>
* <span data-ttu-id="8ecc3-158">Le runtime .NET et les assemblys sont mis en cache dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-158">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="8ecc3-159">Pour plus d’informations et de conseils sur le choix d’un modèle d’hébergement, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-159">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="8ecc3-160">Blazor côté serveur</span><span class="sxs-lookup"><span data-stu-id="8ecc3-160">Blazor server-side</span></span>

<span data-ttu-id="8ecc3-161">Blazor dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-161">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="8ecc3-162">Blazor côté serveur prend en charge l’hébergement des composants Razor sur le serveur dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-162">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="8ecc3-163">Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="8ecc3-163">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="8ecc3-164">Le runtime gère l’envoi des événements d’interface utilisateur du navigateur au serveur et applique les mises à jour de l’interface utilisateur renvoyées par le serveur dans le navigateur après avoir exécuté les composants.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-164">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="8ecc3-165">La connexion utilisée par Blazor côté serveur pour communiquer avec le navigateur est aussi utilisée pour gérer les appels d’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-165">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor côté serveur exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR.](index/_static/blazor-server-side.png)

<span data-ttu-id="8ecc3-167">Pour plus d’informations et de conseils sur le choix d’un modèle d’hébergement, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-167">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="8ecc3-168">Interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="8ecc3-168">JavaScript interop</span></span>

<span data-ttu-id="8ecc3-169">Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, les composants interagissent avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-169">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="8ecc3-170">Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-170">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="8ecc3-171">Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="8ecc3-172">Pour plus d'informations, consultez <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-172">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="8ecc3-173">Partage de code et .NET Standard</span><span class="sxs-lookup"><span data-stu-id="8ecc3-173">Code sharing and .NET Standard</span></span>

<span data-ttu-id="8ecc3-174">Blazor implémente également [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="8ecc3-174">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="8ecc3-175">.NET Standard est une spécification formelle d’API .NET qui sont communes aux implémentations .NET.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-175">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="8ecc3-176">Vous pouvez partager les bibliothèques de classes .NET Standard entre les différentes plateformes .NET, comme Blazor, .NET Framework, .NET Core, Xamarin, Mono et Unity.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-176">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="8ecc3-177">Les API qui ne sont pas applicables à l’intérieur d’un navigateur web (par exemple l’accès au système de fichiers, l’ouverture d’un socket et le threading) lèvent une <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="8ecc3-177">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ecc3-178">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8ecc3-178">Additional resources</span></span>

* [<span data-ttu-id="8ecc3-179">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="8ecc3-179">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="8ecc3-180">Guide C#</span><span class="sxs-lookup"><span data-stu-id="8ecc3-180">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="8ecc3-181">HTML</span><span class="sxs-lookup"><span data-stu-id="8ecc3-181">HTML</span></span>](https://www.w3.org/html/)
