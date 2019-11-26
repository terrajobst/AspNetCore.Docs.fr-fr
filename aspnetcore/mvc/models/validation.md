---
title: Validation de modèle dans ASP.NET Core MVC
author: rick-anderson
description: Découvrez plus d’informations sur la validation de modèle dans ASP.NET Core MVC et Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
uid: mvc/models/validation
ms.openlocfilehash: 19f71799e958e2761832c91cec6762a6d391d2b5
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317424"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="9956b-103">Validation de modèle dans ASP.NET Core MVC et Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9956b-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9956b-104">Par [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="9956b-104">By [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="9956b-105">Cet article explique comment valider l’entrée d’utilisateur dans une application ASP.NET Core MVC ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9956b-105">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="9956b-106">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9956b-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="9956b-107">État du modèle</span><span class="sxs-lookup"><span data-stu-id="9956b-107">Model state</span></span>

<span data-ttu-id="9956b-108">L’état du modèle représente les erreurs qui proviennent de deux sous-systèmes : liaison de modèle et validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-108">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="9956b-109">Les erreurs qui proviennent de la [liaison de modèle](model-binding.md) sont généralement des erreurs de conversion de données.</span><span class="sxs-lookup"><span data-stu-id="9956b-109">Errors that originate from [model binding](model-binding.md) are generally data conversion errors.</span></span> <span data-ttu-id="9956b-110">Par exemple, un « x » est entré dans un champ de type entier.</span><span class="sxs-lookup"><span data-stu-id="9956b-110">For example, an "x" is entered in an integer field.</span></span> <span data-ttu-id="9956b-111">La validation de modèle se produit après la liaison de modèle et signale des erreurs où les données ne sont pas conformes aux règles d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="9956b-111">Model validation occurs after model binding and reports errors where data doesn't conform to business rules.</span></span> <span data-ttu-id="9956b-112">Par exemple, un 0 est entré dans un champ qui attend une évaluation comprise entre 1 et 5.</span><span class="sxs-lookup"><span data-stu-id="9956b-112">For example, a 0 is entered in a field that expects a rating between 1 and 5.</span></span>

<span data-ttu-id="9956b-113">La liaison de modèle et la validation de modèle se produisent avant l’exécution d’une action de contrôleur ou d’une méthode de gestionnaire de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9956b-113">Both model binding and model validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="9956b-114">Pour les applications web, il incombe à l’application d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="9956b-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="9956b-115">En règle générale, les applications web réaffichent la page avec un message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="9956b-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

<span data-ttu-id="9956b-116">Les contrôleurs d’API web ne sont pas obligés de vérifier `ModelState.IsValid` s’ils ont l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="9956b-117">Dans ce cas, une réponse HTTP 400 automatique contenant les détails de l’erreur est retournée lorsque l’état du modèle n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-117">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="9956b-118">Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="9956b-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="9956b-119">Réexécuter la validation</span><span class="sxs-lookup"><span data-stu-id="9956b-119">Rerun validation</span></span>

<span data-ttu-id="9956b-120">La validation est automatique, mais vous souhaiterez peut-être la répéter manuellement.</span><span class="sxs-lookup"><span data-stu-id="9956b-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="9956b-121">Par exemple, vous pourriez calculer une valeur pour une propriété, et souhaiter réexécuter la validation après avoir affecté la valeur calculée comme valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="9956b-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="9956b-122">Pour réexécuter la validation, appelez la méthode `TryValidateModel`, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="9956b-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a><span data-ttu-id="9956b-123">Attributs de validation</span><span class="sxs-lookup"><span data-stu-id="9956b-123">Validation attributes</span></span>

<span data-ttu-id="9956b-124">Les attributs de validation vous permettent de spécifier des règles de validation pour des propriétés de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="9956b-125">L’exemple suivant de l’exemple d’application montre une classe de modèle annotée avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-125">The following example from the sample app shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="9956b-126">L’attribut `[ClassicMovie]` est un attribut de validation personnalisé, et les autres sont prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="9956b-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="9956b-127">Non affiché est `[ClassicMovieWithClientValidator]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-127">Not shown is `[ClassicMovieWithClientValidator]`.</span></span> <span data-ttu-id="9956b-128">`[ClassicMovieWithClientValidator]` illustre une autre façon d’implémenter un attribut personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-128">`[ClassicMovieWithClientValidator]` shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a><span data-ttu-id="9956b-129">Attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="9956b-129">Built-in attributes</span></span>

<span data-ttu-id="9956b-130">Voici certains des attributs de validation prédéfinis :</span><span class="sxs-lookup"><span data-stu-id="9956b-130">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="9956b-131">`[CreditCard]`: vérifie que la propriété a un format de carte de crédit.</span><span class="sxs-lookup"><span data-stu-id="9956b-131">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="9956b-132">`[Compare]`: valide que deux propriétés d’un modèle correspondent.</span><span class="sxs-lookup"><span data-stu-id="9956b-132">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="9956b-133">`[EmailAddress]`: valide que la propriété a un format d’e-mail.</span><span class="sxs-lookup"><span data-stu-id="9956b-133">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="9956b-134">`[Phone]`: valide que la propriété a un format de numéro de téléphone.</span><span class="sxs-lookup"><span data-stu-id="9956b-134">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="9956b-135">`[Range]`: valide que la valeur de la propriété est comprise dans une plage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-135">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="9956b-136">`[RegularExpression]`: valide le fait que la valeur de propriété corresponde à une expression régulière spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-136">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="9956b-137">`[Required]`: vérifie que le champ n’a pas la valeur null.</span><span class="sxs-lookup"><span data-stu-id="9956b-137">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="9956b-138">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Required]](#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="9956b-138">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="9956b-139">`[StringLength]`: valide le fait qu’une valeur de propriété de chaîne ne dépasse pas une limite de longueur spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-139">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="9956b-140">`[Url]`: valide que la propriété a un format d’URL.</span><span class="sxs-lookup"><span data-stu-id="9956b-140">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="9956b-141">`[Remote]`: valide l’entrée sur le client en appelant une méthode d’action sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9956b-141">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="9956b-142">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Remote]](#remote-attribute).</span><span class="sxs-lookup"><span data-stu-id="9956b-142">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="9956b-143">Vous trouverez la liste complète des attributs de validation dans l’espace de noms [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="9956b-143">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="9956b-144">Messages d'erreur</span><span class="sxs-lookup"><span data-stu-id="9956b-144">Error messages</span></span>

<span data-ttu-id="9956b-145">Les attributs de validation vous permettent de spécifier le message d’erreur à afficher pour l’entrée non valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-145">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="9956b-146">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-146">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="9956b-147">En interne, les attributs appellent `String.Format` avec un espace réservé pour le nom de champ et parfois d’autres espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="9956b-147">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="9956b-148">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-148">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="9956b-149">Appliqué à une propriété `Name`, le message d’erreur créé par le code précédent serait « Name length must be between 6 and 8 ».</span><span class="sxs-lookup"><span data-stu-id="9956b-149">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="9956b-150">Pour savoir quels paramètres sont passés à `String.Format` pour le message d’erreur d’un attribut particulier, consultez le [code source de DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="9956b-150">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="9956b-151">Attribut [Required]</span><span class="sxs-lookup"><span data-stu-id="9956b-151">[Required] attribute</span></span>

<span data-ttu-id="9956b-152">Par défaut, le système de validation traite les propriétés ou paramètres n’acceptant pas les valeurs Null comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-152">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="9956b-153">Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) comme `decimal` et `int` n’acceptent pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="9956b-153">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="9956b-154">Validation de [Required] sur le serveur</span><span class="sxs-lookup"><span data-stu-id="9956b-154">[Required] validation on the server</span></span>

<span data-ttu-id="9956b-155">Sur le serveur, une valeur obligatoire est considérée comme manquante si la propriété est Null.</span><span class="sxs-lookup"><span data-stu-id="9956b-155">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="9956b-156">Un champ qui n’accepte pas les valeurs NULL est toujours valide et le message d’erreur de l’attribut `[Required]` n’est jamais affiché.</span><span class="sxs-lookup"><span data-stu-id="9956b-156">A non-nullable field is always valid, and the `[Required]` attribute's error message is never displayed.</span></span>

<span data-ttu-id="9956b-157">Toutefois, la liaison de modèle pour une propriété n’acceptant pas les valeurs Null peut échouer, entraînant l’affichage d’un message d’erreur tel que `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="9956b-157">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="9956b-158">Pour spécifier un message d’erreur personnalisé pour la validation côté serveur des types n’acceptant pas les valeurs Null, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="9956b-158">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="9956b-159">Rendre le champ Nullable (par exemple, `decimal?` au lieu de `decimal`).</span><span class="sxs-lookup"><span data-stu-id="9956b-159">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="9956b-160">Les types valeur [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) sont traités comme des types Nullable standard.</span><span class="sxs-lookup"><span data-stu-id="9956b-160">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="9956b-161">Spécifier le message d’erreur par défaut devant être utilisé par la liaison de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-161">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  <span data-ttu-id="9956b-162">Pour plus d’informations sur les erreurs de liaison de modèle pour lesquelles vous pouvez définir des messages par défaut, consultez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="9956b-162">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="9956b-163">Validation de [Required] sur le client</span><span class="sxs-lookup"><span data-stu-id="9956b-163">[Required] validation on the client</span></span>

<span data-ttu-id="9956b-164">Les chaînes et les types n’acceptant pas les valeurs Null sont gérés différemment sur le client et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9956b-164">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="9956b-165">Sur le client :</span><span class="sxs-lookup"><span data-stu-id="9956b-165">On the client:</span></span>

* <span data-ttu-id="9956b-166">Une valeur est considérée comme présente uniquement si une entrée est tapée pour celle-ci.</span><span class="sxs-lookup"><span data-stu-id="9956b-166">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="9956b-167">Par conséquent, la validation côté client gère les types n’acceptant pas les valeurs Null de la même façon que les types Nullable</span><span class="sxs-lookup"><span data-stu-id="9956b-167">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="9956b-168">Un espace blanc dans un champ de chaîne est considéré comme une entrée valide par la méthode [required](https://jqueryvalidation.org/required-method/) jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="9956b-168">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="9956b-169">La validation côté serveur considère qu’un champ de chaîne obligatoire est non valide si seul un espace blanc est entré.</span><span class="sxs-lookup"><span data-stu-id="9956b-169">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="9956b-170">Comme indiqué précédemment, les types n’acceptant pas les valeurs Null sont traités comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-170">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="9956b-171">Cela signifie que vous bénéficiez d’une validation côté client même si vous n’appliquez pas l’attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-171">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="9956b-172">En revanche, si vous n’utilisez pas l’attribut, vous recevez un message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="9956b-172">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="9956b-173">Pour spécifier un message d’erreur personnalisé, utilisez l’attribut.</span><span class="sxs-lookup"><span data-stu-id="9956b-173">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="9956b-174">Attribut [Remote]</span><span class="sxs-lookup"><span data-stu-id="9956b-174">[Remote] attribute</span></span>

<span data-ttu-id="9956b-175">L’attribut `[Remote]` implémente la validation côté client qui nécessite l’appel d’une méthode sur le serveur pour déterminer si le champ d’entrée est valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-175">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="9956b-176">Par exemple, l’application peut avoir besoin de vérifier si un nom d’utilisateur est déjà en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="9956b-176">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="9956b-177">Pour implémenter la validation à distance</span><span class="sxs-lookup"><span data-stu-id="9956b-177">To implement remote validation:</span></span>

1. <span data-ttu-id="9956b-178">Créez une méthode d’action devant être appelée par JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9956b-178">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="9956b-179">La méthode [remote](https://jqueryvalidation.org/remote-method/) jQuery Validate attend une réponse JSON :</span><span class="sxs-lookup"><span data-stu-id="9956b-179">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="9956b-180">`true` signifie que les données d’entrée sont valides.</span><span class="sxs-lookup"><span data-stu-id="9956b-180">`true` means the input data is valid.</span></span>
   * <span data-ttu-id="9956b-181">`false`, `undefined` ou `null` signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-181">`false`, `undefined`, or `null` means the input is invalid.</span></span> <span data-ttu-id="9956b-182">Affichez le message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="9956b-182">Display the default error message.</span></span>
   * <span data-ttu-id="9956b-183">Toute autre chaîne signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-183">Any other string means the input is invalid.</span></span> <span data-ttu-id="9956b-184">Affichez la chaîne en tant que message d’erreur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-184">Display the string as a custom error message.</span></span>

   <span data-ttu-id="9956b-185">Voici un exemple de méthode d’action qui retourne un message d’erreur personnalisé :</span><span class="sxs-lookup"><span data-stu-id="9956b-185">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="9956b-186">Dans la classe de modèle, annotez la propriété avec un attribut `[Remote]` qui pointe vers la méthode d’action de validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-186">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   <span data-ttu-id="9956b-187">L’attribut `[Remote]` se trouve dans l’espace de noms `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="9956b-187">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="9956b-188">Champs supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9956b-188">Additional fields</span></span>

<span data-ttu-id="9956b-189">La propriété `AdditionalFields` de l’attribut `[Remote]` vous permet de valider des combinaisons de champs relativement à des données présentes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9956b-189">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="9956b-190">Par exemple, si le modèle `User` a des propriétés `FirstName` et `LastName`, vous pouvez vérifier qu’aucun utilisateur existant n’a déjà cette paire de noms.</span><span class="sxs-lookup"><span data-stu-id="9956b-190">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="9956b-191">L'exemple suivant montre comment utiliser `AdditionalFields` :</span><span class="sxs-lookup"><span data-stu-id="9956b-191">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

<span data-ttu-id="9956b-192">`AdditionalFields` peut être défini explicitement avec les chaînes `"FirstName"` et `"LastName"`, mais l’utilisation de l’opérateur [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) simplifie la refactorisation ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9956b-192">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="9956b-193">La méthode d’action pour cette validation doit accepter les arguments `firstName` et `lastName` :</span><span class="sxs-lookup"><span data-stu-id="9956b-193">The action method for this validation must accept both `firstName` and `lastName` arguments:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="9956b-194">Quand l’utilisateur entre un nom ou un prénom, JavaScript effectue un appel à distance pour vérifier si cette paire de noms est déjà utilisée.</span><span class="sxs-lookup"><span data-stu-id="9956b-194">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="9956b-195">Pour valider deux champs supplémentaires ou plus, spécifiez-les sous la forme d’une liste délimitée par des virgules.</span><span class="sxs-lookup"><span data-stu-id="9956b-195">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="9956b-196">Par exemple, pour ajouter une propriété `MiddleName` au modèle, définissez l’attribut `[Remote]` comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-196">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="9956b-197">`AdditionalFields`, comme tous les arguments d’attribut, doit être une expression constante.</span><span class="sxs-lookup"><span data-stu-id="9956b-197">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="9956b-198">Vous ne devez donc pas utiliser une [chaîne interpolée](/dotnet/csharp/language-reference/keywords/interpolated-strings) ou appeler <xref:System.String.Join*> pour initialiser `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="9956b-198">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="9956b-199">Alternatives aux attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="9956b-199">Alternatives to built-in attributes</span></span>

<span data-ttu-id="9956b-200">Si vous avez besoin d’une validation non fournie par les attributs prédéfinis, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="9956b-200">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="9956b-201">[Créer des attributs personnalisés](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="9956b-201">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="9956b-202">[Implémenter IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="9956b-202">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="9956b-203">Attributs personnalisés</span><span class="sxs-lookup"><span data-stu-id="9956b-203">Custom attributes</span></span>

<span data-ttu-id="9956b-204">Pour les scénarios non gérés par les attributs de validation prédéfinis, vous pouvez créer des attributs de validation personnalisés.</span><span class="sxs-lookup"><span data-stu-id="9956b-204">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="9956b-205">Créez une classe qui hérite de <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> et substituez la méthode <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="9956b-205">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="9956b-206">La méthode `IsValid` accepte un objet nommé *value*, qui est l’entrée à valider.</span><span class="sxs-lookup"><span data-stu-id="9956b-206">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="9956b-207">Une surcharge accepte également un objet `ValidationContext`, qui fournit des informations supplémentaires telles que l’instance de modèle créée par la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-207">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="9956b-208">L’exemple suivant vérifie que la date de sortie d’un film appartenant au genre *Classic* n’est pas ultérieure à une année spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-208">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="9956b-209">L’attribut `[ClassicMovie]` :</span><span class="sxs-lookup"><span data-stu-id="9956b-209">The `[ClassicMovie]` attribute:</span></span>

* <span data-ttu-id="9956b-210">S’exécute uniquement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9956b-210">Is only run on the server.</span></span>
* <span data-ttu-id="9956b-211">Pour les films classiques, valide la date de publication :</span><span class="sxs-lookup"><span data-stu-id="9956b-211">For Classic movies, validates the release date:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

<span data-ttu-id="9956b-212">La variable `movie` de l’exemple précédent représente un objet `Movie` qui contient les données de l’envoi du formulaire.</span><span class="sxs-lookup"><span data-stu-id="9956b-212">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="9956b-213">Quand la validation échoue, un `ValidationResult` avec un message d’erreur est retourné.</span><span class="sxs-lookup"><span data-stu-id="9956b-213">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="9956b-214">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="9956b-214">IValidatableObject</span></span>

<span data-ttu-id="9956b-215">L’exemple précédent fonctionne uniquement avec les types `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9956b-215">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="9956b-216">Une autre option de validation au niveau de la classe consiste à implémenter `IValidatableObject` dans la classe de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-216">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="9956b-217">Validation du nœud de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="9956b-217">Top-level node validation</span></span>

<span data-ttu-id="9956b-218">Les nœuds de niveau supérieur incluent les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9956b-218">Top-level nodes include:</span></span>

* <span data-ttu-id="9956b-219">Paramètres d’action</span><span class="sxs-lookup"><span data-stu-id="9956b-219">Action parameters</span></span>
* <span data-ttu-id="9956b-220">Propriétés du contrôleur</span><span class="sxs-lookup"><span data-stu-id="9956b-220">Controller properties</span></span>
* <span data-ttu-id="9956b-221">Paramètres du gestionnaire de page</span><span class="sxs-lookup"><span data-stu-id="9956b-221">Page handler parameters</span></span>
* <span data-ttu-id="9956b-222">Propriétés du modèle de page</span><span class="sxs-lookup"><span data-stu-id="9956b-222">Page model properties</span></span>

<span data-ttu-id="9956b-223">Les nœuds de niveau supérieur liés au modèle sont validés en plus de la validation des propriétés du modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-223">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="9956b-224">Dans l’exemple suivant tiré de l’exemple d’application, la méthode `VerifyPhone` utilise <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> pour valider le paramètre d’action `phone` :</span><span class="sxs-lookup"><span data-stu-id="9956b-224">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="9956b-225">Les nœuds de niveau supérieur peuvent utiliser <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-225">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="9956b-226">Dans l’exemple suivant de l’exemple d’application, la méthode `CheckAge` spécifie que le paramètre `age` doit être lié à partir de la chaîne de requête au moment de l’envoi du formulaire :</span><span class="sxs-lookup"><span data-stu-id="9956b-226">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

<span data-ttu-id="9956b-227">Dans la page de vérification de l’âge (*CheckAge.cshtml*), il existe deux formulaires.</span><span class="sxs-lookup"><span data-stu-id="9956b-227">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="9956b-228">Le premier formulaire envoie une valeur `Age` de `99` sous la forme d’un paramètre de chaîne de requête : `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="9956b-228">The first form submits an `Age` value of `99` as a query string parameter: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="9956b-229">Quand un paramètre `age` au format approprié est envoyé à partir de la chaîne de requête, le formulaire est validé.</span><span class="sxs-lookup"><span data-stu-id="9956b-229">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="9956b-230">Le second formulaire de la page de vérification de l’âge envoie la valeur `Age` dans le corps de la requête, ce qui entraîne un échec de la validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-230">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="9956b-231">L’échec de la liaison est dû au fait que le paramètre `age` doit provenir d’une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="9956b-231">Binding fails because the `age` parameter must come from a query string.</span></span>

## <a name="maximum-errors"></a><span data-ttu-id="9956b-232">Quantité maximale d’erreurs</span><span class="sxs-lookup"><span data-stu-id="9956b-232">Maximum errors</span></span>

<span data-ttu-id="9956b-233">La validation s’arrête quand le nombre maximal d’erreurs est atteint (200 par défaut).</span><span class="sxs-lookup"><span data-stu-id="9956b-233">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="9956b-234">Vous pouvez configurer ce nombre avec le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="9956b-234">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a><span data-ttu-id="9956b-235">Récursivité maximale</span><span class="sxs-lookup"><span data-stu-id="9956b-235">Maximum recursion</span></span>

<span data-ttu-id="9956b-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> parcourt le graphe d’objet du modèle en cours de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="9956b-237">Pour les modèles en profondeur ou récursifs à l’infini, la validation peut entraîner un dépassement de capacité de la pile.</span><span class="sxs-lookup"><span data-stu-id="9956b-237">For models that are deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="9956b-238">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) fournit un moyen d’interrompre la validation de manière anticipée si la récursivité du visiteur dépasse une profondeur configurée.</span><span class="sxs-lookup"><span data-stu-id="9956b-238">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="9956b-239">La valeur par défaut de `MvcOptions.MaxValidationDepth` est 32.</span><span class="sxs-lookup"><span data-stu-id="9956b-239">The default value of `MvcOptions.MaxValidationDepth` is 32.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="9956b-240">Court-circuit automatique</span><span class="sxs-lookup"><span data-stu-id="9956b-240">Automatic short-circuit</span></span>

<span data-ttu-id="9956b-241">La validation est automatiquement court-circuitée (ignorée) si le graphe du modèle ne nécessite pas de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-241">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="9956b-242">Les objets pour lesquels le runtime ignore la validation comprennent les collections de primitives (telles que `byte[]`, `string[]`, `Dictionary<string, string>`) et les graphes d’objets complexes qui n’ont pas de validateur.</span><span class="sxs-lookup"><span data-stu-id="9956b-242">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="9956b-243">Désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="9956b-243">Disable validation</span></span>

<span data-ttu-id="9956b-244">Pour désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="9956b-244">To disable validation:</span></span>

1. <span data-ttu-id="9956b-245">Créez une implémentation de `IObjectModelValidator` qui ne marque aucun champ comme étant non valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-245">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. <span data-ttu-id="9956b-246">Ajoutez le code suivant à `Startup.ConfigureServices` pour remplacer l’implémentation par défaut de `IObjectModelValidator` dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9956b-246">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="9956b-247">Vous risquez toujours de voir des erreurs d’état du modèle provenant de la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-247">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="9956b-248">Validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-248">Client-side validation</span></span>

<span data-ttu-id="9956b-249">La validation côté client empêche l’envoi jusqu’à ce que le formulaire soit valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-249">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="9956b-250">Le bouton Submit exécute le code JavaScript qui envoie le formulaire ou qui affiche des messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="9956b-250">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="9956b-251">La validation côté client permet d’éviter un aller-retour inutile vers le serveur quand il existe des erreurs d’entrée sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="9956b-251">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="9956b-252">Les références de script suivantes dans *_Layout.cshtml* et *_ValidationScriptsPartial.cshtml* prennent en charge la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="9956b-252">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

<span data-ttu-id="9956b-253">Le script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) est une bibliothèque frontale personnalisée de Microsoft qui s’appuie sur le plug-in bien connu [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="9956b-253">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="9956b-254">Sans jQuery Unobtrusive Validation, vous devriez coder la même logique de validation à deux endroits : une fois dans les attributs de validation côté serveur sur les propriétés du modèle, puis à nouveau dans les scripts côté client.</span><span class="sxs-lookup"><span data-stu-id="9956b-254">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="9956b-255">Au lieu de cela, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les attributs de validation et les métadonnées de type des propriétés du modèle afin de restituer les attributs `data-` HTML 5 pour les éléments de formulaire nécessitant une validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-255">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="9956b-256">jQuery Unobtrusive Validation analyse les attributs `data-` et passe la logique à jQuery Validate, en « copiant » la logique de validation côté serveur vers le client.</span><span class="sxs-lookup"><span data-stu-id="9956b-256">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="9956b-257">Vous pouvez afficher les erreurs de validation sur le client en utilisant des Tag Helpers, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="9956b-257">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

<span data-ttu-id="9956b-258">Les balises d’assistance précédentes affichent le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-258">The preceding tag helpers render the following HTML:</span></span>

```html
<div class="form-group">
    <label class="control-label" for="Movie_ReleaseDate">Release Date</label>
    <input class="form-control" type="date" data-val="true"
        data-val-required="The Release Date field is required."
        id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
    <span class="text-danger field-validation-valid"
        data-valmsg-for="Movie.ReleaseDate" data-valmsg-replace="true"></span>
</div>
```

<span data-ttu-id="9956b-259">Notez que les attributs `data-` dans la sortie HTML correspondent aux attributs de validation pour la propriété `Movie.ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="9956b-259">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `Movie.ReleaseDate` property.</span></span> <span data-ttu-id="9956b-260">L’attribut `data-val-required` contient un message d’erreur à afficher si l’utilisateur ne renseigne pas le champ correspondant à la date de sortie.</span><span class="sxs-lookup"><span data-stu-id="9956b-260">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="9956b-261">jQuery Unobtrusive Validation passe cette valeur à la méthode [`required()`](https://jqueryvalidation.org/required-method/) de jQuery Validate, qui affiche alors ce message dans l’élément **\<span>** qui l’accompagne.</span><span class="sxs-lookup"><span data-stu-id="9956b-261">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="9956b-262">La validation du type de données est basée sur le type .NET d’une propriété, sauf en cas de substitution par un attribut `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-262">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="9956b-263">Les navigateurs ont leurs propres messages d’erreur par défaut, mais le package jQuery Validation Unobtrusive Validation peut remplacer ces messages.</span><span class="sxs-lookup"><span data-stu-id="9956b-263">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="9956b-264">Les attributs `[DataType]` et les sous-classes comme `[EmailAddress]` vous permettent de spécifier le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="9956b-264">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="9956b-265">Ajouter une validation à des formulaires dynamiques</span><span class="sxs-lookup"><span data-stu-id="9956b-265">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="9956b-266">jQuery Unobtrusive Validation passe la logique et les paramètres de validation à jQuery Validate lors du premier chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="9956b-266">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="9956b-267">Par conséquent, la validation ne fonctionne pas automatiquement sur les formulaires générés de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="9956b-267">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="9956b-268">Pour activer la validation, vous devez faire en sorte que jQuery Validate analyse le formulaire dynamique immédiatement après l’avoir créé.</span><span class="sxs-lookup"><span data-stu-id="9956b-268">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="9956b-269">Par exemple, le code suivant définit la validation côté client sur un formulaire ajouté par le biais d’AJAX.</span><span class="sxs-lookup"><span data-stu-id="9956b-269">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="9956b-270">La méthode `$.validator.unobtrusive.parse()` accepte un sélecteur jQuery comme argument.</span><span class="sxs-lookup"><span data-stu-id="9956b-270">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="9956b-271">Cette méthode indique à jQuery Unobtrusive Validation d’analyser les attributs `data-` des formulaires dans ce sélecteur.</span><span class="sxs-lookup"><span data-stu-id="9956b-271">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="9956b-272">Les valeurs de ces attributs sont ensuite passées au plug-in jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="9956b-272">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="9956b-273">Ajouter une validation à des contrôles dynamiques</span><span class="sxs-lookup"><span data-stu-id="9956b-273">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="9956b-274">La méthode `$.validator.unobtrusive.parse()` opère sur un formulaire entier, et non sur des contrôles individuels générés de manière dynamique tels que `<input>` et `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="9956b-274">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="9956b-275">Pour réanalyser le formulaire, supprimez les données de validation qui ont été ajoutées quand le formulaire a été analysé précédemment, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-275">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="9956b-276">Validation personnalisée côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-276">Custom client-side validation</span></span>

<span data-ttu-id="9956b-277">La validation personnalisée côté client s’effectue en générant des attributs HTML `data-` qui fonctionnent avec un adaptateur jQuery Validate personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-277">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="9956b-278">L’exemple de code d’adaptateur suivant a été écrit pour les attributs `[ClassicMovie]` et `[ClassicMovieWithClientValidator]` qui ont été introduits plus haut dans cet article :</span><span class="sxs-lookup"><span data-stu-id="9956b-278">The following sample adapter code was written for the `[ClassicMovie]` and `[ClassicMovieWithClientValidator]` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

<span data-ttu-id="9956b-279">Pour plus d’informations sur la façon d’écrire des adaptateurs, consultez la [documentation de jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="9956b-279">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="9956b-280">L’utilisation d’un adaptateur pour un champ donné est déclenchée par des attributs `data-` qui :</span><span class="sxs-lookup"><span data-stu-id="9956b-280">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="9956b-281">Marquent le champ comme étant soumis à validation (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="9956b-281">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="9956b-282">Identifient un nom de règle de validation et un texte de message d’erreur (par exemple, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="9956b-282">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="9956b-283">Fournissent les éventuels paramètres supplémentaires dont le validateur a besoin (par exemple, `data-val-rulename-param1="value"`).</span><span class="sxs-lookup"><span data-stu-id="9956b-283">Provide any additional parameters the validator needs (for example, `data-val-rulename-param1="value"`).</span></span>

<span data-ttu-id="9956b-284">L’exemple suivant montre les attributs `data-` pour l’attribut `ClassicMovie` de l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="9956b-284">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

<span data-ttu-id="9956b-285">Comme mentionné plus haut, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les informations des attributs de validation pour restituer les attributs `data-`.</span><span class="sxs-lookup"><span data-stu-id="9956b-285">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="9956b-286">Il existe deux options pour l’écriture de code qui entraîne la création d’attributs HTML `data-` personnalisés :</span><span class="sxs-lookup"><span data-stu-id="9956b-286">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="9956b-287">Créez une classe qui dérive de `AttributeAdapterBase<TAttribute>` et une classe qui implémente `IValidationAttributeAdapterProvider`, et inscrivez votre attribut et son adaptateur dans l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9956b-287">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="9956b-288">Cette méthode respecte le [principe de responsabilité unique](https://wikipedia.org/wiki/Single_responsibility_principle), dans la mesure où le code de validation lié au serveur et celui lié au client se trouvent dans des classes distinctes.</span><span class="sxs-lookup"><span data-stu-id="9956b-288">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="9956b-289">Il existe un autre avantage : l’adaptateur étant inscrit dans l’injection de dépendances, les autres services dans l’injection de dépendances lui sont accessibles si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="9956b-289">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="9956b-290">Implémentez `IClientModelValidator` dans votre classe `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9956b-290">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="9956b-291">Cette méthode peut être appropriée si l’attribut n’effectue aucune validation côté serveur et n’a besoin d’aucun service à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9956b-291">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="9956b-292">AttributeAdapter pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-292">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="9956b-293">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovie` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="9956b-293">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="9956b-294">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="9956b-294">To add client validation by using this method:</span></span>

1. <span data-ttu-id="9956b-295">Créez une classe d’adaptateurs d’attributs pour l’attribut de validation personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-295">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="9956b-296">Dérivez la classe de [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="9956b-296">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="9956b-297">Créez une méthode `AddValidation` qui ajoute des attributs `data-` à la sortie restituée, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-297">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. <span data-ttu-id="9956b-298">Créez une classe de fournisseurs d’adaptateurs qui implémente <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="9956b-298">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="9956b-299">Dans la méthode `GetAttributeAdapter`, passez l’attribut personnalisé au constructeur de l’adaptateur, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-299">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. <span data-ttu-id="9956b-300">Inscrivez le fournisseur d’adaptateurs auprès de l’injection de dépendances dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="9956b-300">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="9956b-301">IClientModelValidator pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-301">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="9956b-302">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovieWithClientValidator` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="9956b-302">This method of rendering `data-` attributes in HTML is used by the `ClassicMovieWithClientValidator` attribute in the sample app.</span></span> <span data-ttu-id="9956b-303">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="9956b-303">To add client validation by using this method:</span></span>

* <span data-ttu-id="9956b-304">Dans l’attribut de validation personnalisé, implémentez l’interface `IClientModelValidator` et créez une méthode `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="9956b-304">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="9956b-305">Dans la méthode `AddValidation`, ajoutez des attributs `data-` pour la validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-305">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="9956b-306">Désactiver la validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-306">Disable client-side validation</span></span>

<span data-ttu-id="9956b-307">Le code suivant désactive la validation du client dans Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="9956b-307">The following code disables client validation in Razor Pages:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

<span data-ttu-id="9956b-308">Autres options pour désactiver la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="9956b-308">Other options to disable client-side validation:</span></span>

* <span data-ttu-id="9956b-309">Commentez la référence à `_ValidationScriptsPartial` dans tous les fichiers *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="9956b-309">Comment out the reference to `_ValidationScriptsPartial` in all the *.cshtml* files.</span></span>
* <span data-ttu-id="9956b-310">Supprimez le contenu du fichier *Pages\Shared\_ValidationScriptsPartial. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="9956b-310">Remove the contents of the *Pages\Shared\_ValidationScriptsPartial.cshtml* file.</span></span>

<span data-ttu-id="9956b-311">L’approche précédente n’empêchera pas la validation côté client de ASP.NET Core bibliothèque de classes Razor d’identité.</span><span class="sxs-lookup"><span data-stu-id="9956b-311">The preceding approach won't prevent client side validation of ASP.NET Core Identity Razor Class Library.</span></span> <span data-ttu-id="9956b-312">Pour plus d'informations, consultez <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="9956b-312">For more information, see <xref:security/authentication/scaffold-identity>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9956b-313">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9956b-313">Additional resources</span></span>

* [<span data-ttu-id="9956b-314">Espace de noms System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="9956b-314">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="9956b-315">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="9956b-315">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9956b-316">Cet article explique comment valider l’entrée d’utilisateur dans une application ASP.NET Core MVC ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9956b-316">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="9956b-317">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9956b-317">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="9956b-318">État du modèle</span><span class="sxs-lookup"><span data-stu-id="9956b-318">Model state</span></span>

<span data-ttu-id="9956b-319">L’état du modèle représente les erreurs qui proviennent de deux sous-systèmes : liaison de modèle et validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-319">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="9956b-320">Les erreurs qui proviennent de la [liaison de modèle](model-binding.md) sont généralement des erreurs de conversion de données (par exemple, un « x » est entré dans un champ qui attend un nombre entier).</span><span class="sxs-lookup"><span data-stu-id="9956b-320">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="9956b-321">La validation de modèle se produit après la liaison de modèle, et signale les erreurs où les données ne sont pas conformes aux règles d’entreprise (par exemple, un 0 est entré dans un champ qui attend une évaluation comprise entre 1 et 5).</span><span class="sxs-lookup"><span data-stu-id="9956b-321">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="9956b-322">La liaison et la validation de modèle se produisent avant l’exécution d’une action de contrôleur ou d’une méthode de gestionnaire Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9956b-322">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="9956b-323">Pour les applications web, il incombe à l’application d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="9956b-323">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="9956b-324">En règle générale, les applications web réaffichent la page avec un message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="9956b-324">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="9956b-325">Les contrôleurs d’API web ne sont pas obligés de vérifier `ModelState.IsValid` s’ils ont l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-325">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="9956b-326">Dans ce cas, une réponse HTTP 400 automatique contenant les détails de l’erreur est retournée lorsque l’état du modèle n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-326">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="9956b-327">Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="9956b-327">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="9956b-328">Réexécuter la validation</span><span class="sxs-lookup"><span data-stu-id="9956b-328">Rerun validation</span></span>

<span data-ttu-id="9956b-329">La validation est automatique, mais vous souhaiterez peut-être la répéter manuellement.</span><span class="sxs-lookup"><span data-stu-id="9956b-329">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="9956b-330">Par exemple, vous pourriez calculer une valeur pour une propriété, et souhaiter réexécuter la validation après avoir affecté la valeur calculée comme valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="9956b-330">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="9956b-331">Pour réexécuter la validation, appelez la méthode `TryValidateModel`, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="9956b-331">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="9956b-332">Attributs de validation</span><span class="sxs-lookup"><span data-stu-id="9956b-332">Validation attributes</span></span>

<span data-ttu-id="9956b-333">Les attributs de validation vous permettent de spécifier des règles de validation pour des propriétés de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-333">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="9956b-334">L’exemple suivant tiré de l’[exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) montre une classe de modèle qui est annotée avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-334">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="9956b-335">L’attribut `[ClassicMovie]` est un attribut de validation personnalisé, et les autres sont prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="9956b-335">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="9956b-336">Non affiché est `[ClassicMovie2]`, qui illustre une autre façon d’implémenter un attribut personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-336">Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="9956b-337">Attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="9956b-337">Built-in attributes</span></span>

<span data-ttu-id="9956b-338">Les attributs de validation intégrés sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="9956b-338">Built-in validation attributes include:</span></span>

* <span data-ttu-id="9956b-339">`[CreditCard]`: vérifie que la propriété a un format de carte de crédit.</span><span class="sxs-lookup"><span data-stu-id="9956b-339">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="9956b-340">`[Compare]`: valide que deux propriétés d’un modèle correspondent.</span><span class="sxs-lookup"><span data-stu-id="9956b-340">`[Compare]`: Validates that two properties in a model match.</span></span> <span data-ttu-id="9956b-341">Par exemple, le fichier *Register.cshtml.cs* utilise `[Compare]` pour valider les deux correspondances de mots de passe entrées.</span><span class="sxs-lookup"><span data-stu-id="9956b-341">For example, the *Register.cshtml.cs* file uses `[Compare]` to validate the two entered passwords match.</span></span> <span data-ttu-id="9956b-342">L’identité de l' [échafaudage](xref:security/authentication/scaffold-identity) pour afficher le code du Registre.</span><span class="sxs-lookup"><span data-stu-id="9956b-342">[Scaffold Identity](xref:security/authentication/scaffold-identity) to see the Register code.</span></span>
* <span data-ttu-id="9956b-343">`[EmailAddress]`: valide que la propriété a un format d’e-mail.</span><span class="sxs-lookup"><span data-stu-id="9956b-343">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="9956b-344">`[Phone]`: valide que la propriété a un format de numéro de téléphone.</span><span class="sxs-lookup"><span data-stu-id="9956b-344">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="9956b-345">`[Range]`: valide que la valeur de la propriété est comprise dans une plage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-345">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="9956b-346">`[RegularExpression]`: valide le fait que la valeur de propriété corresponde à une expression régulière spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-346">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="9956b-347">`[Required]`: vérifie que le champ n’a pas la valeur null.</span><span class="sxs-lookup"><span data-stu-id="9956b-347">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="9956b-348">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Required]](#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="9956b-348">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="9956b-349">`[StringLength]`: valide le fait qu’une valeur de propriété de chaîne ne dépasse pas une limite de longueur spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-349">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="9956b-350">`[Url]`: valide que la propriété a un format d’URL.</span><span class="sxs-lookup"><span data-stu-id="9956b-350">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="9956b-351">`[Remote]`: valide l’entrée sur le client en appelant une méthode d’action sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9956b-351">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="9956b-352">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Remote]](#remote-attribute).</span><span class="sxs-lookup"><span data-stu-id="9956b-352">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="9956b-353">Vous trouverez la liste complète des attributs de validation dans l’espace de noms [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="9956b-353">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="9956b-354">Messages d'erreur</span><span class="sxs-lookup"><span data-stu-id="9956b-354">Error messages</span></span>

<span data-ttu-id="9956b-355">Les attributs de validation vous permettent de spécifier le message d’erreur à afficher pour l’entrée non valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-355">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="9956b-356">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-356">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="9956b-357">En interne, les attributs appellent `String.Format` avec un espace réservé pour le nom de champ et parfois d’autres espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="9956b-357">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="9956b-358">Exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-358">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="9956b-359">Appliqué à une propriété `Name`, le message d’erreur créé par le code précédent serait « Name length must be between 6 and 8 ».</span><span class="sxs-lookup"><span data-stu-id="9956b-359">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="9956b-360">Pour savoir quels paramètres sont passés à `String.Format` pour le message d’erreur d’un attribut particulier, consultez le [code source de DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="9956b-360">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="9956b-361">Attribut [Required]</span><span class="sxs-lookup"><span data-stu-id="9956b-361">[Required] attribute</span></span>

<span data-ttu-id="9956b-362">Par défaut, le système de validation traite les propriétés ou paramètres n’acceptant pas les valeurs Null comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-362">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="9956b-363">Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) comme `decimal` et `int` n’acceptent pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="9956b-363">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="9956b-364">Validation de [Required] sur le serveur</span><span class="sxs-lookup"><span data-stu-id="9956b-364">[Required] validation on the server</span></span>

<span data-ttu-id="9956b-365">Sur le serveur, une valeur obligatoire est considérée comme manquante si la propriété est Null.</span><span class="sxs-lookup"><span data-stu-id="9956b-365">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="9956b-366">Un champ n’acceptant pas les valeurs Null est toujours valide, et le message d’erreur de l’attribut [Required] n’est jamais affiché.</span><span class="sxs-lookup"><span data-stu-id="9956b-366">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="9956b-367">Toutefois, la liaison de modèle pour une propriété n’acceptant pas les valeurs Null peut échouer, entraînant l’affichage d’un message d’erreur tel que `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="9956b-367">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="9956b-368">Pour spécifier un message d’erreur personnalisé pour la validation côté serveur des types n’acceptant pas les valeurs Null, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="9956b-368">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="9956b-369">Rendre le champ Nullable (par exemple, `decimal?` au lieu de `decimal`).</span><span class="sxs-lookup"><span data-stu-id="9956b-369">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="9956b-370">Les types valeur [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) sont traités comme des types Nullable standard.</span><span class="sxs-lookup"><span data-stu-id="9956b-370">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="9956b-371">Spécifier le message d’erreur par défaut devant être utilisé par la liaison de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-371">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="9956b-372">Pour plus d’informations sur les erreurs de liaison de modèle pour lesquelles vous pouvez définir des messages par défaut, consultez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="9956b-372">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="9956b-373">Validation de [Required] sur le client</span><span class="sxs-lookup"><span data-stu-id="9956b-373">[Required] validation on the client</span></span>

<span data-ttu-id="9956b-374">Les chaînes et les types n’acceptant pas les valeurs Null sont gérés différemment sur le client et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9956b-374">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="9956b-375">Sur le client :</span><span class="sxs-lookup"><span data-stu-id="9956b-375">On the client:</span></span>

* <span data-ttu-id="9956b-376">Une valeur est considérée comme présente uniquement si une entrée est tapée pour celle-ci.</span><span class="sxs-lookup"><span data-stu-id="9956b-376">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="9956b-377">Par conséquent, la validation côté client gère les types n’acceptant pas les valeurs Null de la même façon que les types Nullable</span><span class="sxs-lookup"><span data-stu-id="9956b-377">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="9956b-378">Un espace blanc dans un champ de chaîne est considéré comme une entrée valide par la méthode [required](https://jqueryvalidation.org/required-method/) jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="9956b-378">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="9956b-379">La validation côté serveur considère qu’un champ de chaîne obligatoire est non valide si seul un espace blanc est entré.</span><span class="sxs-lookup"><span data-stu-id="9956b-379">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="9956b-380">Comme indiqué précédemment, les types n’acceptant pas les valeurs Null sont traités comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-380">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="9956b-381">Cela signifie que vous bénéficiez d’une validation côté client même si vous n’appliquez pas l’attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-381">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="9956b-382">En revanche, si vous n’utilisez pas l’attribut, vous recevez un message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="9956b-382">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="9956b-383">Pour spécifier un message d’erreur personnalisé, utilisez l’attribut.</span><span class="sxs-lookup"><span data-stu-id="9956b-383">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="9956b-384">Attribut [Remote]</span><span class="sxs-lookup"><span data-stu-id="9956b-384">[Remote] attribute</span></span>

<span data-ttu-id="9956b-385">L’attribut `[Remote]` implémente la validation côté client qui nécessite l’appel d’une méthode sur le serveur pour déterminer si le champ d’entrée est valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-385">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="9956b-386">Par exemple, l’application peut avoir besoin de vérifier si un nom d’utilisateur est déjà en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="9956b-386">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="9956b-387">Pour implémenter la validation à distance</span><span class="sxs-lookup"><span data-stu-id="9956b-387">To implement remote validation:</span></span>

1. <span data-ttu-id="9956b-388">Créez une méthode d’action devant être appelée par JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9956b-388">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="9956b-389">La méthode [remote](https://jqueryvalidation.org/remote-method/) jQuery Validate attend une réponse JSON :</span><span class="sxs-lookup"><span data-stu-id="9956b-389">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="9956b-390">`"true"` signifie que les données d’entrée sont valides.</span><span class="sxs-lookup"><span data-stu-id="9956b-390">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="9956b-391">`"false"`, `undefined` ou `null` signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-391">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="9956b-392">Affichez le message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="9956b-392">Display the default error message.</span></span>
   * <span data-ttu-id="9956b-393">Toute autre chaîne signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-393">Any other string means the input is invalid.</span></span> <span data-ttu-id="9956b-394">Affichez la chaîne en tant que message d’erreur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-394">Display the string as a custom error message.</span></span>

   <span data-ttu-id="9956b-395">Voici un exemple de méthode d’action qui retourne un message d’erreur personnalisé :</span><span class="sxs-lookup"><span data-stu-id="9956b-395">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="9956b-396">Dans la classe de modèle, annotez la propriété avec un attribut `[Remote]` qui pointe vers la méthode d’action de validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-396">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="9956b-397">L’attribut `[Remote]` se trouve dans l’espace de noms `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="9956b-397">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="9956b-398">Installez le package NuGet [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures), si vous n’utilisez pas le métapaquet `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="9956b-398">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="9956b-399">Champs supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9956b-399">Additional fields</span></span>

<span data-ttu-id="9956b-400">La propriété `AdditionalFields` de l’attribut `[Remote]` vous permet de valider des combinaisons de champs relativement à des données présentes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9956b-400">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="9956b-401">Par exemple, si le modèle `User` a des propriétés `FirstName` et `LastName`, vous pouvez vérifier qu’aucun utilisateur existant n’a déjà cette paire de noms.</span><span class="sxs-lookup"><span data-stu-id="9956b-401">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="9956b-402">L'exemple suivant montre comment utiliser `AdditionalFields` :</span><span class="sxs-lookup"><span data-stu-id="9956b-402">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="9956b-403">`AdditionalFields` peut être défini explicitement avec les chaînes `"FirstName"` et `"LastName"`, mais l’utilisation de l’opérateur [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) simplifie la refactorisation ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9956b-403">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="9956b-404">La méthode d’action pour cette validation doit accepter les arguments de nom et de prénom :</span><span class="sxs-lookup"><span data-stu-id="9956b-404">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="9956b-405">Quand l’utilisateur entre un nom ou un prénom, JavaScript effectue un appel à distance pour vérifier si cette paire de noms est déjà utilisée.</span><span class="sxs-lookup"><span data-stu-id="9956b-405">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="9956b-406">Pour valider deux champs supplémentaires ou plus, spécifiez-les sous la forme d’une liste délimitée par des virgules.</span><span class="sxs-lookup"><span data-stu-id="9956b-406">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="9956b-407">Par exemple, pour ajouter une propriété `MiddleName` au modèle, définissez l’attribut `[Remote]` comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-407">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="9956b-408">`AdditionalFields`, comme tous les arguments d’attribut, doit être une expression constante.</span><span class="sxs-lookup"><span data-stu-id="9956b-408">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="9956b-409">Vous ne devez donc pas utiliser une [chaîne interpolée](/dotnet/csharp/language-reference/keywords/interpolated-strings) ou appeler <xref:System.String.Join*> pour initialiser `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="9956b-409">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="9956b-410">Alternatives aux attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="9956b-410">Alternatives to built-in attributes</span></span>

<span data-ttu-id="9956b-411">Si vous avez besoin d’une validation non fournie par les attributs prédéfinis, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="9956b-411">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="9956b-412">[Créer des attributs personnalisés](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="9956b-412">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="9956b-413">[Implémenter IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="9956b-413">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="9956b-414">Attributs personnalisés</span><span class="sxs-lookup"><span data-stu-id="9956b-414">Custom attributes</span></span>

<span data-ttu-id="9956b-415">Pour les scénarios non gérés par les attributs de validation prédéfinis, vous pouvez créer des attributs de validation personnalisés.</span><span class="sxs-lookup"><span data-stu-id="9956b-415">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="9956b-416">Créez une classe qui hérite de <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> et substituez la méthode <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="9956b-416">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="9956b-417">La méthode `IsValid` accepte un objet nommé *value*, qui est l’entrée à valider.</span><span class="sxs-lookup"><span data-stu-id="9956b-417">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="9956b-418">Une surcharge accepte également un objet `ValidationContext`, qui fournit des informations supplémentaires telles que l’instance de modèle créée par la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-418">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="9956b-419">L’exemple suivant vérifie que la date de sortie d’un film appartenant au genre *Classic* n’est pas ultérieure à une année spécifiée.</span><span class="sxs-lookup"><span data-stu-id="9956b-419">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="9956b-420">L’attribut `[ClassicMovie2]` vérifie d’abord le genre, et continue uniquement s’il s’agit de *Classic*.</span><span class="sxs-lookup"><span data-stu-id="9956b-420">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="9956b-421">Pour les films identifiés comme des classiques, il vérifie si la date de sortie n’est pas ultérieure à la limite passée au constructeur d’attribut.</span><span class="sxs-lookup"><span data-stu-id="9956b-421">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="9956b-422">La variable `movie` de l’exemple précédent représente un objet `Movie` qui contient les données de l’envoi du formulaire.</span><span class="sxs-lookup"><span data-stu-id="9956b-422">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="9956b-423">La méthode `IsValid` vérifie la date et le genre.</span><span class="sxs-lookup"><span data-stu-id="9956b-423">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="9956b-424">Si la validation réussit, `IsValid` retourne un code `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="9956b-424">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="9956b-425">Quand la validation échoue, un `ValidationResult` avec un message d’erreur est retourné.</span><span class="sxs-lookup"><span data-stu-id="9956b-425">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="9956b-426">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="9956b-426">IValidatableObject</span></span>

<span data-ttu-id="9956b-427">L’exemple précédent fonctionne uniquement avec les types `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9956b-427">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="9956b-428">Une autre option de validation au niveau de la classe consiste à implémenter `IValidatableObject` dans la classe de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-428">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="9956b-429">Validation du nœud de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="9956b-429">Top-level node validation</span></span>

<span data-ttu-id="9956b-430">Les nœuds de niveau supérieur incluent les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9956b-430">Top-level nodes include:</span></span>

* <span data-ttu-id="9956b-431">Paramètres d’action</span><span class="sxs-lookup"><span data-stu-id="9956b-431">Action parameters</span></span>
* <span data-ttu-id="9956b-432">Propriétés du contrôleur</span><span class="sxs-lookup"><span data-stu-id="9956b-432">Controller properties</span></span>
* <span data-ttu-id="9956b-433">Paramètres du gestionnaire de page</span><span class="sxs-lookup"><span data-stu-id="9956b-433">Page handler parameters</span></span>
* <span data-ttu-id="9956b-434">Propriétés du modèle de page</span><span class="sxs-lookup"><span data-stu-id="9956b-434">Page model properties</span></span>

<span data-ttu-id="9956b-435">Les nœuds de niveau supérieur liés au modèle sont validés en plus de la validation des propriétés du modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-435">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="9956b-436">Dans l’exemple suivant tiré de l’exemple d’application, la méthode `VerifyPhone` utilise <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> pour valider le paramètre d’action `phone` :</span><span class="sxs-lookup"><span data-stu-id="9956b-436">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="9956b-437">Les nœuds de niveau supérieur peuvent utiliser <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-437">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="9956b-438">Dans l’exemple suivant de l’exemple d’application, la méthode `CheckAge` spécifie que le paramètre `age` doit être lié à partir de la chaîne de requête au moment de l’envoi du formulaire :</span><span class="sxs-lookup"><span data-stu-id="9956b-438">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="9956b-439">Dans la page de vérification de l’âge (*CheckAge.cshtml*), il existe deux formulaires.</span><span class="sxs-lookup"><span data-stu-id="9956b-439">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="9956b-440">Le premier formulaire envoie une valeur `Age` égale à `99` en tant que chaîne de requête : `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="9956b-440">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="9956b-441">Quand un paramètre `age` au format approprié est envoyé à partir de la chaîne de requête, le formulaire est validé.</span><span class="sxs-lookup"><span data-stu-id="9956b-441">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="9956b-442">Le second formulaire de la page de vérification de l’âge envoie la valeur `Age` dans le corps de la requête, ce qui entraîne un échec de la validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-442">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="9956b-443">L’échec de la liaison est dû au fait que le paramètre `age` doit provenir d’une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="9956b-443">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="9956b-444">Lors de l’exécution avec `CompatibilityVersion.Version_2_1` ou version ultérieure, la validation du nœud de niveau supérieur est activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="9956b-444">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="9956b-445">Autrement, la validation du nœud de niveau supérieur est désactivée.</span><span class="sxs-lookup"><span data-stu-id="9956b-445">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="9956b-446">L’option par défaut peut être remplacée en définissant la propriété <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> dans (`Startup.ConfigureServices`), comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="9956b-446">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="9956b-447">Quantité maximale d’erreurs</span><span class="sxs-lookup"><span data-stu-id="9956b-447">Maximum errors</span></span>

<span data-ttu-id="9956b-448">La validation s’arrête quand le nombre maximal d’erreurs est atteint (200 par défaut).</span><span class="sxs-lookup"><span data-stu-id="9956b-448">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="9956b-449">Vous pouvez configurer ce nombre avec le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="9956b-449">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="9956b-450">Récursivité maximale</span><span class="sxs-lookup"><span data-stu-id="9956b-450">Maximum recursion</span></span>

<span data-ttu-id="9956b-451"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> parcourt le graphe d’objet du modèle en cours de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-451"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="9956b-452">Pour les modèles très profonds ou infiniment récursifs, la validation peut entraîner un dépassement de la capacité de la pile.</span><span class="sxs-lookup"><span data-stu-id="9956b-452">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="9956b-453">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) fournit un moyen d’interrompre la validation de manière anticipée si la récursivité du visiteur dépasse une profondeur configurée.</span><span class="sxs-lookup"><span data-stu-id="9956b-453">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="9956b-454">La valeur par défaut de `MvcOptions.MaxValidationDepth` est 32 lors de l’exécution avec `CompatibilityVersion.Version_2_2` ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9956b-454">The default value of `MvcOptions.MaxValidationDepth` is 32 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="9956b-455">Pour les versions antérieures, la valeur est Null, ce qui signifie qu’il n’y a aucune contrainte de profondeur.</span><span class="sxs-lookup"><span data-stu-id="9956b-455">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="9956b-456">Court-circuit automatique</span><span class="sxs-lookup"><span data-stu-id="9956b-456">Automatic short-circuit</span></span>

<span data-ttu-id="9956b-457">La validation est automatiquement court-circuitée (ignorée) si le graphe du modèle ne nécessite pas de validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-457">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="9956b-458">Les objets pour lesquels le runtime ignore la validation comprennent les collections de primitives (telles que `byte[]`, `string[]`, `Dictionary<string, string>`) et les graphes d’objets complexes qui n’ont pas de validateur.</span><span class="sxs-lookup"><span data-stu-id="9956b-458">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="9956b-459">Désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="9956b-459">Disable validation</span></span>

<span data-ttu-id="9956b-460">Pour désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="9956b-460">To disable validation:</span></span>

1. <span data-ttu-id="9956b-461">Créez une implémentation de `IObjectModelValidator` qui ne marque aucun champ comme étant non valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-461">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="9956b-462">Ajoutez le code suivant à `Startup.ConfigureServices` pour remplacer l’implémentation par défaut de `IObjectModelValidator` dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9956b-462">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="9956b-463">Vous risquez toujours de voir des erreurs d’état du modèle provenant de la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="9956b-463">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="9956b-464">Validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-464">Client-side validation</span></span>

<span data-ttu-id="9956b-465">La validation côté client empêche l’envoi jusqu’à ce que le formulaire soit valide.</span><span class="sxs-lookup"><span data-stu-id="9956b-465">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="9956b-466">Le bouton Submit exécute le code JavaScript qui envoie le formulaire ou qui affiche des messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="9956b-466">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="9956b-467">La validation côté client permet d’éviter un aller-retour inutile vers le serveur quand il existe des erreurs d’entrée sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="9956b-467">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="9956b-468">Les références de script suivantes dans *_Layout.cshtml* et *_ValidationScriptsPartial.cshtml* prennent en charge la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="9956b-468">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="9956b-469">Le script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) est une bibliothèque frontale personnalisée de Microsoft qui s’appuie sur le plug-in bien connu [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="9956b-469">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="9956b-470">Sans jQuery Unobtrusive Validation, vous devriez coder la même logique de validation à deux endroits : une fois dans les attributs de validation côté serveur sur les propriétés du modèle, puis à nouveau dans les scripts côté client.</span><span class="sxs-lookup"><span data-stu-id="9956b-470">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="9956b-471">Au lieu de cela, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les attributs de validation et les métadonnées de type des propriétés du modèle afin de restituer les attributs `data-` HTML 5 pour les éléments de formulaire nécessitant une validation.</span><span class="sxs-lookup"><span data-stu-id="9956b-471">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="9956b-472">jQuery Unobtrusive Validation analyse les attributs `data-` et passe la logique à jQuery Validate, en « copiant » la logique de validation côté serveur vers le client.</span><span class="sxs-lookup"><span data-stu-id="9956b-472">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="9956b-473">Vous pouvez afficher les erreurs de validation sur le client en utilisant des Tag Helpers, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="9956b-473">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="9956b-474">Les Tag Helpers précédents restituent le code HTML suivant.</span><span class="sxs-lookup"><span data-stu-id="9956b-474">The preceding tag helpers render the following HTML.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="9956b-475">Notez que les attributs `data-` dans la sortie HTML correspondent aux attributs de validation pour la propriété `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="9956b-475">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="9956b-476">L’attribut `data-val-required` contient un message d’erreur à afficher si l’utilisateur ne renseigne pas le champ correspondant à la date de sortie.</span><span class="sxs-lookup"><span data-stu-id="9956b-476">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="9956b-477">jQuery Unobtrusive Validation passe cette valeur à la méthode [`required()`](https://jqueryvalidation.org/required-method/) de jQuery Validate, qui affiche alors ce message dans l’élément **\<span>** qui l’accompagne.</span><span class="sxs-lookup"><span data-stu-id="9956b-477">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="9956b-478">La validation du type de données est basée sur le type .NET d’une propriété, sauf en cas de substitution par un attribut `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="9956b-478">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="9956b-479">Les navigateurs ont leurs propres messages d’erreur par défaut, mais le package jQuery Validation Unobtrusive Validation peut remplacer ces messages.</span><span class="sxs-lookup"><span data-stu-id="9956b-479">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="9956b-480">Les attributs `[DataType]` et les sous-classes comme `[EmailAddress]` vous permettent de spécifier le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="9956b-480">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="9956b-481">Ajouter une validation à des formulaires dynamiques</span><span class="sxs-lookup"><span data-stu-id="9956b-481">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="9956b-482">jQuery Unobtrusive Validation passe la logique et les paramètres de validation à jQuery Validate lors du premier chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="9956b-482">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="9956b-483">Par conséquent, la validation ne fonctionne pas automatiquement sur les formulaires générés de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="9956b-483">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="9956b-484">Pour activer la validation, vous devez faire en sorte que jQuery Validate analyse le formulaire dynamique immédiatement après l’avoir créé.</span><span class="sxs-lookup"><span data-stu-id="9956b-484">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="9956b-485">Par exemple, le code suivant définit la validation côté client sur un formulaire ajouté par le biais d’AJAX.</span><span class="sxs-lookup"><span data-stu-id="9956b-485">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="9956b-486">La méthode `$.validator.unobtrusive.parse()` accepte un sélecteur jQuery comme argument.</span><span class="sxs-lookup"><span data-stu-id="9956b-486">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="9956b-487">Cette méthode indique à jQuery Unobtrusive Validation d’analyser les attributs `data-` des formulaires dans ce sélecteur.</span><span class="sxs-lookup"><span data-stu-id="9956b-487">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="9956b-488">Les valeurs de ces attributs sont ensuite passées au plug-in jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="9956b-488">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="9956b-489">Ajouter une validation à des contrôles dynamiques</span><span class="sxs-lookup"><span data-stu-id="9956b-489">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="9956b-490">La méthode `$.validator.unobtrusive.parse()` opère sur un formulaire entier, et non sur des contrôles individuels générés de manière dynamique tels que `<input>` et `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="9956b-490">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="9956b-491">Pour réanalyser le formulaire, supprimez les données de validation qui ont été ajoutées quand le formulaire a été analysé précédemment, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-491">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="9956b-492">Validation personnalisée côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-492">Custom client-side validation</span></span>

<span data-ttu-id="9956b-493">La validation personnalisée côté client s’effectue en générant des attributs HTML `data-` qui fonctionnent avec un adaptateur jQuery Validate personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-493">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="9956b-494">L’exemple de code d’adaptateur suivant a été écrit pour les attributs `ClassicMovie` et `ClassicMovie2` qui ont été introduits plus haut dans cet article :</span><span class="sxs-lookup"><span data-stu-id="9956b-494">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="9956b-495">Pour plus d’informations sur la façon d’écrire des adaptateurs, consultez la [documentation de jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="9956b-495">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="9956b-496">L’utilisation d’un adaptateur pour un champ donné est déclenchée par des attributs `data-` qui :</span><span class="sxs-lookup"><span data-stu-id="9956b-496">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="9956b-497">Marquent le champ comme étant soumis à validation (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="9956b-497">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="9956b-498">Identifient un nom de règle de validation et un texte de message d’erreur (par exemple, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="9956b-498">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="9956b-499">Fournissent les éventuels paramètres supplémentaires dont le validateur a besoin (par exemple, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="9956b-499">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="9956b-500">L’exemple suivant montre les attributs `data-` pour l’attribut `ClassicMovie` de l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="9956b-500">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="9956b-501">Comme mentionné plus haut, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les informations des attributs de validation pour restituer les attributs `data-`.</span><span class="sxs-lookup"><span data-stu-id="9956b-501">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="9956b-502">Il existe deux options pour l’écriture de code qui entraîne la création d’attributs HTML `data-` personnalisés :</span><span class="sxs-lookup"><span data-stu-id="9956b-502">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="9956b-503">Créez une classe qui dérive de `AttributeAdapterBase<TAttribute>` et une classe qui implémente `IValidationAttributeAdapterProvider`, et inscrivez votre attribut et son adaptateur dans l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9956b-503">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="9956b-504">Cette méthode respecte le [principe de responsabilité unique](https://wikipedia.org/wiki/Single_responsibility_principle), dans la mesure où le code de validation lié au serveur et celui lié au client se trouvent dans des classes distinctes.</span><span class="sxs-lookup"><span data-stu-id="9956b-504">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="9956b-505">Il existe un autre avantage : l’adaptateur étant inscrit dans l’injection de dépendances, les autres services dans l’injection de dépendances lui sont accessibles si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="9956b-505">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="9956b-506">Implémentez `IClientModelValidator` dans votre classe `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9956b-506">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="9956b-507">Cette méthode peut être appropriée si l’attribut n’effectue aucune validation côté serveur et n’a besoin d’aucun service à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9956b-507">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="9956b-508">AttributeAdapter pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-508">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="9956b-509">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovie` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="9956b-509">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="9956b-510">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="9956b-510">To add client validation by using this method:</span></span>

1. <span data-ttu-id="9956b-511">Créez une classe d’adaptateurs d’attributs pour l’attribut de validation personnalisé.</span><span class="sxs-lookup"><span data-stu-id="9956b-511">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="9956b-512">Dérivez la classe de [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="9956b-512">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="9956b-513">Créez une méthode `AddValidation` qui ajoute des attributs `data-` à la sortie restituée, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-513">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="9956b-514">Créez une classe de fournisseurs d’adaptateurs qui implémente <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="9956b-514">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="9956b-515">Dans la méthode `GetAttributeAdapter`, passez l’attribut personnalisé au constructeur de l’adaptateur, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="9956b-515">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="9956b-516">Inscrivez le fournisseur d’adaptateurs auprès de l’injection de dépendances dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="9956b-516">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="9956b-517">IClientModelValidator pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-517">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="9956b-518">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovie2` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="9956b-518">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="9956b-519">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="9956b-519">To add client validation by using this method:</span></span>

* <span data-ttu-id="9956b-520">Dans l’attribut de validation personnalisé, implémentez l’interface `IClientModelValidator` et créez une méthode `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="9956b-520">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="9956b-521">Dans la méthode `AddValidation`, ajoutez des attributs `data-` pour la validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9956b-521">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="9956b-522">Désactiver la validation côté client</span><span class="sxs-lookup"><span data-stu-id="9956b-522">Disable client-side validation</span></span>

<span data-ttu-id="9956b-523">Le code suivant désactive la validation côté client dans les vues MVC :</span><span class="sxs-lookup"><span data-stu-id="9956b-523">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="9956b-524">Et dans Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="9956b-524">And in Razor Pages:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="9956b-525">Une autre option permettant de désactiver la validation côté client consiste à commenter la référence à `_ValidationScriptsPartial` dans votre fichier *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9956b-525">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9956b-526">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9956b-526">Additional resources</span></span>

* [<span data-ttu-id="9956b-527">Espace de noms System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="9956b-527">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="9956b-528">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="9956b-528">Model Binding</span></span>](model-binding.md)

::: moniker-end
