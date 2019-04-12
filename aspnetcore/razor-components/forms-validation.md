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
# <a name="razor-components-forms-and-validation"></a><span data-ttu-id="0fb4a-103">Validation et les formulaires de composants de razor</span><span class="sxs-lookup"><span data-stu-id="0fb4a-103">Razor Components forms and validation</span></span>

<span data-ttu-id="0fb4a-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0fb4a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0fb4a-105">Formulaires et la validation sont pris en charge dans les composants de Razor à l’aide de [annotations de données](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-105">Forms and validation are supported in Razor Components using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="0fb4a-106">Formulaires et les scénarios de validation sont susceptibles de changer avec chaque version d’évaluation.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="0fb4a-107">Ce qui suit `ExampleModel` type définit la logique de validation à l’aide des annotations de données :</span><span class="sxs-lookup"><span data-stu-id="0fb4a-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="0fb4a-108">Un formulaire est défini à l’aide de la `<EditForm>` composant.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="0fb4a-109">Sous la forme suivante montre le code Razor, des composants et des éléments par défaut :</span><span class="sxs-lookup"><span data-stu-id="0fb4a-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="0fb4a-110">Le formulaire valide l’entrée d’utilisateur dans le `name` champ à l’aide de la validation définie dans le `ExampleModel` type.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="0fb4a-111">Le modèle est créé dans le composant `@functions` bloquer et maintenu dans un champ privé (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-111">The model is created in the component's `@functions` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="0fb4a-112">Le champ est assigné à la `Model` attribut de la `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="0fb4a-113">Le composant de validateur d’Annotations de données (`<DataAnnotationsValidator>`) s’attache à l’aide des annotations de données de la prise en charge de la validation.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="0fb4a-114">Le composant de résumé des validations (`<ValidationSummary>`) Résumé des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* `HandleValidSubmit` <span data-ttu-id="0fb4a-115">se déclenche lorsque l’envoi du formulaire avec succès (validation passe).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-115">is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="0fb4a-116">Un ensemble de composants d’entrée intégrés sont disponibles pour recevoir et valider l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="0fb4a-117">Entrées sont validées lorsqu’elles sont modifiées et lorsqu’un formulaire est envoyé.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="0fb4a-118">Composants d’entrée disponibles sont affichés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="0fb4a-119">Composant d’entrée</span><span class="sxs-lookup"><span data-stu-id="0fb4a-119">Input component</span></span>   | <span data-ttu-id="0fb4a-120">Rendu sous la forme&hellip;</span><span class="sxs-lookup"><span data-stu-id="0fb4a-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="0fb4a-121">Composants d’entrée fournissent le comportement par défaut pour la validation de la modification et la modification de leur classe CSS pour refléter l’état du champ.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-121">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="0fb4a-122">Certains composants incluent la logique d’analyse utile.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-122">Some components include useful parsing logic.</span></span> <span data-ttu-id="0fb4a-123">Par exemple, `<InputDate>` et `<InputNumber>` gérer correctement les valeurs non analysable en les inscrivant en tant qu’erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-123">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="0fb4a-124">Les types qui acceptent les valeurs null prennent également en charge la possibilité de valeur NULL du champ cible (par exemple, `int?`).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-124">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="0fb4a-125">Ce qui suit `Starship` type définit la logique de validation à l’aide d’un plus grand jeu de propriétés et [annotations de données](xref:mvc/models/validation) à l’antérieur `ExampleModel`:</span><span class="sxs-lookup"><span data-stu-id="0fb4a-125">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="0fb4a-126">Sous la forme suivante valide l’entrée d’utilisateur via la validation définie dans le `Starship` modèle :</span><span class="sxs-lookup"><span data-stu-id="0fb4a-126">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="0fb4a-127">Le `<EditForm>` crée un `EditContext` comme un [valeur en cascade](xref:razor-components/components#cascading-values-and-parameters) qui effectue le suivi des métadonnées sur le processus de modification, y compris les champs qui ont été modifiés et les messages de validation actuel.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-127">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:razor-components/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="0fb4a-128">Le `<EditForm>` fournit également des événements pratiques pour valides et soumet (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-128">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="0fb4a-129">Vous pouvez également utiliser `OnSubmit` pour déclencher la validation et vérifier les valeurs de champ avec le code de validation personnalisé.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-129">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="0fb4a-130">Le composant de validateur d’Annotations de données (`<DataAnnotationsValidator>`) attache la prise en charge de validation à l’aide des annotations de données à l’en cascade `EditContext`.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-130">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="0fb4a-131">L’activation de la prise en charge pour la validation à l’aide des annotations de données actuellement nécessite ce mouvement explicit, mais nous envisageons de ce qui en fait le comportement par défaut que vous pouvez ensuite substituer.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-131">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="0fb4a-132">Pour utiliser un système de validation différents que les annotations de données, remplacez le validateur d’Annotations de données avec une implémentation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-132">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="0fb4a-133">L’implémentation ASP.NET Core est disponible pour l’inspection de la source de référence : [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-133">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> *<span data-ttu-id="0fb4a-134">L’implémentation d’ASP.NET Core est soumis à des mises à jour rapides pendant la période de version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-134">The ASP.NET Core implementation is subject to rapid updates during the preview release period.</span></span>*

<span data-ttu-id="0fb4a-135">Le composant de résumé des validations (`<ValidationSummary>`) récapitule tous les messages de validation, qui est similaire à la [Tag Helper de résumé de Validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-135">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="0fb4a-136">Le composant de Message de Validation (`<ValidationMessage>`) affiche les messages de validation pour un champ spécifique, ce qui est similaire à la [Tag Helper Validation Message](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0fb4a-136">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="0fb4a-137">Spécifiez le champ pour la validation avec le `For` attribut et une expression lambda qui nomme la propriété de modèle :</span><span class="sxs-lookup"><span data-stu-id="0fb4a-137">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> <span data-ttu-id="0fb4a-138">Les composants intégrés d’entrée ont des limitations qui devrait résoudre dans les futures versions.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-138">Built-in input components have limitations that we expect to resolve in future releases.</span></span> <span data-ttu-id="0fb4a-139">Par exemple, vous ne pouvez pas spécifier des attributs arbitraires sur généré `<input>` balises.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-139">For example, you can't specify arbitrary attributes on the generated `<input>` tags.</span></span> <span data-ttu-id="0fb4a-140">Créez vos propres sous-classes de composant pour gérer les scénarios non disponibles.</span><span class="sxs-lookup"><span data-stu-id="0fb4a-140">Build your own component subclasses to handle unavailable scenarios.</span></span>
