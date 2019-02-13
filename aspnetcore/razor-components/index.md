---
title: Introduction à Razor Components
author: guardrex
description: Explorez ASP.NET Core Razor Components, un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: razor-components/index
ms.openlocfilehash: 04a73d33cee0deedaf3dc97395836a936b580fbd
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159524"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="4aff3-103">Introduction à Razor Components</span><span class="sxs-lookup"><span data-stu-id="4aff3-103">Introduction to Razor Components</span></span>

<span data-ttu-id="4aff3-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4aff3-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4aff3-105">*Razor Components* constitue un moyen de créer une interface utilisateur web interactive côté client à l’aide de .NET :</span><span class="sxs-lookup"><span data-stu-id="4aff3-105">*Razor Components* is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="4aff3-106">Créez des interfaces utilisateur interactives riches à l’aide de C# plutôt que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4aff3-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="4aff3-107">Partagez les logiques d’applications côté serveur et côté client écrites avec .NET.</span><span class="sxs-lookup"><span data-stu-id="4aff3-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="4aff3-108">Affichez l’interface utilisateur en langage HTML et CSS pour une large prise en charge des navigateurs, y compris les navigateurs mobiles.</span><span class="sxs-lookup"><span data-stu-id="4aff3-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="4aff3-109">Razor Components prend en charge les principales installations que nécessitent la plupart des applications, y compris :</span><span class="sxs-lookup"><span data-stu-id="4aff3-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="4aff3-110">Paramètres</span><span class="sxs-lookup"><span data-stu-id="4aff3-110">Parameters</span></span>
* <span data-ttu-id="4aff3-111">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="4aff3-111">Event handling</span></span>
* <span data-ttu-id="4aff3-112">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="4aff3-112">Data binding</span></span>
* <span data-ttu-id="4aff3-113">Routage</span><span class="sxs-lookup"><span data-stu-id="4aff3-113">Routing</span></span>
* <span data-ttu-id="4aff3-114">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="4aff3-114">Dependency injection</span></span>
* <span data-ttu-id="4aff3-115">Dispositions</span><span class="sxs-lookup"><span data-stu-id="4aff3-115">Layouts</span></span>
* <span data-ttu-id="4aff3-116">Création de modèles</span><span class="sxs-lookup"><span data-stu-id="4aff3-116">Templating</span></span>
* <span data-ttu-id="4aff3-117">Valeurs en cascade</span><span class="sxs-lookup"><span data-stu-id="4aff3-117">Cascading values</span></span>

<span data-ttu-id="4aff3-118">Razor Components dissocie la logique de rendu de composant de la manière dont les mises à jour de l’interface utilisateur sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="4aff3-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="4aff3-119">ASP.NET Core Razor Components dans .NET Core 3.0 ajoute la prise en charge de l’hébergement de Razor Components sur le serveur dans une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4aff3-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="4aff3-120">Les mises à jour de l’interface utilisateur sont gérées par le biais d’une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="4aff3-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="4aff3-121">L’exécution :</span><span class="sxs-lookup"><span data-stu-id="4aff3-121">The runtime:</span></span>

* <span data-ttu-id="4aff3-122">Gère l’envoi des événements d’interface utilisateur depuis le navigateur vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="4aff3-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="4aff3-123">Applique les mises à jour de l’interface utilisateur envoyées par le serveur au navigateur après avoir exécuté les composants.</span><span class="sxs-lookup"><span data-stu-id="4aff3-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="4aff3-124">La connexion utilisée par Razor Components pour communiquer avec le navigateur est aussi utilisée pour gérer les appels d’interopérabilité JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4aff3-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components exécute le code .NET sur le serveur et interagit avec le modèle DOM (Document Object Model) sur le client par le biais d’une connexion SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="4aff3-126">Pour plus d'informations, consultez <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="4aff3-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="4aff3-127">*Blazor* est le modèle d’hébergement expérimental côté client de Razor Components.</span><span class="sxs-lookup"><span data-stu-id="4aff3-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="4aff3-128">Blazor s’exécute sur .NET dans le navigateur à l’aide de standards web ouverts, sans plug-in ni aucune transpilation de code.</span><span class="sxs-lookup"><span data-stu-id="4aff3-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="4aff3-129">Pour plus d'informations, consultez <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="4aff3-129">For more information, see <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="4aff3-130">Composants</span><span class="sxs-lookup"><span data-stu-id="4aff3-130">Components</span></span>

<span data-ttu-id="4aff3-131">Un *composant Razor* est un élément d’interface utilisateur, comme une page, une boîte de dialogue ou un formulaire de saisie de données.</span><span class="sxs-lookup"><span data-stu-id="4aff3-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="4aff3-132">Les composants gèrent les événements de l’utilisateur et définissent une logique de rendu de l’interface utilisateur flexible.</span><span class="sxs-lookup"><span data-stu-id="4aff3-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="4aff3-133">Les composants peuvent être imbriqués et réutilisés.</span><span class="sxs-lookup"><span data-stu-id="4aff3-133">Components can be nested and reused.</span></span>

<span data-ttu-id="4aff3-134">Les composants sont des classes .NET intégrées dans des assemblys .NET pouvant être partagées et distribuées comme packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="4aff3-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="4aff3-135">La classe peut être écrite sous la forme d’une page de balisage Razor (*.cshtml*), ou en tant que classe C# (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="4aff3-135">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="4aff3-136">[Razor](xref:mvc/views/razor) est une syntaxe pour la combinaison de balisage HTML et de code C#.</span><span class="sxs-lookup"><span data-stu-id="4aff3-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="4aff3-137">Razor accroît la productivité des développeurs en leur permettant de basculer entre le balisage et le code C# dans le même fichier avec prise en charge d’[IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="4aff3-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="4aff3-138">Razor Pages et les vues MVC utilisent également Razor.</span><span class="sxs-lookup"><span data-stu-id="4aff3-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="4aff3-139">Contrairement à Razor Pages et aux vues MVC, qui reposent sur un modèle requête/réponse, les composants sont utilisés spécifiquement pour gérer la composition de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4aff3-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="4aff3-140">Razor Components peut être utilisé spécifiquement pour la composition et la logique d’interface utilisateur côté client.</span><span class="sxs-lookup"><span data-stu-id="4aff3-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="4aff3-141">Le balisage suivant est un exemple de composant de boîte de dialogue personnalisée dans un fichier Razor (*DialogComponent.cshtml*) :</span><span class="sxs-lookup"><span data-stu-id="4aff3-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="4aff3-142">Quand ce composant est utilisé ailleurs dans l’application, IntelliSense accélère le développement avec la complétion de syntaxe et de paramètres.</span><span class="sxs-lookup"><span data-stu-id="4aff3-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="4aff3-143">Les composants s’affichent dans une représentation en mémoire du navigateur DOM appelé *arborescence d’affichage*. Elle peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="4aff3-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="4aff3-144">Interopérabilité JavaScript</span><span class="sxs-lookup"><span data-stu-id="4aff3-144">JavaScript interop</span></span>

<span data-ttu-id="4aff3-145">Pour les applications qui nécessitent des bibliothèques JavaScript tierces et des API de navigateur, les composants interagissent avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4aff3-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="4aff3-146">Les composants peuvent utiliser les mêmes API ou bibliothèques que JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4aff3-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="4aff3-147">Le code C# peut appeler du code JavaScript, et le code JavaScript peut appeler du code C#.</span><span class="sxs-lookup"><span data-stu-id="4aff3-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="4aff3-148">Pour plus d’informations, consultez [Interopérabilité JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="4aff3-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4aff3-149">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4aff3-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="4aff3-150">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="4aff3-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="4aff3-151">Guide C#</span><span class="sxs-lookup"><span data-stu-id="4aff3-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="4aff3-152">HTML</span><span class="sxs-lookup"><span data-stu-id="4aff3-152">HTML</span></span>](https://www.w3.org/html/)
