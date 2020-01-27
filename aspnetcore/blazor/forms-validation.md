---
title: ASP.NET Core Blazor Forms et validation
author: guardrex
description: Découvrez comment utiliser les formulaires et les scénarios de validation de champ dans Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/forms-validation
ms.openlocfilehash: 2758bcbbc76c8a59716fe224dd2deb4ca8c06929
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726883"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a>ASP.NET Core Blazor Forms et validation

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

Les formulaires et la validation sont pris en charge dans Blazor à l’aide d' [Annotations de données](xref:mvc/models/validation).

Le type de `ExampleModel` suivant définit la logique de validation à l’aide d’annotations de données :

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Un formulaire est défini à l’aide du composant `EditForm`. Le formulaire suivant illustre des éléments typiques, des composants et du code Razor :

```razor
<EditForm Model="@_exampleModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="_exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel _exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

Dans l'exemple précédent :

* Le formulaire valide l’entrée utilisateur dans le champ `name` à l’aide de la validation définie dans le type de `ExampleModel`. Le modèle est créé dans le bloc `@code` du composant et conservé dans un champ privé (`_exampleModel`). Le champ est assigné à l’attribut `Model` de l’élément `<EditForm>`.
* Le `@bind-Value` du composant `InputText` lie :
  * Propriété de modèle (`_exampleModel.Name`) de la propriété `Value` du composant `InputText`.
  * Délégué d’événement de modification à la propriété `ValueChanged` du composant `InputText`.
* Le composant `DataAnnotationsValidator` attache la prise en charge de la validation à l’aide d’annotations de données.
* Le composant `ValidationSummary` résume les messages de validation.
* `HandleValidSubmit` est déclenchée lorsque le formulaire est envoyé avec succès (réussit la validation).

Un ensemble de composants d’entrée intégrés est disponible pour recevoir et valider les entrées utilisateur. Les entrées sont validées lorsqu’elles sont modifiées et lorsqu’un formulaire est envoyé. Les composants d’entrée disponibles sont répertoriés dans le tableau suivant.

| Composant d’entrée | Rendu en tant que&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Tous les composants d’entrée, y compris les `EditForm`, prennent en charge des attributs arbitraires. Tout attribut qui ne correspond pas à un paramètre de composant est ajouté à l’élément HTML rendu.

Les composants d’entrée fournissent le comportement par défaut pour la validation lors de la modification et la modification de leur classe CSS pour refléter l’état du champ. Certains composants incluent une logique d’analyse utile. Par exemple, `InputDate` et `InputNumber` gérer correctement les valeurs non analysables en les inscrivant en tant qu’erreurs de validation. Les types qui peuvent accepter des valeurs NULL prennent également en charge la possibilité de valeur null du champ cible (par exemple, `int?`).

Le type de `Starship` suivant définit la logique de validation à l’aide d’un plus grand ensemble de propriétés et d’annotations de données que le `ExampleModel`précédent :

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

Dans l’exemple précédent, `Description` est facultatif, car aucune annotation de données n’est présente.

La forme suivante valide l’entrée d’utilisateur à l’aide de la validation définie dans le modèle de `Starship` :

```razor
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@_starship" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label>
            Identifier:
            <InputText @bind-Value="_starship.Identifier" />
        </label>
    </p>
    <p>
        <label>
            Description (optional):
            <InputTextArea @bind-Value="_starship.Description" />
        </label>
    </p>
    <p>
        <label>
            Primary Classification:
            <InputSelect @bind-Value="_starship.Classification">
                <option value="">Select classification ...</option>
                <option value="Exploration">Exploration</option>
                <option value="Diplomacy">Diplomacy</option>
                <option value="Defense">Defense</option>
            </InputSelect>
        </label>
    </p>
    <p>
        <label>
            Maximum Accommodation:
            <InputNumber @bind-Value="_starship.MaximumAccommodation" />
        </label>
    </p>
    <p>
        <label>
            Engineering Approval:
            <InputCheckbox @bind-Value="_starship.IsValidatedDesign" />
        </label>
    </p>
    <p>
        <label>
            Production Date:
            <InputDate @bind-Value="_starship.ProductionDate" />
        </label>
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship _starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

L' `EditForm` crée une `EditContext` sous la forme d’une [valeur en cascade](xref:blazor/components#cascading-values-and-parameters) qui effectue le suivi des métadonnées relatives au processus de modification, notamment les champs qui ont été modifiés et les messages de validation actuels. Le `EditForm` fournit également des événements pratiques pour les envois valides et non valides (`OnValidSubmit`, `OnInvalidSubmit`). Vous pouvez également utiliser `OnSubmit` pour déclencher la validation et vérifier les valeurs de champ avec un code de validation personnalisé.

Dans l’exemple suivant :

* La méthode `HandleSubmit` s’exécute lorsque le bouton **Envoyer** est sélectionné.
* Le formulaire est validé à l’aide de la `EditContext`du formulaire.
* Le formulaire est validé en passant le `EditContext` à la méthode `ServerValidate` qui appelle un point de terminaison d’API Web sur le serveur (*non affiché*).
* Du code supplémentaire est exécuté en fonction du résultat de la validation côté client et côté serveur en vérifiant `isValid`.

```razor
<EditForm EditContext="@_editContext" OnSubmit="@HandleSubmit">

    ...

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship _starship = new Starship();
    private EditContext _editContext;

    protected override void OnInitialized()
    {
        _editContext = new EditContext(_starship);
    }

    private async Task HandleSubmit()
    {
        var isValid = _editContext.Validate() && 
            await ServerValidate(_editContext);

        if (isValid)
        {
            ...
        }
        else
        {
            ...
        }
    }

    private async Task<bool> ServerValidate(EditContext editContext)
    {
        var serverChecksValid = ...

        return serverChecksValid;
    }
}
```

## <a name="inputtext-based-on-the-input-event"></a>InputText en fonction de l’événement d’entrée

Utilisez le composant `InputText` pour créer un composant personnalisé qui utilise l’événement `input` à la place de l’événement `change`.

Créez un composant avec le balisage suivant et utilisez le composant tout comme `InputText` est utilisé :

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="work-with-radio-buttons"></a>Utiliser des cases d’option

Lorsque vous utilisez des cases d’option dans un formulaire, la liaison de données est gérée différemment des autres éléments, car les cases d’option sont évaluées en tant que groupe. La valeur de chaque case d’option est fixe, mais la valeur du groupe de cases d’option est la valeur de la case d’option sélectionnée. L’exemple suivant montre comment :

* Gérer la liaison de données pour un groupe de cases d’option.
* Prendre en charge la validation à l’aide d’un composant `InputRadio` personnalisé.

```razor
@using System.Globalization
@typeparam TValue
@inherits InputBase<TValue>

<input @attributes="AdditionalAttributes" type="radio" value="@SelectedValue" 
       checked="@(SelectedValue.Equals(Value))" @onchange="OnChange" />

@code {
    [Parameter]
    public TValue SelectedValue { get; set; }

    private void OnChange(ChangeEventArgs args)
    {
        CurrentValueAsString = args.Value.ToString();
    }

    protected override bool TryParseValueFromString(string value, 
        out TValue result, out string errorMessage)
    {
        var success = BindConverter.TryConvertTo<TValue>(
            value, CultureInfo.CurrentCulture, out var parsedValue);
        if (success)
        {
            result = parsedValue;
            errorMessage = null;

            return true;
        }
        else
        {
            result = default;
            errorMessage = $"{FieldIdentifier.FieldName} field isn't valid.";

            return false;
        }
    }
}
```

La `EditForm` suivante utilise le composant `InputRadio` précédent pour obtenir et valider une évaluation de l’utilisateur :

```razor
@page "/RadioButtonExample"
@using System.ComponentModel.DataAnnotations

<h1>Radio Button Group Test</h1>

<EditForm Model="_model" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    @for (int i = 1; i <= 5; i++)
    {
        <label>
            <InputRadio name="rate" SelectedValue="i" @bind-Value="_model.Rating" />
            @i
        </label>
    }

    <button type="submit">Submit</button>
</EditForm>

<p>You chose: @_model.Rating</p>

@code {
    private Model _model = new Model();

    private void HandleValidSubmit()
    {
        Console.WriteLine("valid");
    }

    public class Model
    {
        [Range(1, 5)]
        public int Rating { get; set; }
    }
}
```

## <a name="validation-support"></a>Prise en charge de la validation

Le composant `DataAnnotationsValidator` attache la prise en charge de la validation à l’aide d’annotations de données à la `EditContext`en cascade. L’activation de la prise en charge de la validation à l’aide d’annotations de données requiert ce geste explicite. Pour utiliser un système de validation différent des annotations de données, remplacez le `DataAnnotationsValidator` par une implémentation personnalisée. L’implémentation de ASP.NET Core est disponible pour l’inspection dans la source de référence : [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Blazor effectue deux types de validation :

* La *validation de champ* est effectuée lorsque l’utilisateur quitte un champ. Pendant la validation de champ, le composant `DataAnnotationsValidator` associe tous les résultats de validation signalés au champ.
* La *validation du modèle* est effectuée lorsque l’utilisateur envoie le formulaire. Lors de la validation du modèle, le composant `DataAnnotationsValidator` tente de déterminer le champ en fonction du nom du membre que le résultat de validation signale. Les résultats de validation qui ne sont pas associés à un membre individuel sont associés au modèle plutôt qu’à un champ.

### <a name="validation-summary-and-validation-message-components"></a>Résumé des validations et composants de message de validation

Le composant `ValidationSummary` résume tous les messages de validation, ce qui est similaire au [tag Helper de résumé de validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):

```razor
<ValidationSummary />
```

Messages de validation de sortie pour un modèle spécifique avec le paramètre `Model` :
  
```razor
<ValidationSummary Model="@_starship" />
```

Le composant `ValidationMessage` affiche des messages de validation pour un champ spécifique, qui est similaire au [tag Helper du message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Spécifiez le champ pour la validation avec l’attribut `For` et une expression lambda qui nomme la propriété de modèle :

```razor
<ValidationMessage For="@(() => _starship.MaximumAccommodation)" />
```

Les composants `ValidationMessage` et `ValidationSummary` prennent en charge des attributs arbitraires. Tout attribut qui ne correspond pas à un paramètre de composant est ajouté à l’élément `<div>` ou `<ul>` généré.

### <a name="custom-validation-attributes"></a>Attributs de validation personnalisés

Pour vous assurer qu’un résultat de validation est correctement associé à un champ lors de l’utilisation d’un [attribut de validation personnalisé](xref:mvc/models/validation#custom-attributes), transmettez le <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> du contexte de validation lors de la création du <xref:System.ComponentModel.DataAnnotations.ValidationResult>:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

private class MyCustomValidator : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, 
        ValidationContext validationContext)
    {
        ...

        return new ValidationResult("Validation message to user.",
            new[] { validationContext.MemberName });
    }
}
```

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor package de validation des annotations de données

[Microsoft. AspNetCore.Blazor. DataAnnotations. validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) est un package qui comble les lacunes de l’expérience de validation à l’aide du composant `DataAnnotationsValidator`. Le package est actuellement *expérimental*.

### <a name="compareproperty-attribute"></a>Attribut [CompareProperty]

Le <xref:System.ComponentModel.DataAnnotations.CompareAttribute> ne fonctionne pas correctement avec le composant `DataAnnotationsValidator`, car il n’associe pas le résultat de la validation à un membre spécifique. Cela peut entraîner un comportement incohérent entre la validation au niveau du champ et le moment où la totalité du modèle est validée sur une soumission. [Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)Le package *expérimental* DataAnnotations. validation introduit un attribut de validation supplémentaire, `ComparePropertyAttribute`, qui contourne ces limitations. Dans une application Blazor, `[CompareProperty]` est un remplacement direct de l’attribut `[Compare]`.

### <a name="nested-models-collection-types-and-complex-types"></a>Modèles imbriqués, types de collection et types complexes

Blazor prend en charge la validation de l’entrée de formulaire à l’aide d’annotations de données avec la `DataAnnotationsValidator`intégrée. Toutefois, le `DataAnnotationsValidator` valide uniquement les propriétés de niveau supérieur du modèle lié au formulaire qui ne sont pas des propriétés de type collection ou complexe.

Pour valider le graphique d’objets entier du modèle lié, y compris les propriétés de type collection et de type complexe, utilisez le `ObjectGraphDataAnnotationsValidator` fourni par le [Microsoft. AspNetCore.Blazorexpérimental. Package DataAnnotations. validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) :

```razor
<EditForm Model="@_model" OnValidSubmit="HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

Annotez les propriétés de modèle avec `[ValidateComplexType]`. Dans les classes de modèle suivantes, la classe `ShipDescription` contient des annotations de données supplémentaires à valider lorsque le modèle est lié au formulaire :

*Starship.cs*:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    ...

    [ValidateComplexType]
    public ShipDescription ShipDescription { get; set; }

    ...
}
```

*ShipDescription.cs*:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class ShipDescription
{
    [Required]
    [StringLength(40, ErrorMessage = "Description too long (40 char).")]
    public string ShortDescription { get; set; }
    
    [Required]
    [StringLength(240, ErrorMessage = "Description too long (240 char).")]
    public string LongDescription { get; set; }
}
```

### <a name="enable-the-submit-button-based-on-form-validation"></a>Activer le bouton Envoyer en fonction de la validation de formulaire

Pour activer et désactiver le bouton Envoyer en fonction de la validation de formulaire :

* Utilisez la `EditContext` du formulaire pour assigner le modèle lorsque le composant est initialisé.
* Validez le formulaire dans le rappel `OnFieldChanged` du contexte pour activer et désactiver le bouton Envoyer.

```razor
<EditForm EditContext="@_editContext">
    <DataAnnotationsValidator />
    <ValidationSummary />

    ...

    <button type="submit" disabled="@_formInvalid">Submit</button>
</EditForm>

@code {
    private Starship _starship = new Starship();
    private bool _formInvalid = true;
    private EditContext _editContext;

    protected override void OnInitialized()
    {
        _editContext = new EditContext(_starship);

        _editContext.OnFieldChanged += (_, __) =>
        {
            _formInvalid = !_editContext.Validate();
            StateHasChanged();
        };
    }
}
```

Dans l’exemple précédent, définissez `_formInvalid` sur `false` si :

* Le formulaire est préchargé avec des valeurs par défaut valides.
* Vous voulez que le bouton envoyer soit activé lors du chargement du formulaire.

L’un des effets secondaires de l’approche précédente est qu’un composant `ValidationSummary` est rempli avec des champs non valides une fois que l’utilisateur interagit avec un champ quelconque. Ce scénario peut être traité de l’une des manières suivantes :

* N’utilisez pas de `ValidationSummary` composant sur le formulaire.
* Rendre le composant `ValidationSummary` visible lorsque le bouton Envoyer est sélectionné (par exemple, dans une méthode `HandleValidSubmit`).

```razor
<EditForm EditContext="@_editContext" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary style="@_displaySummary" />

    ...

    <button type="submit" disabled="@_formInvalid">Submit</button>
</EditForm>

@code {
    private string _displaySummary = "display:none";

    ...

    private void HandleValidSubmit()
    {
        _displaySummary = "display:block";
    }
}
```
