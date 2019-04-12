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
# <a name="create-and-use-razor-components"></a>Créer et utiliser des composants de Razor

Par [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), et [Morné Zaayman](https://github.com/MorneZaayman)

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)). Consultez la rubrique [Bien démarrer](xref:razor-components/get-started) pour connaître les prérequis.

Les applications de composants de Razor sont créées à l’aide de *composants*. Un composant est un bloc autonome de l’interface utilisateur (IU), par exemple une page, une boîte de dialogue ou un formulaire. Un composant inclut un balisage HTML et la logique de traitement nécessaire pour injecter des données ou de répondre aux événements d’interface utilisateur. Les composants sont flexibles et léger. Ils peuvent être imbriqués, réutilisés et partagés entre des projets.

## <a name="component-classes"></a>Classes de composant

Les composants sont généralement implémentées dans les fichiers de composant de Razor (*.razor*) à l’aide d’une combinaison de C# et le balisage HTML (*.cshtml* fichiers sont utilisés dans les applications Blazor).

Les composants peuvent être créés dans les applications de composants de Razor à l’aide de la *.cshtml* extension de fichier tant que les fichiers sont identifiés en tant que fichiers de composant de Razor à l’aide de la `_RazorComponentInclude` propriété MSBuild. Par exemple, une application créée à l’aide du modèle de composant de Razor Spécifie que tous les *.cshtml* fichiers sous le *composants* dossier doit être traité en tant que fichiers de composants de Razor :

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

L’interface utilisateur pour un composant est défini à l’aide de HTML. La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor). Lors de la compilation d’une application de composants de Razor, le balisage HTML et C# logique de rendu sont converties en une classe de composant. Le nom de la classe générée correspond au nom du fichier.

Membres de la classe de composant sont définies dans un `@functions` bloc (plusieurs `@functions` bloc est autorisé). Dans le `@functions` bloc, l’état du composant (propriétés, champs) est spécifié avec des méthodes pour la gestion des événements ou pour définir une autre logique de composant.

Membres composant peuvent ensuite être utilisées comme partie du composant de rendu à l’aide de la logique C# expressions qui commencent par `@`. Par exemple, un C# champ est restitué en préfixant `@` au nom du champ. L’exemple suivant évalue et effectue le rendu :

* `_headingFontStyle` la valeur de propriété CSS pour `font-style`.
* `_headingText` pour le contenu de la `<h1>` élément.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Une fois que le composant est initialement affichée, le composant régénère son arborescence rendu en réponse aux événements. Composants de Razor compare la nouvelle arborescence de rendu par rapport à le, puis applique toutes les modifications pour du navigateur modèle DOM (Document Object).

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Intégrer des composants des applications MVC et les Pages Razor

Utiliser des composants avec des applications existantes sur les Pages Razor et MVC. Il est inutile de réécrire les pages existantes ou des vues à utiliser les composants de Razor. Lorsque la page ou la vue est restituée, les composants sont préaffichés&dagger; en même temps. 

> [!NOTE]
> &dagger;Pré-rendu du côté serveur est activé pour les applications de Razor composants par défaut. Les applications côté client Blazor prendra en charge pré-rendu dans la prochaine version Preview 4. Pour plus d’informations, consultez [mettre à jour des modèles/intergiciel (middleware) à utiliser le fichier/MapFallbackToPage](https://github.com/aspnet/AspNetCore/issues/8852).

Pour restituer un composant à partir d’une page ou une vue, utilisez le `RenderComponentAsync<TComponent>` méthode d’assistance HTML :

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

Rendu des pages et des vues de composants ne sont pas encore interactives dans la version Preview 3. Par exemple, la sélection d’un bouton ne déclenche un appel de méthode. Une prochaine version d’évaluation traitera cette limitation et ajouter la prise en charge pour les composants de rendu à l’aide de la syntaxe d’élément et d’attribut normale.

Tandis que les pages et vues peuvent utiliser des composants, l’inverse n’est pas vrai. Composants ne pouvez pas utiliser de vue - et -spécifiques à la page scénarios, tels que les sections et les vues partielles. Pour utiliser une logique à partir de la vue partielle dans un composant, du facteur de la logique de la vue partielle dans un composant.

## <a name="using-components"></a>À l’aide de composants

Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML. Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.

Restitue le balisage suivant un `HeadingComponent` (*HeadingComponent.cshtml*) instance :

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a>Paramètres de composant

Composants peuvent avoir *composant paramètres*, qui sont définis à l’aide de *non public* assorties de propriétés de la classe de composant `[Parameter]`. Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.

Dans l’exemple suivant, le `ParentComponent` définit la valeur de la `Title` propriété de la `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>Contenu enfant

Composants peuvent définir le contenu d’un autre composant. Le composant affectation fournit le contenu entre les balises qui spécifient le composant de réception. Par exemple, un `ParentComponent` peut fournir des contenus pour le rendu par un composant enfant en plaçant le contenu à l’intérieur `<ChildComponent>` balises.

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

Le composant enfant a un `ChildContent` propriété qui représente un `RenderFragment`. La valeur de `ChildContent` est placé dans le balisage du composant enfant où le contenu doit être restitué. Dans l’exemple suivant, la valeur de `ChildContent` est reçu à partir du composant parent et restituée dans le panneau de configuration d’amorçage `panel-body`.

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> La réception de la propriété le `RenderFragment` contenu doit être nommé `ChildContent` par convention.

## <a name="data-binding"></a>Liaison de données

Liaison de données à la fois des composants et des éléments DOM est effectuée avec la `bind` attribut. L’exemple suivant lie la `ItalicsCheck` état activé de propriété à la case à cocher :

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

Lorsque la case à cocher est activée et désactivée, la valeur de propriété est mis à jour vers `true` et `false`, respectivement.

La case à cocher est mis à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, pas en réponse à l’évolution de la valeur de propriété. Dans la mesure où les composants s’afficher après l’exécution de code de gestionnaire d’événements, mises à jour de la propriété sont généralement pas immédiatement dans l’interface utilisateur.

À l’aide de `bind` avec un `CurrentValue` propriété (`<input bind="@CurrentValue">`) est fondamentalement équivalent à ce qui suit :

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

Lorsque le composant est affiché, le `value` de l’élément d’entrée provient de la `CurrentValue` propriété. Lorsque l’utilisateur tape dans la zone de texte, le `onchange` événement est déclenché et le `CurrentValue` propriété est définie sur la valeur modifiée. En réalité, la génération de code est un peu plus complexe, car `bind` gère des quelques cas où les conversions de type sont effectuées. En principe, `bind` associe la valeur actuelle d’une expression avec un `value` attribut et gère des modifications à l’aide du gestionnaire enregistré.

En plus de `onchange`, la propriété peut être liée à l’aide d’autres événements tels que `oninput` en étant plus explicite sur les éléments à lier à :

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

Contrairement aux `onchange`, `oninput` se déclenche pour chaque caractère est entrée dans la zone de texte.

**Chaînes de format**

Liaison de données fonctionne avec <xref:System.DateTime> chaînes de format. Autres expressions de format, telles que les devises ou les formats de nombre, ne sont pas disponibles pour l’instant.

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

Le `format-value` attribut spécifie le format de date à appliquer à la `value` de la `input` élément. Le format est également utilisé pour analyser la valeur quand un `onchange` événement se produit.

**Paramètres de composant**

Liaison reconnaît également les paramètres du composant, où `bind-{property}` peut lier une valeur de propriété entre plusieurs composants.

Utilise le composant suivant `ChildComponent` et lie le `ParentYear` paramètre du parent de le `Year` paramètre sur le composant enfant :

Composant parent :

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

Composant enfant :

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

Chargement de la `ParentComponent` génère le balisage suivant :

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Si la valeur de la `ParentYear` propriété est modifiée en sélectionnant le bouton dans le `ParentComponent`, le `Year` propriété de la `ChildComponent` est mis à jour. La nouvelle valeur de `Year` est affiché dans l’interface utilisateur lorsque le `ParentComponent` est un nouvel affichage :

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Le `Year` paramètre est peut être liée, car il possède un accompagnement `YearChanged` événement qui correspond au type de le `Year` paramètre.

Par convention, `<ChildComponent bind-Year="@ParentYear" />` est fondamentalement équivalent à l’écriture,

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

En règle générale, une propriété peut être liée à un gestionnaire événements correspondant à l’aide `bind-property-event` attribut.

## <a name="event-handling"></a>Gestion des événements

Composants de Razor fournissent des fonctionnalités de gestion des événements. Pour un attribut d’élément HTML nommé `on<event>` (par exemple, `onclick`, `onsubmit`) avec une valeur typée de délégué, les composants de Razor traite la valeur de l’attribut comme gestionnaire d’événements. Le nom d’attribut commence toujours par `on`.

Le code suivant appelle la `UpdateHeading` méthode lorsque le bouton est sélectionné dans l’interface utilisateur :

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

Le code suivant appelle la `CheckboxChanged` méthode lorsque la case à cocher est modifié dans l’interface utilisateur :

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

Gestionnaires d’événements peuvent également être asynchrone et retour un <xref:System.Threading.Tasks.Task>. Il est inutile d’appeler manuellement `StateHasChanged()`. Exceptions sont enregistrées lorsqu’ils se produisent.

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

Pour certains événements, types d’arguments spécifiques à l’événement événement sont autorisées. Si l’accès à l’un de ces types d’événements n’est pas nécessaire, elle n’est pas requise dans l’appel de méthode.

La liste d’arguments d’événement pris en charge est :

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

Expressions lambda peuvent également être utilisées :

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

Il est souvent utile de fermer sur des valeurs supplémentaires, comme lors de l’itération sur un jeu d’éléments. L’exemple suivant crée trois boutons, chacune appelant `UpdateHeading` en passant un argument d’événement (`UIMouseEventArgs`) et son numéro de bouton (`buttonNumber`) lorsque sélectionné dans l’interface utilisateur :

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
> Faire **pas** utiliser la variable de boucle (`i`) dans un `for` boucle directement dans une expression lambda. Sinon, la même variable est utilisée par toutes les expressions lambda à l’origine `i`de valeur pour être identiques dans toutes les expressions lambda. Capture pas toujours sa valeur dans une variable locale (`buttonNumber` dans l’exemple précédent), puis l’utiliser.

## <a name="capture-references-to-components"></a>Capturer les références aux composants

Références de composants fournissent un moyen de get une référence à une instance du composant afin que vous pouvez émettre des commandes à cette instance, tels que `Show` ou `Reset`. Pour capturer une référence de composant, ajoutez un `ref` attribut au composant enfant et puis définir un champ avec le même nom et le même type que celui du composant enfant.

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

Lorsque le composant est affiché, le `loginDialog` champ est renseigné avec la `MyLoginDialog` instance du composant enfant. Vous pouvez ensuite appeler les méthodes de .NET sur l’instance du composant.

> [!IMPORTANT]
> Le `loginDialog` variable est renseignée uniquement une fois que le composant est restitué et sa sortie inclut le `MyLoginDialog` élément. Jusqu'à ce point, il n’a rien à faire référence. Pour manipuler les références de composants une fois que le composant a terminé le rendu, utilisez la `OnAfterRenderAsync` ou `OnAfterRender` méthodes.

Tandis que les références de composant de capture utilise une syntaxe similaire à [capture les références d’élément](xref:razor-components/javascript-interop#capture-references-to-elements), il n’est pas un [JavaScript interop](xref:razor-components/javascript-interop) fonctionnalité. Références de composants ne sont pas transmis au code JavaScript ; ils sont utilisés uniquement dans le code .NET.

> [!NOTE]
> Faire **pas** utilisent des références de composant pour muter l’état des composants de l’enfant. Utilisez plutôt les paramètres déclaratifs normales pour passer des données à des composants enfants. Cela entraîne des composants enfants de restituer automatiquement aux moments appropriés à.

## <a name="lifecycle-methods"></a>Méthodes de cycle de vie

`OnInitAsync` et `OnInit` exécuter du code pour initialiser le composant. Pour effectuer une opération asynchrone, utilisez `OnInitAsync` et le `await` mot clé sur l’opération :

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

Pour une opération synchrone, utilisez `OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` et `OnParametersSet` sont appelés quand un composant a reçu les paramètres de son parent et les valeurs sont affectées aux propriétés. Ces méthodes sont exécutées après l’initialisation du composant et ensuite chaque fois que le composant est rendu :

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

`OnAfterRenderAsync` et `OnAfterRender` sont appelées après un composant a terminé le rendu. Références de composant et d’élément sont renseignées à ce stade. Cette étape permet d’effectuer les étapes d’initialisation supplémentaires à l’aide du contenu affiché, telles que l’activation des bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.

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

`SetParameters` peut être substituée pour exécuter du code avant de définir des paramètres :

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Si `base.SetParameters` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants dans une façon requis. Par exemple, les paramètres entrants ne sont pas nécessaire d’affecter aux propriétés sur la classe.

`ShouldRender` peut être substituée pour supprimer l’actualisation de l’interface utilisateur. Si l’implémentation retourne `true`, l’interface utilisateur est actualisée. Même si `ShouldRender` est substitué, le composant est toujours affichée à l’origine.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Élimination de composant avec IDisposable

Si un composant implémente <xref:System.IDisposable>, le [Dispose, méthode](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur. Utilise le composant suivant `@implements IDisposable` et `Dispose` méthode :

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

## <a name="routing"></a>Routage

Routage dans les composants de Razor est obtenu en fournissant un modèle d’itinéraire pour chaque composant accessible dans l’application.

Quand un *.cshtml* de fichiers avec un `@page` directive est compilée, la classe générée est donnée un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle d’itinéraire. Lors de l’exécution, le routeur recherche de classes de composant avec un `RouteAttribute` et restitue le composant a un modèle d’itinéraire qui correspond à l’URL demandée.

Plusieurs modèles d’itinéraire peuvent être appliqués à un composant. Le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Paramètres d’itinéraire

Composants peuvent recevoir des paramètres d’itinéraire à partir du modèle d’itinéraire fourni dans la `@page` directive. Le routeur utilise les paramètres d’itinéraire pour remplir les paramètres de composant correspondants.

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

Paramètres facultatifs ne sont pas pris en charge, par conséquent, deux `@page` directives sont appliquées dans l’exemple ci-dessus. La première permet la navigation vers le composant sans paramètre. La seconde `@page` directive prend le `{text}` paramètre d’itinéraire et affecte la valeur à la `Text` propriété.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Héritage de classe de base pour une expérience « code-behind »

Fichiers composants (*.cshtml*) mélanger le balisage HTML et C# code dans le même fichier de traitement. Le `@inherits` directive peut être utilisée pour fournir une expérience de « code-behind » qui sépare le balisage de composant à partir du code de traitement des applications de composants de Razor.

Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) montre comment un composant peut hériter d’une classe de base, `BlazorRocksBase`, pour fournir les propriétés et les méthodes du composant.

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

La classe de base doit dériver de `ComponentBase`.

## <a name="razor-support"></a>Prise en charge de Razor

**Directives Razor**

Directives Razor sont affichés dans le tableau suivant.

| Directive | Description |
| --------- | ----------- |
| [\@functions](xref:mvc/views/razor#section-5) | Ajoute un C# bloc de code pour un composant. |
| `@implements` | Implémente une interface pour la classe de composant généré. |
| [\@Hérite de](xref:mvc/views/razor#section-3) | Fournit un contrôle complet de la classe qui hérite du composant. |
| [\@inject](xref:mvc/views/razor#section-4) | Permet l’injection de code à partir de service le [conteneur de service](xref:fundamentals/dependency-injection). Pour plus d’informations, consultez [Injection de dépendances dans les vues](xref:mvc/views/dependency-injection). |
| `@layout` | Spécifie un composant de mise en forme. Composants de mise en page sont utilisés afin d’éviter des incohérences et la duplication de code. |
| [\@page](xref:razor-pages/index#razor-pages) | Spécifie que le composant doit gérer les demandes directement. Le `@page` directive peut être spécifiée avec un itinéraire et des paramètres facultatifs. Contrairement aux Pages Razor, le `@page` directive n’a pas besoin être la première directive en haut du fichier. Pour plus d’informations, consultez [Routage](xref:razor-components/routing). |
| [\@À l’aide de](xref:mvc/views/razor#using) | Ajoute le C# `using` directive à la classe de composant généré. |
| [\@addTagHelper](xref:mvc/views/razor#tag-helpers) | Utiliser `@addTagHelper` pour utiliser un composant dans un autre assembly que l’assembly de l’application. |

**Attributs conditionnels**

Les attributs sont rendus conditionnelle basée sur la valeur .NET. Si la valeur est `false` ou `null`, l’attribut n’est pas rendu. Si la valeur est `true`, l’attribut est rendu réduite.

Dans l’exemple suivant, `IsCompleted` détermine si `checked` est restitué dans le balisage du contrôle :

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

Si `IsCompleted` est `true`, la case à cocher est rendue en tant que :

```html
<input type="checkbox" checked>
```

Si `IsCompleted` est `false`, la case à cocher est rendue en tant que :

```html
<input type="checkbox">
```

**Informations supplémentaires sur Razor**

Pour plus d’informations sur Razor, consultez le [référence de la syntaxe Razor](xref:mvc/views/razor).

## <a name="raw-html"></a>HTML brut

Les chaînes sont normalement restitués à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage, qu'elles peuvent contenir est ignorée et traitée en tant que texte littéral. Pour afficher le HTML brut, encapsuler le contenu HTML dans un `MarkupString` valeur. La valeur est analysée en tant que HTML ou SVG et insérée dans le DOM.

> [!WARNING]
> Rendu HTML brut construit à partir d’une non fiables source est un **risque de sécurité** et doit être évitée !

L’exemple suivant illustre l’utilisation du `MarkupString` type à ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Composants basés sur des modèles

Composants basés sur des modèles sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, ce qui peuvent ensuite être utilisés comme partie de la logique de rendu du composant. Composants basés sur des modèles vous permettent de vous montre comment créer des composants de haut niveau qui sont plus facilement réutilisables que les composants standards. Voici deux exemples sont les suivantes :

* Un composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête de la table, de lignes et de pied de page.
* Un composant de liste qui permet à un utilisateur de spécifier un modèle pour le rendu des éléments dans une liste.

### <a name="template-parameters"></a>Paramètres de modèle

Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres de composant de type `RenderFragment` ou `RenderFragment<T>`. Un fragment de rendu représente un segment de l’interface utilisateur qui est restitué par le composant. Un fragment de rendu prend un paramètre qui peut être spécifié lorsque le fragment de rendu est appelé.

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

Lorsque vous utilisez un composant basé sur un modèle, les paramètres du modèle peuvent être spécifiés à l’aide des éléments enfants qui correspondent aux noms des paramètres (`TableHeader` et `RowTemplate` dans l’exemple suivant) :

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

### <a name="template-context-parameters"></a>Paramètres de contexte de modèle

Arguments de composant de type `RenderFragment<T>` passé comme éléments ont un paramètre implicit nommé `context` (par exemple à partir de l’exemple de code précédent, `@context.PetId`), mais vous pouvez modifier le nom de paramètre en utilisant le `Context` attribut sur l’enfant élément. Dans l’exemple suivant, le `RowTemplate` l’élément `Context` attribut spécifie le `pet` paramètre :

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

Vous pouvez également spécifier le `Context` attribut sur l’élément de composant. Spécifié `Context` attribut s’applique à tous les paramètres de modèle spécifié. Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans habillage du n’importe quel élément enfant). Dans l’exemple suivant, le `Context` attribut apparaît sur le `TableTemplate` élément et s’applique à tous les paramètres de modèle :

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

### <a name="generic-typed-components"></a>Composants typées de génériques

Composants basés sur des modèles sont souvent de manière générique de type. Par exemple, un composant de modèle de vue de liste générique peut être utilisé pour restituer `IEnumerable<T>` valeurs. Pour définir un composant générique, utilisez la `@typeparam` directive pour spécifier les paramètres de type.

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

Lorsque vous utilisez des composants typées de génériques, le paramètre de type est déduit si possible :

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

Sinon, le paramètre de type doit être explicitement spécifié à l’aide d’un attribut qui correspond au nom du paramètre de type. Dans l’exemple suivant, `TItem="Pet"` Spécifie le type :

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Paramètres et valeurs en cascade

Dans certains scénarios, il est peu pratique pour transmettre les données à partir d’un composant d’ancêtre à un composant descendant à l’aide [paramètres du composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant. Les valeurs et les paramètres en cascade résolvent ce problème en fournissant un moyen pratique d’un composant d’ancêtre à fournir une valeur pour tous ses composants descendants. Les valeurs et les paramètres en cascade fournissent également une approche pour coordonner des composants.

### <a name="theme-example"></a>Exemple de thème

Dans l’exemple suivant *thème* exemple à partir de l’exemple d’application, le `ThemeInfo` classe spécifie les informations de thème pour le flux vers le bas de la hiérarchie des composants afin que tous les boutons dans une partie donnée de l’application partagent le même style.

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade. Le composant de valeur en cascade encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants dans cette sous-arborescence.

Par exemple, l’exemple d’application spécifie les informations de thème (`ThemeInfo`) dans une des dispositions de l’application comme un paramètre en cascade pour tous les composants qui composent le corps de la disposition de la `@Body` propriété. `ButtonClass` reçoit une valeur de `btn-success` dans le composant de mise en page. N’importe quel composant descendant peut consommer de cette propriété via le `ThemeInfo` objet en cascade.

*Shared/CascadingValuesParametersLayout.cshtml*:

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

Pour rendre des valeurs en cascade, les composants déclarer des paramètres en cascade à l’aide de la `[CascadingParameter]` attribut ou selon une valeur de nom de chaîne :

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

Liaison avec une valeur de nom de chaîne s’applique si vous avez plusieurs valeurs en cascade du même type et que vous avez besoin pour les différencier dans le même sous-arbre.

Valeurs en cascade sont liés aux paramètres en cascade par type.

Dans l’exemple d’application, le composant de thème des paramètres de valeurs en cascade est lié à la `ThemeInfo` valeur en cascade à un paramètre en cascade. Le paramètre est utilisé pour définir la classe CSS pour un des boutons affichés par le composant.

*Pages/CascadingValuesParametersTheme.cshtml*:

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

### <a name="tabset-example"></a>Exemple de TabSet

Paramètres en cascade permettent également aux composants de collaborer au travers de la hiérarchie des composants. Par exemple, considérez les éléments suivants *TabSet* exemple dans l’exemple d’application.

L’exemple d’application a une `ITab` interface qui implémentent des onglets :

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

Le composant TabSet des paramètres de valeurs en cascade utilise le composant ensemble d’onglets, qui contient plusieurs composants d’onglet :

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

Les composants d’onglet enfants ne sont pas explicitement transmis en tant que paramètres à l’ensemble de l’onglet. Au lieu de cela, les composants d’onglet enfants font partie du contenu enfant de l’ensemble de l’onglet. Toutefois, l’ensemble de l’onglet doit tout savoir sur chaque composant de l’onglet afin qu’il puisse afficher les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter du code supplémentaire, le composant de l’ensemble d’onglets *peut fournir lui-même comme une valeur en cascade* qui est ensuite récupéré par les composants d’onglet descendants.

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

La capture de composants onglet descendante l’ensemble d’onglets contenant comme un paramètre en cascade, afin que les composants de l’onglet ajoutent eux-mêmes à l’ensemble d’onglets et les coordonnées sur l’onglet est actif.

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Modèles Razor

Restituer fragments peuvent être définies à l’aide de la syntaxe du modèle Razor. Les modèles Razor sont un moyen de définir un extrait de code de l’interface utilisateur et le format suivant :

```cshtml
@<tag>...</tag>
```

L’exemple suivant illustre comment spécifier `RenderFragment` et `RenderFragment<T>` valeurs.

*RazorTemplates.cshtml*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Restituer des fragments définis à l’aide de Razor modèles peuvent être passés comme arguments à des composants basés sur des modèles ou restitués directement. Par exemple, les modèles précédentes sont directement restitués avec le balisage Razor suivant :

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

Sortie rendue :

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a>Logique de RenderTreeBuilder manuelle

`Microsoft.AspNetCore.Components.RenderTree` Fournit des méthodes pour la manipulation des composants et des éléments, notamment la création de composants manuellement dans C# code.

> [!NOTE]
> Utilisation de `RenderTreeBuilder` pour créer des composants est un scénario avancé. Un composant incorrect (par exemple, une balise non fermés) peut entraîner un comportement non défini.

Envisagez le composant Pet détails suivant (*PetDetails.razor* dans Razor composants ; *PetDetails.cshtml* dans Blazor), qui peut être généré manuellement dans un autre composant :

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

Dans l’exemple suivant, la boucle dans le `CreateComponent` méthode génère trois composants Pet détails. Lors de l’appel `RenderTreeBuilder` méthodes pour créer les composants (`OpenComponent` et `AddAttribute`), les numéros de séquence sont les numéros de ligne de code source. Les composants de Razor algorithme de différence s’appuie sur les numéros de séquence correspondant à des lignes distinctes de code, pas distincte appeler les appels. Lors de la création d’un composant avec `RenderTreeBuilder` méthodes, coder en dur les arguments pour les numéros de séquence. **À l’aide d’un calcul ou un compteur pour générer le numéro de séquence peut entraîner une baisse des performances.**

Composant (*BuiltContent.razor* dans Razor composants ; *BuiltContent.cshtml* dans Blazor) :

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
