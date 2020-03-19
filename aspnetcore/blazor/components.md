---
title: Créer et utiliser des composants ASP.NET Core Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants Razor, notamment comment lier des données, gérer des événements et gérer des cycles de vie de composant.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: 7afc9250cdfb4b791ef939ead0f41b503d83fad8
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511273"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="72bea-103">Créer et utiliser des composants ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="72bea-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="72bea-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="72bea-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="72bea-105">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="72bea-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="72bea-106">les applications Blazor sont générées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="72bea-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="72bea-107">Un composant est un bloc d’interface utilisateur (IU) autonome, tel qu’une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="72bea-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="72bea-108">Un composant comprend le balisage HTML et la logique de traitement requise pour injecter des données ou répondre à des événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72bea-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="72bea-109">Les composants sont flexibles et légers.</span><span class="sxs-lookup"><span data-stu-id="72bea-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="72bea-110">Elles peuvent être imbriquées, réutilisées et partagées entre les projets.</span><span class="sxs-lookup"><span data-stu-id="72bea-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="72bea-111">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="72bea-111">Component classes</span></span>

<span data-ttu-id="72bea-112">Les composants sont implémentés dans les fichiers de composants [Razor](xref:mvc/views/razor) ( *. Razor*) C# à l’aide d’une combinaison de balises et html.</span><span class="sxs-lookup"><span data-stu-id="72bea-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="72bea-113">Un composant de Blazor est officiellement désigné sous le terme de *composant Razor*.</span><span class="sxs-lookup"><span data-stu-id="72bea-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="72bea-114">Le nom d’un composant doit commencer par un caractère majuscule.</span><span class="sxs-lookup"><span data-stu-id="72bea-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="72bea-115">Par exemple, *MyCoolComponent. Razor* est valide et *MyCoolComponent. Razor* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="72bea-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="72bea-116">L’interface utilisateur d’un composant est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="72bea-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="72bea-117">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="72bea-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="72bea-118">Quand une application est compilée, le balisage C# html et la logique de rendu sont convertis en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="72bea-119">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="72bea-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="72bea-120">Les membres de la classe de composants sont définis dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="72bea-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="72bea-121">Dans le bloc `@code`, l’état du composant (propriétés, champs) est spécifié avec les méthodes de gestion des événements ou pour la définition d’une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="72bea-122">Plus d’un bloc `@code` est autorisé.</span><span class="sxs-lookup"><span data-stu-id="72bea-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="72bea-123">Les membres de composant peuvent être utilisés dans le cadre de la logique de C# rendu du composant à l’aide d’expressions qui commencent par `@`.</span><span class="sxs-lookup"><span data-stu-id="72bea-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="72bea-124">Par exemple, un C# champ est rendu en préfixant `@` au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="72bea-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="72bea-125">L’exemple suivant évalue et affiche :</span><span class="sxs-lookup"><span data-stu-id="72bea-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="72bea-126">`_headingFontStyle` à la valeur de propriété CSS pour `font-style`.</span><span class="sxs-lookup"><span data-stu-id="72bea-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="72bea-127">`_headingText` au contenu de l’élément `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="72bea-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="72bea-128">Une fois le composant restitué initialement, le composant régénère son arborescence de rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="72bea-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="72bea-129"> compare ensuite la nouvelle arborescence de rendu à la précédente et applique toutes les modifications à l’Document Object Model du navigateur (DOM).</span><span class="sxs-lookup"><span data-stu-id="72bea-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="72bea-130">Les composants sont C# des classes ordinaires qui peuvent être placées n’importe où dans un projet.</span><span class="sxs-lookup"><span data-stu-id="72bea-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="72bea-131">Les composants qui produisent des pages Web résident généralement dans le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="72bea-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="72bea-132">Les composants qui ne sont pas des pages sont souvent placés dans le dossier *partagé* ou dans un dossier personnalisé ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="72bea-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="72bea-133">En règle générale, l’espace de noms d’un composant est dérivé de l’espace de noms racine de l’application et de l’emplacement (dossier) du composant dans l’application.</span><span class="sxs-lookup"><span data-stu-id="72bea-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="72bea-134">Si l’espace de noms racine de l’application est `BlazorApp` et que le composant `Counter` se trouve dans le dossier *pages* :</span><span class="sxs-lookup"><span data-stu-id="72bea-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="72bea-135">L’espace de noms du composant `Counter` est `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="72bea-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="72bea-136">Le nom de type qualifié complet du composant est `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="72bea-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="72bea-137">Pour plus d’informations, consultez la section [importer des composants](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="72bea-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="72bea-138">Pour utiliser un dossier personnalisé, ajoutez l’espace de noms du dossier personnalisé au composant parent ou au fichier *_Imports. Razor* de l’application.</span><span class="sxs-lookup"><span data-stu-id="72bea-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="72bea-139">Par exemple, l’espace de noms suivant rend les composants dans un dossier de *composants* disponibles lorsque l’espace de noms racine de l’application est `BlazorApp`:</span><span class="sxs-lookup"><span data-stu-id="72bea-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a><span data-ttu-id="72bea-140">Les ressources statiques</span><span class="sxs-lookup"><span data-stu-id="72bea-140">Static assets</span></span>

Blazor<span data-ttu-id="72bea-141"> suit la Convention de ASP.NET Core des applications qui placent des ressources statiques dans le [dossier racine Web (Wwwroot)](xref:fundamentals/index#web-root)du projet.</span><span class="sxs-lookup"><span data-stu-id="72bea-141"> follows the convention of ASP.NET Core apps placing static assets under the project's [web root (wwwroot) folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="72bea-142">Utilisez un chemin d’accès relatif à la base (`/`) pour faire référence à la racine Web d’une ressource statique.</span><span class="sxs-lookup"><span data-stu-id="72bea-142">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="72bea-143">Dans l’exemple suivant, *logo. png* se trouve physiquement dans le dossier *{Project root}/wwwroot/images* :</span><span class="sxs-lookup"><span data-stu-id="72bea-143">In the following example, *logo.png* is physically located in the *{PROJECT ROOT}/wwwroot/images* folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

<span data-ttu-id="72bea-144">Les composants Razor ne prennent **pas** en charge la notation tilde-slash (`~/`).</span><span class="sxs-lookup"><span data-stu-id="72bea-144">Razor components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="72bea-145">Pour plus d’informations sur la définition du chemin d’accès de base d’une application, consultez <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="72bea-145">For information on setting an app's base path, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="72bea-146">Les balises tag Helper ne sont pas prises en charge dans les composants</span><span class="sxs-lookup"><span data-stu-id="72bea-146">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="72bea-147">Les [tag helpers](xref:mvc/views/tag-helpers/intro) ne sont pas pris en charge dans les composants Razor (fichiers *. Razor* ).</span><span class="sxs-lookup"><span data-stu-id="72bea-147">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="72bea-148">Pour fournir des fonctionnalités semblables à tag Helper dans Blazor, créez un composant avec les mêmes fonctionnalités que le tag Helper et utilisez le composant à la place.</span><span class="sxs-lookup"><span data-stu-id="72bea-148">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="72bea-149">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="72bea-149">Use components</span></span>

<span data-ttu-id="72bea-150">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="72bea-150">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="72bea-151">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-151">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="72bea-152">Le balisage suivant dans *index. Razor* rend une instance de `HeadingComponent` :</span><span class="sxs-lookup"><span data-stu-id="72bea-152">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="72bea-153">*Composants/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-153">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="72bea-154">Si un composant contient un élément HTML avec une première lettre majuscule qui ne correspond pas à un nom de composant, un avertissement est émis pour indiquer que l’élément a un nom inattendu.</span><span class="sxs-lookup"><span data-stu-id="72bea-154">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="72bea-155">L’ajout d’une directive `@using` pour l’espace de noms du composant rend le composant disponible, ce qui résout l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="72bea-155">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="72bea-156">Routage</span><span class="sxs-lookup"><span data-stu-id="72bea-156">Routing</span></span>

<span data-ttu-id="72bea-157">Le routage dans Blazor est obtenu en fournissant un modèle de routage à chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="72bea-157">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="72bea-158">Lorsqu’un fichier Razor avec une directive `@page` est compilé, la classe générée reçoit une <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="72bea-158">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="72bea-159">Lors de l’exécution, le routeur recherche les classes de composant avec un `RouteAttribute` et rend le composant qui a un modèle de routage correspondant à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="72bea-159">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="72bea-160">Pour plus d’informations, consultez <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="72bea-160">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="72bea-161">Paramètres</span><span class="sxs-lookup"><span data-stu-id="72bea-161">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="72bea-162">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="72bea-162">Route parameters</span></span>

<span data-ttu-id="72bea-163">Les composants peuvent recevoir des paramètres de routage à partir du modèle de routage fourni dans la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="72bea-163">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="72bea-164">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="72bea-164">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="72bea-165">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-165">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="72bea-166">Les paramètres facultatifs ne sont pas pris en charge. deux directives `@page` sont donc appliquées dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="72bea-166">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="72bea-167">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="72bea-167">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="72bea-168">La deuxième `@page` directive reçoit le paramètre de routage `{text}` et assigne la valeur à la propriété `Text`.</span><span class="sxs-lookup"><span data-stu-id="72bea-168">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="72bea-169">La syntaxe de paramètre *catch-all* (`*`/`**`), qui capture le chemin d’accès dans plusieurs limites de dossiers, n’est **pas** prise en charge dans les composants Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="72bea-169">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="72bea-170">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="72bea-170">Component parameters</span></span>

<span data-ttu-id="72bea-171">Les composants peuvent avoir des *paramètres de composant*, qui sont définis à l’aide de propriétés publiques sur la classe de composant avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="72bea-171">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="72bea-172">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="72bea-172">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="72bea-173">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-173">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="72bea-174">Dans l’exemple suivant tiré de l’exemple d’application, le `ParentComponent` définit la valeur de la propriété `Title` du `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="72bea-174">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="72bea-175">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-175">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="72bea-176">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="72bea-176">Child content</span></span>

<span data-ttu-id="72bea-177">Les composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-177">Components can set the content of another component.</span></span> <span data-ttu-id="72bea-178">Le composant d’affectation fournit le contenu entre les balises qui spécifient le composant récepteur.</span><span class="sxs-lookup"><span data-stu-id="72bea-178">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="72bea-179">Dans l’exemple suivant, la `ChildComponent` a une propriété `ChildContent` qui représente un `RenderFragment`, qui représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="72bea-179">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="72bea-180">La valeur de `ChildContent` est positionnée dans le balisage du composant où le contenu doit être rendu.</span><span class="sxs-lookup"><span data-stu-id="72bea-180">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="72bea-181">La valeur de `ChildContent` est reçue du composant parent et affichée à l’intérieur du `panel-body`du panneau de démarrage.</span><span class="sxs-lookup"><span data-stu-id="72bea-181">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="72bea-182">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="72bea-183">La propriété qui reçoit le contenu du `RenderFragment` doit être nommée `ChildContent` par Convention.</span><span class="sxs-lookup"><span data-stu-id="72bea-183">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="72bea-184">Les `ParentComponent` dans l’exemple d’application peuvent fournir du contenu pour le rendu du `ChildComponent` en plaçant le contenu à l’intérieur des balises de `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="72bea-184">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="72bea-185">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-185">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="72bea-186">Réprojection d’attribut et paramètres arbitraires</span><span class="sxs-lookup"><span data-stu-id="72bea-186">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="72bea-187">Les composants peuvent capturer et restituer des attributs supplémentaires en plus des paramètres déclarés du composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-187">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="72bea-188">Des attributs supplémentaires peuvent être capturés dans un dictionnaire *, puis* réintégrés à un élément lorsque le composant est rendu à l’aide de la directive [`@attributes`](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="72bea-188">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="72bea-189">Ce scénario est utile lors de la définition d’un composant qui produit un élément de balisage qui prend en charge diverses personnalisations.</span><span class="sxs-lookup"><span data-stu-id="72bea-189">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="72bea-190">Par exemple, il peut être fastidieux de définir des attributs séparément pour un `<input>` qui prend en charge de nombreux paramètres.</span><span class="sxs-lookup"><span data-stu-id="72bea-190">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="72bea-191">Dans l’exemple suivant, le premier élément `<input>` (`id="useIndividualParams"`) utilise des paramètres de composant individuels, tandis que le deuxième élément `<input>` (`id="useAttributesDict"`) utilise la projection d’attributs :</span><span class="sxs-lookup"><span data-stu-id="72bea-191">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="72bea-192">Le type du paramètre doit implémenter `IEnumerable<KeyValuePair<string, object>>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="72bea-192">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="72bea-193">L’utilisation de `IReadOnlyDictionary<string, object>` est également une option dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="72bea-193">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="72bea-194">Les éléments `<input>` rendus à l’aide des deux approches sont identiques :</span><span class="sxs-lookup"><span data-stu-id="72bea-194">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="72bea-195">Pour accepter des attributs arbitraires, définissez un paramètre de composant à l’aide de l’attribut `[Parameter]` avec la propriété `CaptureUnmatchedValues` définie sur `true`:</span><span class="sxs-lookup"><span data-stu-id="72bea-195">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="72bea-196">La propriété `CaptureUnmatchedValues` sur `[Parameter]` permet au paramètre de correspondre à tous les attributs qui ne correspondent à aucun autre paramètre.</span><span class="sxs-lookup"><span data-stu-id="72bea-196">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="72bea-197">Un composant ne peut définir qu’un seul paramètre avec `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="72bea-197">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="72bea-198">Le type de propriété utilisé avec `CaptureUnmatchedValues` doit pouvoir être assigné à partir de `Dictionary<string, object>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="72bea-198">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="72bea-199">`IEnumerable<KeyValuePair<string, object>>` ou `IReadOnlyDictionary<string, object>` sont également des options dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="72bea-199">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="72bea-200">La position des `@attributes` par rapport à la position des attributs d’élément est importante.</span><span class="sxs-lookup"><span data-stu-id="72bea-200">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="72bea-201">Lorsque `@attributes` sont réparties sur l’élément, les attributs sont traités de droite à gauche (dernier à premier).</span><span class="sxs-lookup"><span data-stu-id="72bea-201">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="72bea-202">Prenons l’exemple suivant d’un composant qui consomme un composant `Child` :</span><span class="sxs-lookup"><span data-stu-id="72bea-202">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="72bea-203">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-203">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="72bea-204">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-204">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="72bea-205">L’attribut `extra` du composant `Child` est défini à droite de `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="72bea-205">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="72bea-206">Le `<div>` rendu du composant `Parent` contient `extra="5"` lorsqu’il est transmis via l’attribut supplémentaire, car les attributs sont traités de droite à gauche (dernier à premier) :</span><span class="sxs-lookup"><span data-stu-id="72bea-206">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="72bea-207">Dans l’exemple suivant, l’ordre des `extra` et `@attributes` est inversé dans le `<div>`du composant `Child` :</span><span class="sxs-lookup"><span data-stu-id="72bea-207">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="72bea-208">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-208">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="72bea-209">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-209">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="72bea-210">Le `<div>` rendu dans le composant `Parent` contient `extra="10"` lorsqu’il est passé par le biais de l’attribut supplémentaire :</span><span class="sxs-lookup"><span data-stu-id="72bea-210">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="72bea-211">Capturer des références à des composants</span><span class="sxs-lookup"><span data-stu-id="72bea-211">Capture references to components</span></span>

<span data-ttu-id="72bea-212">Les références de composant offrent un moyen de référencer une instance de composant afin que vous puissiez émettre des commandes vers cette instance, telles que `Show` ou `Reset`.</span><span class="sxs-lookup"><span data-stu-id="72bea-212">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="72bea-213">Pour capturer une référence de composant :</span><span class="sxs-lookup"><span data-stu-id="72bea-213">To capture a component reference:</span></span>

* <span data-ttu-id="72bea-214">Ajoutez un attribut [`@ref`](xref:mvc/views/razor#ref) au composant enfant.</span><span class="sxs-lookup"><span data-stu-id="72bea-214">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="72bea-215">Définissez un champ avec le même type que le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="72bea-215">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="72bea-216">Lors du rendu du composant, le champ `_loginDialog` est rempli avec l’instance du composant enfant `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="72bea-216">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="72bea-217">Vous pouvez ensuite appeler des méthodes .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-217">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="72bea-218">La variable `_loginDialog` n’est remplie qu’après le rendu du composant et sa sortie comprend l’élément `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="72bea-218">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="72bea-219">Jusqu’à ce stade, il n’y a rien à référencer.</span><span class="sxs-lookup"><span data-stu-id="72bea-219">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="72bea-220">Pour manipuler des références de composants après la fin du rendu du composant, utilisez les [méthodes OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="72bea-220">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="72bea-221">Bien que la capture de références de composant utilise une syntaxe similaire pour [capturer des références d’élément](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), il ne s’agit pas d’une fonctionnalité JavaScript Interop.</span><span class="sxs-lookup"><span data-stu-id="72bea-221">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="72bea-222">Les références de composants ne sont pas transmises au code JavaScript&mdash;elles sont utilisées uniquement dans le code .NET.</span><span class="sxs-lookup"><span data-stu-id="72bea-222">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="72bea-223">N’utilisez **pas** de références de composant pour muter l’état des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="72bea-223">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="72bea-224">Utilisez plutôt des paramètres déclaratifs normaux pour passer des données aux composants enfants.</span><span class="sxs-lookup"><span data-stu-id="72bea-224">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="72bea-225">L’utilisation de paramètres déclaratifs normaux entraîne le rerendu automatique des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="72bea-225">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="72bea-226">Appeler des méthodes de composant en externe pour mettre à jour l’État</span><span class="sxs-lookup"><span data-stu-id="72bea-226">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="72bea-227"> utilise un `SynchronizationContext` pour appliquer un seul thread logique d’exécution.</span><span class="sxs-lookup"><span data-stu-id="72bea-227"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="72bea-228">Les méthodes de [cycle de vie](xref:blazor/lifecycle) d’un composant et les rappels d’événements déclenchés par Blazor sont exécutés sur ce `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="72bea-228">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="72bea-229">Dans le cas où un composant doit être mis à jour en fonction d’un événement externe, tel qu’un minuteur ou d’autres notifications, utilisez la méthode `InvokeAsync`, qui sera réexpédiée à la `SynchronizationContext`du Blazor.</span><span class="sxs-lookup"><span data-stu-id="72bea-229">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="72bea-230">Par exemple, considérez un *service de notification* qui peut notifier n’importe quel composant d’écoute de l’État mis à jour :</span><span class="sxs-lookup"><span data-stu-id="72bea-230">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="72bea-231">Inscrivez le `NotifierService` en tant que singletion :</span><span class="sxs-lookup"><span data-stu-id="72bea-231">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="72bea-232">Dans Blazor webassembly, inscrivez le service dans `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="72bea-232">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="72bea-233">Dans Blazor Server, inscrivez le service dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72bea-233">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="72bea-234">Utilisez la `NotifierService` pour mettre à jour un composant :</span><span class="sxs-lookup"><span data-stu-id="72bea-234">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="72bea-235">Dans l’exemple précédent, `NotifierService` appelle la méthode `OnNotify` du composant en dehors du `SynchronizationContext`de Blazor.</span><span class="sxs-lookup"><span data-stu-id="72bea-235">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="72bea-236">`InvokeAsync` est utilisé pour basculer vers le contexte correct et pour la mise en file d’attente d’un rendu.</span><span class="sxs-lookup"><span data-stu-id="72bea-236">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="72bea-237">Utiliser \@clé pour contrôler la conservation des éléments et des composants</span><span class="sxs-lookup"><span data-stu-id="72bea-237">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="72bea-238">Lors du rendu d’une liste d’éléments ou de composants, et que les éléments ou composants changent par la suite, l’algorithme de différenciation de Blazordoit déterminer lequel des éléments ou composants précédents peuvent être conservés et comment les objets de modèle doivent être mappés à eux.</span><span class="sxs-lookup"><span data-stu-id="72bea-238">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="72bea-239">Normalement, ce processus est automatique et peut être ignoré, mais dans certains cas, il peut être utile de contrôler le processus.</span><span class="sxs-lookup"><span data-stu-id="72bea-239">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="72bea-240">Prenons l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="72bea-240">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="72bea-241">Le contenu de la collection `People` peut changer avec des entrées insérées, supprimées ou réorganisées.</span><span class="sxs-lookup"><span data-stu-id="72bea-241">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="72bea-242">Lors du rerendu du composant, le composant `<DetailsEditor>` peut changer pour recevoir différentes valeurs de paramètres de `Details`.</span><span class="sxs-lookup"><span data-stu-id="72bea-242">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="72bea-243">Cela peut entraîner un rerendu plus complexe que prévu.</span><span class="sxs-lookup"><span data-stu-id="72bea-243">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="72bea-244">Dans certains cas, le rerendu peut entraîner des différences de comportement visibles, telles que le focus de l’élément perdu.</span><span class="sxs-lookup"><span data-stu-id="72bea-244">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="72bea-245">Le processus de mappage peut être contrôlé à l’aide de l’attribut de directive `@key`.</span><span class="sxs-lookup"><span data-stu-id="72bea-245">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="72bea-246">`@key` oblige l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé :</span><span class="sxs-lookup"><span data-stu-id="72bea-246">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="72bea-247">En cas de modification de la collection `People`, l’algorithme de comparaison conserve l’association entre les instances de `<DetailsEditor>` et les instances `person` :</span><span class="sxs-lookup"><span data-stu-id="72bea-247">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="72bea-248">Si un `Person` est supprimé de la liste `People`, seule l’instance `<DetailsEditor>` correspondante est supprimée de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72bea-248">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="72bea-249">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="72bea-249">Other instances are left unchanged.</span></span>
* <span data-ttu-id="72bea-250">Si une `Person` est insérée à une position dans la liste, une nouvelle instance de `<DetailsEditor>` est insérée à cette position correspondante.</span><span class="sxs-lookup"><span data-stu-id="72bea-250">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="72bea-251">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="72bea-251">Other instances are left unchanged.</span></span>
* <span data-ttu-id="72bea-252">Si `Person` entrées sont réordonnées, les instances de `<DetailsEditor>` correspondantes sont conservées et réordonnées dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72bea-252">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="72bea-253">Dans certains scénarios, l’utilisation de `@key` réduit la complexité du rerendu et évite les problèmes potentiels liés aux parties avec état de la modification du modèle DOM, telles que la position du focus.</span><span class="sxs-lookup"><span data-stu-id="72bea-253">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="72bea-254">Les clés sont locales pour chaque élément ou composant conteneur.</span><span class="sxs-lookup"><span data-stu-id="72bea-254">Keys are local to each container element or component.</span></span> <span data-ttu-id="72bea-255">Les clés ne sont pas comparées globalement dans le document.</span><span class="sxs-lookup"><span data-stu-id="72bea-255">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="72bea-256">Quand utiliser \@clé</span><span class="sxs-lookup"><span data-stu-id="72bea-256">When to use \@key</span></span>

<span data-ttu-id="72bea-257">En général, il est logique d’utiliser `@key` chaque fois qu’une liste est rendue (par exemple, dans un bloc `@foreach`) et qu’une valeur appropriée existe pour définir le `@key`.</span><span class="sxs-lookup"><span data-stu-id="72bea-257">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="72bea-258">Vous pouvez également utiliser `@key` pour empêcher Blazor de conserver un élément ou une sous-arborescence de composant quand un objet change :</span><span class="sxs-lookup"><span data-stu-id="72bea-258">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="72bea-259">Si `@currentPerson` change, la directive d’attribut `@key` force Blazor à ignorer l’intégralité du `<div>` et ses descendants, et à reconstruire la sous-arborescence de l’interface utilisateur avec de nouveaux éléments et composants.</span><span class="sxs-lookup"><span data-stu-id="72bea-259">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="72bea-260">Cela peut être utile si vous avez besoin de garantir qu’aucun État d’interface utilisateur n’est préservé lorsque `@currentPerson` change.</span><span class="sxs-lookup"><span data-stu-id="72bea-260">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="72bea-261">Quand ne pas utiliser la clé de \@</span><span class="sxs-lookup"><span data-stu-id="72bea-261">When not to use \@key</span></span>

<span data-ttu-id="72bea-262">Il y a un coût en matière de performances lors de la comparaison avec `@key`.</span><span class="sxs-lookup"><span data-stu-id="72bea-262">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="72bea-263">Le coût des performances n’est pas important, mais spécifiez uniquement `@key` si les règles de conservation des éléments ou des composants bénéficient de l’application.</span><span class="sxs-lookup"><span data-stu-id="72bea-263">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="72bea-264">Même si `@key` n’est pas utilisé, Blazor conserve autant que possible les instances d’élément et de composant enfants.</span><span class="sxs-lookup"><span data-stu-id="72bea-264">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="72bea-265">Le seul avantage de l’utilisation de `@key` est de contrôler la *façon dont* les instances de modèle sont mappées aux instances de composant conservées, au lieu de l’algorithme de comparaison qui sélectionne le mappage.</span><span class="sxs-lookup"><span data-stu-id="72bea-265">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="72bea-266">Valeurs à utiliser pour \@clé</span><span class="sxs-lookup"><span data-stu-id="72bea-266">What values to use for \@key</span></span>

<span data-ttu-id="72bea-267">En règle générale, il est logique de fournir l’un des types de valeur suivants pour `@key`:</span><span class="sxs-lookup"><span data-stu-id="72bea-267">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="72bea-268">Instances d’objet de modèle (par exemple, une instance de `Person` comme dans l’exemple précédent).</span><span class="sxs-lookup"><span data-stu-id="72bea-268">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="72bea-269">Cela garantit la préservation en fonction de l’égalité des références d’objet.</span><span class="sxs-lookup"><span data-stu-id="72bea-269">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="72bea-270">Identificateurs uniques (par exemple, les valeurs de clé primaire de type `int`, `string`ou `Guid`).</span><span class="sxs-lookup"><span data-stu-id="72bea-270">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="72bea-271">Assurez-vous que les valeurs utilisées pour `@key` ne sont pas en conflit.</span><span class="sxs-lookup"><span data-stu-id="72bea-271">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="72bea-272">Si les valeurs en conflit sont détectées dans le même élément parent, Blazor lève une exception, car il ne peut pas mapper de manière déterministe les anciens éléments ou composants aux nouveaux éléments ou composants.</span><span class="sxs-lookup"><span data-stu-id="72bea-272">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="72bea-273">Utilisez uniquement des valeurs distinctes, telles que des instances d’objets ou des valeurs de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="72bea-273">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="72bea-274">Prise en charge des classes partielles</span><span class="sxs-lookup"><span data-stu-id="72bea-274">Partial class support</span></span>

<span data-ttu-id="72bea-275">Les composants Razor sont générés en tant que classes partielles.</span><span class="sxs-lookup"><span data-stu-id="72bea-275">Razor components are generated as partial classes.</span></span> <span data-ttu-id="72bea-276">Les composants Razor sont créés à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="72bea-276">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="72bea-277">C#le code est défini dans un bloc [`@code`](xref:mvc/views/razor#code) avec le balisage HTML et le code Razor dans un fichier unique.</span><span class="sxs-lookup"><span data-stu-id="72bea-277">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="72bea-278">les modèles de Blazor définissent leurs composants Razor à l’aide de cette approche.</span><span class="sxs-lookup"><span data-stu-id="72bea-278">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="72bea-279">C#le code est placé dans un fichier code-behind défini en tant que classe partielle.</span><span class="sxs-lookup"><span data-stu-id="72bea-279">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="72bea-280">L’exemple suivant montre le composant `Counter` par défaut avec un bloc `@code` dans une application générée à partir d’un modèle Blazor.</span><span class="sxs-lookup"><span data-stu-id="72bea-280">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="72bea-281">Le balisage HTML, le code C# Razor et le code se trouvent dans le même fichier :</span><span class="sxs-lookup"><span data-stu-id="72bea-281">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="72bea-282">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-282">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="72bea-283">Le composant `Counter` peut également être créé à l’aide d’un fichier code-behind avec une classe partielle :</span><span class="sxs-lookup"><span data-stu-id="72bea-283">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="72bea-284">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-284">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="72bea-285">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="72bea-285">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="72bea-286">Ajoutez tous les espaces de noms requis au fichier de classe partielle, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="72bea-286">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="72bea-287">Les espaces de noms standard utilisés par les composants Razor sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="72bea-287">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="72bea-288">Spécifier une classe de base</span><span class="sxs-lookup"><span data-stu-id="72bea-288">Specify a base class</span></span>

<span data-ttu-id="72bea-289">La directive [`@inherits`](xref:mvc/views/razor#inherits) peut être utilisée pour spécifier une classe de base pour un composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-289">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="72bea-290">L’exemple suivant montre comment un composant peut hériter d’une classe de base, `BlazorRocksBase`, pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-290">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="72bea-291">La classe de base doit dériver de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="72bea-291">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="72bea-292">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="72bea-292">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="72bea-293">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="72bea-293">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a><span data-ttu-id="72bea-294">Spécifier un attribut</span><span class="sxs-lookup"><span data-stu-id="72bea-294">Specify an attribute</span></span>

<span data-ttu-id="72bea-295">Les attributs peuvent être spécifiés dans les composants Razor avec la directive [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="72bea-295">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="72bea-296">L’exemple suivant applique l’attribut `[Authorize]` à la classe Component :</span><span class="sxs-lookup"><span data-stu-id="72bea-296">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="72bea-297">Importer des composants</span><span class="sxs-lookup"><span data-stu-id="72bea-297">Import components</span></span>

<span data-ttu-id="72bea-298">L’espace de noms d’un composant créé avec Razor est basé sur (par ordre de priorité) :</span><span class="sxs-lookup"><span data-stu-id="72bea-298">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="72bea-299">[`@namespace`](xref:mvc/views/razor#namespace) la désignation dans le balisage du fichier Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="72bea-299">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="72bea-300">`RootNamespace` du projet dans le fichier projet (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="72bea-300">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="72bea-301">Nom du projet, pris à partir du nom de fichier du fichier projet ( *. csproj*), et chemin d’accès de la racine du projet au composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-301">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="72bea-302">Par exemple, le Framework résout *{Project root}/pages/index.Razor* (*BlazorSample. csproj*) en l’espace de noms `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="72bea-302">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="72bea-303">Les composants C# suivent les règles de liaison de nom.</span><span class="sxs-lookup"><span data-stu-id="72bea-303">Components follow C# name binding rules.</span></span> <span data-ttu-id="72bea-304">Pour le composant `Index` dans cet exemple, les composants de l’étendue sont tous les composants :</span><span class="sxs-lookup"><span data-stu-id="72bea-304">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="72bea-305">Dans le même dossier, *pages*.</span><span class="sxs-lookup"><span data-stu-id="72bea-305">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="72bea-306">Composants de la racine du projet qui ne spécifient pas explicitement un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="72bea-306">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="72bea-307">Les composants définis dans un espace de noms différent sont placés dans la portée à l’aide de la directive [`@using`](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="72bea-307">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="72bea-308">Si un autre composant, `NavMenu.razor`, existe dans le dossier *BlazorSample/Shared/* , le composant peut être utilisé dans `Index.razor` avec l’instruction `@using` suivante :</span><span class="sxs-lookup"><span data-stu-id="72bea-308">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="72bea-309">Les composants peuvent également être référencés à l’aide de leurs noms complets, ce qui ne nécessite pas la directive [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="72bea-309">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="72bea-310">La qualification de `global::` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="72bea-310">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="72bea-311">L’importation de composants avec des instructions `using` avec alias (par exemple, `@using Foo = Bar`) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="72bea-311">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="72bea-312">Les noms partiellement qualifiés ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="72bea-312">Partially qualified names aren't supported.</span></span> <span data-ttu-id="72bea-313">Par exemple, l’ajout de `@using BlazorSample` et la référencement d' `NavMenu.razor` avec `<Shared.NavMenu></Shared.NavMenu>` ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="72bea-313">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="72bea-314">Attributs d’éléments HTML conditionnels</span><span class="sxs-lookup"><span data-stu-id="72bea-314">Conditional HTML element attributes</span></span>

<span data-ttu-id="72bea-315">Les attributs des éléments HTML sont rendus de manière conditionnelle en fonction de la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="72bea-315">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="72bea-316">Si la valeur est `false` ou `null`, l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="72bea-316">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="72bea-317">Si la valeur est `true`, l’attribut est rendu réduit.</span><span class="sxs-lookup"><span data-stu-id="72bea-317">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="72bea-318">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est restitué dans le balisage de l’élément :</span><span class="sxs-lookup"><span data-stu-id="72bea-318">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="72bea-319">Si `IsCompleted` est `true`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="72bea-319">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="72bea-320">Si `IsCompleted` est `false`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="72bea-320">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="72bea-321">Pour plus d’informations, consultez <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="72bea-321">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="72bea-322">Certains attributs HTML, tels que [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), ne fonctionnent pas correctement quand le type .net est un `bool`.</span><span class="sxs-lookup"><span data-stu-id="72bea-322">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="72bea-323">Dans ce cas, utilisez un type de `string` à la place d’un `bool`.</span><span class="sxs-lookup"><span data-stu-id="72bea-323">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="72bea-324">HTML brut</span><span class="sxs-lookup"><span data-stu-id="72bea-324">Raw HTML</span></span>

<span data-ttu-id="72bea-325">Les chaînes sont normalement rendues à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage qu’elles peuvent contenir est ignorée et traitée comme du texte littéral.</span><span class="sxs-lookup"><span data-stu-id="72bea-325">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="72bea-326">Pour afficher le code HTML brut, encapsulez le contenu HTML dans une valeur `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="72bea-326">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="72bea-327">La valeur est analysée au format HTML ou SVG et est insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="72bea-327">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="72bea-328">Le rendu de code HTML brut construit à partir de n’importe quelle source non approuvée constitue un risque pour la **sécurité** et doit être évité !</span><span class="sxs-lookup"><span data-stu-id="72bea-328">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="72bea-329">L’exemple suivant illustre l’utilisation du type `MarkupString` pour ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :</span><span class="sxs-lookup"><span data-stu-id="72bea-329">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="72bea-330">Valeurs et paramètres en cascade</span><span class="sxs-lookup"><span data-stu-id="72bea-330">Cascading values and parameters</span></span>

<span data-ttu-id="72bea-331">Dans certains scénarios, il est peu commode de transmettre des données d’un composant ancêtre à un composant descendant à l’aide de [paramètres de composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-331">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="72bea-332">Les valeurs et paramètres en cascade résolvent ce problème en fournissant un moyen pratique pour un composant ancêtre de fournir une valeur à tous ses composants descendants.</span><span class="sxs-lookup"><span data-stu-id="72bea-332">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="72bea-333">Les valeurs et les paramètres en cascade fournissent également une approche pour les composants à coordonner.</span><span class="sxs-lookup"><span data-stu-id="72bea-333">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="72bea-334">Exemple de thème</span><span class="sxs-lookup"><span data-stu-id="72bea-334">Theme example</span></span>

<span data-ttu-id="72bea-335">Dans l’exemple suivant tiré de l’exemple d’application, la classe `ThemeInfo` spécifie les informations de thème pour descendre dans la hiérarchie des composants afin que tous les boutons d’une partie donnée de l’application partagent le même style.</span><span class="sxs-lookup"><span data-stu-id="72bea-335">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="72bea-336">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="72bea-336">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="72bea-337">Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="72bea-337">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="72bea-338">Le composant `CascadingValue` encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants de cette sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="72bea-338">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="72bea-339">Par exemple, l’exemple d’application spécifie les informations de thème (`ThemeInfo`) dans l’une des dispositions de l’application en tant que paramètre en cascade pour tous les composants qui composent le corps de la disposition de la propriété `@Body`.</span><span class="sxs-lookup"><span data-stu-id="72bea-339">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="72bea-340">une valeur de `btn-success` dans le composant de disposition est assignée à `ButtonClass`.</span><span class="sxs-lookup"><span data-stu-id="72bea-340">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="72bea-341">Tout composant descendant peut consommer cette propriété par le biais du `ThemeInfo` objet en cascade.</span><span class="sxs-lookup"><span data-stu-id="72bea-341">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="72bea-342">composant `CascadingValuesParametersLayout` :</span><span class="sxs-lookup"><span data-stu-id="72bea-342">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="72bea-343">Pour utiliser des valeurs en cascade, les composants déclarent des paramètres en cascade à l’aide de l’attribut `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="72bea-343">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="72bea-344">Les valeurs en cascade sont liées aux paramètres en cascade par type.</span><span class="sxs-lookup"><span data-stu-id="72bea-344">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="72bea-345">Dans l’exemple d’application, le composant `CascadingValuesParametersTheme` lie la valeur en cascade `ThemeInfo` à un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="72bea-345">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="72bea-346">Le paramètre est utilisé pour définir la classe CSS pour l’un des boutons affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-346">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="72bea-347">composant `CascadingValuesParametersTheme` :</span><span class="sxs-lookup"><span data-stu-id="72bea-347">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="72bea-348">Pour mettre en cascade plusieurs valeurs du même type dans la même sous-arborescence, fournissez une chaîne de `Name` unique à chaque composant `CascadingValue` et à la `CascadingParameter`correspondante.</span><span class="sxs-lookup"><span data-stu-id="72bea-348">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="72bea-349">Dans l’exemple suivant, deux composants de `CascadingValue` cascadent différentes instances de `MyCascadingType` par nom :</span><span class="sxs-lookup"><span data-stu-id="72bea-349">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="72bea-350">Dans un composant descendant, les paramètres en cascade reçoivent leurs valeurs des valeurs en cascade correspondantes dans le composant ancêtre par nom :</span><span class="sxs-lookup"><span data-stu-id="72bea-350">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="72bea-351">Exemple TabSet</span><span class="sxs-lookup"><span data-stu-id="72bea-351">TabSet example</span></span>

<span data-ttu-id="72bea-352">Les paramètres en cascade permettent également aux composants de collaborer au sein de la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="72bea-352">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="72bea-353">Par exemple, considérez l’exemple *TabSet* suivant dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="72bea-353">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="72bea-354">L’exemple d’application possède une interface `ITab` que les onglets implémentent :</span><span class="sxs-lookup"><span data-stu-id="72bea-354">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="72bea-355">Le composant `CascadingValuesParametersTabSet` utilise le composant `TabSet`, qui contient plusieurs composants `Tab` :</span><span class="sxs-lookup"><span data-stu-id="72bea-355">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="72bea-356">Les composants `Tab` enfants ne sont pas explicitement passés comme paramètres au `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="72bea-356">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="72bea-357">Au lieu de cela, les composants de `Tab` enfants font partie du contenu enfant du `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="72bea-357">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="72bea-358">Toutefois, le `TabSet` doit toujours connaître chaque composant `Tab` afin qu’il puisse restituer les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter de code supplémentaire, le composant `TabSet` *peut se présenter comme une valeur en cascade* qui est ensuite récupérée par les composants de `Tab` descendants.</span><span class="sxs-lookup"><span data-stu-id="72bea-358">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="72bea-359">composant `TabSet` :</span><span class="sxs-lookup"><span data-stu-id="72bea-359">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="72bea-360">Les composants de `Tab` descendants capturent le `TabSet` contenant sous forme de paramètre en cascade, de sorte que les composants `Tab` s’ajoutent eux-mêmes au `TabSet` et coordonnent l’onglet actif.</span><span class="sxs-lookup"><span data-stu-id="72bea-360">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="72bea-361">composant `Tab` :</span><span class="sxs-lookup"><span data-stu-id="72bea-361">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="72bea-362">Modèles Razor</span><span class="sxs-lookup"><span data-stu-id="72bea-362">Razor templates</span></span>

<span data-ttu-id="72bea-363">Les fragments de rendu peuvent être définis à l’aide de la syntaxe de modèle Razor.</span><span class="sxs-lookup"><span data-stu-id="72bea-363">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="72bea-364">Les modèles Razor sont un moyen de définir un extrait de code d’interface utilisateur et de supposer le format suivant :</span><span class="sxs-lookup"><span data-stu-id="72bea-364">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="72bea-365">L’exemple suivant montre comment spécifier des valeurs `RenderFragment` et `RenderFragment<T>` et afficher des modèles directement dans un composant.</span><span class="sxs-lookup"><span data-stu-id="72bea-365">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="72bea-366">Les fragments de rendu peuvent également être passés comme arguments à des [composants basés](xref:blazor/templated-components)sur un modèle.</span><span class="sxs-lookup"><span data-stu-id="72bea-366">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="72bea-367">Sortie rendue du code précédent :</span><span class="sxs-lookup"><span data-stu-id="72bea-367">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="72bea-368">Images SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="72bea-368">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="72bea-369">Étant donné que Blazor restitue le code HTML, les images prises en charge par le navigateur, y compris les images SVG (Scalable Vector Graphics) ( *. svg*), sont prises en charge via la balise `<img>` :</span><span class="sxs-lookup"><span data-stu-id="72bea-369">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="72bea-370">De même, les images SVG sont prises en charge dans les règles CSS d’un fichier de feuille de style ( *. CSS*) :</span><span class="sxs-lookup"><span data-stu-id="72bea-370">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="72bea-371">Toutefois, le balisage SVG en ligne n’est pas pris en charge dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="72bea-371">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="72bea-372">Si vous placez une balise de `<svg>` directement dans un fichier de composant ( *. Razor*), le rendu d’image de base est pris en charge, mais de nombreux scénarios avancés ne sont pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="72bea-372">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="72bea-373">Par exemple, les balises `<use>` ne sont pas respectées et `@bind` ne peuvent pas être utilisées avec certaines balises SVG.</span><span class="sxs-lookup"><span data-stu-id="72bea-373">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="72bea-374">Nous prévoyons de traiter ces limitations dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="72bea-374">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="72bea-375">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72bea-375">Additional resources</span></span>

* <span data-ttu-id="72bea-376"><xref:security/blazor/server> &ndash; contient des conseils sur la création d’applications serveur Blazor qui doivent rivaliser avec l’épuisement des ressources.</span><span class="sxs-lookup"><span data-stu-id="72bea-376"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
