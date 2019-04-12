---
title: Validation et les formulaires de composants de razor
author: guardrex
description: Découvrez comment utiliser les formulaires et les scénarios de validation de champ dans les composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/forms-validation
ms.openlocfilehash: a4ed75efc6b5a733ce4ff4e82f354a8e2fd48500
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515541"
---
# <a name="razor-components-forms-and-validation"></a>Validation et les formulaires de composants de razor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

Formulaires et la validation sont pris en charge dans les composants de Razor à l’aide de [annotations de données](xref:mvc/models/validation).

> [!NOTE]
> Formulaires et les scénarios de validation sont susceptibles de changer avec chaque version d’évaluation.

Ce qui suit `ExampleModel` type définit la logique de validation à l’aide des annotations de données :

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

Un formulaire est défini à l’aide de la `<EditForm>` composant. Sous la forme suivante montre le code Razor, des composants et des éléments par défaut :

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* Le formulaire valide l’entrée d’utilisateur dans le `name` champ à l’aide de la validation définie dans le `ExampleModel` type. Le modèle est créé dans le composant `@functions` bloquer et maintenu dans un champ privé (`exampleModel`). Le champ est assigné à la `Model` attribut de la `<EditForm>`.
* Le composant de validateur d’Annotations de données (`<DataAnnotationsValidator>`) s’attache à l’aide des annotations de données de la prise en charge de la validation.
* Le composant de résumé des validations (`<ValidationSummary>`) Résumé des messages de validation.
* `HandleValidSubmit` se déclenche lorsque l’envoi du formulaire avec succès (validation passe).

Un ensemble de composants d’entrée intégrés sont disponibles pour recevoir et valider l’entrée utilisateur. Entrées sont validées lorsqu’elles sont modifiées et lorsqu’un formulaire est envoyé. Composants d’entrée disponibles sont affichés dans le tableau suivant.

| Composant d’entrée   | Rendu sous la forme&hellip;       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

Composants d’entrée fournissent le comportement par défaut pour la validation de la modification et la modification de leur classe CSS pour refléter l’état du champ. Certains composants incluent la logique d’analyse utile. Par exemple, `<InputDate>` et `<InputNumber>` gérer correctement les valeurs non analysable en les inscrivant en tant qu’erreurs de validation. Les types qui acceptent les valeurs null prennent également en charge la possibilité de valeur NULL du champ cible (par exemple, `int?`).

Ce qui suit `Starship` type définit la logique de validation à l’aide d’un plus grand jeu de propriétés et [annotations de données](xref:mvc/models/validation) à l’antérieur `ExampleModel`:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

Sous la forme suivante valide l’entrée d’utilisateur via la validation définie dans le `Starship` modèle :

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

Le `<EditForm>` crée un `EditContext` comme un [valeur en cascade](xref:razor-components/components#cascading-values-and-parameters) qui effectue le suivi des métadonnées sur le processus de modification, y compris les champs qui ont été modifiés et les messages de validation actuel. Le `<EditForm>` fournit également des événements pratiques pour valides et soumet (`OnValidSubmit`, `OnInvalidSubmit`). Vous pouvez également utiliser `OnSubmit` pour déclencher la validation et vérifier les valeurs de champ avec le code de validation personnalisé.

Le composant de validateur d’Annotations de données (`<DataAnnotationsValidator>`) attache la prise en charge de validation à l’aide des annotations de données à l’en cascade `EditContext`. L’activation de la prise en charge pour la validation à l’aide des annotations de données actuellement nécessite ce mouvement explicit, mais nous envisageons de ce qui en fait le comportement par défaut que vous pouvez ensuite substituer. Pour utiliser un système de validation différents que les annotations de données, remplacez le validateur d’Annotations de données avec une implémentation personnalisée. L’implémentation ASP.NET Core est disponible pour l’inspection de la source de référence : [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs). *L’implémentation d’ASP.NET Core est soumis à des mises à jour rapides pendant la période de version préliminaire.*

Le composant de résumé des validations (`<ValidationSummary>`) récapitule tous les messages de validation, qui est similaire à la [Tag Helper de résumé de Validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).

Le composant de Message de Validation (`<ValidationMessage>`) affiche les messages de validation pour un champ spécifique, ce qui est similaire à la [Tag Helper Validation Message](xref:mvc/views/working-with-forms#the-validation-message-tag-helper). Spécifiez le champ pour la validation avec le `For` attribut et une expression lambda qui nomme la propriété de modèle :

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> Les composants intégrés d’entrée ont des limitations qui devrait résoudre dans les futures versions. Par exemple, vous ne pouvez pas spécifier des attributs arbitraires sur généré `<input>` balises. Créez vos propres sous-classes de composant pour gérer les scénarios non disponibles.
