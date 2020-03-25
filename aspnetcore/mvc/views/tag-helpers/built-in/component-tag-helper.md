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
# <a name="component-tag-helper-in-aspnet-core"></a>Tag Helper Component dans ASP.NET Core

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

Pour afficher un composant à partir d’une page ou d’une vue, utilisez le [tag Helper Component](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).

Le tag Helper Component suivant restitue le composant `Counter` dans une page ou une vue :

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

Le tag Helper Component peut également transmettre des paramètres à des composants. Prenons le composant `ColorfulCheckbox` suivant qui définit la couleur et la taille de l’étiquette de case à cocher :

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

Les [paramètres de composant](xref:blazor/components#component-parameters) `Size` (`int`) et `Color` (`string`) peuvent être définis par le tag Helper du composant :

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

Le code HTML suivant est affiché dans la page ou la vue :

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

Le passage d’une chaîne entre guillemets requiert une [expression Razor explicite](xref:mvc/views/razor#explicit-razor-expressions), comme indiqué pour `param-Color` dans l’exemple précédent. Le comportement d’analyse Razor pour une valeur de type `string` ne s’applique pas à un attribut `param-*`, car l’attribut est un type `object`.

Le type de paramètre doit être sérialisable JSON, ce qui signifie généralement que le type doit avoir un constructeur par défaut et des propriétés définissables. Par exemple, vous pouvez spécifier une valeur pour `Size` et `Color` dans l’exemple précédent, car les types de `Size` et `Color` sont des types primitifs (`int` et `string`), qui sont pris en charge par le sérialiseur JSON.

<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configure si le composant :

* Est prérendu dans la page.
* Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.

| Mode de rendu | Description |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Génère le rendu du composant en HTML statique et comprend un marqueur pour une application Blazor Server. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Restitue un marqueur pour une application Blazor Server. La sortie du composant n’est pas incluse. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application Blazor. |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Génère le rendu du composant en HTML statique. |

Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie. Les composants ne peuvent pas utiliser les fonctionnalités spécifiques aux vues et aux pages, telles que les vues partielles et les sections. Pour utiliser la logique d’une vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.

Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
