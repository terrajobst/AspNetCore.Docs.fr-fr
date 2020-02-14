---
title: Créer et utiliser des composants ASP.NET Core Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants Razor, notamment comment lier des données, gérer des événements et gérer des cycles de vie de composant.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: 0da0d83a4fde7b753a84bf05d3a9284776f2881f
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213348"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="be6d8-103">Créer et utiliser des composants ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="be6d8-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="be6d8-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="be6d8-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="be6d8-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="be6d8-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="be6d8-106">Les applications éblouissantes sont créées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="be6d8-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="be6d8-107">Un composant est un bloc d’interface utilisateur (IU) autonome, tel qu’une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="be6d8-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="be6d8-108">Un composant comprend le balisage HTML et la logique de traitement requise pour injecter des données ou répondre à des événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="be6d8-109">Les composants sont flexibles et légers.</span><span class="sxs-lookup"><span data-stu-id="be6d8-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="be6d8-110">Elles peuvent être imbriquées, réutilisées et partagées entre les projets.</span><span class="sxs-lookup"><span data-stu-id="be6d8-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="be6d8-111">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="be6d8-111">Component classes</span></span>

<span data-ttu-id="be6d8-112">Les composants sont implémentés dans les fichiers de composants [Razor](xref:mvc/views/razor) ( *. Razor*) C# à l’aide d’une combinaison de balises et html.</span><span class="sxs-lookup"><span data-stu-id="be6d8-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="be6d8-113">Un composant de éblouissant est officiellement désigné sous le terme de *composant Razor*.</span><span class="sxs-lookup"><span data-stu-id="be6d8-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="be6d8-114">Le nom d’un composant doit commencer par un caractère majuscule.</span><span class="sxs-lookup"><span data-stu-id="be6d8-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="be6d8-115">Par exemple, *MyCoolComponent. Razor* est valide et *MyCoolComponent. Razor* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="be6d8-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="be6d8-116">L’interface utilisateur d’un composant est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="be6d8-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="be6d8-117">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="be6d8-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="be6d8-118">Quand une application est compilée, le balisage C# html et la logique de rendu sont convertis en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="be6d8-119">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="be6d8-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="be6d8-120">Les membres de la classe de composants sont définis dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="be6d8-121">Dans le bloc `@code`, l’état du composant (propriétés, champs) est spécifié avec les méthodes de gestion des événements ou pour la définition d’une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="be6d8-122">Plus d’un bloc `@code` est autorisé.</span><span class="sxs-lookup"><span data-stu-id="be6d8-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="be6d8-123">Les membres de composant peuvent être utilisés dans le cadre de la logique de C# rendu du composant à l’aide d’expressions qui commencent par `@`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="be6d8-124">Par exemple, un C# champ est rendu en préfixant `@` au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="be6d8-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="be6d8-125">L’exemple suivant évalue et affiche :</span><span class="sxs-lookup"><span data-stu-id="be6d8-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="be6d8-126">`_headingFontStyle` à la valeur de propriété CSS pour `font-style`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="be6d8-127">`_headingText` au contenu de l’élément `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="be6d8-128">Une fois le composant restitué initialement, le composant régénère son arborescence de rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="be6d8-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="be6d8-129">Il compare ensuite la nouvelle arborescence de rendu à la précédente et applique toutes les modifications à la Document Object Model du navigateur (DOM).</span><span class="sxs-lookup"><span data-stu-id="be6d8-129">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="be6d8-130">Les composants sont C# des classes ordinaires qui peuvent être placées n’importe où dans un projet.</span><span class="sxs-lookup"><span data-stu-id="be6d8-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="be6d8-131">Les composants qui produisent des pages Web résident généralement dans le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="be6d8-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="be6d8-132">Les composants qui ne sont pas des pages sont souvent placés dans le dossier *partagé* ou dans un dossier personnalisé ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="be6d8-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="be6d8-133">En règle générale, l’espace de noms d’un composant est dérivé de l’espace de noms racine de l’application et de l’emplacement (dossier) du composant dans l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="be6d8-134">Si l’espace de noms racine de l’application est `BlazorApp` et que le composant `Counter` se trouve dans le dossier *pages* :</span><span class="sxs-lookup"><span data-stu-id="be6d8-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="be6d8-135">L’espace de noms du composant `Counter` est `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="be6d8-136">Le nom de type qualifié complet du composant est `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="be6d8-137">Pour plus d’informations, consultez la section [importer des composants](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="be6d8-138">Pour utiliser un dossier personnalisé, ajoutez l’espace de noms du dossier personnalisé au composant parent ou au fichier *_Imports. Razor* de l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="be6d8-139">Par exemple, l’espace de noms suivant rend les composants dans un dossier de *composants* disponibles lorsque l’espace de noms racine de l’application est `BlazorApp`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="be6d8-140">Intégrer des composants dans des applications Razor Pages et MVC</span><span class="sxs-lookup"><span data-stu-id="be6d8-140">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="be6d8-141">Les composants Razor peuvent être intégrés dans des applications Razor Pages et MVC.</span><span class="sxs-lookup"><span data-stu-id="be6d8-141">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="be6d8-142">Lorsque la page ou la vue est restituée, les composants peuvent être prérendus en même temps.</span><span class="sxs-lookup"><span data-stu-id="be6d8-142">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="be6d8-143">Pour préparer une Razor Pages ou une application MVC pour héberger des composants Razor, suivez les instructions de la section *intégrer des composants Razor dans des applications Razor pages et MVC* de l’article <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-143">To prepare a Razor Pages or MVC app to host Razor components, follow the guidance in the *Integrate Razor components into Razor Pages and MVC apps* section of the <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps> article.</span></span>

<span data-ttu-id="be6d8-144">Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms qui représente le dossier à la page/la vue ou au fichier *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="be6d8-144">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="be6d8-145">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-145">In the following example:</span></span>

* <span data-ttu-id="be6d8-146">Remplacez `MyAppNamespace` par l’espace de noms de l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-146">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="be6d8-147">Si un dossier nommé *Components* n’est pas utilisé pour contenir les composants, remplacez `Components` par le dossier dans lequel se trouvent les composants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-147">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```csharp
@using MyAppNamespace.Components
```

<span data-ttu-id="be6d8-148">Le fichier *_ViewImports. cshtml* se trouve dans le dossier *pages* d’une application Razor pages ou du dossier *views* d’une application MVC.</span><span class="sxs-lookup"><span data-stu-id="be6d8-148">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="be6d8-149">Pour afficher un composant à partir d’une page ou d’une vue, utilisez le tag Helper `Component` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-149">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="be6d8-150">Le type de paramètre doit être sérialisable JSON, ce qui signifie généralement que le type doit avoir un constructeur par défaut et des propriétés définissables.</span><span class="sxs-lookup"><span data-stu-id="be6d8-150">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="be6d8-151">Par exemple, vous pouvez spécifier une valeur pour `IncrementAmount`, car le type de `IncrementAmount` est un `int`, qui est un type primitif pris en charge par le sérialiseur JSON.</span><span class="sxs-lookup"><span data-stu-id="be6d8-151">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="be6d8-152">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-152">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="be6d8-153">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="be6d8-153">Is prerendered into the page.</span></span>
* <span data-ttu-id="be6d8-154">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-154">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="be6d8-155">Description</span><span class="sxs-lookup"><span data-stu-id="be6d8-155">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="be6d8-156">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application de serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="be6d8-156">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="be6d8-157">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application éblouissante.</span><span class="sxs-lookup"><span data-stu-id="be6d8-157">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="be6d8-158">Restitue un marqueur pour une application de serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="be6d8-158">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="be6d8-159">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="be6d8-159">Output from the component isn't included.</span></span> <span data-ttu-id="be6d8-160">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application éblouissante.</span><span class="sxs-lookup"><span data-stu-id="be6d8-160">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="be6d8-161">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="be6d8-161">Renders the component into static HTML.</span></span> |

<span data-ttu-id="be6d8-162">Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie.</span><span class="sxs-lookup"><span data-stu-id="be6d8-162">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="be6d8-163">Les composants ne peuvent pas utiliser les scénarios spécifiques aux vues et aux pages, tels que les vues partielles et les sections.</span><span class="sxs-lookup"><span data-stu-id="be6d8-163">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="be6d8-164">Pour utiliser la logique de la vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-164">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="be6d8-165">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="be6d8-165">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="be6d8-166">Pour plus d’informations sur la façon dont les composants sont rendus, l’état des composants et le tag Helper `Component`, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="be6d8-166">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>

## <a name="tag-helpers-arent-used-in-components"></a><span data-ttu-id="be6d8-167">Les tag helpers ne sont pas utilisés dans les composants</span><span class="sxs-lookup"><span data-stu-id="be6d8-167">Tag Helpers aren't used in components</span></span>

<span data-ttu-id="be6d8-168">Les [tag helpers](xref:mvc/views/tag-helpers/intro) ne sont pas pris en charge dans les composants Razor (fichiers *. Razor* ).</span><span class="sxs-lookup"><span data-stu-id="be6d8-168">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="be6d8-169">Pour fournir des fonctionnalités semblables à tag Helper dans éblouissant, créez un composant avec les mêmes fonctionnalités que le tag Helper et utilisez le composant à la place.</span><span class="sxs-lookup"><span data-stu-id="be6d8-169">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="be6d8-170">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="be6d8-170">Use components</span></span>

<span data-ttu-id="be6d8-171">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="be6d8-171">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="be6d8-172">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-172">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="be6d8-173">La liaison d’attribut respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="be6d8-173">Attribute binding is case sensitive.</span></span> <span data-ttu-id="be6d8-174">Par exemple, `@bind` est valide et `@Bind` n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="be6d8-174">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="be6d8-175">Le balisage suivant dans *index. Razor* rend une instance de `HeadingComponent` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-175">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="be6d8-176">*Composants/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-176">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="be6d8-177">Si un composant contient un élément HTML avec une première lettre majuscule qui ne correspond pas à un nom de composant, un avertissement est émis pour indiquer que l’élément a un nom inattendu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-177">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="be6d8-178">L’ajout d’une instruction `@using` pour l’espace de noms du composant rend le composant disponible, ce qui supprime l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="be6d8-178">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="be6d8-179">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="be6d8-179">Component parameters</span></span>

<span data-ttu-id="be6d8-180">Les composants peuvent avoir des *paramètres de composant*, qui sont définis à l’aide de propriétés publiques sur la classe de composant avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-180">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="be6d8-181">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="be6d8-181">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="be6d8-182">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="be6d8-183">Dans l’exemple suivant tiré de l’exemple d’application, le `ParentComponent` définit la valeur de la propriété `Title` du `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-183">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="be6d8-184">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-184">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="child-content"></a><span data-ttu-id="be6d8-185">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="be6d8-185">Child content</span></span>

<span data-ttu-id="be6d8-186">Les composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-186">Components can set the content of another component.</span></span> <span data-ttu-id="be6d8-187">Le composant d’affectation fournit le contenu entre les balises qui spécifient le composant récepteur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-187">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="be6d8-188">Dans l’exemple suivant, la `ChildComponent` a une propriété `ChildContent` qui représente un `RenderFragment`, qui représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="be6d8-188">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="be6d8-189">La valeur de `ChildContent` est positionnée dans le balisage du composant où le contenu doit être rendu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-189">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="be6d8-190">La valeur de `ChildContent` est reçue du composant parent et affichée à l’intérieur du `panel-body`du panneau de démarrage.</span><span class="sxs-lookup"><span data-stu-id="be6d8-190">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="be6d8-191">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-191">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="be6d8-192">La propriété qui reçoit le contenu du `RenderFragment` doit être nommée `ChildContent` par Convention.</span><span class="sxs-lookup"><span data-stu-id="be6d8-192">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="be6d8-193">Les `ParentComponent` dans l’exemple d’application peuvent fournir du contenu pour le rendu du `ChildComponent` en plaçant le contenu à l’intérieur des balises de `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-193">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="be6d8-194">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-194">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="be6d8-195">Réprojection d’attribut et paramètres arbitraires</span><span class="sxs-lookup"><span data-stu-id="be6d8-195">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="be6d8-196">Les composants peuvent capturer et restituer des attributs supplémentaires en plus des paramètres déclarés du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-196">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="be6d8-197">Des attributs supplémentaires peuvent être capturés dans un dictionnaire *, puis* réintégrés à un élément lorsque le composant est rendu à l’aide de la directive [`@attributes`](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="be6d8-197">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="be6d8-198">Ce scénario est utile lors de la définition d’un composant qui produit un élément de balisage qui prend en charge diverses personnalisations.</span><span class="sxs-lookup"><span data-stu-id="be6d8-198">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="be6d8-199">Par exemple, il peut être fastidieux de définir des attributs séparément pour un `<input>` qui prend en charge de nombreux paramètres.</span><span class="sxs-lookup"><span data-stu-id="be6d8-199">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="be6d8-200">Dans l’exemple suivant, le premier élément `<input>` (`id="useIndividualParams"`) utilise des paramètres de composant individuels, tandis que le deuxième élément `<input>` (`id="useAttributesDict"`) utilise la projection d’attributs :</span><span class="sxs-lookup"><span data-stu-id="be6d8-200">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="be6d8-201">Le type du paramètre doit implémenter `IEnumerable<KeyValuePair<string, object>>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="be6d8-201">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="be6d8-202">L’utilisation de `IReadOnlyDictionary<string, object>` est également une option dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="be6d8-202">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="be6d8-203">Les éléments `<input>` rendus à l’aide des deux approches sont identiques :</span><span class="sxs-lookup"><span data-stu-id="be6d8-203">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="be6d8-204">Pour accepter des attributs arbitraires, définissez un paramètre de composant à l’aide de l’attribut `[Parameter]` avec la propriété `CaptureUnmatchedValues` définie sur `true`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-204">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="be6d8-205">La propriété `CaptureUnmatchedValues` sur `[Parameter]` permet au paramètre de correspondre à tous les attributs qui ne correspondent à aucun autre paramètre.</span><span class="sxs-lookup"><span data-stu-id="be6d8-205">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="be6d8-206">Un composant ne peut définir qu’un seul paramètre avec `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-206">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="be6d8-207">Le type de propriété utilisé avec `CaptureUnmatchedValues` doit pouvoir être assigné à partir de `Dictionary<string, object>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="be6d8-207">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="be6d8-208">`IEnumerable<KeyValuePair<string, object>>` ou `IReadOnlyDictionary<string, object>` sont également des options dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="be6d8-208">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="be6d8-209">La position des `@attributes` par rapport à la position des attributs d’élément est importante.</span><span class="sxs-lookup"><span data-stu-id="be6d8-209">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="be6d8-210">Lorsque `@attributes` sont réparties sur l’élément, les attributs sont traités de droite à gauche (dernier à premier).</span><span class="sxs-lookup"><span data-stu-id="be6d8-210">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="be6d8-211">Prenons l’exemple suivant d’un composant qui consomme un composant `Child` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-211">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="be6d8-212">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-212">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="be6d8-213">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-213">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="be6d8-214">L’attribut `extra` du composant `Child` est défini à droite de `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-214">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="be6d8-215">Le `<div>` rendu du composant `Parent` contient `extra="5"` lorsqu’il est transmis via l’attribut supplémentaire, car les attributs sont traités de droite à gauche (dernier à premier) :</span><span class="sxs-lookup"><span data-stu-id="be6d8-215">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="be6d8-216">Dans l’exemple suivant, l’ordre des `extra` et `@attributes` est inversé dans le `<div>`du composant `Child` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-216">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="be6d8-217">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-217">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="be6d8-218">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-218">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="be6d8-219">Le `<div>` rendu dans le composant `Parent` contient `extra="10"` lorsqu’il est passé par le biais de l’attribut supplémentaire :</span><span class="sxs-lookup"><span data-stu-id="be6d8-219">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="be6d8-220">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="be6d8-220">Data binding</span></span>

<span data-ttu-id="be6d8-221">La liaison de données aux composants et aux éléments DOM s’effectue à l’aide de l’attribut [`@bind`](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-221">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="be6d8-222">L’exemple suivant lie une propriété `CurrentValue` à la valeur de la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="be6d8-222">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="be6d8-223">Lorsque la zone de texte perd le focus, la valeur de la propriété est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="be6d8-223">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="be6d8-224">La zone de texte est mise à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, et non en réponse à la modification de la valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="be6d8-224">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="be6d8-225">Étant donné que les composants s’affichent après l’exécution du code du gestionnaire d’événements, les mises à jour de propriétés sont *généralement* reflétées dans l’interface utilisateur immédiatement après le déclenchement d’un gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="be6d8-225">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="be6d8-226">L’utilisation de `@bind` avec la propriété `CurrentValue` (`<input @bind="CurrentValue" />`) équivaut essentiellement à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="be6d8-226">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="be6d8-227">Lors du rendu du composant, le `value` de l’élément d’entrée provient de la propriété `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-227">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="be6d8-228">Lorsque l’utilisateur tape dans la zone de texte et modifie le focus de l’élément, l’événement `onchange` est déclenché et la propriété `CurrentValue` est définie sur la valeur modifiée.</span><span class="sxs-lookup"><span data-stu-id="be6d8-228">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="be6d8-229">En réalité, la génération de code est plus complexe, car `@bind` gère les cas où les conversions de types sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="be6d8-229">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="be6d8-230">En principe, `@bind` associe la valeur actuelle d’une expression à un attribut `value` et gère les modifications à l’aide du gestionnaire inscrit.</span><span class="sxs-lookup"><span data-stu-id="be6d8-230">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="be6d8-231">En plus de gérer les événements `onchange` avec la syntaxe `@bind`, une propriété ou un champ peut être lié à l’aide d’autres événements en spécifiant un attribut [`@bind-value`](xref:mvc/views/razor#bind) avec un paramètre `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="be6d8-231">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="be6d8-232">L’exemple suivant lie la propriété `CurrentValue` pour l’événement `oninput` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-232">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="be6d8-233">Contrairement à `onchange`, qui se déclenche lorsque l’élément perd le focus, `oninput` se déclenche lorsque la valeur de la zone de texte change.</span><span class="sxs-lookup"><span data-stu-id="be6d8-233">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="be6d8-234">`@bind-value` dans l’exemple précédent lie :</span><span class="sxs-lookup"><span data-stu-id="be6d8-234">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="be6d8-235">Expression (`CurrentValue`) spécifiée à l’attribut `value` de l’élément.</span><span class="sxs-lookup"><span data-stu-id="be6d8-235">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="be6d8-236">Délégué d’événement de modification à l’événement spécifié par `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-236">A change event delegate to the event specified by `@bind-value:event`.</span></span>

### <a name="unparsable-values"></a><span data-ttu-id="be6d8-237">Valeurs inanalysables</span><span class="sxs-lookup"><span data-stu-id="be6d8-237">Unparsable values</span></span>

<span data-ttu-id="be6d8-238">Quand un utilisateur fournit une valeur non analysable à un élément DataBound, la valeur unanalysable est automatiquement rétablie à sa valeur précédente lorsque l’événement de liaison est déclenché.</span><span class="sxs-lookup"><span data-stu-id="be6d8-238">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="be6d8-239">Examinez le cas suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-239">Consider the following scenario:</span></span>

* <span data-ttu-id="be6d8-240">Un élément `<input>` est lié à un type `int` avec une valeur initiale de `123`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-240">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="be6d8-241">L’utilisateur met à jour la valeur de l’élément à `123.45` dans la page et modifie le focus de l’élément.</span><span class="sxs-lookup"><span data-stu-id="be6d8-241">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="be6d8-242">Dans le scénario précédent, la valeur de l’élément est rétablie à `123`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-242">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="be6d8-243">Lorsque la valeur `123.45` est rejetée en faveur de la valeur d’origine de `123`, l’utilisateur sait que sa valeur n’a pas été acceptée.</span><span class="sxs-lookup"><span data-stu-id="be6d8-243">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="be6d8-244">Par défaut, la liaison s’applique à l’événement `onchange` de l’élément (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="be6d8-244">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="be6d8-245">Utilisez `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` pour définir un événement différent.</span><span class="sxs-lookup"><span data-stu-id="be6d8-245">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="be6d8-246">Pour l’événement `oninput` (`@bind-value:event="oninput"`), la Reversion se produit après toute séquence de touches qui introduit une valeur non analysable.</span><span class="sxs-lookup"><span data-stu-id="be6d8-246">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="be6d8-247">Quand vous ciblez l’événement `oninput` avec un type lié `int`, un utilisateur ne peut pas taper un caractère `.`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-247">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="be6d8-248">Si un caractère de `.` est immédiatement supprimé, l’utilisateur reçoit des commentaires immédiats que seuls des nombres entiers sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="be6d8-248">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="be6d8-249">Il existe des scénarios où le rétablissement de la valeur sur l’événement `oninput` n’est pas idéal, par exemple lorsque l’utilisateur doit être autorisé à effacer une valeur de `<input>` non analysable.</span><span class="sxs-lookup"><span data-stu-id="be6d8-249">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="be6d8-250">Les alternatives sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="be6d8-250">Alternatives include:</span></span>

* <span data-ttu-id="be6d8-251">N’utilisez pas l’événement `oninput`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-251">Don't use the `oninput` event.</span></span> <span data-ttu-id="be6d8-252">Utilisez l’événement `onchange` par défaut (`@bind="{PROPERTY OR FIELD}"`), où une valeur non valide n’est pas rétablie tant que l’élément n’a pas perdu le focus.</span><span class="sxs-lookup"><span data-stu-id="be6d8-252">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="be6d8-253">Effectuer une liaison à un type Nullable, tel que `int?` ou `string`, et fournir une logique personnalisée pour gérer les entrées non valides.</span><span class="sxs-lookup"><span data-stu-id="be6d8-253">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="be6d8-254">Utilisez un [composant de validation de formulaire](xref:blazor/forms-validation), tel que `InputNumber` ou `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-254">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="be6d8-255">Les composants de validation de formulaire offrent une prise en charge intégrée pour gérer les entrées non valides.</span><span class="sxs-lookup"><span data-stu-id="be6d8-255">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="be6d8-256">Composants de validation de formulaire :</span><span class="sxs-lookup"><span data-stu-id="be6d8-256">Form validation components:</span></span>
  * <span data-ttu-id="be6d8-257">Permet à l’utilisateur de fournir une entrée non valide et de recevoir des erreurs de validation sur les `EditContext`associées.</span><span class="sxs-lookup"><span data-stu-id="be6d8-257">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="be6d8-258">Affichez les erreurs de validation dans l’interface utilisateur sans interférer avec l’utilisateur qui saisit des données Webform supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="be6d8-258">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

### <a name="globalization"></a><span data-ttu-id="be6d8-259">Globalisation</span><span class="sxs-lookup"><span data-stu-id="be6d8-259">Globalization</span></span>

<span data-ttu-id="be6d8-260">`@bind` valeurs sont mises en forme pour l’affichage et l’analyse à l’aide des règles de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="be6d8-260">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="be6d8-261">La culture actuelle est accessible à partir de la propriété <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-261">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="be6d8-262">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) est utilisé pour les types de champ suivants (`<input type="{TYPE}" />`) :</span><span class="sxs-lookup"><span data-stu-id="be6d8-262">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="be6d8-263">Les types de champ précédents :</span><span class="sxs-lookup"><span data-stu-id="be6d8-263">The preceding field types:</span></span>

* <span data-ttu-id="be6d8-264">Sont affichés à l’aide des règles de mise en forme appropriées basées sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-264">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="be6d8-265">Ne peut pas contenir de texte de forme libre.</span><span class="sxs-lookup"><span data-stu-id="be6d8-265">Can't contain free-form text.</span></span>
* <span data-ttu-id="be6d8-266">Fournir des caractéristiques d’interaction de l’utilisateur en fonction de l’implémentation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-266">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="be6d8-267">Les types de champs suivants ont des exigences de mise en forme spécifiques et ne sont pas actuellement pris en charge par éblouissant, car ils ne sont pas pris en charge par tous les principaux navigateurs :</span><span class="sxs-lookup"><span data-stu-id="be6d8-267">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="be6d8-268">`@bind` prend en charge le paramètre `@bind:culture` pour fournir une <xref:System.Globalization.CultureInfo?displayProperty=fullName> pour l’analyse et la mise en forme d’une valeur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-268">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="be6d8-269">La spécification d’une culture n’est pas recommandée lors de l’utilisation des types de champ `date` et `number`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-269">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="be6d8-270">`date` et `number` disposent d’une prise en charge intégrée de éblouissant qui fournit la culture requise.</span><span class="sxs-lookup"><span data-stu-id="be6d8-270">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="be6d8-271">Pour plus d’informations sur la façon de définir la culture de l’utilisateur, consultez la section [Localization](#localization) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-271">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

### <a name="format-strings"></a><span data-ttu-id="be6d8-272">Chaînes de format</span><span class="sxs-lookup"><span data-stu-id="be6d8-272">Format strings</span></span>

<span data-ttu-id="be6d8-273">La liaison de données fonctionne avec les chaînes de format <xref:System.DateTime> à l’aide de [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="be6d8-273">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="be6d8-274">D’autres expressions de mise en forme, telles que les formats de devise ou de nombre, ne sont pas disponibles pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-274">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="be6d8-275">Dans le code précédent, le type de champ de l’élément `<input>` (`type`) a par défaut la valeur `text`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-275">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="be6d8-276">`@bind:format` est pris en charge pour la liaison des types .NET suivants :</span><span class="sxs-lookup"><span data-stu-id="be6d8-276">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="be6d8-277"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="be6d8-277"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="be6d8-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="be6d8-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="be6d8-279">L’attribut `@bind:format` spécifie le format de date à appliquer aux `value` de l’élément `<input>`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-279">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="be6d8-280">Le format est également utilisé pour analyser la valeur lorsqu’un événement `onchange` se produit.</span><span class="sxs-lookup"><span data-stu-id="be6d8-280">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="be6d8-281">Il n’est pas recommandé de spécifier un format pour le type de champ `date`, car éblouissant offre une prise en charge intégrée de la mise en forme des dates.</span><span class="sxs-lookup"><span data-stu-id="be6d8-281">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="be6d8-282">Malgré la recommandation, utilisez uniquement le format de date `yyyy-MM-dd` pour que la liaison fonctionne correctement si un format est fourni avec le type de champ `date` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-282">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="be6d8-283">Liaison parent-enfant avec les paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="be6d8-283">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="be6d8-284">La liaison reconnaît les paramètres du composant, où `@bind-{property}` pouvez lier une valeur de propriété d’un composant parent à un composant enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-284">Binding recognizes component parameters, where `@bind-{property}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="be6d8-285">La liaison d’un enfant à un parent est couverte dans la [liaison enfant-parent avec la section de liaison chaînée](#child-to-parent-binding-with-chained-bind) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-285">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="be6d8-286">Le composant enfant suivant (`ChildComponent`) a un paramètre de composant `Year` et `YearChanged` rappel :</span><span class="sxs-lookup"><span data-stu-id="be6d8-286">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="be6d8-287">`EventCallback<T>` est expliqué dans la section [EventCallback suivante](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-287">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="be6d8-288">Le composant parent suivant utilise :</span><span class="sxs-lookup"><span data-stu-id="be6d8-288">The following parent component uses:</span></span>

* <span data-ttu-id="be6d8-289">`ChildComponent` et lie le paramètre `ParentYear` du parent au paramètre `Year` sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-289">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="be6d8-290">L’événement `onclick` est utilisé pour déclencher la méthode `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-290">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="be6d8-291">Pour plus d’informations, consultez la section [gestion des événements](#event-handling) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-291">For more information, see the [Event handling](#event-handling) section.</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="be6d8-292">Le chargement de l' `ParentComponent` produit le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-292">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="be6d8-293">Si la valeur de la propriété `ParentYear` est modifiée en sélectionnant le bouton dans la `ParentComponent`, la propriété `Year` de la `ChildComponent` est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="be6d8-293">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="be6d8-294">La nouvelle valeur de `Year` est rendue dans l’interface utilisateur lorsque le `ParentComponent` est restitué à nouveau :</span><span class="sxs-lookup"><span data-stu-id="be6d8-294">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="be6d8-295">Le paramètre `Year` peut être lié, car il a un `YearChanged` événement associé qui correspond au type du paramètre `Year`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-295">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="be6d8-296">Par Convention, `<ChildComponent @bind-Year="ParentYear" />` revient essentiellement à écrire :</span><span class="sxs-lookup"><span data-stu-id="be6d8-296">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="be6d8-297">En général, une propriété peut être liée à un gestionnaire d’événements correspondant à l’aide de `@bind-property:event` attribut.</span><span class="sxs-lookup"><span data-stu-id="be6d8-297">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="be6d8-298">Par exemple, la propriété `MyProp` peut être liée à `MyEventHandler` à l’aide des deux attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="be6d8-298">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="be6d8-299">Liaison enfant-parent avec liaison chaînée</span><span class="sxs-lookup"><span data-stu-id="be6d8-299">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="be6d8-300">Un scénario courant consiste à chaîner un paramètre lié aux données à un élément de page dans la sortie du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-300">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="be6d8-301">Ce scénario est appelé *liaison chaînée* car plusieurs niveaux de liaison se produisent simultanément.</span><span class="sxs-lookup"><span data-stu-id="be6d8-301">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="be6d8-302">Une liaison chaînée ne peut pas être implémentée avec `@bind` syntaxe dans l’élément de la page.</span><span class="sxs-lookup"><span data-stu-id="be6d8-302">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="be6d8-303">Le gestionnaire d’événements et la valeur doivent être spécifiés séparément.</span><span class="sxs-lookup"><span data-stu-id="be6d8-303">The event handler and value must be specified separately.</span></span> <span data-ttu-id="be6d8-304">Toutefois, un composant parent peut utiliser `@bind` syntaxe avec le paramètre du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-304">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="be6d8-305">Le composant `PasswordField` suivant (*passwordField. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="be6d8-305">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="be6d8-306">Définit la valeur d’un élément `<input>` sur une propriété `Password`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-306">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="be6d8-307">Expose les modifications de la propriété `Password` à un composant parent avec un [EventCallback suivante](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="be6d8-307">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>
* <span data-ttu-id="be6d8-308">Utilise l’événement `onclick` est utilisé pour déclencher la méthode `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-308">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="be6d8-309">Pour plus d’informations, consultez la section [gestion des événements](#event-handling) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-309">For more information, see the [Event handling](#event-handling) section.</span></span>

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="be6d8-310">Le composant `PasswordField` est utilisé dans un autre composant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-310">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="be6d8-311">Pour effectuer des vérifications ou des erreurs d’interruption sur le mot de passe dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="be6d8-311">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="be6d8-312">Créez un champ de stockage pour `Password` (`_password` dans l’exemple de code suivant).</span><span class="sxs-lookup"><span data-stu-id="be6d8-312">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="be6d8-313">Effectuez les vérifications ou les erreurs d’interruption dans le `Password` Setter.</span><span class="sxs-lookup"><span data-stu-id="be6d8-313">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="be6d8-314">L’exemple suivant fournit un retour immédiat à l’utilisateur si un espace est utilisé dans la valeur du mot de passe :</span><span class="sxs-lookup"><span data-stu-id="be6d8-314">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

### <a name="radio-buttons"></a><span data-ttu-id="be6d8-315">Cases d’option</span><span class="sxs-lookup"><span data-stu-id="be6d8-315">Radio buttons</span></span>

<span data-ttu-id="be6d8-316">Pour plus d’informations sur la liaison à des cases d’option dans un formulaire, consultez <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-316">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>

## <a name="event-handling"></a><span data-ttu-id="be6d8-317">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="be6d8-317">Event handling</span></span>

<span data-ttu-id="be6d8-318">Les composants Razor fournissent des fonctionnalités de gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="be6d8-318">Razor components provide event handling features.</span></span> <span data-ttu-id="be6d8-319">Pour un attribut d’élément HTML nommé `on{EVENT}` (par exemple, `onclick` et `onsubmit`) avec une valeur de type délégué, les composants Razor traitent la valeur de l’attribut en tant que gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="be6d8-319">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="be6d8-320">Le nom de l’attribut est toujours mis en forme [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="be6d8-320">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="be6d8-321">Le code suivant appelle la méthode `UpdateHeading` lorsque le bouton est sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="be6d8-321">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="be6d8-322">Le code suivant appelle la méthode `CheckChanged` lorsque la case à cocher est modifiée dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="be6d8-322">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="be6d8-323">Les gestionnaires d’événements peuvent également être asynchrones et retourner un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-323">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="be6d8-324">Il n’est pas nécessaire d’appeler manuellement [StateHasChanged](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="be6d8-324">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="be6d8-325">Les exceptions sont journalisées lorsqu’elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="be6d8-325">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="be6d8-326">Dans l’exemple suivant, `UpdateHeading` est appelé de façon asynchrone quand le bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="be6d8-326">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="be6d8-327">Types d’arguments d’événement</span><span class="sxs-lookup"><span data-stu-id="be6d8-327">Event argument types</span></span>

<span data-ttu-id="be6d8-328">Pour certains événements, les types d’arguments d’événement sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="be6d8-328">For some events, event argument types are permitted.</span></span> <span data-ttu-id="be6d8-329">Si l’accès à l’un de ces types d’événements n’est pas nécessaire, il n’est pas obligatoire dans l’appel de la méthode.</span><span class="sxs-lookup"><span data-stu-id="be6d8-329">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="be6d8-330">Les `EventArgs` prises en charge sont indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-330">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="be6d8-331">Événement</span><span class="sxs-lookup"><span data-stu-id="be6d8-331">Event</span></span>            | <span data-ttu-id="be6d8-332">Classe</span><span class="sxs-lookup"><span data-stu-id="be6d8-332">Class</span></span>                | <span data-ttu-id="be6d8-333">Remarques et événements DOM</span><span class="sxs-lookup"><span data-stu-id="be6d8-333">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="be6d8-334">Presse-papiers</span><span class="sxs-lookup"><span data-stu-id="be6d8-334">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="be6d8-335">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="be6d8-335">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="be6d8-336">Déplacez</span><span class="sxs-lookup"><span data-stu-id="be6d8-336">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="be6d8-337">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="be6d8-337">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="be6d8-338">`DataTransfer` et `DataTransferItem` contiennent des données d’élément glissées.</span><span class="sxs-lookup"><span data-stu-id="be6d8-338">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="be6d8-339">Error</span><span class="sxs-lookup"><span data-stu-id="be6d8-339">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="be6d8-340">Événement</span><span class="sxs-lookup"><span data-stu-id="be6d8-340">Event</span></span>            | `EventArgs`          | <span data-ttu-id="be6d8-341">*Généralités*</span><span class="sxs-lookup"><span data-stu-id="be6d8-341">*General*</span></span><br><span data-ttu-id="be6d8-342">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="be6d8-342">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="be6d8-343">*Presse-papiers*</span><span class="sxs-lookup"><span data-stu-id="be6d8-343">*Clipboard*</span></span><br><span data-ttu-id="be6d8-344">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="be6d8-344">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="be6d8-345">*Input*</span><span class="sxs-lookup"><span data-stu-id="be6d8-345">*Input*</span></span><br><span data-ttu-id="be6d8-346">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="be6d8-346">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="be6d8-347">*Média*</span><span class="sxs-lookup"><span data-stu-id="be6d8-347">*Media*</span></span><br><span data-ttu-id="be6d8-348">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="be6d8-348">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="be6d8-349">Focus</span><span class="sxs-lookup"><span data-stu-id="be6d8-349">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="be6d8-350">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="be6d8-350">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="be6d8-351">N’inclut pas la prise en charge de `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-351">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="be6d8-352">Entrée</span><span class="sxs-lookup"><span data-stu-id="be6d8-352">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="be6d8-353">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="be6d8-353">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="be6d8-354">Clavier</span><span class="sxs-lookup"><span data-stu-id="be6d8-354">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="be6d8-355">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="be6d8-355">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="be6d8-356">Souris</span><span class="sxs-lookup"><span data-stu-id="be6d8-356">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="be6d8-357">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="be6d8-357">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="be6d8-358">Pointeur de la souris</span><span class="sxs-lookup"><span data-stu-id="be6d8-358">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="be6d8-359">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="be6d8-359">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="be6d8-360">Roulette de la souris</span><span class="sxs-lookup"><span data-stu-id="be6d8-360">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="be6d8-361">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="be6d8-361">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="be6d8-362">Progress</span><span class="sxs-lookup"><span data-stu-id="be6d8-362">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="be6d8-363">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="be6d8-363">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="be6d8-364">Entrées tactiles</span><span class="sxs-lookup"><span data-stu-id="be6d8-364">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="be6d8-365">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="be6d8-365">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="be6d8-366">`TouchPoint` représente un point de contact unique sur un appareil tactile.</span><span class="sxs-lookup"><span data-stu-id="be6d8-366">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="be6d8-367">Pour plus d’informations, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="be6d8-367">For more information, see the following resources:</span></span>

* <span data-ttu-id="be6d8-368">[Classes EventArgs dans la source de référence ASP.net Core (branche dotnet/aspnetcore ou version 3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="be6d8-368">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="be6d8-369">[MDN Web docs : GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; contient des informations sur les éléments HTML qui prennent en charge chaque événement DOM.</span><span class="sxs-lookup"><span data-stu-id="be6d8-369">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="be6d8-370">Expressions lambda</span><span class="sxs-lookup"><span data-stu-id="be6d8-370">Lambda expressions</span></span>

<span data-ttu-id="be6d8-371">Les expressions lambda peuvent également être utilisées :</span><span class="sxs-lookup"><span data-stu-id="be6d8-371">Lambda expressions can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="be6d8-372">Il est souvent pratique de se rapprocher de valeurs supplémentaires, par exemple lors de l’itération sur un ensemble d’éléments.</span><span class="sxs-lookup"><span data-stu-id="be6d8-372">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="be6d8-373">L’exemple suivant crée trois boutons, chacun d’entre eux appelant `UpdateHeading` passant un argument d’événement (`MouseEventArgs`) et son numéro de bouton (`buttonNumber`) lorsqu’il est sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="be6d8-373">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="be6d8-374">N’utilisez **pas** la variable de boucle (`i`) dans une boucle `for` directement dans une expression lambda.</span><span class="sxs-lookup"><span data-stu-id="be6d8-374">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="be6d8-375">Dans le cas contraire, la même variable est utilisée par toutes les expressions lambda provoquant la même valeur de `i`dans toutes les expressions lambda.</span><span class="sxs-lookup"><span data-stu-id="be6d8-375">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="be6d8-376">Capturez toujours sa valeur dans une variable locale (`buttonNumber` dans l’exemple précédent), puis utilisez-la.</span><span class="sxs-lookup"><span data-stu-id="be6d8-376">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="be6d8-377">EventCallback suivante</span><span class="sxs-lookup"><span data-stu-id="be6d8-377">EventCallback</span></span>

<span data-ttu-id="be6d8-378">Un scénario courant avec des composants imbriqués est le souhait d’exécuter la méthode d’un composant parent lorsqu’un événement de composant enfant se produit&mdash;par exemple, lorsqu’un événement `onclick` se produit dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-378">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="be6d8-379">Pour exposer des événements entre les composants, utilisez un `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-379">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="be6d8-380">Un composant parent peut affecter une méthode de rappel à l' `EventCallback`d’un composant enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-380">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="be6d8-381">La `ChildComponent` dans l’exemple d’application (*Components/ChildComponent. Razor*) montre comment le gestionnaire de `onclick` d’un bouton est configuré pour recevoir un délégué `EventCallback` de l' `ParentComponent`de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="be6d8-381">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="be6d8-382">Le `EventCallback` est tapé avec `MouseEventArgs`, ce qui est approprié pour un événement `onclick` à partir d’un périphérique :</span><span class="sxs-lookup"><span data-stu-id="be6d8-382">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="be6d8-383">L' `ParentComponent` affecte à la `EventCallback<T>` (`OnClickCallback`) de l’enfant sa méthode `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-383">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="be6d8-384">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-384">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="be6d8-385">Lorsque le bouton est sélectionné dans la `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-385">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="be6d8-386">La méthode `ShowMessage` de `ParentComponent`est appelée.</span><span class="sxs-lookup"><span data-stu-id="be6d8-386">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="be6d8-387">`_messageText` est mis à jour et affiché dans le `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-387">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="be6d8-388">Un appel à [StateHasChanged](xref:blazor/lifecycle#state-changes) n’est pas requis dans la méthode du rappel (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="be6d8-388">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="be6d8-389">`StateHasChanged` est appelé automatiquement pour rerestituer le `ParentComponent`, tout comme les événements enfants déclenchent le rerendu des composants dans les gestionnaires d’événements qui s’exécutent dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-389">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="be6d8-390">`EventCallback` et `EventCallback<T>` autorisent les délégués asynchrones.</span><span class="sxs-lookup"><span data-stu-id="be6d8-390">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="be6d8-391">`EventCallback<T>` est fortement typé et requiert un type d’argument spécifique.</span><span class="sxs-lookup"><span data-stu-id="be6d8-391">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="be6d8-392">`EventCallback` est faiblement typé et autorise tout type d’argument.</span><span class="sxs-lookup"><span data-stu-id="be6d8-392">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="be6d8-393">Appelez une `EventCallback` ou `EventCallback<T>` avec `InvokeAsync` et attendez la <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="be6d8-393">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="be6d8-394">Utilisez `EventCallback` et `EventCallback<T>` pour la gestion des événements et la liaison des paramètres du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-394">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="be6d8-395">Préférez la `EventCallback<T>` fortement typée sur `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-395">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="be6d8-396">`EventCallback<T>` offre un meilleur retour d’erreur aux utilisateurs du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-396">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="be6d8-397">Comme pour d’autres gestionnaires d’événements d’interface utilisateur, la spécification du paramètre d’événement est facultative.</span><span class="sxs-lookup"><span data-stu-id="be6d8-397">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="be6d8-398">Utilisez `EventCallback` quand aucune valeur n’est passée au rappel.</span><span class="sxs-lookup"><span data-stu-id="be6d8-398">Use `EventCallback` when there's no value passed to the callback.</span></span>

### <a name="prevent-default-actions"></a><span data-ttu-id="be6d8-399">Empêcher les actions par défaut</span><span class="sxs-lookup"><span data-stu-id="be6d8-399">Prevent default actions</span></span>

<span data-ttu-id="be6d8-400">Utilisez l’attribut [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive pour empêcher l’action par défaut pour un événement.</span><span class="sxs-lookup"><span data-stu-id="be6d8-400">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="be6d8-401">Quand une clé est sélectionnée sur un appareil d’entrée et que le focus de l’élément se trouve sur une zone de texte, un navigateur affiche normalement le caractère de la clé dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="be6d8-401">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="be6d8-402">Dans l’exemple suivant, le comportement par défaut est évité en spécifiant l’attribut de directive `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-402">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="be6d8-403">Le compteur est incrémenté, et la clé de **+** n’est pas capturée dans la valeur de l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-403">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="be6d8-404">La spécification de l’attribut `@on{EVENT}:preventDefault` sans valeur est équivalente à `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-404">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="be6d8-405">La valeur de l’attribut peut également être une expression.</span><span class="sxs-lookup"><span data-stu-id="be6d8-405">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="be6d8-406">Dans l’exemple suivant, `_shouldPreventDefault` est un champ `bool` défini sur `true` ou `false`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-406">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="be6d8-407">Un gestionnaire d’événements n’est pas nécessaire pour empêcher l’action par défaut.</span><span class="sxs-lookup"><span data-stu-id="be6d8-407">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="be6d8-408">Le gestionnaire d’événements et les scénarios d’action par défaut peuvent être utilisés indépendamment.</span><span class="sxs-lookup"><span data-stu-id="be6d8-408">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="be6d8-409">Arrêter la propagation des événements</span><span class="sxs-lookup"><span data-stu-id="be6d8-409">Stop event propagation</span></span>

<span data-ttu-id="be6d8-410">Utilisez l’attribut [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive pour arrêter la propagation des événements.</span><span class="sxs-lookup"><span data-stu-id="be6d8-410">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="be6d8-411">Dans l’exemple suivant, la sélection de la case à cocher empêche les événements Click du deuxième `<div>` enfant de se propager vers le `<div>`parent :</span><span class="sxs-lookup"><span data-stu-id="be6d8-411">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="be6d8-412">Capturer des références à des composants</span><span class="sxs-lookup"><span data-stu-id="be6d8-412">Capture references to components</span></span>

<span data-ttu-id="be6d8-413">Les références de composant offrent un moyen de référencer une instance de composant afin que vous puissiez émettre des commandes vers cette instance, telles que `Show` ou `Reset`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-413">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="be6d8-414">Pour capturer une référence de composant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-414">To capture a component reference:</span></span>

* <span data-ttu-id="be6d8-415">Ajoutez un attribut [`@ref`](xref:mvc/views/razor#ref) au composant enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-415">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="be6d8-416">Définissez un champ avec le même type que le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-416">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="be6d8-417">Lors du rendu du composant, le champ `_loginDialog` est rempli avec l’instance du composant enfant `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-417">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="be6d8-418">Vous pouvez ensuite appeler des méthodes .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-418">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be6d8-419">La variable `_loginDialog` n’est remplie qu’après le rendu du composant et sa sortie comprend l’élément `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-419">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="be6d8-420">Jusqu’à ce stade, il n’y a rien à référencer.</span><span class="sxs-lookup"><span data-stu-id="be6d8-420">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="be6d8-421">Pour manipuler des références de composants après la fin du rendu du composant, utilisez les [méthodes OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="be6d8-421">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="be6d8-422">Bien que la capture de références de composant utilise une syntaxe similaire pour [capturer des références d’élément](xref:blazor/javascript-interop#capture-references-to-elements), il ne s’agit pas d’une fonctionnalité [JavaScript Interop](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-422">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="be6d8-423">Les références de composants ne sont pas transmises au code JavaScript&mdash;elles sont utilisées uniquement dans le code .NET.</span><span class="sxs-lookup"><span data-stu-id="be6d8-423">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="be6d8-424">N’utilisez **pas** de références de composant pour muter l’état des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-424">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="be6d8-425">Utilisez plutôt des paramètres déclaratifs normaux pour passer des données aux composants enfants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-425">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="be6d8-426">L’utilisation de paramètres déclaratifs normaux entraîne le rerendu automatique des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-426">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="be6d8-427">Appeler des méthodes de composant en externe pour mettre à jour l’État</span><span class="sxs-lookup"><span data-stu-id="be6d8-427">Invoke component methods externally to update state</span></span>

<span data-ttu-id="be6d8-428">Éblouissant utilise un `SynchronizationContext` pour appliquer un seul thread logique d’exécution.</span><span class="sxs-lookup"><span data-stu-id="be6d8-428">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="be6d8-429">Les méthodes de [cycle de vie](xref:blazor/lifecycle) d’un composant et les rappels d’événements déclenchés par éblouissant sont exécutés sur cette `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-429">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="be6d8-430">Dans le cas où un composant doit être mis à jour en fonction d’un événement externe, tel qu’un minuteur ou d’autres notifications, utilisez la méthode `InvokeAsync`, qui sera réexpédiée à l' `SynchronizationContext`de l’éblouissant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-430">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="be6d8-431">Par exemple, considérez un *service de notification* qui peut notifier n’importe quel composant d’écoute de l’État mis à jour :</span><span class="sxs-lookup"><span data-stu-id="be6d8-431">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="be6d8-432">Inscrivez le `NotifierService` en tant que singletion :</span><span class="sxs-lookup"><span data-stu-id="be6d8-432">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="be6d8-433">Dans le webassembly éblouissant, inscrivez le service dans `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-433">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="be6d8-434">Dans le serveur éblouissant, inscrivez le service dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-434">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="be6d8-435">Utilisez la `NotifierService` pour mettre à jour un composant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-435">Use the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="be6d8-436">Dans l’exemple précédent, `NotifierService` appelle la méthode de `OnNotify` du composant en dehors du `SynchronizationContext`de éblouissant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-436">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="be6d8-437">`InvokeAsync` est utilisé pour basculer vers le contexte correct et pour la mise en file d’attente d’un rendu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-437">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="be6d8-438">Utiliser \@clé pour contrôler la conservation des éléments et des composants</span><span class="sxs-lookup"><span data-stu-id="be6d8-438">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="be6d8-439">Lors du rendu d’une liste d’éléments ou de composants, et que les éléments ou composants changent par la suite, l’algorithme de différenciation de éblouissant doit décider quel élément ou composant précédent peut être conservé et comment les objets de modèle doivent être mappés à ces éléments.</span><span class="sxs-lookup"><span data-stu-id="be6d8-439">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="be6d8-440">Normalement, ce processus est automatique et peut être ignoré, mais dans certains cas, il peut être utile de contrôler le processus.</span><span class="sxs-lookup"><span data-stu-id="be6d8-440">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="be6d8-441">Prenons l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-441">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="be6d8-442">Le contenu de la collection `People` peut changer avec des entrées insérées, supprimées ou réorganisées.</span><span class="sxs-lookup"><span data-stu-id="be6d8-442">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="be6d8-443">Lors du rerendu du composant, le composant `<DetailsEditor>` peut changer pour recevoir différentes valeurs de paramètres de `Details`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-443">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="be6d8-444">Cela peut entraîner un rerendu plus complexe que prévu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-444">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="be6d8-445">Dans certains cas, le rerendu peut entraîner des différences de comportement visibles, telles que le focus de l’élément perdu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-445">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="be6d8-446">Le processus de mappage peut être contrôlé à l’aide de l’attribut de directive `@key`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-446">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="be6d8-447">`@key` oblige l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé :</span><span class="sxs-lookup"><span data-stu-id="be6d8-447">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="be6d8-448">En cas de modification de la collection `People`, l’algorithme de comparaison conserve l’association entre les instances de `<DetailsEditor>` et les instances `person` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-448">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="be6d8-449">Si un `Person` est supprimé de la liste `People`, seule l’instance `<DetailsEditor>` correspondante est supprimée de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-449">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="be6d8-450">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="be6d8-450">Other instances are left unchanged.</span></span>
* <span data-ttu-id="be6d8-451">Si une `Person` est insérée à une position dans la liste, une nouvelle instance de `<DetailsEditor>` est insérée à cette position correspondante.</span><span class="sxs-lookup"><span data-stu-id="be6d8-451">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="be6d8-452">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="be6d8-452">Other instances are left unchanged.</span></span>
* <span data-ttu-id="be6d8-453">Si `Person` entrées sont réordonnées, les instances de `<DetailsEditor>` correspondantes sont conservées et réordonnées dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-453">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="be6d8-454">Dans certains scénarios, l’utilisation de `@key` réduit la complexité du rerendu et évite les problèmes potentiels liés aux parties avec état de la modification du modèle DOM, telles que la position du focus.</span><span class="sxs-lookup"><span data-stu-id="be6d8-454">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be6d8-455">Les clés sont locales pour chaque élément ou composant conteneur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-455">Keys are local to each container element or component.</span></span> <span data-ttu-id="be6d8-456">Les clés ne sont pas comparées globalement dans le document.</span><span class="sxs-lookup"><span data-stu-id="be6d8-456">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="be6d8-457">Quand utiliser \@clé</span><span class="sxs-lookup"><span data-stu-id="be6d8-457">When to use \@key</span></span>

<span data-ttu-id="be6d8-458">En général, il est logique d’utiliser `@key` chaque fois qu’une liste est rendue (par exemple, dans un bloc `@foreach`) et qu’une valeur appropriée existe pour définir le `@key`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-458">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="be6d8-459">Vous pouvez également utiliser `@key` pour empêcher éblouissant de conserver un élément ou une sous-arborescence de composants lorsqu’un objet change :</span><span class="sxs-lookup"><span data-stu-id="be6d8-459">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="be6d8-460">Si `@currentPerson` change, la directive d’attribut `@key` force éblouissant à ignorer l’intégralité du `<div>` et de ses descendants, et à reconstruire la sous-arborescence de l’interface utilisateur avec de nouveaux éléments et composants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-460">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="be6d8-461">Cela peut être utile si vous avez besoin de garantir qu’aucun État d’interface utilisateur n’est préservé lorsque `@currentPerson` change.</span><span class="sxs-lookup"><span data-stu-id="be6d8-461">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="be6d8-462">Quand ne pas utiliser la clé de \@</span><span class="sxs-lookup"><span data-stu-id="be6d8-462">When not to use \@key</span></span>

<span data-ttu-id="be6d8-463">Il y a un coût en matière de performances lors de la comparaison avec `@key`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-463">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="be6d8-464">Le coût des performances n’est pas important, mais spécifiez uniquement `@key` si les règles de conservation des éléments ou des composants bénéficient de l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-464">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="be6d8-465">Même si `@key` n’est pas utilisé, éblouissant conserve autant que possible les instances d’élément et de composant enfants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-465">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="be6d8-466">Le seul avantage de l’utilisation de `@key` est de contrôler la *façon dont* les instances de modèle sont mappées aux instances de composant conservées, au lieu de l’algorithme de comparaison qui sélectionne le mappage.</span><span class="sxs-lookup"><span data-stu-id="be6d8-466">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="be6d8-467">Valeurs à utiliser pour \@clé</span><span class="sxs-lookup"><span data-stu-id="be6d8-467">What values to use for \@key</span></span>

<span data-ttu-id="be6d8-468">En règle générale, il est logique de fournir l’un des types de valeur suivants pour `@key`:</span><span class="sxs-lookup"><span data-stu-id="be6d8-468">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="be6d8-469">Instances d’objet de modèle (par exemple, une instance de `Person` comme dans l’exemple précédent).</span><span class="sxs-lookup"><span data-stu-id="be6d8-469">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="be6d8-470">Cela garantit la préservation en fonction de l’égalité des références d’objet.</span><span class="sxs-lookup"><span data-stu-id="be6d8-470">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="be6d8-471">Identificateurs uniques (par exemple, les valeurs de clé primaire de type `int`, `string`ou `Guid`).</span><span class="sxs-lookup"><span data-stu-id="be6d8-471">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="be6d8-472">Assurez-vous que les valeurs utilisées pour `@key` ne sont pas en conflit.</span><span class="sxs-lookup"><span data-stu-id="be6d8-472">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="be6d8-473">Si les valeurs en conflit sont détectées dans le même élément parent, éblouissant lève une exception, car il ne peut pas mapper de manière déterministe les anciens éléments ou composants aux nouveaux éléments ou composants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-473">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="be6d8-474">Utilisez uniquement des valeurs distinctes, telles que des instances d’objets ou des valeurs de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="be6d8-474">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="be6d8-475">Routage</span><span class="sxs-lookup"><span data-stu-id="be6d8-475">Routing</span></span>

<span data-ttu-id="be6d8-476">Le routage dans éblouissant est obtenu en fournissant un modèle de routage à chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-476">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="be6d8-477">Lorsqu’un fichier Razor avec une directive `@page` est compilé, la classe générée reçoit une <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="be6d8-477">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="be6d8-478">Lors de l’exécution, le routeur recherche les classes de composant avec un `RouteAttribute` et rend le composant qui a un modèle de routage correspondant à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="be6d8-478">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="be6d8-479">Plusieurs modèles de routage peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-479">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="be6d8-480">Le composant suivant répond aux demandes de `/BlazorRoute` et de `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-480">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="be6d8-481">*Pages/BlazorRoute. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-481">*Pages/BlazorRoute.razor*:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a><span data-ttu-id="be6d8-482">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="be6d8-482">Route parameters</span></span>

<span data-ttu-id="be6d8-483">Les composants peuvent recevoir des paramètres de routage à partir du modèle de routage fourni dans la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-483">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="be6d8-484">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-484">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="be6d8-485">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-485">*Pages/RouteParameter.razor*:</span></span>

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

<span data-ttu-id="be6d8-486">Les paramètres facultatifs ne sont pas pris en charge. deux directives `@page` sont donc appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="be6d8-486">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="be6d8-487">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="be6d8-487">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="be6d8-488">La deuxième `@page` directive prend le paramètre d’itinéraire `{text}` et assigne la valeur à la propriété `Text`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-488">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="be6d8-489">La syntaxe de paramètre *catch-all* (`*`/`**`), qui capture le chemin d’accès dans plusieurs limites de dossiers, n’est **pas** prise en charge dans les composants Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="be6d8-489">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="be6d8-490">Prise en charge des classes partielles</span><span class="sxs-lookup"><span data-stu-id="be6d8-490">Partial class support</span></span>

<span data-ttu-id="be6d8-491">Les composants Razor sont générés en tant que classes partielles.</span><span class="sxs-lookup"><span data-stu-id="be6d8-491">Razor components are generated as partial classes.</span></span> <span data-ttu-id="be6d8-492">Les composants Razor sont créés à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="be6d8-492">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="be6d8-493">C#le code est défini dans un bloc [`@code`](xref:mvc/views/razor#code) avec le balisage HTML et le code Razor dans un fichier unique.</span><span class="sxs-lookup"><span data-stu-id="be6d8-493">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="be6d8-494">Les modèles éblouissants définissent leurs composants Razor à l’aide de cette approche.</span><span class="sxs-lookup"><span data-stu-id="be6d8-494">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="be6d8-495">C#le code est placé dans un fichier code-behind défini en tant que classe partielle.</span><span class="sxs-lookup"><span data-stu-id="be6d8-495">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="be6d8-496">L’exemple suivant montre le composant `Counter` par défaut avec un bloc `@code` dans une application générée à partir d’un modèle éblouissant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-496">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="be6d8-497">Le balisage HTML, le code C# Razor et le code se trouvent dans le même fichier :</span><span class="sxs-lookup"><span data-stu-id="be6d8-497">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="be6d8-498">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-498">*Counter.razor*:</span></span>

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

<span data-ttu-id="be6d8-499">Le composant `Counter` peut également être créé à l’aide d’un fichier code-behind avec une classe partielle :</span><span class="sxs-lookup"><span data-stu-id="be6d8-499">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="be6d8-500">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-500">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="be6d8-501">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-501">*Counter.razor.cs*:</span></span>

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

<span data-ttu-id="be6d8-502">Ajoutez tous les espaces de noms requis au fichier de classe partielle, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="be6d8-502">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="be6d8-503">Les espaces de noms standard utilisés par les composants Razor sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="be6d8-503">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="be6d8-504">Spécifier une classe de base</span><span class="sxs-lookup"><span data-stu-id="be6d8-504">Specify a base class</span></span>

<span data-ttu-id="be6d8-505">La directive [`@inherits`](xref:mvc/views/razor#inherits) peut être utilisée pour spécifier une classe de base pour un composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-505">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="be6d8-506">L’exemple suivant montre comment un composant peut hériter d’une classe de base, `BlazorRocksBase`, pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-506">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="be6d8-507">La classe de base doit dériver de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-507">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="be6d8-508">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-508">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="be6d8-509">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-509">*BlazorRocksBase.cs*:</span></span>

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

## <a name="specify-an-attribute"></a><span data-ttu-id="be6d8-510">Spécifier un attribut</span><span class="sxs-lookup"><span data-stu-id="be6d8-510">Specify an attribute</span></span>

<span data-ttu-id="be6d8-511">Les attributs peuvent être spécifiés dans les composants Razor avec la directive [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-511">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="be6d8-512">L’exemple suivant applique l’attribut `[Authorize]` à la classe Component :</span><span class="sxs-lookup"><span data-stu-id="be6d8-512">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="be6d8-513">Importer des composants</span><span class="sxs-lookup"><span data-stu-id="be6d8-513">Import components</span></span>

<span data-ttu-id="be6d8-514">L’espace de noms d’un composant créé avec Razor est basé sur (par ordre de priorité) :</span><span class="sxs-lookup"><span data-stu-id="be6d8-514">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="be6d8-515">[`@namespace`](xref:mvc/views/razor#namespace) la désignation dans le balisage du fichier Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="be6d8-515">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="be6d8-516">`RootNamespace` du projet dans le fichier projet (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="be6d8-516">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="be6d8-517">Nom du projet, pris à partir du nom de fichier du fichier projet ( *. csproj*), et chemin d’accès de la racine du projet au composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-517">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="be6d8-518">Par exemple, le Framework résout *{Project root}/pages/index.Razor* (*BlazorSample. csproj*) en l’espace de noms `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-518">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="be6d8-519">Les composants C# suivent les règles de liaison de nom.</span><span class="sxs-lookup"><span data-stu-id="be6d8-519">Components follow C# name binding rules.</span></span> <span data-ttu-id="be6d8-520">Pour le composant `Index` dans cet exemple, les composants de l’étendue sont tous les composants :</span><span class="sxs-lookup"><span data-stu-id="be6d8-520">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="be6d8-521">Dans le même dossier, *pages*.</span><span class="sxs-lookup"><span data-stu-id="be6d8-521">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="be6d8-522">Composants de la racine du projet qui ne spécifient pas explicitement un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="be6d8-522">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="be6d8-523">Les composants définis dans un espace de noms différent sont placés dans la portée à l’aide de la directive [`@using`](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="be6d8-523">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="be6d8-524">Si un autre composant, `NavMenu.razor`, existe dans le dossier *BlazorSample/Shared/* , le composant peut être utilisé dans `Index.razor` avec l’instruction `@using` suivante :</span><span class="sxs-lookup"><span data-stu-id="be6d8-524">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="be6d8-525">Les composants peuvent également être référencés à l’aide de leurs noms complets, ce qui ne nécessite pas la directive [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="be6d8-525">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="be6d8-526">La qualification de `global::` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="be6d8-526">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="be6d8-527">L’importation de composants avec des instructions `using` avec alias (par exemple, `@using Foo = Bar`) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="be6d8-527">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="be6d8-528">Les noms partiellement qualifiés ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="be6d8-528">Partially qualified names aren't supported.</span></span> <span data-ttu-id="be6d8-529">Par exemple, l’ajout de `@using BlazorSample` et la référencement d' `NavMenu.razor` avec `<Shared.NavMenu></Shared.NavMenu>` ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="be6d8-529">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="be6d8-530">Attributs d’éléments HTML conditionnels</span><span class="sxs-lookup"><span data-stu-id="be6d8-530">Conditional HTML element attributes</span></span>

<span data-ttu-id="be6d8-531">Les attributs des éléments HTML sont rendus de manière conditionnelle en fonction de la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="be6d8-531">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="be6d8-532">Si la valeur est `false` ou `null`, l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-532">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="be6d8-533">Si la valeur est `true`, l’attribut est rendu réduit.</span><span class="sxs-lookup"><span data-stu-id="be6d8-533">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="be6d8-534">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est restitué dans le balisage de l’élément :</span><span class="sxs-lookup"><span data-stu-id="be6d8-534">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="be6d8-535">Si `IsCompleted` est `true`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="be6d8-535">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="be6d8-536">Si `IsCompleted` est `false`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="be6d8-536">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="be6d8-537">Pour plus d’informations, consultez <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-537">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="be6d8-538">Certains attributs HTML, tels que [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), ne fonctionnent pas correctement quand le type .net est un `bool`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-538">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="be6d8-539">Dans ce cas, utilisez un type de `string` à la place d’un `bool`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-539">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="be6d8-540">HTML brut</span><span class="sxs-lookup"><span data-stu-id="be6d8-540">Raw HTML</span></span>

<span data-ttu-id="be6d8-541">Les chaînes sont normalement rendues à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage qu’elles peuvent contenir est ignorée et traitée comme du texte littéral.</span><span class="sxs-lookup"><span data-stu-id="be6d8-541">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="be6d8-542">Pour afficher le code HTML brut, encapsulez le contenu HTML dans une valeur `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-542">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="be6d8-543">La valeur est analysée au format HTML ou SVG et est insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="be6d8-543">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="be6d8-544">Le rendu de code HTML brut construit à partir de n’importe quelle source non approuvée constitue un risque pour la **sécurité** et doit être évité !</span><span class="sxs-lookup"><span data-stu-id="be6d8-544">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="be6d8-545">L’exemple suivant illustre l’utilisation du type `MarkupString` pour ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-545">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="be6d8-546">Composants basés sur un modèle</span><span class="sxs-lookup"><span data-stu-id="be6d8-546">Templated components</span></span>

<span data-ttu-id="be6d8-547">Les composants basés sur un modèle sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, qui peuvent ensuite être utilisés dans le cadre de la logique de rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-547">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="be6d8-548">Les composants basés sur un modèle vous permettent de créer des composants de niveau supérieur qui sont plus réutilisables que les composants normaux.</span><span class="sxs-lookup"><span data-stu-id="be6d8-548">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="be6d8-549">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="be6d8-549">A couple of examples include:</span></span>

* <span data-ttu-id="be6d8-550">Composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête, les lignes et le pied de page de la table.</span><span class="sxs-lookup"><span data-stu-id="be6d8-550">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="be6d8-551">Composant de liste qui permet à un utilisateur de spécifier un modèle pour afficher des éléments dans une liste.</span><span class="sxs-lookup"><span data-stu-id="be6d8-551">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="be6d8-552">Paramètres de modèle</span><span class="sxs-lookup"><span data-stu-id="be6d8-552">Template parameters</span></span>

<span data-ttu-id="be6d8-553">Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres de composant de type `RenderFragment` ou `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-553">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="be6d8-554">Un fragment de rendu représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="be6d8-554">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="be6d8-555">`RenderFragment<T>` prend un paramètre de type qui peut être spécifié lors de l’appel du fragment de rendu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-555">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="be6d8-556">composant `TableTemplate` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-556">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="be6d8-557">Lorsque vous utilisez un composant basé sur un modèle, les paramètres de modèle peuvent être spécifiés à l’aide d’éléments enfants qui correspondent aux noms des paramètres (`TableHeader` et `RowTemplate` dans l’exemple suivant) :</span><span class="sxs-lookup"><span data-stu-id="be6d8-557">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="be6d8-558">Paramètres de contexte de modèle</span><span class="sxs-lookup"><span data-stu-id="be6d8-558">Template context parameters</span></span>

<span data-ttu-id="be6d8-559">Les arguments de composant de type `RenderFragment<T>` passés comme éléments ont un paramètre implicite nommé `context` (par exemple, à partir de l’exemple de code précédent, `@context.PetId`), mais vous pouvez modifier le nom de paramètre à l’aide de l’attribut `Context` sur l’élément enfant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-559">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="be6d8-560">Dans l’exemple suivant, l’attribut `Context` de l’élément `RowTemplate` spécifie le paramètre `pet` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-560">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="be6d8-561">Vous pouvez également spécifier l’attribut `Context` sur l’élément Component.</span><span class="sxs-lookup"><span data-stu-id="be6d8-561">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="be6d8-562">L’attribut `Context` spécifié s’applique à tous les paramètres de modèle spécifiés.</span><span class="sxs-lookup"><span data-stu-id="be6d8-562">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="be6d8-563">Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans élément enfant d’encapsulation).</span><span class="sxs-lookup"><span data-stu-id="be6d8-563">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="be6d8-564">Dans l’exemple suivant, l’attribut `Context` apparaît sur l’élément `TableTemplate` et s’applique à tous les paramètres de modèle :</span><span class="sxs-lookup"><span data-stu-id="be6d8-564">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="be6d8-565">Composants génériques</span><span class="sxs-lookup"><span data-stu-id="be6d8-565">Generic-typed components</span></span>

<span data-ttu-id="be6d8-566">Les composants basés sur un modèle sont souvent typés de façon générique.</span><span class="sxs-lookup"><span data-stu-id="be6d8-566">Templated components are often generically typed.</span></span> <span data-ttu-id="be6d8-567">Par exemple, un composant générique `ListViewTemplate` peut être utilisé pour restituer des valeurs `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-567">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="be6d8-568">Pour définir un composant générique, utilisez la directive [`@typeparam`](xref:mvc/views/razor#typeparam) pour spécifier les paramètres de type :</span><span class="sxs-lookup"><span data-stu-id="be6d8-568">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="be6d8-569">Lorsque vous utilisez des composants génériques, le paramètre de type est déduit si possible :</span><span class="sxs-lookup"><span data-stu-id="be6d8-569">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="be6d8-570">Sinon, le paramètre de type doit être spécifié explicitement à l’aide d’un attribut qui correspond au nom du paramètre de type.</span><span class="sxs-lookup"><span data-stu-id="be6d8-570">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="be6d8-571">Dans l’exemple suivant, `TItem="Pet"` spécifie le type :</span><span class="sxs-lookup"><span data-stu-id="be6d8-571">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="be6d8-572">Valeurs et paramètres en cascade</span><span class="sxs-lookup"><span data-stu-id="be6d8-572">Cascading values and parameters</span></span>

<span data-ttu-id="be6d8-573">Dans certains scénarios, il est peu commode de transmettre des données d’un composant ancêtre à un composant descendant à l’aide de [paramètres de composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-573">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="be6d8-574">Les valeurs et paramètres en cascade résolvent ce problème en fournissant un moyen pratique pour un composant ancêtre de fournir une valeur à tous ses composants descendants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-574">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="be6d8-575">Les valeurs et les paramètres en cascade fournissent également une approche pour les composants à coordonner.</span><span class="sxs-lookup"><span data-stu-id="be6d8-575">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="be6d8-576">Exemple de thème</span><span class="sxs-lookup"><span data-stu-id="be6d8-576">Theme example</span></span>

<span data-ttu-id="be6d8-577">Dans l’exemple suivant tiré de l’exemple d’application, la classe `ThemeInfo` spécifie les informations de thème pour descendre dans la hiérarchie des composants afin que tous les boutons d’une partie donnée de l’application partagent le même style.</span><span class="sxs-lookup"><span data-stu-id="be6d8-577">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="be6d8-578">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="be6d8-578">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="be6d8-579">Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="be6d8-579">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="be6d8-580">Le composant `CascadingValue` encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants de cette sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="be6d8-580">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="be6d8-581">Par exemple, l’exemple d’application spécifie les informations de thème (`ThemeInfo`) dans l’une des dispositions de l’application en tant que paramètre en cascade pour tous les composants qui composent le corps de la disposition de la propriété `@Body`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-581">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="be6d8-582">une valeur de `btn-success` dans le composant de disposition est assignée à `ButtonClass`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-582">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="be6d8-583">Tout composant descendant peut consommer cette propriété par le biais du `ThemeInfo` objet en cascade.</span><span class="sxs-lookup"><span data-stu-id="be6d8-583">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="be6d8-584">composant `CascadingValuesParametersLayout` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-584">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="be6d8-585">Pour utiliser des valeurs en cascade, les composants déclarent des paramètres en cascade à l’aide de l’attribut `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-585">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="be6d8-586">Les valeurs en cascade sont liées aux paramètres en cascade par type.</span><span class="sxs-lookup"><span data-stu-id="be6d8-586">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="be6d8-587">Dans l’exemple d’application, le composant `CascadingValuesParametersTheme` lie la valeur en cascade `ThemeInfo` à un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="be6d8-587">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="be6d8-588">Le paramètre est utilisé pour définir la classe CSS pour l’un des boutons affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-588">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="be6d8-589">composant `CascadingValuesParametersTheme` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-589">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="be6d8-590">Pour mettre en cascade plusieurs valeurs du même type dans la même sous-arborescence, fournissez une chaîne de `Name` unique à chaque composant `CascadingValue` et à la `CascadingParameter`correspondante.</span><span class="sxs-lookup"><span data-stu-id="be6d8-590">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="be6d8-591">Dans l’exemple suivant, deux composants de `CascadingValue` cascadent différentes instances de `MyCascadingType` par nom :</span><span class="sxs-lookup"><span data-stu-id="be6d8-591">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="be6d8-592">Dans un composant descendant, les paramètres en cascade reçoivent leurs valeurs des valeurs en cascade correspondantes dans le composant ancêtre par nom :</span><span class="sxs-lookup"><span data-stu-id="be6d8-592">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="be6d8-593">Exemple TabSet</span><span class="sxs-lookup"><span data-stu-id="be6d8-593">TabSet example</span></span>

<span data-ttu-id="be6d8-594">Les paramètres en cascade permettent également aux composants de collaborer au sein de la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-594">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="be6d8-595">Par exemple, considérez l’exemple *TabSet* suivant dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-595">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="be6d8-596">L’exemple d’application possède une interface `ITab` que les onglets implémentent :</span><span class="sxs-lookup"><span data-stu-id="be6d8-596">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="be6d8-597">Le composant `CascadingValuesParametersTabSet` utilise le composant `TabSet`, qui contient plusieurs composants `Tab` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-597">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

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

<span data-ttu-id="be6d8-598">Les composants `Tab` enfants ne sont pas explicitement passés comme paramètres au `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-598">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="be6d8-599">Au lieu de cela, les composants de `Tab` enfants font partie du contenu enfant du `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-599">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="be6d8-600">Toutefois, le `TabSet` doit toujours connaître chaque composant `Tab` afin qu’il puisse restituer les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter de code supplémentaire, le composant `TabSet` *peut se présenter comme une valeur en cascade* qui est ensuite récupérée par les composants de `Tab` descendants.</span><span class="sxs-lookup"><span data-stu-id="be6d8-600">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="be6d8-601">composant `TabSet` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-601">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="be6d8-602">Les composants de `Tab` descendants capturent le `TabSet` contenant sous forme de paramètre en cascade, de sorte que les composants `Tab` s’ajoutent eux-mêmes au `TabSet` et coordonnent l’onglet actif.</span><span class="sxs-lookup"><span data-stu-id="be6d8-602">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="be6d8-603">composant `Tab` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-603">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="be6d8-604">Modèles Razor</span><span class="sxs-lookup"><span data-stu-id="be6d8-604">Razor templates</span></span>

<span data-ttu-id="be6d8-605">Les fragments de rendu peuvent être définis à l’aide de la syntaxe de modèle Razor.</span><span class="sxs-lookup"><span data-stu-id="be6d8-605">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="be6d8-606">Les modèles Razor sont un moyen de définir un extrait de code d’interface utilisateur et de supposer le format suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-606">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="be6d8-607">L’exemple suivant montre comment spécifier des valeurs `RenderFragment` et `RenderFragment<T>` et afficher des modèles directement dans un composant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-607">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="be6d8-608">Les fragments de rendu peuvent également être passés comme arguments à des [composants basés](#templated-components)sur un modèle.</span><span class="sxs-lookup"><span data-stu-id="be6d8-608">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="be6d8-609">Sortie rendue du code précédent :</span><span class="sxs-lookup"><span data-stu-id="be6d8-609">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="be6d8-610">Logique RenderTreeBuilder manuelle</span><span class="sxs-lookup"><span data-stu-id="be6d8-610">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="be6d8-611">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fournit des méthodes pour la manipulation des composants et des éléments, notamment la C# génération manuelle de composants dans le code.</span><span class="sxs-lookup"><span data-stu-id="be6d8-611">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="be6d8-612">L’utilisation de `RenderTreeBuilder` pour créer des composants est un scénario avancé.</span><span class="sxs-lookup"><span data-stu-id="be6d8-612">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="be6d8-613">Un composant mal formé (par exemple, une balise de balisage non fermée) peut entraîner un comportement indéfini.</span><span class="sxs-lookup"><span data-stu-id="be6d8-613">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="be6d8-614">Prenons le composant `PetDetails` suivant, qui peut être intégré manuellement à un autre composant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-614">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="be6d8-615">Dans l’exemple suivant, la boucle de la méthode `CreateComponent` génère trois composants `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-615">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="be6d8-616">Lors de l’appel de méthodes `RenderTreeBuilder` pour créer les composants (`OpenComponent` et `AddAttribute`), les numéros de séquence sont des numéros de ligne de code source.</span><span class="sxs-lookup"><span data-stu-id="be6d8-616">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="be6d8-617">L’algorithme de différence éblouissant s’appuie sur les numéros de séquence correspondant à des lignes de code distinctes, et non sur des appels d’appel distincts.</span><span class="sxs-lookup"><span data-stu-id="be6d8-617">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="be6d8-618">Lors de la création d’un composant avec des méthodes `RenderTreeBuilder`, coder en dur les arguments pour les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="be6d8-618">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="be6d8-619">**L’utilisation d’un calcul ou d’un compteur pour générer le numéro de séquence peut entraîner des performances médiocres.**</span><span class="sxs-lookup"><span data-stu-id="be6d8-619">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="be6d8-620">Pour plus d’informations, consultez la section [numéros de séquence liés à des numéros de ligne de code et non à un ordre d’exécution](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-620">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="be6d8-621">composant `BuiltContent` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-621">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="be6d8-622">Les types dans `Microsoft.AspNetCore.Components.RenderTree` permettent le traitement des *résultats* des opérations de rendu.</span><span class="sxs-lookup"><span data-stu-id="be6d8-622">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="be6d8-623">Il s’agit des détails internes de l’implémentation du Framework éblouissant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-623">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="be6d8-624">Ces types doivent être considérés comme *instables* et susceptibles d’être modifiés dans les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="be6d8-624">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="be6d8-625">Les numéros de séquence sont liés aux numéros de ligne de code et non à l’ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="be6d8-625">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="be6d8-626">Les fichiers de `.razor` éblouissant sont toujours compilés.</span><span class="sxs-lookup"><span data-stu-id="be6d8-626">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="be6d8-627">C’est un avantage considérable pour `.razor`, car l’étape de compilation peut être utilisée pour injecter des informations qui améliorent les performances de l’application au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="be6d8-627">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="be6d8-628">Un exemple clé de ces améliorations concerne les *numéros de séquence*.</span><span class="sxs-lookup"><span data-stu-id="be6d8-628">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="be6d8-629">Les numéros de séquence indiquent au runtime que les sorties proviennent de lignes de code distinctes et ordonnées.</span><span class="sxs-lookup"><span data-stu-id="be6d8-629">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="be6d8-630">Le runtime utilise ces informations pour générer des différences d’arborescence efficaces en temps linéaire, ce qui est beaucoup plus rapide qu’un algorithme de comparaison d’arborescence générale.</span><span class="sxs-lookup"><span data-stu-id="be6d8-630">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="be6d8-631">Considérez le fichier de composant Razor ( *. Razor*) suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-631">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="be6d8-632">Le code précédent se compile comme suit :</span><span class="sxs-lookup"><span data-stu-id="be6d8-632">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="be6d8-633">Lorsque le code s’exécute pour la première fois, si `someFlag` est `true`, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="be6d8-633">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="be6d8-634">Séquence</span><span class="sxs-lookup"><span data-stu-id="be6d8-634">Sequence</span></span> | <span data-ttu-id="be6d8-635">Type</span><span class="sxs-lookup"><span data-stu-id="be6d8-635">Type</span></span>      | <span data-ttu-id="be6d8-636">Données</span><span class="sxs-lookup"><span data-stu-id="be6d8-636">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="be6d8-637">0</span><span class="sxs-lookup"><span data-stu-id="be6d8-637">0</span></span>        | <span data-ttu-id="be6d8-638">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="be6d8-638">Text node</span></span> | <span data-ttu-id="be6d8-639">Premier</span><span class="sxs-lookup"><span data-stu-id="be6d8-639">First</span></span>  |
| <span data-ttu-id="be6d8-640">1</span><span class="sxs-lookup"><span data-stu-id="be6d8-640">1</span></span>        | <span data-ttu-id="be6d8-641">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="be6d8-641">Text node</span></span> | <span data-ttu-id="be6d8-642">Seconde</span><span class="sxs-lookup"><span data-stu-id="be6d8-642">Second</span></span> |

<span data-ttu-id="be6d8-643">Imaginez que `someFlag` devient `false`et que le balisage est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="be6d8-643">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="be6d8-644">Cette fois-ci, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="be6d8-644">This time, the builder receives:</span></span>

| <span data-ttu-id="be6d8-645">Séquence</span><span class="sxs-lookup"><span data-stu-id="be6d8-645">Sequence</span></span> | <span data-ttu-id="be6d8-646">Type</span><span class="sxs-lookup"><span data-stu-id="be6d8-646">Type</span></span>       | <span data-ttu-id="be6d8-647">Données</span><span class="sxs-lookup"><span data-stu-id="be6d8-647">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="be6d8-648">1</span><span class="sxs-lookup"><span data-stu-id="be6d8-648">1</span></span>        | <span data-ttu-id="be6d8-649">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="be6d8-649">Text node</span></span>  | <span data-ttu-id="be6d8-650">Seconde</span><span class="sxs-lookup"><span data-stu-id="be6d8-650">Second</span></span> |

<span data-ttu-id="be6d8-651">Quand le runtime effectue une comparaison, il constate que l’élément au `0` de la séquence a été supprimé, donc il génère le *script d’édition*trivial suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-651">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="be6d8-652">Supprimez le premier nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="be6d8-652">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="be6d8-653">Qu’est-ce qui se passe si vous générez des numéros séquentiels par programmation</span><span class="sxs-lookup"><span data-stu-id="be6d8-653">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="be6d8-654">Imaginez plutôt que vous avez écrit la logique de générateur d’arborescence de rendu suivante :</span><span class="sxs-lookup"><span data-stu-id="be6d8-654">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="be6d8-655">La première sortie est désormais :</span><span class="sxs-lookup"><span data-stu-id="be6d8-655">Now, the first output is:</span></span>

| <span data-ttu-id="be6d8-656">Séquence</span><span class="sxs-lookup"><span data-stu-id="be6d8-656">Sequence</span></span> | <span data-ttu-id="be6d8-657">Type</span><span class="sxs-lookup"><span data-stu-id="be6d8-657">Type</span></span>      | <span data-ttu-id="be6d8-658">Données</span><span class="sxs-lookup"><span data-stu-id="be6d8-658">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="be6d8-659">0</span><span class="sxs-lookup"><span data-stu-id="be6d8-659">0</span></span>        | <span data-ttu-id="be6d8-660">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="be6d8-660">Text node</span></span> | <span data-ttu-id="be6d8-661">Premier</span><span class="sxs-lookup"><span data-stu-id="be6d8-661">First</span></span>  |
| <span data-ttu-id="be6d8-662">1</span><span class="sxs-lookup"><span data-stu-id="be6d8-662">1</span></span>        | <span data-ttu-id="be6d8-663">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="be6d8-663">Text node</span></span> | <span data-ttu-id="be6d8-664">Seconde</span><span class="sxs-lookup"><span data-stu-id="be6d8-664">Second</span></span> |

<span data-ttu-id="be6d8-665">Ce résultat est identique au cas précédent, donc aucun problème négatif n’existe.</span><span class="sxs-lookup"><span data-stu-id="be6d8-665">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="be6d8-666">`someFlag` est `false` sur le deuxième rendu et la sortie est la suivante :</span><span class="sxs-lookup"><span data-stu-id="be6d8-666">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="be6d8-667">Séquence</span><span class="sxs-lookup"><span data-stu-id="be6d8-667">Sequence</span></span> | <span data-ttu-id="be6d8-668">Type</span><span class="sxs-lookup"><span data-stu-id="be6d8-668">Type</span></span>      | <span data-ttu-id="be6d8-669">Données</span><span class="sxs-lookup"><span data-stu-id="be6d8-669">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="be6d8-670">0</span><span class="sxs-lookup"><span data-stu-id="be6d8-670">0</span></span>        | <span data-ttu-id="be6d8-671">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="be6d8-671">Text node</span></span> | <span data-ttu-id="be6d8-672">Seconde</span><span class="sxs-lookup"><span data-stu-id="be6d8-672">Second</span></span> |

<span data-ttu-id="be6d8-673">Cette fois-ci, l’algorithme diff constate que *deux* modifications ont été apportées, et l’algorithme génère le script Edit suivant :</span><span class="sxs-lookup"><span data-stu-id="be6d8-673">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="be6d8-674">Remplacez la valeur du premier nœud de texte par `Second`.</span><span class="sxs-lookup"><span data-stu-id="be6d8-674">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="be6d8-675">Supprimez le deuxième nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="be6d8-675">Remove the second text node.</span></span>

<span data-ttu-id="be6d8-676">La génération des numéros de séquence a perdu toutes les informations utiles sur l’emplacement où les `if/else` branches et les boucles étaient présentes dans le code d’origine.</span><span class="sxs-lookup"><span data-stu-id="be6d8-676">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="be6d8-677">Il en résulte une comparaison de **deux fois plus longtemps** qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="be6d8-677">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="be6d8-678">Il s’agit d’un exemple trivial.</span><span class="sxs-lookup"><span data-stu-id="be6d8-678">This is a trivial example.</span></span> <span data-ttu-id="be6d8-679">Dans des cas plus réalistes avec des structures complexes et profondément imbriquées, et surtout avec des boucles, le coût des performances est plus grave.</span><span class="sxs-lookup"><span data-stu-id="be6d8-679">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="be6d8-680">Au lieu d’identifier immédiatement les blocs de boucle ou les branches qui ont été insérées ou supprimées, l’algorithme diff doit recurse profondément dans les arborescences de rendu et générer des scripts de modification beaucoup plus longs, car il est mal informé de la façon dont les anciennes et les nouvelles structures sont liées les unes aux autres.</span><span class="sxs-lookup"><span data-stu-id="be6d8-680">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="be6d8-681">Conseils et conclusions</span><span class="sxs-lookup"><span data-stu-id="be6d8-681">Guidance and conclusions</span></span>

* <span data-ttu-id="be6d8-682">Les performances de l’application sont affectées si les numéros séquentiels sont générés dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="be6d8-682">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="be6d8-683">L’infrastructure ne peut pas créer ses propres numéros de séquence automatiquement au moment de l’exécution, car les informations nécessaires n’existent pas, sauf si elles sont capturées au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="be6d8-683">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="be6d8-684">N’écrivez pas de longs blocs de `RenderTreeBuilder` logique implémentée manuellement.</span><span class="sxs-lookup"><span data-stu-id="be6d8-684">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="be6d8-685">Préférez `.razor` fichiers et permettre au compilateur de gérer les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="be6d8-685">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="be6d8-686">Si vous ne parvenez pas à éviter une logique de `RenderTreeBuilder` manuelle, fractionnez les blocs de code longs en éléments plus petits encapsulés dans `OpenRegion`/`CloseRegion` appels.</span><span class="sxs-lookup"><span data-stu-id="be6d8-686">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="be6d8-687">Chaque région a son propre espace distinct pour les numéros de séquence, ce qui vous permet de redémarrer à partir de zéro (ou tout autre nombre arbitraire) à l’intérieur de chaque région.</span><span class="sxs-lookup"><span data-stu-id="be6d8-687">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="be6d8-688">Si les numéros de séquence sont codés en dur, l’algorithme diff exige uniquement que les numéros de séquence augmentent dans la valeur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-688">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="be6d8-689">La valeur initiale et les écarts ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="be6d8-689">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="be6d8-690">Une option légitime consiste à utiliser le numéro de ligne de code comme numéro de séquence, ou à commencer à partir de zéro et à augmenter par des ou des centaines (ou un intervalle de préférence).</span><span class="sxs-lookup"><span data-stu-id="be6d8-690">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="be6d8-691"> utilise des numéros de séquence, tandis que d’autres infrastructures d’interface utilisateur de comparaison d’arborescence ne les utilisent pas.</span><span class="sxs-lookup"><span data-stu-id="be6d8-691"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="be6d8-692">La comparaison est beaucoup plus rapide lorsque les numéros de séquence sont utilisés et Blazor présente l’avantage d’une étape de compilation qui traite automatiquement les numéros séquentiels pour les développeurs qui créent des fichiers *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="be6d8-692">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="be6d8-693">Localisation</span><span class="sxs-lookup"><span data-stu-id="be6d8-693">Localization</span></span>

<span data-ttu-id="be6d8-694">les applications Blazor Server sont localisées à l’aide de l' [intergiciel (middleware](xref:fundamentals/localization#localization-middleware)) de localisation.</span><span class="sxs-lookup"><span data-stu-id="be6d8-694">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="be6d8-695">L’intergiciel sélectionne la culture appropriée pour les utilisateurs qui demandent des ressources à partir de l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-695">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="be6d8-696">La culture peut être définie à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="be6d8-696">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="be6d8-697">Cookies</span><span class="sxs-lookup"><span data-stu-id="be6d8-697">Cookies</span></span>](#cookies)
* [<span data-ttu-id="be6d8-698">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="be6d8-698">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="be6d8-699">Pour plus d’informations et d’exemples, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-699">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="be6d8-700">Configurer l’éditeur de liens pour l’internationalisation (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="be6d8-700">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="be6d8-701">Par défaut, la configuration de l’éditeur de liens de Blazorpour les applications webassembly Blazor supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement.</span><span class="sxs-lookup"><span data-stu-id="be6d8-701">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="be6d8-702">Pour plus d’informations et de conseils sur le contrôle du comportement de l’éditeur de liens, consultez <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-702">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="be6d8-703">Cookies</span><span class="sxs-lookup"><span data-stu-id="be6d8-703">Cookies</span></span>

<span data-ttu-id="be6d8-704">Un cookie de culture de localisation peut conserver la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-704">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="be6d8-705">Le cookie est créé par la méthode `OnGet` de la page hôte de l’application (*pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="be6d8-705">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="be6d8-706">L’intergiciel (middleware) de localisation lit le cookie sur les demandes suivantes pour définir la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-706">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="be6d8-707">L’utilisation d’un cookie garantit que la connexion WebSocket peut propager correctement la culture.</span><span class="sxs-lookup"><span data-stu-id="be6d8-707">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="be6d8-708">Si les schémas de localisation sont basés sur le chemin d’URL ou la chaîne de requête, il est possible que le schéma ne soit pas en mesure de fonctionner avec les WebSockets, et donc de ne pas conserver la culture.</span><span class="sxs-lookup"><span data-stu-id="be6d8-708">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="be6d8-709">Par conséquent, l’utilisation d’un cookie de culture de localisation est l’approche recommandée.</span><span class="sxs-lookup"><span data-stu-id="be6d8-709">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="be6d8-710">Toute technique peut être utilisée pour assigner une culture si la culture est rendue persistante dans un cookie de localisation.</span><span class="sxs-lookup"><span data-stu-id="be6d8-710">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="be6d8-711">Si l’application a déjà un schéma de localisation établi pour les ASP.NET Core côté serveur, continuez à utiliser l’infrastructure de localisation existante de l’application et à définir le cookie de la culture de localisation dans le schéma de l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-711">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="be6d8-712">L’exemple suivant montre comment définir la culture actuelle dans un cookie qui peut être lu par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="be6d8-712">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="be6d8-713">Créez un fichier *pages/Host. cshtml. cs* avec le contenu suivant dans l’application Blazor Server :</span><span class="sxs-lookup"><span data-stu-id="be6d8-713">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="be6d8-714">La localisation est gérée dans l’application :</span><span class="sxs-lookup"><span data-stu-id="be6d8-714">Localization is handled in the app:</span></span>

1. <span data-ttu-id="be6d8-715">Le navigateur envoie une requête HTTP initiale à l’application.</span><span class="sxs-lookup"><span data-stu-id="be6d8-715">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="be6d8-716">La culture est affectée par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="be6d8-716">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="be6d8-717">La méthode `OnGet` dans *_Host. cshtml. cs* rend persistante la culture dans un cookie dans le cadre de la réponse.</span><span class="sxs-lookup"><span data-stu-id="be6d8-717">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="be6d8-718">Le navigateur ouvre une connexion WebSocket pour créer une session Blazor Server interactive.</span><span class="sxs-lookup"><span data-stu-id="be6d8-718">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="be6d8-719">L’intergiciel de localisation lit le cookie et assigne la culture.</span><span class="sxs-lookup"><span data-stu-id="be6d8-719">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="be6d8-720">La session du serveur de Blazor commence par la culture correcte.</span><span class="sxs-lookup"><span data-stu-id="be6d8-720">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="be6d8-721">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="be6d8-721">Provide UI to choose the culture</span></span>

<span data-ttu-id="be6d8-722">Pour fournir une interface utilisateur permettant à un utilisateur de sélectionner une culture, il est recommandé d’effectuer une *approche basée sur la redirection* .</span><span class="sxs-lookup"><span data-stu-id="be6d8-722">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="be6d8-723">Le processus est similaire à ce qui se produit dans une application Web lorsqu’un utilisateur tente d’accéder à une ressource sécurisée&mdash;l’utilisateur est redirigé vers une page de connexion, puis redirigé vers la ressource d’origine.</span><span class="sxs-lookup"><span data-stu-id="be6d8-723">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="be6d8-724">L’application conserve la culture sélectionnée de l’utilisateur via une redirection vers un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-724">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="be6d8-725">Le contrôleur définit la culture sélectionnée de l’utilisateur dans un cookie et redirige l’utilisateur vers l’URI d’origine.</span><span class="sxs-lookup"><span data-stu-id="be6d8-725">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="be6d8-726">Établissez un point de terminaison HTTP sur le serveur pour définir la culture sélectionnée de l’utilisateur dans un cookie et effectuer la redirection vers l’URI d’origine :</span><span class="sxs-lookup"><span data-stu-id="be6d8-726">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="be6d8-727">Utilisez le résultat de l’action `LocalRedirect` pour empêcher les attaques de redirection ouvertes.</span><span class="sxs-lookup"><span data-stu-id="be6d8-727">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="be6d8-728">Pour plus d’informations, consultez <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-728">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="be6d8-729">Le composant suivant montre un exemple d’exécution de la redirection initiale lorsque l’utilisateur sélectionne une culture :</span><span class="sxs-lookup"><span data-stu-id="be6d8-729">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="be6d8-730">Utiliser les scénarios de localisation .NET dans les applications Blazor</span><span class="sxs-lookup"><span data-stu-id="be6d8-730">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="be6d8-731">Dans Blazor Apps, les scénarios de localisation et de globalisation .NET suivants sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="be6d8-731">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="be6d8-732">. Système de ressources du réseau</span><span class="sxs-lookup"><span data-stu-id="be6d8-732">.NET's resources system</span></span>
* <span data-ttu-id="be6d8-733">Mise en forme des nombres et des dates spécifiques à la culture</span><span class="sxs-lookup"><span data-stu-id="be6d8-733">Culture-specific number and date formatting</span></span>

<span data-ttu-id="be6d8-734">la fonctionnalité de `@bind` de Blazoreffectue une globalisation basée sur la culture actuelle de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="be6d8-734">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="be6d8-735">Pour plus d’informations, consultez la section [liaison de données](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="be6d8-735">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="be6d8-736">Un ensemble limité de scénarios de localisation de ASP.NET Core est actuellement pris en charge :</span><span class="sxs-lookup"><span data-stu-id="be6d8-736">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="be6d8-737">`IStringLocalizer<>` *est pris en charge* dans les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="be6d8-737">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="be6d8-738">la localisation des annotations de données `IHtmlLocalizer<>`, `IViewLocalizer<>`et est ASP.NET Core les scénarios MVC et **non pris en charge** dans les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="be6d8-738">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="be6d8-739">Pour plus d’informations, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="be6d8-739">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="be6d8-740">Images SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="be6d8-740">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="be6d8-741">Étant donné que Blazor restitue le code HTML, les images prises en charge par le navigateur, y compris les images SVG (Scalable Vector Graphics) ( *. svg*), sont prises en charge via la balise `<img>` :</span><span class="sxs-lookup"><span data-stu-id="be6d8-741">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="be6d8-742">De même, les images SVG sont prises en charge dans les règles CSS d’un fichier de feuille de style ( *. CSS*) :</span><span class="sxs-lookup"><span data-stu-id="be6d8-742">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="be6d8-743">Toutefois, le balisage SVG en ligne n’est pas pris en charge dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="be6d8-743">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="be6d8-744">Si vous placez une balise de `<svg>` directement dans un fichier de composant ( *. Razor*), le rendu d’image de base est pris en charge, mais de nombreux scénarios avancés ne sont pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="be6d8-744">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="be6d8-745">Par exemple, les balises `<use>` ne sont pas respectées et `@bind` ne peuvent pas être utilisées avec certaines balises SVG.</span><span class="sxs-lookup"><span data-stu-id="be6d8-745">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="be6d8-746">Nous prévoyons de traiter ces limitations dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="be6d8-746">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be6d8-747">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="be6d8-747">Additional resources</span></span>

* <span data-ttu-id="be6d8-748"><xref:security/blazor/server> &ndash; contient des conseils sur la création d’applications serveur Blazor qui doivent rivaliser avec l’épuisement des ressources.</span><span class="sxs-lookup"><span data-stu-id="be6d8-748"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
