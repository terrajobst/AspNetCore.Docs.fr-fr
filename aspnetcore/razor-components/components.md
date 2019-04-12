---
title: Créer et utiliser des composants de Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants de Razor, y compris comment lier à des données, gérer les événements et gérer les cycles de vie de composant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515564"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="74d36-103">Créer et utiliser des composants de Razor</span><span class="sxs-lookup"><span data-stu-id="74d36-103">Create and use Razor Components</span></span>

<span data-ttu-id="74d36-104">Par [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), et [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="74d36-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="74d36-105">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="74d36-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="74d36-106">Consultez la rubrique [Bien démarrer](xref:razor-components/get-started) pour connaître les prérequis.</span><span class="sxs-lookup"><span data-stu-id="74d36-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="74d36-107">Les applications de composants de Razor sont créées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="74d36-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="74d36-108">Un composant est un bloc autonome de l’interface utilisateur (IU), par exemple une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="74d36-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="74d36-109">Un composant inclut un balisage HTML et la logique de traitement nécessaire pour injecter des données ou de répondre aux événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="74d36-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="74d36-110">Les composants sont flexibles et léger.</span><span class="sxs-lookup"><span data-stu-id="74d36-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="74d36-111">Ils peuvent être imbriqués, réutilisés et partagés entre des projets.</span><span class="sxs-lookup"><span data-stu-id="74d36-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="74d36-112">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="74d36-112">Component classes</span></span>

<span data-ttu-id="74d36-113">Les composants sont généralement implémentées dans les fichiers de composant de Razor (*.razor*) à l’aide d’une combinaison de C# et le balisage HTML (*.cshtml* fichiers sont utilisés dans les applications Blazor).</span><span class="sxs-lookup"><span data-stu-id="74d36-113">Components are typically implemented in Razor Component files (*.razor*) using a combination of C# and HTML markup (*.cshtml* files are used in Blazor apps).</span></span>

<span data-ttu-id="74d36-114">Les composants peuvent être créés dans les applications de composants de Razor à l’aide de la *.cshtml* extension de fichier tant que les fichiers sont identifiés en tant que fichiers de composant de Razor à l’aide de la `_RazorComponentInclude` propriété MSBuild.</span><span class="sxs-lookup"><span data-stu-id="74d36-114">Components can be authored in Razor Components apps using the *.cshtml* file extension as long as the files are identified as Razor Component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="74d36-115">Par exemple, une application créée à l’aide du modèle de composant de Razor Spécifie que tous les *.cshtml* fichiers sous le *composants* dossier doit être traité en tant que fichiers de composants de Razor :</span><span class="sxs-lookup"><span data-stu-id="74d36-115">For example, an app created using the Razor Component template specifies that all *.cshtml* files under the *Components* folder should be treated as Razor Components files:</span></span>

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

<span data-ttu-id="74d36-116">L’interface utilisateur pour un composant est défini à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="74d36-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="74d36-117">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="74d36-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="74d36-118">Lors de la compilation d’une application de composants de Razor, le balisage HTML et C# logique de rendu sont converties en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-118">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="74d36-119">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="74d36-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="74d36-120">Membres de la classe de composant sont définies dans un `@functions` bloc (plusieurs `@functions` bloc est autorisé).</span><span class="sxs-lookup"><span data-stu-id="74d36-120">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="74d36-121">Dans le `@functions` bloc, l’état du composant (propriétés, champs) est spécifié avec des méthodes pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-121">In the `@functions` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="74d36-122">Membres composant peuvent ensuite être utilisées comme partie du composant de rendu à l’aide de la logique C# expressions qui commencent par `@`.</span><span class="sxs-lookup"><span data-stu-id="74d36-122">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="74d36-123">Par exemple, un C# champ est restitué en préfixant `@` au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="74d36-123">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="74d36-124">L’exemple suivant évalue et effectue le rendu :</span><span class="sxs-lookup"><span data-stu-id="74d36-124">The following example evaluates and renders:</span></span>

* `_headingFontStyle` <span data-ttu-id="74d36-125">la valeur de propriété CSS pour `font-style`.</span><span class="sxs-lookup"><span data-stu-id="74d36-125">to the CSS property value for `font-style`.</span></span>
* `_headingText` <span data-ttu-id="74d36-126">pour le contenu de la `<h1>` élément.</span><span class="sxs-lookup"><span data-stu-id="74d36-126">to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="74d36-127">Une fois que le composant est initialement affichée, le composant régénère son arborescence rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="74d36-127">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="74d36-128">Composants de Razor compare la nouvelle arborescence de rendu par rapport à le, puis applique toutes les modifications pour du navigateur modèle DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="74d36-128">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="74d36-129">Intégrer des composants des applications MVC et les Pages Razor</span><span class="sxs-lookup"><span data-stu-id="74d36-129">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="74d36-130">Utiliser des composants avec des applications existantes sur les Pages Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="74d36-130">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="74d36-131">Il est inutile de réécrire les pages existantes ou des vues à utiliser les composants de Razor.</span><span class="sxs-lookup"><span data-stu-id="74d36-131">There's no need to rewrite existing pages or views to use Razor Components.</span></span> <span data-ttu-id="74d36-132">Lorsque la page ou la vue est restituée, les composants sont préaffichés&dagger; en même temps.</span><span class="sxs-lookup"><span data-stu-id="74d36-132">When the page or view is rendered, components are prerendered&dagger; at the same time.</span></span> 

> [!NOTE]
> <span data-ttu-id="74d36-133">&dagger;Pré-rendu du côté serveur est activé pour les applications de Razor composants par défaut.</span><span class="sxs-lookup"><span data-stu-id="74d36-133">&dagger;Server-side prerendering is enabled for Razor Components apps by default.</span></span> <span data-ttu-id="74d36-134">Les applications côté client Blazor prendra en charge pré-rendu dans la prochaine version Preview 4.</span><span class="sxs-lookup"><span data-stu-id="74d36-134">Client-side Blazor apps will support prerendering in the upcoming Preview 4 release.</span></span> <span data-ttu-id="74d36-135">Pour plus d’informations, consultez [mettre à jour des modèles/intergiciel (middleware) à utiliser le fichier/MapFallbackToPage](https://github.com/aspnet/AspNetCore/issues/8852).</span><span class="sxs-lookup"><span data-stu-id="74d36-135">For more information, see [Update templates/middleware to use MapFallbackToPage/File](https://github.com/aspnet/AspNetCore/issues/8852).</span></span>

<span data-ttu-id="74d36-136">Pour restituer un composant à partir d’une page ou une vue, utilisez le `RenderComponentAsync<TComponent>` méthode d’assistance HTML :</span><span class="sxs-lookup"><span data-stu-id="74d36-136">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="74d36-137">Rendu des pages et des vues de composants ne sont pas encore interactives dans la version Preview 3.</span><span class="sxs-lookup"><span data-stu-id="74d36-137">Components rendered from pages and views aren't yet interactive in the Preview 3 release.</span></span> <span data-ttu-id="74d36-138">Par exemple, la sélection d’un bouton ne déclenche un appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="74d36-138">For example, selecting a button doesn't trigger a method call.</span></span> <span data-ttu-id="74d36-139">Une prochaine version d’évaluation traitera cette limitation et ajouter la prise en charge pour les composants de rendu à l’aide de la syntaxe d’élément et d’attribut normale.</span><span class="sxs-lookup"><span data-stu-id="74d36-139">A future preview will address this limitation and add support for rendering components using the normal element and attribute syntax.</span></span>

<span data-ttu-id="74d36-140">Tandis que les pages et vues peuvent utiliser des composants, l’inverse n’est pas vrai.</span><span class="sxs-lookup"><span data-stu-id="74d36-140">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="74d36-141">Composants ne pouvez pas utiliser de vue - et -spécifiques à la page scénarios, tels que les sections et les vues partielles.</span><span class="sxs-lookup"><span data-stu-id="74d36-141">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="74d36-142">Pour utiliser une logique à partir de la vue partielle dans un composant, du facteur de la logique de la vue partielle dans un composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-142">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

## <a name="using-components"></a><span data-ttu-id="74d36-143">À l’aide de composants</span><span class="sxs-lookup"><span data-stu-id="74d36-143">Using components</span></span>

<span data-ttu-id="74d36-144">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="74d36-144">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="74d36-145">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-145">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="74d36-146">Restitue le balisage suivant un `HeadingComponent` (*HeadingComponent.cshtml*) instance :</span><span class="sxs-lookup"><span data-stu-id="74d36-146">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="74d36-147">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="74d36-147">Component parameters</span></span>

<span data-ttu-id="74d36-148">Composants peuvent avoir *composant paramètres*, qui sont définis à l’aide de *non public* assorties de propriétés de la classe de composant `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="74d36-148">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="74d36-149">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="74d36-149">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="74d36-150">Dans l’exemple suivant, le `ParentComponent` définit la valeur de la `Title` propriété de la `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="74d36-150">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="74d36-151">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-151">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

<span data-ttu-id="74d36-152">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-152">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="74d36-153">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="74d36-153">Child content</span></span>

<span data-ttu-id="74d36-154">Composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-154">Components can set the content of another component.</span></span> <span data-ttu-id="74d36-155">Le composant affectation fournit le contenu entre les balises qui spécifient le composant de réception.</span><span class="sxs-lookup"><span data-stu-id="74d36-155">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="74d36-156">Par exemple, un `ParentComponent` peut fournir des contenus pour le rendu par un composant enfant en plaçant le contenu à l’intérieur `<ChildComponent>` balises.</span><span class="sxs-lookup"><span data-stu-id="74d36-156">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="74d36-157">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-157">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

<span data-ttu-id="74d36-158">Le composant enfant a un `ChildContent` propriété qui représente un `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="74d36-158">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="74d36-159">La valeur de `ChildContent` est placé dans le balisage du composant enfant où le contenu doit être restitué.</span><span class="sxs-lookup"><span data-stu-id="74d36-159">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="74d36-160">Dans l’exemple suivant, la valeur de `ChildContent` est reçu à partir du composant parent et restituée dans le panneau de configuration d’amorçage `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="74d36-160">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="74d36-161">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-161">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="74d36-162">La réception de la propriété le `RenderFragment` contenu doit être nommé `ChildContent` par convention.</span><span class="sxs-lookup"><span data-stu-id="74d36-162">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="74d36-163">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="74d36-163">Data binding</span></span>

<span data-ttu-id="74d36-164">Liaison de données à la fois des composants et des éléments DOM est effectuée avec la `bind` attribut.</span><span class="sxs-lookup"><span data-stu-id="74d36-164">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="74d36-165">L’exemple suivant lie la `ItalicsCheck` état activé de propriété à la case à cocher :</span><span class="sxs-lookup"><span data-stu-id="74d36-165">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

<span data-ttu-id="74d36-166">Lorsque la case à cocher est activée et désactivée, la valeur de propriété est mis à jour vers `true` et `false`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="74d36-166">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="74d36-167">La case à cocher est mis à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, pas en réponse à l’évolution de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="74d36-167">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="74d36-168">Dans la mesure où les composants s’afficher après l’exécution de code de gestionnaire d’événements, mises à jour de la propriété sont généralement pas immédiatement dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="74d36-168">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="74d36-169">À l’aide de `bind` avec un `CurrentValue` propriété (`<input bind="@CurrentValue">`) est fondamentalement équivalent à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="74d36-169">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue">`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

<span data-ttu-id="74d36-170">Lorsque le composant est affiché, le `value` de l’élément d’entrée provient de la `CurrentValue` propriété.</span><span class="sxs-lookup"><span data-stu-id="74d36-170">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="74d36-171">Lorsque l’utilisateur tape dans la zone de texte, le `onchange` événement est déclenché et le `CurrentValue` propriété est définie sur la valeur modifiée.</span><span class="sxs-lookup"><span data-stu-id="74d36-171">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="74d36-172">En réalité, la génération de code est un peu plus complexe, car `bind` gère des quelques cas où les conversions de type sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="74d36-172">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="74d36-173">En principe, `bind` associe la valeur actuelle d’une expression avec un `value` attribut et gère des modifications à l’aide du gestionnaire enregistré.</span><span class="sxs-lookup"><span data-stu-id="74d36-173">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="74d36-174">En plus de `onchange`, la propriété peut être liée à l’aide d’autres événements tels que `oninput` en étant plus explicite sur les éléments à lier à :</span><span class="sxs-lookup"><span data-stu-id="74d36-174">In addition to `onchange`, the property can be bound using other events like `oninput` by being more explicit about what to bind to:</span></span>

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

<span data-ttu-id="74d36-175">Contrairement aux `onchange`, `oninput` se déclenche pour chaque caractère est entrée dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="74d36-175">Unlike `onchange`, `oninput` fires for every character that is input into the text box.</span></span>

**<span data-ttu-id="74d36-176">Chaînes de format</span><span class="sxs-lookup"><span data-stu-id="74d36-176">Format strings</span></span>**

<span data-ttu-id="74d36-177">Liaison de données fonctionne avec <xref:System.DateTime> chaînes de format.</span><span class="sxs-lookup"><span data-stu-id="74d36-177">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="74d36-178">Autres expressions de format, telles que les devises ou les formats de nombre, ne sont pas disponibles pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="74d36-178">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="74d36-179">Le `format-value` attribut spécifie le format de date à appliquer à la `value` de la `input` élément.</span><span class="sxs-lookup"><span data-stu-id="74d36-179">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="74d36-180">Le format est également utilisé pour analyser la valeur quand un `onchange` événement se produit.</span><span class="sxs-lookup"><span data-stu-id="74d36-180">The format is also used to parse the value when an `onchange` event occurs.</span></span>

**<span data-ttu-id="74d36-181">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="74d36-181">Component parameters</span></span>**

<span data-ttu-id="74d36-182">Liaison reconnaît également les paramètres du composant, où `bind-{property}` peut lier une valeur de propriété entre plusieurs composants.</span><span class="sxs-lookup"><span data-stu-id="74d36-182">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="74d36-183">Utilise le composant suivant `ChildComponent` et lie le `ParentYear` paramètre du parent de le `Year` paramètre sur le composant enfant :</span><span class="sxs-lookup"><span data-stu-id="74d36-183">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="74d36-184">Composant parent :</span><span class="sxs-lookup"><span data-stu-id="74d36-184">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="74d36-185">Composant enfant :</span><span class="sxs-lookup"><span data-stu-id="74d36-185">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

<span data-ttu-id="74d36-186">Chargement de la `ParentComponent` génère le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="74d36-186">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="74d36-187">Si la valeur de la `ParentYear` propriété est modifiée en sélectionnant le bouton dans le `ParentComponent`, le `Year` propriété de la `ChildComponent` est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="74d36-187">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="74d36-188">La nouvelle valeur de `Year` est affiché dans l’interface utilisateur lorsque le `ParentComponent` est un nouvel affichage :</span><span class="sxs-lookup"><span data-stu-id="74d36-188">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="74d36-189">Le `Year` paramètre est peut être liée, car il possède un accompagnement `YearChanged` événement qui correspond au type de le `Year` paramètre.</span><span class="sxs-lookup"><span data-stu-id="74d36-189">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="74d36-190">Par convention, `<ChildComponent bind-Year="@ParentYear" />` est fondamentalement équivalent à l’écriture,</span><span class="sxs-lookup"><span data-stu-id="74d36-190">By convention, `<ChildComponent bind-Year="@ParentYear" />` is essentially equivalent to writing,</span></span>

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

<span data-ttu-id="74d36-191">En règle générale, une propriété peut être liée à un gestionnaire événements correspondant à l’aide `bind-property-event` attribut.</span><span class="sxs-lookup"><span data-stu-id="74d36-191">In general, a property can be bound to a corresponding event handler using `bind-property-event` attribute.</span></span>

## <a name="event-handling"></a><span data-ttu-id="74d36-192">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="74d36-192">Event handling</span></span>

<span data-ttu-id="74d36-193">Composants de Razor fournissent des fonctionnalités de gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="74d36-193">Razor Components provide event handling features.</span></span> <span data-ttu-id="74d36-194">Pour un attribut d’élément HTML nommé `on<event>` (par exemple, `onclick`, `onsubmit`) avec une valeur typée de délégué, les composants de Razor traite la valeur de l’attribut comme gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="74d36-194">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="74d36-195">Le nom d’attribut commence toujours par `on`.</span><span class="sxs-lookup"><span data-stu-id="74d36-195">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="74d36-196">Le code suivant appelle la `UpdateHeading` méthode lorsque le bouton est sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="74d36-196">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="74d36-197">Le code suivant appelle la `CheckboxChanged` méthode lorsque la case à cocher est modifié dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="74d36-197">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="74d36-198">Gestionnaires d’événements peuvent également être asynchrone et retour un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="74d36-198">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="74d36-199">Il est inutile d’appeler manuellement `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="74d36-199">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="74d36-200">Exceptions sont enregistrées lorsqu’ils se produisent.</span><span class="sxs-lookup"><span data-stu-id="74d36-200">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="74d36-201">Pour certains événements, types d’arguments spécifiques à l’événement événement sont autorisées.</span><span class="sxs-lookup"><span data-stu-id="74d36-201">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="74d36-202">Si l’accès à l’un de ces types d’événements n’est pas nécessaire, elle n’est pas requise dans l’appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="74d36-202">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="74d36-203">La liste d’arguments d’événement pris en charge est :</span><span class="sxs-lookup"><span data-stu-id="74d36-203">The list of supported event arguments is:</span></span>

* <span data-ttu-id="74d36-204">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="74d36-204">UIEventArgs</span></span>
* <span data-ttu-id="74d36-205">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="74d36-205">UIChangeEventArgs</span></span>
* <span data-ttu-id="74d36-206">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="74d36-206">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="74d36-207">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="74d36-207">UIMouseEventArgs</span></span>

<span data-ttu-id="74d36-208">Expressions lambda peuvent également être utilisées :</span><span class="sxs-lookup"><span data-stu-id="74d36-208">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="74d36-209">Il est souvent utile de fermer sur des valeurs supplémentaires, comme lors de l’itération sur un jeu d’éléments.</span><span class="sxs-lookup"><span data-stu-id="74d36-209">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="74d36-210">L’exemple suivant crée trois boutons, chacune appelant `UpdateHeading` en passant un argument d’événement (`UIMouseEventArgs`) et son numéro de bouton (`buttonNumber`) lorsque sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="74d36-210">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="74d36-211">Faire **pas** utiliser la variable de boucle (`i`) dans un `for` boucle directement dans une expression lambda.</span><span class="sxs-lookup"><span data-stu-id="74d36-211">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="74d36-212">Sinon, la même variable est utilisée par toutes les expressions lambda à l’origine `i`de valeur pour être identiques dans toutes les expressions lambda.</span><span class="sxs-lookup"><span data-stu-id="74d36-212">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="74d36-213">Capture pas toujours sa valeur dans une variable locale (`buttonNumber` dans l’exemple précédent), puis l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="74d36-213">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="74d36-214">Capturer les références aux composants</span><span class="sxs-lookup"><span data-stu-id="74d36-214">Capture references to components</span></span>

<span data-ttu-id="74d36-215">Références de composants fournissent un moyen de get une référence à une instance du composant afin que vous pouvez émettre des commandes à cette instance, tels que `Show` ou `Reset`.</span><span class="sxs-lookup"><span data-stu-id="74d36-215">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="74d36-216">Pour capturer une référence de composant, ajoutez un `ref` attribut au composant enfant et puis définir un champ avec le même nom et le même type que celui du composant enfant.</span><span class="sxs-lookup"><span data-stu-id="74d36-216">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="74d36-217">Lorsque le composant est affiché, le `loginDialog` champ est renseigné avec la `MyLoginDialog` instance du composant enfant.</span><span class="sxs-lookup"><span data-stu-id="74d36-217">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="74d36-218">Vous pouvez ensuite appeler les méthodes de .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-218">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="74d36-219">Le `loginDialog` variable est renseignée uniquement une fois que le composant est restitué et sa sortie inclut le `MyLoginDialog` élément.</span><span class="sxs-lookup"><span data-stu-id="74d36-219">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="74d36-220">Jusqu'à ce point, il n’a rien à faire référence.</span><span class="sxs-lookup"><span data-stu-id="74d36-220">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="74d36-221">Pour manipuler les références de composants une fois que le composant a terminé le rendu, utilisez la `OnAfterRenderAsync` ou `OnAfterRender` méthodes.</span><span class="sxs-lookup"><span data-stu-id="74d36-221">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="74d36-222">Tandis que les références de composant de capture utilise une syntaxe similaire à [capture les références d’élément](xref:razor-components/javascript-interop#capture-references-to-elements), il n’est pas un [JavaScript interop](xref:razor-components/javascript-interop) fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="74d36-222">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="74d36-223">Références de composants ne sont pas transmis au code JavaScript ; ils sont utilisés uniquement dans le code .NET.</span><span class="sxs-lookup"><span data-stu-id="74d36-223">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="74d36-224">Faire **pas** utilisent des références de composant pour muter l’état des composants de l’enfant.</span><span class="sxs-lookup"><span data-stu-id="74d36-224">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="74d36-225">Utilisez plutôt les paramètres déclaratifs normales pour passer des données à des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="74d36-225">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="74d36-226">Cela entraîne des composants enfants de restituer automatiquement aux moments appropriés à.</span><span class="sxs-lookup"><span data-stu-id="74d36-226">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="74d36-227">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="74d36-227">Lifecycle methods</span></span>

`OnInitAsync` <span data-ttu-id="74d36-228">et `OnInit` exécuter du code pour initialiser le composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-228">and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="74d36-229">Pour effectuer une opération asynchrone, utilisez `OnInitAsync` et le `await` mot clé sur l’opération :</span><span class="sxs-lookup"><span data-stu-id="74d36-229">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="74d36-230">Pour une opération synchrone, utilisez `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="74d36-230">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` <span data-ttu-id="74d36-231">et `OnParametersSet` sont appelés quand un composant a reçu les paramètres de son parent et les valeurs sont affectées aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="74d36-231">and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="74d36-232">Ces méthodes sont exécutées après l’initialisation du composant et ensuite chaque fois que le composant est rendu :</span><span class="sxs-lookup"><span data-stu-id="74d36-232">These methods are executed after component initialization and then each time the component is rendered:</span></span>

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

`OnAfterRenderAsync` <span data-ttu-id="74d36-233">et `OnAfterRender` sont appelées après un composant a terminé le rendu.</span><span class="sxs-lookup"><span data-stu-id="74d36-233">and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="74d36-234">Références de composant et d’élément sont renseignées à ce stade.</span><span class="sxs-lookup"><span data-stu-id="74d36-234">Element and component references are populated at this point.</span></span> <span data-ttu-id="74d36-235">Cette étape permet d’effectuer les étapes d’initialisation supplémentaires à l’aide du contenu affiché, telles que l’activation des bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.</span><span class="sxs-lookup"><span data-stu-id="74d36-235">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

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

`SetParameters` <span data-ttu-id="74d36-236">peut être substituée pour exécuter du code avant de définir des paramètres :</span><span class="sxs-lookup"><span data-stu-id="74d36-236">can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="74d36-237">Si `base.SetParameters` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants dans une façon requis.</span><span class="sxs-lookup"><span data-stu-id="74d36-237">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="74d36-238">Par exemple, les paramètres entrants ne sont pas nécessaire d’affecter aux propriétés sur la classe.</span><span class="sxs-lookup"><span data-stu-id="74d36-238">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

`ShouldRender` <span data-ttu-id="74d36-239">peut être substituée pour supprimer l’actualisation de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="74d36-239">can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="74d36-240">Si l’implémentation retourne `true`, l’interface utilisateur est actualisée.</span><span class="sxs-lookup"><span data-stu-id="74d36-240">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="74d36-241">Même si `ShouldRender` est substitué, le composant est toujours affichée à l’origine.</span><span class="sxs-lookup"><span data-stu-id="74d36-241">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="74d36-242">Élimination de composant avec IDisposable</span><span class="sxs-lookup"><span data-stu-id="74d36-242">Component disposal with IDisposable</span></span>

<span data-ttu-id="74d36-243">Si un composant implémente <xref:System.IDisposable>, le [Dispose, méthode](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="74d36-243">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="74d36-244">Utilise le composant suivant `@implements IDisposable` et `Dispose` méthode :</span><span class="sxs-lookup"><span data-stu-id="74d36-244">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="74d36-245">Routage</span><span class="sxs-lookup"><span data-stu-id="74d36-245">Routing</span></span>

<span data-ttu-id="74d36-246">Routage dans les composants de Razor est obtenu en fournissant un modèle d’itinéraire pour chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="74d36-246">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="74d36-247">Quand un *.cshtml* de fichiers avec un `@page` directive est compilée, la classe générée est donnée un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="74d36-247">When a *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="74d36-248">Lors de l’exécution, le routeur recherche de classes de composant avec un `RouteAttribute` et restitue le composant a un modèle d’itinéraire qui correspond à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="74d36-248">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="74d36-249">Plusieurs modèles d’itinéraire peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-249">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="74d36-250">Le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="74d36-250">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="74d36-251">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="74d36-251">Route parameters</span></span>

<span data-ttu-id="74d36-252">Composants peuvent recevoir des paramètres d’itinéraire à partir du modèle d’itinéraire fourni dans la `@page` directive.</span><span class="sxs-lookup"><span data-stu-id="74d36-252">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="74d36-253">Le routeur utilise les paramètres d’itinéraire pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="74d36-253">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="74d36-254">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-254">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="74d36-255">Paramètres facultatifs ne sont pas pris en charge, par conséquent, deux `@page` directives sont appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="74d36-255">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="74d36-256">La première permet la navigation vers le composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="74d36-256">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="74d36-257">La seconde `@page` directive prend le `{text}` paramètre d’itinéraire et affecte la valeur à la `Text` propriété.</span><span class="sxs-lookup"><span data-stu-id="74d36-257">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="74d36-258">Héritage de classe de base pour une expérience « code-behind »</span><span class="sxs-lookup"><span data-stu-id="74d36-258">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="74d36-259">Fichiers composants (*.cshtml*) mélanger le balisage HTML et C# code dans le même fichier de traitement.</span><span class="sxs-lookup"><span data-stu-id="74d36-259">Component files (*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="74d36-260">Le `@inherits` directive peut être utilisée pour fournir une expérience de « code-behind » qui sépare le balisage de composant à partir du code de traitement des applications de composants de Razor.</span><span class="sxs-lookup"><span data-stu-id="74d36-260">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="74d36-261">Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) montre comment un composant peut hériter d’une classe de base, `BlazorRocksBase`, pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-261">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="74d36-262">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-262">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="74d36-263">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="74d36-263">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="74d36-264">La classe de base doit dériver de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="74d36-264">The base class should derive from `ComponentBase`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="74d36-265">Prise en charge de Razor</span><span class="sxs-lookup"><span data-stu-id="74d36-265">Razor support</span></span>

**<span data-ttu-id="74d36-266">Directives Razor</span><span class="sxs-lookup"><span data-stu-id="74d36-266">Razor directives</span></span>**

<span data-ttu-id="74d36-267">Directives Razor sont affichés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="74d36-267">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="74d36-268">Directive</span><span class="sxs-lookup"><span data-stu-id="74d36-268">Directive</span></span> | <span data-ttu-id="74d36-269">Description</span><span class="sxs-lookup"><span data-stu-id="74d36-269">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="74d36-270">\@functions</span><span class="sxs-lookup"><span data-stu-id="74d36-270">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="74d36-271">Ajoute un C# bloc de code pour un composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-271">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="74d36-272">Implémente une interface pour la classe de composant généré.</span><span class="sxs-lookup"><span data-stu-id="74d36-272">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="74d36-273">\@Hérite de</span><span class="sxs-lookup"><span data-stu-id="74d36-273">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="74d36-274">Fournit un contrôle complet de la classe qui hérite du composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-274">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="74d36-275">\@inject</span><span class="sxs-lookup"><span data-stu-id="74d36-275">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="74d36-276">Permet l’injection de code à partir de service le [conteneur de service](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="74d36-276">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="74d36-277">Pour plus d’informations, consultez [Injection de dépendances dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="74d36-277">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="74d36-278">Spécifie un composant de mise en forme.</span><span class="sxs-lookup"><span data-stu-id="74d36-278">Specifies a layout component.</span></span> <span data-ttu-id="74d36-279">Composants de mise en page sont utilisés afin d’éviter des incohérences et la duplication de code.</span><span class="sxs-lookup"><span data-stu-id="74d36-279">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="74d36-280">\@page</span><span class="sxs-lookup"><span data-stu-id="74d36-280">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="74d36-281">Spécifie que le composant doit gérer les demandes directement.</span><span class="sxs-lookup"><span data-stu-id="74d36-281">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="74d36-282">Le `@page` directive peut être spécifiée avec un itinéraire et des paramètres facultatifs.</span><span class="sxs-lookup"><span data-stu-id="74d36-282">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="74d36-283">Contrairement aux Pages Razor, le `@page` directive n’a pas besoin être la première directive en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="74d36-283">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="74d36-284">Pour plus d’informations, consultez [Routage](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="74d36-284">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [<span data-ttu-id="74d36-285">\@À l’aide de</span><span class="sxs-lookup"><span data-stu-id="74d36-285">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="74d36-286">Ajoute le C# `using` directive à la classe de composant généré.</span><span class="sxs-lookup"><span data-stu-id="74d36-286">Adds the C# `using` directive to the generated component class.</span></span> |
| [<span data-ttu-id="74d36-287">\@addTagHelper</span><span class="sxs-lookup"><span data-stu-id="74d36-287">\@addTagHelper</span></span>](xref:mvc/views/razor#tag-helpers) | <span data-ttu-id="74d36-288">Utiliser `@addTagHelper` pour utiliser un composant dans un autre assembly que l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="74d36-288">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

**<span data-ttu-id="74d36-289">Attributs conditionnels</span><span class="sxs-lookup"><span data-stu-id="74d36-289">Conditional attributes</span></span>**

<span data-ttu-id="74d36-290">Les attributs sont rendus conditionnelle basée sur la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="74d36-290">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="74d36-291">Si la valeur est `false` ou `null`, l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="74d36-291">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="74d36-292">Si la valeur est `true`, l’attribut est rendu réduite.</span><span class="sxs-lookup"><span data-stu-id="74d36-292">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="74d36-293">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est restitué dans le balisage du contrôle :</span><span class="sxs-lookup"><span data-stu-id="74d36-293">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="74d36-294">Si `IsCompleted` est `true`, la case à cocher est rendue en tant que :</span><span class="sxs-lookup"><span data-stu-id="74d36-294">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked>
```

<span data-ttu-id="74d36-295">Si `IsCompleted` est `false`, la case à cocher est rendue en tant que :</span><span class="sxs-lookup"><span data-stu-id="74d36-295">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox">
```

**<span data-ttu-id="74d36-296">Informations supplémentaires sur Razor</span><span class="sxs-lookup"><span data-stu-id="74d36-296">Additional information on Razor</span></span>**

<span data-ttu-id="74d36-297">Pour plus d’informations sur Razor, consultez le [référence de la syntaxe Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="74d36-297">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="74d36-298">HTML brut</span><span class="sxs-lookup"><span data-stu-id="74d36-298">Raw HTML</span></span>

<span data-ttu-id="74d36-299">Les chaînes sont normalement restitués à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage, qu'elles peuvent contenir est ignorée et traitée en tant que texte littéral.</span><span class="sxs-lookup"><span data-stu-id="74d36-299">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="74d36-300">Pour afficher le HTML brut, encapsuler le contenu HTML dans un `MarkupString` valeur.</span><span class="sxs-lookup"><span data-stu-id="74d36-300">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="74d36-301">La valeur est analysée en tant que HTML ou SVG et insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="74d36-301">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="74d36-302">Rendu HTML brut construit à partir d’une non fiables source est un **risque de sécurité** et doit être évitée !</span><span class="sxs-lookup"><span data-stu-id="74d36-302">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="74d36-303">L’exemple suivant illustre l’utilisation du `MarkupString` type à ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :</span><span class="sxs-lookup"><span data-stu-id="74d36-303">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="74d36-304">Composants basés sur des modèles</span><span class="sxs-lookup"><span data-stu-id="74d36-304">Templated components</span></span>

<span data-ttu-id="74d36-305">Composants basés sur des modèles sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, ce qui peuvent ensuite être utilisés comme partie de la logique de rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-305">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="74d36-306">Composants basés sur des modèles vous permettent de vous montre comment créer des composants de haut niveau qui sont plus facilement réutilisables que les composants standards.</span><span class="sxs-lookup"><span data-stu-id="74d36-306">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="74d36-307">Voici deux exemples sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="74d36-307">A couple of examples include:</span></span>

* <span data-ttu-id="74d36-308">Un composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête de la table, de lignes et de pied de page.</span><span class="sxs-lookup"><span data-stu-id="74d36-308">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="74d36-309">Un composant de liste qui permet à un utilisateur de spécifier un modèle pour le rendu des éléments dans une liste.</span><span class="sxs-lookup"><span data-stu-id="74d36-309">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="74d36-310">Paramètres de modèle</span><span class="sxs-lookup"><span data-stu-id="74d36-310">Template parameters</span></span>

<span data-ttu-id="74d36-311">Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres de composant de type `RenderFragment` ou `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="74d36-311">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="74d36-312">Un fragment de rendu représente un segment de l’interface utilisateur qui est restitué par le composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-312">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="74d36-313">Un fragment de rendu prend un paramètre qui peut être spécifié lorsque le fragment de rendu est appelé.</span><span class="sxs-lookup"><span data-stu-id="74d36-313">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="74d36-314">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-314">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="74d36-315">Lorsque vous utilisez un composant basé sur un modèle, les paramètres du modèle peuvent être spécifiés à l’aide des éléments enfants qui correspondent aux noms des paramètres (`TableHeader` et `RowTemplate` dans l’exemple suivant) :</span><span class="sxs-lookup"><span data-stu-id="74d36-315">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="74d36-316">Paramètres de contexte de modèle</span><span class="sxs-lookup"><span data-stu-id="74d36-316">Template context parameters</span></span>

<span data-ttu-id="74d36-317">Arguments de composant de type `RenderFragment<T>` passé comme éléments ont un paramètre implicit nommé `context` (par exemple à partir de l’exemple de code précédent, `@context.PetId`), mais vous pouvez modifier le nom de paramètre en utilisant le `Context` attribut sur l’enfant élément.</span><span class="sxs-lookup"><span data-stu-id="74d36-317">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="74d36-318">Dans l’exemple suivant, le `RowTemplate` l’élément `Context` attribut spécifie le `pet` paramètre :</span><span class="sxs-lookup"><span data-stu-id="74d36-318">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="74d36-319">Vous pouvez également spécifier le `Context` attribut sur l’élément de composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-319">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="74d36-320">Spécifié `Context` attribut s’applique à tous les paramètres de modèle spécifié.</span><span class="sxs-lookup"><span data-stu-id="74d36-320">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="74d36-321">Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans habillage du n’importe quel élément enfant).</span><span class="sxs-lookup"><span data-stu-id="74d36-321">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="74d36-322">Dans l’exemple suivant, le `Context` attribut apparaît sur le `TableTemplate` élément et s’applique à tous les paramètres de modèle :</span><span class="sxs-lookup"><span data-stu-id="74d36-322">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="74d36-323">Composants typées de génériques</span><span class="sxs-lookup"><span data-stu-id="74d36-323">Generic-typed components</span></span>

<span data-ttu-id="74d36-324">Composants basés sur des modèles sont souvent de manière générique de type.</span><span class="sxs-lookup"><span data-stu-id="74d36-324">Templated components are often generically typed.</span></span> <span data-ttu-id="74d36-325">Par exemple, un composant de modèle de vue de liste générique peut être utilisé pour restituer `IEnumerable<T>` valeurs.</span><span class="sxs-lookup"><span data-stu-id="74d36-325">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="74d36-326">Pour définir un composant générique, utilisez la `@typeparam` directive pour spécifier les paramètres de type.</span><span class="sxs-lookup"><span data-stu-id="74d36-326">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="74d36-327">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-327">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

<span data-ttu-id="74d36-328">Lorsque vous utilisez des composants typées de génériques, le paramètre de type est déduit si possible :</span><span class="sxs-lookup"><span data-stu-id="74d36-328">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="74d36-329">Sinon, le paramètre de type doit être explicitement spécifié à l’aide d’un attribut qui correspond au nom du paramètre de type.</span><span class="sxs-lookup"><span data-stu-id="74d36-329">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="74d36-330">Dans l’exemple suivant, `TItem="Pet"` Spécifie le type :</span><span class="sxs-lookup"><span data-stu-id="74d36-330">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="74d36-331">Paramètres et valeurs en cascade</span><span class="sxs-lookup"><span data-stu-id="74d36-331">Cascading values and parameters</span></span>

<span data-ttu-id="74d36-332">Dans certains scénarios, il est peu pratique pour transmettre les données à partir d’un composant d’ancêtre à un composant descendant à l’aide [paramètres du composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-332">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="74d36-333">Les valeurs et les paramètres en cascade résolvent ce problème en fournissant un moyen pratique d’un composant d’ancêtre à fournir une valeur pour tous ses composants descendants.</span><span class="sxs-lookup"><span data-stu-id="74d36-333">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="74d36-334">Les valeurs et les paramètres en cascade fournissent également une approche pour coordonner des composants.</span><span class="sxs-lookup"><span data-stu-id="74d36-334">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="74d36-335">Exemple de thème</span><span class="sxs-lookup"><span data-stu-id="74d36-335">Theme example</span></span>

<span data-ttu-id="74d36-336">Dans l’exemple suivant *thème* exemple à partir de l’exemple d’application, le `ThemeInfo` classe spécifie les informations de thème pour le flux vers le bas de la hiérarchie des composants afin que tous les boutons dans une partie donnée de l’application partagent le même style.</span><span class="sxs-lookup"><span data-stu-id="74d36-336">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="74d36-337">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="74d36-337">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="74d36-338">Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="74d36-338">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="74d36-339">Le composant de valeur en cascade encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants dans cette sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="74d36-339">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="74d36-340">Par exemple, l’exemple d’application spécifie les informations de thème (`ThemeInfo`) dans une des dispositions de l’application comme un paramètre en cascade pour tous les composants qui composent le corps de la disposition de la `@Body` propriété.</span><span class="sxs-lookup"><span data-stu-id="74d36-340">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> `ButtonClass` <span data-ttu-id="74d36-341">reçoit une valeur de `btn-success` dans le composant de mise en page.</span><span class="sxs-lookup"><span data-stu-id="74d36-341">is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="74d36-342">N’importe quel composant descendant peut consommer de cette propriété via le `ThemeInfo` objet en cascade.</span><span class="sxs-lookup"><span data-stu-id="74d36-342">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="74d36-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-343">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

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

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="74d36-344">Pour rendre des valeurs en cascade, les composants déclarer des paramètres en cascade à l’aide de la `[CascadingParameter]` attribut ou selon une valeur de nom de chaîne :</span><span class="sxs-lookup"><span data-stu-id="74d36-344">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="74d36-345">Liaison avec une valeur de nom de chaîne s’applique si vous avez plusieurs valeurs en cascade du même type et que vous avez besoin pour les différencier dans le même sous-arbre.</span><span class="sxs-lookup"><span data-stu-id="74d36-345">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="74d36-346">Valeurs en cascade sont liés aux paramètres en cascade par type.</span><span class="sxs-lookup"><span data-stu-id="74d36-346">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="74d36-347">Dans l’exemple d’application, le composant de thème des paramètres de valeurs en cascade est lié à la `ThemeInfo` valeur en cascade à un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="74d36-347">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="74d36-348">Le paramètre est utilisé pour définir la classe CSS pour un des boutons affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="74d36-348">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="74d36-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-349">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="74d36-350">Exemple de TabSet</span><span class="sxs-lookup"><span data-stu-id="74d36-350">TabSet example</span></span>

<span data-ttu-id="74d36-351">Paramètres en cascade permettent également aux composants de collaborer au travers de la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="74d36-351">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="74d36-352">Par exemple, considérez les éléments suivants *TabSet* exemple dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="74d36-352">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="74d36-353">L’exemple d’application a une `ITab` interface qui implémentent des onglets :</span><span class="sxs-lookup"><span data-stu-id="74d36-353">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="74d36-354">Le composant TabSet des paramètres de valeurs en cascade utilise le composant ensemble d’onglets, qui contient plusieurs composants d’onglet :</span><span class="sxs-lookup"><span data-stu-id="74d36-354">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="74d36-355">Les composants d’onglet enfants ne sont pas explicitement transmis en tant que paramètres à l’ensemble de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="74d36-355">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="74d36-356">Au lieu de cela, les composants d’onglet enfants font partie du contenu enfant de l’ensemble de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="74d36-356">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="74d36-357">Toutefois, l’ensemble de l’onglet doit tout savoir sur chaque composant de l’onglet afin qu’il puisse afficher les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter du code supplémentaire, le composant de l’ensemble d’onglets *peut fournir lui-même comme une valeur en cascade* qui est ensuite récupéré par les composants d’onglet descendants.</span><span class="sxs-lookup"><span data-stu-id="74d36-357">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="74d36-358">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-358">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="74d36-359">La capture de composants onglet descendante l’ensemble d’onglets contenant comme un paramètre en cascade, afin que les composants de l’onglet ajoutent eux-mêmes à l’ensemble d’onglets et les coordonnées sur l’onglet est actif.</span><span class="sxs-lookup"><span data-stu-id="74d36-359">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="74d36-360">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-360">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="74d36-361">Modèles Razor</span><span class="sxs-lookup"><span data-stu-id="74d36-361">Razor templates</span></span>

<span data-ttu-id="74d36-362">Restituer fragments peuvent être définies à l’aide de la syntaxe du modèle Razor.</span><span class="sxs-lookup"><span data-stu-id="74d36-362">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="74d36-363">Les modèles Razor sont un moyen de définir un extrait de code de l’interface utilisateur et le format suivant :</span><span class="sxs-lookup"><span data-stu-id="74d36-363">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="74d36-364">L’exemple suivant illustre comment spécifier `RenderFragment` et `RenderFragment<T>` valeurs.</span><span class="sxs-lookup"><span data-stu-id="74d36-364">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="74d36-365">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="74d36-365">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="74d36-366">Restituer des fragments définis à l’aide de Razor modèles peuvent être passés comme arguments à des composants basés sur des modèles ou restitués directement.</span><span class="sxs-lookup"><span data-stu-id="74d36-366">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="74d36-367">Par exemple, les modèles précédentes sont directement restitués avec le balisage Razor suivant :</span><span class="sxs-lookup"><span data-stu-id="74d36-367">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="74d36-368">Sortie rendue :</span><span class="sxs-lookup"><span data-stu-id="74d36-368">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="74d36-369">Logique de RenderTreeBuilder manuelle</span><span class="sxs-lookup"><span data-stu-id="74d36-369">Manual RenderTreeBuilder logic</span></span>

`Microsoft.AspNetCore.Components.RenderTree` <span data-ttu-id="74d36-370">Fournit des méthodes pour la manipulation des composants et des éléments, notamment la création de composants manuellement dans C# code.</span><span class="sxs-lookup"><span data-stu-id="74d36-370">provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="74d36-371">Utilisation de `RenderTreeBuilder` pour créer des composants est un scénario avancé.</span><span class="sxs-lookup"><span data-stu-id="74d36-371">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="74d36-372">Un composant incorrect (par exemple, une balise non fermés) peut entraîner un comportement non défini.</span><span class="sxs-lookup"><span data-stu-id="74d36-372">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="74d36-373">Envisagez le composant Pet détails suivant (*PetDetails.razor* dans Razor composants ; *PetDetails.cshtml* dans Blazor), qui peut être généré manuellement dans un autre composant :</span><span class="sxs-lookup"><span data-stu-id="74d36-373">Consider the following Pet Details component (*PetDetails.razor* in Razor Components; *PetDetails.cshtml* in Blazor), which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="74d36-374">Dans l’exemple suivant, la boucle dans le `CreateComponent` méthode génère trois composants Pet détails.</span><span class="sxs-lookup"><span data-stu-id="74d36-374">In the following example, the loop in the `CreateComponent` method generates three Pet Details components.</span></span> <span data-ttu-id="74d36-375">Lors de l’appel `RenderTreeBuilder` méthodes pour créer les composants (`OpenComponent` et `AddAttribute`), les numéros de séquence sont les numéros de ligne de code source.</span><span class="sxs-lookup"><span data-stu-id="74d36-375">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="74d36-376">Les composants de Razor algorithme de différence s’appuie sur les numéros de séquence correspondant à des lignes distinctes de code, pas distincte appeler les appels.</span><span class="sxs-lookup"><span data-stu-id="74d36-376">The Razor Components difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="74d36-377">Lors de la création d’un composant avec `RenderTreeBuilder` méthodes, coder en dur les arguments pour les numéros de séquence.</span><span class="sxs-lookup"><span data-stu-id="74d36-377">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> **<span data-ttu-id="74d36-378">À l’aide d’un calcul ou un compteur pour générer le numéro de séquence peut entraîner une baisse des performances.</span><span class="sxs-lookup"><span data-stu-id="74d36-378">Using a calculation or counter to generate the sequence number can lead to poor performance.</span></span>**

<span data-ttu-id="74d36-379">Composant (*BuiltContent.razor* dans Razor composants ; *BuiltContent.cshtml* dans Blazor) :</span><span class="sxs-lookup"><span data-stu-id="74d36-379">Built component (*BuiltContent.razor* in Razor Components; *BuiltContent.cshtml* in Blazor):</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
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
