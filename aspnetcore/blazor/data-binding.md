---
title: Liaison de données ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur les scénarios de liaison de données pour les composants et les éléments DOM dans Blazor applications.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: c38e6095d4e93d3eead10fa8bb0356b2113bb220
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453232"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="156fb-103">Liaison de données ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="156fb-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="156fb-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="156fb-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="156fb-105">La liaison de données aux composants et aux éléments DOM s’effectue à l’aide de l’attribut [`@bind`](xref:mvc/views/razor#bind) .</span><span class="sxs-lookup"><span data-stu-id="156fb-105">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="156fb-106">L’exemple suivant lie une propriété `CurrentValue` à la valeur de la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="156fb-106">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="156fb-107">Lorsque la zone de texte perd le focus, la valeur de la propriété est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="156fb-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="156fb-108">La zone de texte est mise à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, et non en réponse à la modification de la valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="156fb-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="156fb-109">Étant donné que les composants s’affichent après l’exécution du code du gestionnaire d’événements, les mises à jour de propriétés sont *généralement* reflétées dans l’interface utilisateur immédiatement après le déclenchement d’un gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="156fb-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="156fb-110">L’utilisation de `@bind` avec la propriété `CurrentValue` (`<input @bind="CurrentValue" />`) équivaut essentiellement à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="156fb-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="156fb-111">Lors du rendu du composant, le `value` de l’élément d’entrée provient de la propriété `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="156fb-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="156fb-112">Lorsque l’utilisateur tape dans la zone de texte et modifie le focus de l’élément, l’événement `onchange` est déclenché et la propriété `CurrentValue` est définie sur la valeur modifiée.</span><span class="sxs-lookup"><span data-stu-id="156fb-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="156fb-113">En réalité, la génération de code est plus complexe, car `@bind` gère les cas où les conversions de types sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="156fb-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="156fb-114">En principe, `@bind` associe la valeur actuelle d’une expression à un attribut `value` et gère les modifications à l’aide du gestionnaire inscrit.</span><span class="sxs-lookup"><span data-stu-id="156fb-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="156fb-115">En plus de gérer les événements `onchange` avec la syntaxe `@bind`, une propriété ou un champ peut être lié à l’aide d’autres événements en spécifiant un attribut [`@bind-value`](xref:mvc/views/razor#bind) avec un paramètre `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="156fb-115">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="156fb-116">L’exemple suivant lie la propriété `CurrentValue` pour l’événement `oninput` :</span><span class="sxs-lookup"><span data-stu-id="156fb-116">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="156fb-117">Contrairement à `onchange`, qui se déclenche lorsque l’élément perd le focus, `oninput` se déclenche lorsque la valeur de la zone de texte change.</span><span class="sxs-lookup"><span data-stu-id="156fb-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="156fb-118">`@bind-value` dans l’exemple précédent lie :</span><span class="sxs-lookup"><span data-stu-id="156fb-118">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="156fb-119">Expression (`CurrentValue`) spécifiée à l’attribut `value` de l’élément.</span><span class="sxs-lookup"><span data-stu-id="156fb-119">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="156fb-120">Délégué d’événement de modification à l’événement spécifié par `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="156fb-120">A change event delegate to the event specified by `@bind-value:event`.</span></span>

### <a name="unparsable-values"></a><span data-ttu-id="156fb-121">Valeurs inanalysables</span><span class="sxs-lookup"><span data-stu-id="156fb-121">Unparsable values</span></span>

<span data-ttu-id="156fb-122">Quand un utilisateur fournit une valeur non analysable à un élément DataBound, la valeur unanalysable est automatiquement rétablie à sa valeur précédente lorsque l’événement de liaison est déclenché.</span><span class="sxs-lookup"><span data-stu-id="156fb-122">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="156fb-123">Examinez le cas suivant :</span><span class="sxs-lookup"><span data-stu-id="156fb-123">Consider the following scenario:</span></span>

* <span data-ttu-id="156fb-124">Un élément `<input>` est lié à un type `int` avec une valeur initiale de `123`:</span><span class="sxs-lookup"><span data-stu-id="156fb-124">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="156fb-125">L’utilisateur met à jour la valeur de l’élément à `123.45` dans la page et modifie le focus de l’élément.</span><span class="sxs-lookup"><span data-stu-id="156fb-125">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="156fb-126">Dans le scénario précédent, la valeur de l’élément est rétablie à `123`.</span><span class="sxs-lookup"><span data-stu-id="156fb-126">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="156fb-127">Lorsque la valeur `123.45` est rejetée en faveur de la valeur d’origine de `123`, l’utilisateur sait que sa valeur n’a pas été acceptée.</span><span class="sxs-lookup"><span data-stu-id="156fb-127">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="156fb-128">Par défaut, la liaison s’applique à l’événement `onchange` de l’élément (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="156fb-128">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="156fb-129">Utilisez `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` pour définir un événement différent.</span><span class="sxs-lookup"><span data-stu-id="156fb-129">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="156fb-130">Pour l’événement `oninput` (`@bind-value:event="oninput"`), la Reversion se produit après toute séquence de touches qui introduit une valeur non analysable.</span><span class="sxs-lookup"><span data-stu-id="156fb-130">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="156fb-131">Quand vous ciblez l’événement `oninput` avec un type lié `int`, un utilisateur ne peut pas taper un caractère `.`.</span><span class="sxs-lookup"><span data-stu-id="156fb-131">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="156fb-132">Si un caractère de `.` est immédiatement supprimé, l’utilisateur reçoit des commentaires immédiats que seuls des nombres entiers sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="156fb-132">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="156fb-133">Il existe des scénarios où le rétablissement de la valeur sur l’événement `oninput` n’est pas idéal, par exemple lorsque l’utilisateur doit être autorisé à effacer une valeur de `<input>` non analysable.</span><span class="sxs-lookup"><span data-stu-id="156fb-133">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="156fb-134">Les alternatives sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="156fb-134">Alternatives include:</span></span>

* <span data-ttu-id="156fb-135">N’utilisez pas l’événement `oninput`.</span><span class="sxs-lookup"><span data-stu-id="156fb-135">Don't use the `oninput` event.</span></span> <span data-ttu-id="156fb-136">Utilisez l’événement `onchange` par défaut (`@bind="{PROPERTY OR FIELD}"`), où une valeur non valide n’est pas rétablie tant que l’élément n’a pas perdu le focus.</span><span class="sxs-lookup"><span data-stu-id="156fb-136">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="156fb-137">Effectuer une liaison à un type Nullable, tel que `int?` ou `string`, et fournir une logique personnalisée pour gérer les entrées non valides.</span><span class="sxs-lookup"><span data-stu-id="156fb-137">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="156fb-138">Utilisez un [composant de validation de formulaire](xref:blazor/forms-validation), tel que `InputNumber` ou `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="156fb-138">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="156fb-139">Les composants de validation de formulaire offrent une prise en charge intégrée pour gérer les entrées non valides.</span><span class="sxs-lookup"><span data-stu-id="156fb-139">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="156fb-140">Composants de validation de formulaire :</span><span class="sxs-lookup"><span data-stu-id="156fb-140">Form validation components:</span></span>
  * <span data-ttu-id="156fb-141">Permet à l’utilisateur de fournir une entrée non valide et de recevoir des erreurs de validation sur les `EditContext`associées.</span><span class="sxs-lookup"><span data-stu-id="156fb-141">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="156fb-142">Affichez les erreurs de validation dans l’interface utilisateur sans interférer avec l’utilisateur qui saisit des données Webform supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="156fb-142">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

### <a name="format-strings"></a><span data-ttu-id="156fb-143">Chaînes de format</span><span class="sxs-lookup"><span data-stu-id="156fb-143">Format strings</span></span>

<span data-ttu-id="156fb-144">La liaison de données fonctionne avec les chaînes de format <xref:System.DateTime> à l’aide de [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="156fb-144">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="156fb-145">D’autres expressions de mise en forme, telles que les formats de devise ou de nombre, ne sont pas disponibles pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="156fb-145">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="156fb-146">Dans le code précédent, le type de champ de l’élément `<input>` (`type`) a par défaut la valeur `text`.</span><span class="sxs-lookup"><span data-stu-id="156fb-146">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="156fb-147">`@bind:format` est pris en charge pour la liaison des types .NET suivants :</span><span class="sxs-lookup"><span data-stu-id="156fb-147">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="156fb-148"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="156fb-148"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="156fb-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="156fb-149"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="156fb-150">L’attribut `@bind:format` spécifie le format de date à appliquer aux `value` de l’élément `<input>`.</span><span class="sxs-lookup"><span data-stu-id="156fb-150">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="156fb-151">Le format est également utilisé pour analyser la valeur lorsqu’un événement `onchange` se produit.</span><span class="sxs-lookup"><span data-stu-id="156fb-151">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="156fb-152">Il n’est pas recommandé de spécifier un format pour le type de champ `date`, car Blazor offre une prise en charge intégrée de la mise en forme des dates.</span><span class="sxs-lookup"><span data-stu-id="156fb-152">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="156fb-153">Malgré la recommandation, utilisez uniquement le format de date `yyyy-MM-dd` pour que la liaison fonctionne correctement si un format est fourni avec le type de champ `date` :</span><span class="sxs-lookup"><span data-stu-id="156fb-153">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="156fb-154">Liaison parent-enfant avec les paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="156fb-154">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="156fb-155">La liaison reconnaît les paramètres du composant, où `@bind-{property}` pouvez lier une valeur de propriété d’un composant parent à un composant enfant.</span><span class="sxs-lookup"><span data-stu-id="156fb-155">Binding recognizes component parameters, where `@bind-{property}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="156fb-156">La liaison d’un enfant à un parent est couverte dans la [liaison enfant-parent avec la section de liaison chaînée](#child-to-parent-binding-with-chained-bind) .</span><span class="sxs-lookup"><span data-stu-id="156fb-156">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="156fb-157">Le composant enfant suivant (`ChildComponent`) a un paramètre de composant `Year` et `YearChanged` rappel :</span><span class="sxs-lookup"><span data-stu-id="156fb-157">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="156fb-158">`EventCallback<T>` est expliqué dans <xref:blazor/event-handling#eventcallback>.</span><span class="sxs-lookup"><span data-stu-id="156fb-158">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="156fb-159">Le composant parent suivant utilise :</span><span class="sxs-lookup"><span data-stu-id="156fb-159">The following parent component uses:</span></span>

* <span data-ttu-id="156fb-160">`ChildComponent` et lie le paramètre `ParentYear` du parent au paramètre `Year` sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="156fb-160">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="156fb-161">L’événement `onclick` est utilisé pour déclencher la méthode `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="156fb-161">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="156fb-162">Pour plus d’informations, consultez <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="156fb-162">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="156fb-163">Le chargement de l' `ParentComponent` produit le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="156fb-163">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="156fb-164">Si la valeur de la propriété `ParentYear` est modifiée en sélectionnant le bouton dans la `ParentComponent`, la propriété `Year` de la `ChildComponent` est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="156fb-164">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="156fb-165">La nouvelle valeur de `Year` est rendue dans l’interface utilisateur lorsque le `ParentComponent` est restitué à nouveau :</span><span class="sxs-lookup"><span data-stu-id="156fb-165">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="156fb-166">Le paramètre `Year` peut être lié, car il a un `YearChanged` événement associé qui correspond au type du paramètre `Year`.</span><span class="sxs-lookup"><span data-stu-id="156fb-166">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="156fb-167">Par Convention, `<ChildComponent @bind-Year="ParentYear" />` revient essentiellement à écrire :</span><span class="sxs-lookup"><span data-stu-id="156fb-167">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="156fb-168">En général, une propriété peut être liée à un gestionnaire d’événements correspondant à l’aide de `@bind-property:event` attribut.</span><span class="sxs-lookup"><span data-stu-id="156fb-168">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="156fb-169">Par exemple, la propriété `MyProp` peut être liée à `MyEventHandler` à l’aide des deux attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="156fb-169">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="156fb-170">Liaison enfant-parent avec liaison chaînée</span><span class="sxs-lookup"><span data-stu-id="156fb-170">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="156fb-171">Un scénario courant consiste à chaîner un paramètre lié aux données à un élément de page dans la sortie du composant.</span><span class="sxs-lookup"><span data-stu-id="156fb-171">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="156fb-172">Ce scénario est appelé *liaison chaînée* car plusieurs niveaux de liaison se produisent simultanément.</span><span class="sxs-lookup"><span data-stu-id="156fb-172">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="156fb-173">Une liaison chaînée ne peut pas être implémentée avec `@bind` syntaxe dans l’élément de la page.</span><span class="sxs-lookup"><span data-stu-id="156fb-173">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="156fb-174">Le gestionnaire d’événements et la valeur doivent être spécifiés séparément.</span><span class="sxs-lookup"><span data-stu-id="156fb-174">The event handler and value must be specified separately.</span></span> <span data-ttu-id="156fb-175">Toutefois, un composant parent peut utiliser `@bind` syntaxe avec le paramètre du composant.</span><span class="sxs-lookup"><span data-stu-id="156fb-175">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="156fb-176">Le composant `PasswordField` suivant (*passwordField. Razor*) :</span><span class="sxs-lookup"><span data-stu-id="156fb-176">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="156fb-177">Définit la valeur d’un élément `<input>` sur une propriété `Password`.</span><span class="sxs-lookup"><span data-stu-id="156fb-177">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="156fb-178">Expose les modifications de la propriété `Password` à un composant parent avec un [EventCallback suivante](xref:blazor/event-handling#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="156fb-178">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="156fb-179">Utilise l’événement `onclick` est utilisé pour déclencher la méthode `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="156fb-179">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="156fb-180">Pour plus d’informations, consultez <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="156fb-180">For more information, see <xref:blazor/event-handling>.</span></span>

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

<span data-ttu-id="156fb-181">Le composant `PasswordField` est utilisé dans un autre composant :</span><span class="sxs-lookup"><span data-stu-id="156fb-181">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="156fb-182">Pour effectuer des vérifications ou des erreurs d’interruption sur le mot de passe dans l’exemple précédent :</span><span class="sxs-lookup"><span data-stu-id="156fb-182">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="156fb-183">Créez un champ de stockage pour `Password` (`_password` dans l’exemple de code suivant).</span><span class="sxs-lookup"><span data-stu-id="156fb-183">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="156fb-184">Effectuez les vérifications ou les erreurs d’interruption dans le `Password` Setter.</span><span class="sxs-lookup"><span data-stu-id="156fb-184">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="156fb-185">L’exemple suivant fournit un retour immédiat à l’utilisateur si un espace est utilisé dans la valeur du mot de passe :</span><span class="sxs-lookup"><span data-stu-id="156fb-185">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

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

### <a name="radio-buttons"></a><span data-ttu-id="156fb-186">Cases d’option</span><span class="sxs-lookup"><span data-stu-id="156fb-186">Radio buttons</span></span>

<span data-ttu-id="156fb-187">Pour plus d’informations sur la liaison à des cases d’option dans un formulaire, consultez <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="156fb-187">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>
