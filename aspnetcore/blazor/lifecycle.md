---
title: ASP.NET Core Blazor cycle de vie
author: guardrex
description: Découvrez comment utiliser les méthodes de cycle de vie des composants Razor dans ASP.NET Core applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: df5bb676df59b538179a69978040521c4ee78ed1
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146366"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="35650-103">ASP.NET Core Blazor cycle de vie</span><span class="sxs-lookup"><span data-stu-id="35650-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="35650-104">Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="35650-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="35650-105">L’infrastructure Blazor comprend des méthodes de cycle de vie synchrones et asynchrones.</span><span class="sxs-lookup"><span data-stu-id="35650-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="35650-106">Substituez les méthodes de cycle de vie pour effectuer des opérations supplémentaires sur les composants lors de l’initialisation et du rendu des composants.</span><span class="sxs-lookup"><span data-stu-id="35650-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="35650-107">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="35650-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="35650-108">Méthodes d’initialisation de composant</span><span class="sxs-lookup"><span data-stu-id="35650-108">Component initialization methods</span></span>

<span data-ttu-id="35650-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> sont appelés lorsque le composant est initialisé après avoir reçu ses paramètres initiaux de son composant parent.</span><span class="sxs-lookup"><span data-stu-id="35650-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="35650-110">Utilisez `OnInitializedAsync` lorsque le composant exécute une opération asynchrone et doit être actualisé à la fin de l’opération.</span><span class="sxs-lookup"><span data-stu-id="35650-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span> <span data-ttu-id="35650-111">Ces méthodes sont appelées une seule fois lors de la première instanciation du composant.</span><span class="sxs-lookup"><span data-stu-id="35650-111">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="35650-112">Pour une opération synchrone, remplacez `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="35650-112">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="35650-113">Pour effectuer une opération asynchrone, substituez `OnInitializedAsync` et utilisez le mot clé `await` sur l’opération :</span><span class="sxs-lookup"><span data-stu-id="35650-113">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="35650-114">Les paramètres Before sont définis</span><span class="sxs-lookup"><span data-stu-id="35650-114">Before parameters are set</span></span>

<span data-ttu-id="35650-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> définit les paramètres fournis par le parent du composant dans l’arborescence de rendu :</span><span class="sxs-lookup"><span data-stu-id="35650-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="35650-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contient le jeu entier de valeurs de paramètre chaque fois que `SetParametersAsync` est appelé.</span><span class="sxs-lookup"><span data-stu-id="35650-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="35650-117">L’implémentation par défaut de `SetParametersAsync` définit la valeur de chaque propriété avec l’attribut `[Parameter]` ou `[CascadingParameter]` qui a une valeur correspondante dans le `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="35650-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="35650-118">Les paramètres qui n’ont pas de valeur correspondante dans `ParameterView` restent inchangés.</span><span class="sxs-lookup"><span data-stu-id="35650-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="35650-119">Si `base.SetParametersAync` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants de la façon requise.</span><span class="sxs-lookup"><span data-stu-id="35650-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="35650-120">Par exemple, il n’est pas nécessaire d’assigner les paramètres entrants aux propriétés de la classe.</span><span class="sxs-lookup"><span data-stu-id="35650-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="35650-121">Une fois les paramètres définis</span><span class="sxs-lookup"><span data-stu-id="35650-121">After parameters are set</span></span>

<span data-ttu-id="35650-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> sont appelées :</span><span class="sxs-lookup"><span data-stu-id="35650-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="35650-123">Lorsque le composant est initialisé et a reçu son premier jeu de paramètres de son composant parent.</span><span class="sxs-lookup"><span data-stu-id="35650-123">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="35650-124">Lorsque le composant parent est à nouveau rendu et fournit :</span><span class="sxs-lookup"><span data-stu-id="35650-124">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="35650-125">Seuls les types immuables primitifs connus dont au moins un paramètre a été modifié.</span><span class="sxs-lookup"><span data-stu-id="35650-125">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="35650-126">Tous les paramètres de type complexe.</span><span class="sxs-lookup"><span data-stu-id="35650-126">Any complex-typed parameters.</span></span> <span data-ttu-id="35650-127">L’infrastructure ne peut pas savoir si les valeurs d’un paramètre de type complexe ont muté en interne, donc elle traite le jeu de paramètres comme étant modifié.</span><span class="sxs-lookup"><span data-stu-id="35650-127">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="35650-128">Un travail asynchrone lors de l’application de paramètres et de valeurs de propriété doit se produire pendant l’événement de cycle de vie `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="35650-128">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="35650-129">Après le rendu du composant</span><span class="sxs-lookup"><span data-stu-id="35650-129">After component render</span></span>

<span data-ttu-id="35650-130"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> sont appelées après la fin du rendu d’un composant.</span><span class="sxs-lookup"><span data-stu-id="35650-130"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="35650-131">Les références d’élément et de composant sont remplies à ce stade.</span><span class="sxs-lookup"><span data-stu-id="35650-131">Element and component references are populated at this point.</span></span> <span data-ttu-id="35650-132">Utilisez cette étape pour effectuer des étapes d’initialisation supplémentaires à l’aide du contenu rendu, par exemple l’activation de bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.</span><span class="sxs-lookup"><span data-stu-id="35650-132">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="35650-133">Le paramètre `firstRender` pour `OnAfterRenderAsync` et `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="35650-133">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="35650-134">A la valeur `true` la première fois que l’instance de composant est restituée.</span><span class="sxs-lookup"><span data-stu-id="35650-134">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="35650-135">Peut être utilisé pour s’assurer que le travail d’initialisation n’est effectué qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="35650-135">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="35650-136">Le travail asynchrone immédiatement après le rendu doit se produire pendant l’événement de cycle de vie `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="35650-136">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="35650-137">Même si vous retournez un <xref:System.Threading.Tasks.Task> à partir d' `OnAfterRenderAsync`, l’infrastructure ne planifie pas un cycle de rendu supplémentaire pour votre composant une fois cette tâche terminée.</span><span class="sxs-lookup"><span data-stu-id="35650-137">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="35650-138">Cela permet d’éviter une boucle de rendu infinie.</span><span class="sxs-lookup"><span data-stu-id="35650-138">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="35650-139">Elle est différente des autres méthodes de cycle de vie, qui planifient un cycle de rendu supplémentaire une fois que la tâche retournée est terminée.</span><span class="sxs-lookup"><span data-stu-id="35650-139">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="35650-140">`OnAfterRender` et `OnAfterRenderAsync` *ne sont pas appelées lors du prérendu sur le serveur.*</span><span class="sxs-lookup"><span data-stu-id="35650-140">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="35650-141">Supprimer l’actualisation de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="35650-141">Suppress UI refreshing</span></span>

<span data-ttu-id="35650-142">Substituez <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> pour supprimer l’actualisation de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="35650-142">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="35650-143">Si l’implémentation retourne `true`, l’interface utilisateur est actualisée :</span><span class="sxs-lookup"><span data-stu-id="35650-143">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="35650-144">`ShouldRender` est appelée chaque fois que le composant est restitué.</span><span class="sxs-lookup"><span data-stu-id="35650-144">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="35650-145">Même si `ShouldRender` est substitué, le composant est toujours restitué initialement.</span><span class="sxs-lookup"><span data-stu-id="35650-145">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="35650-146">Modifications d'état</span><span class="sxs-lookup"><span data-stu-id="35650-146">State changes</span></span>

<span data-ttu-id="35650-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifie le composant que son état a changé.</span><span class="sxs-lookup"><span data-stu-id="35650-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="35650-148">Le cas échéant, l’appel de `StateHasChanged` entraîne le rerendu du composant.</span><span class="sxs-lookup"><span data-stu-id="35650-148">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="35650-149">Gérer les actions asynchrones incomplètes au rendu</span><span class="sxs-lookup"><span data-stu-id="35650-149">Handle incomplete async actions at render</span></span>

<span data-ttu-id="35650-150">Les actions asynchrones exécutées dans des événements de cycle de vie peuvent ne pas être terminées avant le rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="35650-150">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="35650-151">Les objets peuvent être `null` ou remplis de façon incomplète avec des données pendant l’exécution de la méthode Lifecycle.</span><span class="sxs-lookup"><span data-stu-id="35650-151">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="35650-152">Fournissez une logique de rendu pour confirmer que les objets sont initialisés.</span><span class="sxs-lookup"><span data-stu-id="35650-152">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="35650-153">Affichez les éléments d’interface utilisateur d’espace réservé (par exemple, un message de chargement) pendant que les objets sont `null`.</span><span class="sxs-lookup"><span data-stu-id="35650-153">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="35650-154">Dans le composant `FetchData` des modèles Blazor, `OnInitializedAsync` est remplacé par asynchrone recevoir les données de prévision (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="35650-154">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="35650-155">Lorsque `forecasts` est `null`, un message de chargement s’affiche pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="35650-155">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="35650-156">Une fois la `Task` retournée par `OnInitializedAsync` terminée, le composant est rerendu avec l’État mis à jour.</span><span class="sxs-lookup"><span data-stu-id="35650-156">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="35650-157">*Pages/fetchData. Razor* dans le modèle Blazor Server :</span><span class="sxs-lookup"><span data-stu-id="35650-157">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="35650-158">Suppression de composant avec IDisposable</span><span class="sxs-lookup"><span data-stu-id="35650-158">Component disposal with IDisposable</span></span>

<span data-ttu-id="35650-159">Si un composant implémente <xref:System.IDisposable>, la [méthode dispose](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="35650-159">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="35650-160">Le composant suivant utilise `@implements IDisposable` et la méthode `Dispose` :</span><span class="sxs-lookup"><span data-stu-id="35650-160">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="35650-161">L’appel de <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> dans `Dispose` n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="35650-161">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="35650-162">`StateHasChanged` peut être appelé dans le cadre de la suppression du convertisseur, il n’est donc pas possible de demander des mises à jour de l’interface utilisateur à ce stade.</span><span class="sxs-lookup"><span data-stu-id="35650-162">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="35650-163">Gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="35650-163">Handle errors</span></span>

<span data-ttu-id="35650-164">Pour plus d’informations sur la gestion des erreurs lors de l’exécution de la méthode de cycle de vie, consultez <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="35650-164">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
