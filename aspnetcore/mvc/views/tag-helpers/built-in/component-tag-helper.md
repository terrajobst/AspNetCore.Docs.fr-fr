---
title: Tag Helper Component dans ASP.NET Core
author: guardrex
ms.author: riande
description: Découvrez comment utiliser le tag Helper du composant ASP.NET Core pour afficher les composants Razor dans les pages et les vues.
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 801ceb73de5bb4ef7500624e1fbddbf96d1ab89c
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226395"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="fa152-103">Tag Helper Component dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa152-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="fa152-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fa152-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fa152-105">Pour afficher un composant à partir d’une page ou d’une vue, utilisez le [tag Helper Component](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span><span class="sxs-lookup"><span data-stu-id="fa152-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

<span data-ttu-id="fa152-106">Le tag Helper Component suivant restitue le composant `Counter` dans une page ou une vue :</span><span class="sxs-lookup"><span data-stu-id="fa152-106">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="fa152-107">Le tag Helper Component peut également transmettre des paramètres à des composants.</span><span class="sxs-lookup"><span data-stu-id="fa152-107">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="fa152-108">Prenons le composant `ColorfulCheckbox` suivant qui définit la couleur et la taille de l’étiquette de case à cocher :</span><span class="sxs-lookup"><span data-stu-id="fa152-108">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

<span data-ttu-id="fa152-109">Les [paramètres de composant](xref:blazor/components#component-parameters) `Size` (`int`) et `Color` (`string`) peuvent être définis par le tag Helper du composant :</span><span class="sxs-lookup"><span data-stu-id="fa152-109">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="fa152-110">Le code HTML suivant est affiché dans la page ou la vue :</span><span class="sxs-lookup"><span data-stu-id="fa152-110">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="fa152-111">Le passage d’une chaîne entre guillemets requiert une [expression Razor explicite](xref:mvc/views/razor#explicit-razor-expressions), comme indiqué pour `param-Color` dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="fa152-111">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="fa152-112">Le comportement d’analyse Razor pour une valeur de type `string` ne s’applique pas à un attribut `param-*`, car l’attribut est un type `object`.</span><span class="sxs-lookup"><span data-stu-id="fa152-112">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="fa152-113">Le type de paramètre doit être sérialisable JSON, ce qui signifie généralement que le type doit avoir un constructeur par défaut et des propriétés définissables.</span><span class="sxs-lookup"><span data-stu-id="fa152-113">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="fa152-114">Par exemple, vous pouvez spécifier une valeur pour `Size` et `Color` dans l’exemple précédent, car les types de `Size` et `Color` sont des types primitifs (`int` et `string`), qui sont pris en charge par le sérialiseur JSON.</span><span class="sxs-lookup"><span data-stu-id="fa152-114">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="fa152-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configure si le composant :</span><span class="sxs-lookup"><span data-stu-id="fa152-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="fa152-116">Est prérendu dans la page.</span><span class="sxs-lookup"><span data-stu-id="fa152-116">Is prerendered into the page.</span></span>
* <span data-ttu-id="fa152-117">Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fa152-117">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="fa152-118">Mode de rendu</span><span class="sxs-lookup"><span data-stu-id="fa152-118">Render Mode</span></span> | <span data-ttu-id="fa152-119">Description</span><span class="sxs-lookup"><span data-stu-id="fa152-119">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="fa152-120">Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="fa152-120">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="fa152-121">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="fa152-121">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="fa152-122">Restitue un marqueur pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="fa152-122">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="fa152-123">La sortie du composant n’est pas incluse.</span><span class="sxs-lookup"><span data-stu-id="fa152-123">Output from the component isn't included.</span></span> <span data-ttu-id="fa152-124">Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor.</span><span class="sxs-lookup"><span data-stu-id="fa152-124">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="fa152-125">Génère le rendu du composant en HTML statique.</span><span class="sxs-lookup"><span data-stu-id="fa152-125">Renders the component into static HTML.</span></span> |

<span data-ttu-id="fa152-126">Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie.</span><span class="sxs-lookup"><span data-stu-id="fa152-126">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="fa152-127">Les composants ne peuvent pas utiliser les fonctionnalités spécifiques aux vues et aux pages, telles que les vues partielles et les sections.</span><span class="sxs-lookup"><span data-stu-id="fa152-127">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="fa152-128">Pour utiliser la logique d’une vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="fa152-128">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="fa152-129">Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="fa152-129">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fa152-130">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fa152-130">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
