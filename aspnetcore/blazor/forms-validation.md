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
ms.openlocfilehash: a94a433f26e451bbadc73615e502e46d273f05c2
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828137"
---
# <a name="aspnet-core-opno-locblazor-forms-and-validation"></a><span data-ttu-id="9680a-103">ASP.NET Core Blazor Forms et validation</span><span class="sxs-lookup"><span data-stu-id="9680a-103">ASP.NET Core Blazor forms and validation</span></span>

<span data-ttu-id="9680a-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9680a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9680a-105">Les formulaires et la validation sont pris en charge dans Blazor à l’aide d' [Annotations de données](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="9680a-105">Forms and validation are supported in Blazor using [data annotations](xref:mvc/models/validation).</span></span>

<span data-ttu-id="9680a-106">Le type de `ExampleModel` suivant définit la logique de validation à l’aide d’annotations de données :</span><span class="sxs-lookup"><span data-stu-id="9680a-106">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="9680a-107">Un formulaire est défini à l’aide du composant `EditForm`.</span><span class="sxs-lookup"><span data-stu-id="9680a-107">A form is defined using the `EditForm` component.</span></span> <span data-ttu-id="9680a-108">Le formulaire suivant illustre des éléments typiques, des composants et du code Razor :</span><span class="sxs-lookup"><span data-stu-id="9680a-108">The following form demonstrates typical elements, components, and Razor code:</span></span>

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

* <span data-ttu-id="9680a-109">Le formulaire valide l’entrée utilisateur dans le champ `name` à l’aide de la validation définie dans le type de `ExampleModel`.</span><span class="sxs-lookup"><span data-stu-id="9680a-109">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="9680a-110">Le modèle est créé dans le bloc `@code` du composant et conservé dans un champ privé (`exampleModel`).</span><span class="sxs-lookup"><span data-stu-id="9680a-110">The model is created in the component's `@code` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="9680a-111">Le champ est assigné à l’attribut `Model` de l’élément `<EditForm>`.</span><span class="sxs-lookup"><span data-stu-id="9680a-111">The field is assigned to the `Model` attribute of the `<EditForm>` element.</span></span>
* <span data-ttu-id="9680a-112">Le composant `DataAnnotationsValidator` attache la prise en charge de la validation à l’aide d’annotations de données.</span><span class="sxs-lookup"><span data-stu-id="9680a-112">The `DataAnnotationsValidator` component attaches validation support using data annotations.</span></span>
* <span data-ttu-id="9680a-113">Le composant `ValidationSummary` résume les messages de validation.</span><span class="sxs-lookup"><span data-stu-id="9680a-113">The `ValidationSummary` component summarizes validation messages.</span></span>
* <span data-ttu-id="9680a-114">`HandleValidSubmit` est déclenchée lorsque le formulaire est envoyé avec succès (réussit la validation).</span><span class="sxs-lookup"><span data-stu-id="9680a-114">`HandleValidSubmit` is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="9680a-115">Un ensemble de composants d’entrée intégrés est disponible pour recevoir et valider les entrées utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9680a-115">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="9680a-116">Les entrées sont validées lorsqu’elles sont modifiées et lorsqu’un formulaire est envoyé.</span><span class="sxs-lookup"><span data-stu-id="9680a-116">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="9680a-117">Les composants d’entrée disponibles sont répertoriés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="9680a-117">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="9680a-118">Composant d’entrée</span><span class="sxs-lookup"><span data-stu-id="9680a-118">Input component</span></span> | <span data-ttu-id="9680a-119">Rendu en tant que&hellip;</span><span class="sxs-lookup"><span data-stu-id="9680a-119">Rendered as&hellip;</span></span>       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

<span data-ttu-id="9680a-120">Tous les composants d’entrée, y compris les `EditForm`, prennent en charge des attributs arbitraires.</span><span class="sxs-lookup"><span data-stu-id="9680a-120">All of the input components, including `EditForm`, support arbitrary attributes.</span></span> <span data-ttu-id="9680a-121">Tout attribut qui ne correspond pas à un paramètre de composant est ajouté à l’élément HTML rendu.</span><span class="sxs-lookup"><span data-stu-id="9680a-121">Any attribute that doesn't match a component parameter is added to the rendered HTML element.</span></span>

<span data-ttu-id="9680a-122">Les composants d’entrée fournissent le comportement par défaut pour la validation lors de la modification et la modification de leur classe CSS pour refléter l’état du champ.</span><span class="sxs-lookup"><span data-stu-id="9680a-122">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="9680a-123">Certains composants incluent une logique d’analyse utile.</span><span class="sxs-lookup"><span data-stu-id="9680a-123">Some components include useful parsing logic.</span></span> <span data-ttu-id="9680a-124">Par exemple, `InputDate` et `InputNumber` gérer correctement les valeurs non analysables en les inscrivant en tant qu’erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="9680a-124">For example, `InputDate` and `InputNumber` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="9680a-125">Les types qui peuvent accepter des valeurs NULL prennent également en charge la possibilité de valeur null du champ cible (par exemple, `int?`).</span><span class="sxs-lookup"><span data-stu-id="9680a-125">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="9680a-126">Le type de `Starship` suivant définit la logique de validation à l’aide d’un plus grand ensemble de propriétés et d’annotations de données que le `ExampleModel`précédent :</span><span class="sxs-lookup"><span data-stu-id="9680a-126">The following `Starship` type defines validation logic using a larger set of properties and data annotations than the earlier `ExampleModel`:</span></span>

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

<span data-ttu-id="9680a-127">Dans l’exemple précédent, `Description` est facultatif, car aucune annotation de données n’est présente.</span><span class="sxs-lookup"><span data-stu-id="9680a-127">In the preceding example, `Description` is optional because no data annotations are present.</span></span>

<span data-ttu-id="9680a-128">La forme suivante valide l’entrée d’utilisateur à l’aide de la validation définie dans le modèle de `Starship` :</span><span class="sxs-lookup"><span data-stu-id="9680a-128">The following form validates user input using the validation defined in the `Starship` model:</span></span>

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

<span data-ttu-id="9680a-129">L' `EditForm` crée une `EditContext` sous la forme d’une [valeur en cascade](xref:blazor/components#cascading-values-and-parameters) qui effectue le suivi des métadonnées relatives au processus de modification, notamment les champs qui ont été modifiés et les messages de validation actuels.</span><span class="sxs-lookup"><span data-stu-id="9680a-129">The `EditForm` creates an `EditContext` as a [cascading value](xref:blazor/components#cascading-values-and-parameters) that tracks metadata about the edit process, including which fields have been modified and the current validation messages.</span></span> <span data-ttu-id="9680a-130">Le `EditForm` fournit également des événements pratiques pour les envois valides et non valides (`OnValidSubmit`, `OnInvalidSubmit`).</span><span class="sxs-lookup"><span data-stu-id="9680a-130">The `EditForm` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="9680a-131">Vous pouvez également utiliser `OnSubmit` pour déclencher la validation et vérifier les valeurs de champ avec un code de validation personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9680a-131">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

## <a name="inputtext-based-on-the-input-event"></a><span data-ttu-id="9680a-132">InputText en fonction de l’événement d’entrée</span><span class="sxs-lookup"><span data-stu-id="9680a-132">InputText based on the input event</span></span>

<span data-ttu-id="9680a-133">Utilisez le composant `InputText` pour créer un composant personnalisé qui utilise l’événement `input` à la place de l’événement `change`.</span><span class="sxs-lookup"><span data-stu-id="9680a-133">Use the `InputText` component to create a custom component that uses the `input` event instead of the `change` event.</span></span>

<span data-ttu-id="9680a-134">Créez un composant avec le balisage suivant et utilisez le composant tout comme `InputText` est utilisé :</span><span class="sxs-lookup"><span data-stu-id="9680a-134">Create a component with the following markup, and use the component just as `InputText` is used:</span></span>

```razor
@inherits InputText

<input 
    @attributes="AdditionalAttributes" 
    class="@CssClass" 
    value="@CurrentValue" 
    @oninput="EventCallback.Factory.CreateBinder<string>(
        this, __value => CurrentValueAsString = __value, CurrentValueAsString)" />
```

## <a name="validation-support"></a><span data-ttu-id="9680a-135">Prise en charge de la validation</span><span class="sxs-lookup"><span data-stu-id="9680a-135">Validation support</span></span>

<span data-ttu-id="9680a-136">Le composant `DataAnnotationsValidator` attache la prise en charge de la validation à l’aide d’annotations de données à la `EditContext`en cascade.</span><span class="sxs-lookup"><span data-stu-id="9680a-136">The `DataAnnotationsValidator` component attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="9680a-137">L’activation de la prise en charge de la validation à l’aide d’annotations de données requiert ce geste explicite.</span><span class="sxs-lookup"><span data-stu-id="9680a-137">Enabling support for validation using data annotations requires this explicit gesture.</span></span> <span data-ttu-id="9680a-138">Pour utiliser un système de validation différent des annotations de données, remplacez le `DataAnnotationsValidator` par une implémentation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="9680a-138">To use a different validation system than data annotations, replace the `DataAnnotationsValidator` with a custom implementation.</span></span> <span data-ttu-id="9680a-139">L’implémentation de ASP.NET Core est disponible pour l’inspection dans la source de référence : [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="9680a-139">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/dotnet/AspNetCore/blob/master/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs).</span></span>

Blazor<span data-ttu-id="9680a-140"> effectue deux types de validation :</span><span class="sxs-lookup"><span data-stu-id="9680a-140"> performs two types of validation:</span></span>

* <span data-ttu-id="9680a-141">La *validation de champ* est effectuée lorsque l’utilisateur quitte un champ.</span><span class="sxs-lookup"><span data-stu-id="9680a-141">*Field validation* is performed when the user tabs out of a field.</span></span> <span data-ttu-id="9680a-142">Pendant la validation de champ, le composant `DataAnnotationsValidator` associe tous les résultats de validation signalés au champ.</span><span class="sxs-lookup"><span data-stu-id="9680a-142">During field validation, the `DataAnnotationsValidator` component associates all reported validation results with the field.</span></span>
* <span data-ttu-id="9680a-143">La *validation du modèle* est effectuée lorsque l’utilisateur envoie le formulaire.</span><span class="sxs-lookup"><span data-stu-id="9680a-143">*Model validation* is performed when the user submits the form.</span></span> <span data-ttu-id="9680a-144">Lors de la validation du modèle, le composant `DataAnnotationsValidator` tente de déterminer le champ en fonction du nom du membre que le résultat de validation signale.</span><span class="sxs-lookup"><span data-stu-id="9680a-144">During model validation, the `DataAnnotationsValidator` component attempts to determine the field based on the member name that the validation result reports.</span></span> <span data-ttu-id="9680a-145">Les résultats de validation qui ne sont pas associés à un membre individuel sont associés au modèle plutôt qu’à un champ.</span><span class="sxs-lookup"><span data-stu-id="9680a-145">Validation results that aren't associated with an individual member are associated with the model rather than a field.</span></span>

### <a name="validation-summary-and-validation-message-components"></a><span data-ttu-id="9680a-146">Résumé des validations et composants de message de validation</span><span class="sxs-lookup"><span data-stu-id="9680a-146">Validation Summary and Validation Message components</span></span>

<span data-ttu-id="9680a-147">Le composant `ValidationSummary` résume tous les messages de validation, ce qui est similaire au [tag Helper de résumé de validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="9680a-147">The `ValidationSummary` component summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper):</span></span>

```razor
<ValidationSummary />
```

<span data-ttu-id="9680a-148">Messages de validation de sortie pour un modèle spécifique avec le paramètre `Model` :</span><span class="sxs-lookup"><span data-stu-id="9680a-148">Output validation messages for a specific model with the `Model` parameter:</span></span>
  
```razor
<ValidationSummary Model="@starship" />
```

<span data-ttu-id="9680a-149">Le composant `ValidationMessage` affiche des messages de validation pour un champ spécifique, qui est similaire au [tag Helper du message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9680a-149">The `ValidationMessage` component displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="9680a-150">Spécifiez le champ pour la validation avec l’attribut `For` et une expression lambda qui nomme la propriété de modèle :</span><span class="sxs-lookup"><span data-stu-id="9680a-150">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```razor
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

<span data-ttu-id="9680a-151">Les composants `ValidationMessage` et `ValidationSummary` prennent en charge des attributs arbitraires.</span><span class="sxs-lookup"><span data-stu-id="9680a-151">The `ValidationMessage` and `ValidationSummary` components support arbitrary attributes.</span></span> <span data-ttu-id="9680a-152">Tout attribut qui ne correspond pas à un paramètre de composant est ajouté à l’élément `<div>` ou `<ul>` généré.</span><span class="sxs-lookup"><span data-stu-id="9680a-152">Any attribute that doesn't match a component parameter is added to the generated `<div>` or `<ul>` element.</span></span>

### <a name="custom-validation-attributes"></a><span data-ttu-id="9680a-153">Attributs de validation personnalisés</span><span class="sxs-lookup"><span data-stu-id="9680a-153">Custom validation attributes</span></span>

<span data-ttu-id="9680a-154">Pour vous assurer qu’un résultat de validation est correctement associé à un champ lors de l’utilisation d’un [attribut de validation personnalisé](xref:mvc/models/validation#custom-attributes), transmettez le <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> du contexte de validation lors de la création du <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span><span class="sxs-lookup"><span data-stu-id="9680a-154">To ensure that a validation result is correctly associated with a field when using a [custom validation attribute](xref:mvc/models/validation#custom-attributes), pass the validation context's <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName> when creating the <xref:System.ComponentModel.DataAnnotations.ValidationResult>:</span></span>

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

### <a name="opno-locblazor-data-annotations-validation-package"></a>Blazor<span data-ttu-id="9680a-155"> package de validation des annotations de données</span><span class="sxs-lookup"><span data-stu-id="9680a-155"> data annotations validation package</span></span>

<span data-ttu-id="9680a-156">[Microsoft. AspNetCore.Blazor. DataAnnotations. validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) est un package qui comble les lacunes de l’expérience de validation à l’aide du composant `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="9680a-156">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) is a package that fills validation experience gaps using the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="9680a-157">Le package est actuellement *expérimental*.</span><span class="sxs-lookup"><span data-stu-id="9680a-157">The package is currently *experimental*.</span></span>

### <a name="compareproperty-attribute"></a><span data-ttu-id="9680a-158">Attribut [CompareProperty]</span><span class="sxs-lookup"><span data-stu-id="9680a-158">[CompareProperty] attribute</span></span>

<span data-ttu-id="9680a-159">Le <xref:System.ComponentModel.DataAnnotations.CompareAttribute> ne fonctionne pas correctement avec le composant `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="9680a-159">The <xref:System.ComponentModel.DataAnnotations.CompareAttribute> doesn't work well with the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="9680a-160">[Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation)Le package *expérimental* DataAnnotations. validation introduit un attribut de validation supplémentaire, `ComparePropertyAttribute`, qui contourne ces limitations.</span><span class="sxs-lookup"><span data-stu-id="9680a-160">The [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) *experimental* package introduces an additional validation attribute, `ComparePropertyAttribute`, that works around these limitations.</span></span> <span data-ttu-id="9680a-161">Dans une application Blazor, `[CompareProperty]` est un remplacement direct de l’attribut `[Compare]`.</span><span class="sxs-lookup"><span data-stu-id="9680a-161">In a Blazor app, `[CompareProperty]` is a direct replacement for the `[Compare]` attribute.</span></span> <span data-ttu-id="9680a-162">Pour plus d’informations, consultez [CompareAttribute ignoré avec OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).</span><span class="sxs-lookup"><span data-stu-id="9680a-162">For more information, see [CompareAttribute ignored with OnValidSubmit EditForm (dotnet/AspNetCore #10643)](https://github.com/dotnet/AspNetCore/issues/10643#issuecomment-543909748).</span></span>

### <a name="nested-models-collection-types-and-complex-types"></a><span data-ttu-id="9680a-163">Modèles imbriqués, types de collection et types complexes</span><span class="sxs-lookup"><span data-stu-id="9680a-163">Nested models, collection types, and complex types</span></span>

Blazor<span data-ttu-id="9680a-164"> prend en charge la validation de l’entrée de formulaire à l’aide d’annotations de données avec la `DataAnnotationsValidator`intégrée.</span><span class="sxs-lookup"><span data-stu-id="9680a-164"> provides support for validating form input using data annotations with the built-in `DataAnnotationsValidator`.</span></span> <span data-ttu-id="9680a-165">Toutefois, le `DataAnnotationsValidator` valide uniquement les propriétés de niveau supérieur du modèle lié au formulaire qui ne sont pas des propriétés de type collection ou complexe.</span><span class="sxs-lookup"><span data-stu-id="9680a-165">However, the `DataAnnotationsValidator` only validates top-level properties of the model bound to the form that aren't collection- or complex-type properties.</span></span>

<span data-ttu-id="9680a-166">Pour valider le graphique d’objets entier du modèle lié, y compris les propriétés de type collection et de type complexe, utilisez le `ObjectGraphDataAnnotationsValidator` fourni par le [Microsoft. AspNetCore.Blazorexpérimental. Package DataAnnotations. validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) :</span><span class="sxs-lookup"><span data-stu-id="9680a-166">To validate the bound model's entire object graph, including collection- and complex-type properties, use the `ObjectGraphDataAnnotationsValidator` provided by the *experimental* [Microsoft.AspNetCore.Blazor.DataAnnotations.Validation](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.DataAnnotations.Validation) package:</span></span>

```razor
<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <ObjectGraphDataAnnotationsValidator />
    ...
</EditForm>
```

<span data-ttu-id="9680a-167">Annotez les propriétés de modèle avec `[ValidateComplexType]`.</span><span class="sxs-lookup"><span data-stu-id="9680a-167">Annotate model properties with `[ValidateComplexType]`.</span></span> <span data-ttu-id="9680a-168">Dans les classes de modèle suivantes, la classe `ShipDescription` contient des annotations de données supplémentaires à valider lorsque le modèle est lié au formulaire :</span><span class="sxs-lookup"><span data-stu-id="9680a-168">In the following model classes, the `ShipDescription` class contains additional data annotations to validate when the model is bound to the form:</span></span>

<span data-ttu-id="9680a-169">*Starship.cs*:</span><span class="sxs-lookup"><span data-stu-id="9680a-169">*Starship.cs*:</span></span>

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

<span data-ttu-id="9680a-170">*ShipDescription.cs*:</span><span class="sxs-lookup"><span data-stu-id="9680a-170">*ShipDescription.cs*:</span></span>

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

### <a name="validation-of-complex-or-collection-type-properties"></a><span data-ttu-id="9680a-171">Validation des propriétés de type complexe ou de collection</span><span class="sxs-lookup"><span data-stu-id="9680a-171">Validation of complex or collection type properties</span></span>

<span data-ttu-id="9680a-172">Les attributs de validation appliqués aux propriétés d’un modèle valident le moment où le formulaire est envoyé.</span><span class="sxs-lookup"><span data-stu-id="9680a-172">Validation attributes applied to the properties of a model validate when the form is submitted.</span></span> <span data-ttu-id="9680a-173">Toutefois, les propriétés des collections ou des types de données complexes d’un modèle ne sont pas validées lors de l’envoi du formulaire par le composant `DataAnnotationsValidator`.</span><span class="sxs-lookup"><span data-stu-id="9680a-173">However, the properties of collections or complex data types of a model aren't validated on form submission by the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="9680a-174">Pour honorer les attributs de validation imbriqués dans ce scénario, utilisez un composant de validation personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9680a-174">To honor the nested validation attributes in this scenario, use a custom validation component.</span></span> <span data-ttu-id="9680a-175">Pour obtenir un exemple, consultez l' [exemple de ValidationBlazor (ASPNET/Samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span><span class="sxs-lookup"><span data-stu-id="9680a-175">For an example, see the [Blazor Validation sample (aspnet/samples)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Validation).</span></span>

::: moniker-end
