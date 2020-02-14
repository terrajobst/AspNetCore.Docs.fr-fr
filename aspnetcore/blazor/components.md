---
title: Créer et utiliser des composants ASP.NET Core Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants Razor, notamment comment lier des données, gérer des événements et gérer des cycles de vie de composant.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: 0da0d83a4fde7b753a84bf05d3a9284776f2881f
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213348"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Créer et utiliser des composants ASP.NET Core Razor

Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Les applications éblouissantes sont créées à l’aide de *composants*. Un composant est un bloc d’interface utilisateur (IU) autonome, tel qu’une page, une boîte de dialogue ou un formulaire. Un composant comprend le balisage HTML et la logique de traitement requise pour injecter des données ou répondre à des événements d’interface utilisateur. Les composants sont flexibles et légers. Elles peuvent être imbriquées, réutilisées et partagées entre les projets.

## <a name="component-classes"></a>Classes de composant

Les composants sont implémentés dans les fichiers de composants [Razor](xref:mvc/views/razor) ( *. Razor*) C# à l’aide d’une combinaison de balises et html. Un composant de éblouissant est officiellement désigné sous le terme de *composant Razor*.

Le nom d’un composant doit commencer par un caractère majuscule. Par exemple, *MyCoolComponent. Razor* est valide et *MyCoolComponent. Razor* n’est pas valide.

L’interface utilisateur d’un composant est définie à l’aide de HTML. La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor). Quand une application est compilée, le balisage C# html et la logique de rendu sont convertis en une classe de composant. Le nom de la classe générée correspond au nom du fichier.

Les membres de la classe de composants sont définis dans un bloc `@code`. Dans le bloc `@code`, l’état du composant (propriétés, champs) est spécifié avec les méthodes de gestion des événements ou pour la définition d’une autre logique de composant. Plus d’un bloc `@code` est autorisé.

Les membres de composant peuvent être utilisés dans le cadre de la logique de C# rendu du composant à l’aide d’expressions qui commencent par `@`. Par exemple, un C# champ est rendu en préfixant `@` au nom du champ. L’exemple suivant évalue et affiche :

* `_headingFontStyle` à la valeur de propriété CSS pour `font-style`.
* `_headingText` au contenu de l’élément `<h1>`.

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Une fois le composant restitué initialement, le composant régénère son arborescence de rendu en réponse aux événements. Il compare ensuite la nouvelle arborescence de rendu à la précédente et applique toutes les modifications à la Document Object Model du navigateur (DOM).

Les composants sont C# des classes ordinaires qui peuvent être placées n’importe où dans un projet. Les composants qui produisent des pages Web résident généralement dans le dossier *pages* . Les composants qui ne sont pas des pages sont souvent placés dans le dossier *partagé* ou dans un dossier personnalisé ajouté au projet.

En règle générale, l’espace de noms d’un composant est dérivé de l’espace de noms racine de l’application et de l’emplacement (dossier) du composant dans l’application. Si l’espace de noms racine de l’application est `BlazorApp` et que le composant `Counter` se trouve dans le dossier *pages* :

* L’espace de noms du composant `Counter` est `BlazorApp.Pages`.
* Le nom de type qualifié complet du composant est `BlazorApp.Pages.Counter`.

Pour plus d’informations, consultez la section [importer des composants](#import-components) .

Pour utiliser un dossier personnalisé, ajoutez l’espace de noms du dossier personnalisé au composant parent ou au fichier *_Imports. Razor* de l’application. Par exemple, l’espace de noms suivant rend les composants dans un dossier de *composants* disponibles lorsque l’espace de noms racine de l’application est `BlazorApp`:

```razor
@using BlazorApp.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Intégrer des composants dans des applications Razor Pages et MVC

Les composants Razor peuvent être intégrés dans des applications Razor Pages et MVC. Lorsque la page ou la vue est restituée, les composants peuvent être prérendus en même temps.

Pour préparer une Razor Pages ou une application MVC pour héberger des composants Razor, suivez les instructions de la section *intégrer des composants Razor dans des applications Razor pages et MVC* de l’article <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps>.

Lorsque vous utilisez un dossier personnalisé pour stocker les composants de l’application, ajoutez l’espace de noms qui représente le dossier à la page/la vue ou au fichier *_ViewImports. cshtml* . Dans l’exemple suivant :

* Remplacez `MyAppNamespace` par l’espace de noms de l’application.
* Si un dossier nommé *Components* n’est pas utilisé pour contenir les composants, remplacez `Components` par le dossier dans lequel se trouvent les composants.

```csharp
@using MyAppNamespace.Components
```

Le fichier *_ViewImports. cshtml* se trouve dans le dossier *pages* d’une application Razor pages ou du dossier *views* d’une application MVC.

Pour afficher un composant à partir d’une page ou d’une vue, utilisez le tag Helper `Component` :

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Le type de paramètre doit être sérialisable JSON, ce qui signifie généralement que le type doit avoir un constructeur par défaut et des propriétés définissables. Par exemple, vous pouvez spécifier une valeur pour `IncrementAmount`, car le type de `IncrementAmount` est un `int`, qui est un type primitif pris en charge par le sérialiseur JSON.

`RenderMode` configure si le composant :

* Est prérendu dans la page.
* Est rendu en HTML statique sur la page ou s’il contient les informations nécessaires pour démarrer une application éblouissant à partir de l’agent utilisateur.

| `RenderMode`        | Description |
| ------------------- | ----------- |
| `ServerPrerendered` | Génère le rendu du composant en HTML statique et comprend un marqueur pour une application de serveur éblouissante. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application éblouissante. |
| `Server`            | Restitue un marqueur pour une application de serveur éblouissante. La sortie du composant n’est pas incluse. Au démarrage de l’agent utilisateur, ce marqueur est utilisé pour démarrer une application éblouissante. |
| `Static`            | Génère le rendu du composant en HTML statique. |

Alors que les pages et les vues peuvent utiliser des composants, la réciproque n’est pas vraie. Les composants ne peuvent pas utiliser les scénarios spécifiques aux vues et aux pages, tels que les vues partielles et les sections. Pour utiliser la logique de la vue partielle dans un composant, factorisez la logique de la vue partielle dans un composant.

Le rendu des composants serveur à partir d’une page HTML statique n’est pas pris en charge.

Pour plus d’informations sur la façon dont les composants sont rendus, l’état des composants et le tag Helper `Component`, consultez les articles suivants :

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>

## <a name="tag-helpers-arent-used-in-components"></a>Les tag helpers ne sont pas utilisés dans les composants

Les [tag helpers](xref:mvc/views/tag-helpers/intro) ne sont pas pris en charge dans les composants Razor (fichiers *. Razor* ). Pour fournir des fonctionnalités semblables à tag Helper dans éblouissant, créez un composant avec les mêmes fonctionnalités que le tag Helper et utilisez le composant à la place.

## <a name="use-components"></a>Utiliser des composants

Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML. Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.

La liaison d’attribut respecte la casse. Par exemple, `@bind` est valide et `@Bind` n’est pas valide.

Le balisage suivant dans *index. Razor* rend une instance de `HeadingComponent` :

```razor
<HeadingComponent />
```

*Composants/HeadingComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

Si un composant contient un élément HTML avec une première lettre majuscule qui ne correspond pas à un nom de composant, un avertissement est émis pour indiquer que l’élément a un nom inattendu. L’ajout d’une instruction `@using` pour l’espace de noms du composant rend le composant disponible, ce qui supprime l’avertissement.

## <a name="component-parameters"></a>Paramètres de composant

Les composants peuvent avoir des *paramètres de composant*, qui sont définis à l’aide de propriétés publiques sur la classe de composant avec l’attribut `[Parameter]`. Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.

*Composants/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

Dans l’exemple suivant tiré de l’exemple d’application, le `ParentComponent` définit la valeur de la propriété `Title` du `ChildComponent`.

*Pages/ParentComponent. Razor*:

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="child-content"></a>Contenu enfant

Les composants peuvent définir le contenu d’un autre composant. Le composant d’affectation fournit le contenu entre les balises qui spécifient le composant récepteur.

Dans l’exemple suivant, la `ChildComponent` a une propriété `ChildContent` qui représente un `RenderFragment`, qui représente un segment de l’interface utilisateur à restituer. La valeur de `ChildContent` est positionnée dans le balisage du composant où le contenu doit être rendu. La valeur de `ChildContent` est reçue du composant parent et affichée à l’intérieur du `panel-body`du panneau de démarrage.

*Composants/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> La propriété qui reçoit le contenu du `RenderFragment` doit être nommée `ChildContent` par Convention.

Les `ParentComponent` dans l’exemple d’application peuvent fournir du contenu pour le rendu du `ChildComponent` en plaçant le contenu à l’intérieur des balises de `<ChildComponent>`.

*Pages/ParentComponent. Razor*:

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Réprojection d’attribut et paramètres arbitraires

Les composants peuvent capturer et restituer des attributs supplémentaires en plus des paramètres déclarés du composant. Des attributs supplémentaires peuvent être capturés dans un dictionnaire *, puis* réintégrés à un élément lorsque le composant est rendu à l’aide de la directive [`@attributes`](xref:mvc/views/razor#attributes) Razor. Ce scénario est utile lors de la définition d’un composant qui produit un élément de balisage qui prend en charge diverses personnalisations. Par exemple, il peut être fastidieux de définir des attributs séparément pour un `<input>` qui prend en charge de nombreux paramètres.

Dans l’exemple suivant, le premier élément `<input>` (`id="useIndividualParams"`) utilise des paramètres de composant individuels, tandis que le deuxième élément `<input>` (`id="useAttributesDict"`) utilise la projection d’attributs :

```razor
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

Le type du paramètre doit implémenter `IEnumerable<KeyValuePair<string, object>>` avec des clés de chaîne. L’utilisation de `IReadOnlyDictionary<string, object>` est également une option dans ce scénario.

Les éléments `<input>` rendus à l’aide des deux approches sont identiques :

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

Pour accepter des attributs arbitraires, définissez un paramètre de composant à l’aide de l’attribut `[Parameter]` avec la propriété `CaptureUnmatchedValues` définie sur `true`:

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

La propriété `CaptureUnmatchedValues` sur `[Parameter]` permet au paramètre de correspondre à tous les attributs qui ne correspondent à aucun autre paramètre. Un composant ne peut définir qu’un seul paramètre avec `CaptureUnmatchedValues`. Le type de propriété utilisé avec `CaptureUnmatchedValues` doit pouvoir être assigné à partir de `Dictionary<string, object>` avec des clés de chaîne. `IEnumerable<KeyValuePair<string, object>>` ou `IReadOnlyDictionary<string, object>` sont également des options dans ce scénario.

La position des `@attributes` par rapport à la position des attributs d’élément est importante. Lorsque `@attributes` sont réparties sur l’élément, les attributs sont traités de droite à gauche (dernier à premier). Prenons l’exemple suivant d’un composant qui consomme un composant `Child` :

*ParentComponent. Razor*:

```razor
<ChildComponent extra="10" />
```

*ChildComponent. Razor*:

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

L’attribut `extra` du composant `Child` est défini à droite de `@attributes`. Le `<div>` rendu du composant `Parent` contient `extra="5"` lorsqu’il est transmis via l’attribut supplémentaire, car les attributs sont traités de droite à gauche (dernier à premier) :

```html
<div extra="5" />
```

Dans l’exemple suivant, l’ordre des `extra` et `@attributes` est inversé dans le `<div>`du composant `Child` :

*ParentComponent. Razor*:

```razor
<ChildComponent extra="10" />
```

*ChildComponent. Razor*:

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

Le `<div>` rendu dans le composant `Parent` contient `extra="10"` lorsqu’il est passé par le biais de l’attribut supplémentaire :

```html
<div extra="10" />
```

## <a name="data-binding"></a>Liaison de données

La liaison de données aux composants et aux éléments DOM s’effectue à l’aide de l’attribut [`@bind`](xref:mvc/views/razor#bind) . L’exemple suivant lie une propriété `CurrentValue` à la valeur de la zone de texte :

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

Lorsque la zone de texte perd le focus, la valeur de la propriété est mise à jour.

La zone de texte est mise à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, et non en réponse à la modification de la valeur de la propriété. Étant donné que les composants s’affichent après l’exécution du code du gestionnaire d’événements, les mises à jour de propriétés sont *généralement* reflétées dans l’interface utilisateur immédiatement après le déclenchement d’un gestionnaire d’événements.

L’utilisation de `@bind` avec la propriété `CurrentValue` (`<input @bind="CurrentValue" />`) équivaut essentiellement à ce qui suit :

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

Lors du rendu du composant, le `value` de l’élément d’entrée provient de la propriété `CurrentValue`. Lorsque l’utilisateur tape dans la zone de texte et modifie le focus de l’élément, l’événement `onchange` est déclenché et la propriété `CurrentValue` est définie sur la valeur modifiée. En réalité, la génération de code est plus complexe, car `@bind` gère les cas où les conversions de types sont effectuées. En principe, `@bind` associe la valeur actuelle d’une expression à un attribut `value` et gère les modifications à l’aide du gestionnaire inscrit.

En plus de gérer les événements `onchange` avec la syntaxe `@bind`, une propriété ou un champ peut être lié à l’aide d’autres événements en spécifiant un attribut [`@bind-value`](xref:mvc/views/razor#bind) avec un paramètre `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)). L’exemple suivant lie la propriété `CurrentValue` pour l’événement `oninput` :

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

Contrairement à `onchange`, qui se déclenche lorsque l’élément perd le focus, `oninput` se déclenche lorsque la valeur de la zone de texte change.

`@bind-value` dans l’exemple précédent lie :

* Expression (`CurrentValue`) spécifiée à l’attribut `value` de l’élément.
* Délégué d’événement de modification à l’événement spécifié par `@bind-value:event`.

### <a name="unparsable-values"></a>Valeurs inanalysables

Quand un utilisateur fournit une valeur non analysable à un élément DataBound, la valeur unanalysable est automatiquement rétablie à sa valeur précédente lorsque l’événement de liaison est déclenché.

Examinez le cas suivant :

* Un élément `<input>` est lié à un type `int` avec une valeur initiale de `123`:

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* L’utilisateur met à jour la valeur de l’élément à `123.45` dans la page et modifie le focus de l’élément.

Dans le scénario précédent, la valeur de l’élément est rétablie à `123`. Lorsque la valeur `123.45` est rejetée en faveur de la valeur d’origine de `123`, l’utilisateur sait que sa valeur n’a pas été acceptée.

Par défaut, la liaison s’applique à l’événement `onchange` de l’élément (`@bind="{PROPERTY OR FIELD}"`). Utilisez `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` pour définir un événement différent. Pour l’événement `oninput` (`@bind-value:event="oninput"`), la Reversion se produit après toute séquence de touches qui introduit une valeur non analysable. Quand vous ciblez l’événement `oninput` avec un type lié `int`, un utilisateur ne peut pas taper un caractère `.`. Si un caractère de `.` est immédiatement supprimé, l’utilisateur reçoit des commentaires immédiats que seuls des nombres entiers sont autorisés. Il existe des scénarios où le rétablissement de la valeur sur l’événement `oninput` n’est pas idéal, par exemple lorsque l’utilisateur doit être autorisé à effacer une valeur de `<input>` non analysable. Les alternatives sont les suivantes :

* N’utilisez pas l’événement `oninput`. Utilisez l’événement `onchange` par défaut (`@bind="{PROPERTY OR FIELD}"`), où une valeur non valide n’est pas rétablie tant que l’élément n’a pas perdu le focus.
* Effectuer une liaison à un type Nullable, tel que `int?` ou `string`, et fournir une logique personnalisée pour gérer les entrées non valides.
* Utilisez un [composant de validation de formulaire](xref:blazor/forms-validation), tel que `InputNumber` ou `InputDate`. Les composants de validation de formulaire offrent une prise en charge intégrée pour gérer les entrées non valides. Composants de validation de formulaire :
  * Permet à l’utilisateur de fournir une entrée non valide et de recevoir des erreurs de validation sur les `EditContext`associées.
  * Affichez les erreurs de validation dans l’interface utilisateur sans interférer avec l’utilisateur qui saisit des données Webform supplémentaires.

### <a name="globalization"></a>Globalisation

`@bind` valeurs sont mises en forme pour l’affichage et l’analyse à l’aide des règles de la culture actuelle.

La culture actuelle est accessible à partir de la propriété <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) est utilisé pour les types de champ suivants (`<input type="{TYPE}" />`) :

* `date`
* `number`

Les types de champ précédents :

* Sont affichés à l’aide des règles de mise en forme appropriées basées sur le navigateur.
* Ne peut pas contenir de texte de forme libre.
* Fournir des caractéristiques d’interaction de l’utilisateur en fonction de l’implémentation du navigateur.

Les types de champs suivants ont des exigences de mise en forme spécifiques et ne sont pas actuellement pris en charge par éblouissant, car ils ne sont pas pris en charge par tous les principaux navigateurs :

* `datetime-local`
* `month`
* `week`

`@bind` prend en charge le paramètre `@bind:culture` pour fournir une <xref:System.Globalization.CultureInfo?displayProperty=fullName> pour l’analyse et la mise en forme d’une valeur. La spécification d’une culture n’est pas recommandée lors de l’utilisation des types de champ `date` et `number`. `date` et `number` disposent d’une prise en charge intégrée de éblouissant qui fournit la culture requise.

Pour plus d’informations sur la façon de définir la culture de l’utilisateur, consultez la section [Localization](#localization) .

### <a name="format-strings"></a>Chaînes de format

La liaison de données fonctionne avec les chaînes de format <xref:System.DateTime> à l’aide de [`@bind:format`](xref:mvc/views/razor#bind). D’autres expressions de mise en forme, telles que les formats de devise ou de nombre, ne sont pas disponibles pour l’instant.

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

Dans le code précédent, le type de champ de l’élément `<input>` (`type`) a par défaut la valeur `text`. `@bind:format` est pris en charge pour la liaison des types .NET suivants :

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

L’attribut `@bind:format` spécifie le format de date à appliquer aux `value` de l’élément `<input>`. Le format est également utilisé pour analyser la valeur lorsqu’un événement `onchange` se produit.

Il n’est pas recommandé de spécifier un format pour le type de champ `date`, car éblouissant offre une prise en charge intégrée de la mise en forme des dates. Malgré la recommandation, utilisez uniquement le format de date `yyyy-MM-dd` pour que la liaison fonctionne correctement si un format est fourni avec le type de champ `date` :

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

### <a name="parent-to-child-binding-with-component-parameters"></a>Liaison parent-enfant avec les paramètres de composant

La liaison reconnaît les paramètres du composant, où `@bind-{property}` pouvez lier une valeur de propriété d’un composant parent à un composant enfant. La liaison d’un enfant à un parent est couverte dans la [liaison enfant-parent avec la section de liaison chaînée](#child-to-parent-binding-with-chained-bind) .

Le composant enfant suivant (`ChildComponent`) a un paramètre de composant `Year` et `YearChanged` rappel :

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

`EventCallback<T>` est expliqué dans la section [EventCallback suivante](#eventcallback) .

Le composant parent suivant utilise :

* `ChildComponent` et lie le paramètre `ParentYear` du parent au paramètre `Year` sur le composant enfant.
* L’événement `onclick` est utilisé pour déclencher la méthode `ChangeTheYear`. Pour plus d’informations, consultez la section [gestion des événements](#event-handling) .

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

Le chargement de l' `ParentComponent` produit le balisage suivant :

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Si la valeur de la propriété `ParentYear` est modifiée en sélectionnant le bouton dans la `ParentComponent`, la propriété `Year` de la `ChildComponent` est mise à jour. La nouvelle valeur de `Year` est rendue dans l’interface utilisateur lorsque le `ParentComponent` est restitué à nouveau :

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Le paramètre `Year` peut être lié, car il a un `YearChanged` événement associé qui correspond au type du paramètre `Year`.

Par Convention, `<ChildComponent @bind-Year="ParentYear" />` revient essentiellement à écrire :

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

En général, une propriété peut être liée à un gestionnaire d’événements correspondant à l’aide de `@bind-property:event` attribut. Par exemple, la propriété `MyProp` peut être liée à `MyEventHandler` à l’aide des deux attributs suivants :

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

### <a name="child-to-parent-binding-with-chained-bind"></a>Liaison enfant-parent avec liaison chaînée

Un scénario courant consiste à chaîner un paramètre lié aux données à un élément de page dans la sortie du composant. Ce scénario est appelé *liaison chaînée* car plusieurs niveaux de liaison se produisent simultanément.

Une liaison chaînée ne peut pas être implémentée avec `@bind` syntaxe dans l’élément de la page. Le gestionnaire d’événements et la valeur doivent être spécifiés séparément. Toutefois, un composant parent peut utiliser `@bind` syntaxe avec le paramètre du composant.

Le composant `PasswordField` suivant (*passwordField. Razor*) :

* Définit la valeur d’un élément `<input>` sur une propriété `Password`.
* Expose les modifications de la propriété `Password` à un composant parent avec un [EventCallback suivante](#eventcallback).
* Utilise l’événement `onclick` est utilisé pour déclencher la méthode `ToggleShowPassword`. Pour plus d’informations, consultez la section [gestion des événements](#event-handling) .

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

Le composant `PasswordField` est utilisé dans un autre composant :

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

Pour effectuer des vérifications ou des erreurs d’interruption sur le mot de passe dans l’exemple précédent :

* Créez un champ de stockage pour `Password` (`_password` dans l’exemple de code suivant).
* Effectuez les vérifications ou les erreurs d’interruption dans le `Password` Setter.

L’exemple suivant fournit un retour immédiat à l’utilisateur si un espace est utilisé dans la valeur du mot de passe :

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

### <a name="radio-buttons"></a>Cases d’option

Pour plus d’informations sur la liaison à des cases d’option dans un formulaire, consultez <xref:blazor/forms-validation#work-with-radio-buttons>.

## <a name="event-handling"></a>Gestion des événements

Les composants Razor fournissent des fonctionnalités de gestion des événements. Pour un attribut d’élément HTML nommé `on{EVENT}` (par exemple, `onclick` et `onsubmit`) avec une valeur de type délégué, les composants Razor traitent la valeur de l’attribut en tant que gestionnaire d’événements. Le nom de l’attribut est toujours mis en forme [`@on{EVENT}`](xref:mvc/views/razor#onevent).

Le code suivant appelle la méthode `UpdateHeading` lorsque le bouton est sélectionné dans l’interface utilisateur :

```razor
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

Le code suivant appelle la méthode `CheckChanged` lorsque la case à cocher est modifiée dans l’interface utilisateur :

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Les gestionnaires d’événements peuvent également être asynchrones et retourner un <xref:System.Threading.Tasks.Task>. Il n’est pas nécessaire d’appeler manuellement [StateHasChanged](xref:blazor/lifecycle#state-changes). Les exceptions sont journalisées lorsqu’elles se produisent.

Dans l’exemple suivant, `UpdateHeading` est appelé de façon asynchrone quand le bouton est sélectionné :

```razor
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

### <a name="event-argument-types"></a>Types d’arguments d’événement

Pour certains événements, les types d’arguments d’événement sont autorisés. Si l’accès à l’un de ces types d’événements n’est pas nécessaire, il n’est pas obligatoire dans l’appel de la méthode.

Les `EventArgs` prises en charge sont indiquées dans le tableau suivant.

| Événement            | Classe                | Remarques et événements DOM |
| ---------------- | -------------------- | -------------------- |
| Presse-papiers        | `ClipboardEventArgs` | `oncut`, `oncopy`, `onpaste` |
| Déplacez             | `DragEventArgs`      | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`<br><br>`DataTransfer` et `DataTransferItem` contiennent des données d’élément glissées. |
| Error            | `ErrorEventArgs`     | `onerror` |
| Événement            | `EventArgs`          | *Généralités*<br>`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`<br><br>*Presse-papiers*<br>`onbeforecut`, `onbeforecopy`, `onbeforepaste`<br><br>*Input*<br>`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`<br><br>*Média*<br>`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting` |
| Focus            | `FocusEventArgs`     | `onfocus`, `onblur`, `onfocusin`, `onfocusout`<br><br>N’inclut pas la prise en charge de `relatedTarget`. |
| Entrée            | `ChangeEventArgs`    | `onchange`, `oninput` |
| Clavier         | `KeyboardEventArgs`  | `onkeydown`, `onkeypress`, `onkeyup` |
| Souris            | `MouseEventArgs`     | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` |
| Pointeur de la souris    | `PointerEventArgs`   | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` |
| Roulette de la souris      | `WheelEventArgs`     | `onwheel`, `onmousewheel` |
| Progress         | `ProgressEventArgs`  | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` |
| Entrées tactiles            | `TouchEventArgs`     | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`<br><br>`TouchPoint` représente un point de contact unique sur un appareil tactile. |

Pour plus d’informations, consultez les ressources suivantes :

* [Classes EventArgs dans la source de référence ASP.net Core (branche dotnet/aspnetcore ou version 3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).
* [MDN Web docs : GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; contient des informations sur les éléments HTML qui prennent en charge chaque événement DOM.

### <a name="lambda-expressions"></a>Expressions lambda

Les expressions lambda peuvent également être utilisées :

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Il est souvent pratique de se rapprocher de valeurs supplémentaires, par exemple lors de l’itération sur un ensemble d’éléments. L’exemple suivant crée trois boutons, chacun d’entre eux appelant `UpdateHeading` passant un argument d’événement (`MouseEventArgs`) et son numéro de bouton (`buttonNumber`) lorsqu’il est sélectionné dans l’interface utilisateur :

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> N’utilisez **pas** la variable de boucle (`i`) dans une boucle `for` directement dans une expression lambda. Dans le cas contraire, la même variable est utilisée par toutes les expressions lambda provoquant la même valeur de `i`dans toutes les expressions lambda. Capturez toujours sa valeur dans une variable locale (`buttonNumber` dans l’exemple précédent), puis utilisez-la.

### <a name="eventcallback"></a>EventCallback suivante

Un scénario courant avec des composants imbriqués est le souhait d’exécuter la méthode d’un composant parent lorsqu’un événement de composant enfant se produit&mdash;par exemple, lorsqu’un événement `onclick` se produit dans l’enfant. Pour exposer des événements entre les composants, utilisez un `EventCallback`. Un composant parent peut affecter une méthode de rappel à l' `EventCallback`d’un composant enfant.

La `ChildComponent` dans l’exemple d’application (*Components/ChildComponent. Razor*) montre comment le gestionnaire de `onclick` d’un bouton est configuré pour recevoir un délégué `EventCallback` de l' `ParentComponent`de l’exemple. Le `EventCallback` est tapé avec `MouseEventArgs`, ce qui est approprié pour un événement `onclick` à partir d’un périphérique :

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

L' `ParentComponent` affecte à la `EventCallback<T>` (`OnClickCallback`) de l’enfant sa méthode `ShowMessage`.

*Pages/ParentComponent. Razor*:

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

Lorsque le bouton est sélectionné dans la `ChildComponent`:

* La méthode `ShowMessage` de `ParentComponent`est appelée. `_messageText` est mis à jour et affiché dans le `ParentComponent`.
* Un appel à [StateHasChanged](xref:blazor/lifecycle#state-changes) n’est pas requis dans la méthode du rappel (`ShowMessage`). `StateHasChanged` est appelé automatiquement pour rerestituer le `ParentComponent`, tout comme les événements enfants déclenchent le rerendu des composants dans les gestionnaires d’événements qui s’exécutent dans l’enfant.

`EventCallback` et `EventCallback<T>` autorisent les délégués asynchrones. `EventCallback<T>` est fortement typé et requiert un type d’argument spécifique. `EventCallback` est faiblement typé et autorise tout type d’argument.

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

Appelez une `EventCallback` ou `EventCallback<T>` avec `InvokeAsync` et attendez la <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Utilisez `EventCallback` et `EventCallback<T>` pour la gestion des événements et la liaison des paramètres du composant.

Préférez la `EventCallback<T>` fortement typée sur `EventCallback`. `EventCallback<T>` offre un meilleur retour d’erreur aux utilisateurs du composant. Comme pour d’autres gestionnaires d’événements d’interface utilisateur, la spécification du paramètre d’événement est facultative. Utilisez `EventCallback` quand aucune valeur n’est passée au rappel.

### <a name="prevent-default-actions"></a>Empêcher les actions par défaut

Utilisez l’attribut [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive pour empêcher l’action par défaut pour un événement.

Quand une clé est sélectionnée sur un appareil d’entrée et que le focus de l’élément se trouve sur une zone de texte, un navigateur affiche normalement le caractère de la clé dans la zone de texte. Dans l’exemple suivant, le comportement par défaut est évité en spécifiant l’attribut de directive `@onkeypress:preventDefault`. Le compteur est incrémenté, et la clé de **+** n’est pas capturée dans la valeur de l’élément `<input>` :

```razor
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

La spécification de l’attribut `@on{EVENT}:preventDefault` sans valeur est équivalente à `@on{EVENT}:preventDefault="true"`.

La valeur de l’attribut peut également être une expression. Dans l’exemple suivant, `_shouldPreventDefault` est un champ `bool` défini sur `true` ou `false`:

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

Un gestionnaire d’événements n’est pas nécessaire pour empêcher l’action par défaut. Le gestionnaire d’événements et les scénarios d’action par défaut peuvent être utilisés indépendamment.

### <a name="stop-event-propagation"></a>Arrêter la propagation des événements

Utilisez l’attribut [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive pour arrêter la propagation des événements.

Dans l’exemple suivant, la sélection de la case à cocher empêche les événements Click du deuxième `<div>` enfant de se propager vers le `<div>`parent :

```razor
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

## <a name="capture-references-to-components"></a>Capturer des références à des composants

Les références de composant offrent un moyen de référencer une instance de composant afin que vous puissiez émettre des commandes vers cette instance, telles que `Show` ou `Reset`. Pour capturer une référence de composant :

* Ajoutez un attribut [`@ref`](xref:mvc/views/razor#ref) au composant enfant.
* Définissez un champ avec le même type que le composant enfant.

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

Lors du rendu du composant, le champ `_loginDialog` est rempli avec l’instance du composant enfant `MyLoginDialog`. Vous pouvez ensuite appeler des méthodes .NET sur l’instance du composant.

> [!IMPORTANT]
> La variable `_loginDialog` n’est remplie qu’après le rendu du composant et sa sortie comprend l’élément `MyLoginDialog`. Jusqu’à ce stade, il n’y a rien à référencer. Pour manipuler des références de composants après la fin du rendu du composant, utilisez les [méthodes OnAfterRenderAsync ou OnAfterRender](xref:blazor/lifecycle#after-component-render).

Bien que la capture de références de composant utilise une syntaxe similaire pour [capturer des références d’élément](xref:blazor/javascript-interop#capture-references-to-elements), il ne s’agit pas d’une fonctionnalité [JavaScript Interop](xref:blazor/javascript-interop) . Les références de composants ne sont pas transmises au code JavaScript&mdash;elles sont utilisées uniquement dans le code .NET.

> [!NOTE]
> N’utilisez **pas** de références de composant pour muter l’état des composants enfants. Utilisez plutôt des paramètres déclaratifs normaux pour passer des données aux composants enfants. L’utilisation de paramètres déclaratifs normaux entraîne le rerendu automatique des composants enfants.

## <a name="invoke-component-methods-externally-to-update-state"></a>Appeler des méthodes de composant en externe pour mettre à jour l’État

Éblouissant utilise un `SynchronizationContext` pour appliquer un seul thread logique d’exécution. Les méthodes de [cycle de vie](xref:blazor/lifecycle) d’un composant et les rappels d’événements déclenchés par éblouissant sont exécutés sur cette `SynchronizationContext`. Dans le cas où un composant doit être mis à jour en fonction d’un événement externe, tel qu’un minuteur ou d’autres notifications, utilisez la méthode `InvokeAsync`, qui sera réexpédiée à l' `SynchronizationContext`de l’éblouissant.

Par exemple, considérez un *service de notification* qui peut notifier n’importe quel composant d’écoute de l’État mis à jour :

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

Inscrivez le `NotifierService` en tant que singletion :

* Dans le webassembly éblouissant, inscrivez le service dans `Program.Main`:

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* Dans le serveur éblouissant, inscrivez le service dans `Startup.ConfigureServices`:

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

Utilisez la `NotifierService` pour mettre à jour un composant :

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

Dans l’exemple précédent, `NotifierService` appelle la méthode de `OnNotify` du composant en dehors du `SynchronizationContext`de éblouissant. `InvokeAsync` est utilisé pour basculer vers le contexte correct et pour la mise en file d’attente d’un rendu.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Utiliser \@clé pour contrôler la conservation des éléments et des composants

Lors du rendu d’une liste d’éléments ou de composants, et que les éléments ou composants changent par la suite, l’algorithme de différenciation de éblouissant doit décider quel élément ou composant précédent peut être conservé et comment les objets de modèle doivent être mappés à ces éléments. Normalement, ce processus est automatique et peut être ignoré, mais dans certains cas, il peut être utile de contrôler le processus.

Prenons l’exemple suivant :

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

Le contenu de la collection `People` peut changer avec des entrées insérées, supprimées ou réorganisées. Lors du rerendu du composant, le composant `<DetailsEditor>` peut changer pour recevoir différentes valeurs de paramètres de `Details`. Cela peut entraîner un rerendu plus complexe que prévu. Dans certains cas, le rerendu peut entraîner des différences de comportement visibles, telles que le focus de l’élément perdu.

Le processus de mappage peut être contrôlé à l’aide de l’attribut de directive `@key`. `@key` oblige l’algorithme de comparaison à garantir la préservation des éléments ou des composants en fonction de la valeur de la clé :

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

En cas de modification de la collection `People`, l’algorithme de comparaison conserve l’association entre les instances de `<DetailsEditor>` et les instances `person` :

* Si un `Person` est supprimé de la liste `People`, seule l’instance `<DetailsEditor>` correspondante est supprimée de l’interface utilisateur. Les autres instances restent inchangées.
* Si une `Person` est insérée à une position dans la liste, une nouvelle instance de `<DetailsEditor>` est insérée à cette position correspondante. Les autres instances restent inchangées.
* Si `Person` entrées sont réordonnées, les instances de `<DetailsEditor>` correspondantes sont conservées et réordonnées dans l’interface utilisateur.

Dans certains scénarios, l’utilisation de `@key` réduit la complexité du rerendu et évite les problèmes potentiels liés aux parties avec état de la modification du modèle DOM, telles que la position du focus.

> [!IMPORTANT]
> Les clés sont locales pour chaque élément ou composant conteneur. Les clés ne sont pas comparées globalement dans le document.

### <a name="when-to-use-key"></a>Quand utiliser \@clé

En général, il est logique d’utiliser `@key` chaque fois qu’une liste est rendue (par exemple, dans un bloc `@foreach`) et qu’une valeur appropriée existe pour définir le `@key`.

Vous pouvez également utiliser `@key` pour empêcher éblouissant de conserver un élément ou une sous-arborescence de composants lorsqu’un objet change :

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

Si `@currentPerson` change, la directive d’attribut `@key` force éblouissant à ignorer l’intégralité du `<div>` et de ses descendants, et à reconstruire la sous-arborescence de l’interface utilisateur avec de nouveaux éléments et composants. Cela peut être utile si vous avez besoin de garantir qu’aucun État d’interface utilisateur n’est préservé lorsque `@currentPerson` change.

### <a name="when-not-to-use-key"></a>Quand ne pas utiliser la clé de \@

Il y a un coût en matière de performances lors de la comparaison avec `@key`. Le coût des performances n’est pas important, mais spécifiez uniquement `@key` si les règles de conservation des éléments ou des composants bénéficient de l’application.

Même si `@key` n’est pas utilisé, éblouissant conserve autant que possible les instances d’élément et de composant enfants. Le seul avantage de l’utilisation de `@key` est de contrôler la *façon dont* les instances de modèle sont mappées aux instances de composant conservées, au lieu de l’algorithme de comparaison qui sélectionne le mappage.

### <a name="what-values-to-use-for-key"></a>Valeurs à utiliser pour \@clé

En règle générale, il est logique de fournir l’un des types de valeur suivants pour `@key`:

* Instances d’objet de modèle (par exemple, une instance de `Person` comme dans l’exemple précédent). Cela garantit la préservation en fonction de l’égalité des références d’objet.
* Identificateurs uniques (par exemple, les valeurs de clé primaire de type `int`, `string`ou `Guid`).

Assurez-vous que les valeurs utilisées pour `@key` ne sont pas en conflit. Si les valeurs en conflit sont détectées dans le même élément parent, éblouissant lève une exception, car il ne peut pas mapper de manière déterministe les anciens éléments ou composants aux nouveaux éléments ou composants. Utilisez uniquement des valeurs distinctes, telles que des instances d’objets ou des valeurs de clé primaire.

## <a name="routing"></a>Routage

Le routage dans éblouissant est obtenu en fournissant un modèle de routage à chaque composant accessible dans l’application.

Lorsqu’un fichier Razor avec une directive `@page` est compilé, la classe générée reçoit une <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle de routage. Lors de l’exécution, le routeur recherche les classes de composant avec un `RouteAttribute` et rend le composant qui a un modèle de routage correspondant à l’URL demandée.

Plusieurs modèles de routage peuvent être appliqués à un composant. Le composant suivant répond aux demandes de `/BlazorRoute` et de `/DifferentBlazorRoute`.

*Pages/BlazorRoute. Razor*:

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a>Paramètres d’itinéraire

Les composants peuvent recevoir des paramètres de routage à partir du modèle de routage fourni dans la directive `@page`. Le routeur utilise des paramètres de routage pour remplir les paramètres de composant correspondants.

*Pages/RouteParameter. Razor*:

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

Les paramètres facultatifs ne sont pas pris en charge. deux directives `@page` sont donc appliquées dans l’exemple ci-dessus. La première permet de naviguer jusqu’au composant sans paramètre. La deuxième `@page` directive prend le paramètre d’itinéraire `{text}` et assigne la valeur à la propriété `Text`.

La syntaxe de paramètre *catch-all* (`*`/`**`), qui capture le chemin d’accès dans plusieurs limites de dossiers, n’est **pas** prise en charge dans les composants Razor ( *. Razor*).

## <a name="partial-class-support"></a>Prise en charge des classes partielles

Les composants Razor sont générés en tant que classes partielles. Les composants Razor sont créés à l’aide de l’une des approches suivantes :

* C#le code est défini dans un bloc [`@code`](xref:mvc/views/razor#code) avec le balisage HTML et le code Razor dans un fichier unique. Les modèles éblouissants définissent leurs composants Razor à l’aide de cette approche.
* C#le code est placé dans un fichier code-behind défini en tant que classe partielle.

L’exemple suivant montre le composant `Counter` par défaut avec un bloc `@code` dans une application générée à partir d’un modèle éblouissant. Le balisage HTML, le code C# Razor et le code se trouvent dans le même fichier :

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

Le composant `Counter` peut également être créé à l’aide d’un fichier code-behind avec une classe partielle :

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.Razor.cs*:

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

Ajoutez tous les espaces de noms requis au fichier de classe partielle, si nécessaire. Les espaces de noms standard utilisés par les composants Razor sont les suivants :

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a>Spécifier une classe de base

La directive [`@inherits`](xref:mvc/views/razor#inherits) peut être utilisée pour spécifier une classe de base pour un composant. L’exemple suivant montre comment un composant peut hériter d’une classe de base, `BlazorRocksBase`, pour fournir les propriétés et les méthodes du composant. La classe de base doit dériver de `ComponentBase`.

*Pages/BlazorRocks. Razor*:

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

*BlazorRocksBase.cs*:

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

## <a name="specify-an-attribute"></a>Spécifier un attribut

Les attributs peuvent être spécifiés dans les composants Razor avec la directive [`@attribute`](xref:mvc/views/razor#attribute) . L’exemple suivant applique l’attribut `[Authorize]` à la classe Component :

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a>Importer des composants

L’espace de noms d’un composant créé avec Razor est basé sur (par ordre de priorité) :

* [`@namespace`](xref:mvc/views/razor#namespace) la désignation dans le balisage du fichier Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).
* `RootNamespace` du projet dans le fichier projet (`<RootNamespace>BlazorSample</RootNamespace>`).
* Nom du projet, pris à partir du nom de fichier du fichier projet ( *. csproj*), et chemin d’accès de la racine du projet au composant. Par exemple, le Framework résout *{Project root}/pages/index.Razor* (*BlazorSample. csproj*) en l’espace de noms `BlazorSample.Pages`. Les composants C# suivent les règles de liaison de nom. Pour le composant `Index` dans cet exemple, les composants de l’étendue sont tous les composants :
  * Dans le même dossier, *pages*.
  * Composants de la racine du projet qui ne spécifient pas explicitement un espace de noms différent.

Les composants définis dans un espace de noms différent sont placés dans la portée à l’aide de la directive [`@using`](xref:mvc/views/razor#using) de Razor.

Si un autre composant, `NavMenu.razor`, existe dans le dossier *BlazorSample/Shared/* , le composant peut être utilisé dans `Index.razor` avec l’instruction `@using` suivante :

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

Les composants peuvent également être référencés à l’aide de leurs noms complets, ce qui ne nécessite pas la directive [`@using`](xref:mvc/views/razor#using) :

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> La qualification de `global::` n’est pas prise en charge.
>
> L’importation de composants avec des instructions `using` avec alias (par exemple, `@using Foo = Bar`) n’est pas prise en charge.
>
> Les noms partiellement qualifiés ne sont pas pris en charge. Par exemple, l’ajout de `@using BlazorSample` et la référencement d' `NavMenu.razor` avec `<Shared.NavMenu></Shared.NavMenu>` ne sont pas pris en charge.

## <a name="conditional-html-element-attributes"></a>Attributs d’éléments HTML conditionnels

Les attributs des éléments HTML sont rendus de manière conditionnelle en fonction de la valeur .NET. Si la valeur est `false` ou `null`, l’attribut n’est pas rendu. Si la valeur est `true`, l’attribut est rendu réduit.

Dans l’exemple suivant, `IsCompleted` détermine si `checked` est restitué dans le balisage de l’élément :

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

Si `IsCompleted` est `true`, la case à cocher s’affiche comme suit :

```html
<input type="checkbox" checked />
```

Si `IsCompleted` est `false`, la case à cocher s’affiche comme suit :

```html
<input type="checkbox" />
```

Pour plus d’informations, consultez <xref:mvc/views/razor>.

> [!WARNING]
> Certains attributs HTML, tels que [Aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), ne fonctionnent pas correctement quand le type .net est un `bool`. Dans ce cas, utilisez un type de `string` à la place d’un `bool`.

## <a name="raw-html"></a>HTML brut

Les chaînes sont normalement rendues à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage qu’elles peuvent contenir est ignorée et traitée comme du texte littéral. Pour afficher le code HTML brut, encapsulez le contenu HTML dans une valeur `MarkupString`. La valeur est analysée au format HTML ou SVG et est insérée dans le DOM.

> [!WARNING]
> Le rendu de code HTML brut construit à partir de n’importe quelle source non approuvée constitue un risque pour la **sécurité** et doit être évité !

L’exemple suivant illustre l’utilisation du type `MarkupString` pour ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Composants basés sur un modèle

Les composants basés sur un modèle sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, qui peuvent ensuite être utilisés dans le cadre de la logique de rendu du composant. Les composants basés sur un modèle vous permettent de créer des composants de niveau supérieur qui sont plus réutilisables que les composants normaux. Voici quelques exemples :

* Composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête, les lignes et le pied de page de la table.
* Composant de liste qui permet à un utilisateur de spécifier un modèle pour afficher des éléments dans une liste.

### <a name="template-parameters"></a>Paramètres de modèle

Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres de composant de type `RenderFragment` ou `RenderFragment<T>`. Un fragment de rendu représente un segment de l’interface utilisateur à restituer. `RenderFragment<T>` prend un paramètre de type qui peut être spécifié lors de l’appel du fragment de rendu.

composant `TableTemplate` :

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

Lorsque vous utilisez un composant basé sur un modèle, les paramètres de modèle peuvent être spécifiés à l’aide d’éléments enfants qui correspondent aux noms des paramètres (`TableHeader` et `RowTemplate` dans l’exemple suivant) :

```razor
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

### <a name="template-context-parameters"></a>Paramètres de contexte de modèle

Les arguments de composant de type `RenderFragment<T>` passés comme éléments ont un paramètre implicite nommé `context` (par exemple, à partir de l’exemple de code précédent, `@context.PetId`), mais vous pouvez modifier le nom de paramètre à l’aide de l’attribut `Context` sur l’élément enfant. Dans l’exemple suivant, l’attribut `Context` de l’élément `RowTemplate` spécifie le paramètre `pet` :

```razor
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

Vous pouvez également spécifier l’attribut `Context` sur l’élément Component. L’attribut `Context` spécifié s’applique à tous les paramètres de modèle spécifiés. Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans élément enfant d’encapsulation). Dans l’exemple suivant, l’attribut `Context` apparaît sur l’élément `TableTemplate` et s’applique à tous les paramètres de modèle :

```razor
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

### <a name="generic-typed-components"></a>Composants génériques

Les composants basés sur un modèle sont souvent typés de façon générique. Par exemple, un composant générique `ListViewTemplate` peut être utilisé pour restituer des valeurs `IEnumerable<T>`. Pour définir un composant générique, utilisez la directive [`@typeparam`](xref:mvc/views/razor#typeparam) pour spécifier les paramètres de type :

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

Lorsque vous utilisez des composants génériques, le paramètre de type est déduit si possible :

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

Sinon, le paramètre de type doit être spécifié explicitement à l’aide d’un attribut qui correspond au nom du paramètre de type. Dans l’exemple suivant, `TItem="Pet"` spécifie le type :

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Valeurs et paramètres en cascade

Dans certains scénarios, il est peu commode de transmettre des données d’un composant ancêtre à un composant descendant à l’aide de [paramètres de composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant. Les valeurs et paramètres en cascade résolvent ce problème en fournissant un moyen pratique pour un composant ancêtre de fournir une valeur à tous ses composants descendants. Les valeurs et les paramètres en cascade fournissent également une approche pour les composants à coordonner.

### <a name="theme-example"></a>Exemple de thème

Dans l’exemple suivant tiré de l’exemple d’application, la classe `ThemeInfo` spécifie les informations de thème pour descendre dans la hiérarchie des composants afin que tous les boutons d’une partie donnée de l’application partagent le même style.

*UIThemeClasses/themeinfo. cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade. Le composant `CascadingValue` encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants de cette sous-arborescence.

Par exemple, l’exemple d’application spécifie les informations de thème (`ThemeInfo`) dans l’une des dispositions de l’application en tant que paramètre en cascade pour tous les composants qui composent le corps de la disposition de la propriété `@Body`. une valeur de `btn-success` dans le composant de disposition est assignée à `ButtonClass`. Tout composant descendant peut consommer cette propriété par le biais du `ThemeInfo` objet en cascade.

composant `CascadingValuesParametersLayout` :

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Pour utiliser des valeurs en cascade, les composants déclarent des paramètres en cascade à l’aide de l’attribut `[CascadingParameter]`. Les valeurs en cascade sont liées aux paramètres en cascade par type.

Dans l’exemple d’application, le composant `CascadingValuesParametersTheme` lie la valeur en cascade `ThemeInfo` à un paramètre en cascade. Le paramètre est utilisé pour définir la classe CSS pour l’un des boutons affichés par le composant.

composant `CascadingValuesParametersTheme` :

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

Pour mettre en cascade plusieurs valeurs du même type dans la même sous-arborescence, fournissez une chaîne de `Name` unique à chaque composant `CascadingValue` et à la `CascadingParameter`correspondante. Dans l’exemple suivant, deux composants de `CascadingValue` cascadent différentes instances de `MyCascadingType` par nom :

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

Dans un composant descendant, les paramètres en cascade reçoivent leurs valeurs des valeurs en cascade correspondantes dans le composant ancêtre par nom :

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>Exemple TabSet

Les paramètres en cascade permettent également aux composants de collaborer au sein de la hiérarchie des composants. Par exemple, considérez l’exemple *TabSet* suivant dans l’exemple d’application.

L’exemple d’application possède une interface `ITab` que les onglets implémentent :

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

Le composant `CascadingValuesParametersTabSet` utilise le composant `TabSet`, qui contient plusieurs composants `Tab` :

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

Les composants `Tab` enfants ne sont pas explicitement passés comme paramètres au `TabSet`. Au lieu de cela, les composants de `Tab` enfants font partie du contenu enfant du `TabSet`. Toutefois, le `TabSet` doit toujours connaître chaque composant `Tab` afin qu’il puisse restituer les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter de code supplémentaire, le composant `TabSet` *peut se présenter comme une valeur en cascade* qui est ensuite récupérée par les composants de `Tab` descendants.

composant `TabSet` :

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

Les composants de `Tab` descendants capturent le `TabSet` contenant sous forme de paramètre en cascade, de sorte que les composants `Tab` s’ajoutent eux-mêmes au `TabSet` et coordonnent l’onglet actif.

composant `Tab` :

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>Modèles Razor

Les fragments de rendu peuvent être définis à l’aide de la syntaxe de modèle Razor. Les modèles Razor sont un moyen de définir un extrait de code d’interface utilisateur et de supposer le format suivant :

```razor
@<{HTML tag}>...</{HTML tag}>
```

L’exemple suivant montre comment spécifier des valeurs `RenderFragment` et `RenderFragment<T>` et afficher des modèles directement dans un composant. Les fragments de rendu peuvent également être passés comme arguments à des [composants basés](#templated-components)sur un modèle.

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

Sortie rendue du code précédent :

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="manual-rendertreebuilder-logic"></a>Logique RenderTreeBuilder manuelle

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fournit des méthodes pour la manipulation des composants et des éléments, notamment la C# génération manuelle de composants dans le code.

> [!NOTE]
> L’utilisation de `RenderTreeBuilder` pour créer des composants est un scénario avancé. Un composant mal formé (par exemple, une balise de balisage non fermée) peut entraîner un comportement indéfini.

Prenons le composant `PetDetails` suivant, qui peut être intégré manuellement à un autre composant :

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

Dans l’exemple suivant, la boucle de la méthode `CreateComponent` génère trois composants `PetDetails`. Lors de l’appel de méthodes `RenderTreeBuilder` pour créer les composants (`OpenComponent` et `AddAttribute`), les numéros de séquence sont des numéros de ligne de code source. L’algorithme de différence éblouissant s’appuie sur les numéros de séquence correspondant à des lignes de code distinctes, et non sur des appels d’appel distincts. Lors de la création d’un composant avec des méthodes `RenderTreeBuilder`, coder en dur les arguments pour les numéros de séquence. **L’utilisation d’un calcul ou d’un compteur pour générer le numéro de séquence peut entraîner des performances médiocres.** Pour plus d’informations, consultez la section [numéros de séquence liés à des numéros de ligne de code et non à un ordre d’exécution](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .

composant `BuiltContent` :

```razor
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

> [!WARNING]
> Les types dans `Microsoft.AspNetCore.Components.RenderTree` permettent le traitement des *résultats* des opérations de rendu. Il s’agit des détails internes de l’implémentation du Framework éblouissant. Ces types doivent être considérés comme *instables* et susceptibles d’être modifiés dans les versions ultérieures.

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Les numéros de séquence sont liés aux numéros de ligne de code et non à l’ordre d’exécution

Les fichiers de `.razor` éblouissant sont toujours compilés. C’est un avantage considérable pour `.razor`, car l’étape de compilation peut être utilisée pour injecter des informations qui améliorent les performances de l’application au moment de l’exécution.

Un exemple clé de ces améliorations concerne les *numéros de séquence*. Les numéros de séquence indiquent au runtime que les sorties proviennent de lignes de code distinctes et ordonnées. Le runtime utilise ces informations pour générer des différences d’arborescence efficaces en temps linéaire, ce qui est beaucoup plus rapide qu’un algorithme de comparaison d’arborescence générale.

Considérez le fichier de composant Razor ( *. Razor*) suivant :

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

Le code précédent se compile comme suit :

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Lorsque le code s’exécute pour la première fois, si `someFlag` est `true`, le générateur reçoit :

| Séquence | Type      | Données   |
| :------: | --------- | :----: |
| 0        | Nœud de texte | Premier  |
| 1        | Nœud de texte | Seconde |

Imaginez que `someFlag` devient `false`et que le balisage est de nouveau restitué. Cette fois-ci, le générateur reçoit :

| Séquence | Type       | Données   |
| :------: | ---------- | :----: |
| 1        | Nœud de texte  | Seconde |

Quand le runtime effectue une comparaison, il constate que l’élément au `0` de la séquence a été supprimé, donc il génère le *script d’édition*trivial suivant :

* Supprimez le premier nœud de texte.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Qu’est-ce qui se passe si vous générez des numéros séquentiels par programmation

Imaginez plutôt que vous avez écrit la logique de générateur d’arborescence de rendu suivante :

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

La première sortie est désormais :

| Séquence | Type      | Données   |
| :------: | --------- | :----: |
| 0        | Nœud de texte | Premier  |
| 1        | Nœud de texte | Seconde |

Ce résultat est identique au cas précédent, donc aucun problème négatif n’existe. `someFlag` est `false` sur le deuxième rendu et la sortie est la suivante :

| Séquence | Type      | Données   |
| :------: | --------- | ------ |
| 0        | Nœud de texte | Seconde |

Cette fois-ci, l’algorithme diff constate que *deux* modifications ont été apportées, et l’algorithme génère le script Edit suivant :

* Remplacez la valeur du premier nœud de texte par `Second`.
* Supprimez le deuxième nœud de texte.

La génération des numéros de séquence a perdu toutes les informations utiles sur l’emplacement où les `if/else` branches et les boucles étaient présentes dans le code d’origine. Il en résulte une comparaison de **deux fois plus longtemps** qu’auparavant.

Il s’agit d’un exemple trivial. Dans des cas plus réalistes avec des structures complexes et profondément imbriquées, et surtout avec des boucles, le coût des performances est plus grave. Au lieu d’identifier immédiatement les blocs de boucle ou les branches qui ont été insérées ou supprimées, l’algorithme diff doit recurse profondément dans les arborescences de rendu et générer des scripts de modification beaucoup plus longs, car il est mal informé de la façon dont les anciennes et les nouvelles structures sont liées les unes aux autres.

#### <a name="guidance-and-conclusions"></a>Conseils et conclusions

* Les performances de l’application sont affectées si les numéros séquentiels sont générés dynamiquement.
* L’infrastructure ne peut pas créer ses propres numéros de séquence automatiquement au moment de l’exécution, car les informations nécessaires n’existent pas, sauf si elles sont capturées au moment de la compilation.
* N’écrivez pas de longs blocs de `RenderTreeBuilder` logique implémentée manuellement. Préférez `.razor` fichiers et permettre au compilateur de gérer les numéros de séquence. Si vous ne parvenez pas à éviter une logique de `RenderTreeBuilder` manuelle, fractionnez les blocs de code longs en éléments plus petits encapsulés dans `OpenRegion`/`CloseRegion` appels. Chaque région a son propre espace distinct pour les numéros de séquence, ce qui vous permet de redémarrer à partir de zéro (ou tout autre nombre arbitraire) à l’intérieur de chaque région.
* Si les numéros de séquence sont codés en dur, l’algorithme diff exige uniquement que les numéros de séquence augmentent dans la valeur. La valeur initiale et les écarts ne sont pas pertinents. Une option légitime consiste à utiliser le numéro de ligne de code comme numéro de séquence, ou à commencer à partir de zéro et à augmenter par des ou des centaines (ou un intervalle de préférence). 
* Blazor utilise des numéros de séquence, tandis que d’autres infrastructures d’interface utilisateur de comparaison d’arborescence ne les utilisent pas. La comparaison est beaucoup plus rapide lorsque les numéros de séquence sont utilisés et Blazor présente l’avantage d’une étape de compilation qui traite automatiquement les numéros séquentiels pour les développeurs qui créent des fichiers *. Razor* .

## <a name="localization"></a>Localisation

les applications Blazor Server sont localisées à l’aide de l' [intergiciel (middleware](xref:fundamentals/localization#localization-middleware)) de localisation. L’intergiciel sélectionne la culture appropriée pour les utilisateurs qui demandent des ressources à partir de l’application.

La culture peut être définie à l’aide de l’une des approches suivantes :

* [Cookies](#cookies)
* [Fournir l’interface utilisateur pour choisir la culture](#provide-ui-to-choose-the-culture)

Pour plus d’informations et d’exemples, consultez <xref:fundamentals/localization>.

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a>Configurer l’éditeur de liens pour l’internationalisation (Blazor webassembly)

Par défaut, la configuration de l’éditeur de liens de Blazorpour les applications webassembly Blazor supprime les informations d’internationalisation, à l’exception des paramètres régionaux demandés explicitement. Pour plus d’informations et de conseils sur le contrôle du comportement de l’éditeur de liens, consultez <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.

### <a name="cookies"></a>Cookies

Un cookie de culture de localisation peut conserver la culture de l’utilisateur. Le cookie est créé par la méthode `OnGet` de la page hôte de l’application (*pages/Host. cshtml. cs*). L’intergiciel (middleware) de localisation lit le cookie sur les demandes suivantes pour définir la culture de l’utilisateur. 

L’utilisation d’un cookie garantit que la connexion WebSocket peut propager correctement la culture. Si les schémas de localisation sont basés sur le chemin d’URL ou la chaîne de requête, il est possible que le schéma ne soit pas en mesure de fonctionner avec les WebSockets, et donc de ne pas conserver la culture. Par conséquent, l’utilisation d’un cookie de culture de localisation est l’approche recommandée.

Toute technique peut être utilisée pour assigner une culture si la culture est rendue persistante dans un cookie de localisation. Si l’application a déjà un schéma de localisation établi pour les ASP.NET Core côté serveur, continuez à utiliser l’infrastructure de localisation existante de l’application et à définir le cookie de la culture de localisation dans le schéma de l’application.

L’exemple suivant montre comment définir la culture actuelle dans un cookie qui peut être lu par l’intergiciel (middleware) de localisation. Créez un fichier *pages/Host. cshtml. cs* avec le contenu suivant dans l’application Blazor Server :

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

La localisation est gérée dans l’application :

1. Le navigateur envoie une requête HTTP initiale à l’application.
1. La culture est affectée par l’intergiciel (middleware) de localisation.
1. La méthode `OnGet` dans *_Host. cshtml. cs* rend persistante la culture dans un cookie dans le cadre de la réponse.
1. Le navigateur ouvre une connexion WebSocket pour créer une session Blazor Server interactive.
1. L’intergiciel de localisation lit le cookie et assigne la culture.
1. La session du serveur de Blazor commence par la culture correcte.

### <a name="provide-ui-to-choose-the-culture"></a>Fournir l’interface utilisateur pour choisir la culture

Pour fournir une interface utilisateur permettant à un utilisateur de sélectionner une culture, il est recommandé d’effectuer une *approche basée sur la redirection* . Le processus est similaire à ce qui se produit dans une application Web lorsqu’un utilisateur tente d’accéder à une ressource sécurisée&mdash;l’utilisateur est redirigé vers une page de connexion, puis redirigé vers la ressource d’origine. 

L’application conserve la culture sélectionnée de l’utilisateur via une redirection vers un contrôleur. Le contrôleur définit la culture sélectionnée de l’utilisateur dans un cookie et redirige l’utilisateur vers l’URI d’origine.

Établissez un point de terminaison HTTP sur le serveur pour définir la culture sélectionnée de l’utilisateur dans un cookie et effectuer la redirection vers l’URI d’origine :

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
> Utilisez le résultat de l’action `LocalRedirect` pour empêcher les attaques de redirection ouvertes. Pour plus d’informations, consultez <xref:security/preventing-open-redirects>.

Le composant suivant montre un exemple d’exécution de la redirection initiale lorsque l’utilisateur sélectionne une culture :

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a>Utiliser les scénarios de localisation .NET dans les applications Blazor

Dans Blazor Apps, les scénarios de localisation et de globalisation .NET suivants sont disponibles :

* . Système de ressources du réseau
* Mise en forme des nombres et des dates spécifiques à la culture

la fonctionnalité de `@bind` de Blazoreffectue une globalisation basée sur la culture actuelle de l’utilisateur. Pour plus d’informations, consultez la section [liaison de données](#data-binding) .

Un ensemble limité de scénarios de localisation de ASP.NET Core est actuellement pris en charge :

* `IStringLocalizer<>` *est pris en charge* dans les applications Blazor.
* la localisation des annotations de données `IHtmlLocalizer<>`, `IViewLocalizer<>`et est ASP.NET Core les scénarios MVC et **non pris en charge** dans les applications Blazor.

Pour plus d’informations, consultez <xref:fundamentals/localization>.

## <a name="scalable-vector-graphics-svg-images"></a>Images SVG (Scalable Vector Graphics)

Étant donné que Blazor restitue le code HTML, les images prises en charge par le navigateur, y compris les images SVG (Scalable Vector Graphics) ( *. svg*), sont prises en charge via la balise `<img>` :

```html
<img alt="Example image" src="some-image.svg" />
```

De même, les images SVG sont prises en charge dans les règles CSS d’un fichier de feuille de style ( *. CSS*) :

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Toutefois, le balisage SVG en ligne n’est pas pris en charge dans tous les scénarios. Si vous placez une balise de `<svg>` directement dans un fichier de composant ( *. Razor*), le rendu d’image de base est pris en charge, mais de nombreux scénarios avancés ne sont pas encore pris en charge. Par exemple, les balises `<use>` ne sont pas respectées et `@bind` ne peuvent pas être utilisées avec certaines balises SVG. Nous prévoyons de traiter ces limitations dans une version ultérieure.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/blazor/server> &ndash; contient des conseils sur la création d’applications serveur Blazor qui doivent rivaliser avec l’épuisement des ressources.
