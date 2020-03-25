---
title: ASP.NET Core Blazor cycle de vie
author: guardrex
description: Découvrez comment utiliser les méthodes de cycle de vie des composants Razor dans ASP.NET Core applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: 831f575afa6ce11d06c016d43ecd1bb59d09eab6
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218906"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="f6cc8-103">ASP.NET Core Blazor cycle de vie</span><span class="sxs-lookup"><span data-stu-id="f6cc8-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="f6cc8-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f6cc8-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="f6cc8-105">L’infrastructure Blazor comprend des méthodes de cycle de vie synchrones et asynchrones.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="f6cc8-106">Substituez les méthodes de cycle de vie pour effectuer des opérations supplémentaires sur les composants lors de l’initialisation et du rendu des composants.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="f6cc8-107">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="f6cc8-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="f6cc8-108">Méthodes d’initialisation de composant</span><span class="sxs-lookup"><span data-stu-id="f6cc8-108">Component initialization methods</span></span>

<span data-ttu-id="f6cc8-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> sont appelés lorsque le composant est initialisé après avoir reçu ses paramètres initiaux de son composant parent.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="f6cc8-110">Utilisez `OnInitializedAsync` lorsque le composant exécute une opération asynchrone et doit être actualisé à la fin de l’opération.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span>

<span data-ttu-id="f6cc8-111">Pour une opération synchrone, remplacez `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-111">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="f6cc8-112">Pour effectuer une opération asynchrone, substituez `OnInitializedAsync` et utilisez le mot clé `await` sur l’opération :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-112">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor<span data-ttu-id="f6cc8-113"> les applications serveur qui [préaffichent](xref:blazor/hosting-model-configuration#render-mode) l' `OnInitializedAsync` de l’appel de contenu à **_deux reprises_** :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-113"> Server apps that [prerender their content](xref:blazor/hosting-model-configuration#render-mode) call `OnInitializedAsync` **_twice_**:</span></span>

* <span data-ttu-id="f6cc8-114">Une fois lorsque le composant est initialement restitué de manière statique dans le cadre de la page.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-114">Once when the component is initially rendered statically as part of the page.</span></span>
* <span data-ttu-id="f6cc8-115">Une deuxième fois lorsque le navigateur établit une connexion au serveur.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-115">A second time when the browser establishes a connection back to the server.</span></span>

<span data-ttu-id="f6cc8-116">Pour empêcher l’exécution à deux reprises du code du développeur dans `OnInitializedAsync`, consultez la section [reconnexion avec état après le prérendu](#stateful-reconnection-after-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="f6cc8-116">To prevent developer code in `OnInitializedAsync` from running twice, see the [Stateful reconnection after prerendering](#stateful-reconnection-after-prerendering) section.</span></span>

<span data-ttu-id="f6cc8-117">Bien qu’une application Blazor Server soit prérestitué, certaines actions, telles que l’appel en JavaScript, ne sont pas possibles, car une connexion avec le navigateur n’a pas été établie.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-117">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="f6cc8-118">Les composants peuvent avoir besoin d’être restitués différemment lorsqu’ils sont prérendus.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-118">Components may need to render differently when prerendered.</span></span> <span data-ttu-id="f6cc8-119">Pour plus d’informations, consultez la section [détecter quand l’application est un prérendu](#detect-when-the-app-is-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="f6cc8-119">For more information, see the [Detect when the app is prerendering](#detect-when-the-app-is-prerendering) section.</span></span>

<span data-ttu-id="f6cc8-120">Si des gestionnaires d’événements sont configurés, décrochez-les lors de la suppression.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-120">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f6cc8-121">Pour plus d’informations, consultez la section [Suppression de composant avec IDisposable](#component-disposal-with-idisposable) .</span><span class="sxs-lookup"><span data-stu-id="f6cc8-121">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="before-parameters-are-set"></a><span data-ttu-id="f6cc8-122">Les paramètres Before sont définis</span><span class="sxs-lookup"><span data-stu-id="f6cc8-122">Before parameters are set</span></span>

<span data-ttu-id="f6cc8-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> définit les paramètres fournis par le parent du composant dans l’arborescence de rendu :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="f6cc8-124"><xref:Microsoft.AspNetCore.Components.ParameterView> contient le jeu entier de valeurs de paramètre chaque fois que `SetParametersAsync` est appelé.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-124"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="f6cc8-125">L’implémentation par défaut de `SetParametersAsync` définit la valeur de chaque propriété avec l’attribut `[Parameter]` ou `[CascadingParameter]` qui a une valeur correspondante dans le `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-125">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="f6cc8-126">Les paramètres qui n’ont pas de valeur correspondante dans `ParameterView` restent inchangés.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-126">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="f6cc8-127">Si `base.SetParametersAync` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants de la façon requise.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-127">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="f6cc8-128">Par exemple, il n’est pas nécessaire d’assigner les paramètres entrants aux propriétés de la classe.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-128">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

<span data-ttu-id="f6cc8-129">Si des gestionnaires d’événements sont configurés, décrochez-les lors de la suppression.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-129">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f6cc8-130">Pour plus d’informations, consultez la section [Suppression de composant avec IDisposable](#component-disposal-with-idisposable) .</span><span class="sxs-lookup"><span data-stu-id="f6cc8-130">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="f6cc8-131">Une fois les paramètres définis</span><span class="sxs-lookup"><span data-stu-id="f6cc8-131">After parameters are set</span></span>

<span data-ttu-id="f6cc8-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> sont appelées :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="f6cc8-133">Lorsque le composant est initialisé et a reçu son premier jeu de paramètres de son composant parent.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-133">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="f6cc8-134">Lorsque le composant parent est à nouveau rendu et fournit :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-134">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="f6cc8-135">Seuls les types immuables primitifs connus dont au moins un paramètre a été modifié.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-135">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="f6cc8-136">Tous les paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-136">Any complex-typed parameters.</span></span> <span data-ttu-id="f6cc8-137">L’infrastructure ne peut pas savoir si les valeurs d’un paramètre de type complexe ont muté en interne, donc elle traite le jeu de paramètres comme étant modifié.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-137">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="f6cc8-138">Un travail asynchrone lors de l’application de paramètres et de valeurs de propriété doit se produire pendant l’événement de cycle de vie `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-138">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="f6cc8-139">Si des gestionnaires d’événements sont configurés, décrochez-les lors de la suppression.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-139">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f6cc8-140">Pour plus d’informations, consultez la section [Suppression de composant avec IDisposable](#component-disposal-with-idisposable) .</span><span class="sxs-lookup"><span data-stu-id="f6cc8-140">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-component-render"></a><span data-ttu-id="f6cc8-141">Après le rendu du composant</span><span class="sxs-lookup"><span data-stu-id="f6cc8-141">After component render</span></span>

<span data-ttu-id="f6cc8-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> sont appelées après la fin du rendu d’un composant.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="f6cc8-143">Les références d’élément et de composant sont remplies à ce stade.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-143">Element and component references are populated at this point.</span></span> <span data-ttu-id="f6cc8-144">Utilisez cette étape pour effectuer des étapes d’initialisation supplémentaires à l’aide du contenu rendu, par exemple l’activation de bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-144">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="f6cc8-145">Le paramètre `firstRender` pour `OnAfterRenderAsync` et `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-145">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="f6cc8-146">A la valeur `true` la première fois que l’instance de composant est restituée.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-146">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="f6cc8-147">Peut être utilisé pour s’assurer que le travail d’initialisation n’est effectué qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-147">Can be used to ensure that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="f6cc8-148">Le travail asynchrone immédiatement après le rendu doit se produire pendant l’événement de cycle de vie `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-148">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="f6cc8-149">Même si vous retournez un <xref:System.Threading.Tasks.Task> à partir d' `OnAfterRenderAsync`, l’infrastructure ne planifie pas un cycle de rendu supplémentaire pour votre composant une fois cette tâche terminée.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-149">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="f6cc8-150">Cela permet d’éviter une boucle de rendu infinie.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-150">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="f6cc8-151">Elle est différente des autres méthodes de cycle de vie, qui planifient un cycle de rendu supplémentaire une fois que la tâche retournée est terminée.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-151">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="f6cc8-152">`OnAfterRender` et `OnAfterRenderAsync` *ne sont pas appelées lors du prérendu sur le serveur.*</span><span class="sxs-lookup"><span data-stu-id="f6cc8-152">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

<span data-ttu-id="f6cc8-153">Si des gestionnaires d’événements sont configurés, décrochez-les lors de la suppression.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-153">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f6cc8-154">Pour plus d’informations, consultez la section [Suppression de composant avec IDisposable](#component-disposal-with-idisposable) .</span><span class="sxs-lookup"><span data-stu-id="f6cc8-154">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="f6cc8-155">Supprimer l’actualisation de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="f6cc8-155">Suppress UI refreshing</span></span>

<span data-ttu-id="f6cc8-156">Substituez <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> pour supprimer l’actualisation de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-156">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="f6cc8-157">Si l’implémentation retourne `true`, l’interface utilisateur est actualisée :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-157">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="f6cc8-158">`ShouldRender` est appelée chaque fois que le composant est restitué.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-158">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="f6cc8-159">Même si `ShouldRender` est substitué, le composant est toujours restitué initialement.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-159">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="f6cc8-160">Changements d'état</span><span class="sxs-lookup"><span data-stu-id="f6cc8-160">State changes</span></span>

<span data-ttu-id="f6cc8-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifie le composant que son état a changé.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="f6cc8-162">Le cas échéant, l’appel de `StateHasChanged` entraîne le rerendu du composant.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-162">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="f6cc8-163">Gérer les actions asynchrones incomplètes au rendu</span><span class="sxs-lookup"><span data-stu-id="f6cc8-163">Handle incomplete async actions at render</span></span>

<span data-ttu-id="f6cc8-164">Les actions asynchrones exécutées dans des événements de cycle de vie peuvent ne pas être terminées avant le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-164">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="f6cc8-165">Les objets peuvent être `null` ou remplis de façon incomplète avec des données pendant l’exécution de la méthode Lifecycle.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-165">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="f6cc8-166">Fournissez une logique de rendu pour confirmer que les objets sont initialisés.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-166">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="f6cc8-167">Affichez les éléments d’interface utilisateur d’espace réservé (par exemple, un message de chargement) pendant que les objets sont `null`.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-167">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="f6cc8-168">Dans le composant `FetchData` des modèles Blazor, `OnInitializedAsync` est remplacé par asynchrone recevoir les données de prévision (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="f6cc8-168">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="f6cc8-169">Lorsque `forecasts` est `null`, un message de chargement s’affiche pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-169">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="f6cc8-170">Une fois la `Task` retournée par `OnInitializedAsync` terminée, le composant est rerendu avec l’État mis à jour.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-170">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="f6cc8-171">*Pages/fetchData. Razor* dans le modèle Blazor Server :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-171">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="f6cc8-172">Suppression de composant avec IDisposable</span><span class="sxs-lookup"><span data-stu-id="f6cc8-172">Component disposal with IDisposable</span></span>

<span data-ttu-id="f6cc8-173">Si un composant implémente <xref:System.IDisposable>, la [méthode dispose](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-173">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="f6cc8-174">Le composant suivant utilise `@implements IDisposable` et la méthode `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-174">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```razor
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

> [!NOTE]
> <span data-ttu-id="f6cc8-175">L’appel de <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> dans `Dispose` n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-175">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="f6cc8-176">`StateHasChanged` peut être appelé dans le cadre de la suppression du convertisseur, il n’est donc pas possible de demander des mises à jour de l’interface utilisateur à ce stade.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-176">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

<span data-ttu-id="f6cc8-177">Annule l’abonnement des gestionnaires d’événements des événements .NET.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-177">Unsubscribe event handlers from .NET events.</span></span> <span data-ttu-id="f6cc8-178">Les exemples de [formulaire deBlazor](xref:blazor/forms-validation) suivants montrent comment décrocher un gestionnaire d’événements dans la méthode `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-178">The following [Blazor form](xref:blazor/forms-validation) examples show how to unhook an event handler in the `Dispose` method:</span></span>

* <span data-ttu-id="f6cc8-179">Approche de champ privé et lambda</span><span class="sxs-lookup"><span data-stu-id="f6cc8-179">Private field and lambda approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* <span data-ttu-id="f6cc8-180">Approche de la méthode privée</span><span class="sxs-lookup"><span data-stu-id="f6cc8-180">Private method approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a><span data-ttu-id="f6cc8-181">des erreurs</span><span class="sxs-lookup"><span data-stu-id="f6cc8-181">Handle errors</span></span>

<span data-ttu-id="f6cc8-182">Pour plus d’informations sur la gestion des erreurs lors de l’exécution de la méthode de cycle de vie, consultez <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-182">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>

## <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="f6cc8-183">Reconnexion avec état après le prérendu</span><span class="sxs-lookup"><span data-stu-id="f6cc8-183">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="f6cc8-184">Dans une application Blazor Server lorsque `RenderMode` est `ServerPrerendered`, le composant est initialement restitué de manière statique dans le cadre de la page.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-184">In a Blazor Server app when `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="f6cc8-185">Une fois que le navigateur a établi une connexion au serveur, le composant est *à nouveau*rendu et le composant est maintenant interactif.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-185">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="f6cc8-186">Si la méthode de cycle de vie [OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods) pour initialiser le composant est présente, la méthode est exécutée *deux fois*:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-186">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="f6cc8-187">Lorsque le composant est prérendu statiquement.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-187">When the component is prerendered statically.</span></span>
* <span data-ttu-id="f6cc8-188">Après l’établissement de la connexion au serveur.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-188">After the server connection has been established.</span></span>

<span data-ttu-id="f6cc8-189">Cela peut entraîner une modification notable des données affichées dans l’interface utilisateur lorsque le composant est finalement restitué.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-189">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="f6cc8-190">Pour éviter le double rendu du scénario dans une application Blazor Server :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-190">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="f6cc8-191">Transmettez un identificateur qui peut être utilisé pour mettre en cache l’État pendant le prérendu et pour récupérer l’état après le redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-191">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="f6cc8-192">Utilisez l’identificateur pendant le prérendu pour enregistrer l’état du composant.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-192">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="f6cc8-193">Utilisez l’identificateur après le prérendu pour récupérer l’État mis en cache.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-193">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="f6cc8-194">Le code suivant illustre une `WeatherForecastService` mise à jour dans une application de serveur Blazor basée sur un modèle qui évite le double rendu :</span><span class="sxs-lookup"><span data-stu-id="f6cc8-194">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

<span data-ttu-id="f6cc8-195">Pour plus d’informations sur la `RenderMode`, consultez <xref:blazor/hosting-model-configuration#render-mode>.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-195">For more information on the `RenderMode`, see <xref:blazor/hosting-model-configuration#render-mode>.</span></span>

## <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="f6cc8-196">Détecter quand l’application est prérendu</span><span class="sxs-lookup"><span data-stu-id="f6cc8-196">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]
