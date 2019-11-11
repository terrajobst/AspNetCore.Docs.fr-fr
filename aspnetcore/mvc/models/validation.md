---
title: Validation de modèle dans ASP.NET Core MVC
author: rick-anderson
description: Découvrez plus d’informations sur la validation de modèle dans ASP.NET Core MVC et Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2019
uid: mvc/models/validation
ms.openlocfilehash: 8e9e588c8665962b2fe285b0feab977b16938154
ms.sourcegitcommit: 99cdd60a26ff0970880bb43c558d78b1ef17c237
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2019
ms.locfileid: "73915528"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="f8e3b-103">Validation de modèle dans ASP.NET Core MVC et Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f8e3b-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f8e3b-104">Cet article explique comment valider l’entrée d’utilisateur dans une application ASP.NET Core MVC ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="f8e3b-105">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="f8e3b-106">État du modèle</span><span class="sxs-lookup"><span data-stu-id="f8e3b-106">Model state</span></span>

<span data-ttu-id="f8e3b-107">L’état du modèle représente les erreurs qui proviennent de deux sous-systèmes : liaison de modèle et validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="f8e3b-108">Les erreurs qui proviennent de la [liaison de modèle](model-binding.md) sont généralement des erreurs de conversion de données (par exemple, un « x » est entré dans un champ qui attend un nombre entier).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="f8e3b-109">La validation de modèle se produit après la liaison de modèle, et signale les erreurs où les données ne sont pas conformes aux règles d’entreprise (par exemple, un 0 est entré dans un champ qui attend une évaluation comprise entre 1 et 5).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="f8e3b-110">La liaison et la validation de modèle se produisent avant l’exécution d’une action de contrôleur ou d’une méthode de gestionnaire Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="f8e3b-111">Pour les applications web, il incombe à l’application d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-111">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="f8e3b-112">En règle générale, les applications web réaffichent la page avec un message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="f8e3b-113">Les contrôleurs d’API web ne sont pas obligés de vérifier `ModelState.IsValid` s’ils ont l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-113">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="f8e3b-114">Dans ce cas, une réponse HTTP 400 automatique contenant les détails du problème est retournée quand l’état du modèle n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-114">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="f8e3b-115">Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-115">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="f8e3b-116">Réexécuter la validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-116">Rerun validation</span></span>

<span data-ttu-id="f8e3b-117">La validation est automatique, mais vous souhaiterez peut-être la répéter manuellement.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-117">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="f8e3b-118">Par exemple, vous pourriez calculer une valeur pour une propriété, et souhaiter réexécuter la validation après avoir affecté la valeur calculée comme valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-118">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="f8e3b-119">Pour réexécuter la validation, appelez la méthode `TryValidateModel`, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-119">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="f8e3b-120">Attributs de validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-120">Validation attributes</span></span>

<span data-ttu-id="f8e3b-121">Les attributs de validation vous permettent de spécifier des règles de validation pour des propriétés de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-121">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="f8e3b-122">L’exemple suivant tiré de l’[exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) montre une classe de modèle qui est annotée avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-122">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="f8e3b-123">L’attribut `[ClassicMovie]` est un attribut de validation personnalisé, et les autres sont prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-123">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="f8e3b-124">(`[ClassicMovie2]` n’est pas affiché ici ; il montre une autre façon d’implémenter un attribut personnalisé.)</span><span class="sxs-lookup"><span data-stu-id="f8e3b-124">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="f8e3b-125">Attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="f8e3b-125">Built-in attributes</span></span>

<span data-ttu-id="f8e3b-126">Voici certains des attributs de validation prédéfinis :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-126">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="f8e3b-127">`[CreditCard]`: vérifie que la propriété a un format de carte de crédit.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-127">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="f8e3b-128">`[Compare]`: valide que deux propriétés d’un modèle correspondent.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-128">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="f8e3b-129">`[EmailAddress]`: valide que la propriété a un format d’e-mail.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-129">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="f8e3b-130">`[Phone]`: valide que la propriété a un format de numéro de téléphone.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-130">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="f8e3b-131">`[Range]`: valide que la valeur de la propriété est comprise dans une plage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-131">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="f8e3b-132">`[RegularExpression]`: valide le fait que la valeur de propriété corresponde à une expression régulière spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-132">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="f8e3b-133">`[Required]`: vérifie que le champ n’a pas la valeur null.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-133">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="f8e3b-134">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Required]](#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-134">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="f8e3b-135">`[StringLength]`: valide le fait qu’une valeur de propriété de chaîne ne dépasse pas une limite de longueur spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-135">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="f8e3b-136">`[Url]`: valide que la propriété a un format d’URL.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-136">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="f8e3b-137">`[Remote]`: valide l’entrée sur le client en appelant une méthode d’action sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-137">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="f8e3b-138">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Remote]](#remote-attribute).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-138">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="f8e3b-139">Vous trouverez la liste complète des attributs de validation dans l’espace de noms [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-139">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="f8e3b-140">Messages d’erreur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-140">Error messages</span></span>

<span data-ttu-id="f8e3b-141">Les attributs de validation vous permettent de spécifier le message d’erreur à afficher pour l’entrée non valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-141">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="f8e3b-142">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-142">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="f8e3b-143">En interne, les attributs appellent `String.Format` avec un espace réservé pour le nom de champ et parfois d’autres espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-143">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="f8e3b-144">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-144">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="f8e3b-145">Appliqué à une propriété `Name`, le message d’erreur créé par le code précédent serait « Name length must be between 6 and 8 ».</span><span class="sxs-lookup"><span data-stu-id="f8e3b-145">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="f8e3b-146">Pour savoir quels paramètres sont passés à `String.Format` pour le message d’erreur d’un attribut particulier, consultez le [code source de DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-146">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="f8e3b-147">Attribut [Required]</span><span class="sxs-lookup"><span data-stu-id="f8e3b-147">[Required] attribute</span></span>

<span data-ttu-id="f8e3b-148">Par défaut, le système de validation traite les propriétés ou paramètres n’acceptant pas les valeurs Null comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-148">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="f8e3b-149">Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) comme `decimal` et `int` n’acceptent pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-149">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="f8e3b-150">Validation de [Required] sur le serveur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-150">[Required] validation on the server</span></span>

<span data-ttu-id="f8e3b-151">Sur le serveur, une valeur obligatoire est considérée comme manquante si la propriété est Null.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-151">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="f8e3b-152">Un champ n’acceptant pas les valeurs Null est toujours valide, et le message d’erreur de l’attribut [Required] n’est jamais affiché.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-152">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="f8e3b-153">Toutefois, la liaison de modèle pour une propriété n’acceptant pas les valeurs Null peut échouer, entraînant l’affichage d’un message d’erreur tel que `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-153">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="f8e3b-154">Pour spécifier un message d’erreur personnalisé pour la validation côté serveur des types n’acceptant pas les valeurs Null, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-154">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="f8e3b-155">Rendre le champ Nullable (par exemple, `decimal?` au lieu de `decimal`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-155">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="f8e3b-156">Les types valeur [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) sont traités comme des types Nullable standard.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-156">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="f8e3b-157">Spécifier le message d’erreur par défaut devant être utilisé par la liaison de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-157">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="f8e3b-158">Pour plus d’informations sur les erreurs de liaison de modèle pour lesquelles vous pouvez définir des messages par défaut, consultez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-158">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="f8e3b-159">Validation de [Required] sur le client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-159">[Required] validation on the client</span></span>

<span data-ttu-id="f8e3b-160">Les chaînes et les types n’acceptant pas les valeurs Null sont gérés différemment sur le client et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-160">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="f8e3b-161">Sur le client :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-161">On the client:</span></span>

* <span data-ttu-id="f8e3b-162">Une valeur est considérée comme présente uniquement si une entrée est tapée pour celle-ci.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-162">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="f8e3b-163">Par conséquent, la validation côté client gère les types n’acceptant pas les valeurs Null de la même façon que les types Nullable</span><span class="sxs-lookup"><span data-stu-id="f8e3b-163">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="f8e3b-164">Un espace blanc dans un champ de chaîne est considéré comme une entrée valide par la méthode [required](https://jqueryvalidation.org/required-method/) jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-164">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="f8e3b-165">La validation côté serveur considère qu’un champ de chaîne obligatoire est non valide si seul un espace blanc est entré.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-165">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="f8e3b-166">Comme indiqué précédemment, les types n’acceptant pas les valeurs Null sont traités comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-166">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="f8e3b-167">Cela signifie que vous bénéficiez d’une validation côté client même si vous n’appliquez pas l’attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-167">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="f8e3b-168">En revanche, si vous n’utilisez pas l’attribut, vous recevez un message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-168">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="f8e3b-169">Pour spécifier un message d’erreur personnalisé, utilisez l’attribut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-169">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="f8e3b-170">Attribut [Remote]</span><span class="sxs-lookup"><span data-stu-id="f8e3b-170">[Remote] attribute</span></span>

<span data-ttu-id="f8e3b-171">L’attribut `[Remote]` implémente la validation côté client qui nécessite l’appel d’une méthode sur le serveur pour déterminer si le champ d’entrée est valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-171">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="f8e3b-172">Par exemple, l’application peut avoir besoin de vérifier si un nom d’utilisateur est déjà en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-172">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="f8e3b-173">Pour implémenter la validation à distance</span><span class="sxs-lookup"><span data-stu-id="f8e3b-173">To implement remote validation:</span></span>

1. <span data-ttu-id="f8e3b-174">Créez une méthode d’action devant être appelée par JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-174">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="f8e3b-175">La méthode [remote](https://jqueryvalidation.org/remote-method/) jQuery Validate attend une réponse JSON :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-175">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="f8e3b-176">`"true"` signifie que les données d’entrée sont valides.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-176">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="f8e3b-177">`"false"`, `undefined` ou `null` signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-177">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="f8e3b-178">Affichez le message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-178">Display the default error message.</span></span>
   * <span data-ttu-id="f8e3b-179">Toute autre chaîne signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-179">Any other string means the input is invalid.</span></span> <span data-ttu-id="f8e3b-180">Affichez la chaîne en tant que message d’erreur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-180">Display the string as a custom error message.</span></span>

   <span data-ttu-id="f8e3b-181">Voici un exemple de méthode d’action qui retourne un message d’erreur personnalisé :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-181">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="f8e3b-182">Dans la classe de modèle, annotez la propriété avec un attribut `[Remote]` qui pointe vers la méthode d’action de validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-182">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="f8e3b-183">L'attribut `[Remote]` se trouve dans l'espace de noms `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-183">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="f8e3b-184">Installez le package NuGet [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures), si vous n’utilisez pas le métapaquet `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-184">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="f8e3b-185">Champs supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f8e3b-185">Additional fields</span></span>

<span data-ttu-id="f8e3b-186">La propriété `AdditionalFields` de l’attribut `[Remote]` vous permet de valider des combinaisons de champs relativement à des données présentes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-186">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="f8e3b-187">Par exemple, si le modèle `User` a des propriétés `FirstName` et `LastName`, vous pouvez vérifier qu’aucun utilisateur existant n’a déjà cette paire de noms.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-187">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="f8e3b-188">L'exemple suivant montre comment utiliser `AdditionalFields` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-188">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="f8e3b-189">`AdditionalFields` peut être défini explicitement avec les chaînes `"FirstName"` et `"LastName"`, mais l’utilisation de l’opérateur [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) simplifie la refactorisation ultérieure.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-189">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="f8e3b-190">La méthode d’action pour cette validation doit accepter les arguments de nom et de prénom :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-190">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="f8e3b-191">Quand l’utilisateur entre un nom ou un prénom, JavaScript effectue un appel à distance pour vérifier si cette paire de noms est déjà utilisée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-191">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="f8e3b-192">Pour valider deux champs supplémentaires ou plus, spécifiez-les sous la forme d’une liste délimitée par des virgules.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-192">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="f8e3b-193">Par exemple, pour ajouter une propriété `MiddleName` au modèle, définissez l’attribut `[Remote]` comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-193">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="f8e3b-194">`AdditionalFields`, comme tous les arguments d’attribut, doit être une expression constante.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-194">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="f8e3b-195">Vous ne devez donc pas utiliser une [chaîne interpolée](/dotnet/csharp/language-reference/keywords/interpolated-strings) ou appeler <xref:System.String.Join*> pour initialiser `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-195">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="f8e3b-196">Alternatives aux attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="f8e3b-196">Alternatives to built-in attributes</span></span>

<span data-ttu-id="f8e3b-197">Si vous avez besoin d’une validation non fournie par les attributs prédéfinis, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-197">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="f8e3b-198">[Créer des attributs personnalisés](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-198">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="f8e3b-199">[Implémenter IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-199">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="f8e3b-200">Attributs personnalisés</span><span class="sxs-lookup"><span data-stu-id="f8e3b-200">Custom attributes</span></span>

<span data-ttu-id="f8e3b-201">Pour les scénarios non gérés par les attributs de validation prédéfinis, vous pouvez créer des attributs de validation personnalisés.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-201">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="f8e3b-202">Créez une classe qui hérite de <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> et substituez la méthode <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-202">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="f8e3b-203">La méthode `IsValid` accepte un objet nommé *value*, qui est l’entrée à valider.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-203">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="f8e3b-204">Une surcharge accepte également un objet `ValidationContext`, qui fournit des informations supplémentaires telles que l’instance de modèle créée par la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-204">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="f8e3b-205">L’exemple suivant vérifie que la date de sortie d’un film appartenant au genre *Classic* n’est pas ultérieure à une année spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-205">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="f8e3b-206">L’attribut `[ClassicMovie2]` vérifie d’abord le genre, et continue uniquement s’il s’agit de *Classic*.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-206">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="f8e3b-207">Pour les films identifiés comme des classiques, il vérifie si la date de sortie n’est pas ultérieure à la limite passée au constructeur d’attribut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-207">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="f8e3b-208">La variable `movie` de l’exemple précédent représente un objet `Movie` qui contient les données de l’envoi du formulaire.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-208">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="f8e3b-209">La méthode `IsValid` vérifie la date et le genre.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-209">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="f8e3b-210">Si la validation réussit, `IsValid` retourne un code `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-210">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="f8e3b-211">Quand la validation échoue, un `ValidationResult` avec un message d’erreur est retourné.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-211">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="f8e3b-212">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="f8e3b-212">IValidatableObject</span></span>

<span data-ttu-id="f8e3b-213">L’exemple précédent fonctionne uniquement avec les types `Movie`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-213">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="f8e3b-214">Une autre option de validation au niveau de la classe consiste à implémenter `IValidatableObject` dans la classe de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-214">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="f8e3b-215">Validation du nœud de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-215">Top-level node validation</span></span>

<span data-ttu-id="f8e3b-216">Les nœuds de niveau supérieur incluent les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-216">Top-level nodes include:</span></span>

* <span data-ttu-id="f8e3b-217">Paramètres d’action</span><span class="sxs-lookup"><span data-stu-id="f8e3b-217">Action parameters</span></span>
* <span data-ttu-id="f8e3b-218">Propriétés du contrôleur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-218">Controller properties</span></span>
* <span data-ttu-id="f8e3b-219">Paramètres du gestionnaire de page</span><span class="sxs-lookup"><span data-stu-id="f8e3b-219">Page handler parameters</span></span>
* <span data-ttu-id="f8e3b-220">Propriétés du modèle de page</span><span class="sxs-lookup"><span data-stu-id="f8e3b-220">Page model properties</span></span>

<span data-ttu-id="f8e3b-221">Les nœuds de niveau supérieur liés au modèle sont validés en plus de la validation des propriétés du modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-221">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="f8e3b-222">Dans l’exemple suivant tiré de l’exemple d’application, la méthode `VerifyPhone` utilise <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> pour valider le paramètre d’action `phone` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-222">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="f8e3b-223">Les nœuds de niveau supérieur peuvent utiliser <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-223">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="f8e3b-224">Dans l’exemple suivant de l’exemple d’application, la méthode `CheckAge` spécifie que le paramètre `age` doit être lié à partir de la chaîne de requête au moment de l’envoi du formulaire :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-224">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="f8e3b-225">Dans la page de vérification de l’âge (*CheckAge.cshtml*), il existe deux formulaires.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-225">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="f8e3b-226">Le premier formulaire envoie une valeur `Age` égale à `99` en tant que chaîne de requête : `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-226">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="f8e3b-227">Quand un paramètre `age` au format approprié est envoyé à partir de la chaîne de requête, le formulaire est validé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-227">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="f8e3b-228">Le second formulaire de la page de vérification de l’âge envoie la valeur `Age` dans le corps de la requête, ce qui entraîne un échec de la validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-228">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="f8e3b-229">L’échec de la liaison est dû au fait que le paramètre `age` doit provenir d’une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-229">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="f8e3b-230">Lors de l’exécution avec `CompatibilityVersion.Version_2_1` ou version ultérieure, la validation du nœud de niveau supérieur est activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-230">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="f8e3b-231">Autrement, la validation du nœud de niveau supérieur est désactivée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-231">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="f8e3b-232">L’option par défaut peut être remplacée en définissant la propriété <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> dans (`Startup.ConfigureServices`), comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-232">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="f8e3b-233">Quantité maximale d’erreurs</span><span class="sxs-lookup"><span data-stu-id="f8e3b-233">Maximum errors</span></span>

<span data-ttu-id="f8e3b-234">La validation s’arrête quand le nombre maximal d’erreurs est atteint (200 par défaut).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-234">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="f8e3b-235">Vous pouvez configurer ce nombre avec le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-235">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="f8e3b-236">Récursivité maximale</span><span class="sxs-lookup"><span data-stu-id="f8e3b-236">Maximum recursion</span></span>

<span data-ttu-id="f8e3b-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> parcourt le graphe d’objet du modèle en cours de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="f8e3b-238">Pour les modèles très profonds ou infiniment récursifs, la validation peut entraîner un dépassement de la capacité de la pile.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-238">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="f8e3b-239">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) fournit un moyen d’interrompre la validation de manière anticipée si la récursivité du visiteur dépasse une profondeur configurée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-239">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="f8e3b-240">La valeur par défaut de `MvcOptions.MaxValidationDepth` est 200 lors de l’exécution avec `CompatibilityVersion.Version_2_2` ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-240">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="f8e3b-241">Pour les versions antérieures, la valeur est Null, ce qui signifie qu’il n’y a aucune contrainte de profondeur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-241">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="f8e3b-242">Court-circuit automatique</span><span class="sxs-lookup"><span data-stu-id="f8e3b-242">Automatic short-circuit</span></span>

<span data-ttu-id="f8e3b-243">La validation est automatiquement court-circuitée (ignorée) si le graphe du modèle ne nécessite pas de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-243">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="f8e3b-244">Les objets pour lesquels le runtime ignore la validation comprennent les collections de primitives (telles que `byte[]`, `string[]`, `Dictionary<string, string>`) et les graphes d’objets complexes qui n’ont pas de validateur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-244">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="f8e3b-245">Désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-245">Disable validation</span></span>

<span data-ttu-id="f8e3b-246">Pour désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-246">To disable validation:</span></span>

1. <span data-ttu-id="f8e3b-247">Créez une implémentation de `IObjectModelValidator` qui ne marque aucun champ comme étant non valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-247">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="f8e3b-248">Ajoutez le code suivant à `Startup.ConfigureServices` pour remplacer l’implémentation par défaut de `IObjectModelValidator` dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-248">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="f8e3b-249">Vous risquez toujours de voir des erreurs d’état du modèle provenant de la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-249">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="f8e3b-250">Validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-250">Client-side validation</span></span>

<span data-ttu-id="f8e3b-251">La validation côté client empêche l’envoi jusqu’à ce que le formulaire soit valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-251">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="f8e3b-252">Le bouton Submit exécute le code JavaScript qui envoie le formulaire ou qui affiche des messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-252">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="f8e3b-253">La validation côté client permet d’éviter un aller-retour inutile vers le serveur quand il existe des erreurs d’entrée sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-253">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="f8e3b-254">Les références de script suivantes dans *_Layout.cshtml* et *_ValidationScriptsPartial.cshtml* prennent en charge la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-254">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="f8e3b-255">Le script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) est une bibliothèque frontale personnalisée de Microsoft qui s’appuie sur le plug-in bien connu [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-255">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="f8e3b-256">Sans jQuery Unobtrusive Validation, vous devriez coder la même logique de validation à deux endroits : une fois dans les attributs de validation côté serveur sur les propriétés du modèle, puis à nouveau dans les scripts côté client.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-256">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="f8e3b-257">Au lieu de cela, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les attributs de validation et les métadonnées de type des propriétés du modèle afin de restituer les attributs `data-` HTML 5 pour les éléments de formulaire nécessitant une validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-257">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="f8e3b-258">jQuery Unobtrusive Validation analyse les attributs `data-` et passe la logique à jQuery Validate, en « copiant » la logique de validation côté serveur vers le client.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-258">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="f8e3b-259">Vous pouvez afficher les erreurs de validation sur le client en utilisant des Tag Helpers, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-259">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="f8e3b-260">Les Tag Helpers précédents restituent le code HTML suivant.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-260">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="f8e3b-261">Notez que les attributs `data-` dans la sortie HTML correspondent aux attributs de validation pour la propriété `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-261">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="f8e3b-262">L’attribut `data-val-required` contient un message d’erreur à afficher si l’utilisateur ne renseigne pas le champ correspondant à la date de sortie.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-262">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="f8e3b-263">jQuery Unobtrusive Validation passe cette valeur à la méthode [`required()`](https://jqueryvalidation.org/required-method/) de jQuery Validate, qui affiche alors ce message dans l’élément **\<span>** qui l’accompagne.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-263">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="f8e3b-264">La validation du type de données est basée sur le type .NET d’une propriété, sauf en cas de substitution par un attribut `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-264">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="f8e3b-265">Les navigateurs ont leurs propres messages d’erreur par défaut, mais le package jQuery Validation Unobtrusive Validation peut remplacer ces messages.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-265">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="f8e3b-266">Les attributs `[DataType]` et les sous-classes comme `[EmailAddress]` vous permettent de spécifier le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-266">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="f8e3b-267">Ajouter une validation à des formulaires dynamiques</span><span class="sxs-lookup"><span data-stu-id="f8e3b-267">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="f8e3b-268">jQuery Unobtrusive Validation passe la logique et les paramètres de validation à jQuery Validate lors du premier chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-268">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="f8e3b-269">Par conséquent, la validation ne fonctionne pas automatiquement sur les formulaires générés de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-269">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="f8e3b-270">Pour activer la validation, vous devez faire en sorte que jQuery Validate analyse le formulaire dynamique immédiatement après l’avoir créé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-270">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="f8e3b-271">Par exemple, le code suivant définit la validation côté client sur un formulaire ajouté par le biais d’AJAX.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-271">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="f8e3b-272">La méthode `$.validator.unobtrusive.parse()` accepte un sélecteur jQuery comme argument.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-272">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="f8e3b-273">Cette méthode indique à jQuery Unobtrusive Validation d’analyser les attributs `data-` des formulaires dans ce sélecteur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-273">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="f8e3b-274">Les valeurs de ces attributs sont ensuite passées au plug-in jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-274">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="f8e3b-275">Ajouter une validation à des contrôles dynamiques</span><span class="sxs-lookup"><span data-stu-id="f8e3b-275">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="f8e3b-276">La méthode `$.validator.unobtrusive.parse()` opère sur un formulaire entier, et non sur des contrôles individuels générés de manière dynamique tels que `<input>` et `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-276">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="f8e3b-277">Pour réanalyser le formulaire, supprimez les données de validation qui ont été ajoutées quand le formulaire a été analysé précédemment, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-277">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="f8e3b-278">Validation personnalisée côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-278">Custom client-side validation</span></span>

<span data-ttu-id="f8e3b-279">La validation personnalisée côté client s’effectue en générant des attributs HTML `data-` qui fonctionnent avec un adaptateur jQuery Validate personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-279">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="f8e3b-280">L’exemple de code d’adaptateur suivant a été écrit pour les attributs `ClassicMovie` et `ClassicMovie2` qui ont été introduits plus haut dans cet article :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-280">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="f8e3b-281">Pour plus d’informations sur la façon d’écrire des adaptateurs, consultez la [documentation de jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-281">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="f8e3b-282">L’utilisation d’un adaptateur pour un champ donné est déclenchée par des attributs `data-` qui :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-282">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="f8e3b-283">Marquent le champ comme étant soumis à validation (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-283">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="f8e3b-284">Identifient un nom de règle de validation et un texte de message d’erreur (par exemple, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-284">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="f8e3b-285">Fournissent les éventuels paramètres supplémentaires dont le validateur a besoin (par exemple, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-285">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="f8e3b-286">L’exemple suivant montre les attributs `data-` pour l’attribut `ClassicMovie` de l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-286">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="f8e3b-287">Comme mentionné plus haut, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les informations des attributs de validation pour restituer les attributs `data-`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-287">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="f8e3b-288">Il existe deux options pour l’écriture de code qui entraîne la création d’attributs HTML `data-` personnalisés :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-288">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="f8e3b-289">Créez une classe qui dérive de `AttributeAdapterBase<TAttribute>` et une classe qui implémente `IValidationAttributeAdapterProvider`, et inscrivez votre attribut et son adaptateur dans l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-289">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="f8e3b-290">Cette méthode respecte le [principe de responsabilité unique](https://wikipedia.org/wiki/Single_responsibility_principle), dans la mesure où le code de validation lié au serveur et celui lié au client se trouvent dans des classes distinctes.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-290">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="f8e3b-291">Il existe un autre avantage : l’adaptateur étant inscrit dans l’injection de dépendances, les autres services dans l’injection de dépendances lui sont accessibles si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-291">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="f8e3b-292">Implémentez `IClientModelValidator` dans votre classe `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-292">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="f8e3b-293">Cette méthode peut être appropriée si l’attribut n’effectue aucune validation côté serveur et n’a besoin d’aucun service à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-293">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="f8e3b-294">AttributeAdapter pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-294">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="f8e3b-295">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovie` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-295">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="f8e3b-296">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="f8e3b-296">To add client validation by using this method:</span></span>

1. <span data-ttu-id="f8e3b-297">Créez une classe d’adaptateurs d’attributs pour l’attribut de validation personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-297">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="f8e3b-298">Dérivez la classe de [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-298">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="f8e3b-299">Créez une méthode `AddValidation` qui ajoute des attributs `data-` à la sortie restituée, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-299">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="f8e3b-300">Créez une classe de fournisseurs d’adaptateurs qui implémente <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-300">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="f8e3b-301">Dans la méthode `GetAttributeAdapter`, passez l’attribut personnalisé au constructeur de l’adaptateur, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-301">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="f8e3b-302">Inscrivez le fournisseur d’adaptateurs auprès de l’injection de dépendances dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-302">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="f8e3b-303">IClientModelValidator pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-303">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="f8e3b-304">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovie2` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-304">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="f8e3b-305">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="f8e3b-305">To add client validation by using this method:</span></span>

* <span data-ttu-id="f8e3b-306">Dans l’attribut de validation personnalisé, implémentez l’interface `IClientModelValidator` et créez une méthode `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-306">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="f8e3b-307">Dans la méthode `AddValidation`, ajoutez des attributs `data-` pour la validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-307">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="f8e3b-308">Désactiver la validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-308">Disable client-side validation</span></span>

<span data-ttu-id="f8e3b-309">Le code suivant désactive la validation côté client dans les vues MVC :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-309">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="f8e3b-310">Et dans Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-310">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="f8e3b-311">Une autre option permettant de désactiver la validation côté client consiste à commenter la référence à `_ValidationScriptsPartial` dans votre fichier *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-311">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8e3b-312">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f8e3b-312">Additional resources</span></span>

* [<span data-ttu-id="f8e3b-313">Espace de noms System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="f8e3b-313">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="f8e3b-314">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="f8e3b-314">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f8e3b-315">Cet article explique comment valider l’entrée d’utilisateur dans une application ASP.NET Core MVC ou Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-315">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="f8e3b-316">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-316">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="f8e3b-317">État du modèle</span><span class="sxs-lookup"><span data-stu-id="f8e3b-317">Model state</span></span>

<span data-ttu-id="f8e3b-318">L’état du modèle représente les erreurs qui proviennent de deux sous-systèmes : liaison de modèle et validation de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-318">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="f8e3b-319">Les erreurs qui proviennent de la [liaison de modèle](model-binding.md) sont généralement des erreurs de conversion de données (par exemple, un « x » est entré dans un champ qui attend un nombre entier).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-319">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="f8e3b-320">La validation de modèle se produit après la liaison de modèle, et signale les erreurs où les données ne sont pas conformes aux règles d’entreprise (par exemple, un 0 est entré dans un champ qui attend une évaluation comprise entre 1 et 5).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-320">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="f8e3b-321">La liaison et la validation de modèle se produisent avant l’exécution d’une action de contrôleur ou d’une méthode de gestionnaire Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-321">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="f8e3b-322">Pour les applications web, il incombe à l’application d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-322">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="f8e3b-323">En règle générale, les applications web réaffichent la page avec un message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-323">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="f8e3b-324">Les contrôleurs d’API web ne sont pas obligés de vérifier `ModelState.IsValid` s’ils ont l’attribut `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-324">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="f8e3b-325">Dans ce cas, une réponse HTTP 400 automatique contenant les détails du problème est retournée quand l’état du modèle n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-325">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="f8e3b-326">Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-326">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="f8e3b-327">Réexécuter la validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-327">Rerun validation</span></span>

<span data-ttu-id="f8e3b-328">La validation est automatique, mais vous souhaiterez peut-être la répéter manuellement.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-328">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="f8e3b-329">Par exemple, vous pourriez calculer une valeur pour une propriété, et souhaiter réexécuter la validation après avoir affecté la valeur calculée comme valeur de la propriété.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-329">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="f8e3b-330">Pour réexécuter la validation, appelez la méthode `TryValidateModel`, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-330">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="f8e3b-331">Attributs de validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-331">Validation attributes</span></span>

<span data-ttu-id="f8e3b-332">Les attributs de validation vous permettent de spécifier des règles de validation pour des propriétés de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-332">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="f8e3b-333">L’exemple suivant tiré de l’[exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) montre une classe de modèle qui est annotée avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-333">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="f8e3b-334">L’attribut `[ClassicMovie]` est un attribut de validation personnalisé, et les autres sont prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-334">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="f8e3b-335">(`[ClassicMovie2]` n’est pas affiché ici ; il montre une autre façon d’implémenter un attribut personnalisé.)</span><span class="sxs-lookup"><span data-stu-id="f8e3b-335">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="f8e3b-336">Attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="f8e3b-336">Built-in attributes</span></span>

<span data-ttu-id="f8e3b-337">Voici certains des attributs de validation prédéfinis :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-337">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="f8e3b-338">`[CreditCard]`: vérifie que la propriété a un format de carte de crédit.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-338">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="f8e3b-339">`[Compare]`: valide que deux propriétés d’un modèle correspondent.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-339">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="f8e3b-340">`[EmailAddress]`: valide que la propriété a un format d’e-mail.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-340">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="f8e3b-341">`[Phone]`: valide que la propriété a un format de numéro de téléphone.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-341">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="f8e3b-342">`[Range]`: valide que la valeur de la propriété est comprise dans une plage spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-342">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="f8e3b-343">`[RegularExpression]`: valide le fait que la valeur de propriété corresponde à une expression régulière spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-343">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="f8e3b-344">`[Required]`: vérifie que le champ n’a pas la valeur null.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-344">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="f8e3b-345">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Required]](#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-345">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="f8e3b-346">`[StringLength]`: valide le fait qu’une valeur de propriété de chaîne ne dépasse pas une limite de longueur spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-346">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="f8e3b-347">`[Url]`: valide que la propriété a un format d’URL.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-347">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="f8e3b-348">`[Remote]`: valide l’entrée sur le client en appelant une méthode d’action sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-348">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="f8e3b-349">Pour plus d’informations sur le comportement de cet attribut, consultez [Attribut [Remote]](#remote-attribute).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-349">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="f8e3b-350">Vous trouverez la liste complète des attributs de validation dans l’espace de noms [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-350">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="f8e3b-351">Messages d’erreur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-351">Error messages</span></span>

<span data-ttu-id="f8e3b-352">Les attributs de validation vous permettent de spécifier le message d’erreur à afficher pour l’entrée non valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-352">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="f8e3b-353">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-353">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="f8e3b-354">En interne, les attributs appellent `String.Format` avec un espace réservé pour le nom de champ et parfois d’autres espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-354">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="f8e3b-355">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-355">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="f8e3b-356">Appliqué à une propriété `Name`, le message d’erreur créé par le code précédent serait « Name length must be between 6 and 8 ».</span><span class="sxs-lookup"><span data-stu-id="f8e3b-356">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="f8e3b-357">Pour savoir quels paramètres sont passés à `String.Format` pour le message d’erreur d’un attribut particulier, consultez le [code source de DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-357">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="f8e3b-358">Attribut [Required]</span><span class="sxs-lookup"><span data-stu-id="f8e3b-358">[Required] attribute</span></span>

<span data-ttu-id="f8e3b-359">Par défaut, le système de validation traite les propriétés ou paramètres n’acceptant pas les valeurs Null comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-359">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="f8e3b-360">Les [types valeur](/dotnet/csharp/language-reference/keywords/value-types) comme `decimal` et `int` n’acceptent pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-360">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="f8e3b-361">Validation de [Required] sur le serveur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-361">[Required] validation on the server</span></span>

<span data-ttu-id="f8e3b-362">Sur le serveur, une valeur obligatoire est considérée comme manquante si la propriété est Null.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-362">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="f8e3b-363">Un champ n’acceptant pas les valeurs Null est toujours valide, et le message d’erreur de l’attribut [Required] n’est jamais affiché.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-363">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="f8e3b-364">Toutefois, la liaison de modèle pour une propriété n’acceptant pas les valeurs Null peut échouer, entraînant l’affichage d’un message d’erreur tel que `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-364">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="f8e3b-365">Pour spécifier un message d’erreur personnalisé pour la validation côté serveur des types n’acceptant pas les valeurs Null, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-365">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="f8e3b-366">Rendre le champ Nullable (par exemple, `decimal?` au lieu de `decimal`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-366">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="f8e3b-367">Les types valeur [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) sont traités comme des types Nullable standard.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-367">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="f8e3b-368">Spécifier le message d’erreur par défaut devant être utilisé par la liaison de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-368">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="f8e3b-369">Pour plus d’informations sur les erreurs de liaison de modèle pour lesquelles vous pouvez définir des messages par défaut, consultez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-369">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="f8e3b-370">Validation de [Required] sur le client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-370">[Required] validation on the client</span></span>

<span data-ttu-id="f8e3b-371">Les chaînes et les types n’acceptant pas les valeurs Null sont gérés différemment sur le client et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-371">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="f8e3b-372">Sur le client :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-372">On the client:</span></span>

* <span data-ttu-id="f8e3b-373">Une valeur est considérée comme présente uniquement si une entrée est tapée pour celle-ci.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-373">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="f8e3b-374">Par conséquent, la validation côté client gère les types n’acceptant pas les valeurs Null de la même façon que les types Nullable</span><span class="sxs-lookup"><span data-stu-id="f8e3b-374">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="f8e3b-375">Un espace blanc dans un champ de chaîne est considéré comme une entrée valide par la méthode [required](https://jqueryvalidation.org/required-method/) jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-375">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="f8e3b-376">La validation côté serveur considère qu’un champ de chaîne obligatoire est non valide si seul un espace blanc est entré.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-376">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="f8e3b-377">Comme indiqué précédemment, les types n’acceptant pas les valeurs Null sont traités comme s’ils avaient un attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-377">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="f8e3b-378">Cela signifie que vous bénéficiez d’une validation côté client même si vous n’appliquez pas l’attribut `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-378">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="f8e3b-379">En revanche, si vous n’utilisez pas l’attribut, vous recevez un message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-379">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="f8e3b-380">Pour spécifier un message d’erreur personnalisé, utilisez l’attribut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-380">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="f8e3b-381">Attribut [Remote]</span><span class="sxs-lookup"><span data-stu-id="f8e3b-381">[Remote] attribute</span></span>

<span data-ttu-id="f8e3b-382">L’attribut `[Remote]` implémente la validation côté client qui nécessite l’appel d’une méthode sur le serveur pour déterminer si le champ d’entrée est valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-382">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="f8e3b-383">Par exemple, l’application peut avoir besoin de vérifier si un nom d’utilisateur est déjà en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-383">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="f8e3b-384">Pour implémenter la validation à distance</span><span class="sxs-lookup"><span data-stu-id="f8e3b-384">To implement remote validation:</span></span>

1. <span data-ttu-id="f8e3b-385">Créez une méthode d’action devant être appelée par JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-385">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="f8e3b-386">La méthode [remote](https://jqueryvalidation.org/remote-method/) jQuery Validate attend une réponse JSON :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-386">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="f8e3b-387">`"true"` signifie que les données d’entrée sont valides.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-387">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="f8e3b-388">`"false"`, `undefined` ou `null` signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-388">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="f8e3b-389">Affichez le message d’erreur par défaut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-389">Display the default error message.</span></span>
   * <span data-ttu-id="f8e3b-390">Toute autre chaîne signifie que l’entrée n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-390">Any other string means the input is invalid.</span></span> <span data-ttu-id="f8e3b-391">Affichez la chaîne en tant que message d’erreur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-391">Display the string as a custom error message.</span></span>

   <span data-ttu-id="f8e3b-392">Voici un exemple de méthode d’action qui retourne un message d’erreur personnalisé :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-392">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="f8e3b-393">Dans la classe de modèle, annotez la propriété avec un attribut `[Remote]` qui pointe vers la méthode d’action de validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-393">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="f8e3b-394">L'attribut `[Remote]` se trouve dans l'espace de noms `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-394">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="f8e3b-395">Installez le package NuGet [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures), si vous n’utilisez pas le métapaquet `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-395">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="f8e3b-396">Champs supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f8e3b-396">Additional fields</span></span>

<span data-ttu-id="f8e3b-397">La propriété `AdditionalFields` de l’attribut `[Remote]` vous permet de valider des combinaisons de champs relativement à des données présentes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-397">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="f8e3b-398">Par exemple, si le modèle `User` a des propriétés `FirstName` et `LastName`, vous pouvez vérifier qu’aucun utilisateur existant n’a déjà cette paire de noms.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-398">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="f8e3b-399">L'exemple suivant montre comment utiliser `AdditionalFields` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-399">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="f8e3b-400">`AdditionalFields` peut être défini explicitement avec les chaînes `"FirstName"` et `"LastName"`, mais l’utilisation de l’opérateur [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) simplifie la refactorisation ultérieure.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-400">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="f8e3b-401">La méthode d’action pour cette validation doit accepter les arguments de nom et de prénom :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-401">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="f8e3b-402">Quand l’utilisateur entre un nom ou un prénom, JavaScript effectue un appel à distance pour vérifier si cette paire de noms est déjà utilisée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-402">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="f8e3b-403">Pour valider deux champs supplémentaires ou plus, spécifiez-les sous la forme d’une liste délimitée par des virgules.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-403">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="f8e3b-404">Par exemple, pour ajouter une propriété `MiddleName` au modèle, définissez l’attribut `[Remote]` comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-404">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="f8e3b-405">`AdditionalFields`, comme tous les arguments d’attribut, doit être une expression constante.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-405">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="f8e3b-406">Vous ne devez donc pas utiliser une [chaîne interpolée](/dotnet/csharp/language-reference/keywords/interpolated-strings) ou appeler <xref:System.String.Join*> pour initialiser `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-406">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="f8e3b-407">Alternatives aux attributs prédéfinis</span><span class="sxs-lookup"><span data-stu-id="f8e3b-407">Alternatives to built-in attributes</span></span>

<span data-ttu-id="f8e3b-408">Si vous avez besoin d’une validation non fournie par les attributs prédéfinis, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-408">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="f8e3b-409">[Créer des attributs personnalisés](#custom-attributes).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-409">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="f8e3b-410">[Implémenter IValidatableObject](#ivalidatableobject).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-410">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="f8e3b-411">Attributs personnalisés</span><span class="sxs-lookup"><span data-stu-id="f8e3b-411">Custom attributes</span></span>

<span data-ttu-id="f8e3b-412">Pour les scénarios non gérés par les attributs de validation prédéfinis, vous pouvez créer des attributs de validation personnalisés.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-412">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="f8e3b-413">Créez une classe qui hérite de <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> et substituez la méthode <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-413">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="f8e3b-414">La méthode `IsValid` accepte un objet nommé *value*, qui est l’entrée à valider.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-414">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="f8e3b-415">Une surcharge accepte également un objet `ValidationContext`, qui fournit des informations supplémentaires telles que l’instance de modèle créée par la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-415">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="f8e3b-416">L’exemple suivant vérifie que la date de sortie d’un film appartenant au genre *Classic* n’est pas ultérieure à une année spécifiée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-416">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="f8e3b-417">L’attribut `[ClassicMovie2]` vérifie d’abord le genre, et continue uniquement s’il s’agit de *Classic*.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-417">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="f8e3b-418">Pour les films identifiés comme des classiques, il vérifie si la date de sortie n’est pas ultérieure à la limite passée au constructeur d’attribut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-418">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="f8e3b-419">La variable `movie` de l’exemple précédent représente un objet `Movie` qui contient les données de l’envoi du formulaire.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-419">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="f8e3b-420">La méthode `IsValid` vérifie la date et le genre.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-420">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="f8e3b-421">Si la validation réussit, `IsValid` retourne un code `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-421">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="f8e3b-422">Quand la validation échoue, un `ValidationResult` avec un message d’erreur est retourné.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-422">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="f8e3b-423">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="f8e3b-423">IValidatableObject</span></span>

<span data-ttu-id="f8e3b-424">L’exemple précédent fonctionne uniquement avec les types `Movie`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-424">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="f8e3b-425">Une autre option de validation au niveau de la classe consiste à implémenter `IValidatableObject` dans la classe de modèle, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-425">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="f8e3b-426">Validation du nœud de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-426">Top-level node validation</span></span>

<span data-ttu-id="f8e3b-427">Les nœuds de niveau supérieur incluent les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-427">Top-level nodes include:</span></span>

* <span data-ttu-id="f8e3b-428">Paramètres d’action</span><span class="sxs-lookup"><span data-stu-id="f8e3b-428">Action parameters</span></span>
* <span data-ttu-id="f8e3b-429">Propriétés du contrôleur</span><span class="sxs-lookup"><span data-stu-id="f8e3b-429">Controller properties</span></span>
* <span data-ttu-id="f8e3b-430">Paramètres du gestionnaire de page</span><span class="sxs-lookup"><span data-stu-id="f8e3b-430">Page handler parameters</span></span>
* <span data-ttu-id="f8e3b-431">Propriétés du modèle de page</span><span class="sxs-lookup"><span data-stu-id="f8e3b-431">Page model properties</span></span>

<span data-ttu-id="f8e3b-432">Les nœuds de niveau supérieur liés au modèle sont validés en plus de la validation des propriétés du modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-432">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="f8e3b-433">Dans l’exemple suivant tiré de l’exemple d’application, la méthode `VerifyPhone` utilise <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> pour valider le paramètre d’action `phone` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-433">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="f8e3b-434">Les nœuds de niveau supérieur peuvent utiliser <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> avec des attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-434">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="f8e3b-435">Dans l’exemple suivant de l’exemple d’application, la méthode `CheckAge` spécifie que le paramètre `age` doit être lié à partir de la chaîne de requête au moment de l’envoi du formulaire :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-435">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="f8e3b-436">Dans la page de vérification de l’âge (*CheckAge.cshtml*), il existe deux formulaires.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-436">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="f8e3b-437">Le premier formulaire envoie une valeur `Age` égale à `99` en tant que chaîne de requête : `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-437">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="f8e3b-438">Quand un paramètre `age` au format approprié est envoyé à partir de la chaîne de requête, le formulaire est validé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-438">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="f8e3b-439">Le second formulaire de la page de vérification de l’âge envoie la valeur `Age` dans le corps de la requête, ce qui entraîne un échec de la validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-439">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="f8e3b-440">L’échec de la liaison est dû au fait que le paramètre `age` doit provenir d’une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-440">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="f8e3b-441">Lors de l’exécution avec `CompatibilityVersion.Version_2_1` ou version ultérieure, la validation du nœud de niveau supérieur est activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-441">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="f8e3b-442">Autrement, la validation du nœud de niveau supérieur est désactivée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-442">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="f8e3b-443">L’option par défaut peut être remplacée en définissant la propriété <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> dans (`Startup.ConfigureServices`), comme illustré ici :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-443">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="f8e3b-444">Quantité maximale d’erreurs</span><span class="sxs-lookup"><span data-stu-id="f8e3b-444">Maximum errors</span></span>

<span data-ttu-id="f8e3b-445">La validation s’arrête quand le nombre maximal d’erreurs est atteint (200 par défaut).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-445">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="f8e3b-446">Vous pouvez configurer ce nombre avec le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-446">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="f8e3b-447">Récursivité maximale</span><span class="sxs-lookup"><span data-stu-id="f8e3b-447">Maximum recursion</span></span>

<span data-ttu-id="f8e3b-448"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> parcourt le graphe d’objet du modèle en cours de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-448"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="f8e3b-449">Pour les modèles très profonds ou infiniment récursifs, la validation peut entraîner un dépassement de la capacité de la pile.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-449">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="f8e3b-450">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) fournit un moyen d’interrompre la validation de manière anticipée si la récursivité du visiteur dépasse une profondeur configurée.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-450">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="f8e3b-451">La valeur par défaut de `MvcOptions.MaxValidationDepth` est 200 lors de l’exécution avec `CompatibilityVersion.Version_2_2` ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-451">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="f8e3b-452">Pour les versions antérieures, la valeur est Null, ce qui signifie qu’il n’y a aucune contrainte de profondeur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-452">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="f8e3b-453">Court-circuit automatique</span><span class="sxs-lookup"><span data-stu-id="f8e3b-453">Automatic short-circuit</span></span>

<span data-ttu-id="f8e3b-454">La validation est automatiquement court-circuitée (ignorée) si le graphe du modèle ne nécessite pas de validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-454">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="f8e3b-455">Les objets pour lesquels le runtime ignore la validation comprennent les collections de primitives (telles que `byte[]`, `string[]`, `Dictionary<string, string>`) et les graphes d’objets complexes qui n’ont pas de validateur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-455">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="f8e3b-456">Désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-456">Disable validation</span></span>

<span data-ttu-id="f8e3b-457">Pour désactiver la validation</span><span class="sxs-lookup"><span data-stu-id="f8e3b-457">To disable validation:</span></span>

1. <span data-ttu-id="f8e3b-458">Créez une implémentation de `IObjectModelValidator` qui ne marque aucun champ comme étant non valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-458">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="f8e3b-459">Ajoutez le code suivant à `Startup.ConfigureServices` pour remplacer l’implémentation par défaut de `IObjectModelValidator` dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-459">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="f8e3b-460">Vous risquez toujours de voir des erreurs d’état du modèle provenant de la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-460">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="f8e3b-461">Validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-461">Client-side validation</span></span>

<span data-ttu-id="f8e3b-462">La validation côté client empêche l’envoi jusqu’à ce que le formulaire soit valide.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-462">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="f8e3b-463">Le bouton Submit exécute le code JavaScript qui envoie le formulaire ou qui affiche des messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-463">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="f8e3b-464">La validation côté client permet d’éviter un aller-retour inutile vers le serveur quand il existe des erreurs d’entrée sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-464">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="f8e3b-465">Les références de script suivantes dans *_Layout.cshtml* et *_ValidationScriptsPartial.cshtml* prennent en charge la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-465">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="f8e3b-466">Le script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) est une bibliothèque frontale personnalisée de Microsoft qui s’appuie sur le plug-in bien connu [jQuery Validate](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-466">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="f8e3b-467">Sans jQuery Unobtrusive Validation, vous devriez coder la même logique de validation à deux endroits : une fois dans les attributs de validation côté serveur sur les propriétés du modèle, puis à nouveau dans les scripts côté client.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-467">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="f8e3b-468">Au lieu de cela, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les attributs de validation et les métadonnées de type des propriétés du modèle afin de restituer les attributs `data-` HTML 5 pour les éléments de formulaire nécessitant une validation.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-468">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="f8e3b-469">jQuery Unobtrusive Validation analyse les attributs `data-` et passe la logique à jQuery Validate, en « copiant » la logique de validation côté serveur vers le client.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-469">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="f8e3b-470">Vous pouvez afficher les erreurs de validation sur le client en utilisant des Tag Helpers, comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-470">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="f8e3b-471">Les Tag Helpers précédents restituent le code HTML suivant.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-471">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="f8e3b-472">Notez que les attributs `data-` dans la sortie HTML correspondent aux attributs de validation pour la propriété `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-472">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="f8e3b-473">L’attribut `data-val-required` contient un message d’erreur à afficher si l’utilisateur ne renseigne pas le champ correspondant à la date de sortie.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-473">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="f8e3b-474">jQuery Unobtrusive Validation passe cette valeur à la méthode [`required()`](https://jqueryvalidation.org/required-method/) de jQuery Validate, qui affiche alors ce message dans l’élément **\<span>** qui l’accompagne.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-474">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="f8e3b-475">La validation du type de données est basée sur le type .NET d’une propriété, sauf en cas de substitution par un attribut `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-475">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="f8e3b-476">Les navigateurs ont leurs propres messages d’erreur par défaut, mais le package jQuery Validation Unobtrusive Validation peut remplacer ces messages.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-476">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="f8e3b-477">Les attributs `[DataType]` et les sous-classes comme `[EmailAddress]` vous permettent de spécifier le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-477">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="f8e3b-478">Ajouter une validation à des formulaires dynamiques</span><span class="sxs-lookup"><span data-stu-id="f8e3b-478">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="f8e3b-479">jQuery Unobtrusive Validation passe la logique et les paramètres de validation à jQuery Validate lors du premier chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-479">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="f8e3b-480">Par conséquent, la validation ne fonctionne pas automatiquement sur les formulaires générés de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-480">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="f8e3b-481">Pour activer la validation, vous devez faire en sorte que jQuery Validate analyse le formulaire dynamique immédiatement après l’avoir créé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-481">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="f8e3b-482">Par exemple, le code suivant définit la validation côté client sur un formulaire ajouté par le biais d’AJAX.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-482">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="f8e3b-483">La méthode `$.validator.unobtrusive.parse()` accepte un sélecteur jQuery comme argument.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-483">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="f8e3b-484">Cette méthode indique à jQuery Unobtrusive Validation d’analyser les attributs `data-` des formulaires dans ce sélecteur.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-484">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="f8e3b-485">Les valeurs de ces attributs sont ensuite passées au plug-in jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-485">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="f8e3b-486">Ajouter une validation à des contrôles dynamiques</span><span class="sxs-lookup"><span data-stu-id="f8e3b-486">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="f8e3b-487">La méthode `$.validator.unobtrusive.parse()` opère sur un formulaire entier, et non sur des contrôles individuels générés de manière dynamique tels que `<input>` et `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-487">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="f8e3b-488">Pour réanalyser le formulaire, supprimez les données de validation qui ont été ajoutées quand le formulaire a été analysé précédemment, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-488">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="f8e3b-489">Validation personnalisée côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-489">Custom client-side validation</span></span>

<span data-ttu-id="f8e3b-490">La validation personnalisée côté client s’effectue en générant des attributs HTML `data-` qui fonctionnent avec un adaptateur jQuery Validate personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-490">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="f8e3b-491">L’exemple de code d’adaptateur suivant a été écrit pour les attributs `ClassicMovie` et `ClassicMovie2` qui ont été introduits plus haut dans cet article :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-491">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="f8e3b-492">Pour plus d’informations sur la façon d’écrire des adaptateurs, consultez la [documentation de jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-492">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="f8e3b-493">L’utilisation d’un adaptateur pour un champ donné est déclenchée par des attributs `data-` qui :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-493">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="f8e3b-494">Marquent le champ comme étant soumis à validation (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-494">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="f8e3b-495">Identifient un nom de règle de validation et un texte de message d’erreur (par exemple, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-495">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="f8e3b-496">Fournissent les éventuels paramètres supplémentaires dont le validateur a besoin (par exemple, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-496">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="f8e3b-497">L’exemple suivant montre les attributs `data-` pour l’attribut `ClassicMovie` de l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-497">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="f8e3b-498">Comme mentionné plus haut, les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les [helpers HTML](xref:mvc/views/overview) utilisent les informations des attributs de validation pour restituer les attributs `data-`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-498">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="f8e3b-499">Il existe deux options pour l’écriture de code qui entraîne la création d’attributs HTML `data-` personnalisés :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-499">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="f8e3b-500">Créez une classe qui dérive de `AttributeAdapterBase<TAttribute>` et une classe qui implémente `IValidationAttributeAdapterProvider`, et inscrivez votre attribut et son adaptateur dans l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-500">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="f8e3b-501">Cette méthode respecte le [principe de responsabilité unique](https://wikipedia.org/wiki/Single_responsibility_principle), dans la mesure où le code de validation lié au serveur et celui lié au client se trouvent dans des classes distinctes.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-501">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="f8e3b-502">Il existe un autre avantage : l’adaptateur étant inscrit dans l’injection de dépendances, les autres services dans l’injection de dépendances lui sont accessibles si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-502">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="f8e3b-503">Implémentez `IClientModelValidator` dans votre classe `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-503">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="f8e3b-504">Cette méthode peut être appropriée si l’attribut n’effectue aucune validation côté serveur et n’a besoin d’aucun service à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-504">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="f8e3b-505">AttributeAdapter pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-505">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="f8e3b-506">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovie` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-506">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="f8e3b-507">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="f8e3b-507">To add client validation by using this method:</span></span>

1. <span data-ttu-id="f8e3b-508">Créez une classe d’adaptateurs d’attributs pour l’attribut de validation personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-508">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="f8e3b-509">Dérivez la classe de [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="f8e3b-509">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="f8e3b-510">Créez une méthode `AddValidation` qui ajoute des attributs `data-` à la sortie restituée, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-510">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="f8e3b-511">Créez une classe de fournisseurs d’adaptateurs qui implémente <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-511">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="f8e3b-512">Dans la méthode `GetAttributeAdapter`, passez l’attribut personnalisé au constructeur de l’adaptateur, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-512">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="f8e3b-513">Inscrivez le fournisseur d’adaptateurs auprès de l’injection de dépendances dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-513">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="f8e3b-514">IClientModelValidator pour la validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-514">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="f8e3b-515">Cette méthode de rendu des attributs `data-` en HTML est utilisée par l’attribut `ClassicMovie2` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-515">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="f8e3b-516">Pour ajouter la validation côté client à l’aide de cette méthode</span><span class="sxs-lookup"><span data-stu-id="f8e3b-516">To add client validation by using this method:</span></span>

* <span data-ttu-id="f8e3b-517">Dans l’attribut de validation personnalisé, implémentez l’interface `IClientModelValidator` et créez une méthode `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-517">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="f8e3b-518">Dans la méthode `AddValidation`, ajoutez des attributs `data-` pour la validation, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-518">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="f8e3b-519">Désactiver la validation côté client</span><span class="sxs-lookup"><span data-stu-id="f8e3b-519">Disable client-side validation</span></span>

<span data-ttu-id="f8e3b-520">Le code suivant désactive la validation côté client dans les vues MVC :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-520">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="f8e3b-521">Et dans Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="f8e3b-521">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="f8e3b-522">Une autre option permettant de désactiver la validation côté client consiste à commenter la référence à `_ValidationScriptsPartial` dans votre fichier *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f8e3b-522">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8e3b-523">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f8e3b-523">Additional resources</span></span>

* [<span data-ttu-id="f8e3b-524">Espace de noms System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="f8e3b-524">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="f8e3b-525">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="f8e3b-525">Model Binding</span></span>](model-binding.md)

::: moniker-end
