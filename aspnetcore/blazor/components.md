---
title: Créer et utiliser des composants ASP.NET Core Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants Razor, notamment comment lier des données, gérer des événements et gérer des cycles de vie de composant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/components
ms.openlocfilehash: dbd0879d200061151e8307346adef784967bf123
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878397"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="992c7-103">Créer et utiliser des composants ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="992c7-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="992c7-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="992c7-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="992c7-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="992c7-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="992c7-106">Les applications éblouissantes sont créées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="992c7-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="992c7-107">Un composant est un bloc d’interface utilisateur (IU) autonome, tel qu’une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="992c7-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="992c7-108">Un composant comprend le balisage HTML et la logique de traitement requise pour injecter des données ou répondre à des événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="992c7-109">Les composants sont flexibles et légers.</span><span class="sxs-lookup"><span data-stu-id="992c7-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="992c7-110">Elles peuvent être imbriquées, réutilisées et partagées entre les projets.</span><span class="sxs-lookup"><span data-stu-id="992c7-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="992c7-111">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="992c7-111">Component classes</span></span>

<span data-ttu-id="992c7-112">Les composants sont implémentés dans les fichiers de composants [Razor](xref:mvc/views/razor) ( *. Razor*) C# à l’aide d’une combinaison de balises et html.</span><span class="sxs-lookup"><span data-stu-id="992c7-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="992c7-113">Un composant de éblouissant est officiellement désigné sous le terme de *composant Razor*.</span><span class="sxs-lookup"><span data-stu-id="992c7-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="992c7-114">Le nom d’un composant doit commencer par un caractère majuscule.</span><span class="sxs-lookup"><span data-stu-id="992c7-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="992c7-115">Par exemple, *MyCoolComponent. Razor* est valide et *MyCoolComponent. Razor* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="992c7-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="992c7-116">Les composants peuvent être créés à l’aide de l’extension de fichier *. cshtml* tant que les fichiers sont identifiés en tant que `_RazorComponentInclude` fichiers de composant Razor à l’aide de la propriété MSBuild.</span><span class="sxs-lookup"><span data-stu-id="992c7-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="992c7-117">Par exemple, une application qui spécifie que tous les fichiers *. cshtml* sous le dossier *pages* doivent être traités comme des fichiers de composants Razor :</span><span class="sxs-lookup"><span data-stu-id="992c7-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="992c7-118">L’interface utilisateur d’un composant est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="992c7-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="992c7-119">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="992c7-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="992c7-120">Quand une application est compilée, le balisage C# html et la logique de rendu sont convertis en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="992c7-121">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="992c7-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="992c7-122">Les membres de la classe de composants sont définis dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="992c7-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="992c7-123">Dans le `@code` bloc, l’état du composant (propriétés, champs) est spécifié avec des méthodes pour la gestion des événements ou pour la définition d’une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="992c7-124">Plus d’un bloc `@code` est autorisé.</span><span class="sxs-lookup"><span data-stu-id="992c7-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="992c7-125">Dans les préversions précédentes de ASP.net Core 3,0 `@functions` , les blocs étaient utilisés dans les mêmes `@code` buts que les blocs dans les composants Razor.</span><span class="sxs-lookup"><span data-stu-id="992c7-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="992c7-126">`@functions`les blocs continuent de fonctionner dans les composants Razor, mais nous vous `@code` recommandons d’utiliser le bloc dans ASP.net Core 3,0 Preview 6 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="992c7-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="992c7-127">Les membres de composant peuvent être utilisés dans le cadre de la logique de C# rendu du composant à `@`l’aide d’expressions qui commencent par.</span><span class="sxs-lookup"><span data-stu-id="992c7-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="992c7-128">Par exemple, un C# champ est rendu en préfixant `@` le nom du champ.</span><span class="sxs-lookup"><span data-stu-id="992c7-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="992c7-129">L’exemple suivant évalue et affiche :</span><span class="sxs-lookup"><span data-stu-id="992c7-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="992c7-130">`_headingFontStyle`à la valeur de propriété CSS `font-style`pour.</span><span class="sxs-lookup"><span data-stu-id="992c7-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="992c7-131">`_headingText`au contenu de l' `<h1>` élément.</span><span class="sxs-lookup"><span data-stu-id="992c7-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="992c7-132">Une fois le composant restitué initialement, le composant régénère son arborescence de rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="992c7-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="992c7-133">Il compare ensuite la nouvelle arborescence de rendu à la précédente et applique toutes les modifications à la Document Object Model du navigateur (DOM).</span><span class="sxs-lookup"><span data-stu-id="992c7-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="992c7-134">Les composants sont C# des classes ordinaires qui peuvent être placées n’importe où dans un projet.</span><span class="sxs-lookup"><span data-stu-id="992c7-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="992c7-135">Les composants qui produisent des pages Web résident généralement dans le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="992c7-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="992c7-136">Les composants qui ne sont pas des pages sont souvent placés dans le dossier *partagé* ou dans un dossier personnalisé ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="992c7-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="992c7-137">Pour utiliser un dossier personnalisé, ajoutez l’espace de noms du dossier personnalisé au composant parent ou au fichier *_Imports. Razor* de l’application.</span><span class="sxs-lookup"><span data-stu-id="992c7-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="992c7-138">Par exemple, l’espace de noms suivant rend les composants dans un dossier de *composants* disponibles lorsque l’espace `WebApplication`de noms racine de l’application est :</span><span class="sxs-lookup"><span data-stu-id="992c7-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="992c7-139">Intégrer des composants dans des applications Razor Pages et MVC</span><span class="sxs-lookup"><span data-stu-id="992c7-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="992c7-140">Utilisez des composants avec des applications Razor Pages et MVC existantes.</span><span class="sxs-lookup"><span data-stu-id="992c7-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="992c7-141">Il n’est pas nécessaire de réécrire des pages ou des vues existantes pour utiliser des composants Razor.</span><span class="sxs-lookup"><span data-stu-id="992c7-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="992c7-142">Lorsque la page ou la vue est restituée, les composants sont prérendus en même temps.</span><span class="sxs-lookup"><span data-stu-id="992c7-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="992c7-143">Pour afficher un composant à partir d’une page ou d’une `RenderComponentAsync<TComponent>` vue, utilisez la méthode d’assistance HTML :</span><span class="sxs-lookup"><span data-stu-id="992c7-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="992c7-144">Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie.</span><span class="sxs-lookup"><span data-stu-id="992c7-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="992c7-145">Les composants ne peuvent pas utiliser les scénarios spécifiques aux vues et aux pages, tels que les vues partielles et les sections.</span><span class="sxs-lookup"><span data-stu-id="992c7-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="992c7-146">Pour utiliser la logique de la vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="992c7-147">Pour plus d’informations sur la façon dont les composants sont rendus et l’état des composants géré dans les applications côté <xref:blazor/hosting-models> serveur éblouissantes, consultez l’article.</span><span class="sxs-lookup"><span data-stu-id="992c7-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="992c7-148">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="992c7-148">Use components</span></span>

<span data-ttu-id="992c7-149">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="992c7-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="992c7-150">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="992c7-151">La liaison d’attribut respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="992c7-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="992c7-152">Par exemple, `@bind` est valide et `@Bind` n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="992c7-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="992c7-153">Le balisage suivant dans *index. Razor* rend une `HeadingComponent` instance de :</span><span class="sxs-lookup"><span data-stu-id="992c7-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="992c7-154">*Composants/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="992c7-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="992c7-155">Si un composant contient un élément HTML avec une première lettre majuscule qui ne correspond pas à un nom de composant, un avertissement est émis pour indiquer que l’élément a un nom inattendu.</span><span class="sxs-lookup"><span data-stu-id="992c7-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="992c7-156">L’ajout `@using` d’une instruction pour l’espace de noms du composant rend le composant disponible, ce qui supprime l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="992c7-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="992c7-157">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="992c7-157">Component parameters</span></span>

<span data-ttu-id="992c7-158">Les composants peuvent avoir des *paramètres de composant*, qui sont définis à l’aide de propriétés publiques `[Parameter]` sur la classe de composant avec l’attribut.</span><span class="sxs-lookup"><span data-stu-id="992c7-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="992c7-159">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="992c7-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="992c7-160">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="992c7-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="992c7-161">Dans l’exemple suivant, le `ParentComponent` définit la valeur de la `Title` propriété de `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="992c7-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="992c7-162">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="992c7-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="992c7-163">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="992c7-163">Child content</span></span>

<span data-ttu-id="992c7-164">Les composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-164">Components can set the content of another component.</span></span> <span data-ttu-id="992c7-165">Le composant d’affectation fournit le contenu entre les balises qui spécifient le composant récepteur.</span><span class="sxs-lookup"><span data-stu-id="992c7-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="992c7-166">Dans l’exemple suivant, `ChildComponent` a une `ChildContent` propriété qui représente un `RenderFragment`, qui représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="992c7-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="992c7-167">La valeur de `ChildContent` est positionnée dans le balisage du composant où le contenu doit être rendu.</span><span class="sxs-lookup"><span data-stu-id="992c7-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="992c7-168">La valeur de `ChildContent` est reçue du composant parent et rendue à l’intérieur du panneau de `panel-body`démarrage.</span><span class="sxs-lookup"><span data-stu-id="992c7-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="992c7-169">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="992c7-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="992c7-170">La propriété qui reçoit `RenderFragment` le contenu doit être `ChildContent` nommée par Convention.</span><span class="sxs-lookup"><span data-stu-id="992c7-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="992c7-171">Les éléments `ParentComponent` suivants peuvent fournir du contenu pour `ChildComponent` le rendu de en plaçant le `<ChildComponent>` contenu à l’intérieur des balises.</span><span class="sxs-lookup"><span data-stu-id="992c7-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="992c7-172">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="992c7-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="992c7-173">Réprojection d’attribut et paramètres arbitraires</span><span class="sxs-lookup"><span data-stu-id="992c7-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="992c7-174">Les composants peuvent capturer et restituer des attributs supplémentaires en plus des paramètres déclarés du composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="992c7-175">Des attributs supplémentaires peuvent être capturés dans un dictionnaire *, puis* réintégrés à un élément lorsque le composant est rendu [@attributes](xref:mvc/views/razor#attributes) à l’aide de la directive Razor.</span><span class="sxs-lookup"><span data-stu-id="992c7-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="992c7-176">Ce scénario est utile lors de la définition d’un composant qui produit un élément de balisage qui prend en charge diverses personnalisations.</span><span class="sxs-lookup"><span data-stu-id="992c7-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="992c7-177">Par exemple, il peut être fastidieux de définir des attributs séparément pour `<input>` un qui prend en charge de nombreux paramètres.</span><span class="sxs-lookup"><span data-stu-id="992c7-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="992c7-178">Dans l’exemple suivant, le premier `<input>` élément (`id="useIndividualParams"`) utilise des paramètres de composant individuels, tandis que`id="useAttributesDict"`le deuxième `<input>` élément () utilise la projection d’attributs :</span><span class="sxs-lookup"><span data-stu-id="992c7-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="992c7-179">Le type du paramètre doit implémenter `IEnumerable<KeyValuePair<string, object>>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="992c7-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="992c7-180">L' `IReadOnlyDictionary<string, object>` utilisation de est également une option dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="992c7-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="992c7-181">Les éléments `<input>` rendus à l’aide des deux approches sont identiques :</span><span class="sxs-lookup"><span data-stu-id="992c7-181">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="992c7-182">Pour accepter des attributs arbitraires, définissez un paramètre de `[Parameter]` composant à l' `CaptureUnmatchedValues` aide de l' `true`attribut avec la propriété définie sur :</span><span class="sxs-lookup"><span data-stu-id="992c7-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="992c7-183">La `CaptureUnmatchedValues` propriété sur `[Parameter]` permet au paramètre de correspondre à tous les attributs qui ne correspondent à aucun autre paramètre.</span><span class="sxs-lookup"><span data-stu-id="992c7-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="992c7-184">Un composant ne peut définir qu’un seul paramètre `CaptureUnmatchedValues`avec.</span><span class="sxs-lookup"><span data-stu-id="992c7-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="992c7-185">Le type de propriété utilisé `CaptureUnmatchedValues` avec doit pouvoir être assigné `Dictionary<string, object>` à partir de avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="992c7-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="992c7-186">`IEnumerable<KeyValuePair<string, object>>`ou `IReadOnlyDictionary<string, object>` sont également des options dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="992c7-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="992c7-187">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="992c7-187">Data binding</span></span>

<span data-ttu-id="992c7-188">La liaison de données aux composants et aux éléments DOM s’effectue [@bind](xref:mvc/views/razor#bind) à l’aide de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="992c7-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="992c7-189">L’exemple suivant lie le `_italicsCheck` champ à l’état activé de la case à cocher :</span><span class="sxs-lookup"><span data-stu-id="992c7-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="992c7-190">Lorsque la case à cocher est activée et désactivée, la valeur de `true` la `false`propriété est mise à jour à et, respectivement.</span><span class="sxs-lookup"><span data-stu-id="992c7-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="992c7-191">La case à cocher est mise à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, et non en réponse à la modification de la valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="992c7-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="992c7-192">Étant donné que les composants s’affichent après l’exécution du code du gestionnaire d’événements, les mises à jour de propriétés sont généralement reflétées dans l’interface utilisateur immédiatement.</span><span class="sxs-lookup"><span data-stu-id="992c7-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="992c7-193">L' `@bind` utilisation de `CurrentValue` avec une`<input @bind="CurrentValue" />`propriété () équivaut essentiellement à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="992c7-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="992c7-194">Lors du rendu du composant, le `value` de l’élément d’entrée provient de `CurrentValue` la propriété.</span><span class="sxs-lookup"><span data-stu-id="992c7-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="992c7-195">Lorsque l’utilisateur tape dans la zone de texte, `onchange` l’événement est déclenché et `CurrentValue` la propriété est définie sur la valeur modifiée.</span><span class="sxs-lookup"><span data-stu-id="992c7-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="992c7-196">En réalité, la génération de code est un peu plus complexe `@bind` , car gère quelques cas où des conversions de type sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="992c7-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="992c7-197">En principe, `@bind` associe la valeur actuelle d’une expression à `value` un attribut et gère les modifications à l’aide du gestionnaire inscrit.</span><span class="sxs-lookup"><span data-stu-id="992c7-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="992c7-198">En plus de gérer `onchange` les événements `@bind` avec la syntaxe, une propriété ou un champ peut être lié à l’aide [@bind-value](xref:mvc/views/razor#bind) d’autres événements `event` en spécifiant un attribut avec un paramètre ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="992c7-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="992c7-199">L’exemple suivant lie la `CurrentValue` propriété de l' `oninput` événement :</span><span class="sxs-lookup"><span data-stu-id="992c7-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="992c7-200">Contrairement `onchange`à, qui se déclenche lorsque l’élément perd `oninput` le focus, se déclenche lorsque la valeur de la zone de texte change.</span><span class="sxs-lookup"><span data-stu-id="992c7-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="992c7-201">**Globalisation**</span><span class="sxs-lookup"><span data-stu-id="992c7-201">**Globalization**</span></span>

<span data-ttu-id="992c7-202">`@bind`les valeurs sont mises en forme pour être affichées et analysées à l’aide des règles de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="992c7-202">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="992c7-203">La culture actuelle est accessible à partir de <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> la propriété.</span><span class="sxs-lookup"><span data-stu-id="992c7-203">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="992c7-204">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) est utilisé pour les types de champ suivants`<input type="{TYPE}" />`() :</span><span class="sxs-lookup"><span data-stu-id="992c7-204">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="992c7-205">Les types de champ précédents :</span><span class="sxs-lookup"><span data-stu-id="992c7-205">The preceding field types:</span></span>

* <span data-ttu-id="992c7-206">Sont affichés à l’aide des règles de mise en forme appropriées basées sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-206">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="992c7-207">Ne peut pas contenir de texte de forme libre.</span><span class="sxs-lookup"><span data-stu-id="992c7-207">Can't contain free-form text.</span></span>
* <span data-ttu-id="992c7-208">Fournir des caractéristiques d’interaction de l’utilisateur en fonction de l’implémentation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-208">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="992c7-209">Les types de champs suivants ont des exigences de mise en forme spécifiques et ne sont pas actuellement pris en charge par éblouissant, car ils ne sont pas pris en charge par tous les principaux navigateurs :</span><span class="sxs-lookup"><span data-stu-id="992c7-209">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="992c7-210">`@bind`prend en `@bind:culture` charge le paramètre pour <xref:System.Globalization.CultureInfo?displayProperty=fullName> fournir un pour l’analyse et la mise en forme d’une valeur.</span><span class="sxs-lookup"><span data-stu-id="992c7-210">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="992c7-211">La `date` spécification d’une culture n’est pas recommandée `number` lors de l’utilisation des types de champ et.</span><span class="sxs-lookup"><span data-stu-id="992c7-211">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="992c7-212">`date`et `number` disposent d’une prise en charge intégrée de éblouissant qui fournit la culture requise.</span><span class="sxs-lookup"><span data-stu-id="992c7-212">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="992c7-213">Pour plus d’informations sur la façon de définir la culture de l'utilisateur, consultez la section [localization](#localization).</span><span class="sxs-lookup"><span data-stu-id="992c7-213">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="992c7-214">**Chaînes de format**</span><span class="sxs-lookup"><span data-stu-id="992c7-214">**Format strings**</span></span>

<span data-ttu-id="992c7-215">La liaison de données <xref:System.DateTime> fonctionne avec les [@bind:format](xref:mvc/views/razor#bind)chaînes de format à l’aide de.</span><span class="sxs-lookup"><span data-stu-id="992c7-215">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="992c7-216">D’autres expressions de mise en forme, telles que les formats de devise ou de nombre, ne sont pas disponibles pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="992c7-216">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="992c7-217">Dans le code précédent, le `<input>` type de champ de l'`type`élément () est `text`défini par défaut sur.</span><span class="sxs-lookup"><span data-stu-id="992c7-217">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="992c7-218">`@bind:format`est pris en charge pour la liaison des types .NET suivants :</span><span class="sxs-lookup"><span data-stu-id="992c7-218">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="992c7-219"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="992c7-219"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="992c7-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="992c7-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="992c7-221">L' `@bind:format` attribut spécifie le format de date à appliquer `value` au de `<input>` l’élément.</span><span class="sxs-lookup"><span data-stu-id="992c7-221">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="992c7-222">Le format est également utilisé pour analyser la valeur lorsqu’un `onchange` événement se produit.</span><span class="sxs-lookup"><span data-stu-id="992c7-222">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="992c7-223">Il n’est pas recommandé `date` de spécifier un format pour le type de champ, car éblouissant offre une prise en charge intégrée de la mise en forme des dates.</span><span class="sxs-lookup"><span data-stu-id="992c7-223">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="992c7-224">**Paramètres du composant**</span><span class="sxs-lookup"><span data-stu-id="992c7-224">**Component parameters**</span></span>

<span data-ttu-id="992c7-225">La liaison reconnaît les paramètres du `@bind-{property}` composant, où peut lier une valeur de propriété à plusieurs composants.</span><span class="sxs-lookup"><span data-stu-id="992c7-225">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="992c7-226">Le composant enfant suivant (`ChildComponent`) a un `Year` paramètre de composant `YearChanged` et un rappel :</span><span class="sxs-lookup"><span data-stu-id="992c7-226">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="992c7-227">`EventCallback<T>`est expliqué dans la section [EventCallback suivante](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="992c7-227">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="992c7-228">Le composant parent suivant utilise `ChildComponent` et lie le `ParentYear` paramètre `Year` du parent au paramètre sur le composant enfant :</span><span class="sxs-lookup"><span data-stu-id="992c7-228">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="992c7-229">Le chargement `ParentComponent` de génère le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="992c7-229">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="992c7-230">Si la valeur de la `ParentYear` propriété est modifiée en sélectionnant le bouton dans `ParentComponent`le `ChildComponent` , `Year` la propriété de est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="992c7-230">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="992c7-231">La nouvelle valeur de `Year` est rendue dans l’interface utilisateur lorsque `ParentComponent` le est restitué à nouveau :</span><span class="sxs-lookup"><span data-stu-id="992c7-231">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="992c7-232">Le `Year` paramètre peut être lié, car il a un `YearChanged` événement auxiliaire qui `Year` correspond au type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="992c7-232">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="992c7-233">Par Convention, `<ChildComponent @bind-Year="ParentYear" />` est essentiellement équivalent à l’écriture :</span><span class="sxs-lookup"><span data-stu-id="992c7-233">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="992c7-234">En général, une propriété peut être liée à un gestionnaire d’événements correspondant `@bind-property:event` à l’aide de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="992c7-234">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="992c7-235">Par exemple, la propriété `MyProp` peut être liée à `MyEventHandler` à l’aide des deux attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="992c7-235">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="992c7-236">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="992c7-236">Event handling</span></span>

<span data-ttu-id="992c7-237">Les composants Razor fournissent des fonctionnalités de gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="992c7-237">Razor components provide event handling features.</span></span> <span data-ttu-id="992c7-238">Pour un attribut d’élément HTML `on{event}` nommé (par exemple `onclick` , `onsubmit`et) avec une valeur typée de type délégué, les composants Razor traitent la valeur de l’attribut en tant que gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="992c7-238">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="992c7-239">Le nom de l’attribut est toujours mis en forme [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="992c7-239">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="992c7-240">Le code suivant appelle la `UpdateHeading` méthode lorsque le bouton est sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="992c7-240">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="992c7-241">Le code suivant appelle la `CheckChanged` méthode lorsque la case à cocher est modifiée dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="992c7-241">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="992c7-242">Les gestionnaires d’événements peuvent également être asynchrones et <xref:System.Threading.Tasks.Task>retourner un.</span><span class="sxs-lookup"><span data-stu-id="992c7-242">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="992c7-243">Il n’est pas nécessaire d’appeler `StateHasChanged()`manuellement.</span><span class="sxs-lookup"><span data-stu-id="992c7-243">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="992c7-244">Les exceptions sont journalisées lorsqu’elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="992c7-244">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="992c7-245">Dans l’exemple suivant, `UpdateHeading` est appelé de façon asynchrone quand le bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="992c7-245">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="992c7-246">Types d’arguments d’événement</span><span class="sxs-lookup"><span data-stu-id="992c7-246">Event argument types</span></span>

<span data-ttu-id="992c7-247">Pour certains événements, les types d’arguments d’événement sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="992c7-247">For some events, event argument types are permitted.</span></span> <span data-ttu-id="992c7-248">Si l’accès à l’un de ces types d’événements n’est pas nécessaire, il n’est pas obligatoire dans l’appel de la méthode.</span><span class="sxs-lookup"><span data-stu-id="992c7-248">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="992c7-249">Les éléments [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) pris en charge sont présentés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="992c7-249">Supported [EventArgs](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web) are shown in the following table.</span></span>

| <span data-ttu-id="992c7-250">Événement</span><span class="sxs-lookup"><span data-stu-id="992c7-250">Event</span></span> | <span data-ttu-id="992c7-251">Classe</span><span class="sxs-lookup"><span data-stu-id="992c7-251">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="992c7-252">Presse-papiers</span><span class="sxs-lookup"><span data-stu-id="992c7-252">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="992c7-253">Déplacez</span><span class="sxs-lookup"><span data-stu-id="992c7-253">Drag</span></span>             | <span data-ttu-id="992c7-254">`DragEventArgs`et contiennent des`DataTransferItem` données d’élément glissées. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="992c7-254">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="992c7-255">Error</span><span class="sxs-lookup"><span data-stu-id="992c7-255">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="992c7-256">Focus</span><span class="sxs-lookup"><span data-stu-id="992c7-256">Focus</span></span>            | <span data-ttu-id="992c7-257">`FocusEventArgs`N’inclut pas la prise en charge de `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="992c7-257">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="992c7-258">Modification de`<input>`</span><span class="sxs-lookup"><span data-stu-id="992c7-258">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="992c7-259">Clavier</span><span class="sxs-lookup"><span data-stu-id="992c7-259">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="992c7-260">Souris</span><span class="sxs-lookup"><span data-stu-id="992c7-260">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="992c7-261">Pointeur de la souris</span><span class="sxs-lookup"><span data-stu-id="992c7-261">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="992c7-262">Roulette de la souris</span><span class="sxs-lookup"><span data-stu-id="992c7-262">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="992c7-263">Progression</span><span class="sxs-lookup"><span data-stu-id="992c7-263">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="992c7-264">Entrées tactiles</span><span class="sxs-lookup"><span data-stu-id="992c7-264">Touch</span></span>            | <span data-ttu-id="992c7-265">`TouchEventArgs`&ndash; représenteunpointdecontactunique`TouchPoint` sur un appareil tactile.</span><span class="sxs-lookup"><span data-stu-id="992c7-265">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="992c7-266">Pour plus d’informations sur les propriétés et le comportement de gestion des événements des événements du tableau précédent, consultez [classes EventArgs dans la source de référence (ASPNET/AspNetCore Release/3.0-preview9 Branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="992c7-266">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0-preview9 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview9/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="992c7-267">Expressions lambda</span><span class="sxs-lookup"><span data-stu-id="992c7-267">Lambda expressions</span></span>

<span data-ttu-id="992c7-268">Les expressions lambda peuvent également être utilisées :</span><span class="sxs-lookup"><span data-stu-id="992c7-268">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="992c7-269">Il est souvent pratique de se rapprocher de valeurs supplémentaires, par exemple lors de l’itération sur un ensemble d’éléments.</span><span class="sxs-lookup"><span data-stu-id="992c7-269">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="992c7-270">L’exemple suivant crée trois boutons, chacun d’entre eux `UpdateHeading` qui passe un argument d'`UIMouseEventArgs`événement () et son numéro`buttonNumber`de bouton () lorsqu’ils sont sélectionnés dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="992c7-270">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="992c7-271">N’utilisez **pas** la variable de boucle`i`() dans `for` une boucle directement dans une expression lambda.</span><span class="sxs-lookup"><span data-stu-id="992c7-271">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="992c7-272">Dans le cas contraire, la même variable est utilisée par `i`toutes les expressions lambda, ce qui signifie que la valeur de est identique dans toutes les expressions lambda.</span><span class="sxs-lookup"><span data-stu-id="992c7-272">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="992c7-273">Capturez toujours sa valeur dans une variable locale`buttonNumber` (dans l’exemple précédent), puis utilisez-la.</span><span class="sxs-lookup"><span data-stu-id="992c7-273">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="992c7-274">EventCallback suivante</span><span class="sxs-lookup"><span data-stu-id="992c7-274">EventCallback</span></span>

<span data-ttu-id="992c7-275">Un scénario courant avec des composants imbriqués est le désir d’exécuter la méthode d’un composant parent lorsqu’un événement de&mdash;composant enfant se produit, `onclick` par exemple, lorsqu’un événement se produit dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="992c7-275">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="992c7-276">Pour exposer des événements entre les composants, `EventCallback`utilisez un.</span><span class="sxs-lookup"><span data-stu-id="992c7-276">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="992c7-277">Un composant parent peut affecter une méthode de rappel à un composant `EventCallback`enfant.</span><span class="sxs-lookup"><span data-stu-id="992c7-277">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="992c7-278">L `ChildComponent` 'de l’exemple d’application montre comment un `onclick` gestionnaire de bouton est configuré pour recevoir `EventCallback` un délégué à partir de `ParentComponent`l’exemple de.</span><span class="sxs-lookup"><span data-stu-id="992c7-278">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="992c7-279">Le `EventCallback` est typé avec `UIMouseEventArgs`, ce qui est approprié pour `onclick` un événement à partir d’un périphérique :</span><span class="sxs-lookup"><span data-stu-id="992c7-279">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="992c7-280">Définit la méthode`ShowMessage` de `EventCallback<T>`l’enfant: `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="992c7-280">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="992c7-281">Lorsque le bouton est sélectionné dans le `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="992c7-281">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="992c7-282">La `ParentComponent`méthode `ShowMessage` de est appelée.</span><span class="sxs-lookup"><span data-stu-id="992c7-282">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="992c7-283">`messageText`est mis à jour et affiché `ParentComponent`dans le.</span><span class="sxs-lookup"><span data-stu-id="992c7-283">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="992c7-284">Un appel à `StateHasChanged` n’est pas requis dans la méthode du`ShowMessage`rappel ().</span><span class="sxs-lookup"><span data-stu-id="992c7-284">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="992c7-285">`StateHasChanged`est appelé automatiquement pour rerestituer `ParentComponent`le, tout comme les événements enfants déclenchent le rerendu des composants dans les gestionnaires d’événements qui s’exécutent dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="992c7-285">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="992c7-286">`EventCallback`et `EventCallback<T>` autorisent les délégués asynchrones.</span><span class="sxs-lookup"><span data-stu-id="992c7-286">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="992c7-287">`EventCallback<T>`est fortement typé et requiert un type d’argument spécifique.</span><span class="sxs-lookup"><span data-stu-id="992c7-287">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="992c7-288">`EventCallback`est faiblement typé et autorise tout type d’argument.</span><span class="sxs-lookup"><span data-stu-id="992c7-288">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="992c7-289">`InvokeAsync` <xref:System.Threading.Tasks.Task>Appelez ou avec`EventCallback<T>` et en attente de : `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="992c7-289">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="992c7-290">Utilisez `EventCallback` et`EventCallback<T>` pour la gestion des événements et les paramètres de composant de liaison.</span><span class="sxs-lookup"><span data-stu-id="992c7-290">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="992c7-291">Préférez le fortement typé `EventCallback<T>`. `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="992c7-291">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="992c7-292">`EventCallback<T>`fournit un meilleur retour d’erreur aux utilisateurs du composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-292">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="992c7-293">Comme pour d’autres gestionnaires d’événements d’interface utilisateur, la spécification du paramètre d’événement est facultative.</span><span class="sxs-lookup"><span data-stu-id="992c7-293">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="992c7-294">Utilisez `EventCallback` quand aucune valeur n’est passée au rappel.</span><span class="sxs-lookup"><span data-stu-id="992c7-294">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="992c7-295">Capturer des références à des composants</span><span class="sxs-lookup"><span data-stu-id="992c7-295">Capture references to components</span></span>

<span data-ttu-id="992c7-296">Les références de composant offrent un moyen de référencer une instance de composant afin que vous puissiez émettre des commandes vers `Show` cette `Reset`instance, telles que ou.</span><span class="sxs-lookup"><span data-stu-id="992c7-296">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="992c7-297">Pour capturer une référence de composant :</span><span class="sxs-lookup"><span data-stu-id="992c7-297">To capture a component reference:</span></span>

* <span data-ttu-id="992c7-298">Ajoutez un [@ref](xref:mvc/views/razor#ref) attribut au composant enfant.</span><span class="sxs-lookup"><span data-stu-id="992c7-298">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="992c7-299">Définissez un champ avec le même type que le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="992c7-299">Define a field with the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="992c7-300">Lors du rendu du composant, `loginDialog` le champ est rempli avec l’instance du `MyLoginDialog` composant enfant.</span><span class="sxs-lookup"><span data-stu-id="992c7-300">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="992c7-301">Vous pouvez ensuite appeler des méthodes .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-301">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="992c7-302">La `loginDialog` variable est remplie uniquement après le rendu du composant et sa sortie comprend l' `MyLoginDialog` élément.</span><span class="sxs-lookup"><span data-stu-id="992c7-302">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="992c7-303">Jusqu’à ce stade, il n’y a rien à référencer.</span><span class="sxs-lookup"><span data-stu-id="992c7-303">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="992c7-304">Pour manipuler des références de composants après la fin du rendu du composant `OnAfterRenderAsync` , `OnAfterRender` utilisez les méthodes ou.</span><span class="sxs-lookup"><span data-stu-id="992c7-304">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="992c7-305">Bien que la capture de références de composant utilise une syntaxe similaire pour [capturer des références d’élément](xref:blazor/javascript-interop#capture-references-to-elements), il ne s’agit pas d’une fonctionnalité [JavaScript Interop](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="992c7-305">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="992c7-306">Les références de composant ne sont pas&mdash;transmises au code JavaScript et ne sont utilisées que dans le code .net.</span><span class="sxs-lookup"><span data-stu-id="992c7-306">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="992c7-307">N’utilisez **pas** de références de composant pour muter l’état des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="992c7-307">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="992c7-308">Utilisez plutôt des paramètres déclaratifs normaux pour passer des données aux composants enfants.</span><span class="sxs-lookup"><span data-stu-id="992c7-308">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="992c7-309">L’utilisation de paramètres déclaratifs normaux entraîne le rerendu automatique des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="992c7-309">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="992c7-310">Appeler des méthodes de composant en externe pour mettre à jour l’État</span><span class="sxs-lookup"><span data-stu-id="992c7-310">Invoke component methods externally to update state</span></span>

<span data-ttu-id="992c7-311">Éblouissant utilise un `SynchronizationContext` pour appliquer un seul thread logique d’exécution.</span><span class="sxs-lookup"><span data-stu-id="992c7-311">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="992c7-312">Les méthodes de cycle de vie d’un composant et les rappels d’événements déclenchés par éblouissant sont `SynchronizationContext`exécutés sur ce.</span><span class="sxs-lookup"><span data-stu-id="992c7-312">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="992c7-313">Dans le cas où un composant doit être mis à jour en fonction d’un événement externe, tel qu’un minuteur ou `InvokeAsync` d’autres notifications, utilisez la méthode, `SynchronizationContext`qui sera réexpédiée vers le.</span><span class="sxs-lookup"><span data-stu-id="992c7-313">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="992c7-314">Par exemple, considérez un *service de notification* qui peut notifier n’importe quel composant d’écoute de l’État mis à jour :</span><span class="sxs-lookup"><span data-stu-id="992c7-314">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

    public event Action<string, int, Task> Notify;
}
```

<span data-ttu-id="992c7-315">Utilisation du `NotifierService` pour mettre à jour un composant :</span><span class="sxs-lookup"><span data-stu-id="992c7-315">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="992c7-316">Dans l’exemple précédent, `NotifierService` appelle la méthode du `OnNotify` composant `SynchronizationContext`en dehors de l’élément éblouissant.</span><span class="sxs-lookup"><span data-stu-id="992c7-316">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="992c7-317">`InvokeAsync`est utilisé pour basculer vers le contexte correct et pour la mise en file d’attente d’un rendu.</span><span class="sxs-lookup"><span data-stu-id="992c7-317">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="992c7-318">Utiliser \@la clé pour contrôler la conservation des éléments et des composants</span><span class="sxs-lookup"><span data-stu-id="992c7-318">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="992c7-319">Lors du rendu d’une liste d’éléments ou de composants, et que les éléments ou composants changent par la suite, l’algorithme de différenciation de éblouissant doit décider quel élément ou composant précédent peut être conservé et comment les objets de modèle doivent être mappés à ces éléments.</span><span class="sxs-lookup"><span data-stu-id="992c7-319">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="992c7-320">Normalement, ce processus est automatique et peut être ignoré, mais dans certains cas, il peut être utile de contrôler le processus.</span><span class="sxs-lookup"><span data-stu-id="992c7-320">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="992c7-321">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="992c7-321">Consider the following example:</span></span>

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

<span data-ttu-id="992c7-322">Le contenu de la `People` collection peut changer avec des entrées insérées, supprimées ou réorganisées.</span><span class="sxs-lookup"><span data-stu-id="992c7-322">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="992c7-323">Lors du rerendu du composant, le `<DetailsEditor>` composant peut changer pour recevoir des `Details` valeurs de paramètre différentes.</span><span class="sxs-lookup"><span data-stu-id="992c7-323">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="992c7-324">Cela peut entraîner un rerendu plus complexe que prévu.</span><span class="sxs-lookup"><span data-stu-id="992c7-324">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="992c7-325">Dans certains cas, le rerendu peut entraîner des différences de comportement visibles, telles que le focus de l’élément perdu.</span><span class="sxs-lookup"><span data-stu-id="992c7-325">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="992c7-326">Le processus de mappage peut être contrôlé à `@key` l’aide de l’attribut directive.</span><span class="sxs-lookup"><span data-stu-id="992c7-326">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="992c7-327">`@key`force l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé :</span><span class="sxs-lookup"><span data-stu-id="992c7-327">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="992c7-328">Lorsque la `People` collection est modifiée, l’algorithme de comparaison conserve l’Association `<DetailsEditor>` entre les `person` instances et les instances :</span><span class="sxs-lookup"><span data-stu-id="992c7-328">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="992c7-329">Si un `Person` est supprimé de la `People` liste, seule l’instance `<DetailsEditor>` correspondante est supprimée de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-329">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="992c7-330">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="992c7-330">Other instances are left unchanged.</span></span>
* <span data-ttu-id="992c7-331">Si un `Person` est inséré à une position dans la liste, une nouvelle `<DetailsEditor>` instance est insérée à cette position correspondante.</span><span class="sxs-lookup"><span data-stu-id="992c7-331">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="992c7-332">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="992c7-332">Other instances are left unchanged.</span></span>
* <span data-ttu-id="992c7-333">Si `Person` les entrées sont réordonnées, les `<DetailsEditor>` instances correspondantes sont conservées et réordonnées dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-333">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="992c7-334">Dans certains scénarios, l’utilisation `@key` de réduit la complexité du rerendu et évite les problèmes potentiels liés aux parties avec état de la modification du modèle DOM, telles que la position du focus.</span><span class="sxs-lookup"><span data-stu-id="992c7-334">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="992c7-335">Les clés sont locales pour chaque élément ou composant conteneur.</span><span class="sxs-lookup"><span data-stu-id="992c7-335">Keys are local to each container element or component.</span></span> <span data-ttu-id="992c7-336">Les clés ne sont pas comparées globalement dans le document.</span><span class="sxs-lookup"><span data-stu-id="992c7-336">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="992c7-337">Quand utiliser \@la clé</span><span class="sxs-lookup"><span data-stu-id="992c7-337">When to use \@key</span></span>

<span data-ttu-id="992c7-338">En règle générale, il est judicieux `@key` d’utiliser chaque fois qu’une liste est rendue (par `@foreach` exemple, dans un bloc) et qu’une `@key`valeur appropriée existe pour définir.</span><span class="sxs-lookup"><span data-stu-id="992c7-338">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="992c7-339">Vous pouvez également utiliser `@key` pour empêcher éblouissant de conserver un élément ou une sous-arborescence de composants lorsqu’un objet change :</span><span class="sxs-lookup"><span data-stu-id="992c7-339">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="992c7-340">En `@currentPerson` cas de modification `@key` , la directive d’attribut force éblouissant à ignorer `<div>` la totalité et ses descendants et à reconstruire la sous-arborescence de l’interface utilisateur avec de nouveaux éléments et composants.</span><span class="sxs-lookup"><span data-stu-id="992c7-340">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="992c7-341">Cela peut être utile si vous devez garantir qu’aucun État d’interface utilisateur n’est `@currentPerson` préservé lorsque des modifications sont apportées.</span><span class="sxs-lookup"><span data-stu-id="992c7-341">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="992c7-342">Quand ne pas utiliser \@la clé</span><span class="sxs-lookup"><span data-stu-id="992c7-342">When not to use \@key</span></span>

<span data-ttu-id="992c7-343">Il y a un coût en matière de performances `@key`lors de la comparaison avec.</span><span class="sxs-lookup"><span data-stu-id="992c7-343">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="992c7-344">Le coût des performances n’est pas important, `@key` mais spécifiez uniquement si les règles de conservation des éléments ou des composants bénéficient de l’application.</span><span class="sxs-lookup"><span data-stu-id="992c7-344">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="992c7-345">Même si `@key` n’est pas utilisé, éblouissant conserve autant que possible les instances d’élément et de composant enfants.</span><span class="sxs-lookup"><span data-stu-id="992c7-345">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="992c7-346">Le seul avantage de l' `@key` utilisation de est de contrôler la *façon dont* les instances de modèle sont mappées aux instances de composant conservées, au lieu de l’algorithme de comparaison qui sélectionne le mappage.</span><span class="sxs-lookup"><span data-stu-id="992c7-346">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="992c7-347">Valeurs à utiliser pour \@la clé</span><span class="sxs-lookup"><span data-stu-id="992c7-347">What values to use for \@key</span></span>

<span data-ttu-id="992c7-348">En règle générale, il est logique de fournir l’un des types de valeur `@key`suivants pour :</span><span class="sxs-lookup"><span data-stu-id="992c7-348">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="992c7-349">Instances d’objet de modèle (par exemple `Person` , une instance comme dans l’exemple précédent).</span><span class="sxs-lookup"><span data-stu-id="992c7-349">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="992c7-350">Cela garantit la préservation en fonction de l’égalité des références d’objet.</span><span class="sxs-lookup"><span data-stu-id="992c7-350">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="992c7-351">Identificateurs uniques (par exemple, les valeurs de clé primaire `int`de `string`type, `Guid`ou).</span><span class="sxs-lookup"><span data-stu-id="992c7-351">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="992c7-352">Vérifiez que les valeurs utilisées `@key` pour ne sont pas en conflit.</span><span class="sxs-lookup"><span data-stu-id="992c7-352">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="992c7-353">Si les valeurs en conflit sont détectées dans le même élément parent, éblouissant lève une exception, car il ne peut pas mapper de manière déterministe les anciens éléments ou composants aux nouveaux éléments ou composants.</span><span class="sxs-lookup"><span data-stu-id="992c7-353">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="992c7-354">Utilisez uniquement des valeurs distinctes, telles que des instances d’objets ou des valeurs de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="992c7-354">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="992c7-355">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="992c7-355">Lifecycle methods</span></span>

<span data-ttu-id="992c7-356">`OnInitializedAsync`et `OnInitialized` exécuter le code pour initialiser le composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-356">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="992c7-357">Pour effectuer une opération asynchrone, utilisez `OnInitializedAsync` et le `await` mot clé sur l’opération :</span><span class="sxs-lookup"><span data-stu-id="992c7-357">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="992c7-358">Pour une opération synchrone, utilisez `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="992c7-358">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="992c7-359">`OnParametersSetAsync`et `OnParametersSet` sont appelées lorsqu’un composant a reçu des paramètres de son parent et que les valeurs sont assignées aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="992c7-359">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="992c7-360">Ces méthodes sont exécutées après l’initialisation du composant et chaque fois que le composant est rendu :</span><span class="sxs-lookup"><span data-stu-id="992c7-360">These methods are executed after component initialization and each time the component is rendered:</span></span>

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

<span data-ttu-id="992c7-361">`OnAfterRenderAsync`et `OnAfterRender` sont appelées après la fin d’un rendu d’un composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-361">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="992c7-362">Les références d’élément et de composant sont remplies à ce stade.</span><span class="sxs-lookup"><span data-stu-id="992c7-362">Element and component references are populated at this point.</span></span> <span data-ttu-id="992c7-363">Utilisez cette étape pour effectuer des étapes d’initialisation supplémentaires à l’aide du contenu rendu, par exemple l’activation de bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.</span><span class="sxs-lookup"><span data-stu-id="992c7-363">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="992c7-364">`OnAfterRender`*n’est pas appelé lors du prérendu sur le serveur.*</span><span class="sxs-lookup"><span data-stu-id="992c7-364">`OnAfterRender` *is not called when prerendering on the server.*</span></span>

<span data-ttu-id="992c7-365">Le `firstRender` paramètre pour `OnAfterRenderAsync` et `OnAfterRender` est :</span><span class="sxs-lookup"><span data-stu-id="992c7-365">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="992c7-366">Défini sur `true` la première fois que l’instance de composant est appelée.</span><span class="sxs-lookup"><span data-stu-id="992c7-366">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="992c7-367">Garantit que le travail d’initialisation n’est effectué qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="992c7-367">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="992c7-368">Gérer les actions asynchrones incomplètes au rendu</span><span class="sxs-lookup"><span data-stu-id="992c7-368">Handle incomplete async actions at render</span></span>

<span data-ttu-id="992c7-369">Les actions asynchrones exécutées dans des événements de cycle de vie peuvent ne pas être terminées avant le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-369">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="992c7-370">Les objets peuvent `null` être ou être remplis de façon incomplète avec des données pendant l’exécution de la méthode Lifecycle.</span><span class="sxs-lookup"><span data-stu-id="992c7-370">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="992c7-371">Fournissez une logique de rendu pour confirmer que les objets sont initialisés.</span><span class="sxs-lookup"><span data-stu-id="992c7-371">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="992c7-372">Affichez les éléments d’interface utilisateur d’espace réservé (par exemple, un `null`message de chargement) tandis que les objets sont.</span><span class="sxs-lookup"><span data-stu-id="992c7-372">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="992c7-373">Dans le `FetchData` composant des modèles éblouissant, `OnInitializedAsync` est remplacé par asynchrone recevoir les données de prévision (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="992c7-373">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="992c7-374">Lorsque `forecasts` a `null`la valeur, un message de chargement est affiché à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-374">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="992c7-375">Une fois `Task` la `OnInitializedAsync` retournée terminée, le composant est rerendu avec l’État mis à jour.</span><span class="sxs-lookup"><span data-stu-id="992c7-375">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="992c7-376">*Pages/FetchData.razor* :</span><span class="sxs-lookup"><span data-stu-id="992c7-376">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="992c7-377">Exécuter du code avant la définition des paramètres</span><span class="sxs-lookup"><span data-stu-id="992c7-377">Execute code before parameters are set</span></span>

<span data-ttu-id="992c7-378">`SetParameters`peut être substitué pour exécuter du code avant que les paramètres soient définis :</span><span class="sxs-lookup"><span data-stu-id="992c7-378">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="992c7-379">Si `base.SetParameters` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants de la façon requise.</span><span class="sxs-lookup"><span data-stu-id="992c7-379">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="992c7-380">Par exemple, il n’est pas nécessaire que les paramètres entrants soient assignés aux propriétés de la classe.</span><span class="sxs-lookup"><span data-stu-id="992c7-380">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="992c7-381">Supprimer l’actualisation de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="992c7-381">Suppress refreshing of the UI</span></span>

<span data-ttu-id="992c7-382">`ShouldRender`peut être substitué pour supprimer l’actualisation de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-382">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="992c7-383">Si l’implémentation retourne `true`, l’interface utilisateur est actualisée.</span><span class="sxs-lookup"><span data-stu-id="992c7-383">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="992c7-384">Même si `ShouldRender` est substitué, le composant est toujours restitué initialement.</span><span class="sxs-lookup"><span data-stu-id="992c7-384">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="992c7-385">Suppression de composant avec IDisposable</span><span class="sxs-lookup"><span data-stu-id="992c7-385">Component disposal with IDisposable</span></span>

<span data-ttu-id="992c7-386">Si un composant implémente <xref:System.IDisposable>, la [méthode dispose](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-386">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="992c7-387">Le composant suivant utilise `@implements IDisposable` et la `Dispose` méthode :</span><span class="sxs-lookup"><span data-stu-id="992c7-387">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="992c7-388">Routage</span><span class="sxs-lookup"><span data-stu-id="992c7-388">Routing</span></span>

<span data-ttu-id="992c7-389">Le routage dans éblouissant est obtenu en fournissant un modèle de routage à chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="992c7-389">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="992c7-390">Lorsqu’un fichier Razor avec une `@page` directive est compilé, la classe générée reçoit un qui <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifie le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="992c7-390">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="992c7-391">Lors de l’exécution, le routeur recherche les classes de `RouteAttribute` composant avec un et rend le composant qui a un modèle de routage correspondant à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="992c7-391">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="992c7-392">Plusieurs modèles de routage peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-392">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="992c7-393">Le composant suivant répond aux demandes pour `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="992c7-393">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="992c7-394">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="992c7-394">Route parameters</span></span>

<span data-ttu-id="992c7-395">Les composants peuvent recevoir des paramètres de routage du modèle de routage `@page` fourni dans la directive.</span><span class="sxs-lookup"><span data-stu-id="992c7-395">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="992c7-396">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="992c7-396">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="992c7-397">*Composant de paramètre de routage*:</span><span class="sxs-lookup"><span data-stu-id="992c7-397">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="992c7-398">Les paramètres facultatifs ne sont pas `@page` pris en charge. deux directives sont donc appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="992c7-398">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="992c7-399">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="992c7-399">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="992c7-400">La deuxième `@page` directive prend le `{text}` paramètre d’itinéraire et assigne la valeur à `Text` la propriété.</span><span class="sxs-lookup"><span data-stu-id="992c7-400">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="992c7-401">Héritage de la classe de base pour une expérience « code-behind »</span><span class="sxs-lookup"><span data-stu-id="992c7-401">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="992c7-402">Les fichiers de composants associent C# le balisage HTML et le code de traitement dans le même fichier.</span><span class="sxs-lookup"><span data-stu-id="992c7-402">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="992c7-403">La `@inherits` directive peut être utilisée pour fournir des applications éblouissantes avec une expérience « code-behind » qui sépare le balisage de composant du code de traitement.</span><span class="sxs-lookup"><span data-stu-id="992c7-403">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="992c7-404">L' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) montre comment un composant peut hériter d’une `BlazorRocksBase`classe de base,, pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-404">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="992c7-405">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="992c7-405">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="992c7-406">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="992c7-406">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="992c7-407">La classe de base doit dériver de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="992c7-407">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="992c7-408">Importer des composants</span><span class="sxs-lookup"><span data-stu-id="992c7-408">Import components</span></span>

<span data-ttu-id="992c7-409">L’espace de noms d’un composant créé avec Razor est basé sur :</span><span class="sxs-lookup"><span data-stu-id="992c7-409">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="992c7-410">Du `RootNamespace`projet.</span><span class="sxs-lookup"><span data-stu-id="992c7-410">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="992c7-411">Chemin d’accès de la racine du projet au composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-411">The path from the project root to the component.</span></span> <span data-ttu-id="992c7-412">Par exemple, `ComponentsSample/Pages/Index.razor` se trouve dans l' `ComponentsSample.Pages`espace de noms.</span><span class="sxs-lookup"><span data-stu-id="992c7-412">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="992c7-413">Les composants C# suivent les règles de liaison de nom.</span><span class="sxs-lookup"><span data-stu-id="992c7-413">Components follow C# name binding rules.</span></span> <span data-ttu-id="992c7-414">Dans le cas de *index. Razor*, tous les composants dans le même dossier, les mêmes *pages*et le dossier parent, *ComponentsSample*, sont dans l’étendue.</span><span class="sxs-lookup"><span data-stu-id="992c7-414">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="992c7-415">Les composants définis dans un espace de noms différent peuvent être mis en portée à l’aide de la directive [ \@using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="992c7-415">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="992c7-416">Si un autre composant `NavMenu.razor`,, existe dans le `ComponentsSample/Shared/`dossier, le composant peut être utilisé `Index.razor` dans avec l' `@using` instruction suivante :</span><span class="sxs-lookup"><span data-stu-id="992c7-416">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="992c7-417">Les composants peuvent également être référencés à l’aide de leurs noms complets, ce qui supprime [ \@](xref:mvc/views/razor#using) la nécessité de la directive using :</span><span class="sxs-lookup"><span data-stu-id="992c7-417">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="992c7-418">La `global::` qualification n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="992c7-418">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="992c7-419">L’importation de composants avec `using` des instructions avec alias ( `@using Foo = Bar`par exemple,) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="992c7-419">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="992c7-420">Les noms partiellement qualifiés ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="992c7-420">Partially qualified names aren't supported.</span></span> <span data-ttu-id="992c7-421">Par exemple, l' `@using ComponentsSample` ajout et `NavMenu.razor` la `<Shared.NavMenu></Shared.NavMenu>` référencement avec ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="992c7-421">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="992c7-422">Attributs d’éléments HTML conditionnels</span><span class="sxs-lookup"><span data-stu-id="992c7-422">Conditional HTML element attributes</span></span>

<span data-ttu-id="992c7-423">Les attributs des éléments HTML sont rendus de manière conditionnelle en fonction de la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="992c7-423">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="992c7-424">Si la valeur est `false` ou `null`, l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="992c7-424">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="992c7-425">Si la valeur est `true`, l’attribut est rendu réduit.</span><span class="sxs-lookup"><span data-stu-id="992c7-425">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="992c7-426">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est rendu dans le balisage de l’élément :</span><span class="sxs-lookup"><span data-stu-id="992c7-426">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="992c7-427">Si `IsCompleted` est`true`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="992c7-427">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="992c7-428">Si `IsCompleted` est`false`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="992c7-428">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="992c7-429">Pour plus d'informations, consultez <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="992c7-429">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="992c7-430">HTML brut</span><span class="sxs-lookup"><span data-stu-id="992c7-430">Raw HTML</span></span>

<span data-ttu-id="992c7-431">Les chaînes sont normalement rendues à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage qu’elles peuvent contenir est ignorée et traitée comme du texte littéral.</span><span class="sxs-lookup"><span data-stu-id="992c7-431">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="992c7-432">Pour afficher le code HTML brut, encapsulez le `MarkupString` contenu HTML dans une valeur.</span><span class="sxs-lookup"><span data-stu-id="992c7-432">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="992c7-433">La valeur est analysée au format HTML ou SVG et est insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="992c7-433">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="992c7-434">Le rendu de code HTML brut construit à partir de n’importe quelle source non approuvée constitue un risque pour la **sécurité** et doit être évité !</span><span class="sxs-lookup"><span data-stu-id="992c7-434">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="992c7-435">L’exemple suivant illustre l’utilisation `MarkupString` du type pour ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :</span><span class="sxs-lookup"><span data-stu-id="992c7-435">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="992c7-436">Composants basés sur un modèle</span><span class="sxs-lookup"><span data-stu-id="992c7-436">Templated components</span></span>

<span data-ttu-id="992c7-437">Les composants basés sur un modèle sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, qui peuvent ensuite être utilisés dans le cadre de la logique de rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-437">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="992c7-438">Les composants basés sur un modèle vous permettent de créer des composants de niveau supérieur qui sont plus réutilisables que les composants normaux.</span><span class="sxs-lookup"><span data-stu-id="992c7-438">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="992c7-439">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="992c7-439">A couple of examples include:</span></span>

* <span data-ttu-id="992c7-440">Composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête, les lignes et le pied de page de la table.</span><span class="sxs-lookup"><span data-stu-id="992c7-440">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="992c7-441">Composant de liste qui permet à un utilisateur de spécifier un modèle pour afficher des éléments dans une liste.</span><span class="sxs-lookup"><span data-stu-id="992c7-441">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="992c7-442">Paramètres de modèle</span><span class="sxs-lookup"><span data-stu-id="992c7-442">Template parameters</span></span>

<span data-ttu-id="992c7-443">Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres `RenderFragment` de `RenderFragment<T>`composant de type ou.</span><span class="sxs-lookup"><span data-stu-id="992c7-443">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="992c7-444">Un fragment de rendu représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="992c7-444">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="992c7-445">`RenderFragment<T>`prend un paramètre de type qui peut être spécifié lors de l’appel du fragment de rendu.</span><span class="sxs-lookup"><span data-stu-id="992c7-445">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="992c7-446">`TableTemplate`-</span><span class="sxs-lookup"><span data-stu-id="992c7-446">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="992c7-447">Lorsque vous utilisez un composant basé sur un modèle, les paramètres de modèle peuvent être spécifiés à l’aide d’éléments enfants`TableHeader` qui `RowTemplate` correspondent aux noms des paramètres (et dans l’exemple suivant) :</span><span class="sxs-lookup"><span data-stu-id="992c7-447">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="992c7-448">Paramètres de contexte de modèle</span><span class="sxs-lookup"><span data-stu-id="992c7-448">Template context parameters</span></span>

<span data-ttu-id="992c7-449">Les arguments de composant `RenderFragment<T>` de type passé comme éléments ont un paramètre `context` implicite nommé (par exemple, à partir `@context.PetId`de l’exemple de code précédent,), mais vous `Context` pouvez modifier le nom de paramètre à l’aide de l’attribut sur l’enfant. appartient.</span><span class="sxs-lookup"><span data-stu-id="992c7-449">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="992c7-450">Dans l’exemple suivant, l' `RowTemplate` attribut de `Context` l’élément spécifie le `pet` paramètre :</span><span class="sxs-lookup"><span data-stu-id="992c7-450">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="992c7-451">Vous pouvez également spécifier l' `Context` attribut sur l’élément de composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-451">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="992c7-452">L’attribut `Context` spécifié s’applique à tous les paramètres de modèle spécifiés.</span><span class="sxs-lookup"><span data-stu-id="992c7-452">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="992c7-453">Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans élément enfant d’encapsulation).</span><span class="sxs-lookup"><span data-stu-id="992c7-453">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="992c7-454">Dans l’exemple suivant, l' `Context` attribut apparaît sur l' `TableTemplate` élément et s’applique à tous les paramètres de modèle :</span><span class="sxs-lookup"><span data-stu-id="992c7-454">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="992c7-455">Composants génériques</span><span class="sxs-lookup"><span data-stu-id="992c7-455">Generic-typed components</span></span>

<span data-ttu-id="992c7-456">Les composants basés sur un modèle sont souvent typés de façon générique.</span><span class="sxs-lookup"><span data-stu-id="992c7-456">Templated components are often generically typed.</span></span> <span data-ttu-id="992c7-457">Par exemple, un composant `ListViewTemplate` générique peut être utilisé pour restituer `IEnumerable<T>` des valeurs.</span><span class="sxs-lookup"><span data-stu-id="992c7-457">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="992c7-458">Pour définir un composant générique, utilisez la `@typeparam` directive pour spécifier les paramètres de type :</span><span class="sxs-lookup"><span data-stu-id="992c7-458">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="992c7-459">Lorsque vous utilisez des composants génériques, le paramètre de type est déduit si possible :</span><span class="sxs-lookup"><span data-stu-id="992c7-459">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="992c7-460">Sinon, le paramètre de type doit être spécifié explicitement à l’aide d’un attribut qui correspond au nom du paramètre de type.</span><span class="sxs-lookup"><span data-stu-id="992c7-460">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="992c7-461">Dans l’exemple suivant, `TItem="Pet"` spécifie le type :</span><span class="sxs-lookup"><span data-stu-id="992c7-461">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="992c7-462">Valeurs et paramètres en cascade</span><span class="sxs-lookup"><span data-stu-id="992c7-462">Cascading values and parameters</span></span>

<span data-ttu-id="992c7-463">Dans certains scénarios, il est peu commode de transmettre des données d’un composant ancêtre à un composant descendant à l’aide de [paramètres de composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-463">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="992c7-464">Les valeurs et paramètres en cascade résolvent ce problème en fournissant un moyen pratique pour un composant ancêtre de fournir une valeur à tous ses composants descendants.</span><span class="sxs-lookup"><span data-stu-id="992c7-464">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="992c7-465">Les valeurs et les paramètres en cascade fournissent également une approche pour les composants à coordonner.</span><span class="sxs-lookup"><span data-stu-id="992c7-465">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="992c7-466">Exemple de thème</span><span class="sxs-lookup"><span data-stu-id="992c7-466">Theme example</span></span>

<span data-ttu-id="992c7-467">Dans l’exemple suivant tiré de l’exemple d’application `ThemeInfo` , la classe spécifie les informations de thème pour descendre dans la hiérarchie des composants, de sorte que tous les boutons d’une partie donnée de l’application partagent le même style.</span><span class="sxs-lookup"><span data-stu-id="992c7-467">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="992c7-468">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="992c7-468">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="992c7-469">Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="992c7-469">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="992c7-470">Le `CascadingValue` composant encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants de cette sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="992c7-470">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="992c7-471">Par exemple, l’exemple d’application spécifie les`ThemeInfo`informations de thème () dans l’une des dispositions de l’application en tant que paramètre en cascade pour tous les composants qui composent le corps de la disposition de la `@Body` propriété.</span><span class="sxs-lookup"><span data-stu-id="992c7-471">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="992c7-472">`ButtonClass`la valeur `btn-success` est affectée à dans le composant Layout.</span><span class="sxs-lookup"><span data-stu-id="992c7-472">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="992c7-473">Tout composant descendant peut consommer cette propriété par le `ThemeInfo` biais de l’objet en cascade.</span><span class="sxs-lookup"><span data-stu-id="992c7-473">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="992c7-474">`CascadingValuesParametersLayout`-</span><span class="sxs-lookup"><span data-stu-id="992c7-474">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="992c7-475">Pour utiliser des valeurs en cascade, les composants déclarent des paramètres en cascade `[CascadingParameter]` à l’aide de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="992c7-475">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="992c7-476">Les valeurs en cascade sont liées aux paramètres en cascade par type.</span><span class="sxs-lookup"><span data-stu-id="992c7-476">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="992c7-477">Dans l’exemple d’application, `CascadingValuesParametersTheme` le composant lie la `ThemeInfo` valeur en cascade à un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="992c7-477">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="992c7-478">Le paramètre est utilisé pour définir la classe CSS pour l’un des boutons affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-478">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="992c7-479">`CascadingValuesParametersTheme`-</span><span class="sxs-lookup"><span data-stu-id="992c7-479">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="992c7-480">Pour mettre en cascade plusieurs valeurs du même type dans la même sous-arborescence, fournissez une chaîne `CascadingValue` unique `Name` à chaque composant `CascadingParameter`et à son correspondant.</span><span class="sxs-lookup"><span data-stu-id="992c7-480">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="992c7-481">Dans l’exemple suivant, deux `CascadingValue` composants montent en cascade `MyCascadingType` différentes instances de par nom :</span><span class="sxs-lookup"><span data-stu-id="992c7-481">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="992c7-482">Dans un composant descendant, les paramètres en cascade reçoivent leurs valeurs des valeurs en cascade correspondantes dans le composant ancêtre par nom :</span><span class="sxs-lookup"><span data-stu-id="992c7-482">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="992c7-483">Exemple TabSet</span><span class="sxs-lookup"><span data-stu-id="992c7-483">TabSet example</span></span>

<span data-ttu-id="992c7-484">Les paramètres en cascade permettent également aux composants de collaborer au sein de la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="992c7-484">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="992c7-485">Par exemple, considérez l’exemple *TabSet* suivant dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="992c7-485">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="992c7-486">L’exemple d’application possède `ITab` une interface qui implémente les onglets suivants :</span><span class="sxs-lookup"><span data-stu-id="992c7-486">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="992c7-487">Le `CascadingValuesParametersTabSet` composant utilise le `TabSet` composant, qui contient plusieurs `Tab` composants :</span><span class="sxs-lookup"><span data-stu-id="992c7-487">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="992c7-488">Les composants `Tab` enfants ne sont pas explicitement passés comme paramètres `TabSet`à.</span><span class="sxs-lookup"><span data-stu-id="992c7-488">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="992c7-489">Au lieu de cela `Tab` , les composants enfants font partie du contenu enfant `TabSet`de.</span><span class="sxs-lookup"><span data-stu-id="992c7-489">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="992c7-490">Toutefois, le `TabSet` doit toujours connaître chaque `Tab` composant pour pouvoir afficher les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter de code supplémentaire `TabSet` , le composant *peut se présenter comme une valeur en cascade* qui est ensuite récupérée par les `Tab` composants descendants.</span><span class="sxs-lookup"><span data-stu-id="992c7-490">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="992c7-491">`TabSet`-</span><span class="sxs-lookup"><span data-stu-id="992c7-491">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="992c7-492">Les composants `Tab` descendants capturent `TabSet` le contenant comme paramètre en cascade, de `Tab` sorte que les composants s' `TabSet` ajoutent eux-mêmes à la et à la coordonnée sur laquelle l’onglet est actif.</span><span class="sxs-lookup"><span data-stu-id="992c7-492">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="992c7-493">`Tab`-</span><span class="sxs-lookup"><span data-stu-id="992c7-493">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="992c7-494">Modèles Razor</span><span class="sxs-lookup"><span data-stu-id="992c7-494">Razor templates</span></span>

<span data-ttu-id="992c7-495">Les fragments de rendu peuvent être définis à l’aide de la syntaxe de modèle Razor.</span><span class="sxs-lookup"><span data-stu-id="992c7-495">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="992c7-496">Les modèles Razor sont un moyen de définir un extrait de code d’interface utilisateur et de supposer le format suivant :</span><span class="sxs-lookup"><span data-stu-id="992c7-496">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="992c7-497">L’exemple suivant montre comment spécifier `RenderFragment` des valeurs et `RenderFragment<T>` et restituer des modèles directement dans un composant.</span><span class="sxs-lookup"><span data-stu-id="992c7-497">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="992c7-498">Les fragments de rendu peuvent également être passés comme arguments à des [composants basés](#templated-components)sur un modèle.</span><span class="sxs-lookup"><span data-stu-id="992c7-498">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="992c7-499">Sortie rendue du code précédent :</span><span class="sxs-lookup"><span data-stu-id="992c7-499">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="992c7-500">Logique RenderTreeBuilder manuelle</span><span class="sxs-lookup"><span data-stu-id="992c7-500">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="992c7-501">`Microsoft.AspNetCore.Components.RenderTree`fournit des méthodes pour manipuler des composants et des éléments, y compris la C# génération manuelle de composants dans le code.</span><span class="sxs-lookup"><span data-stu-id="992c7-501">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="992c7-502">L’utilisation `RenderTreeBuilder` de pour créer des composants est un scénario avancé.</span><span class="sxs-lookup"><span data-stu-id="992c7-502">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="992c7-503">Un composant mal formé (par exemple, une balise de balisage non fermée) peut entraîner un comportement indéfini.</span><span class="sxs-lookup"><span data-stu-id="992c7-503">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="992c7-504">Prenons le composant `PetDetails` suivant, qui peut être intégré manuellement à un autre composant :</span><span class="sxs-lookup"><span data-stu-id="992c7-504">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="992c7-505">Dans l’exemple suivant, la boucle de la `CreateComponent` méthode génère trois `PetDetails` composants.</span><span class="sxs-lookup"><span data-stu-id="992c7-505">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="992c7-506">Lors de `RenderTreeBuilder` l’appel de méthodes pour créer`OpenComponent` les `AddAttribute`composants (et), les numéros de séquence sont des numéros de ligne de code source.</span><span class="sxs-lookup"><span data-stu-id="992c7-506">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="992c7-507">L’algorithme de différence éblouissant s’appuie sur les numéros de séquence correspondant à des lignes de code distinctes, et non sur des appels d’appel distincts.</span><span class="sxs-lookup"><span data-stu-id="992c7-507">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="992c7-508">Lors de la création d' `RenderTreeBuilder` un composant avec des méthodes, coder en dur les arguments pour les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="992c7-508">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="992c7-509">**L’utilisation d’un calcul ou d’un compteur pour générer le numéro de séquence peut entraîner des performances médiocres.**</span><span class="sxs-lookup"><span data-stu-id="992c7-509">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="992c7-510">Pour plus d’informations, consultez la section [numéros de séquence liés à des numéros de ligne de code et non à un ordre d’exécution](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="992c7-510">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="992c7-511">`BuiltContent`-</span><span class="sxs-lookup"><span data-stu-id="992c7-511">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="992c7-512">Les numéros de séquence sont liés aux numéros de ligne de code et non à l’ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="992c7-512">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="992c7-513">`.razor` Les fichiers éblouissants sont toujours compilés.</span><span class="sxs-lookup"><span data-stu-id="992c7-513">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="992c7-514">C’est un avantage considérable pour, `.razor` car l’étape de compilation peut être utilisée pour injecter des informations qui améliorent les performances de l’application au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="992c7-514">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="992c7-515">Un exemple clé de ces améliorations concerne les *numéros de séquence*.</span><span class="sxs-lookup"><span data-stu-id="992c7-515">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="992c7-516">Les numéros de séquence indiquent au runtime que les sorties proviennent de lignes de code distinctes et ordonnées.</span><span class="sxs-lookup"><span data-stu-id="992c7-516">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="992c7-517">Le runtime utilise ces informations pour générer des différences d’arborescence efficaces en temps linéaire, ce qui est beaucoup plus rapide qu’un algorithme de comparaison d’arborescence générale.</span><span class="sxs-lookup"><span data-stu-id="992c7-517">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="992c7-518">Considérons le fichier `.razor` simple suivant :</span><span class="sxs-lookup"><span data-stu-id="992c7-518">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="992c7-519">Le code précédent se compile comme suit :</span><span class="sxs-lookup"><span data-stu-id="992c7-519">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="992c7-520">Lorsque le code s’exécute pour la première fois, si `someFlag` est `true`, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="992c7-520">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="992c7-521">Séquence</span><span class="sxs-lookup"><span data-stu-id="992c7-521">Sequence</span></span> | <span data-ttu-id="992c7-522">Type</span><span class="sxs-lookup"><span data-stu-id="992c7-522">Type</span></span>      | <span data-ttu-id="992c7-523">Données</span><span class="sxs-lookup"><span data-stu-id="992c7-523">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="992c7-524">0</span><span class="sxs-lookup"><span data-stu-id="992c7-524">0</span></span>        | <span data-ttu-id="992c7-525">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="992c7-525">Text node</span></span> | <span data-ttu-id="992c7-526">Première</span><span class="sxs-lookup"><span data-stu-id="992c7-526">First</span></span>  |
| <span data-ttu-id="992c7-527">1</span><span class="sxs-lookup"><span data-stu-id="992c7-527">1</span></span>        | <span data-ttu-id="992c7-528">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="992c7-528">Text node</span></span> | <span data-ttu-id="992c7-529">Seconde</span><span class="sxs-lookup"><span data-stu-id="992c7-529">Second</span></span> |

<span data-ttu-id="992c7-530">Imaginez que `someFlag` devient `false`et le balisage est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="992c7-530">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="992c7-531">Cette fois-ci, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="992c7-531">This time, the builder receives:</span></span>

| <span data-ttu-id="992c7-532">Séquence</span><span class="sxs-lookup"><span data-stu-id="992c7-532">Sequence</span></span> | <span data-ttu-id="992c7-533">Type</span><span class="sxs-lookup"><span data-stu-id="992c7-533">Type</span></span>       | <span data-ttu-id="992c7-534">Données</span><span class="sxs-lookup"><span data-stu-id="992c7-534">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="992c7-535">1</span><span class="sxs-lookup"><span data-stu-id="992c7-535">1</span></span>        | <span data-ttu-id="992c7-536">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="992c7-536">Text node</span></span>  | <span data-ttu-id="992c7-537">Seconde</span><span class="sxs-lookup"><span data-stu-id="992c7-537">Second</span></span> |

<span data-ttu-id="992c7-538">Quand le runtime effectue une comparaison, il constate que l’élément au niveau `0` de la séquence a été supprimé. il génère donc le *script d’édition*trivial suivant :</span><span class="sxs-lookup"><span data-stu-id="992c7-538">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="992c7-539">Supprimez le premier nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="992c7-539">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="992c7-540">Qu’est-ce qui se passe si vous générez des numéros séquentiels par programmation</span><span class="sxs-lookup"><span data-stu-id="992c7-540">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="992c7-541">Imaginez plutôt que vous avez écrit la logique de générateur d’arborescence de rendu suivante :</span><span class="sxs-lookup"><span data-stu-id="992c7-541">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="992c7-542">La première sortie est désormais :</span><span class="sxs-lookup"><span data-stu-id="992c7-542">Now, the first output is:</span></span>

| <span data-ttu-id="992c7-543">Séquence</span><span class="sxs-lookup"><span data-stu-id="992c7-543">Sequence</span></span> | <span data-ttu-id="992c7-544">Type</span><span class="sxs-lookup"><span data-stu-id="992c7-544">Type</span></span>      | <span data-ttu-id="992c7-545">Données</span><span class="sxs-lookup"><span data-stu-id="992c7-545">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="992c7-546">0</span><span class="sxs-lookup"><span data-stu-id="992c7-546">0</span></span>        | <span data-ttu-id="992c7-547">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="992c7-547">Text node</span></span> | <span data-ttu-id="992c7-548">Première</span><span class="sxs-lookup"><span data-stu-id="992c7-548">First</span></span>  |
| <span data-ttu-id="992c7-549">1</span><span class="sxs-lookup"><span data-stu-id="992c7-549">1</span></span>        | <span data-ttu-id="992c7-550">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="992c7-550">Text node</span></span> | <span data-ttu-id="992c7-551">Seconde</span><span class="sxs-lookup"><span data-stu-id="992c7-551">Second</span></span> |

<span data-ttu-id="992c7-552">Ce résultat est identique au cas précédent, donc aucun problème négatif n’existe.</span><span class="sxs-lookup"><span data-stu-id="992c7-552">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="992c7-553">`someFlag`se `false` trouve sur le deuxième rendu et la sortie est :</span><span class="sxs-lookup"><span data-stu-id="992c7-553">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="992c7-554">Séquence</span><span class="sxs-lookup"><span data-stu-id="992c7-554">Sequence</span></span> | <span data-ttu-id="992c7-555">Type</span><span class="sxs-lookup"><span data-stu-id="992c7-555">Type</span></span>      | <span data-ttu-id="992c7-556">Données</span><span class="sxs-lookup"><span data-stu-id="992c7-556">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="992c7-557">0</span><span class="sxs-lookup"><span data-stu-id="992c7-557">0</span></span>        | <span data-ttu-id="992c7-558">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="992c7-558">Text node</span></span> | <span data-ttu-id="992c7-559">Seconde</span><span class="sxs-lookup"><span data-stu-id="992c7-559">Second</span></span> |

<span data-ttu-id="992c7-560">Cette fois-ci, l’algorithme diff constate que *deux* modifications ont été apportées, et l’algorithme génère le script Edit suivant :</span><span class="sxs-lookup"><span data-stu-id="992c7-560">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="992c7-561">Remplacez la valeur du premier nœud de texte par `Second`.</span><span class="sxs-lookup"><span data-stu-id="992c7-561">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="992c7-562">Supprimez le deuxième nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="992c7-562">Remove the second text node.</span></span>

<span data-ttu-id="992c7-563">La génération des numéros de séquence a perdu toutes les informations utiles sur `if/else` l’emplacement où les branches et les boucles étaient présentes dans le code d’origine.</span><span class="sxs-lookup"><span data-stu-id="992c7-563">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="992c7-564">Il en résulte une comparaison de **deux fois plus longtemps** qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="992c7-564">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="992c7-565">Il s’agit d’un exemple trivial.</span><span class="sxs-lookup"><span data-stu-id="992c7-565">This is a trivial example.</span></span> <span data-ttu-id="992c7-566">Dans des cas plus réalistes avec des structures complexes et profondément imbriquées, et surtout avec des boucles, le coût des performances est plus grave.</span><span class="sxs-lookup"><span data-stu-id="992c7-566">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="992c7-567">Au lieu d’identifier immédiatement les blocs de boucle ou les branches qui ont été insérées ou supprimées, l’algorithme diff doit recurse profondément dans les arborescences de rendu et générer des scripts de modification beaucoup plus longs, car il est mal informé de la façon dont les anciennes et les nouvelles structures sont liées les unes aux autres.</span><span class="sxs-lookup"><span data-stu-id="992c7-567">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="992c7-568">Conseils et conclusions</span><span class="sxs-lookup"><span data-stu-id="992c7-568">Guidance and conclusions</span></span>

* <span data-ttu-id="992c7-569">Les performances de l’application sont affectées si les numéros séquentiels sont générés dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="992c7-569">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="992c7-570">L’infrastructure ne peut pas créer ses propres numéros de séquence automatiquement au moment de l’exécution, car les informations nécessaires n’existent pas, sauf si elles sont capturées au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="992c7-570">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="992c7-571">N’écrivez pas de longs blocs de logique `RenderTreeBuilder` implémentée manuellement.</span><span class="sxs-lookup"><span data-stu-id="992c7-571">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="992c7-572">Préférer `.razor` les fichiers et permettre au compilateur de gérer les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="992c7-572">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="992c7-573">Si les numéros de séquence sont codés en dur, l’algorithme diff exige uniquement que les numéros de séquence augmentent dans la valeur.</span><span class="sxs-lookup"><span data-stu-id="992c7-573">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="992c7-574">La valeur initiale et les écarts ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="992c7-574">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="992c7-575">Une option légitime consiste à utiliser le numéro de ligne de code comme numéro de séquence, ou à commencer à partir de zéro et à augmenter par des ou des centaines (ou un intervalle de préférence).</span><span class="sxs-lookup"><span data-stu-id="992c7-575">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="992c7-576">Éblouissant utilise des numéros de séquence, tandis que d’autres infrastructures d’interface utilisateur de comparaison d’arborescence ne les utilisent pas.</span><span class="sxs-lookup"><span data-stu-id="992c7-576">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="992c7-577">La comparaison est beaucoup plus rapide lorsque les numéros de séquence sont utilisés, et éblouissant présente l’avantage d’une étape de compilation qui traite automatiquement les numéros séquentiels `.razor` pour les développeurs qui créent des fichiers.</span><span class="sxs-lookup"><span data-stu-id="992c7-577">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="992c7-578">Localisation</span><span class="sxs-lookup"><span data-stu-id="992c7-578">Localization</span></span>

<span data-ttu-id="992c7-579">Les applications côté serveur éblouissantes sont localisées à l’aide d’un [intergiciel (middleware](xref:fundamentals/localization#localization-middleware)) de localisation.</span><span class="sxs-lookup"><span data-stu-id="992c7-579">Blazor server-side apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="992c7-580">L’intergiciel sélectionne la culture appropriée pour les utilisateurs qui demandent des ressources à partir de l’application.</span><span class="sxs-lookup"><span data-stu-id="992c7-580">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="992c7-581">La culture peut être définie à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="992c7-581">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="992c7-582">Cookies</span><span class="sxs-lookup"><span data-stu-id="992c7-582">Cookies</span></span>](#cookies)
* [<span data-ttu-id="992c7-583">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="992c7-583">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="992c7-584">Pour plus d’informations et d’exemples, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="992c7-584">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="992c7-585">Cookies</span><span class="sxs-lookup"><span data-stu-id="992c7-585">Cookies</span></span>

<span data-ttu-id="992c7-586">Un cookie de culture de localisation peut conserver la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-586">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="992c7-587">Le cookie est créé par la `OnGet` méthode de la page hôte de l’application (*pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="992c7-587">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="992c7-588">L’intergiciel (middleware) de localisation lit le cookie sur les demandes suivantes pour définir la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-588">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="992c7-589">L’utilisation d’un cookie garantit que la connexion WebSocket peut propager correctement la culture.</span><span class="sxs-lookup"><span data-stu-id="992c7-589">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="992c7-590">Si les schémas de localisation sont basés sur le chemin d’URL ou la chaîne de requête, il est possible que le schéma ne soit pas en mesure de fonctionner avec les WebSockets, et donc de ne pas conserver la culture.</span><span class="sxs-lookup"><span data-stu-id="992c7-590">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="992c7-591">Par conséquent, l’utilisation d’un cookie de culture de localisation est l’approche recommandée.</span><span class="sxs-lookup"><span data-stu-id="992c7-591">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="992c7-592">Toute technique peut être utilisée pour assigner une culture si la culture est rendue persistante dans un cookie de localisation.</span><span class="sxs-lookup"><span data-stu-id="992c7-592">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="992c7-593">Si l’application a déjà un schéma de localisation établi pour les ASP.NET Core côté serveur, continuez à utiliser l’infrastructure de localisation existante de l’application et à définir le cookie de la culture de localisation dans le schéma de l’application.</span><span class="sxs-lookup"><span data-stu-id="992c7-593">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="992c7-594">L’exemple suivant montre comment définir la culture actuelle dans un cookie qui peut être lu par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="992c7-594">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="992c7-595">Créez un fichier *pages/Host. cshtml. cs* avec le contenu suivant dans l’application côté serveur éblouissant :</span><span class="sxs-lookup"><span data-stu-id="992c7-595">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor server-side app:</span></span>

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

<span data-ttu-id="992c7-596">La localisation est gérée dans l’application :</span><span class="sxs-lookup"><span data-stu-id="992c7-596">Localization is handled in the app:</span></span>

1. <span data-ttu-id="992c7-597">Le navigateur envoie une requête HTTP initiale à l’application.</span><span class="sxs-lookup"><span data-stu-id="992c7-597">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="992c7-598">La culture est affectée par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="992c7-598">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="992c7-599">La `OnGet` méthode dans *_Host. cshtml. cs* rend persistante la culture dans un cookie dans le cadre de la réponse.</span><span class="sxs-lookup"><span data-stu-id="992c7-599">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="992c7-600">Le navigateur ouvre une connexion WebSocket pour créer une session du serveur éblouissante interactive.</span><span class="sxs-lookup"><span data-stu-id="992c7-600">The browser opens a WebSocket connection to create an interactive Blazor server-side session.</span></span>
1. <span data-ttu-id="992c7-601">L’intergiciel de localisation lit le cookie et assigne la culture.</span><span class="sxs-lookup"><span data-stu-id="992c7-601">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="992c7-602">La session éblouissante côté serveur commence par la culture correcte.</span><span class="sxs-lookup"><span data-stu-id="992c7-602">The Blazor server-side session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="992c7-603">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="992c7-603">Provide UI to choose the culture</span></span>

<span data-ttu-id="992c7-604">Pour fournir une interface utilisateur permettant à un utilisateur de sélectionner une culture, il est recommandé d’effectuer une *approche basée sur la redirection* .</span><span class="sxs-lookup"><span data-stu-id="992c7-604">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="992c7-605">Le processus est similaire à ce qui se produit dans une application Web lorsqu’un utilisateur tente d’accéder à&mdash;une ressource sécurisée. l’utilisateur est redirigé vers une page de connexion, puis redirigé vers la ressource d’origine.</span><span class="sxs-lookup"><span data-stu-id="992c7-605">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="992c7-606">L’application conserve la culture sélectionnée de l’utilisateur via une redirection vers un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="992c7-606">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="992c7-607">Le contrôleur définit la culture sélectionnée de l’utilisateur dans un cookie et redirige l’utilisateur vers l’URI d’origine.</span><span class="sxs-lookup"><span data-stu-id="992c7-607">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="992c7-608">Établissez un point de terminaison HTTP sur le serveur pour définir la culture sélectionnée de l’utilisateur dans un cookie et effectuer la redirection vers l’URI d’origine :</span><span class="sxs-lookup"><span data-stu-id="992c7-608">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="992c7-609">Utilisez le `LocalRedirect` résultat de l’action pour empêcher les attaques de redirection ouvertes.</span><span class="sxs-lookup"><span data-stu-id="992c7-609">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="992c7-610">Pour plus d'informations, consultez <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="992c7-610">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="992c7-611">Le composant suivant montre un exemple d’exécution de la redirection initiale lorsque l’utilisateur sélectionne une culture :</span><span class="sxs-lookup"><span data-stu-id="992c7-611">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

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
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="992c7-612">Utiliser des scénarios de localisation .NET dans des applications éblouissantes</span><span class="sxs-lookup"><span data-stu-id="992c7-612">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="992c7-613">Dans les applications éblouissantes, les scénarios de localisation et de globalisation .NET suivants sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="992c7-613">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="992c7-614">. Système de ressources du réseau</span><span class="sxs-lookup"><span data-stu-id="992c7-614">.NET's resources system</span></span>
* <span data-ttu-id="992c7-615">Mise en forme des nombres et des dates spécifiques à la culture</span><span class="sxs-lookup"><span data-stu-id="992c7-615">Culture-specific number and date formatting</span></span>

<span data-ttu-id="992c7-616">La fonctionnalité de `@bind` éblouissant effectue une globalisation basée sur la culture actuelle de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="992c7-616">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="992c7-617">Pour plus d’informations, consultez la section [liaison de données](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="992c7-617">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="992c7-618">Un ensemble limité de scénarios de localisation de ASP.NET Core est actuellement pris en charge :</span><span class="sxs-lookup"><span data-stu-id="992c7-618">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="992c7-619">`IStringLocalizer<>`*est pris en charge* dans les applications éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="992c7-619">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="992c7-620">`IHtmlLocalizer<>`la `IViewLocalizer<>`localisation des annotations de données, et est ASP.net Core les scénarios MVC et **non pris en charge** dans les applications éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="992c7-620">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="992c7-621">Pour plus d'informations, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="992c7-621">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="992c7-622">Images SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="992c7-622">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="992c7-623">Étant donné que éblouissant rend les images HTML, prises en charge par le navigateur, y compris les images SVG (Scalable Vector Graphics) ( *. svg*), sont prises en charge via la `<img>` balise :</span><span class="sxs-lookup"><span data-stu-id="992c7-623">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="992c7-624">De même, les images SVG sont prises en charge dans les règles CSS d’un fichier de feuille de style ( *. CSS*) :</span><span class="sxs-lookup"><span data-stu-id="992c7-624">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="992c7-625">Toutefois, le balisage SVG en ligne n’est pas pris en charge dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="992c7-625">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="992c7-626">Si vous placez une `<svg>` balise directement dans un fichier de composant ( *. Razor*), le rendu d’image de base est pris en charge, mais de nombreux scénarios avancés ne sont pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="992c7-626">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="992c7-627">Par exemple, `<use>` les balises ne sont pas `@bind` actuellement respectées et ne peuvent pas être utilisées avec certaines balises SVG.</span><span class="sxs-lookup"><span data-stu-id="992c7-627">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="992c7-628">Nous prévoyons de traiter ces limitations dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="992c7-628">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="992c7-629">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="992c7-629">Additional resources</span></span>

* <span data-ttu-id="992c7-630"><xref:security/blazor/server-side>&ndash; Contient des conseils sur la création d’applications côté serveur éblouissantes qui doivent rivaliser avec l’épuisement des ressources.</span><span class="sxs-lookup"><span data-stu-id="992c7-630"><xref:security/blazor/server-side> &ndash; Includes guidance on building Blazor server-side apps that must contend with resource exhaustion.</span></span>
