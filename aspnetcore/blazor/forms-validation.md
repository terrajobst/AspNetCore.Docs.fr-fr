---
title: ASP.NET Core des formulaires et des validations éblouissants
author: guardrex
description: Découvrez comment utiliser des formulaires et des scénarios de validation de champ dans éblouissant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/forms-validation
ms.openlocfilehash: 6f6ace3deb7ed4262b643d897273bc767334b5e6
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207180"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>ASP.NET Core des formulaires et des validations éblouissants

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

Les formulaires et la validation sont pris en charge dans éblouissant à l’aide d' [Annotations de données](xref:mvc/models/validation).

Le type `ExampleModel` suivant définit la logique de validation à l’aide d’annotations de données :

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Un formulaire est défini à l' `EditForm` aide du composant. Le formulaire suivant illustre des éléments typiques, des composants et du code Razor :

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="@exampleModel.Name" />

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

* Le formulaire valide l’entrée utilisateur dans le `name` champ à l’aide de la validation `ExampleModel` définie dans le type. Le modèle est créé dans le bloc du `@code` composant et conservé dans un champ privé (`exampleModel`). Le champ est assigné à l' `Model` attribut de l' `<EditForm>` élément.
* Le `DataAnnotationsValidator` composant attache la prise en charge de la validation à l’aide d’annotations de données.
* Le `ValidationSummary` composant résume les messages de validation.
* `HandleValidSubmit`est déclenché quand le formulaire est envoyé avec succès (réussit la validation).

Un ensemble de composants d’entrée intégrés est disponible pour recevoir et valider les entrées utilisateur. Les entrées sont validées lorsqu’elles sont modifiées et lorsqu’un formulaire est envoyé. Les composants d’entrée disponibles sont répertoriés dans le tableau suivant.

| Composant d’entrée | Rendu comme&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

Tous les composants d’entrée, y `EditForm`compris, prennent en charge les attributs arbitraires. Tout attribut qui ne correspond pas à un paramètre de composant est ajouté à l’élément HTML rendu.

Les composants d’entrée fournissent le comportement par défaut pour la validation lors de la modification et la modification de leur classe CSS pour refléter l’état du champ. Certains composants incluent une logique d’analyse utile. Par exemple, `InputDate` et `InputNumber` gèrent correctement les valeurs non analysables en les inscrivant en tant qu’erreurs de validation. Les types qui peuvent accepter des valeurs NULL prennent également en charge la possibilité de valeur null du `int?`champ cible (par exemple,).

Le type `Starship` suivant définit la logique de validation à l’aide d’un plus grand ensemble de propriétés et `ExampleModel`d’annotations de données que la précédente :

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

La forme suivante valide l’entrée d’utilisateur à l’aide de la `Starship` validation définie dans le modèle :

```cshtml
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

Crée un `EditContext` comme valeur en [cascade](xref:blazor/components#cascading-values-and-parameters) qui effectue le suivi des métadonnées relatives au processus de modification, notamment les champs qui ont été modifiés et les messages de validation actuels. `EditForm` Le `EditForm` fournit également des événements pratiques pour les envois valides`OnValidSubmit`et `OnInvalidSubmit`non valides (,). Vous pouvez également utiliser `OnSubmit` pour déclencher la validation et vérifier les valeurs de champ avec un code de validation personnalisé.

## <a name="inputtext-based-on-the-input-event"></a>InputText en fonction de l’événement d’entrée

Utilisez le `InputText` composant pour créer un composant personnalisé qui utilise l' `input` événement au lieu de `change` l’événement.

Créez un composant avec le balisage suivant et utilisez le composant de la `InputText` même façon que :

```cshtml
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a>Prise en charge de la validation

Le `DataAnnotationsValidator` composant attache la prise en charge de la validation à l’aide d' `EditContext`annotations de données à la Cascaded. L’activation de la prise en charge de la validation à l’aide d’annotations de données requiert ce geste explicite. Pour utiliser un système de validation différent des annotations de données `DataAnnotationsValidator` , remplacez par une implémentation personnalisée. L’implémentation de ASP.NET Core est disponible pour l’inspection dans la source de référence : [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).

Le `ValidationSummary` composant résume tous les messages de validation, ce qui est similaire au [tag Helper de résumé de validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Le `ValidationMessage` composant affiche des messages de validation pour un champ spécifique, qui est similaire au [tag Helper de message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Spécifiez le champ pour la validation `For` avec l’attribut et une expression lambda qui nomme la propriété de modèle :

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

Les `ValidationMessage` composants `ValidationSummary` et prennent en charge des attributs arbitraires. Tout attribut qui ne correspond pas à un paramètre de composant est ajouté `<div>` à `<ul>` l’élément ou généré.

### <a name="validation-of-complex-or-collection-type-properties"></a>Validation des propriétés de type complexe ou de collection

Les attributs de validation appliqués aux propriétés d’un modèle valident le moment où le formulaire est envoyé. Toutefois, les propriétés des collections ou des types de données complexes d’un modèle ne sont pas validées lors de l’envoi du formulaire. Pour honorer les attributs de validation imbriqués dans ce scénario, utilisez un composant de validation personnalisé. Pour obtenir un exemple, consultez l' [exemple de validation éblouissant dans le référentiel GitHub ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).
