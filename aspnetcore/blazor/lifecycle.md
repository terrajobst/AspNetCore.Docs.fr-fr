---
title: ASP.NET Core Blazor cycle de vie
author: guardrex
description: Découvrez comment utiliser les méthodes de cycle de vie des composants Razor dans ASP.NET Core applications Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: 280ea832f492852e425e3e15c61cac54fd1e39d6
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879668"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>ASP.NET Core Blazor cycle de vie

Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)

L’infrastructure Blazor comprend des méthodes de cycle de vie synchrones et asynchrones. Substituez les méthodes de cycle de vie pour effectuer des opérations supplémentaires sur les composants lors de l’initialisation et du rendu des composants.

## <a name="lifecycle-methods"></a>Méthodes de cycle de vie

### <a name="component-initialization-methods"></a>Méthodes d’initialisation de composant

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> exécuter du code qui initialise un composant. Ces méthodes sont appelées une seule fois lors de la première instanciation du composant.

Pour effectuer une opération asynchrone, utilisez `OnInitializedAsync` et le mot clé `await` sur l’opération :

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> Un travail asynchrone pendant l’initialisation d’un composant doit se produire pendant l’événement de cycle de vie `OnInitializedAsync`.

Pour une opération synchrone, utilisez `OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a>Les paramètres Before sont définis

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> définit les paramètres fournis par le parent du composant dans l’arborescence de rendu :

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView> contient le jeu entier de valeurs de paramètre chaque fois que `SetParametersAsync` est appelé.

L’implémentation par défaut de `SetParametersAsync` définit la valeur de chaque propriété avec l’attribut `[Parameter]` ou `[CascadingParameter]` qui a une valeur correspondante dans le `ParameterView`. Les paramètres qui n’ont pas de valeur correspondante dans `ParameterView` restent inchangés.

Si `base.SetParametersAync` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants de la façon requise. Par exemple, il n’est pas nécessaire d’assigner les paramètres entrants aux propriétés de la classe.

### <a name="after-parameters-are-set"></a>Une fois les paramètres définis

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> sont appelées lorsqu’un composant a reçu des paramètres de son parent et que les valeurs sont assignées aux propriétés. Ces méthodes sont exécutées après l’initialisation du composant et chaque fois que de nouvelles valeurs de paramètres sont spécifiées :

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> Un travail asynchrone lors de l’application de paramètres et de valeurs de propriété doit se produire pendant l’événement de cycle de vie `OnParametersSetAsync`.

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a>Après le rendu du composant

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> et <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> sont appelées après la fin du rendu d’un composant. Les références d’élément et de composant sont remplies à ce stade. Utilisez cette étape pour effectuer des étapes d’initialisation supplémentaires à l’aide du contenu rendu, par exemple l’activation de bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.

Le paramètre `firstRender` pour `OnAfterRenderAsync` et `OnAfterRender`:

* A la valeur `true` la première fois que l’instance de composant est restituée.
* Peut être utilisé pour s’assurer que le travail d’initialisation n’est effectué qu’une seule fois.

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
> Le travail asynchrone immédiatement après le rendu doit se produire pendant l’événement de cycle de vie `OnAfterRenderAsync`.
>
> Même si vous retournez un <xref:System.Threading.Tasks.Task> à partir d' `OnAfterRenderAsync`, l’infrastructure ne planifie pas un cycle de rendu supplémentaire pour votre composant une fois cette tâche terminée. Cela permet d’éviter une boucle de rendu infinie. Elle est différente des autres méthodes de cycle de vie, qui planifient un cycle de rendu supplémentaire une fois que la tâche retournée est terminée.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

`OnAfterRender` et `OnAfterRenderAsync` *ne sont pas appelées lors du prérendu sur le serveur.*

### <a name="suppress-ui-refreshing"></a>Supprimer l’actualisation de l’interface utilisateur

Substituez <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> pour supprimer l’actualisation de l’interface utilisateur. Si l’implémentation retourne `true`, l’interface utilisateur est actualisée :

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

`ShouldRender` est appelée chaque fois que le composant est restitué.

Même si `ShouldRender` est substitué, le composant est toujours restitué initialement.

## <a name="state-changes"></a>Modification de l'état

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifie le composant que son état a changé. Le cas échéant, l’appel de `StateHasChanged` entraîne le rerendu du composant.

## <a name="handle-incomplete-async-actions-at-render"></a>Gérer les actions asynchrones incomplètes au rendu

Les actions asynchrones exécutées dans des événements de cycle de vie peuvent ne pas être terminées avant le rendu du composant. Les objets peuvent être `null` ou remplis de façon incomplète avec des données pendant l’exécution de la méthode Lifecycle. Fournissez une logique de rendu pour confirmer que les objets sont initialisés. Affichez les éléments d’interface utilisateur d’espace réservé (par exemple, un message de chargement) pendant que les objets sont `null`.

Dans le composant `FetchData` des modèles Blazor, `OnInitializedAsync` est remplacé par asynchrone recevoir les données de prévision (`forecasts`). Lorsque `forecasts` est `null`, un message de chargement s’affiche pour l’utilisateur. Une fois la `Task` retournée par `OnInitializedAsync` terminée, le composant est rerendu avec l’État mis à jour.

*Pages/fetchData. Razor* dans le modèle Blazor Server :

[!code-cshtml[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>Suppression de composant avec IDisposable

Si un composant implémente <xref:System.IDisposable>, la [méthode dispose](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur. Le composant suivant utilise `@implements IDisposable` et la méthode `Dispose` :

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

> [!NOTE]
> L’appel de <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> dans `Dispose` n’est pas pris en charge. `StateHasChanged` peut être appelé dans le cadre de la suppression du convertisseur, il n’est donc pas possible de demander des mises à jour de l’interface utilisateur à ce stade.

## <a name="handle-errors"></a>Gérer les erreurs

Pour plus d’informations sur la gestion des erreurs lors de l’exécution de la méthode de cycle de vie, consultez <xref:blazor/handle-errors#lifecycle-methods>.
