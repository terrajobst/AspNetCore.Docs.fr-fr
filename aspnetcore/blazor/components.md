---
title: Créer et utiliser des composants ASP.NET Core Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants Razor, notamment comment lier des données, gérer des événements et gérer des cycles de vie de composant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: 764e5e7db995b2dcadccf6d93c826ccf32c9ba04
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681004"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="78772-103">Créer et utiliser des composants ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="78772-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="78772-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="78772-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="78772-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78772-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="78772-106">les applications Blazor sont générées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="78772-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="78772-107">Un composant est un bloc d’interface utilisateur (IU) autonome, tel qu’une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="78772-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="78772-108">Un composant comprend le balisage HTML et la logique de traitement requise pour injecter des données ou répondre à des événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="78772-109">Les composants sont flexibles et légers.</span><span class="sxs-lookup"><span data-stu-id="78772-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="78772-110">Elles peuvent être imbriquées, réutilisées et partagées entre les projets.</span><span class="sxs-lookup"><span data-stu-id="78772-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="78772-111">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="78772-111">Component classes</span></span>

<span data-ttu-id="78772-112">Les composants sont implémentés dans les fichiers de composants [Razor](xref:mvc/views/razor) ( *. Razor*) C# à l’aide d’une combinaison de balises et html.</span><span class="sxs-lookup"><span data-stu-id="78772-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="78772-113">Un composant de Blazor est officiellement désigné sous le terme de *composant Razor*.</span><span class="sxs-lookup"><span data-stu-id="78772-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="78772-114">Le nom d’un composant doit commencer par un caractère majuscule.</span><span class="sxs-lookup"><span data-stu-id="78772-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="78772-115">Par exemple, *MyCoolComponent. Razor* est valide et *MyCoolComponent. Razor* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="78772-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="78772-116">L’interface utilisateur d’un composant est définie à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="78772-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="78772-117">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="78772-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="78772-118">Quand une application est compilée, le balisage C# html et la logique de rendu sont convertis en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="78772-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="78772-119">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="78772-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="78772-120">Les membres de la classe de composants sont définis dans un bloc `@code`.</span><span class="sxs-lookup"><span data-stu-id="78772-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="78772-121">Dans le bloc `@code`, l’état du composant (propriétés, champs) est spécifié avec les méthodes de gestion des événements ou pour la définition d’une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="78772-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="78772-122">Plus d’un bloc `@code` est autorisé.</span><span class="sxs-lookup"><span data-stu-id="78772-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="78772-123">Dans les versions préliminaires antérieures de ASP.NET Core 3,0, les blocs `@functions` ont été utilisés dans le même but que les blocs `@code` dans les composants Razor.</span><span class="sxs-lookup"><span data-stu-id="78772-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="78772-124">les blocs `@functions` continuent de fonctionner dans les composants Razor, mais nous vous recommandons d’utiliser le bloc `@code` dans ASP.NET Core 3,0 Preview 6 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="78772-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="78772-125">Les membres de composant peuvent être utilisés dans le cadre de la logique de C# rendu du composant à l’aide d’expressions qui commencent par `@`.</span><span class="sxs-lookup"><span data-stu-id="78772-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="78772-126">Par exemple, un C# champ est rendu en préfixant `@` au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="78772-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="78772-127">L’exemple suivant évalue et affiche :</span><span class="sxs-lookup"><span data-stu-id="78772-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="78772-128">`_headingFontStyle` à la valeur de propriété CSS pour `font-style`.</span><span class="sxs-lookup"><span data-stu-id="78772-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="78772-129">`_headingText` au contenu de l’élément `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="78772-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="78772-130">Une fois le composant restitué initialement, le composant régénère son arborescence de rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="78772-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="78772-131"> compare ensuite la nouvelle arborescence de rendu à la précédente et applique toutes les modifications à l’Document Object Model du navigateur (DOM).</span><span class="sxs-lookup"><span data-stu-id="78772-131"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="78772-132">Les composants sont C# des classes ordinaires qui peuvent être placées n’importe où dans un projet.</span><span class="sxs-lookup"><span data-stu-id="78772-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="78772-133">Les composants qui produisent des pages Web résident généralement dans le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="78772-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="78772-134">Les composants qui ne sont pas des pages sont souvent placés dans le dossier *partagé* ou dans un dossier personnalisé ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="78772-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="78772-135">Pour utiliser un dossier personnalisé, ajoutez l’espace de noms du dossier personnalisé au composant parent ou au fichier *_Imports. Razor* de l’application.</span><span class="sxs-lookup"><span data-stu-id="78772-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="78772-136">Par exemple, l’espace de noms suivant rend les composants dans un dossier de *composants* disponibles lorsque l’espace de noms racine de l’application est `WebApplication`:</span><span class="sxs-lookup"><span data-stu-id="78772-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="78772-137">Intégrer des composants dans des applications Razor Pages et MVC</span><span class="sxs-lookup"><span data-stu-id="78772-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="78772-138">Utilisez des composants avec des applications Razor Pages et MVC existantes.</span><span class="sxs-lookup"><span data-stu-id="78772-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="78772-139">Il n’est pas nécessaire de réécrire des pages ou des vues existantes pour utiliser des composants Razor.</span><span class="sxs-lookup"><span data-stu-id="78772-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="78772-140">Lorsque la page ou la vue est restituée, les composants sont prérendus en même temps.</span><span class="sxs-lookup"><span data-stu-id="78772-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="78772-141">Pour afficher un composant à partir d’une page ou d’une vue, utilisez le tag Helper `Component` :</span><span class="sxs-lookup"><span data-stu-id="78772-141">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="78772-142">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="78772-142">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="78772-143">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="78772-143">Is prerendered into the page.</span></span>
* <span data-ttu-id="78772-144">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer un Blazor application à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-144">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="78772-145">Description</span><span class="sxs-lookup"><span data-stu-id="78772-145">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="78772-146">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="78772-146">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="78772-147">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-147">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="78772-148">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="78772-148">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="78772-149">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="78772-149">Output from the component isn't included.</span></span> <span data-ttu-id="78772-150">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-150">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="78772-151">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="78772-151">Renders the component into static HTML.</span></span> |

<span data-ttu-id="78772-152">Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie.</span><span class="sxs-lookup"><span data-stu-id="78772-152">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="78772-153">Les composants ne peuvent pas utiliser les scénarios spécifiques aux vues et aux pages, tels que les vues partielles et les sections.</span><span class="sxs-lookup"><span data-stu-id="78772-153">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="78772-154">Pour utiliser la logique de la vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="78772-154">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="78772-155">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-155">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="78772-156">Pour plus d’informations sur la façon dont les composants sont rendus, l’état des composants et le tag Helper `Component`, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="78772-156">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="78772-157">Pour afficher un composant à partir d’une page ou d’une vue, utilisez la méthode d’assistance HTML `RenderComponentAsync<TComponent>` :</span><span class="sxs-lookup"><span data-stu-id="78772-157">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

<span data-ttu-id="78772-158">`RenderMode` configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="78772-158">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="78772-159">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="78772-159">Is prerendered into the page.</span></span>
* <span data-ttu-id="78772-160">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer un Blazor application à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-160">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="78772-161">Description</span><span class="sxs-lookup"><span data-stu-id="78772-161">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="78772-162">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="78772-162">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="78772-163">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-163">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="78772-164">Les paramètres ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-164">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="78772-165">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="78772-165">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="78772-166">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="78772-166">Output from the component isn't included.</span></span> <span data-ttu-id="78772-167">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-167">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="78772-168">Les paramètres ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-168">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="78772-169">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="78772-169">Renders the component into static HTML.</span></span> <span data-ttu-id="78772-170">Les paramètres sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-170">Parameters are supported.</span></span> |

<span data-ttu-id="78772-171">Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie.</span><span class="sxs-lookup"><span data-stu-id="78772-171">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="78772-172">Les composants ne peuvent pas utiliser les scénarios spécifiques aux vues et aux pages, tels que les vues partielles et les sections.</span><span class="sxs-lookup"><span data-stu-id="78772-172">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="78772-173">Pour utiliser la logique de la vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="78772-173">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="78772-174">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-174">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="78772-175">Pour plus d’informations sur la façon dont les composants sont rendus, l’état des composants et le `RenderComponentAsync` application auxiliaire HTML, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="78772-175">For more information on how components are rendered, component state, and the `RenderComponentAsync` HTML Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

## <a name="use-components"></a><span data-ttu-id="78772-176">Utiliser des composants</span><span class="sxs-lookup"><span data-stu-id="78772-176">Use components</span></span>

<span data-ttu-id="78772-177">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="78772-177">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="78772-178">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-178">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="78772-179">La liaison d’attribut respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="78772-179">Attribute binding is case sensitive.</span></span> <span data-ttu-id="78772-180">Par exemple, `@bind` est valide et `@Bind` n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="78772-180">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="78772-181">Le balisage suivant dans *index. Razor* rend une instance de `HeadingComponent` :</span><span class="sxs-lookup"><span data-stu-id="78772-181">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="78772-182">*Composants/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-182">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="78772-183">Si un composant contient un élément HTML avec une première lettre majuscule qui ne correspond pas à un nom de composant, un avertissement est émis pour indiquer que l’élément a un nom inattendu.</span><span class="sxs-lookup"><span data-stu-id="78772-183">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="78772-184">L’ajout d’une instruction `@using` pour l’espace de noms du composant rend le composant disponible, ce qui supprime l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="78772-184">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="78772-185">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="78772-185">Component parameters</span></span>

<span data-ttu-id="78772-186">Les composants peuvent avoir des *paramètres de composant*, qui sont définis à l’aide de propriétés publiques sur la classe de composant avec l’attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="78772-186">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="78772-187">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="78772-187">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="78772-188">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-188">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="78772-189">Dans l’exemple suivant, le `ParentComponent` définit la valeur de la propriété `Title` du `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="78772-189">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="78772-190">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-190">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="78772-191">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="78772-191">Child content</span></span>

<span data-ttu-id="78772-192">Les composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="78772-192">Components can set the content of another component.</span></span> <span data-ttu-id="78772-193">Le composant d’affectation fournit le contenu entre les balises qui spécifient le composant récepteur.</span><span class="sxs-lookup"><span data-stu-id="78772-193">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="78772-194">Dans l’exemple suivant, la `ChildComponent` a une propriété `ChildContent` qui représente un `RenderFragment`, qui représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="78772-194">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="78772-195">La valeur de `ChildContent` est positionnée dans le balisage du composant où le contenu doit être rendu.</span><span class="sxs-lookup"><span data-stu-id="78772-195">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="78772-196">La valeur de `ChildContent` est reçue du composant parent et affichée à l’intérieur du `panel-body`du panneau de démarrage.</span><span class="sxs-lookup"><span data-stu-id="78772-196">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="78772-197">*Composants/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-197">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="78772-198">La propriété qui reçoit le contenu du `RenderFragment` doit être nommée `ChildContent` par Convention.</span><span class="sxs-lookup"><span data-stu-id="78772-198">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="78772-199">Les `ParentComponent` suivantes peuvent fournir du contenu pour le rendu du `ChildComponent` en plaçant le contenu à l’intérieur des balises de `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="78772-199">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="78772-200">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-200">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="78772-201">Réprojection d’attribut et paramètres arbitraires</span><span class="sxs-lookup"><span data-stu-id="78772-201">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="78772-202">Les composants peuvent capturer et restituer des attributs supplémentaires en plus des paramètres déclarés du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-202">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="78772-203">Des attributs supplémentaires peuvent être capturés dans un dictionnaire *, puis* réintégrés à un élément lorsque le composant est rendu à l’aide de la directive [@attributes](xref:mvc/views/razor#attributes) Razor.</span><span class="sxs-lookup"><span data-stu-id="78772-203">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="78772-204">Ce scénario est utile lors de la définition d’un composant qui produit un élément de balisage qui prend en charge diverses personnalisations.</span><span class="sxs-lookup"><span data-stu-id="78772-204">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="78772-205">Par exemple, il peut être fastidieux de définir des attributs séparément pour un `<input>` qui prend en charge de nombreux paramètres.</span><span class="sxs-lookup"><span data-stu-id="78772-205">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="78772-206">Dans l’exemple suivant, le premier élément `<input>` (`id="useIndividualParams"`) utilise des paramètres de composant individuels, tandis que le deuxième élément `<input>` (`id="useAttributesDict"`) utilise la projection d’attributs :</span><span class="sxs-lookup"><span data-stu-id="78772-206">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="78772-207">Le type du paramètre doit implémenter `IEnumerable<KeyValuePair<string, object>>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="78772-207">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="78772-208">L’utilisation de `IReadOnlyDictionary<string, object>` est également une option dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="78772-208">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="78772-209">Les éléments `<input>` rendus à l’aide des deux approches sont identiques :</span><span class="sxs-lookup"><span data-stu-id="78772-209">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="78772-210">Pour accepter des attributs arbitraires, définissez un paramètre de composant à l’aide de l’attribut `[Parameter]` avec la propriété `CaptureUnmatchedValues` définie sur `true`:</span><span class="sxs-lookup"><span data-stu-id="78772-210">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="78772-211">La propriété `CaptureUnmatchedValues` sur `[Parameter]` permet au paramètre de correspondre à tous les attributs qui ne correspondent à aucun autre paramètre.</span><span class="sxs-lookup"><span data-stu-id="78772-211">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="78772-212">Un composant ne peut définir qu’un seul paramètre avec `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="78772-212">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="78772-213">Le type de propriété utilisé avec `CaptureUnmatchedValues` doit pouvoir être assigné à partir de `Dictionary<string, object>` avec des clés de chaîne.</span><span class="sxs-lookup"><span data-stu-id="78772-213">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="78772-214">`IEnumerable<KeyValuePair<string, object>>` ou `IReadOnlyDictionary<string, object>` sont également des options dans ce scénario.</span><span class="sxs-lookup"><span data-stu-id="78772-214">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="78772-215">La position des `@attributes` par rapport à la position des attributs d’élément est importante.</span><span class="sxs-lookup"><span data-stu-id="78772-215">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="78772-216">Lorsque `@attributes` sont réparties sur l’élément, les attributs sont traités de droite à gauche (dernier à premier).</span><span class="sxs-lookup"><span data-stu-id="78772-216">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="78772-217">Prenons l’exemple suivant d’un composant qui consomme un composant `Child` :</span><span class="sxs-lookup"><span data-stu-id="78772-217">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="78772-218">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-218">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="78772-219">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-219">*ChildComponent.razor*:</span></span>

```cshtml
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="78772-220">L’attribut `extra` du composant `Child` est défini à droite de `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="78772-220">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="78772-221">Le `<div>` rendu du composant `Parent` contient `extra="5"` lorsqu’il est transmis via l’attribut supplémentaire, car les attributs sont traités de droite à gauche (dernier à premier) :</span><span class="sxs-lookup"><span data-stu-id="78772-221">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="78772-222">Dans l’exemple suivant, l’ordre des `extra` et `@attributes` est inversé dans le `<div>`du composant `Child` :</span><span class="sxs-lookup"><span data-stu-id="78772-222">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="78772-223">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-223">*ParentComponent.razor*:</span></span>

```cshtml
<ChildComponent extra="10" />
```

<span data-ttu-id="78772-224">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-224">*ChildComponent.razor*:</span></span>

```cshtml
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="78772-225">Le `<div>` rendu dans le composant `Parent` contient `extra="10"` lorsqu’il est passé par le biais de l’attribut supplémentaire :</span><span class="sxs-lookup"><span data-stu-id="78772-225">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="78772-226">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="78772-226">Data binding</span></span>

<span data-ttu-id="78772-227">La liaison de données aux composants et aux éléments DOM s’effectue à l’aide de l’attribut [@bind](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="78772-227">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="78772-228">L’exemple suivant lie une propriété `CurrentValue` à la valeur de la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="78772-228">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="78772-229">Lorsque la zone de texte perd le focus, la valeur de la propriété est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="78772-229">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="78772-230">La zone de texte est mise à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, et non en réponse à la modification de la valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="78772-230">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="78772-231">Étant donné que les composants s’affichent après l’exécution du code du gestionnaire d’événements, les mises à jour de propriétés sont *généralement* reflétées dans l’interface utilisateur immédiatement après le déclenchement d’un gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="78772-231">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="78772-232">L’utilisation de `@bind` avec la propriété `CurrentValue` (`<input @bind="CurrentValue" />`) équivaut essentiellement à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="78772-232">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="78772-233">Lors du rendu du composant, le `value` de l’élément d’entrée provient de la propriété `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="78772-233">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="78772-234">Lorsque l’utilisateur tape dans la zone de texte et modifie le focus de l’élément, l’événement `onchange` est déclenché et la propriété `CurrentValue` est définie sur la valeur modifiée.</span><span class="sxs-lookup"><span data-stu-id="78772-234">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="78772-235">En réalité, la génération de code est plus complexe, car `@bind` gère les cas où les conversions de types sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="78772-235">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="78772-236">En principe, `@bind` associe la valeur actuelle d’une expression à un attribut `value` et gère les modifications à l’aide du gestionnaire inscrit.</span><span class="sxs-lookup"><span data-stu-id="78772-236">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="78772-237">En plus de gérer les événements `onchange` avec la syntaxe `@bind`, une propriété ou un champ peut être lié à l’aide d’autres événements en spécifiant un attribut [@bind-value](xref:mvc/views/razor#bind) avec un paramètre `event` ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="78772-237">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="78772-238">L’exemple suivant lie la propriété `CurrentValue` pour l’événement `oninput` :</span><span class="sxs-lookup"><span data-stu-id="78772-238">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="78772-239">Contrairement à `onchange`, qui se déclenche lorsque l’élément perd le focus, `oninput` se déclenche lorsque la valeur de la zone de texte change.</span><span class="sxs-lookup"><span data-stu-id="78772-239">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="78772-240">**Valeurs inanalysables**</span><span class="sxs-lookup"><span data-stu-id="78772-240">**Unparsable values**</span></span>

<span data-ttu-id="78772-241">Quand un utilisateur fournit une valeur non analysable à un élément DataBound, la valeur unanalysable est automatiquement rétablie à sa valeur précédente lorsque l’événement de liaison est déclenché.</span><span class="sxs-lookup"><span data-stu-id="78772-241">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="78772-242">Considérez le scénario suivant :</span><span class="sxs-lookup"><span data-stu-id="78772-242">Consider the following scenario:</span></span>

* <span data-ttu-id="78772-243">Un élément `<input>` est lié à un type `int` avec une valeur initiale de `123`:</span><span class="sxs-lookup"><span data-stu-id="78772-243">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="78772-244">L’utilisateur met à jour la valeur de l’élément à `123.45` dans la page et modifie le focus de l’élément.</span><span class="sxs-lookup"><span data-stu-id="78772-244">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="78772-245">Dans le scénario précédent, la valeur de l’élément est rétablie à `123`.</span><span class="sxs-lookup"><span data-stu-id="78772-245">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="78772-246">Lorsque la valeur `123.45` est rejetée en faveur de la valeur d’origine de `123`, l’utilisateur sait que sa valeur n’a pas été acceptée.</span><span class="sxs-lookup"><span data-stu-id="78772-246">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="78772-247">Par défaut, la liaison s’applique à l’événement `onchange` de l’élément (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="78772-247">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="78772-248">Utilisez `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` pour définir un événement différent.</span><span class="sxs-lookup"><span data-stu-id="78772-248">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="78772-249">Pour l’événement `oninput` (`@bind-value:event="oninput"`), la Reversion se produit après toute séquence de touches qui introduit une valeur non analysable.</span><span class="sxs-lookup"><span data-stu-id="78772-249">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="78772-250">Quand vous ciblez l’événement `oninput` avec un type lié `int`, un utilisateur ne peut pas taper un caractère `.`.</span><span class="sxs-lookup"><span data-stu-id="78772-250">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="78772-251">Si un caractère de `.` est immédiatement supprimé, l’utilisateur reçoit des commentaires immédiats que seuls des nombres entiers sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="78772-251">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="78772-252">Il existe des scénarios où le rétablissement de la valeur sur l’événement `oninput` n’est pas idéal, par exemple lorsque l’utilisateur doit être autorisé à effacer une valeur de `<input>` non analysable.</span><span class="sxs-lookup"><span data-stu-id="78772-252">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="78772-253">Les alternatives sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="78772-253">Alternatives include:</span></span>

* <span data-ttu-id="78772-254">N’utilisez pas l’événement `oninput`.</span><span class="sxs-lookup"><span data-stu-id="78772-254">Don't use the `oninput` event.</span></span> <span data-ttu-id="78772-255">Utilisez l’événement `onchange` par défaut (`@bind="{PROPERTY OR FIELD}"`), où une valeur non valide n’est pas rétablie tant que l’élément n’a pas perdu le focus.</span><span class="sxs-lookup"><span data-stu-id="78772-255">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="78772-256">Effectuer une liaison à un type Nullable, tel que `int?` ou `string`, et fournir une logique personnalisée pour gérer les entrées non valides.</span><span class="sxs-lookup"><span data-stu-id="78772-256">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="78772-257">Utilisez un [composant de validation de formulaire](xref:blazor/forms-validation), tel que `InputNumber` ou `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="78772-257">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="78772-258">Les composants de validation de formulaire offrent une prise en charge intégrée pour gérer les entrées non valides.</span><span class="sxs-lookup"><span data-stu-id="78772-258">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="78772-259">Composants de validation de formulaire :</span><span class="sxs-lookup"><span data-stu-id="78772-259">Form validation components:</span></span>
  * <span data-ttu-id="78772-260">Permet à l’utilisateur de fournir une entrée non valide et de recevoir des erreurs de validation sur les `EditContext`associées.</span><span class="sxs-lookup"><span data-stu-id="78772-260">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="78772-261">Affichez les erreurs de validation dans l’interface utilisateur sans interférer avec l’utilisateur qui saisit des données Webform supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="78772-261">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="78772-262">**Globalisation**</span><span class="sxs-lookup"><span data-stu-id="78772-262">**Globalization**</span></span>

<span data-ttu-id="78772-263">`@bind` valeurs sont mises en forme pour l’affichage et l’analyse à l’aide des règles de la culture actuelle.</span><span class="sxs-lookup"><span data-stu-id="78772-263">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="78772-264">La culture actuelle est accessible à partir de la propriété <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="78772-264">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="78772-265">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) est utilisé pour les types de champ suivants (`<input type="{TYPE}" />`) :</span><span class="sxs-lookup"><span data-stu-id="78772-265">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="78772-266">Les types de champ précédents :</span><span class="sxs-lookup"><span data-stu-id="78772-266">The preceding field types:</span></span>

* <span data-ttu-id="78772-267">Sont affichés à l’aide des règles de mise en forme appropriées basées sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="78772-267">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="78772-268">Ne peut pas contenir de texte de forme libre.</span><span class="sxs-lookup"><span data-stu-id="78772-268">Can't contain free-form text.</span></span>
* <span data-ttu-id="78772-269">Fournir des caractéristiques d’interaction de l’utilisateur en fonction de l’implémentation du navigateur.</span><span class="sxs-lookup"><span data-stu-id="78772-269">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="78772-270">Les types de champs suivants ont des exigences de mise en forme spécifiques et ne sont pas actuellement pris en charge par Blazor, car ils ne sont pas pris en charge par tous les principaux navigateurs :</span><span class="sxs-lookup"><span data-stu-id="78772-270">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="78772-271">`@bind` prend en charge le paramètre `@bind:culture` pour fournir une <xref:System.Globalization.CultureInfo?displayProperty=fullName> pour l’analyse et la mise en forme d’une valeur.</span><span class="sxs-lookup"><span data-stu-id="78772-271">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="78772-272">La spécification d’une culture n’est pas recommandée lors de l’utilisation des types de champ `date` et `number`.</span><span class="sxs-lookup"><span data-stu-id="78772-272">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="78772-273">`date` et `number` disposent d’une prise en charge intégrée Blazor qui fournit la culture requise.</span><span class="sxs-lookup"><span data-stu-id="78772-273">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="78772-274">Pour plus d’informations sur la façon de définir la culture de l’utilisateur, consultez la section [Localization](#localization) .</span><span class="sxs-lookup"><span data-stu-id="78772-274">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="78772-275">**Chaînes de format**</span><span class="sxs-lookup"><span data-stu-id="78772-275">**Format strings**</span></span>

<span data-ttu-id="78772-276">La liaison de données fonctionne avec les chaînes de format <xref:System.DateTime> à l’aide de [@bind:format](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="78772-276">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="78772-277">D’autres expressions de mise en forme, telles que les formats de devise ou de nombre, ne sont pas disponibles pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="78772-277">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="78772-278">Dans le code précédent, le type de champ de l’élément `<input>` (`type`) a par défaut la valeur `text`.</span><span class="sxs-lookup"><span data-stu-id="78772-278">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="78772-279">`@bind:format` est pris en charge pour la liaison des types .NET suivants :</span><span class="sxs-lookup"><span data-stu-id="78772-279">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="78772-280"><xref:System.DateTime?displayProperty=fullName> ?</span><span class="sxs-lookup"><span data-stu-id="78772-280"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="78772-281"><xref:System.DateTimeOffset?displayProperty=fullName> ?</span><span class="sxs-lookup"><span data-stu-id="78772-281"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="78772-282">L’attribut `@bind:format` spécifie le format de date à appliquer aux `value` de l’élément `<input>`.</span><span class="sxs-lookup"><span data-stu-id="78772-282">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="78772-283">Le format est également utilisé pour analyser la valeur lorsqu’un événement `onchange` se produit.</span><span class="sxs-lookup"><span data-stu-id="78772-283">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="78772-284">Il n’est pas recommandé de spécifier un format pour le type de champ `date`, car Blazor offre une prise en charge intégrée de la mise en forme des dates.</span><span class="sxs-lookup"><span data-stu-id="78772-284">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="78772-285">**Paramètres du composant**</span><span class="sxs-lookup"><span data-stu-id="78772-285">**Component parameters**</span></span>

<span data-ttu-id="78772-286">La liaison reconnaît les paramètres du composant, où `@bind-{property}` pouvez lier une valeur de propriété à plusieurs composants.</span><span class="sxs-lookup"><span data-stu-id="78772-286">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="78772-287">Le composant enfant suivant (`ChildComponent`) a un paramètre de composant `Year` et `YearChanged` rappel :</span><span class="sxs-lookup"><span data-stu-id="78772-287">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="78772-288">`EventCallback<T>` est expliqué dans la section [EventCallback suivante](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="78772-288">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="78772-289">Le composant parent suivant utilise `ChildComponent` et lie le paramètre `ParentYear` du parent au paramètre `Year` sur le composant enfant :</span><span class="sxs-lookup"><span data-stu-id="78772-289">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="78772-290">Le chargement de l' `ParentComponent` produit le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="78772-290">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="78772-291">Si la valeur de la propriété `ParentYear` est modifiée en sélectionnant le bouton dans la `ParentComponent`, la propriété `Year` de la `ChildComponent` est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="78772-291">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="78772-292">La nouvelle valeur de `Year` est rendue dans l’interface utilisateur lorsque le `ParentComponent` est restitué à nouveau :</span><span class="sxs-lookup"><span data-stu-id="78772-292">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="78772-293">Le paramètre `Year` peut être lié, car il a un `YearChanged` événement associé qui correspond au type du paramètre `Year`.</span><span class="sxs-lookup"><span data-stu-id="78772-293">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="78772-294">Par Convention, `<ChildComponent @bind-Year="ParentYear" />` revient essentiellement à écrire :</span><span class="sxs-lookup"><span data-stu-id="78772-294">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="78772-295">En général, une propriété peut être liée à un gestionnaire d’événements correspondant à l’aide de `@bind-property:event` attribut.</span><span class="sxs-lookup"><span data-stu-id="78772-295">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="78772-296">Par exemple, la propriété `MyProp` peut être liée à `MyEventHandler` à l’aide des deux attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="78772-296">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="78772-297">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="78772-297">Event handling</span></span>

<span data-ttu-id="78772-298">Les composants Razor fournissent des fonctionnalités de gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="78772-298">Razor components provide event handling features.</span></span> <span data-ttu-id="78772-299">Pour un attribut d’élément HTML nommé `on{EVENT}` (par exemple, `onclick` et `onsubmit`) avec une valeur de type délégué, les composants Razor traitent la valeur de l’attribut en tant que gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="78772-299">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="78772-300">Le nom de l’attribut est toujours mis en forme [@on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="78772-300">The attribute's name is always formatted [@on{EVENT}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="78772-301">Le code suivant appelle la méthode `UpdateHeading` lorsque le bouton est sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="78772-301">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
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

<span data-ttu-id="78772-302">Le code suivant appelle la méthode `CheckChanged` lorsque la case à cocher est modifiée dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="78772-302">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="78772-303">Les gestionnaires d’événements peuvent également être asynchrones et retourner un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="78772-303">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="78772-304">Il n’est pas nécessaire d’appeler manuellement [StateHasChanged](xref:blazor/lifecycle#state-changes).</span><span class="sxs-lookup"><span data-stu-id="78772-304">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="78772-305">Les exceptions sont journalisées lorsqu’elles se produisent.</span><span class="sxs-lookup"><span data-stu-id="78772-305">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="78772-306">Dans l’exemple suivant, `UpdateHeading` est appelé de façon asynchrone quand le bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="78772-306">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
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

### <a name="event-argument-types"></a><span data-ttu-id="78772-307">Types d’arguments d’événement</span><span class="sxs-lookup"><span data-stu-id="78772-307">Event argument types</span></span>

<span data-ttu-id="78772-308">Pour certains événements, les types d’arguments d’événement sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="78772-308">For some events, event argument types are permitted.</span></span> <span data-ttu-id="78772-309">Si l’accès à l’un de ces types d’événements n’est pas nécessaire, il n’est pas obligatoire dans l’appel de la méthode.</span><span class="sxs-lookup"><span data-stu-id="78772-309">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="78772-310">Les `EventArgs` prises en charge sont indiquées dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="78772-310">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="78772-311">Event</span><span class="sxs-lookup"><span data-stu-id="78772-311">Event</span></span>            | <span data-ttu-id="78772-312">Classe</span><span class="sxs-lookup"><span data-stu-id="78772-312">Class</span></span>                | <span data-ttu-id="78772-313">Remarques et événements DOM</span><span class="sxs-lookup"><span data-stu-id="78772-313">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="78772-314">Presse-papiers</span><span class="sxs-lookup"><span data-stu-id="78772-314">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="78772-315">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="78772-315">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="78772-316">Déplacez</span><span class="sxs-lookup"><span data-stu-id="78772-316">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="78772-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="78772-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="78772-318">`DataTransfer` et `DataTransferItem` contiennent des données d’élément glissées.</span><span class="sxs-lookup"><span data-stu-id="78772-318">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="78772-319">Erreur du</span><span class="sxs-lookup"><span data-stu-id="78772-319">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="78772-320">Event</span><span class="sxs-lookup"><span data-stu-id="78772-320">Event</span></span>            | `EventArgs`          | <span data-ttu-id="78772-321">*Général*</span><span class="sxs-lookup"><span data-stu-id="78772-321">*General*</span></span><br><span data-ttu-id="78772-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="78772-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="78772-323">*Presse-papiers*</span><span class="sxs-lookup"><span data-stu-id="78772-323">*Clipboard*</span></span><br><span data-ttu-id="78772-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="78772-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="78772-325">*Entrée*</span><span class="sxs-lookup"><span data-stu-id="78772-325">*Input*</span></span><br><span data-ttu-id="78772-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="78772-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="78772-327">*Media*</span><span class="sxs-lookup"><span data-stu-id="78772-327">*Media*</span></span><br><span data-ttu-id="78772-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="78772-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="78772-329">Focus</span><span class="sxs-lookup"><span data-stu-id="78772-329">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="78772-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="78772-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="78772-331">N’inclut pas la prise en charge de `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="78772-331">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="78772-332">Input</span><span class="sxs-lookup"><span data-stu-id="78772-332">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="78772-333">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="78772-333">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="78772-334">Clavier</span><span class="sxs-lookup"><span data-stu-id="78772-334">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="78772-335">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="78772-335">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="78772-336">Souris</span><span class="sxs-lookup"><span data-stu-id="78772-336">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="78772-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="78772-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="78772-338">Pointeur de la souris</span><span class="sxs-lookup"><span data-stu-id="78772-338">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="78772-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="78772-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="78772-340">Roulette de la souris</span><span class="sxs-lookup"><span data-stu-id="78772-340">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="78772-341">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="78772-341">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="78772-342">Progression</span><span class="sxs-lookup"><span data-stu-id="78772-342">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="78772-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="78772-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="78772-344">Entrées tactiles</span><span class="sxs-lookup"><span data-stu-id="78772-344">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="78772-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="78772-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="78772-346">`TouchPoint` représente un point de contact unique sur un appareil tactile.</span><span class="sxs-lookup"><span data-stu-id="78772-346">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="78772-347">Pour plus d’informations sur les propriétés et le comportement de gestion des événements des événements dans le tableau précédent, consultez [classes EventArgs dans la source de référence (ASPNET/AspNetCore Release/3.0 Branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="78772-347">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="78772-348">Expressions lambda</span><span class="sxs-lookup"><span data-stu-id="78772-348">Lambda expressions</span></span>

<span data-ttu-id="78772-349">Les expressions lambda peuvent également être utilisées :</span><span class="sxs-lookup"><span data-stu-id="78772-349">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="78772-350">Il est souvent pratique de se rapprocher de valeurs supplémentaires, par exemple lors de l’itération sur un ensemble d’éléments.</span><span class="sxs-lookup"><span data-stu-id="78772-350">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="78772-351">L’exemple suivant crée trois boutons, chacun d’entre eux appelant `UpdateHeading` passant un argument d’événement (`MouseEventArgs`) et son numéro de bouton (`buttonNumber`) lorsqu’il est sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="78772-351">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="78772-352">N’utilisez **pas** la variable de boucle (`i`) dans une boucle `for` directement dans une expression lambda.</span><span class="sxs-lookup"><span data-stu-id="78772-352">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="78772-353">Dans le cas contraire, la même variable est utilisée par toutes les expressions lambda provoquant la même valeur de `i`dans toutes les expressions lambda.</span><span class="sxs-lookup"><span data-stu-id="78772-353">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="78772-354">Capturez toujours sa valeur dans une variable locale (`buttonNumber` dans l’exemple précédent), puis utilisez-la.</span><span class="sxs-lookup"><span data-stu-id="78772-354">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="78772-355">EventCallback suivante</span><span class="sxs-lookup"><span data-stu-id="78772-355">EventCallback</span></span>

<span data-ttu-id="78772-356">Un scénario courant avec des composants imbriqués est le souhait d’exécuter la méthode d’un composant parent lorsqu’un événement de composant enfant se produit&mdash;par exemple, lorsqu’un événement `onclick` se produit dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="78772-356">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="78772-357">Pour exposer des événements entre les composants, utilisez un `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="78772-357">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="78772-358">Un composant parent peut affecter une méthode de rappel à l' `EventCallback`d’un composant enfant.</span><span class="sxs-lookup"><span data-stu-id="78772-358">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="78772-359">La `ChildComponent` dans l’exemple d’application montre comment le gestionnaire de `onclick` d’un bouton est configuré pour recevoir un délégué `EventCallback` de l' `ParentComponent`de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="78772-359">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="78772-360">Le `EventCallback` est tapé avec `MouseEventArgs`, ce qui est approprié pour un événement `onclick` à partir d’un périphérique :</span><span class="sxs-lookup"><span data-stu-id="78772-360">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="78772-361">Le `ParentComponent` définit le `EventCallback<T>` de l’enfant sur sa méthode `ShowMessage` :</span><span class="sxs-lookup"><span data-stu-id="78772-361">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="78772-362">Lorsque le bouton est sélectionné dans la `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="78772-362">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="78772-363">La méthode `ShowMessage` de `ParentComponent`est appelée.</span><span class="sxs-lookup"><span data-stu-id="78772-363">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="78772-364">`messageText` est mis à jour et affiché dans le `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="78772-364">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="78772-365">Un appel à [StateHasChanged](xref:blazor/lifecycle#state-changes) n’est pas requis dans la méthode du rappel (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="78772-365">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="78772-366">`StateHasChanged` est appelé automatiquement pour rerestituer le `ParentComponent`, tout comme les événements enfants déclenchent le rerendu des composants dans les gestionnaires d’événements qui s’exécutent dans l’enfant.</span><span class="sxs-lookup"><span data-stu-id="78772-366">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="78772-367">`EventCallback` et `EventCallback<T>` autorisent les délégués asynchrones.</span><span class="sxs-lookup"><span data-stu-id="78772-367">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="78772-368">`EventCallback<T>` est fortement typé et requiert un type d’argument spécifique.</span><span class="sxs-lookup"><span data-stu-id="78772-368">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="78772-369">`EventCallback` est faiblement typé et autorise tout type d’argument.</span><span class="sxs-lookup"><span data-stu-id="78772-369">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="78772-370">Appelez une `EventCallback` ou `EventCallback<T>` avec `InvokeAsync` et attendez la <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="78772-370">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="78772-371">Utilisez `EventCallback` et `EventCallback<T>` pour la gestion des événements et la liaison des paramètres du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-371">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="78772-372">Préférez la `EventCallback<T>` fortement typée sur `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="78772-372">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="78772-373">`EventCallback<T>` offre un meilleur retour d’erreur aux utilisateurs du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-373">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="78772-374">Comme pour d’autres gestionnaires d’événements d’interface utilisateur, la spécification du paramètre d’événement est facultative.</span><span class="sxs-lookup"><span data-stu-id="78772-374">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="78772-375">Utilisez `EventCallback` quand aucune valeur n’est passée au rappel.</span><span class="sxs-lookup"><span data-stu-id="78772-375">Use `EventCallback` when there's no value passed to the callback.</span></span>

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a><span data-ttu-id="78772-376">Empêcher les actions par défaut</span><span class="sxs-lookup"><span data-stu-id="78772-376">Prevent default actions</span></span>

<span data-ttu-id="78772-377">Utilisez l’attribut [@on{Event} :p directive reventdefault](xref:mvc/views/razor#oneventpreventdefault) pour empêcher l’action par défaut pour un événement.</span><span class="sxs-lookup"><span data-stu-id="78772-377">Use the [@on{EVENT}:preventDefault](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="78772-378">Quand une clé est sélectionnée sur un appareil d’entrée et que le focus de l’élément se trouve sur une zone de texte, un navigateur affiche normalement le caractère de la clé dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="78772-378">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="78772-379">Dans l’exemple suivant, le comportement par défaut est évité en spécifiant l’attribut de directive `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="78772-379">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="78772-380">Le compteur est incrémenté, et la clé de **+** n’est pas capturée dans la valeur de l’élément `<input>` :</span><span class="sxs-lookup"><span data-stu-id="78772-380">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```cshtml
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

<span data-ttu-id="78772-381">La spécification de l’attribut `@on{EVENT}:preventDefault` sans valeur est équivalente à `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="78772-381">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="78772-382">La valeur de l’attribut peut également être une expression.</span><span class="sxs-lookup"><span data-stu-id="78772-382">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="78772-383">Dans l’exemple suivant, `_shouldPreventDefault` est un champ `bool` défini sur `true` ou `false`:</span><span class="sxs-lookup"><span data-stu-id="78772-383">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```cshtml
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="78772-384">Un gestionnaire d’événements n’est pas nécessaire pour empêcher l’action par défaut.</span><span class="sxs-lookup"><span data-stu-id="78772-384">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="78772-385">Le gestionnaire d’événements et les scénarios d’action par défaut peuvent être utilisés indépendamment.</span><span class="sxs-lookup"><span data-stu-id="78772-385">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="78772-386">Arrêter la propagation des événements</span><span class="sxs-lookup"><span data-stu-id="78772-386">Stop event propagation</span></span>

<span data-ttu-id="78772-387">Utilisez l’attribut de directive [@on{Event} : stopPropagation](xref:mvc/views/razor#oneventstoppropagation) pour arrêter la propagation des événements.</span><span class="sxs-lookup"><span data-stu-id="78772-387">Use the [@on{EVENT}:stopPropagation](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="78772-388">Dans l’exemple suivant, la sélection de la case à cocher empêche les événements Click du deuxième `<div>` enfant de se propager vers le `<div>`parent :</span><span class="sxs-lookup"><span data-stu-id="78772-388">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```cshtml
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

::: moniker-end

## <a name="chained-bind"></a><span data-ttu-id="78772-389">Liaison chaînée</span><span class="sxs-lookup"><span data-stu-id="78772-389">Chained bind</span></span>

<span data-ttu-id="78772-390">Un scénario courant consiste à chaîner un paramètre lié aux données à un élément de page dans la sortie du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-390">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="78772-391">Ce scénario est appelé *liaison chaînée* car plusieurs niveaux de liaison se produisent simultanément.</span><span class="sxs-lookup"><span data-stu-id="78772-391">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="78772-392">Une liaison chaînée ne peut pas être implémentée avec `@bind` syntaxe dans l’élément de la page.</span><span class="sxs-lookup"><span data-stu-id="78772-392">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="78772-393">Le gestionnaire d’événements et la valeur doivent être spécifiés séparément.</span><span class="sxs-lookup"><span data-stu-id="78772-393">The event handler and value must be specified separately.</span></span> <span data-ttu-id="78772-394">Toutefois, un composant parent peut utiliser `@bind` syntaxe avec le paramètre du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-394">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="78772-395">Le composant `PasswordField` suivant (*passwordField. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="78772-395">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="78772-396">Définit la valeur d’un élément `<input>` sur une propriété `Password`.</span><span class="sxs-lookup"><span data-stu-id="78772-396">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="78772-397">Expose les modifications de la propriété `Password` à un composant parent avec un [EventCallback suivante](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="78772-397">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

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
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="78772-398">Le composant `PasswordField` est utilisé dans un autre composant :</span><span class="sxs-lookup"><span data-stu-id="78772-398">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="78772-399">Pour effectuer des vérifications ou des erreurs d’interruption sur le mot de passe dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="78772-399">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="78772-400">Créez un champ de stockage pour `Password` (`password` dans l’exemple de code suivant).</span><span class="sxs-lookup"><span data-stu-id="78772-400">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="78772-401">Effectuez les vérifications ou les erreurs d’interruption dans le `Password` Setter.</span><span class="sxs-lookup"><span data-stu-id="78772-401">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="78772-402">L’exemple suivant fournit un retour immédiat à l’utilisateur si un espace est utilisé dans la valeur du mot de passe :</span><span class="sxs-lookup"><span data-stu-id="78772-402">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
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
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="78772-403">Capturer des références à des composants</span><span class="sxs-lookup"><span data-stu-id="78772-403">Capture references to components</span></span>

<span data-ttu-id="78772-404">Les références de composant offrent un moyen de référencer une instance de composant afin que vous puissiez émettre des commandes vers cette instance, telles que `Show` ou `Reset`.</span><span class="sxs-lookup"><span data-stu-id="78772-404">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="78772-405">Pour capturer une référence de composant :</span><span class="sxs-lookup"><span data-stu-id="78772-405">To capture a component reference:</span></span>

* <span data-ttu-id="78772-406">Ajoutez un attribut [@ref](xref:mvc/views/razor#ref) au composant enfant.</span><span class="sxs-lookup"><span data-stu-id="78772-406">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="78772-407">Définissez un champ avec le même type que le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="78772-407">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="78772-408">Lors du rendu du composant, le champ `loginDialog` est rempli avec l’instance du composant enfant `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="78772-408">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="78772-409">Vous pouvez ensuite appeler des méthodes .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-409">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78772-410">La variable `loginDialog` n’est remplie qu’après le rendu du composant et sa sortie comprend l’élément `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="78772-410">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="78772-411">Jusqu’à ce stade, il n’y a rien à référencer.</span><span class="sxs-lookup"><span data-stu-id="78772-411">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="78772-412">Pour manipuler des références de composants après la fin du rendu du composant, utilisez les [méthodes OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="78772-412">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="78772-413">Bien que la capture de références de composant utilise une syntaxe similaire pour [capturer des références d’élément](xref:blazor/javascript-interop#capture-references-to-elements), il ne s’agit pas d’une fonctionnalité [JavaScript Interop](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="78772-413">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="78772-414">Les références de composants ne sont pas transmises au code JavaScript&mdash;elles sont utilisées uniquement dans le code .NET.</span><span class="sxs-lookup"><span data-stu-id="78772-414">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="78772-415">N’utilisez **pas** de références de composant pour muter l’état des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="78772-415">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="78772-416">Utilisez plutôt des paramètres déclaratifs normaux pour passer des données aux composants enfants.</span><span class="sxs-lookup"><span data-stu-id="78772-416">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="78772-417">L’utilisation de paramètres déclaratifs normaux entraîne le rerendu automatique des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="78772-417">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="78772-418">Appeler des méthodes de composant en externe pour mettre à jour l’État</span><span class="sxs-lookup"><span data-stu-id="78772-418">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="78772-419"> utilise un `SynchronizationContext` pour appliquer un seul thread logique d’exécution.</span><span class="sxs-lookup"><span data-stu-id="78772-419"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="78772-420">Les méthodes de [cycle de vie](xref:blazor/lifecycle) d’un composant et les rappels d’événements déclenchés par Blazor sont exécutés sur ce `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="78772-420">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="78772-421">Dans le cas où un composant doit être mis à jour en fonction d’un événement externe, tel qu’un minuteur ou d’autres notifications, utilisez la méthode `InvokeAsync`, qui sera réexpédiée à la `SynchronizationContext`du Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-421">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="78772-422">Par exemple, considérez un *service de notification* qui peut notifier n’importe quel composant d’écoute de l’État mis à jour :</span><span class="sxs-lookup"><span data-stu-id="78772-422">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="78772-423">Utilisation du `NotifierService` pour mettre à jour un composant :</span><span class="sxs-lookup"><span data-stu-id="78772-423">Usage of the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="78772-424">Dans l’exemple précédent, `NotifierService` appelle la méthode `OnNotify` du composant en dehors du `SynchronizationContext`de Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-424">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="78772-425">`InvokeAsync` est utilisé pour basculer vers le contexte correct et pour la mise en file d’attente d’un rendu.</span><span class="sxs-lookup"><span data-stu-id="78772-425">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="78772-426">Utiliser \@clé pour contrôler la conservation des éléments et des composants</span><span class="sxs-lookup"><span data-stu-id="78772-426">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="78772-427">Lors du rendu d’une liste d’éléments ou de composants, et que les éléments ou composants changent par la suite, l’algorithme de différenciation de Blazordoit déterminer lequel des éléments ou composants précédents peuvent être conservés et comment les objets de modèle doivent être mappés à eux.</span><span class="sxs-lookup"><span data-stu-id="78772-427">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="78772-428">Normalement, ce processus est automatique et peut être ignoré, mais dans certains cas, il peut être utile de contrôler le processus.</span><span class="sxs-lookup"><span data-stu-id="78772-428">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="78772-429">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="78772-429">Consider the following example:</span></span>

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

<span data-ttu-id="78772-430">Le contenu de la collection `People` peut changer avec des entrées insérées, supprimées ou réorganisées.</span><span class="sxs-lookup"><span data-stu-id="78772-430">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="78772-431">Lors du rerendu du composant, le composant `<DetailsEditor>` peut changer pour recevoir différentes valeurs de paramètres de `Details`.</span><span class="sxs-lookup"><span data-stu-id="78772-431">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="78772-432">Cela peut entraîner un rerendu plus complexe que prévu.</span><span class="sxs-lookup"><span data-stu-id="78772-432">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="78772-433">Dans certains cas, le rerendu peut entraîner des différences de comportement visibles, telles que le focus de l’élément perdu.</span><span class="sxs-lookup"><span data-stu-id="78772-433">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="78772-434">Le processus de mappage peut être contrôlé à l’aide de l’attribut de directive `@key`.</span><span class="sxs-lookup"><span data-stu-id="78772-434">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="78772-435">`@key` oblige l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé :</span><span class="sxs-lookup"><span data-stu-id="78772-435">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="78772-436">En cas de modification de la collection `People`, l’algorithme de comparaison conserve l’association entre les instances de `<DetailsEditor>` et les instances `person` :</span><span class="sxs-lookup"><span data-stu-id="78772-436">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="78772-437">Si un `Person` est supprimé de la liste `People`, seule l’instance `<DetailsEditor>` correspondante est supprimée de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-437">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="78772-438">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="78772-438">Other instances are left unchanged.</span></span>
* <span data-ttu-id="78772-439">Si une `Person` est insérée à une position dans la liste, une nouvelle instance de `<DetailsEditor>` est insérée à cette position correspondante.</span><span class="sxs-lookup"><span data-stu-id="78772-439">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="78772-440">Les autres instances restent inchangées.</span><span class="sxs-lookup"><span data-stu-id="78772-440">Other instances are left unchanged.</span></span>
* <span data-ttu-id="78772-441">Si `Person` entrées sont réordonnées, les instances de `<DetailsEditor>` correspondantes sont conservées et réordonnées dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-441">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="78772-442">Dans certains scénarios, l’utilisation de `@key` réduit la complexité du rerendu et évite les problèmes potentiels liés aux parties avec état de la modification du modèle DOM, telles que la position du focus.</span><span class="sxs-lookup"><span data-stu-id="78772-442">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78772-443">Les clés sont locales pour chaque élément ou composant conteneur.</span><span class="sxs-lookup"><span data-stu-id="78772-443">Keys are local to each container element or component.</span></span> <span data-ttu-id="78772-444">Les clés ne sont pas comparées globalement dans le document.</span><span class="sxs-lookup"><span data-stu-id="78772-444">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="78772-445">Quand utiliser \@clé</span><span class="sxs-lookup"><span data-stu-id="78772-445">When to use \@key</span></span>

<span data-ttu-id="78772-446">En général, il est logique d’utiliser `@key` chaque fois qu’une liste est rendue (par exemple, dans un bloc `@foreach`) et qu’une valeur appropriée existe pour définir le `@key`.</span><span class="sxs-lookup"><span data-stu-id="78772-446">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="78772-447">Vous pouvez également utiliser `@key` pour empêcher Blazor de conserver un élément ou une sous-arborescence de composant quand un objet change :</span><span class="sxs-lookup"><span data-stu-id="78772-447">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="78772-448">Si `@currentPerson` change, la directive d’attribut `@key` force Blazor à ignorer l’intégralité du `<div>` et ses descendants, et à reconstruire la sous-arborescence de l’interface utilisateur avec de nouveaux éléments et composants.</span><span class="sxs-lookup"><span data-stu-id="78772-448">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="78772-449">Cela peut être utile si vous avez besoin de garantir qu’aucun État d’interface utilisateur n’est préservé lorsque `@currentPerson` change.</span><span class="sxs-lookup"><span data-stu-id="78772-449">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="78772-450">Quand ne pas utiliser la clé de \@</span><span class="sxs-lookup"><span data-stu-id="78772-450">When not to use \@key</span></span>

<span data-ttu-id="78772-451">Il y a un coût en matière de performances lors de la comparaison avec `@key`.</span><span class="sxs-lookup"><span data-stu-id="78772-451">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="78772-452">Le coût des performances n’est pas important, mais spécifiez uniquement `@key` si les règles de conservation des éléments ou des composants bénéficient de l’application.</span><span class="sxs-lookup"><span data-stu-id="78772-452">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="78772-453">Même si `@key` n’est pas utilisé, Blazor conserve autant que possible les instances d’élément et de composant enfants.</span><span class="sxs-lookup"><span data-stu-id="78772-453">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="78772-454">Le seul avantage de l’utilisation de `@key` est de contrôler la *façon dont* les instances de modèle sont mappées aux instances de composant conservées, au lieu de l’algorithme de comparaison qui sélectionne le mappage.</span><span class="sxs-lookup"><span data-stu-id="78772-454">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="78772-455">Valeurs à utiliser pour \@clé</span><span class="sxs-lookup"><span data-stu-id="78772-455">What values to use for \@key</span></span>

<span data-ttu-id="78772-456">En règle générale, il est logique de fournir l’un des types de valeur suivants pour `@key`:</span><span class="sxs-lookup"><span data-stu-id="78772-456">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="78772-457">Instances d’objet de modèle (par exemple, une instance de `Person` comme dans l’exemple précédent).</span><span class="sxs-lookup"><span data-stu-id="78772-457">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="78772-458">Cela garantit la préservation en fonction de l’égalité des références d’objet.</span><span class="sxs-lookup"><span data-stu-id="78772-458">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="78772-459">Identificateurs uniques (par exemple, les valeurs de clé primaire de type `int`, `string`ou `Guid`).</span><span class="sxs-lookup"><span data-stu-id="78772-459">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="78772-460">Assurez-vous que les valeurs utilisées pour `@key` ne sont pas en conflit.</span><span class="sxs-lookup"><span data-stu-id="78772-460">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="78772-461">Si les valeurs en conflit sont détectées dans le même élément parent, Blazor lève une exception, car il ne peut pas mapper de manière déterministe les anciens éléments ou composants aux nouveaux éléments ou composants.</span><span class="sxs-lookup"><span data-stu-id="78772-461">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="78772-462">Utilisez uniquement des valeurs distinctes, telles que des instances d’objets ou des valeurs de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="78772-462">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="78772-463">Routage</span><span class="sxs-lookup"><span data-stu-id="78772-463">Routing</span></span>

<span data-ttu-id="78772-464">Le routage dans Blazor est obtenu en fournissant un modèle de routage à chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="78772-464">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="78772-465">Lorsqu’un fichier Razor avec une directive `@page` est compilé, la classe générée reçoit une <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="78772-465">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="78772-466">Lors de l’exécution, le routeur recherche les classes de composant avec un `RouteAttribute` et rend le composant qui a un modèle de routage correspondant à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="78772-466">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="78772-467">Plusieurs modèles de routage peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="78772-467">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="78772-468">Le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="78772-468">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="78772-469">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="78772-469">Route parameters</span></span>

<span data-ttu-id="78772-470">Les composants peuvent recevoir des paramètres de routage à partir du modèle de routage fourni dans la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="78772-470">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="78772-471">Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="78772-471">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="78772-472">*Composant de paramètre de routage*:</span><span class="sxs-lookup"><span data-stu-id="78772-472">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="78772-473">Les paramètres facultatifs ne sont pas pris en charge. deux directives `@page` sont donc appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="78772-473">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="78772-474">La première permet de naviguer jusqu’au composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="78772-474">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="78772-475">La deuxième `@page` directive prend le paramètre d’itinéraire `{text}` et assigne la valeur à la propriété `Text`.</span><span class="sxs-lookup"><span data-stu-id="78772-475">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="78772-476">La syntaxe de paramètre *catch-all* (`*`/`**`), qui capture le chemin d’accès dans plusieurs limites de dossiers, n’est **pas** prise en charge dans les composants Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="78772-476">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="78772-477">Prise en charge des classes partielles</span><span class="sxs-lookup"><span data-stu-id="78772-477">Partial class support</span></span>

<span data-ttu-id="78772-478">Les composants Razor sont générés en tant que classes partielles.</span><span class="sxs-lookup"><span data-stu-id="78772-478">Razor components are generated as partial classes.</span></span> <span data-ttu-id="78772-479">Les composants Razor sont créés à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="78772-479">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="78772-480">C#le code est défini dans un bloc [@code](xref:mvc/views/razor#code) avec le balisage HTML et le code Razor dans un fichier unique.</span><span class="sxs-lookup"><span data-stu-id="78772-480">C# code is defined in an [@code](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="78772-481">les modèles de Blazor définissent leurs composants Razor à l’aide de cette approche.</span><span class="sxs-lookup"><span data-stu-id="78772-481">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="78772-482">C#le code est placé dans un fichier code-behind défini en tant que classe partielle.</span><span class="sxs-lookup"><span data-stu-id="78772-482">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="78772-483">L’exemple suivant montre le composant `Counter` par défaut avec un bloc `@code` dans une application générée à partir d’un modèle Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-483">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="78772-484">Le balisage HTML, le code C# Razor et le code se trouvent dans le même fichier :</span><span class="sxs-lookup"><span data-stu-id="78772-484">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="78772-485">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-485">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="78772-486">Le composant `Counter` peut également être créé à l’aide d’un fichier code-behind avec une classe partielle :</span><span class="sxs-lookup"><span data-stu-id="78772-486">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="78772-487">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-487">*Counter.razor*:</span></span>

```cshtml
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="78772-488">*Counter.Razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="78772-488">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

## <a name="specify-a-component-base-class"></a><span data-ttu-id="78772-489">Spécifier une classe de base de composant</span><span class="sxs-lookup"><span data-stu-id="78772-489">Specify a component base class</span></span>

<span data-ttu-id="78772-490">La directive `@inherits` peut être utilisée pour spécifier une classe de base pour un composant.</span><span class="sxs-lookup"><span data-stu-id="78772-490">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="78772-491">L' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) montre comment un composant peut hériter d’une classe de base, `BlazorRocksBase`, pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-491">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="78772-492">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="78772-492">*Pages/BlazorRocks.razor*:</span></span>

```cshtml
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="78772-493">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="78772-493">*BlazorRocksBase.cs*:</span></span>

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

<span data-ttu-id="78772-494">La classe de base doit dériver de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="78772-494">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="78772-495">Importer des composants</span><span class="sxs-lookup"><span data-stu-id="78772-495">Import components</span></span>

<span data-ttu-id="78772-496">L’espace de noms d’un composant créé avec Razor est basé sur (par ordre de priorité) :</span><span class="sxs-lookup"><span data-stu-id="78772-496">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="78772-497">[@namespace](xref:mvc/views/razor#namespace) la désignation dans le balisage du fichier Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="78772-497">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="78772-498">`RootNamespace` du projet dans le fichier projet (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="78772-498">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="78772-499">Nom du projet, pris à partir du nom de fichier du fichier projet ( *. csproj*), et chemin d’accès de la racine du projet au composant.</span><span class="sxs-lookup"><span data-stu-id="78772-499">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="78772-500">Par exemple, le Framework résout *{Project root}/pages/index.Razor* (*BlazorSample. csproj*) en l’espace de noms `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="78772-500">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="78772-501">Les composants C# suivent les règles de liaison de nom.</span><span class="sxs-lookup"><span data-stu-id="78772-501">Components follow C# name binding rules.</span></span> <span data-ttu-id="78772-502">Pour le composant `Index` dans cet exemple, les composants de l’étendue sont tous les composants :</span><span class="sxs-lookup"><span data-stu-id="78772-502">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="78772-503">Dans le même dossier, *pages*.</span><span class="sxs-lookup"><span data-stu-id="78772-503">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="78772-504">Composants de la racine du projet qui ne spécifient pas explicitement un espace de noms différent.</span><span class="sxs-lookup"><span data-stu-id="78772-504">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="78772-505">Les composants définis dans un espace de noms différent sont placés dans la portée à l’aide de la directive [@using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="78772-505">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="78772-506">Si un autre composant, `NavMenu.razor`, existe dans le dossier *BlazorSample/Shared/* , le composant peut être utilisé dans `Index.razor` avec l’instruction `@using` suivante :</span><span class="sxs-lookup"><span data-stu-id="78772-506">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="78772-507">Les composants peuvent également être référencés à l’aide de leurs noms complets, ce qui ne nécessite pas la directive [@using](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="78772-507">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="78772-508">La qualification de `global::` n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-508">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="78772-509">L’importation de composants avec des instructions `using` avec alias (par exemple, `@using Foo = Bar`) n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-509">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="78772-510">Les noms partiellement qualifiés ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-510">Partially qualified names aren't supported.</span></span> <span data-ttu-id="78772-511">Par exemple, l’ajout de `@using BlazorSample` et la référencement d' `NavMenu.razor` avec `<Shared.NavMenu></Shared.NavMenu>` ne sont pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-511">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="78772-512">Attributs d’éléments HTML conditionnels</span><span class="sxs-lookup"><span data-stu-id="78772-512">Conditional HTML element attributes</span></span>

<span data-ttu-id="78772-513">Les attributs des éléments HTML sont rendus de manière conditionnelle en fonction de la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="78772-513">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="78772-514">Si la valeur est `false` ou `null`, l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="78772-514">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="78772-515">Si la valeur est `true`, l’attribut est rendu réduit.</span><span class="sxs-lookup"><span data-stu-id="78772-515">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="78772-516">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est restitué dans le balisage de l’élément :</span><span class="sxs-lookup"><span data-stu-id="78772-516">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="78772-517">Si `IsCompleted` est `true`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="78772-517">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="78772-518">Si `IsCompleted` est `false`, la case à cocher s’affiche comme suit :</span><span class="sxs-lookup"><span data-stu-id="78772-518">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="78772-519">Pour plus d'informations, consultez <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="78772-519">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="78772-520">Certains attributs HTML, tels que [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), ne fonctionnent pas correctement quand le type .net est un `bool`.</span><span class="sxs-lookup"><span data-stu-id="78772-520">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="78772-521">Dans ce cas, utilisez un type de `string` à la place d’un `bool`.</span><span class="sxs-lookup"><span data-stu-id="78772-521">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="78772-522">HTML brut</span><span class="sxs-lookup"><span data-stu-id="78772-522">Raw HTML</span></span>

<span data-ttu-id="78772-523">Les chaînes sont normalement rendues à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage qu’elles peuvent contenir est ignorée et traitée comme du texte littéral.</span><span class="sxs-lookup"><span data-stu-id="78772-523">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="78772-524">Pour afficher le code HTML brut, encapsulez le contenu HTML dans une valeur `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="78772-524">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="78772-525">La valeur est analysée au format HTML ou SVG et est insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="78772-525">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="78772-526">Le rendu de code HTML brut construit à partir de n’importe quelle source non approuvée constitue un risque pour la **sécurité** et doit être évité !</span><span class="sxs-lookup"><span data-stu-id="78772-526">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="78772-527">L’exemple suivant illustre l’utilisation du type `MarkupString` pour ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :</span><span class="sxs-lookup"><span data-stu-id="78772-527">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="78772-528">Composants basés sur un modèle</span><span class="sxs-lookup"><span data-stu-id="78772-528">Templated components</span></span>

<span data-ttu-id="78772-529">Les composants basés sur un modèle sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, qui peuvent ensuite être utilisés dans le cadre de la logique de rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="78772-529">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="78772-530">Les composants basés sur un modèle vous permettent de créer des composants de niveau supérieur qui sont plus réutilisables que les composants normaux.</span><span class="sxs-lookup"><span data-stu-id="78772-530">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="78772-531">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="78772-531">A couple of examples include:</span></span>

* <span data-ttu-id="78772-532">Composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête, les lignes et le pied de page de la table.</span><span class="sxs-lookup"><span data-stu-id="78772-532">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="78772-533">Composant de liste qui permet à un utilisateur de spécifier un modèle pour afficher des éléments dans une liste.</span><span class="sxs-lookup"><span data-stu-id="78772-533">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="78772-534">Paramètres de modèle</span><span class="sxs-lookup"><span data-stu-id="78772-534">Template parameters</span></span>

<span data-ttu-id="78772-535">Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres de composant de type `RenderFragment` ou `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="78772-535">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="78772-536">Un fragment de rendu représente un segment de l’interface utilisateur à restituer.</span><span class="sxs-lookup"><span data-stu-id="78772-536">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="78772-537">`RenderFragment<T>` prend un paramètre de type qui peut être spécifié lors de l’appel du fragment de rendu.</span><span class="sxs-lookup"><span data-stu-id="78772-537">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="78772-538">composant `TableTemplate` :</span><span class="sxs-lookup"><span data-stu-id="78772-538">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="78772-539">Lorsque vous utilisez un composant basé sur un modèle, les paramètres de modèle peuvent être spécifiés à l’aide d’éléments enfants qui correspondent aux noms des paramètres (`TableHeader` et `RowTemplate` dans l’exemple suivant) :</span><span class="sxs-lookup"><span data-stu-id="78772-539">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
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

### <a name="template-context-parameters"></a><span data-ttu-id="78772-540">Paramètres de contexte de modèle</span><span class="sxs-lookup"><span data-stu-id="78772-540">Template context parameters</span></span>

<span data-ttu-id="78772-541">Les arguments de composant de type `RenderFragment<T>` passés comme éléments ont un paramètre implicite nommé `context` (par exemple, à partir de l’exemple de code précédent, `@context.PetId`), mais vous pouvez modifier le nom de paramètre à l’aide de l’attribut `Context` sur l’élément enfant.</span><span class="sxs-lookup"><span data-stu-id="78772-541">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="78772-542">Dans l’exemple suivant, l’attribut `Context` de l’élément `RowTemplate` spécifie le paramètre `pet` :</span><span class="sxs-lookup"><span data-stu-id="78772-542">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
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

<span data-ttu-id="78772-543">Vous pouvez également spécifier l’attribut `Context` sur l’élément Component.</span><span class="sxs-lookup"><span data-stu-id="78772-543">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="78772-544">L’attribut `Context` spécifié s’applique à tous les paramètres de modèle spécifiés.</span><span class="sxs-lookup"><span data-stu-id="78772-544">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="78772-545">Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans élément enfant d’encapsulation).</span><span class="sxs-lookup"><span data-stu-id="78772-545">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="78772-546">Dans l’exemple suivant, l’attribut `Context` apparaît sur l’élément `TableTemplate` et s’applique à tous les paramètres de modèle :</span><span class="sxs-lookup"><span data-stu-id="78772-546">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
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

### <a name="generic-typed-components"></a><span data-ttu-id="78772-547">Composants génériques</span><span class="sxs-lookup"><span data-stu-id="78772-547">Generic-typed components</span></span>

<span data-ttu-id="78772-548">Les composants basés sur un modèle sont souvent typés de façon générique.</span><span class="sxs-lookup"><span data-stu-id="78772-548">Templated components are often generically typed.</span></span> <span data-ttu-id="78772-549">Par exemple, un composant générique `ListViewTemplate` peut être utilisé pour restituer des valeurs `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="78772-549">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="78772-550">Pour définir un composant générique, utilisez la directive [@typeparam](xref:mvc/views/razor#typeparam) pour spécifier les paramètres de type :</span><span class="sxs-lookup"><span data-stu-id="78772-550">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="78772-551">Lorsque vous utilisez des composants génériques, le paramètre de type est déduit si possible :</span><span class="sxs-lookup"><span data-stu-id="78772-551">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="78772-552">Sinon, le paramètre de type doit être spécifié explicitement à l’aide d’un attribut qui correspond au nom du paramètre de type.</span><span class="sxs-lookup"><span data-stu-id="78772-552">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="78772-553">Dans l’exemple suivant, `TItem="Pet"` spécifie le type :</span><span class="sxs-lookup"><span data-stu-id="78772-553">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="78772-554">Valeurs et paramètres en cascade</span><span class="sxs-lookup"><span data-stu-id="78772-554">Cascading values and parameters</span></span>

<span data-ttu-id="78772-555">Dans certains scénarios, il est peu commode de transmettre des données d’un composant ancêtre à un composant descendant à l’aide de [paramètres de composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant.</span><span class="sxs-lookup"><span data-stu-id="78772-555">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="78772-556">Les valeurs et paramètres en cascade résolvent ce problème en fournissant un moyen pratique pour un composant ancêtre de fournir une valeur à tous ses composants descendants.</span><span class="sxs-lookup"><span data-stu-id="78772-556">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="78772-557">Les valeurs et les paramètres en cascade fournissent également une approche pour les composants à coordonner.</span><span class="sxs-lookup"><span data-stu-id="78772-557">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="78772-558">Exemple de thème</span><span class="sxs-lookup"><span data-stu-id="78772-558">Theme example</span></span>

<span data-ttu-id="78772-559">Dans l’exemple suivant tiré de l’exemple d’application, la classe `ThemeInfo` spécifie les informations de thème pour descendre dans la hiérarchie des composants afin que tous les boutons d’une partie donnée de l’application partagent le même style.</span><span class="sxs-lookup"><span data-stu-id="78772-559">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="78772-560">*UIThemeClasses/themeinfo. cs*:</span><span class="sxs-lookup"><span data-stu-id="78772-560">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="78772-561">Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="78772-561">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="78772-562">Le composant `CascadingValue` encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants de cette sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="78772-562">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="78772-563">Par exemple, l’exemple d’application spécifie les informations de thème (`ThemeInfo`) dans l’une des dispositions de l’application en tant que paramètre en cascade pour tous les composants qui composent le corps de la disposition de la propriété `@Body`.</span><span class="sxs-lookup"><span data-stu-id="78772-563">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="78772-564">une valeur de `btn-success` dans le composant de disposition est assignée à `ButtonClass`.</span><span class="sxs-lookup"><span data-stu-id="78772-564">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="78772-565">Tout composant descendant peut consommer cette propriété par le biais du `ThemeInfo` objet en cascade.</span><span class="sxs-lookup"><span data-stu-id="78772-565">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="78772-566">composant `CascadingValuesParametersLayout` :</span><span class="sxs-lookup"><span data-stu-id="78772-566">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
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

<span data-ttu-id="78772-567">Pour utiliser des valeurs en cascade, les composants déclarent des paramètres en cascade à l’aide de l’attribut `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="78772-567">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="78772-568">Les valeurs en cascade sont liées aux paramètres en cascade par type.</span><span class="sxs-lookup"><span data-stu-id="78772-568">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="78772-569">Dans l’exemple d’application, le composant `CascadingValuesParametersTheme` lie la valeur en cascade `ThemeInfo` à un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="78772-569">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="78772-570">Le paramètre est utilisé pour définir la classe CSS pour l’un des boutons affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="78772-570">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="78772-571">composant `CascadingValuesParametersTheme` :</span><span class="sxs-lookup"><span data-stu-id="78772-571">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="78772-572">Pour mettre en cascade plusieurs valeurs du même type dans la même sous-arborescence, fournissez une chaîne de `Name` unique à chaque composant `CascadingValue` et à la `CascadingParameter`correspondante.</span><span class="sxs-lookup"><span data-stu-id="78772-572">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="78772-573">Dans l’exemple suivant, deux composants de `CascadingValue` cascadent différentes instances de `MyCascadingType` par nom :</span><span class="sxs-lookup"><span data-stu-id="78772-573">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="78772-574">Dans un composant descendant, les paramètres en cascade reçoivent leurs valeurs des valeurs en cascade correspondantes dans le composant ancêtre par nom :</span><span class="sxs-lookup"><span data-stu-id="78772-574">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="78772-575">Exemple TabSet</span><span class="sxs-lookup"><span data-stu-id="78772-575">TabSet example</span></span>

<span data-ttu-id="78772-576">Les paramètres en cascade permettent également aux composants de collaborer au sein de la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="78772-576">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="78772-577">Par exemple, considérez l’exemple *TabSet* suivant dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="78772-577">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="78772-578">L’exemple d’application possède une interface `ITab` que les onglets implémentent :</span><span class="sxs-lookup"><span data-stu-id="78772-578">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="78772-579">Le composant `CascadingValuesParametersTabSet` utilise le composant `TabSet`, qui contient plusieurs composants `Tab` :</span><span class="sxs-lookup"><span data-stu-id="78772-579">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="78772-580">Les composants `Tab` enfants ne sont pas explicitement passés comme paramètres au `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="78772-580">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="78772-581">Au lieu de cela, les composants de `Tab` enfants font partie du contenu enfant du `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="78772-581">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="78772-582">Toutefois, le `TabSet` doit toujours connaître chaque composant `Tab` afin qu’il puisse restituer les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter de code supplémentaire, le composant `TabSet` *peut se présenter comme une valeur en cascade* qui est ensuite récupérée par les composants de `Tab` descendants.</span><span class="sxs-lookup"><span data-stu-id="78772-582">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="78772-583">composant `TabSet` :</span><span class="sxs-lookup"><span data-stu-id="78772-583">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="78772-584">Les composants de `Tab` descendants capturent le `TabSet` contenant sous forme de paramètre en cascade, de sorte que les composants `Tab` s’ajoutent eux-mêmes au `TabSet` et coordonnent l’onglet actif.</span><span class="sxs-lookup"><span data-stu-id="78772-584">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="78772-585">composant `Tab` :</span><span class="sxs-lookup"><span data-stu-id="78772-585">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="78772-586">Modèles Razor</span><span class="sxs-lookup"><span data-stu-id="78772-586">Razor templates</span></span>

<span data-ttu-id="78772-587">Les fragments de rendu peuvent être définis à l’aide de la syntaxe de modèle Razor.</span><span class="sxs-lookup"><span data-stu-id="78772-587">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="78772-588">Les modèles Razor sont un moyen de définir un extrait de code d’interface utilisateur et de supposer le format suivant :</span><span class="sxs-lookup"><span data-stu-id="78772-588">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="78772-589">L’exemple suivant montre comment spécifier des valeurs `RenderFragment` et `RenderFragment<T>` et afficher des modèles directement dans un composant.</span><span class="sxs-lookup"><span data-stu-id="78772-589">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="78772-590">Les fragments de rendu peuvent également être passés comme arguments à des [composants basés](#templated-components)sur un modèle.</span><span class="sxs-lookup"><span data-stu-id="78772-590">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="78772-591">Sortie rendue du code précédent :</span><span class="sxs-lookup"><span data-stu-id="78772-591">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="78772-592">Logique RenderTreeBuilder manuelle</span><span class="sxs-lookup"><span data-stu-id="78772-592">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="78772-593">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fournit des méthodes pour la manipulation des composants et des éléments, notamment la C# génération manuelle de composants dans le code.</span><span class="sxs-lookup"><span data-stu-id="78772-593">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="78772-594">L’utilisation de `RenderTreeBuilder` pour créer des composants est un scénario avancé.</span><span class="sxs-lookup"><span data-stu-id="78772-594">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="78772-595">Un composant mal formé (par exemple, une balise de balisage non fermée) peut entraîner un comportement indéfini.</span><span class="sxs-lookup"><span data-stu-id="78772-595">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="78772-596">Prenons le composant `PetDetails` suivant, qui peut être intégré manuellement à un autre composant :</span><span class="sxs-lookup"><span data-stu-id="78772-596">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="78772-597">Dans l’exemple suivant, la boucle de la méthode `CreateComponent` génère trois composants `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="78772-597">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="78772-598">Lors de l’appel de méthodes `RenderTreeBuilder` pour créer les composants (`OpenComponent` et `AddAttribute`), les numéros de séquence sont des numéros de ligne de code source.</span><span class="sxs-lookup"><span data-stu-id="78772-598">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="78772-599">L’algorithme de différence Blazor s’appuie sur les numéros de séquence correspondant à des lignes de code distinctes, et non sur des appels d’appel distincts.</span><span class="sxs-lookup"><span data-stu-id="78772-599">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="78772-600">Lors de la création d’un composant avec des méthodes `RenderTreeBuilder`, coder en dur les arguments pour les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="78772-600">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="78772-601">**L’utilisation d’un calcul ou d’un compteur pour générer le numéro de séquence peut entraîner des performances médiocres.**</span><span class="sxs-lookup"><span data-stu-id="78772-601">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="78772-602">Pour plus d’informations, consultez la section [numéros de séquence liés à des numéros de ligne de code et non à un ordre d’exécution](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="78772-602">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="78772-603">composant `BuiltContent` :</span><span class="sxs-lookup"><span data-stu-id="78772-603">`BuiltContent` component:</span></span>

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

> <span data-ttu-id="78772-604">! TRES Les types dans `Microsoft.AspNetCore.Components.RenderTree` permettent le traitement des *résultats* des opérations de rendu.</span><span class="sxs-lookup"><span data-stu-id="78772-604">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="78772-605">Il s’agit des détails internes de l’implémentation du Framework Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-605">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="78772-606">Ces types doivent être considérés comme *instables* et susceptibles d’être modifiés dans les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="78772-606">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="78772-607">Les numéros de séquence sont liés aux numéros de ligne de code et non à l’ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="78772-607">Sequence numbers relate to code line numbers and not execution order</span></span>

Blazor<span data-ttu-id="78772-608"> `.razor` fichiers sont toujours compilés.</span><span class="sxs-lookup"><span data-stu-id="78772-608"> `.razor` files are always compiled.</span></span> <span data-ttu-id="78772-609">C’est un avantage considérable pour `.razor`, car l’étape de compilation peut être utilisée pour injecter des informations qui améliorent les performances de l’application au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="78772-609">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="78772-610">Un exemple clé de ces améliorations concerne les *numéros de séquence*.</span><span class="sxs-lookup"><span data-stu-id="78772-610">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="78772-611">Les numéros de séquence indiquent au runtime que les sorties proviennent de lignes de code distinctes et ordonnées.</span><span class="sxs-lookup"><span data-stu-id="78772-611">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="78772-612">Le runtime utilise ces informations pour générer des différences d’arborescence efficaces en temps linéaire, ce qui est beaucoup plus rapide qu’un algorithme de comparaison d’arborescence générale.</span><span class="sxs-lookup"><span data-stu-id="78772-612">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="78772-613">Considérez le fichier `.razor` simple suivant :</span><span class="sxs-lookup"><span data-stu-id="78772-613">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="78772-614">Le code précédent se compile comme suit :</span><span class="sxs-lookup"><span data-stu-id="78772-614">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="78772-615">Lorsque le code s’exécute pour la première fois, si `someFlag` est `true`, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="78772-615">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="78772-616">Séquence</span><span class="sxs-lookup"><span data-stu-id="78772-616">Sequence</span></span> | <span data-ttu-id="78772-617">Type</span><span class="sxs-lookup"><span data-stu-id="78772-617">Type</span></span>      | <span data-ttu-id="78772-618">Données</span><span class="sxs-lookup"><span data-stu-id="78772-618">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="78772-619">0</span><span class="sxs-lookup"><span data-stu-id="78772-619">0</span></span>        | <span data-ttu-id="78772-620">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="78772-620">Text node</span></span> | <span data-ttu-id="78772-621">First</span><span class="sxs-lookup"><span data-stu-id="78772-621">First</span></span>  |
| <span data-ttu-id="78772-622">1</span><span class="sxs-lookup"><span data-stu-id="78772-622">1</span></span>        | <span data-ttu-id="78772-623">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="78772-623">Text node</span></span> | <span data-ttu-id="78772-624">Seconde</span><span class="sxs-lookup"><span data-stu-id="78772-624">Second</span></span> |

<span data-ttu-id="78772-625">Imaginez que `someFlag` devient `false`et que le balisage est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="78772-625">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="78772-626">Cette fois-ci, le générateur reçoit :</span><span class="sxs-lookup"><span data-stu-id="78772-626">This time, the builder receives:</span></span>

| <span data-ttu-id="78772-627">Séquence</span><span class="sxs-lookup"><span data-stu-id="78772-627">Sequence</span></span> | <span data-ttu-id="78772-628">Type</span><span class="sxs-lookup"><span data-stu-id="78772-628">Type</span></span>       | <span data-ttu-id="78772-629">Données</span><span class="sxs-lookup"><span data-stu-id="78772-629">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="78772-630">1</span><span class="sxs-lookup"><span data-stu-id="78772-630">1</span></span>        | <span data-ttu-id="78772-631">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="78772-631">Text node</span></span>  | <span data-ttu-id="78772-632">Seconde</span><span class="sxs-lookup"><span data-stu-id="78772-632">Second</span></span> |

<span data-ttu-id="78772-633">Quand le runtime effectue une comparaison, il constate que l’élément au `0` de la séquence a été supprimé, donc il génère le *script d’édition*trivial suivant :</span><span class="sxs-lookup"><span data-stu-id="78772-633">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="78772-634">Supprimez le premier nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="78772-634">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="78772-635">Qu’est-ce qui se passe si vous générez des numéros séquentiels par programmation</span><span class="sxs-lookup"><span data-stu-id="78772-635">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="78772-636">Imaginez plutôt que vous avez écrit la logique de générateur d’arborescence de rendu suivante :</span><span class="sxs-lookup"><span data-stu-id="78772-636">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="78772-637">La première sortie est désormais :</span><span class="sxs-lookup"><span data-stu-id="78772-637">Now, the first output is:</span></span>

| <span data-ttu-id="78772-638">Séquence</span><span class="sxs-lookup"><span data-stu-id="78772-638">Sequence</span></span> | <span data-ttu-id="78772-639">Type</span><span class="sxs-lookup"><span data-stu-id="78772-639">Type</span></span>      | <span data-ttu-id="78772-640">Données</span><span class="sxs-lookup"><span data-stu-id="78772-640">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="78772-641">0</span><span class="sxs-lookup"><span data-stu-id="78772-641">0</span></span>        | <span data-ttu-id="78772-642">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="78772-642">Text node</span></span> | <span data-ttu-id="78772-643">First</span><span class="sxs-lookup"><span data-stu-id="78772-643">First</span></span>  |
| <span data-ttu-id="78772-644">1</span><span class="sxs-lookup"><span data-stu-id="78772-644">1</span></span>        | <span data-ttu-id="78772-645">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="78772-645">Text node</span></span> | <span data-ttu-id="78772-646">Seconde</span><span class="sxs-lookup"><span data-stu-id="78772-646">Second</span></span> |

<span data-ttu-id="78772-647">Ce résultat est identique au cas précédent, donc aucun problème négatif n’existe.</span><span class="sxs-lookup"><span data-stu-id="78772-647">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="78772-648">`someFlag` est `false` sur le deuxième rendu et la sortie est la suivante :</span><span class="sxs-lookup"><span data-stu-id="78772-648">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="78772-649">Séquence</span><span class="sxs-lookup"><span data-stu-id="78772-649">Sequence</span></span> | <span data-ttu-id="78772-650">Type</span><span class="sxs-lookup"><span data-stu-id="78772-650">Type</span></span>      | <span data-ttu-id="78772-651">Données</span><span class="sxs-lookup"><span data-stu-id="78772-651">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="78772-652">0</span><span class="sxs-lookup"><span data-stu-id="78772-652">0</span></span>        | <span data-ttu-id="78772-653">Nœud de texte</span><span class="sxs-lookup"><span data-stu-id="78772-653">Text node</span></span> | <span data-ttu-id="78772-654">Seconde</span><span class="sxs-lookup"><span data-stu-id="78772-654">Second</span></span> |

<span data-ttu-id="78772-655">Cette fois-ci, l’algorithme diff constate que *deux* modifications ont été apportées, et l’algorithme génère le script Edit suivant :</span><span class="sxs-lookup"><span data-stu-id="78772-655">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="78772-656">Remplacez la valeur du premier nœud de texte par `Second`.</span><span class="sxs-lookup"><span data-stu-id="78772-656">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="78772-657">Supprimez le deuxième nœud de texte.</span><span class="sxs-lookup"><span data-stu-id="78772-657">Remove the second text node.</span></span>

<span data-ttu-id="78772-658">La génération des numéros de séquence a perdu toutes les informations utiles sur l’emplacement où les `if/else` branches et les boucles étaient présentes dans le code d’origine.</span><span class="sxs-lookup"><span data-stu-id="78772-658">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="78772-659">Il en résulte une comparaison de **deux fois plus longtemps** qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="78772-659">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="78772-660">Il s’agit d’un exemple trivial.</span><span class="sxs-lookup"><span data-stu-id="78772-660">This is a trivial example.</span></span> <span data-ttu-id="78772-661">Dans des cas plus réalistes avec des structures complexes et profondément imbriquées, et surtout avec des boucles, le coût des performances est plus grave.</span><span class="sxs-lookup"><span data-stu-id="78772-661">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="78772-662">Au lieu d’identifier immédiatement les blocs de boucle ou les branches qui ont été insérées ou supprimées, l’algorithme diff doit recurse profondément dans les arborescences de rendu et générer des scripts de modification beaucoup plus longs, car il est mal informé de la façon dont les anciennes et les nouvelles structures sont liées les unes aux autres.</span><span class="sxs-lookup"><span data-stu-id="78772-662">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="78772-663">Conseils et conclusions</span><span class="sxs-lookup"><span data-stu-id="78772-663">Guidance and conclusions</span></span>

* <span data-ttu-id="78772-664">Les performances de l’application sont affectées si les numéros séquentiels sont générés dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="78772-664">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="78772-665">L’infrastructure ne peut pas créer ses propres numéros de séquence automatiquement au moment de l’exécution, car les informations nécessaires n’existent pas, sauf si elles sont capturées au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="78772-665">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="78772-666">N’écrivez pas de longs blocs de `RenderTreeBuilder` logique implémentée manuellement.</span><span class="sxs-lookup"><span data-stu-id="78772-666">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="78772-667">Préférez `.razor` fichiers et permettre au compilateur de gérer les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="78772-667">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="78772-668">Si vous ne parvenez pas à éviter une logique de `RenderTreeBuilder` manuelle, fractionnez les blocs de code longs en éléments plus petits encapsulés dans `OpenRegion`/`CloseRegion` appels.</span><span class="sxs-lookup"><span data-stu-id="78772-668">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="78772-669">Chaque région a son propre espace distinct pour les numéros de séquence, ce qui vous permet de redémarrer à partir de zéro (ou tout autre nombre arbitraire) à l’intérieur de chaque région.</span><span class="sxs-lookup"><span data-stu-id="78772-669">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="78772-670">Si les numéros de séquence sont codés en dur, l’algorithme diff exige uniquement que les numéros de séquence augmentent dans la valeur.</span><span class="sxs-lookup"><span data-stu-id="78772-670">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="78772-671">La valeur initiale et les écarts ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="78772-671">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="78772-672">Une option légitime consiste à utiliser le numéro de ligne de code comme numéro de séquence, ou à commencer à partir de zéro et à augmenter par des ou des centaines (ou un intervalle de préférence).</span><span class="sxs-lookup"><span data-stu-id="78772-672">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="78772-673"> utilise des numéros de séquence, tandis que d’autres infrastructures d’interface utilisateur de comparaison d’arborescence ne les utilisent pas.</span><span class="sxs-lookup"><span data-stu-id="78772-673"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="78772-674">La comparaison est beaucoup plus rapide lorsque les numéros de séquence sont utilisés et Blazor présente l’avantage d’une étape de compilation qui traite automatiquement les numéros séquentiels pour les développeurs qui créent des fichiers *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="78772-674">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="78772-675">Localisation</span><span class="sxs-lookup"><span data-stu-id="78772-675">Localization</span></span>

<span data-ttu-id="78772-676">les applications Blazor Server sont localisées à l’aide de l' [intergiciel (middleware](xref:fundamentals/localization#localization-middleware)) de localisation.</span><span class="sxs-lookup"><span data-stu-id="78772-676">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="78772-677">L’intergiciel sélectionne la culture appropriée pour les utilisateurs qui demandent des ressources à partir de l’application.</span><span class="sxs-lookup"><span data-stu-id="78772-677">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="78772-678">La culture peut être définie à l’aide de l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="78772-678">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="78772-679">Cookies</span><span class="sxs-lookup"><span data-stu-id="78772-679">Cookies</span></span>](#cookies)
* [<span data-ttu-id="78772-680">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="78772-680">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="78772-681">Pour plus d’informations et d’exemples, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="78772-681">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="78772-682">Configurer l’éditeur de liens pour l’internationalisation (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="78772-682">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="78772-683">Par défaut, la configuration de l’éditeur de liens de Blazorpour les applications webassembly Blazor supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement.</span><span class="sxs-lookup"><span data-stu-id="78772-683">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="78772-684">Pour plus d’informations et de conseils sur le contrôle du comportement de l’éditeur de liens, consultez <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="78772-684">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="78772-685">Cookies</span><span class="sxs-lookup"><span data-stu-id="78772-685">Cookies</span></span>

<span data-ttu-id="78772-686">Un cookie de culture de localisation peut conserver la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-686">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="78772-687">Le cookie est créé par la méthode `OnGet` de la page hôte de l’application (*pages/Host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="78772-687">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="78772-688">L’intergiciel (middleware) de localisation lit le cookie sur les demandes suivantes pour définir la culture de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-688">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="78772-689">L’utilisation d’un cookie garantit que la connexion WebSocket peut propager correctement la culture.</span><span class="sxs-lookup"><span data-stu-id="78772-689">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="78772-690">Si les schémas de localisation sont basés sur le chemin d’URL ou la chaîne de requête, il est possible que le schéma ne soit pas en mesure de fonctionner avec les WebSockets, et donc de ne pas conserver la culture.</span><span class="sxs-lookup"><span data-stu-id="78772-690">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="78772-691">Par conséquent, l’utilisation d’un cookie de culture de localisation est l’approche recommandée.</span><span class="sxs-lookup"><span data-stu-id="78772-691">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="78772-692">Toute technique peut être utilisée pour assigner une culture si la culture est rendue persistante dans un cookie de localisation.</span><span class="sxs-lookup"><span data-stu-id="78772-692">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="78772-693">Si l’application a déjà un schéma de localisation établi pour les ASP.NET Core côté serveur, continuez à utiliser l’infrastructure de localisation existante de l’application et à définir le cookie de la culture de localisation dans le schéma de l’application.</span><span class="sxs-lookup"><span data-stu-id="78772-693">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="78772-694">L’exemple suivant montre comment définir la culture actuelle dans un cookie qui peut être lu par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="78772-694">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="78772-695">Créez un fichier *pages/Host. cshtml. cs* avec le contenu suivant dans l’application Blazor Server :</span><span class="sxs-lookup"><span data-stu-id="78772-695">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="78772-696">La localisation est gérée dans l’application :</span><span class="sxs-lookup"><span data-stu-id="78772-696">Localization is handled in the app:</span></span>

1. <span data-ttu-id="78772-697">Le navigateur envoie une requête HTTP initiale à l’application.</span><span class="sxs-lookup"><span data-stu-id="78772-697">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="78772-698">La culture est affectée par l’intergiciel (middleware) de localisation.</span><span class="sxs-lookup"><span data-stu-id="78772-698">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="78772-699">La méthode `OnGet` dans *_Host. cshtml. cs* rend persistante la culture dans un cookie dans le cadre de la réponse.</span><span class="sxs-lookup"><span data-stu-id="78772-699">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="78772-700">Le navigateur ouvre une connexion WebSocket pour créer une session Blazor Server interactive.</span><span class="sxs-lookup"><span data-stu-id="78772-700">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="78772-701">L’intergiciel de localisation lit le cookie et assigne la culture.</span><span class="sxs-lookup"><span data-stu-id="78772-701">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="78772-702">La session du serveur de Blazor commence par la culture correcte.</span><span class="sxs-lookup"><span data-stu-id="78772-702">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="78772-703">Fournir l’interface utilisateur pour choisir la culture</span><span class="sxs-lookup"><span data-stu-id="78772-703">Provide UI to choose the culture</span></span>

<span data-ttu-id="78772-704">Pour fournir une interface utilisateur permettant à un utilisateur de sélectionner une culture, il est recommandé d’effectuer une *approche basée sur la redirection* .</span><span class="sxs-lookup"><span data-stu-id="78772-704">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="78772-705">Le processus est similaire à ce qui se produit dans une application Web lorsqu’un utilisateur tente d’accéder à une ressource sécurisée&mdash;l’utilisateur est redirigé vers une page de connexion, puis redirigé vers la ressource d’origine.</span><span class="sxs-lookup"><span data-stu-id="78772-705">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="78772-706">L’application conserve la culture sélectionnée de l’utilisateur via une redirection vers un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="78772-706">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="78772-707">Le contrôleur définit la culture sélectionnée de l’utilisateur dans un cookie et redirige l’utilisateur vers l’URI d’origine.</span><span class="sxs-lookup"><span data-stu-id="78772-707">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="78772-708">Établissez un point de terminaison HTTP sur le serveur pour définir la culture sélectionnée de l’utilisateur dans un cookie et effectuer la redirection vers l’URI d’origine :</span><span class="sxs-lookup"><span data-stu-id="78772-708">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="78772-709">Utilisez le résultat de l’action `LocalRedirect` pour empêcher les attaques de redirection ouvertes.</span><span class="sxs-lookup"><span data-stu-id="78772-709">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="78772-710">Pour plus d'informations, consultez <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="78772-710">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="78772-711">Le composant suivant montre un exemple d’exécution de la redirection initiale lorsque l’utilisateur sélectionne une culture :</span><span class="sxs-lookup"><span data-stu-id="78772-711">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="78772-712">Utiliser les scénarios de localisation .NET dans les applications Blazor</span><span class="sxs-lookup"><span data-stu-id="78772-712">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="78772-713">Dans Blazor Apps, les scénarios de localisation et de globalisation .NET suivants sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="78772-713">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="78772-714">. Système de ressources du réseau</span><span class="sxs-lookup"><span data-stu-id="78772-714">.NET's resources system</span></span>
* <span data-ttu-id="78772-715">Mise en forme des nombres et des dates spécifiques à la culture</span><span class="sxs-lookup"><span data-stu-id="78772-715">Culture-specific number and date formatting</span></span>

<span data-ttu-id="78772-716">la fonctionnalité de `@bind` de Blazoreffectue une globalisation basée sur la culture actuelle de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="78772-716">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="78772-717">Pour plus d’informations, consultez la section [liaison de données](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="78772-717">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="78772-718">Un ensemble limité de scénarios de localisation de ASP.NET Core est actuellement pris en charge :</span><span class="sxs-lookup"><span data-stu-id="78772-718">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="78772-719">`IStringLocalizer<>` *est pris en charge* dans les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-719">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="78772-720">la localisation des annotations de données `IHtmlLocalizer<>`, `IViewLocalizer<>`et est ASP.NET Core les scénarios MVC et **non pris en charge** dans les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="78772-720">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="78772-721">Pour plus d'informations, consultez <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="78772-721">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="78772-722">Images SVG (Scalable Vector Graphics)</span><span class="sxs-lookup"><span data-stu-id="78772-722">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="78772-723">Étant donné que Blazor restitue le code HTML, les images prises en charge par le navigateur, y compris les images SVG (Scalable Vector Graphics) ( *. svg*), sont prises en charge via la balise `<img>` :</span><span class="sxs-lookup"><span data-stu-id="78772-723">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="78772-724">De même, les images SVG sont prises en charge dans les règles CSS d’un fichier de feuille de style ( *. CSS*) :</span><span class="sxs-lookup"><span data-stu-id="78772-724">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="78772-725">Toutefois, le balisage SVG en ligne n’est pas pris en charge dans tous les scénarios.</span><span class="sxs-lookup"><span data-stu-id="78772-725">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="78772-726">Si vous placez une balise de `<svg>` directement dans un fichier de composant ( *. Razor*), le rendu d’image de base est pris en charge, mais de nombreux scénarios avancés ne sont pas encore pris en charge.</span><span class="sxs-lookup"><span data-stu-id="78772-726">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="78772-727">Par exemple, les balises `<use>` ne sont pas respectées et `@bind` ne peuvent pas être utilisées avec certaines balises SVG.</span><span class="sxs-lookup"><span data-stu-id="78772-727">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="78772-728">Nous prévoyons de traiter ces limitations dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="78772-728">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78772-729">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="78772-729">Additional resources</span></span>

* <span data-ttu-id="78772-730"><xref:security/blazor/server> &ndash; contient des conseils sur la création d’applications serveur Blazor qui doivent rivaliser avec l’épuisement des ressources.</span><span class="sxs-lookup"><span data-stu-id="78772-730"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
