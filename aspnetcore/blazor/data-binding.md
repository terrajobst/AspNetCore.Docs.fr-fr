---
title: Liaison de données ASP.NET Core Blazor
author: guardrex
description: En savoir plus sur les fonctionnalités de liaison de données pour les composants et les éléments DOM dans Blazor applications.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: 5b49d2598a451ee607e034913bd1aeaa03f941c6
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511195"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a>Liaison de données ASP.NET Core Blazor

Par [Luke Latham](https://github.com/guardrex) et [Daniel Roth](https://github.com/danroth27)

Les composants Razor fournissent des fonctionnalités de liaison de données via un attribut d’élément HTML nommé [`@bind`](xref:mvc/views/razor#bind) avec un champ, une propriété ou une valeur d’expression Razor.

L’exemple suivant lie la propriété `CurrentValue` à la valeur de la zone de texte :

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

Liez une propriété ou un champ à d’autres événements en incluant également un attribut `@bind:event` avec un paramètre `event`. L’exemple suivant lie la propriété `CurrentValue` sur l’événement `oninput` :

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

Contrairement à `onchange`, qui se déclenche lorsque l’élément perd le focus, `oninput` se déclenche lorsque la valeur de la zone de texte change.

Utilisez `@bind-{ATTRIBUTE}` avec la syntaxe `@bind-{ATTRIBUTE}:event` pour lier des attributs d’élément autres que `value`. Dans l’exemple suivant, le style du paragraphe est mis à jour lorsque la valeur `_paragraphStyle` change :

```razor
@page "/binding-example"

<p>
    <input type="text" @bind="_paragraphStyle" />
</p>

<p @bind-style="_paragraphStyle" @bind-style:event="onchange">
    Blazorify the app!
</p>

@code {
    private string _paragraphStyle = "color:red";
}
```

La liaison d’attribut respecte la casse. Par exemple, `@bind` est valide et `@Bind` n’est pas valide.

## <a name="unparsable-values"></a>Valeurs inanalysables

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

Par défaut, la liaison s’applique à l’événement `onchange` de l’élément (`@bind="{PROPERTY OR FIELD}"`). Utilisez `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` pour déclencher la liaison sur un événement différent. Pour l’événement `oninput` (`@bind:event="oninput"`), la Reversion se produit après toute séquence de touches qui introduit une valeur non analysable. Quand vous ciblez l’événement `oninput` avec un type lié `int`, un utilisateur ne peut pas taper un caractère `.`. Si un caractère de `.` est immédiatement supprimé, l’utilisateur reçoit des commentaires immédiats que seuls des nombres entiers sont autorisés. Il existe des scénarios où le rétablissement de la valeur sur l’événement `oninput` n’est pas idéal, par exemple lorsque l’utilisateur doit être autorisé à effacer une valeur de `<input>` non analysable. Les alternatives sont les suivantes :

* N’utilisez pas l’événement `oninput`. Utilisez l’événement `onchange` par défaut (spécifiez uniquement `@bind="{PROPERTY OR FIELD}"`), où une valeur non valide n’est pas rétablie tant que l’élément perd le focus.
* Effectuer une liaison à un type Nullable, tel que `int?` ou `string`, et fournir une logique personnalisée pour gérer les entrées non valides.
* Utilisez un [composant de validation de formulaire](xref:blazor/forms-validation), tel que `InputNumber` ou `InputDate`. Les composants de validation de formulaire offrent une prise en charge intégrée pour gérer les entrées non valides. Composants de validation de formulaire :
  * Permet à l’utilisateur de fournir une entrée non valide et de recevoir des erreurs de validation sur les `EditContext`associées.
  * Affichez les erreurs de validation dans l’interface utilisateur sans interférer avec l’utilisateur qui saisit des données Webform supplémentaires.

## <a name="format-strings"></a>Chaînes de format

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

Il n’est pas recommandé de spécifier un format pour le type de champ `date`, car Blazor offre une prise en charge intégrée de la mise en forme des dates. Malgré la recommandation, utilisez uniquement le format de date `yyyy-MM-dd` pour que la liaison fonctionne correctement si un format est fourni avec le type de champ `date` :

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a>Liaison parent-enfant avec les paramètres de composant

La liaison reconnaît les paramètres du composant, où `@bind-{PROPERTY}` pouvez lier une valeur de propriété d’un composant parent à un composant enfant. La liaison d’un enfant à un parent est couverte dans la [liaison enfant-parent avec la section de liaison chaînée](#child-to-parent-binding-with-chained-bind) .

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

`EventCallback<T>` est expliqué dans <xref:blazor/event-handling#eventcallback>.

Le composant parent suivant utilise :

* `ChildComponent` et lie le paramètre `ParentYear` du parent au paramètre `Year` sur le composant enfant.
* L’événement `onclick` est utilisé pour déclencher la méthode `ChangeTheYear`. Pour plus d’informations, consultez <xref:blazor/event-handling>.

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

En général, une propriété peut être liée à un gestionnaire d’événements correspondant en incluant un attribut `@bind-{PROPRETY}:event`. Par exemple, la propriété `MyProp` peut être liée à `MyEventHandler` à l’aide des deux attributs suivants :

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a>Liaison enfant-parent avec liaison chaînée

Un scénario courant consiste à chaîner un paramètre lié aux données à un élément de page dans la sortie du composant. Ce scénario est appelé *liaison chaînée* car plusieurs niveaux de liaison se produisent simultanément.

Une liaison chaînée ne peut pas être implémentée avec `@bind` syntaxe dans l’élément de la page. Le gestionnaire d’événements et la valeur doivent être spécifiés séparément. Toutefois, un composant parent peut utiliser `@bind` syntaxe avec le paramètre du composant.

Le composant `PasswordField` suivant (*passwordField. Razor*) :

* Définit la valeur d’un élément `<input>` sur une propriété `Password`.
* Expose les modifications de la propriété `Password` à un composant parent avec un [EventCallback suivante](xref:blazor/event-handling#eventcallback).
* Utilise l’événement `onclick` est utilisé pour déclencher la méthode `ToggleShowPassword`. Pour plus d’informations, consultez <xref:blazor/event-handling>.

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

## <a name="radio-buttons"></a>Cases d’option

Pour plus d’informations sur la liaison à des cases d’option dans un formulaire, consultez <xref:blazor/forms-validation#work-with-radio-buttons>.
