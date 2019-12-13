---
title: ASP.NET Core Blazor Forms et validation
author: guardrex
description: Découvrez comment utiliser les formulaires et les scénarios de validation de champ dans Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/forms-validation
ms.openlocfilehash: f4c1845ee4b6ff9274b7117167367ccdd9f36c12
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943691"
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

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* Le formulaire valide l’entrée utilisateur dans le champ `name` à l’aide de la validation définie dans le type de `ExampleModel`. Le modèle est créé dans le bloc `@code` du composant et conservé dans un champ privé (`exampleModel`). Le champ est assigné à l’attribut `Model` de l’élément `<EditForm>`.
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

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

L' `EditForm` crée une `EditContext` sous la forme d’une [valeur en cascade](xref:blazor/components#cascading-values-and-parameters) qui effectue le suivi des métadonnées relatives au processus de modification, notamment les champs qui ont été modifiés et les messages de validation actuels. Le `EditForm` fournit également des événements pratiques pour les envois valides et non valides (`OnValidSubmit`, `OnInvalidSubmit`). Vous pouvez également utiliser `OnSubmit` pour déclencher la validation et vérifier les valeurs de champ avec un code de validation personnalisé.

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

## <a name="validation-support"></a>Prise en charge de la validation

Le composant `DataAnnotationsValidator` attache la prise en charge de la validation à l’aide d’annotations de données à la `EditContext`en cascade. L’activation de la prise en charge de la validation à l’aide d’annotations de données requiert ce geste explicite. Pour utiliser un système de validation différent des annotations de données, remplacez le `DataAnnotationsValidator` par une implémentation personnalisée. L’implémentation de ASP.NET Core est disponible pour l’inspection dans la source de référence : [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

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
<ValidationSummary Model="@starship" />
```

Le composant `ValidationMessage` affiche des messages de validation pour un champ spécifique, qui est similaire au [tag Helper du message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Spécifiez le champ pour la validation avec l’attribut `For` et une expression lambda qui nomme la propriété de modèle :

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
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

::: moniker range=">= aspnetcore-3.1"

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor package de validation des annotations de données

[Microsoft. AspNetCore.Blazor. DataAnnotations. validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) est un package qui comble les lacunes de l’expérience de validation à l’aide du composant `DataAnnotationsValidator`. Le package est actuellement *expérimental*.

### <a name="compareproperty-attribute"></a>Attribut [CompareProperty]

Le <xref:System.ComponentModel.DataAnnotations.CompareAttribute> ne fonctionne pas correctement avec le composant `DataAnnotationsValidator`. [Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)Le package *expérimental* DataAnnotations. validation introduit un attribut de validation supplémentaire, `ComparePropertyAttribute`, qui contourne ces limitations. Dans une application Blazor, `[CompareProperty]` est un remplacement direct de l’attribut `[Compare]`. Pour plus d’informations, consultez [CompareAttribute ignoré avec OnValidSubmit EditForm (ASPNET/AspNetCore #10643)](https://github.com/aspnet/AspNetCore/issues/10643#issuecomment-543909748).

### <a name="nested-models-collection-types-and-complex-types"></a>Modèles imbriqués, types de collection et types complexes

Blazor prend en charge la validation de l’entrée de formulaire à l’aide d’annotations de données avec la `DataAnnotationsValidator`intégrée. Toutefois, le `DataAnnotationsValidator` valide uniquement les propriétés de niveau supérieur du modèle lié au formulaire qui ne sont pas des propriétés de type collection ou complexe.

Pour valider le graphique d’objets entier du modèle lié, y compris les propriétés de type collection et de type complexe, utilisez le `ObjectGraphDataAnnotationsValidator` fourni par le [Microsoft. AspNetCore.Blazorexpérimental. Package DataAnnotations. validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) :

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

### <a name="validation-of-complex-or-collection-type-properties"></a>Validation des propriétés de type complexe ou de collection

Les attributs de validation appliqués aux propriétés d’un modèle valident le moment où le formulaire est envoyé. Toutefois, les propriétés des collections ou des types de données complexes d’un modèle ne sont pas validées lors de l’envoi du formulaire par le composant `DataAnnotationsValidator`. Pour honorer les attributs de validation imbriqués dans ce scénario, utilisez un composant de validation personnalisé. Pour obtenir un exemple, consultez l' [exemple de ValidationBlazor (ASPNET/Samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).

::: moniker-end
