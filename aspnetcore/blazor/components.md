---
title: Créer et utiliser des composants ASP.NET Core Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants Razor, notamment comment lier des données, gérer des événements et gérer des cycles de vie de composant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/components
ms.openlocfilehash: 07e9153ccfdc78d1da57b815d33220f7fa597cc7
ms.sourcegitcommit: 4b00e77f9984ce76356e829cfe7f75f0f61a7a8f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70145736"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="0230b-103">Créer et utiliser des composants ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="0230b-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="0230b-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0230b-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="0230b-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0230b-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0230b-106">Les applications éblouissantes sont créées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="0230b-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="0230b-107">Un composant est un bloc d’interface utilisateur (IU) autonome, tel qu’une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="0230b-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="0230b-108">Un composant comprend le balisage HTML et la logique de traitement requise pour injecter des données ou répondre à des événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="0230b-109">Les composants sont flexibles et légers.</span><span class="sxs-lookup"><span data-stu-id="0230b-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="0230b-110">Elles peuvent être imbriquées, réutilisées et partagées entre les projets.</span><span class="sxs-lookup"><span data-stu-id="0230b-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="0230b-111">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="0230b-111">Component classes</span></span>

<span data-ttu-id="0230b-112">Les composants sont implémentés dans les fichiers de composants [Razor](xref:mvc/views/razor) ( *. Razor*) C# à l’aide d’une combinaison de balises et html.</span><span class="sxs-lookup"><span data-stu-id="0230b-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="0230b-113">Un composant de éblouissant est officiellement désigné sous le terme de *composant Razor*.</span><span class="sxs-lookup"><span data-stu-id="0230b-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="0230b-114">Le nom d’un composant doit commencer par un caractère majuscule.</span><span class="sxs-lookup"><span data-stu-id="0230b-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="0230b-115">Par exemple, *MyCoolComponent. Razor* est valide et *MyCoolComponent. Razor* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="0230b-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="0230b-116">Les composants peuvent être créés à l’aide de l’extension de fichier *. cshtml* tant que les fichiers sont identifiés en tant que `_RazorComponentInclude` fichiers de composant Razor à l’aide de la propriété MSBuild.</span><span class="sxs-lookup"><span data-stu-id="0230b-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="0230b-117">Par exemple, une application qui spécifie que tous les fichiers *. cshtml* sous le dossier *pages* doivent être traités comme des fichiers de composants Razor:</span><span class="sxs-lookup"><span data-stu-id="0230b-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="0230b-118">L’interface utilisateur d’un composant est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="0230b-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="0230b-119">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="0230b-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="0230b-120">Quand une application est compilée, le balisage C# html et la logique de rendu sont convertis en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="0230b-121">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="0230b-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="0230b-122">Les membres de la classe de composants sont définis dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="0230b-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="0230b-123">Dans le `@code` bloc, l’état du composant (propriétés, champs) est spécifié avec des méthodes pour la gestion des événements ou pour la définition d’une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="0230b-124">Plus d’un bloc `@code` est autorisé.</span><span class="sxs-lookup"><span data-stu-id="0230b-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="0230b-125">Dans les préversions précédentes de ASP.net Core 3,0 `@functions` , les blocs étaient utilisés dans les mêmes `@code` buts que les blocs dans les composants Razor.</span><span class="sxs-lookup"><span data-stu-id="0230b-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="0230b-126">`@functions`les blocs continuent de fonctionner dans les composants Razor, mais nous vous `@code` recommandons d’utiliser le bloc dans ASP.net Core 3,0 Preview 6 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="0230b-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="0230b-127">Les membres de composant peuvent être utilisés dans le cadre de la logique de C# rendu du composant à `@`l’aide d’expressions qui commencent par.</span><span class="sxs-lookup"><span data-stu-id="0230b-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="0230b-128">Par exemple, un C# champ est rendu en préfixant `@` le nom du champ.</span><span class="sxs-lookup"><span data-stu-id="0230b-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="0230b-129">L’exemple suivant évalue et affiche:</span><span class="sxs-lookup"><span data-stu-id="0230b-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="0230b-130">`_headingFontStyle`à la valeur de propriété CSS `font-style`pour.</span><span class="sxs-lookup"><span data-stu-id="0230b-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="0230b-131">`_headingText`au contenu de l' `<h1>` élément.</span><span class="sxs-lookup"><span data-stu-id="0230b-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="0230b-132">Une fois le composant restitué initialement, le composant régénère son arborescence de rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="0230b-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="0230b-133">Il compare ensuite la nouvelle arborescence de rendu à la précédente et applique toutes les modifications à la Document Object Model du navigateur (DOM).</span><span class="sxs-lookup"><span data-stu-id="0230b-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="0230b-134">Les composants sont C# des classes ordinaires qui peuvent être placées n’importe où dans un projet.</span><span class="sxs-lookup"><span data-stu-id="0230b-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="0230b-135">Les composants qui produisent des pages Web résident généralement dans le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="0230b-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="0230b-136">Les composants qui ne sont pas des pages sont souvent placés dans le dossier *partagé* ou dans un dossier personnalisé ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="0230b-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="0230b-137">Pour utiliser un dossier personnalisé, ajoutez l’espace de noms du dossier personnalisé au composant parent ou au fichier *_Imports. Razor* de l’application.</span><span class="sxs-lookup"><span data-stu-id="0230b-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="0230b-138">Par exemple, l’espace de noms suivant rend les composants dans un dossier de *composants* disponibles lorsque l’espace `WebApplication`de noms racine de l’application est:</span><span class="sxs-lookup"><span data-stu-id="0230b-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="0230b-139">Intégrer des composants dans des applications Razor Pages et MVC</span><span class="sxs-lookup"><span data-stu-id="0230b-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="0230b-140">Utilisez des composants avec des applications Razor Pages et MVC existantes.</span><span class="sxs-lookup"><span data-stu-id="0230b-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="0230b-141">Il n’est pas nécessaire de réécrire des pages ou des vues existantes pour utiliser des composants Razor.</span><span class="sxs-lookup"><span data-stu-id="0230b-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="0230b-142">Lorsque la page ou la vue est restituée, les composants sont prérendus en même temps.</span><span class="sxs-lookup"><span data-stu-id="0230b-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="0230b-143">Pour afficher un composant à partir d’une page ou d’une `RenderComponentAsync<TComponent>` vue, utilisez la méthode d’assistance HTML:</span><span class="sxs-lookup"><span data-stu-id="0230b-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="0230b-144">Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie.</span><span class="sxs-lookup"><span data-stu-id="0230b-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="0230b-145">Les composants ne peuvent pas utiliser les scénarios spécifiques aux vues et aux pages, tels que les vues partielles et les sections.</span><span class="sxs-lookup"><span data-stu-id="0230b-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="0230b-146">Pour utiliser la logique de la vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="0230b-147">Pour plus d’informations sur la façon dont les composants sont rendus et l’état des composants géré dans les applications côté <xref:blazor/hosting-models> serveur éblouissantes, consultez l’article.</span><span class="sxs-lookup"><span data-stu-id="0230b-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="0230b-148">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="0230b-148">Use components</span></span>

<span data-ttu-id="0230b-149">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="0230b-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="0230b-150">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="0230b-151">La liaison d’attribut respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="0230b-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="0230b-152">Par exemple, `@bind` est valide et `@Bind` n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="0230b-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="0230b-153">Le balisage suivant dans *index. Razor* rend une `HeadingComponent` instance de:</span><span class="sxs-lookup"><span data-stu-id="0230b-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="0230b-154">*Composants/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0230b-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="0230b-155">Si un composant contient un élément HTML avec une première lettre majuscule qui ne correspond pas à un nom de composant, un avertissement est émis pour indiquer que l’élément a un nom inattendu.</span><span class="sxs-lookup"><span data-stu-id="0230b-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="0230b-156">L’ajout `@using` d’une instruction pour l’espace de noms du composant rend le composant disponible, ce qui supprime l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="0230b-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="0230b-157">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="0230b-157">Component parameters</span></span>

<span data-ttu-id="0230b-158">Les composants peuvent avoir des *paramètres de composant*, qui sont définis à l’aide de propriétés publiques `[Parameter]` sur la classe de composant avec l’attribut.</span><span class="sxs-lookup"><span data-stu-id="0230b-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="0230b-159">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="0230b-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="0230b-160">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0230b-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="0230b-161">Dans l’exemple suivant, le `ParentComponent` définit la valeur de la `Title` propriété de `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="0230b-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="0230b-162">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0230b-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="0230b-163">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="0230b-163">Child content</span></span>

<span data-ttu-id="0230b-164">Les composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-164">Components can set the content of another component.</span></span> <span data-ttu-id="0230b-165">Le composant d’affectation fournit le contenu entre les balises qui spécifient le composant récepteur.</span><span class="sxs-lookup"><span data-stu-id="0230b-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="0230b-166">Dans l’exemple suivant, `ChildComponent` a une `ChildContent` propriété qui représente un `RenderFragment`, qui représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="0230b-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="0230b-167">La valeur de `ChildContent` est positionnée dans le balisage du composant où le contenu doit être rendu.</span><span class="sxs-lookup"><span data-stu-id="0230b-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="0230b-168">La valeur de `ChildContent` est reçue du composant parent et rendue à l’intérieur du panneau de `panel-body`démarrage.</span><span class="sxs-lookup"><span data-stu-id="0230b-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="0230b-169">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0230b-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="0230b-170">La propriété qui reçoit `RenderFragment` le contenu doit être `ChildContent` nommée par Convention.</span><span class="sxs-lookup"><span data-stu-id="0230b-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="0230b-171">Les éléments `ParentComponent` suivants peuvent fournir du contenu pour `ChildComponent` le rendu de en plaçant le `<ChildComponent>` contenu à l’intérieur des balises.</span><span class="sxs-lookup"><span data-stu-id="0230b-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="0230b-172">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0230b-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="0230b-173">Réprojection d’attribut et paramètres arbitraires</span><span class="sxs-lookup"><span data-stu-id="0230b-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="0230b-174">Les composants peuvent capturer et restituer des attributs supplémentaires en plus des paramètres déclarés du composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="0230b-175">Des attributs supplémentaires peuvent être capturés dans un dictionnaire , puis réintégrés à un élément lorsque le composant est rendu [@attributes](xref:mvc/views/razor#attributes) à l’aide de la directive Razor.</span><span class="sxs-lookup"><span data-stu-id="0230b-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="0230b-176">Ce scénario est utile lors de la définition d’un composant qui produit un élément de balisage qui prend en charge diverses personnalisations.</span><span class="sxs-lookup"><span data-stu-id="0230b-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="0230b-177">Par exemple, il peut être fastidieux de définir des attributs séparément pour `<input>` un qui prend en charge de nombreux paramètres.</span><span class="sxs-lookup"><span data-stu-id="0230b-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="0230b-178">Dans l’exemple suivant, le premier `<input>` élément (`id="useIndividualParams"`) utilise des paramètres de composant individuels, tandis que`id="useAttributesDict"`le deuxième `<input>` élément () utilise la projection d’attributs:</span><span class="sxs-lookup"><span data-stu-id="0230b-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
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

<span data-ttu-id="0230b-179">Le type du paramètre doit implémenter `IEnumerable<KeyValuePair<string, object>>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="0230b-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="0230b-180">L' `IReadOnlyDictionary<string, object>` utilisation de est également une option dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="0230b-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="0230b-181">Les éléments `<input>` rendus à l’aide des deux approches sont identiques:</span><span class="sxs-lookup"><span data-stu-id="0230b-181">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="0230b-182">Pour accepter des attributs arbitraires, définissez un paramètre de `[Parameter]` composant à l' `CaptureUnmatchedValues` aide de l' `true`attribut avec la propriété définie sur:</span><span class="sxs-lookup"><span data-stu-id="0230b-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="0230b-183">La `CaptureUnmatchedValues` propriété sur `[Parameter]` permet au paramètre de correspondre à tous les attributs qui ne correspondent à aucun autre paramètre.</span><span class="sxs-lookup"><span data-stu-id="0230b-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="0230b-184">Un composant ne peut définir qu’un seul paramètre `CaptureUnmatchedValues`avec.</span><span class="sxs-lookup"><span data-stu-id="0230b-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="0230b-185">Le type de propriété utilisé `CaptureUnmatchedValues` avec doit pouvoir être assigné `Dictionary<string, object>` à partir de avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="0230b-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="0230b-186">`IEnumerable<KeyValuePair<string, object>>`ou `IReadOnlyDictionary<string, object>` sont également des options dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="0230b-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="0230b-187">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="0230b-187">Data binding</span></span>

<span data-ttu-id="0230b-188">La liaison de données aux composants et aux éléments DOM s’effectue [@bind](xref:mvc/views/razor#bind) à l’aide de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="0230b-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="0230b-189">L’exemple suivant lie le `_italicsCheck` champ à l’état activé de la case à cocher:</span><span class="sxs-lookup"><span data-stu-id="0230b-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="0230b-190">Lorsque la case à cocher est activée et désactivée, la valeur de `true` la `false`propriété est mise à jour à et, respectivement.</span><span class="sxs-lookup"><span data-stu-id="0230b-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="0230b-191">La case à cocher est mise à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, et non en réponse à la modification de la valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="0230b-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="0230b-192">Étant donné que les composants s’affichent après l’exécution du code du gestionnaire d’événements, les mises à jour de propriétés sont généralement reflétées dans l’interface utilisateur immédiatement.</span><span class="sxs-lookup"><span data-stu-id="0230b-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="0230b-193">L' `@bind` utilisation de `CurrentValue` avec une`<input @bind="CurrentValue" />`propriété () équivaut essentiellement à ce qui suit:</span><span class="sxs-lookup"><span data-stu-id="0230b-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="0230b-194">Lors du rendu du composant, le `value` de l’élément d’entrée provient de `CurrentValue` la propriété.</span><span class="sxs-lookup"><span data-stu-id="0230b-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="0230b-195">Lorsque l’utilisateur tape dans la zone de texte, `onchange` l’événement est déclenché et `CurrentValue` la propriété est définie sur la valeur modifiée.</span><span class="sxs-lookup"><span data-stu-id="0230b-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="0230b-196">En réalité, la génération de code est un peu plus complexe `@bind` , car gère quelques cas où des conversions de type sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="0230b-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="0230b-197">En principe, `@bind` associe la valeur actuelle d’une expression à `value` un attribut et gère les modifications à l’aide du gestionnaire inscrit.</span><span class="sxs-lookup"><span data-stu-id="0230b-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="0230b-198">En plus de gérer `onchange` les événements `@bind` avec la syntaxe, une propriété ou un champ peut être lié à l’aide [@bind-value](xref:mvc/views/razor#bind) d’autres événements `event` en spécifiant un attribut avec un paramètre ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="0230b-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="0230b-199">L’exemple suivant lie la `CurrentValue` propriété de l' `oninput` événement:</span><span class="sxs-lookup"><span data-stu-id="0230b-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="0230b-200">Contrairement `onchange`à, qui se déclenche lorsque l’élément perd `oninput` le focus, se déclenche lorsque la valeur de la zone de texte change.</span><span class="sxs-lookup"><span data-stu-id="0230b-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="0230b-201">**Globalisation**</span><span class="sxs-lookup"><span data-stu-id="0230b-201">**Globalization**</span></span>

<span data-ttu-id="0230b-202">`@bind`les valeurs sont mises en forme pour être affichées et analysées à l’aide des règles de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="0230b-202">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="0230b-203">La culture actuelle est accessible à partir de <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> la propriété.</span><span class="sxs-lookup"><span data-stu-id="0230b-203">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="0230b-204">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) est utilisé pour les types de champ suivants`<input type="{TYPE}" />`():</span><span class="sxs-lookup"><span data-stu-id="0230b-204">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="0230b-205">Les types de champ précédents:</span><span class="sxs-lookup"><span data-stu-id="0230b-205">The preceding field types:</span></span>

* <span data-ttu-id="0230b-206">Sont affichés à l’aide des règles de mise en forme appropriées basées sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-206">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="0230b-207">Ne peut pas contenir de texte de forme libre.</span><span class="sxs-lookup"><span data-stu-id="0230b-207">Can't contain free-form text.</span></span>
* <span data-ttu-id="0230b-208">Fournir des caractéristiques d’interaction de l’utilisateur en fonction de l’implémentation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-208">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="0230b-209">Les types de champs suivants ont des exigences de mise en forme spécifiques et ne sont pas actuellement pris en charge par éblouissant, car ils ne sont pas pris en charge par tous les principaux navigateurs:</span><span class="sxs-lookup"><span data-stu-id="0230b-209">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="0230b-210">`@bind`prend en `@bind:culture` charge le paramètre pour <xref:System.Globalization.CultureInfo?displayProperty=fullName> fournir un pour l’analyse et la mise en forme d’une valeur.</span><span class="sxs-lookup"><span data-stu-id="0230b-210">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="0230b-211">La `date` spécification d’une culture n’est pas recommandée `number` lors de l’utilisation des types de champ et.</span><span class="sxs-lookup"><span data-stu-id="0230b-211">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="0230b-212">`date`et `number` disposent d’une prise en charge intégrée de éblouissant qui fournit la culture requise.</span><span class="sxs-lookup"><span data-stu-id="0230b-212">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="0230b-213">Pour plus d’informations sur la façon de définir la culture de l'utilisateur, consultez la section [localization](#localization).</span><span class="sxs-lookup"><span data-stu-id="0230b-213">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="0230b-214">**Chaînes de format**</span><span class="sxs-lookup"><span data-stu-id="0230b-214">**Format strings**</span></span>

<span data-ttu-id="0230b-215">La liaison de données <xref:System.DateTime> fonctionne avec les [@bind:format](xref:mvc/views/razor#bind)chaînes de format à l’aide de.</span><span class="sxs-lookup"><span data-stu-id="0230b-215">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="0230b-216">D’autres expressions de mise en forme, telles que les formats de devise ou de nombre, ne sont pas disponibles pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="0230b-216">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="0230b-217">Dans le code précédent, le `<input>` type de champ de l'`type`élément () est `text`défini par défaut sur.</span><span class="sxs-lookup"><span data-stu-id="0230b-217">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="0230b-218">`@bind:format`est pris en charge pour la liaison des types .NET suivants:</span><span class="sxs-lookup"><span data-stu-id="0230b-218">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="0230b-219"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="0230b-219"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="0230b-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="0230b-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="0230b-221">L' `@bind:format` attribut spécifie le format de date à appliquer `value` au de `<input>` l’élément.</span><span class="sxs-lookup"><span data-stu-id="0230b-221">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="0230b-222">Le format est également utilisé pour analyser la valeur lorsqu’un `onchange` événement se produit.</span><span class="sxs-lookup"><span data-stu-id="0230b-222">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="0230b-223">Il n’est pas recommandé `date` de spécifier un format pour le type de champ, car éblouissant offre une prise en charge intégrée de la mise en forme des dates.</span><span class="sxs-lookup"><span data-stu-id="0230b-223">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="0230b-224">**Paramètres du composant**</span><span class="sxs-lookup"><span data-stu-id="0230b-224">**Component parameters**</span></span>

<span data-ttu-id="0230b-225">La liaison reconnaît les paramètres du `@bind-{property}` composant, où peut lier une valeur de propriété à plusieurs composants.</span><span class="sxs-lookup"><span data-stu-id="0230b-225">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="0230b-226">Le composant enfant suivant (`ChildComponent`) a un `Year` paramètre de composant `YearChanged` et un rappel:</span><span class="sxs-lookup"><span data-stu-id="0230b-226">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="0230b-227">`EventCallback<T>`est expliqué dans la section [EventCallback suivante](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="0230b-227">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="0230b-228">Le composant parent suivant utilise `ChildComponent` et lie le `ParentYear` paramètre `Year` du parent au paramètre sur le composant enfant:</span><span class="sxs-lookup"><span data-stu-id="0230b-228">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
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

<span data-ttu-id="0230b-229">Le chargement `ParentComponent` de génère le balisage suivant:</span><span class="sxs-lookup"><span data-stu-id="0230b-229">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="0230b-230">Si la valeur de la `ParentYear` propriété est modifiée en sélectionnant le bouton dans `ParentComponent`le `ChildComponent` , `Year` la propriété de est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="0230b-230">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="0230b-231">La nouvelle valeur de `Year` est rendue dans l’interface utilisateur lorsque `ParentComponent` le est restitué à nouveau:</span><span class="sxs-lookup"><span data-stu-id="0230b-231">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="0230b-232">Le `Year` paramètre peut être lié, car il a un `YearChanged` événement auxiliaire qui `Year` correspond au type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="0230b-232">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="0230b-233">Par Convention, `<ChildComponent @bind-Year="ParentYear" />` est essentiellement équivalent à l’écriture:</span><span class="sxs-lookup"><span data-stu-id="0230b-233">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="0230b-234">En général, une propriété peut être liée à un gestionnaire d’événements correspondant `@bind-property:event` à l’aide de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="0230b-234">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="0230b-235">Par exemple, la propriété `MyProp` peut être liée à `MyEventHandler` à l’aide des deux attributs suivants:</span><span class="sxs-lookup"><span data-stu-id="0230b-235">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="0230b-236">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="0230b-236">Event handling</span></span>

<span data-ttu-id="0230b-237">Les composants Razor fournissent des fonctionnalités de gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="0230b-237">Razor components provide event handling features.</span></span> <span data-ttu-id="0230b-238">Pour un attribut d’élément HTML `on{event}` nommé (par exemple `onclick` , `onsubmit`et) avec une valeur typée de type délégué, les composants Razor traitent la valeur de l’attribut en tant que gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="0230b-238">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="0230b-239">Le nom de l’attribut est toujours mis en forme [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="0230b-239">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="0230b-240">Le code suivant appelle la `UpdateHeading` méthode lorsque le bouton est sélectionné dans l’interface utilisateur:</span><span class="sxs-lookup"><span data-stu-id="0230b-240">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="0230b-241">Le code suivant appelle la `CheckChanged` méthode lorsque la case à cocher est modifiée dans l’interface utilisateur:</span><span class="sxs-lookup"><span data-stu-id="0230b-241">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="0230b-242">Les gestionnaires d’événements peuvent également être asynchrones et <xref:System.Threading.Tasks.Task>retourner un.</span><span class="sxs-lookup"><span data-stu-id="0230b-242">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="0230b-243">Il n’est pas nécessaire d’appeler `StateHasChanged()`manuellement.</span><span class="sxs-lookup"><span data-stu-id="0230b-243">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="0230b-244">Les exceptions sont journalisées lorsqu’elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="0230b-244">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="0230b-245">Dans l’exemple suivant, `UpdateHeading` est appelé de façon asynchrone quand le bouton est sélectionné:</span><span class="sxs-lookup"><span data-stu-id="0230b-245">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="0230b-246">Types d’arguments d’événement</span><span class="sxs-lookup"><span data-stu-id="0230b-246">Event argument types</span></span>

<span data-ttu-id="0230b-247">Pour certains événements, les types d’arguments d’événement sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="0230b-247">For some events, event argument types are permitted.</span></span> <span data-ttu-id="0230b-248">Si l’accès à l’un de ces types d’événements n’est pas nécessaire, il n’est pas obligatoire dans l’appel de la méthode.</span><span class="sxs-lookup"><span data-stu-id="0230b-248">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="0230b-249">Les [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) prises en charge sont indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0230b-249">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="0230b-250">Événement</span><span class="sxs-lookup"><span data-stu-id="0230b-250">Event</span></span> | <span data-ttu-id="0230b-251">Classe</span><span class="sxs-lookup"><span data-stu-id="0230b-251">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="0230b-252">Presse-papiers</span><span class="sxs-lookup"><span data-stu-id="0230b-252">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="0230b-253">Déplacez</span><span class="sxs-lookup"><span data-stu-id="0230b-253">Drag</span></span>  | <span data-ttu-id="0230b-254">`UIDragEventArgs`est utilisé pour contenir les données glissées pendant une opération de glisser-déplacer et peut contenir un ou `UIDataTransferItem`plusieurs. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="0230b-254">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="0230b-255">`UIDataTransferItem`représente un élément de données de glissement.</span><span class="sxs-lookup"><span data-stu-id="0230b-255">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="0230b-256">Error</span><span class="sxs-lookup"><span data-stu-id="0230b-256">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="0230b-257">Focus</span><span class="sxs-lookup"><span data-stu-id="0230b-257">Focus</span></span> | <span data-ttu-id="0230b-258">`UIFocusEventArgs`N’inclut pas la prise en charge de `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="0230b-258">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="0230b-259">Modification de`<input>`</span><span class="sxs-lookup"><span data-stu-id="0230b-259">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="0230b-260">Clavier</span><span class="sxs-lookup"><span data-stu-id="0230b-260">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="0230b-261">Souris</span><span class="sxs-lookup"><span data-stu-id="0230b-261">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="0230b-262">Pointeur de la souris</span><span class="sxs-lookup"><span data-stu-id="0230b-262">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="0230b-263">Roulette de la souris</span><span class="sxs-lookup"><span data-stu-id="0230b-263">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="0230b-264">Progression</span><span class="sxs-lookup"><span data-stu-id="0230b-264">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="0230b-265">Entrées tactiles</span><span class="sxs-lookup"><span data-stu-id="0230b-265">Touch</span></span> | <span data-ttu-id="0230b-266">`UITouchEventArgs`&ndash; représenteunpointdecontactunique`UITouchPoint` sur un appareil tactile.</span><span class="sxs-lookup"><span data-stu-id="0230b-266">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="0230b-267">Pour plus d’informations sur les propriétés et le comportement de gestion des événements des événements du tableau précédent, consultez [classes EventArgs dans la source de référence (ASPNET/AspNetCore Release/3.0-preview9 Branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src).</span><span class="sxs-lookup"><span data-stu-id="0230b-267">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0-preview9 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="0230b-268">Expressions lambda</span><span class="sxs-lookup"><span data-stu-id="0230b-268">Lambda expressions</span></span>

<span data-ttu-id="0230b-269">Les expressions lambda peuvent également être utilisées:</span><span class="sxs-lookup"><span data-stu-id="0230b-269">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="0230b-270">Il est souvent pratique de se rapprocher de valeurs supplémentaires, par exemple lors de l’itération sur un ensemble d’éléments.</span><span class="sxs-lookup"><span data-stu-id="0230b-270">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="0230b-271">L’exemple suivant crée trois boutons, chacun d’entre eux `UpdateHeading` qui passe un argument d'`UIMouseEventArgs`événement () et son numéro`buttonNumber`de bouton () lorsqu’ils sont sélectionnés dans l’interface utilisateur:</span><span class="sxs-lookup"><span data-stu-id="0230b-271">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="0230b-272">N’utilisez **pas** la variable de boucle`i`() dans `for` une boucle directement dans une expression lambda.</span><span class="sxs-lookup"><span data-stu-id="0230b-272">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="0230b-273">Dans le cas contraire, la même variable est utilisée par `i`toutes les expressions lambda, ce qui signifie que la valeur de est identique dans toutes les expressions lambda.</span><span class="sxs-lookup"><span data-stu-id="0230b-273">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="0230b-274">Capturez toujours sa valeur dans une variable locale`buttonNumber` (dans l’exemple précédent), puis utilisez-la.</span><span class="sxs-lookup"><span data-stu-id="0230b-274">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="0230b-275">EventCallback suivante</span><span class="sxs-lookup"><span data-stu-id="0230b-275">EventCallback</span></span>

<span data-ttu-id="0230b-276">Un scénario courant avec des composants imbriqués est le désir d’exécuter la méthode d’un composant parent lorsqu’un événement de&mdash;composant enfant se produit, `onclick` par exemple, lorsqu’un événement se produit dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="0230b-276">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="0230b-277">Pour exposer des événements entre les composants, `EventCallback`utilisez un.</span><span class="sxs-lookup"><span data-stu-id="0230b-277">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="0230b-278">Un composant parent peut affecter une méthode de rappel à un composant `EventCallback`enfant.</span><span class="sxs-lookup"><span data-stu-id="0230b-278">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="0230b-279">L `ChildComponent` 'de l’exemple d’application montre comment un `onclick` gestionnaire de bouton est configuré pour recevoir `EventCallback` un délégué à partir de `ParentComponent`l’exemple de.</span><span class="sxs-lookup"><span data-stu-id="0230b-279">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="0230b-280">Le `EventCallback` est typé avec `UIMouseEventArgs`, ce qui est approprié pour `onclick` un événement à partir d’un périphérique:</span><span class="sxs-lookup"><span data-stu-id="0230b-280">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="0230b-281">Définit la méthode`ShowMessage` de `EventCallback<T>`l’enfant: `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="0230b-281">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="0230b-282">Lorsque le bouton est sélectionné dans le `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="0230b-282">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="0230b-283">La `ParentComponent`méthode `ShowMessage` de est appelée.</span><span class="sxs-lookup"><span data-stu-id="0230b-283">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="0230b-284">`messageText`est mis à jour et affiché `ParentComponent`dans le.</span><span class="sxs-lookup"><span data-stu-id="0230b-284">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="0230b-285">Un appel à `StateHasChanged` n’est pas requis dans la méthode du`ShowMessage`rappel ().</span><span class="sxs-lookup"><span data-stu-id="0230b-285">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="0230b-286">`StateHasChanged`est appelé automatiquement pour rerestituer `ParentComponent`le, tout comme les événements enfants déclenchent le rerendu des composants dans les gestionnaires d’événements qui s’exécutent dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="0230b-286">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="0230b-287">`EventCallback`et `EventCallback<T>` autorisent les délégués asynchrones.</span><span class="sxs-lookup"><span data-stu-id="0230b-287">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="0230b-288">`EventCallback<T>`est fortement typé et requiert un type d’argument spécifique.</span><span class="sxs-lookup"><span data-stu-id="0230b-288">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="0230b-289">`EventCallback`est faiblement typé et autorise tout type d’argument.</span><span class="sxs-lookup"><span data-stu-id="0230b-289">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="0230b-290">`InvokeAsync` <xref:System.Threading.Tasks.Task>Appelez ou avec`EventCallback<T>` et en attente de: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="0230b-290">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="0230b-291">Utilisez `EventCallback` et`EventCallback<T>` pour la gestion des événements et les paramètres de composant de liaison.</span><span class="sxs-lookup"><span data-stu-id="0230b-291">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="0230b-292">Préférez le fortement typé `EventCallback<T>`. `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="0230b-292">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="0230b-293">`EventCallback<T>`fournit un meilleur retour d’erreur aux utilisateurs du composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-293">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="0230b-294">Comme pour d’autres gestionnaires d’événements d’interface utilisateur, la spécification du paramètre d’événement est facultative.</span><span class="sxs-lookup"><span data-stu-id="0230b-294">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="0230b-295">Utilisez `EventCallback` quand aucune valeur n’est passée au rappel.</span><span class="sxs-lookup"><span data-stu-id="0230b-295">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="0230b-296">Capturer des références à des composants</span><span class="sxs-lookup"><span data-stu-id="0230b-296">Capture references to components</span></span>

<span data-ttu-id="0230b-297">Les références de composant offrent un moyen de référencer une instance de composant afin que vous puissiez émettre des commandes vers `Show` cette `Reset`instance, telles que ou.</span><span class="sxs-lookup"><span data-stu-id="0230b-297">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="0230b-298">Pour capturer une référence de composant:</span><span class="sxs-lookup"><span data-stu-id="0230b-298">To capture a component reference:</span></span>

* <span data-ttu-id="0230b-299">Ajoutez un [@ref](xref:mvc/views/razor#ref) attribut au composant enfant.</span><span class="sxs-lookup"><span data-stu-id="0230b-299">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="0230b-300">Définissez un champ avec le même type que le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="0230b-300">Define a field with the same type as the child component.</span></span>
* <span data-ttu-id="0230b-301">Fournissez `@ref:suppressField` le paramètre, qui supprime la génération de champ de sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="0230b-301">Provide the `@ref:suppressField` parameter, which suppresses backing field generation.</span></span> <span data-ttu-id="0230b-302">Pour plus d’informations, consultez Suppression de la [prise en charge @ref du champ de stockage automatique pour dans 3.0.0-preview9](https://github.com/aspnet/Announcements/issues/381).</span><span class="sxs-lookup"><span data-stu-id="0230b-302">For more information, see [Removing automatic backing field support for @ref in 3.0.0-preview9](https://github.com/aspnet/Announcements/issues/381).</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" @ref:suppressField ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="0230b-303">Lors du rendu du composant, `loginDialog` le champ est rempli avec l’instance du `MyLoginDialog` composant enfant.</span><span class="sxs-lookup"><span data-stu-id="0230b-303">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="0230b-304">Vous pouvez ensuite appeler des méthodes .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-304">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0230b-305">La `loginDialog` variable est remplie uniquement après le rendu du composant et sa sortie comprend l' `MyLoginDialog` élément.</span><span class="sxs-lookup"><span data-stu-id="0230b-305">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="0230b-306">Jusqu’à ce stade, il n’y a rien à référencer.</span><span class="sxs-lookup"><span data-stu-id="0230b-306">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="0230b-307">Pour manipuler des références de composants après la fin du rendu du composant `OnAfterRenderAsync` , `OnAfterRender` utilisez les méthodes ou.</span><span class="sxs-lookup"><span data-stu-id="0230b-307">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<!-- HOLD https://github.com/aspnet/AspNetCore.Docs/pull/13818
Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.

The Razor compiler automatically generates a backing field for element and component references when using [@ref](xref:mvc/views/razor#ref). In the following example, there's no need to create a `myLoginDialog` field for the `LoginDialog` component:

```cshtml
<LoginDialog @ref="myLoginDialog" ... />

@code {
    private void OnSomething()
    {
        myLoginDialog.Show();
    }
}
```

When the component is rendered, the generated `myLoginDialog` field is populated with the `LoginDialog` component instance. You can then invoke .NET methods on the component instance.

In some cases, a backing field is required. For example, declare a backing field when referencing generic components. To suppress backing field generation, specify the `@ref:suppressField` parameter.

> [!IMPORTANT]
> The generated `myLoginDialog` variable is only populated after the component is rendered and its output includes the `LoginDialog` element. Until that point, there's nothing to reference. To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.
-->

<span data-ttu-id="0230b-308">Bien que la capture de références de composant utilise une syntaxe similaire pour [capturer des références d’élément](xref:blazor/javascript-interop#capture-references-to-elements), il ne s’agit pas d’une fonctionnalité [JavaScript Interop](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="0230b-308">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="0230b-309">Les références de composant ne sont pas&mdash;transmises au code JavaScript et ne sont utilisées que dans le code .net.</span><span class="sxs-lookup"><span data-stu-id="0230b-309">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="0230b-310">N’utilisez **pas** de références de composant pour muter l’état des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="0230b-310">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="0230b-311">Utilisez plutôt des paramètres déclaratifs normaux pour passer des données aux composants enfants.</span><span class="sxs-lookup"><span data-stu-id="0230b-311">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="0230b-312">L’utilisation de paramètres déclaratifs normaux entraîne le rerendu automatique des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="0230b-312">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="0230b-313">Utiliser \@la clé pour contrôler la conservation des éléments et des composants</span><span class="sxs-lookup"><span data-stu-id="0230b-313">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="0230b-314">Lors du rendu d’une liste d’éléments ou de composants, et que les éléments ou composants changent par la suite, l’algorithme de différenciation de éblouissant doit décider quel élément ou composant précédent peut être conservé et comment les objets de modèle doivent être mappés à ces éléments.</span><span class="sxs-lookup"><span data-stu-id="0230b-314">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="0230b-315">Normalement, ce processus est automatique et peut être ignoré, mais dans certains cas, il peut être utile de contrôler le processus.</span><span class="sxs-lookup"><span data-stu-id="0230b-315">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="0230b-316">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="0230b-316">Consider the following example:</span></span>

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

<span data-ttu-id="0230b-317">Le contenu de la `People` collection peut changer avec des entrées insérées, supprimées ou réorganisées.</span><span class="sxs-lookup"><span data-stu-id="0230b-317">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="0230b-318">Lors du rerendu du composant, le `<DetailsEditor>` composant peut changer pour recevoir des `Details` valeurs de paramètre différentes.</span><span class="sxs-lookup"><span data-stu-id="0230b-318">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="0230b-319">Cela peut entraîner un rerendu plus complexe que prévu.</span><span class="sxs-lookup"><span data-stu-id="0230b-319">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="0230b-320">Dans certains cas, le rerendu peut entraîner des différences de comportement visibles, telles que le focus de l’élément perdu.</span><span class="sxs-lookup"><span data-stu-id="0230b-320">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="0230b-321">Le processus de mappage peut être contrôlé à `@key` l’aide de l’attribut directive.</span><span class="sxs-lookup"><span data-stu-id="0230b-321">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="0230b-322">`@key`force l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé:</span><span class="sxs-lookup"><span data-stu-id="0230b-322">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="0230b-323">Lorsque la `People` collection est modifiée, l’algorithme de comparaison conserve l’Association `<DetailsEditor>` entre les `person` instances et les instances:</span><span class="sxs-lookup"><span data-stu-id="0230b-323">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="0230b-324">Si un `Person` est supprimé de la `People` liste, seule l’instance `<DetailsEditor>` correspondante est supprimée de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-324">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="0230b-325">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="0230b-325">Other instances are left unchanged.</span></span>
* <span data-ttu-id="0230b-326">Si un `Person` est inséré à une position dans la liste, une nouvelle `<DetailsEditor>` instance est insérée à cette position correspondante.</span><span class="sxs-lookup"><span data-stu-id="0230b-326">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="0230b-327">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="0230b-327">Other instances are left unchanged.</span></span>
* <span data-ttu-id="0230b-328">Si `Person` les entrées sont réordonnées, les `<DetailsEditor>` instances correspondantes sont conservées et réordonnées dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-328">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="0230b-329">Dans certains scénarios, l’utilisation `@key` de réduit la complexité du rerendu et évite les problèmes potentiels liés aux parties avec état de la modification du modèle DOM, telles que la position du focus.</span><span class="sxs-lookup"><span data-stu-id="0230b-329">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0230b-330">Les clés sont locales pour chaque élément ou composant conteneur.</span><span class="sxs-lookup"><span data-stu-id="0230b-330">Keys are local to each container element or component.</span></span> <span data-ttu-id="0230b-331">Les clés ne sont pas comparées globalement dans le document.</span><span class="sxs-lookup"><span data-stu-id="0230b-331">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="0230b-332">Quand utiliser \@la clé</span><span class="sxs-lookup"><span data-stu-id="0230b-332">When to use \@key</span></span>

<span data-ttu-id="0230b-333">En règle générale, il est judicieux `@key` d’utiliser chaque fois qu’une liste est rendue (par `@foreach` exemple, dans un bloc) et qu’une `@key`valeur appropriée existe pour définir.</span><span class="sxs-lookup"><span data-stu-id="0230b-333">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="0230b-334">Vous pouvez également utiliser `@key` pour empêcher éblouissant de conserver un élément ou une sous-arborescence de composants lorsqu’un objet change:</span><span class="sxs-lookup"><span data-stu-id="0230b-334">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="0230b-335">En `@currentPerson` cas de modification `@key` , la directive d’attribut force éblouissant à ignorer `<div>` la totalité et ses descendants et à reconstruire la sous-arborescence de l’interface utilisateur avec de nouveaux éléments et composants.</span><span class="sxs-lookup"><span data-stu-id="0230b-335">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="0230b-336">Cela peut être utile si vous devez garantir qu’aucun État d’interface utilisateur n’est `@currentPerson` préservé lorsque des modifications sont apportées.</span><span class="sxs-lookup"><span data-stu-id="0230b-336">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="0230b-337">Quand ne pas utiliser \@la clé</span><span class="sxs-lookup"><span data-stu-id="0230b-337">When not to use \@key</span></span>

<span data-ttu-id="0230b-338">Il y a un coût en matière de performances `@key`lors de la comparaison avec.</span><span class="sxs-lookup"><span data-stu-id="0230b-338">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="0230b-339">Le coût des performances n’est pas important, `@key` mais spécifiez uniquement si les règles de conservation des éléments ou des composants bénéficient de l’application.</span><span class="sxs-lookup"><span data-stu-id="0230b-339">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="0230b-340">Même si `@key` n’est pas utilisé, éblouissant conserve autant que possible les instances d’élément et de composant enfants.</span><span class="sxs-lookup"><span data-stu-id="0230b-340">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="0230b-341">Le seul avantage de l' `@key` utilisation de est de contrôler la *façon dont* les instances de modèle sont mappées aux instances de composant conservées, au lieu de l’algorithme de comparaison qui sélectionne le mappage.</span><span class="sxs-lookup"><span data-stu-id="0230b-341">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="0230b-342">Valeurs à utiliser pour \@la clé</span><span class="sxs-lookup"><span data-stu-id="0230b-342">What values to use for \@key</span></span>

<span data-ttu-id="0230b-343">En règle générale, il est logique de fournir l’un des types de valeur `@key`suivants pour:</span><span class="sxs-lookup"><span data-stu-id="0230b-343">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="0230b-344">Instances d’objet de modèle (par exemple `Person` , une instance comme dans l’exemple précédent).</span><span class="sxs-lookup"><span data-stu-id="0230b-344">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="0230b-345">Cela garantit la préservation en fonction de l’égalité des références d’objet.</span><span class="sxs-lookup"><span data-stu-id="0230b-345">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="0230b-346">Identificateurs uniques (par exemple, les valeurs de clé primaire `int`de `string`type, `Guid`ou).</span><span class="sxs-lookup"><span data-stu-id="0230b-346">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="0230b-347">Vérifiez que les valeurs utilisées `@key` pour ne sont pas en conflit.</span><span class="sxs-lookup"><span data-stu-id="0230b-347">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="0230b-348">Si les valeurs en conflit sont détectées dans le même élément parent, éblouissant lève une exception, car il ne peut pas mapper de manière déterministe les anciens éléments ou composants aux nouveaux éléments ou composants.</span><span class="sxs-lookup"><span data-stu-id="0230b-348">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="0230b-349">Utilisez uniquement des valeurs distinctes, telles que des instances d’objets ou des valeurs de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="0230b-349">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="0230b-350">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="0230b-350">Lifecycle methods</span></span>

<span data-ttu-id="0230b-351">`OnInitializedAsync`et `OnInitialized` exécuter le code pour initialiser le composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-351">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="0230b-352">Pour effectuer une opération asynchrone, utilisez `OnInitializedAsync` et le `await` mot clé sur l’opération:</span><span class="sxs-lookup"><span data-stu-id="0230b-352">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="0230b-353">Pour une opération synchrone, utilisez `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="0230b-353">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="0230b-354">`OnParametersSetAsync`et `OnParametersSet` sont appelées lorsqu’un composant a reçu des paramètres de son parent et que les valeurs sont assignées aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="0230b-354">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="0230b-355">Ces méthodes sont exécutées après l’initialisation du composant et chaque fois que le composant est rendu:</span><span class="sxs-lookup"><span data-stu-id="0230b-355">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="0230b-356">`OnAfterRenderAsync`et `OnAfterRender` sont appelées après la fin d’un rendu d’un composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-356">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="0230b-357">Les références d’élément et de composant sont remplies à ce stade.</span><span class="sxs-lookup"><span data-stu-id="0230b-357">Element and component references are populated at this point.</span></span> <span data-ttu-id="0230b-358">Utilisez cette étape pour effectuer des étapes d’initialisation supplémentaires à l’aide du contenu rendu, par exemple l’activation de bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.</span><span class="sxs-lookup"><span data-stu-id="0230b-358">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="0230b-359">Gérer les actions asynchrones incomplètes au rendu</span><span class="sxs-lookup"><span data-stu-id="0230b-359">Handle incomplete async actions at render</span></span>

<span data-ttu-id="0230b-360">Les actions asynchrones exécutées dans des événements de cycle de vie peuvent ne pas être terminées avant le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-360">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="0230b-361">Les objets peuvent `null` être ou être remplis de façon incomplète avec des données pendant l’exécution de la méthode Lifecycle.</span><span class="sxs-lookup"><span data-stu-id="0230b-361">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="0230b-362">Fournissez une logique de rendu pour confirmer que les objets sont initialisés.</span><span class="sxs-lookup"><span data-stu-id="0230b-362">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="0230b-363">Affichez les éléments d’interface utilisateur d’espace réservé (par exemple, un `null`message de chargement) tandis que les objets sont.</span><span class="sxs-lookup"><span data-stu-id="0230b-363">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="0230b-364">Dans le `FetchData` composant des modèles éblouissant, `OnInitializedAsync` est remplacé par asynchrone recevoir les données de prévision (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="0230b-364">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="0230b-365">Lorsque `forecasts` a `null`la valeur, un message de chargement est affiché à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-365">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="0230b-366">Une fois `Task` la `OnInitializedAsync` retournée terminée, le composant est rerendu avec l’État mis à jour.</span><span class="sxs-lookup"><span data-stu-id="0230b-366">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="0230b-367">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="0230b-367">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="0230b-368">Exécuter du code avant la définition des paramètres</span><span class="sxs-lookup"><span data-stu-id="0230b-368">Execute code before parameters are set</span></span>

<span data-ttu-id="0230b-369">`SetParameters`peut être substitué pour exécuter du code avant que les paramètres soient définis:</span><span class="sxs-lookup"><span data-stu-id="0230b-369">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="0230b-370">Si `base.SetParameters` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants de la façon requise.</span><span class="sxs-lookup"><span data-stu-id="0230b-370">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="0230b-371">Par exemple, il n’est pas nécessaire que les paramètres entrants soient assignés aux propriétés de la classe.</span><span class="sxs-lookup"><span data-stu-id="0230b-371">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="0230b-372">Supprimer l’actualisation de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="0230b-372">Suppress refreshing of the UI</span></span>

<span data-ttu-id="0230b-373">`ShouldRender`peut être substitué pour supprimer l’actualisation de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-373">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="0230b-374">Si l’implémentation retourne `true`, l’interface utilisateur est actualisée.</span><span class="sxs-lookup"><span data-stu-id="0230b-374">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="0230b-375">Même si `ShouldRender` est substitué, le composant est toujours restitué initialement.</span><span class="sxs-lookup"><span data-stu-id="0230b-375">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="0230b-376">Suppression de composant avec IDisposable</span><span class="sxs-lookup"><span data-stu-id="0230b-376">Component disposal with IDisposable</span></span>

<span data-ttu-id="0230b-377">Si un composant implémente <xref:System.IDisposable>, la [méthode dispose](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-377">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="0230b-378">Le composant suivant utilise `@implements IDisposable` et la `Dispose` méthode:</span><span class="sxs-lookup"><span data-stu-id="0230b-378">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="0230b-379">Routage</span><span class="sxs-lookup"><span data-stu-id="0230b-379">Routing</span></span>

<span data-ttu-id="0230b-380">Le routage dans éblouissant est obtenu en fournissant un modèle de routage à chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="0230b-380">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="0230b-381">Lorsqu’un fichier Razor avec une `@page` directive est compilé, la classe générée reçoit un qui <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifie le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="0230b-381">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="0230b-382">Lors de l’exécution, le routeur recherche les classes de `RouteAttribute` composant avec un et rend le composant qui a un modèle de routage correspondant à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="0230b-382">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="0230b-383">Plusieurs modèles de routage peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-383">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="0230b-384">Le composant suivant répond aux demandes pour `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="0230b-384">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="0230b-385">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="0230b-385">Route parameters</span></span>

<span data-ttu-id="0230b-386">Les composants peuvent recevoir des paramètres de routage du modèle de routage `@page` fourni dans la directive.</span><span class="sxs-lookup"><span data-stu-id="0230b-386">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="0230b-387">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="0230b-387">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="0230b-388">*Composant de paramètre de routage*:</span><span class="sxs-lookup"><span data-stu-id="0230b-388">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="0230b-389">Les paramètres facultatifs ne sont pas `@page` pris en charge. deux directives sont donc appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="0230b-389">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="0230b-390">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="0230b-390">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="0230b-391">La deuxième `@page` directive prend le `{text}` paramètre d’itinéraire et assigne la valeur à `Text` la propriété.</span><span class="sxs-lookup"><span data-stu-id="0230b-391">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="0230b-392">Héritage de la classe de base pour une expérience «code-behind»</span><span class="sxs-lookup"><span data-stu-id="0230b-392">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="0230b-393">Les fichiers de composants associent C# le balisage HTML et le code de traitement dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="0230b-393">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="0230b-394">La `@inherits` directive peut être utilisée pour fournir des applications éblouissantes avec une expérience «code-behind» qui sépare le balisage de composant du code de traitement.</span><span class="sxs-lookup"><span data-stu-id="0230b-394">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="0230b-395">L' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) montre comment un composant peut hériter d’une `BlazorRocksBase`classe de base,, pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-395">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="0230b-396">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="0230b-396">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="0230b-397">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="0230b-397">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="0230b-398">La classe de base doit dériver de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="0230b-398">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="0230b-399">Importer des composants</span><span class="sxs-lookup"><span data-stu-id="0230b-399">Import components</span></span>

<span data-ttu-id="0230b-400">L’espace de noms d’un composant créé avec Razor est basé sur:</span><span class="sxs-lookup"><span data-stu-id="0230b-400">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="0230b-401">Du `RootNamespace`projet.</span><span class="sxs-lookup"><span data-stu-id="0230b-401">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="0230b-402">Chemin d’accès de la racine du projet au composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-402">The path from the project root to the component.</span></span> <span data-ttu-id="0230b-403">Par exemple, `ComponentsSample/Pages/Index.razor` se trouve dans l' `ComponentsSample.Pages`espace de noms.</span><span class="sxs-lookup"><span data-stu-id="0230b-403">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="0230b-404">Les composants C# suivent les règles de liaison de nom.</span><span class="sxs-lookup"><span data-stu-id="0230b-404">Components follow C# name binding rules.</span></span> <span data-ttu-id="0230b-405">Dans le cas de *index. Razor*, tous les composants dans le même dossier, les mêmes *pages*et le dossier parent, *ComponentsSample*, sont dans l’étendue.</span><span class="sxs-lookup"><span data-stu-id="0230b-405">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="0230b-406">Les composants définis dans un espace de noms différent peuvent être mis en portée à l’aide de la directive [ \@using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="0230b-406">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="0230b-407">Si un autre composant `NavMenu.razor`,, existe dans le `ComponentsSample/Shared/`dossier, le composant peut être utilisé `Index.razor` dans avec l' `@using` instruction suivante:</span><span class="sxs-lookup"><span data-stu-id="0230b-407">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="0230b-408">Les composants peuvent également être référencés à l’aide de leurs noms complets, ce qui supprime [ \@](xref:mvc/views/razor#using) la nécessité de la directive using:</span><span class="sxs-lookup"><span data-stu-id="0230b-408">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="0230b-409">La `global::` qualification n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="0230b-409">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="0230b-410">L’importation de composants avec `using` des instructions avec alias ( `@using Foo = Bar`par exemple,) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="0230b-410">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="0230b-411">Les noms partiellement qualifiés ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="0230b-411">Partially qualified names aren't supported.</span></span> <span data-ttu-id="0230b-412">Par exemple, l' `@using ComponentsSample` ajout et `NavMenu.razor` la `<Shared.NavMenu></Shared.NavMenu>` référencement avec ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="0230b-412">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="0230b-413">Attributs d’éléments HTML conditionnels</span><span class="sxs-lookup"><span data-stu-id="0230b-413">Conditional HTML element attributes</span></span>

<span data-ttu-id="0230b-414">Les attributs des éléments HTML sont rendus de manière conditionnelle en fonction de la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="0230b-414">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="0230b-415">Si la valeur est `false` ou `null`, l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="0230b-415">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="0230b-416">Si la valeur est `true`, l’attribut est rendu réduit.</span><span class="sxs-lookup"><span data-stu-id="0230b-416">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="0230b-417">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est rendu dans le balisage de l’élément:</span><span class="sxs-lookup"><span data-stu-id="0230b-417">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="0230b-418">Si `IsCompleted` est`true`, la case à cocher s’affiche comme suit:</span><span class="sxs-lookup"><span data-stu-id="0230b-418">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="0230b-419">Si `IsCompleted` est`false`, la case à cocher s’affiche comme suit:</span><span class="sxs-lookup"><span data-stu-id="0230b-419">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="0230b-420">Pour plus d'informations, consultez <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="0230b-420">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="0230b-421">HTML brut</span><span class="sxs-lookup"><span data-stu-id="0230b-421">Raw HTML</span></span>

<span data-ttu-id="0230b-422">Les chaînes sont normalement rendues à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage qu’elles peuvent contenir est ignorée et traitée comme du texte littéral.</span><span class="sxs-lookup"><span data-stu-id="0230b-422">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="0230b-423">Pour afficher le code HTML brut, encapsulez le `MarkupString` contenu HTML dans une valeur.</span><span class="sxs-lookup"><span data-stu-id="0230b-423">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="0230b-424">La valeur est analysée au format HTML ou SVG et est insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="0230b-424">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="0230b-425">Le rendu de code HTML brut construit à partir de n’importe quelle source non approuvée constitue un risque pour la **sécurité** et doit être évité!</span><span class="sxs-lookup"><span data-stu-id="0230b-425">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="0230b-426">L’exemple suivant illustre l’utilisation `MarkupString` du type pour ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant:</span><span class="sxs-lookup"><span data-stu-id="0230b-426">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="0230b-427">Composants basés sur un modèle</span><span class="sxs-lookup"><span data-stu-id="0230b-427">Templated components</span></span>

<span data-ttu-id="0230b-428">Les composants basés sur un modèle sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, qui peuvent ensuite être utilisés dans le cadre de la logique de rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-428">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="0230b-429">Les composants basés sur un modèle vous permettent de créer des composants de niveau supérieur qui sont plus réutilisables que les composants normaux.</span><span class="sxs-lookup"><span data-stu-id="0230b-429">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="0230b-430">Voici quelques exemples:</span><span class="sxs-lookup"><span data-stu-id="0230b-430">A couple of examples include:</span></span>

* <span data-ttu-id="0230b-431">Composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête, les lignes et le pied de page de la table.</span><span class="sxs-lookup"><span data-stu-id="0230b-431">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="0230b-432">Composant de liste qui permet à un utilisateur de spécifier un modèle pour afficher des éléments dans une liste.</span><span class="sxs-lookup"><span data-stu-id="0230b-432">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="0230b-433">Paramètres de modèle</span><span class="sxs-lookup"><span data-stu-id="0230b-433">Template parameters</span></span>

<span data-ttu-id="0230b-434">Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres `RenderFragment` de `RenderFragment<T>`composant de type ou.</span><span class="sxs-lookup"><span data-stu-id="0230b-434">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="0230b-435">Un fragment de rendu représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="0230b-435">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="0230b-436">`RenderFragment<T>`prend un paramètre de type qui peut être spécifié lors de l’appel du fragment de rendu.</span><span class="sxs-lookup"><span data-stu-id="0230b-436">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="0230b-437">`TableTemplate`-</span><span class="sxs-lookup"><span data-stu-id="0230b-437">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="0230b-438">Lorsque vous utilisez un composant basé sur un modèle, les paramètres de modèle peuvent être spécifiés à l’aide d’éléments enfants`TableHeader` qui `RowTemplate` correspondent aux noms des paramètres (et dans l’exemple suivant):</span><span class="sxs-lookup"><span data-stu-id="0230b-438">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
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

### <a name="template-context-parameters"></a><span data-ttu-id="0230b-439">Paramètres de contexte de modèle</span><span class="sxs-lookup"><span data-stu-id="0230b-439">Template context parameters</span></span>

<span data-ttu-id="0230b-440">Les arguments de composant `RenderFragment<T>` de type passé comme éléments ont un paramètre `context` implicite nommé (par exemple, à partir `@context.PetId`de l’exemple de code précédent,), mais vous `Context` pouvez modifier le nom de paramètre à l’aide de l’attribut sur l’enfant. appartient.</span><span class="sxs-lookup"><span data-stu-id="0230b-440">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="0230b-441">Dans l’exemple suivant, l' `RowTemplate` attribut de `Context` l’élément spécifie le `pet` paramètre:</span><span class="sxs-lookup"><span data-stu-id="0230b-441">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
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

<span data-ttu-id="0230b-442">Vous pouvez également spécifier l' `Context` attribut sur l’élément de composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-442">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="0230b-443">L’attribut `Context` spécifié s’applique à tous les paramètres de modèle spécifiés.</span><span class="sxs-lookup"><span data-stu-id="0230b-443">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="0230b-444">Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans élément enfant d’encapsulation).</span><span class="sxs-lookup"><span data-stu-id="0230b-444">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="0230b-445">Dans l’exemple suivant, l' `Context` attribut apparaît sur l' `TableTemplate` élément et s’applique à tous les paramètres de modèle:</span><span class="sxs-lookup"><span data-stu-id="0230b-445">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

### <a name="generic-typed-components"></a><span data-ttu-id="0230b-446">Composants génériques</span><span class="sxs-lookup"><span data-stu-id="0230b-446">Generic-typed components</span></span>

<span data-ttu-id="0230b-447">Les composants basés sur un modèle sont souvent typés de façon générique.</span><span class="sxs-lookup"><span data-stu-id="0230b-447">Templated components are often generically typed.</span></span> <span data-ttu-id="0230b-448">Par exemple, un composant `ListViewTemplate` générique peut être utilisé pour restituer `IEnumerable<T>` des valeurs.</span><span class="sxs-lookup"><span data-stu-id="0230b-448">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="0230b-449">Pour définir un composant générique, utilisez la `@typeparam` directive pour spécifier les paramètres de type:</span><span class="sxs-lookup"><span data-stu-id="0230b-449">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="0230b-450">Lorsque vous utilisez des composants génériques, le paramètre de type est déduit si possible:</span><span class="sxs-lookup"><span data-stu-id="0230b-450">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="0230b-451">Sinon, le paramètre de type doit être spécifié explicitement à l’aide d’un attribut qui correspond au nom du paramètre de type.</span><span class="sxs-lookup"><span data-stu-id="0230b-451">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="0230b-452">Dans l’exemple suivant, `TItem="Pet"` spécifie le type:</span><span class="sxs-lookup"><span data-stu-id="0230b-452">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="0230b-453">Valeurs et paramètres en cascade</span><span class="sxs-lookup"><span data-stu-id="0230b-453">Cascading values and parameters</span></span>

<span data-ttu-id="0230b-454">Dans certains scénarios, il est peu commode de transmettre des données d’un composant ancêtre à un composant descendant à l’aide de [paramètres de composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-454">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="0230b-455">Les valeurs et paramètres en cascade résolvent ce problème en fournissant un moyen pratique pour un composant ancêtre de fournir une valeur à tous ses composants descendants.</span><span class="sxs-lookup"><span data-stu-id="0230b-455">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="0230b-456">Les valeurs et les paramètres en cascade fournissent également une approche pour les composants à coordonner.</span><span class="sxs-lookup"><span data-stu-id="0230b-456">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="0230b-457">Exemple de thème</span><span class="sxs-lookup"><span data-stu-id="0230b-457">Theme example</span></span>

<span data-ttu-id="0230b-458">Dans l’exemple suivant tiré de l’exemple d’application `ThemeInfo` , la classe spécifie les informations de thème pour descendre dans la hiérarchie des composants, de sorte que tous les boutons d’une partie donnée de l’application partagent le même style.</span><span class="sxs-lookup"><span data-stu-id="0230b-458">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="0230b-459">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="0230b-459">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="0230b-460">Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="0230b-460">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="0230b-461">Le `CascadingValue` composant encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants de cette sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="0230b-461">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="0230b-462">Par exemple, l’exemple d’application spécifie les`ThemeInfo`informations de thème () dans l’une des dispositions de l’application en tant que paramètre en cascade pour tous les composants qui composent le corps de la disposition de la `@Body` propriété.</span><span class="sxs-lookup"><span data-stu-id="0230b-462">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="0230b-463">`ButtonClass`la valeur `btn-success` est affectée à dans le composant Layout.</span><span class="sxs-lookup"><span data-stu-id="0230b-463">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="0230b-464">Tout composant descendant peut consommer cette propriété par le `ThemeInfo` biais de l’objet en cascade.</span><span class="sxs-lookup"><span data-stu-id="0230b-464">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="0230b-465">`CascadingValuesParametersLayout`-</span><span class="sxs-lookup"><span data-stu-id="0230b-465">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="0230b-466">Pour utiliser des valeurs en cascade, les composants déclarent des paramètres en cascade `[CascadingParameter]` à l’aide de l’attribut ou d’une valeur de nom de chaîne:</span><span class="sxs-lookup"><span data-stu-id="0230b-466">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="0230b-467">La liaison avec une valeur de nom de chaîne est pertinente si vous avez plusieurs valeurs en cascade du même type et que vous devez les différencier dans la même sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="0230b-467">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="0230b-468">Les valeurs en cascade sont liées aux paramètres en cascade par type.</span><span class="sxs-lookup"><span data-stu-id="0230b-468">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="0230b-469">Dans l’exemple d’application, `CascadingValuesParametersTheme` le composant lie la `ThemeInfo` valeur en cascade à un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="0230b-469">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="0230b-470">Le paramètre est utilisé pour définir la classe CSS pour l’un des boutons affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-470">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="0230b-471">`CascadingValuesParametersTheme`-</span><span class="sxs-lookup"><span data-stu-id="0230b-471">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

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
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="0230b-472">Exemple TabSet</span><span class="sxs-lookup"><span data-stu-id="0230b-472">TabSet example</span></span>

<span data-ttu-id="0230b-473">Les paramètres en cascade permettent également aux composants de collaborer au sein de la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="0230b-473">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="0230b-474">Par exemple, considérez l’exemple *TabSet* suivant dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="0230b-474">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="0230b-475">L’exemple d’application possède `ITab` une interface qui implémente les onglets suivants:</span><span class="sxs-lookup"><span data-stu-id="0230b-475">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="0230b-476">Le `CascadingValuesParametersTabSet` composant utilise le `TabSet` composant, qui contient plusieurs `Tab` composants:</span><span class="sxs-lookup"><span data-stu-id="0230b-476">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="0230b-477">Les composants `Tab` enfants ne sont pas explicitement passés comme paramètres `TabSet`à.</span><span class="sxs-lookup"><span data-stu-id="0230b-477">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="0230b-478">Au lieu de cela `Tab` , les composants enfants font partie du contenu enfant `TabSet`de.</span><span class="sxs-lookup"><span data-stu-id="0230b-478">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="0230b-479">Toutefois, le `TabSet` doit toujours connaître chaque `Tab` composant pour pouvoir afficher les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter de code supplémentaire `TabSet` , le composant *peut se présenter comme une valeur en cascade* qui est ensuite récupérée par les `Tab` composants descendants.</span><span class="sxs-lookup"><span data-stu-id="0230b-479">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="0230b-480">`TabSet`-</span><span class="sxs-lookup"><span data-stu-id="0230b-480">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="0230b-481">Les composants `Tab` descendants capturent `TabSet` le contenant comme paramètre en cascade, de `Tab` sorte que les composants s' `TabSet` ajoutent eux-mêmes à la et à la coordonnée sur laquelle l’onglet est actif.</span><span class="sxs-lookup"><span data-stu-id="0230b-481">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="0230b-482">`Tab`-</span><span class="sxs-lookup"><span data-stu-id="0230b-482">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="0230b-483">Modèles Razor</span><span class="sxs-lookup"><span data-stu-id="0230b-483">Razor templates</span></span>

<span data-ttu-id="0230b-484">Les fragments de rendu peuvent être définis à l’aide de la syntaxe de modèle Razor.</span><span class="sxs-lookup"><span data-stu-id="0230b-484">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="0230b-485">Les modèles Razor sont un moyen de définir un extrait de code d’interface utilisateur et de supposer le format suivant:</span><span class="sxs-lookup"><span data-stu-id="0230b-485">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="0230b-486">L’exemple suivant montre comment spécifier `RenderFragment` des valeurs et `RenderFragment<T>` et restituer des modèles directement dans un composant.</span><span class="sxs-lookup"><span data-stu-id="0230b-486">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="0230b-487">Les fragments de rendu peuvent également être passés comme arguments à des [composants basés](#templated-components)sur un modèle.</span><span class="sxs-lookup"><span data-stu-id="0230b-487">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="0230b-488">Sortie rendue du code précédent:</span><span class="sxs-lookup"><span data-stu-id="0230b-488">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="0230b-489">Logique RenderTreeBuilder manuelle</span><span class="sxs-lookup"><span data-stu-id="0230b-489">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="0230b-490">`Microsoft.AspNetCore.Components.RenderTree`fournit des méthodes pour manipuler des composants et des éléments, y compris la C# génération manuelle de composants dans le code.</span><span class="sxs-lookup"><span data-stu-id="0230b-490">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="0230b-491">L’utilisation `RenderTreeBuilder` de pour créer des composants est un scénario avancé.</span><span class="sxs-lookup"><span data-stu-id="0230b-491">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="0230b-492">Un composant mal formé (par exemple, une balise de balisage non fermée) peut entraîner un comportement indéfini.</span><span class="sxs-lookup"><span data-stu-id="0230b-492">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="0230b-493">Prenons le composant `PetDetails` suivant, qui peut être intégré manuellement à un autre composant:</span><span class="sxs-lookup"><span data-stu-id="0230b-493">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="0230b-494">Dans l’exemple suivant, la boucle de la `CreateComponent` méthode génère trois `PetDetails` composants.</span><span class="sxs-lookup"><span data-stu-id="0230b-494">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="0230b-495">Lors de `RenderTreeBuilder` l’appel de méthodes pour créer`OpenComponent` les `AddAttribute`composants (et), les numéros de séquence sont des numéros de ligne de code source.</span><span class="sxs-lookup"><span data-stu-id="0230b-495">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="0230b-496">L’algorithme de différence éblouissant s’appuie sur les numéros de séquence correspondant à des lignes de code distinctes, et non sur des appels d’appel distincts.</span><span class="sxs-lookup"><span data-stu-id="0230b-496">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="0230b-497">Lors de la création d' `RenderTreeBuilder` un composant avec des méthodes, coder en dur les arguments pour les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="0230b-497">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="0230b-498">**L’utilisation d’un calcul ou d’un compteur pour générer le numéro de séquence peut entraîner des performances médiocres.**</span><span class="sxs-lookup"><span data-stu-id="0230b-498">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="0230b-499">Pour plus d’informations, consultez la section [numéros de séquence liés à des numéros de ligne de code et non à un ordre d’exécution](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="0230b-499">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="0230b-500">`BuiltContent`-</span><span class="sxs-lookup"><span data-stu-id="0230b-500">`BuiltContent` component:</span></span>

```cshtml
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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="0230b-501">Les numéros de séquence sont liés aux numéros de ligne de code et non à l’ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="0230b-501">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="0230b-502">`.razor` Les fichiers éblouissants sont toujours compilés.</span><span class="sxs-lookup"><span data-stu-id="0230b-502">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="0230b-503">C’est un avantage considérable pour, `.razor` car l’étape de compilation peut être utilisée pour injecter des informations qui améliorent les performances de l’application au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0230b-503">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="0230b-504">Un exemple clé de ces améliorations concerne les *numéros de séquence*.</span><span class="sxs-lookup"><span data-stu-id="0230b-504">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="0230b-505">Les numéros de séquence indiquent au runtime que les sorties proviennent de lignes de code distinctes et ordonnées.</span><span class="sxs-lookup"><span data-stu-id="0230b-505">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="0230b-506">Le runtime utilise ces informations pour générer des différences d’arborescence efficaces en temps linéaire, ce qui est beaucoup plus rapide qu’un algorithme de comparaison d’arborescence générale.</span><span class="sxs-lookup"><span data-stu-id="0230b-506">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="0230b-507">Considérons le fichier `.razor` simple suivant:</span><span class="sxs-lookup"><span data-stu-id="0230b-507">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="0230b-508">Le code précédent se compile comme suit:</span><span class="sxs-lookup"><span data-stu-id="0230b-508">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="0230b-509">Lorsque le code s’exécute pour la première fois, si `someFlag` est `true`, le générateur reçoit:</span><span class="sxs-lookup"><span data-stu-id="0230b-509">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="0230b-510">Séquence</span><span class="sxs-lookup"><span data-stu-id="0230b-510">Sequence</span></span> | <span data-ttu-id="0230b-511">Type</span><span class="sxs-lookup"><span data-stu-id="0230b-511">Type</span></span>      | <span data-ttu-id="0230b-512">Données</span><span class="sxs-lookup"><span data-stu-id="0230b-512">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="0230b-513">0</span><span class="sxs-lookup"><span data-stu-id="0230b-513">0</span></span>        | <span data-ttu-id="0230b-514">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="0230b-514">Text node</span></span> | <span data-ttu-id="0230b-515">Première</span><span class="sxs-lookup"><span data-stu-id="0230b-515">First</span></span>  |
| <span data-ttu-id="0230b-516">1</span><span class="sxs-lookup"><span data-stu-id="0230b-516">1</span></span>        | <span data-ttu-id="0230b-517">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="0230b-517">Text node</span></span> | <span data-ttu-id="0230b-518">Seconde</span><span class="sxs-lookup"><span data-stu-id="0230b-518">Second</span></span> |

<span data-ttu-id="0230b-519">Imaginez que `someFlag` devient `false`et le balisage est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="0230b-519">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="0230b-520">Cette fois-ci, le générateur reçoit:</span><span class="sxs-lookup"><span data-stu-id="0230b-520">This time, the builder receives:</span></span>

| <span data-ttu-id="0230b-521">Séquence</span><span class="sxs-lookup"><span data-stu-id="0230b-521">Sequence</span></span> | <span data-ttu-id="0230b-522">Type</span><span class="sxs-lookup"><span data-stu-id="0230b-522">Type</span></span>       | <span data-ttu-id="0230b-523">Données</span><span class="sxs-lookup"><span data-stu-id="0230b-523">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="0230b-524">1</span><span class="sxs-lookup"><span data-stu-id="0230b-524">1</span></span>        | <span data-ttu-id="0230b-525">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="0230b-525">Text node</span></span>  | <span data-ttu-id="0230b-526">Seconde</span><span class="sxs-lookup"><span data-stu-id="0230b-526">Second</span></span> |

<span data-ttu-id="0230b-527">Quand le runtime effectue une comparaison, il constate que l’élément au niveau `0` de la séquence a été supprimé. il génère donc le *script d’édition*trivial suivant:</span><span class="sxs-lookup"><span data-stu-id="0230b-527">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="0230b-528">Supprimez le premier nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="0230b-528">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="0230b-529">Qu’est-ce qui se passe si vous générez des numéros séquentiels par programmation</span><span class="sxs-lookup"><span data-stu-id="0230b-529">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="0230b-530">Imaginez plutôt que vous avez écrit la logique de générateur d’arborescence de rendu suivante:</span><span class="sxs-lookup"><span data-stu-id="0230b-530">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="0230b-531">La première sortie est désormais:</span><span class="sxs-lookup"><span data-stu-id="0230b-531">Now, the first output is:</span></span>

| <span data-ttu-id="0230b-532">Séquence</span><span class="sxs-lookup"><span data-stu-id="0230b-532">Sequence</span></span> | <span data-ttu-id="0230b-533">Type</span><span class="sxs-lookup"><span data-stu-id="0230b-533">Type</span></span>      | <span data-ttu-id="0230b-534">Données</span><span class="sxs-lookup"><span data-stu-id="0230b-534">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="0230b-535">0</span><span class="sxs-lookup"><span data-stu-id="0230b-535">0</span></span>        | <span data-ttu-id="0230b-536">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="0230b-536">Text node</span></span> | <span data-ttu-id="0230b-537">Première</span><span class="sxs-lookup"><span data-stu-id="0230b-537">First</span></span>  |
| <span data-ttu-id="0230b-538">1</span><span class="sxs-lookup"><span data-stu-id="0230b-538">1</span></span>        | <span data-ttu-id="0230b-539">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="0230b-539">Text node</span></span> | <span data-ttu-id="0230b-540">Seconde</span><span class="sxs-lookup"><span data-stu-id="0230b-540">Second</span></span> |

<span data-ttu-id="0230b-541">Ce résultat est identique au cas précédent, donc aucun problème négatif n’existe.</span><span class="sxs-lookup"><span data-stu-id="0230b-541">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="0230b-542">`someFlag`se `false` trouve sur le deuxième rendu et la sortie est:</span><span class="sxs-lookup"><span data-stu-id="0230b-542">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="0230b-543">Séquence</span><span class="sxs-lookup"><span data-stu-id="0230b-543">Sequence</span></span> | <span data-ttu-id="0230b-544">Type</span><span class="sxs-lookup"><span data-stu-id="0230b-544">Type</span></span>      | <span data-ttu-id="0230b-545">Données</span><span class="sxs-lookup"><span data-stu-id="0230b-545">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="0230b-546">0</span><span class="sxs-lookup"><span data-stu-id="0230b-546">0</span></span>        | <span data-ttu-id="0230b-547">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="0230b-547">Text node</span></span> | <span data-ttu-id="0230b-548">Seconde</span><span class="sxs-lookup"><span data-stu-id="0230b-548">Second</span></span> |

<span data-ttu-id="0230b-549">Cette fois-ci, l’algorithme diff constate que *deux* modifications ont été apportées, et l’algorithme génère le script Edit suivant:</span><span class="sxs-lookup"><span data-stu-id="0230b-549">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="0230b-550">Remplacez la valeur du premier nœud de texte par `Second`.</span><span class="sxs-lookup"><span data-stu-id="0230b-550">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="0230b-551">Supprimez le deuxième nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="0230b-551">Remove the second text node.</span></span>

<span data-ttu-id="0230b-552">La génération des numéros de séquence a perdu toutes les informations utiles sur `if/else` l’emplacement où les branches et les boucles étaient présentes dans le code d’origine.</span><span class="sxs-lookup"><span data-stu-id="0230b-552">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="0230b-553">Il en résulte une comparaison de **deux fois plus longtemps** qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="0230b-553">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="0230b-554">Il s’agit d’un exemple trivial.</span><span class="sxs-lookup"><span data-stu-id="0230b-554">This is a trivial example.</span></span> <span data-ttu-id="0230b-555">Dans des cas plus réalistes avec des structures complexes et profondément imbriquées, et surtout avec des boucles, le coût des performances est plus grave.</span><span class="sxs-lookup"><span data-stu-id="0230b-555">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="0230b-556">Au lieu d’identifier immédiatement les blocs de boucle ou les branches qui ont été insérées ou supprimées, l’algorithme diff doit recurse profondément dans les arborescences de rendu et générer des scripts de modification beaucoup plus longs, car il est mal informé de la façon dont les anciennes et les nouvelles structures sont liées les unes aux autres.</span><span class="sxs-lookup"><span data-stu-id="0230b-556">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="0230b-557">Conseils et conclusions</span><span class="sxs-lookup"><span data-stu-id="0230b-557">Guidance and conclusions</span></span>

* <span data-ttu-id="0230b-558">Les performances de l’application sont affectées si les numéros séquentiels sont générés dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="0230b-558">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="0230b-559">L’infrastructure ne peut pas créer ses propres numéros de séquence automatiquement au moment de l’exécution, car les informations nécessaires n’existent pas, sauf si elles sont capturées au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="0230b-559">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="0230b-560">N’écrivez pas de longs blocs de logique `RenderTreeBuilder` implémentée manuellement.</span><span class="sxs-lookup"><span data-stu-id="0230b-560">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="0230b-561">Préférer `.razor` les fichiers et permettre au compilateur de gérer les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="0230b-561">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="0230b-562">Si les numéros de séquence sont codés en dur, l’algorithme diff exige uniquement que les numéros de séquence augmentent dans la valeur.</span><span class="sxs-lookup"><span data-stu-id="0230b-562">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="0230b-563">La valeur initiale et les écarts ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="0230b-563">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="0230b-564">Une option légitime consiste à utiliser le numéro de ligne de code comme numéro de séquence, ou à commencer à partir de zéro et à augmenter par des ou des centaines (ou un intervalle de préférence).</span><span class="sxs-lookup"><span data-stu-id="0230b-564">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="0230b-565">Éblouissant utilise des numéros de séquence, tandis que d’autres infrastructures d’interface utilisateur de comparaison d’arborescence ne les utilisent pas.</span><span class="sxs-lookup"><span data-stu-id="0230b-565">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="0230b-566">La comparaison est beaucoup plus rapide lorsque les numéros de séquence sont utilisés, et éblouissant présente l’avantage d’une étape de compilation qui traite automatiquement les numéros séquentiels `.razor` pour les développeurs qui créent des fichiers.</span><span class="sxs-lookup"><span data-stu-id="0230b-566">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="0230b-567">Localisation</span><span class="sxs-lookup"><span data-stu-id="0230b-567">Localization</span></span>

<span data-ttu-id="0230b-568">Les applications côté serveur éblouissantes sont localisées à l’aide d’un [intergiciel (middleware](xref:fundamentals/localization#localization-middleware)) de localisation.</span><span class="sxs-lookup"><span data-stu-id="0230b-568">Blazor server-side apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="0230b-569">L’intergiciel sélectionne la culture appropriée pour les utilisateurs qui demandent des ressources à partir de l’application.</span><span class="sxs-lookup"><span data-stu-id="0230b-569">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="0230b-570">La culture peut être définie à l’aide de l’une des approches suivantes:</span><span class="sxs-lookup"><span data-stu-id="0230b-570">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="0230b-571">Cookies</span><span class="sxs-lookup"><span data-stu-id="0230b-571">Cookies</span></span>](#cookies)
* [<span data-ttu-id="0230b-572">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="0230b-572">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="0230b-573">Pour plus d’informations et d’exemples, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="0230b-573">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="0230b-574">Cookies</span><span class="sxs-lookup"><span data-stu-id="0230b-574">Cookies</span></span>

<span data-ttu-id="0230b-575">Un cookie de culture de localisation peut conserver la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-575">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="0230b-576">Le cookie est créé par la `OnGet` méthode de la page hôte de l’application (*pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="0230b-576">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="0230b-577">L’intergiciel (middleware) de localisation lit le cookie sur les demandes suivantes pour définir la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-577">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="0230b-578">L’utilisation d’un cookie garantit que la connexion WebSocket peut propager correctement la culture.</span><span class="sxs-lookup"><span data-stu-id="0230b-578">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="0230b-579">Si les schémas de localisation sont basés sur le chemin d’URL ou la chaîne de requête, il est possible que le schéma ne soit pas en mesure de fonctionner avec les WebSockets, et donc de ne pas conserver la culture.</span><span class="sxs-lookup"><span data-stu-id="0230b-579">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="0230b-580">Par conséquent, l’utilisation d’un cookie de culture de localisation est l’approche recommandée.</span><span class="sxs-lookup"><span data-stu-id="0230b-580">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="0230b-581">Toute technique peut être utilisée pour assigner une culture si la culture est rendue persistante dans un cookie de localisation.</span><span class="sxs-lookup"><span data-stu-id="0230b-581">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="0230b-582">Si l’application a déjà un schéma de localisation établi pour les ASP.NET Core côté serveur, continuez à utiliser l’infrastructure de localisation existante de l’application et à définir le cookie de la culture de localisation dans le schéma de l’application.</span><span class="sxs-lookup"><span data-stu-id="0230b-582">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="0230b-583">L’exemple suivant montre comment définir la culture actuelle dans un cookie qui peut être lu par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="0230b-583">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="0230b-584">Créez un fichier *pages/Host. cshtml. cs* avec le contenu suivant dans l’application côté serveur éblouissant:</span><span class="sxs-lookup"><span data-stu-id="0230b-584">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor server-side app:</span></span>

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

<span data-ttu-id="0230b-585">La localisation est gérée dans l’application:</span><span class="sxs-lookup"><span data-stu-id="0230b-585">Localization is handled in the app:</span></span>

1. <span data-ttu-id="0230b-586">Le navigateur envoie une requête HTTP initiale à l’application.</span><span class="sxs-lookup"><span data-stu-id="0230b-586">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="0230b-587">La culture est affectée par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="0230b-587">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="0230b-588">La `OnGet` méthode dans *_Host. cshtml. cs* rend persistante la culture dans un cookie dans le cadre de la réponse.</span><span class="sxs-lookup"><span data-stu-id="0230b-588">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="0230b-589">Le navigateur ouvre une connexion WebSocket pour créer une session du serveur éblouissante interactive.</span><span class="sxs-lookup"><span data-stu-id="0230b-589">The browser opens a WebSocket connection to create an interactive Blazor server-side session.</span></span>
1. <span data-ttu-id="0230b-590">L’intergiciel de localisation lit le cookie et assigne la culture.</span><span class="sxs-lookup"><span data-stu-id="0230b-590">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="0230b-591">La session éblouissante côté serveur commence par la culture correcte.</span><span class="sxs-lookup"><span data-stu-id="0230b-591">The Blazor server-side session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="0230b-592">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="0230b-592">Provide UI to choose the culture</span></span>

<span data-ttu-id="0230b-593">Pour fournir une interface utilisateur permettant à un utilisateur de sélectionner une culture, il est recommandé d’effectuer une *approche basée sur* la redirection.</span><span class="sxs-lookup"><span data-stu-id="0230b-593">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="0230b-594">Le processus est similaire à ce qui se produit dans une application Web lorsqu’un utilisateur tente d’accéder à&mdash;une ressource sécurisée. l’utilisateur est redirigé vers une page de connexion, puis redirigé vers la ressource d’origine.</span><span class="sxs-lookup"><span data-stu-id="0230b-594">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="0230b-595">L’application conserve la culture sélectionnée de l’utilisateur via une redirection vers un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0230b-595">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="0230b-596">Le contrôleur définit la culture sélectionnée de l’utilisateur dans un cookie et redirige l’utilisateur vers l’URI d’origine.</span><span class="sxs-lookup"><span data-stu-id="0230b-596">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="0230b-597">Établissez un point de terminaison HTTP sur le serveur pour définir la culture sélectionnée de l’utilisateur dans un cookie et effectuer la redirection vers l’URI d’origine:</span><span class="sxs-lookup"><span data-stu-id="0230b-597">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="0230b-598">Utilisez le `LocalRedirect` résultat de l’action pour empêcher les attaques de redirection ouvertes.</span><span class="sxs-lookup"><span data-stu-id="0230b-598">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="0230b-599">Pour plus d'informations, consultez <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="0230b-599">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="0230b-600">Le composant suivant montre un exemple d’exécution de la redirection initiale lorsque l’utilisateur sélectionne une culture:</span><span class="sxs-lookup"><span data-stu-id="0230b-600">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject IUriHelper UriHelper

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(UIChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(UriHelper.GetAbsoluteUri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        UriHelper.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="0230b-601">Utiliser des scénarios de localisation .NET dans des applications éblouissantes</span><span class="sxs-lookup"><span data-stu-id="0230b-601">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="0230b-602">Dans les applications éblouissantes, les scénarios de localisation et de globalisation .NET suivants sont disponibles:</span><span class="sxs-lookup"><span data-stu-id="0230b-602">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="0230b-603">. Système de ressources du réseau</span><span class="sxs-lookup"><span data-stu-id="0230b-603">.NET's resources system</span></span>
* <span data-ttu-id="0230b-604">Mise en forme des nombres et des dates spécifiques à la culture</span><span class="sxs-lookup"><span data-stu-id="0230b-604">Culture-specific number and date formatting</span></span>

<span data-ttu-id="0230b-605">La fonctionnalité de `@bind` éblouissant effectue une globalisation basée sur la culture actuelle de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0230b-605">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="0230b-606">Pour plus d’informations, consultez la section [liaison de données](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="0230b-606">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="0230b-607">Un ensemble limité de scénarios de localisation de ASP.NET Core est actuellement pris en charge:</span><span class="sxs-lookup"><span data-stu-id="0230b-607">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="0230b-608">`IStringLocalizer<>`*est pris en charge* dans les applications éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="0230b-608">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="0230b-609">`IHtmlLocalizer<>`la `IViewLocalizer<>`localisation des annotations de données, et est ASP.net Core les scénarios MVC et **non pris en charge** dans les applications éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="0230b-609">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="0230b-610">Pour plus d'informations, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="0230b-610">For more information, see <xref:fundamentals/localization>.</span></span>
