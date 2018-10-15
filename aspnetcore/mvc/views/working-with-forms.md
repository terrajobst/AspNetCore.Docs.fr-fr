---
title: Tag Helpers dans les formulaires dans ASP.NET Core
author: rick-anderson
description: Décrit les Tag Helpers intégrés, utilisés avec des formulaires.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
uid: mvc/views/working-with-forms
ms.openlocfilehash: e613dc1e85b84cc5e2b8ad2bf3958040257d1966
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911277"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="2c01f-103">Tag Helpers dans les formulaires dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c01f-103">Tag Helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="2c01f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette) et [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="2c01f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="2c01f-105">Ce document montre comment utiliser les formulaires, ainsi que les éléments HTML couramment utilisés dans un formulaire.</span><span class="sxs-lookup"><span data-stu-id="2c01f-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="2c01f-106">L’élément HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) fournit le principal mécanisme utilisé par les applications web pour publier des données sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2c01f-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="2c01f-107">La majeure partie de ce document décrit les [Tag Helpers](tag-helpers/intro.md) et explique comment ils peuvent vous aider à créer des formulaires HTML robustes.</span><span class="sxs-lookup"><span data-stu-id="2c01f-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="2c01f-108">Nous vous recommandons de lire [Introduction aux Tag Helpers](tag-helpers/intro.md) avant de lire ce document.</span><span class="sxs-lookup"><span data-stu-id="2c01f-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="2c01f-109">Dans de nombreux cas, les HTML Helpers offrent une autre approche par rapport à un Tag Helper spécifique. Toutefois, il est clair que les Tag Helpers ne remplacent pas les HTML Helpers, et qu’il n’existe pas toujours un Tag Helper pour chaque HTML Helper.</span><span class="sxs-lookup"><span data-stu-id="2c01f-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="2c01f-110">Quand une alternative HTML Helper existe, elle est mentionnée.</span><span class="sxs-lookup"><span data-stu-id="2c01f-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="2c01f-111">Tag Helper Form</span><span class="sxs-lookup"><span data-stu-id="2c01f-111">The Form Tag Helper</span></span>

<span data-ttu-id="2c01f-112">Le Tag Helper [Form](https://www.w3.org/TR/html401/interact/forms.html) :</span><span class="sxs-lookup"><span data-stu-id="2c01f-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="2c01f-113">Génère pour la balise HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) la valeur de l’attribut `action` d’un routage nommé ou d’une action de contrôleur MVC</span><span class="sxs-lookup"><span data-stu-id="2c01f-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="2c01f-114">Génère un [jeton de vérification de requête](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) masqué pour empêcher la falsification de requête intersites (quand il est utilisé avec l’attribut `[ValidateAntiForgeryToken]` dans la méthode d’action HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="2c01f-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="2c01f-115">Fournit l’attribut `asp-route-<Parameter Name>`, où `<Parameter Name>` est ajouté aux valeurs de routage.</span><span class="sxs-lookup"><span data-stu-id="2c01f-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="2c01f-116">Les paramètres `routeValues` de `Html.BeginForm` et `Html.BeginRouteForm` fournissent des fonctionnalités similaires.</span><span class="sxs-lookup"><span data-stu-id="2c01f-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="2c01f-117">Comporte une alternative HTML Helper avec `Html.BeginForm` et `Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="2c01f-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="2c01f-118">Aperçu :</span><span class="sxs-lookup"><span data-stu-id="2c01f-118">Sample:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="2c01f-119">Le Tag Helper Form ci-dessus génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="2c01f-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

<span data-ttu-id="2c01f-120">Le runtime MVC génère la valeur de l’attribut `action` à partir des attributs `asp-controller` et `asp-action` du Tag Helper Form.</span><span class="sxs-lookup"><span data-stu-id="2c01f-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="2c01f-121">Le Tag Helper Form génère également un [jeton de vérification de requête](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) masqué pour empêcher la falsification de requête intersites (quand il est utilisé avec l’attribut `[ValidateAntiForgeryToken]` dans la méthode d’action HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="2c01f-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="2c01f-122">Il est difficile de protéger un formulaire HTML contre une falsification de requête intersites. Le Tag Helper Form se charge de fournir ce service à votre place.</span><span class="sxs-lookup"><span data-stu-id="2c01f-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="2c01f-123">Utilisation d’un routage nommé</span><span class="sxs-lookup"><span data-stu-id="2c01f-123">Using a named route</span></span>

<span data-ttu-id="2c01f-124">L’attribut Tag Helper `asp-route` peut également générer des balises pour l’attribut HTML `action`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="2c01f-125">Une application avec un [routage](../../fundamentals/routing.md) nommé `register` peut utiliser les balises suivantes pour la page d’inscription :</span><span class="sxs-lookup"><span data-stu-id="2c01f-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="2c01f-126">Un bon nombre des vues du dossier *Vues/Compte* (généré quand vous créez une application web avec des *comptes d’utilisateurs individuels*) contiennent l’attribut [asp-route-returnurl](xref:mvc/views/working-with-forms) :</span><span class="sxs-lookup"><span data-stu-id="2c01f-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](xref:mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="2c01f-127">Avec les modèles intégrés, `returnUrl` est uniquement rempli automatiquement quand vous essayez d’accéder à une ressource autorisée sans l’authentification ou l’autorisation nécessaire.</span><span class="sxs-lookup"><span data-stu-id="2c01f-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="2c01f-128">Quand vous tentez d’effectuer un accès non autorisé, l’intergiciel (middleware) de sécurité vous redirige vers la page de connexion avec le `returnUrl` défini.</span><span class="sxs-lookup"><span data-stu-id="2c01f-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="2c01f-129">Tag Helper Input</span><span class="sxs-lookup"><span data-stu-id="2c01f-129">The Input Tag Helper</span></span>

<span data-ttu-id="2c01f-130">Le Tag Helper Input lie un élément HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) à une expression de modèle dans votre vue Razor.</span><span class="sxs-lookup"><span data-stu-id="2c01f-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="2c01f-131">Syntaxe :</span><span class="sxs-lookup"><span data-stu-id="2c01f-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="2c01f-132">Tag Helper Input :</span><span class="sxs-lookup"><span data-stu-id="2c01f-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="2c01f-133">Génère les attributs HTML `id` et `name` pour le nom d’expression spécifié dans l’attribut `asp-for`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="2c01f-134">`asp-for="Property1.Property2"` équivaut à `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="2c01f-135">Nom de l’expression utilisée pour la valeur de l’attribut `asp-for`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="2c01f-136">Pour plus d’informations, consultez la section [Noms d’expressions](#expression-names).</span><span class="sxs-lookup"><span data-stu-id="2c01f-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="2c01f-137">Définit la valeur de l’attribut HTML `type` en fonction du type de modèle et des attributs d’[annotation de données](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) appliqués à la propriété de modèle</span><span class="sxs-lookup"><span data-stu-id="2c01f-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="2c01f-138">Ne remplace pas la valeur de l’attribut HTML `type` quand une valeur est spécifiée</span><span class="sxs-lookup"><span data-stu-id="2c01f-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="2c01f-139">Génère des attributs de validation [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) à partir des attributs d’[annotation de données](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) appliqués aux propriétés de modèle</span><span class="sxs-lookup"><span data-stu-id="2c01f-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="2c01f-140">Chevauche des fonctionnalités HTML Helper avec `Html.TextBoxFor` et `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="2c01f-141">Pour plus d’informations, consultez la section **Alternatives HTML Helper au Tag Helper Input**.</span><span class="sxs-lookup"><span data-stu-id="2c01f-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="2c01f-142">Fournit un typage fort.</span><span class="sxs-lookup"><span data-stu-id="2c01f-142">Provides strong typing.</span></span> <span data-ttu-id="2c01f-143">Si le nom de la propriété change et si vous ne mettez pas à jour le Tag Helper, vous obtenez une erreur similaire à celle-ci :</span><span class="sxs-lookup"><span data-stu-id="2c01f-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="2c01f-144">Le Tag Helper `Input` définit l’attribut HTML `type` en fonction du type .NET.</span><span class="sxs-lookup"><span data-stu-id="2c01f-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="2c01f-145">Le tableau suivant liste certains types .NET usuels et le type HTML généré (tous les types .NET ne sont pas listés).</span><span class="sxs-lookup"><span data-stu-id="2c01f-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="2c01f-146">Type .NET</span><span class="sxs-lookup"><span data-stu-id="2c01f-146">.NET type</span></span>|<span data-ttu-id="2c01f-147">Type d’entrée</span><span class="sxs-lookup"><span data-stu-id="2c01f-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="2c01f-148">Bool</span><span class="sxs-lookup"><span data-stu-id="2c01f-148">Bool</span></span>|<span data-ttu-id="2c01f-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="2c01f-149">type=”checkbox”</span></span>|
|<span data-ttu-id="2c01f-150">Chaîne</span><span class="sxs-lookup"><span data-stu-id="2c01f-150">String</span></span>|<span data-ttu-id="2c01f-151">type=”text”</span><span class="sxs-lookup"><span data-stu-id="2c01f-151">type=”text”</span></span>|
|<span data-ttu-id="2c01f-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="2c01f-152">DateTime</span></span>|<span data-ttu-id="2c01f-153">type=”datetime”</span><span class="sxs-lookup"><span data-stu-id="2c01f-153">type=”datetime”</span></span>|
|<span data-ttu-id="2c01f-154">Byte</span><span class="sxs-lookup"><span data-stu-id="2c01f-154">Byte</span></span>|<span data-ttu-id="2c01f-155">type=”number”</span><span class="sxs-lookup"><span data-stu-id="2c01f-155">type=”number”</span></span>|
|<span data-ttu-id="2c01f-156">Int</span><span class="sxs-lookup"><span data-stu-id="2c01f-156">Int</span></span>|<span data-ttu-id="2c01f-157">type=”number”</span><span class="sxs-lookup"><span data-stu-id="2c01f-157">type=”number”</span></span>|
|<span data-ttu-id="2c01f-158">Single, Double</span><span class="sxs-lookup"><span data-stu-id="2c01f-158">Single, Double</span></span>|<span data-ttu-id="2c01f-159">type=”number”</span><span class="sxs-lookup"><span data-stu-id="2c01f-159">type=”number”</span></span>|


<span data-ttu-id="2c01f-160">Le tableau suivant présente des attributs d’[annotations de données](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) usuels que le Tag Helper Input mappe à des types d’entrée spécifiques (tous les attributs de validation ne sont pas listés) :</span><span class="sxs-lookup"><span data-stu-id="2c01f-160">The following table shows some common [data annotations](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="2c01f-161">Attribut</span><span class="sxs-lookup"><span data-stu-id="2c01f-161">Attribute</span></span>|<span data-ttu-id="2c01f-162">Type d’entrée</span><span class="sxs-lookup"><span data-stu-id="2c01f-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="2c01f-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="2c01f-163">[EmailAddress]</span></span>|<span data-ttu-id="2c01f-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="2c01f-164">type=”email”</span></span>|
|<span data-ttu-id="2c01f-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="2c01f-165">[Url]</span></span>|<span data-ttu-id="2c01f-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="2c01f-166">type=”url”</span></span>|
|<span data-ttu-id="2c01f-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="2c01f-167">[HiddenInput]</span></span>|<span data-ttu-id="2c01f-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="2c01f-168">type=”hidden”</span></span>|
|<span data-ttu-id="2c01f-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="2c01f-169">[Phone]</span></span>|<span data-ttu-id="2c01f-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="2c01f-170">type=”tel”</span></span>|
|<span data-ttu-id="2c01f-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="2c01f-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="2c01f-172">type=”password”</span><span class="sxs-lookup"><span data-stu-id="2c01f-172">type=”password”</span></span>|
|<span data-ttu-id="2c01f-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="2c01f-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="2c01f-174">type=”date”</span><span class="sxs-lookup"><span data-stu-id="2c01f-174">type=”date”</span></span>|
|<span data-ttu-id="2c01f-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="2c01f-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="2c01f-176">type=”time”</span><span class="sxs-lookup"><span data-stu-id="2c01f-176">type=”time”</span></span>|


<span data-ttu-id="2c01f-177">Aperçu :</span><span class="sxs-lookup"><span data-stu-id="2c01f-177">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="2c01f-178">Le code ci-dessus génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="2c01f-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="2c01f-179">Les annotations de données appliquées aux propriétés `Email` et `Password` génèrent des métadonnées pour le modèle.</span><span class="sxs-lookup"><span data-stu-id="2c01f-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="2c01f-180">Le Tag Helper Input consomme les métadonnées du modèle et génère les attributs [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` (consultez [Validation de modèle](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="2c01f-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="2c01f-181">Ces attributs décrivent les validateurs à attacher aux champs d’entrée.</span><span class="sxs-lookup"><span data-stu-id="2c01f-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="2c01f-182">Cela permet d’effectuer une validation HTML5 et [jQuery](https://jquery.com/) discrète.</span><span class="sxs-lookup"><span data-stu-id="2c01f-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="2c01f-183">Les attributs discrets ont le format `data-val-rule="Error Message"`, où rule est le nom de la règle de validation (par exemple `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) Si un message d’erreur est fourni dans l’attribut, il est affiché en tant que valeur de l’attribut `data-val-rule`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="2c01f-184">Il existe également des attributs ayant la forme `data-val-ruleName-argumentName="argumentValue"` et qui fournissent des détails supplémentaires sur la règle, par exemple `data-val-maxlength-max="1024"`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="2c01f-185">Alternatives HTML Helper au Tag Helper Input</span><span class="sxs-lookup"><span data-stu-id="2c01f-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="2c01f-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` et `Html.EditorFor` ont des fonctionnalités qui chevauchent celles du Tag Helper Input.</span><span class="sxs-lookup"><span data-stu-id="2c01f-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="2c01f-187">Le Tag Helper Input définit automatiquement l’attribut `type`, contrairement à `Html.TextBox` et `Html.TextBoxFor`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="2c01f-188">`Html.Editor` et `Html.EditorFor` gèrent les collections, les objets complexes et les modèles, contrairement au Tag Helper Input.</span><span class="sxs-lookup"><span data-stu-id="2c01f-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="2c01f-189">Le Tag Helper Input, `Html.EditorFor` et `Html.TextBoxFor` sont fortement typés (ils utilisent des expressions lambda), contrairement à `Html.TextBox` et `Html.Editor` (qui utilisent des noms d’expressions).</span><span class="sxs-lookup"><span data-stu-id="2c01f-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="2c01f-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="2c01f-190">HtmlAttributes</span></span>

<span data-ttu-id="2c01f-191">`@Html.Editor()` et `@Html.EditorFor()` utilisent une entrée `ViewDataDictionary` spéciale nommée `htmlAttributes` durant l’exécution de leurs modèles par défaut.</span><span class="sxs-lookup"><span data-stu-id="2c01f-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="2c01f-192">Ce comportement est éventuellement amélioré à l’aide des paramètres `additionalViewData`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="2c01f-193">La clé « htmlAttributes » ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="2c01f-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="2c01f-194">La clé « htmlAttributes » est prise en charge de manière similaire à l’objet `htmlAttributes` passé aux Helpers d’entrée tels que `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="2c01f-195">Noms d’expressions</span><span class="sxs-lookup"><span data-stu-id="2c01f-195">Expression names</span></span>

<span data-ttu-id="2c01f-196">La valeur de l’attribut `asp-for` est un `ModelExpression` et correspond au côté droit d’une expression lambda.</span><span class="sxs-lookup"><span data-stu-id="2c01f-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="2c01f-197">Ainsi, `asp-for="Property1"` devient `m => m.Property1` dans le code généré, ce qui explique pourquoi vous n’avez pas besoin de le faire commencer par `Model`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="2c01f-198">Vous pouvez utiliser le caractère « \@ » pour débuter une expression inline avant `m.` :</span><span class="sxs-lookup"><span data-stu-id="2c01f-198">You can use the "\@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="2c01f-199">Génère ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="2c01f-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="2c01f-200">Avec les propriétés de collection, `asp-for="CollectionProperty[23].Member"` génère le même nom que `asp-for="CollectionProperty[i].Member"` quand `i` a la valeur `23`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

<span data-ttu-id="2c01f-201">Quand ASP.NET Core MVC calcule la valeur de `ModelExpression`, plusieurs sources sont inspectées, notamment `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-201">When ASP.NET Core MVC calculates the value of `ModelExpression`, it inspects several sources, including `ModelState`.</span></span> <span data-ttu-id="2c01f-202">Prenez le cas de `<input type="text" asp-for="@Name" />`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-202">Consider `<input type="text" asp-for="@Name" />`.</span></span> <span data-ttu-id="2c01f-203">L’attribut `value` calculé est la première valeur non-null des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="2c01f-203">The calculated `value` attribute is the first non-null value from:</span></span>

* <span data-ttu-id="2c01f-204">Entrée `ModelState` avec la clé « Name ».</span><span class="sxs-lookup"><span data-stu-id="2c01f-204">`ModelState` entry with key "Name".</span></span>
* <span data-ttu-id="2c01f-205">Résultat de l’expression `Model.Name`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-205">Result of the expression `Model.Name`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="2c01f-206">Navigation dans les propriétés enfants</span><span class="sxs-lookup"><span data-stu-id="2c01f-206">Navigating child properties</span></span>

<span data-ttu-id="2c01f-207">Vous pouvez également accéder aux propriétés enfants à l’aide du chemin de propriété du modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="2c01f-207">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="2c01f-208">Prenons le cas d’une classe de modèle plus complexe qui contient une propriété enfant `Address`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-208">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="2c01f-209">Dans la vue, nous effectuons une liaison à `Address.AddressLine1` :</span><span class="sxs-lookup"><span data-stu-id="2c01f-209">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="2c01f-210">Le code HTML suivant est généré pour `Address.AddressLine1` :</span><span class="sxs-lookup"><span data-stu-id="2c01f-210">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="2c01f-211">Noms d’expressions et collections</span><span class="sxs-lookup"><span data-stu-id="2c01f-211">Expression names and Collections</span></span>

<span data-ttu-id="2c01f-212">Dans cet exemple, un modèle contient un tableau de `Colors` :</span><span class="sxs-lookup"><span data-stu-id="2c01f-212">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="2c01f-213">Méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="2c01f-213">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="2c01f-214">Le code Razor suivant montre comment accéder à un élément `Color` spécifique :</span><span class="sxs-lookup"><span data-stu-id="2c01f-214">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="2c01f-215">Modèle *Views/Shared/EditorTemplates/String.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2c01f-215">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="2c01f-216">Exemple utilisant `List<T>` :</span><span class="sxs-lookup"><span data-stu-id="2c01f-216">Sample using `List<T>`:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="2c01f-217">Le code Razor suivant montre comment effectuer une itération dans une collection :</span><span class="sxs-lookup"><span data-stu-id="2c01f-217">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="2c01f-218">Modèle *Views/Shared/EditorTemplates/ToDoItem.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2c01f-218">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="2c01f-219">Utilisez toujours `for` (et *non* `foreach`) pour effectuer une itération dans une liste.</span><span class="sxs-lookup"><span data-stu-id="2c01f-219">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="2c01f-220">L’évaluation d’un indexeur dans une expression LINQ peut être lourde. Elle doit être réduite autant que possible.</span><span class="sxs-lookup"><span data-stu-id="2c01f-220">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="2c01f-221">L’exemple de code commenté ci-dessus montre comment remplacer l’expression lambda par l’opérateur `@` pour accéder à chaque `ToDoItem` dans la liste.</span><span class="sxs-lookup"><span data-stu-id="2c01f-221">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="2c01f-222">Tag Helper Textarea</span><span class="sxs-lookup"><span data-stu-id="2c01f-222">The Textarea Tag Helper</span></span>

<span data-ttu-id="2c01f-223">Le Tag Helper `Textarea Tag Helper` est similaire au Tag Helper Input.</span><span class="sxs-lookup"><span data-stu-id="2c01f-223">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="2c01f-224">Génère les attributs `id` et `name`, ainsi que les attributs de validation des données du modèle pour un élément [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea).</span><span class="sxs-lookup"><span data-stu-id="2c01f-224">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="2c01f-225">Fournit un typage fort.</span><span class="sxs-lookup"><span data-stu-id="2c01f-225">Provides strong typing.</span></span>

* <span data-ttu-id="2c01f-226">Alternative HTML Helper : `Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="2c01f-226">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="2c01f-227">Aperçu :</span><span class="sxs-lookup"><span data-stu-id="2c01f-227">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="2c01f-228">Le code HTML suivant est généré :</span><span class="sxs-lookup"><span data-stu-id="2c01f-228">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="2c01f-229">Tag Helper Label</span><span class="sxs-lookup"><span data-stu-id="2c01f-229">The Label Tag Helper</span></span>

* <span data-ttu-id="2c01f-230">Génère la légende d’étiquette et l’attribut `for` d’un élément [<label>](https://www.w3.org/wiki/HTML/Elements/label) pour un nom d’expression</span><span class="sxs-lookup"><span data-stu-id="2c01f-230">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="2c01f-231">Alternative HTML Helper : `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-231">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="2c01f-232">Le `Label Tag Helper` offre les avantages suivants sur un élément d’étiquette HTML pur :</span><span class="sxs-lookup"><span data-stu-id="2c01f-232">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="2c01f-233">Vous obtenez automatiquement la valeur d’étiquette descriptive à partir de l’attribut `Display`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-233">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="2c01f-234">Le nom d’affichage prévu peut changer plus tard. La combinaison de l’attribut `Display` et du Tag Helper Label applique `Display` partout où il est utilisé.</span><span class="sxs-lookup"><span data-stu-id="2c01f-234">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="2c01f-235">Moins de balises dans le code source</span><span class="sxs-lookup"><span data-stu-id="2c01f-235">Less markup in source code</span></span>

* <span data-ttu-id="2c01f-236">Typage fort avec la propriété de modèle.</span><span class="sxs-lookup"><span data-stu-id="2c01f-236">Strong typing with the model property.</span></span>

<span data-ttu-id="2c01f-237">Aperçu :</span><span class="sxs-lookup"><span data-stu-id="2c01f-237">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="2c01f-238">Le code HTML suivant est généré pour l’élément `<label>` :</span><span class="sxs-lookup"><span data-stu-id="2c01f-238">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="2c01f-239">Le Tag Helper Label a généré la valeur « Email » pour l’attribut `for`, qui représente l’ID associé à l’élément `<input>`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-239">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="2c01f-240">Les Tag Helpers génèrent des éléments `id` et `for` cohérents pour qu’ils puissent être correctement associés.</span><span class="sxs-lookup"><span data-stu-id="2c01f-240">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="2c01f-241">La légende de cet exemple provient de l’attribut `Display`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-241">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="2c01f-242">Si le modèle ne contient pas d’attribut `Display`, la légende correspond au nom de propriété de l’expression.</span><span class="sxs-lookup"><span data-stu-id="2c01f-242">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="2c01f-243">Tag Helpers Validation</span><span class="sxs-lookup"><span data-stu-id="2c01f-243">The Validation Tag Helpers</span></span>

<span data-ttu-id="2c01f-244">Il existe deux Tag Helpers Validation.</span><span class="sxs-lookup"><span data-stu-id="2c01f-244">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="2c01f-245">Le `Validation Message Tag Helper` (qui affiche un message de validation pour une seule propriété de votre modèle) et le `Validation Summary Tag Helper` (qui affiche un récapitulatif des erreurs de validation).</span><span class="sxs-lookup"><span data-stu-id="2c01f-245">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="2c01f-246">Le `Input Tag Helper` ajoute des attributs de validation HTML5 côté client aux éléments d’entrée en fonction des attributs d’annotation de données pour vos classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="2c01f-246">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="2c01f-247">La validation est également effectuée sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2c01f-247">Validation is also performed on the server.</span></span> <span data-ttu-id="2c01f-248">Le Tag Helper Validation affiche ces messages d’erreur quand une erreur de validation se produit.</span><span class="sxs-lookup"><span data-stu-id="2c01f-248">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="2c01f-249">Le Tag Helper Validation Message</span><span class="sxs-lookup"><span data-stu-id="2c01f-249">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="2c01f-250">Ajoute l’attribut [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` à l’élément [span](https://developer.mozilla.org/docs/Web/HTML/Element/span), qui attache les messages d’erreur de validation du champ d’entrée de la propriété de modèle spécifiée.</span><span class="sxs-lookup"><span data-stu-id="2c01f-250">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="2c01f-251">Quand une erreur de validation côté client se produit, [jQuery](https://jquery.com/) affiche le message d’erreur dans l’élément `<span>`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-251">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="2c01f-252">La validation a également lieu sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2c01f-252">Validation also takes place on the server.</span></span> <span data-ttu-id="2c01f-253">Il arrive que JavaScript soit désactivé sur les clients et qu’une partie de la validation puisse être effectuée uniquement côté serveur.</span><span class="sxs-lookup"><span data-stu-id="2c01f-253">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="2c01f-254">Alternative HTML Helper : `Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="2c01f-254">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="2c01f-255">Le `Validation Message Tag Helper` est utilisé avec l’attribut `asp-validation-for` sur un élément HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span).</span><span class="sxs-lookup"><span data-stu-id="2c01f-255">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="2c01f-256">Le Tag Helper Validation Message génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="2c01f-256">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="2c01f-257">En règle générale, vous utilisez le `Validation Message Tag Helper` après un Tag Helper `Input` pour la même propriété.</span><span class="sxs-lookup"><span data-stu-id="2c01f-257">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="2c01f-258">Dans ce cas, les messages d’erreur de validation s’affichent près de l’entrée qui a provoqué l’erreur.</span><span class="sxs-lookup"><span data-stu-id="2c01f-258">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="2c01f-259">Vous devez avoir une vue avec les références de script JavaScript et [jQuery](https://jquery.com/) appropriées pour la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="2c01f-259">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="2c01f-260">Pour plus d’informations, consultez [Validation de modèle](../models/validation.md).</span><span class="sxs-lookup"><span data-stu-id="2c01f-260">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="2c01f-261">Quand une erreur de validation côté serveur se produit (par exemple, quand vous disposez d’une validation personnalisée côté serveur ou quand la validation côté client est désactivée), MVC place ce message d’erreur dans le corps de l’élément `<span>`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-261">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="2c01f-262">Le Tag Helper Validation Summary</span><span class="sxs-lookup"><span data-stu-id="2c01f-262">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="2c01f-263">Cible les éléments `<div>` avec les attributs `asp-validation-summary`</span><span class="sxs-lookup"><span data-stu-id="2c01f-263">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="2c01f-264">Alternative HTML Helper : `@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="2c01f-264">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="2c01f-265">Le `Validation Summary Tag Helper` est utilisé pour afficher un récapitulatif des messages de validation.</span><span class="sxs-lookup"><span data-stu-id="2c01f-265">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="2c01f-266">La valeur de l’attribut `asp-validation-summary` peut correspondre à l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="2c01f-266">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="2c01f-267">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="2c01f-267">asp-validation-summary</span></span>|<span data-ttu-id="2c01f-268">Messages de validation affichés</span><span class="sxs-lookup"><span data-stu-id="2c01f-268">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="2c01f-269">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="2c01f-269">ValidationSummary.All</span></span>|<span data-ttu-id="2c01f-270">Niveau de la propriété et du modèle</span><span class="sxs-lookup"><span data-stu-id="2c01f-270">Property and model level</span></span>|
|<span data-ttu-id="2c01f-271">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="2c01f-271">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="2c01f-272">Modèle</span><span class="sxs-lookup"><span data-stu-id="2c01f-272">Model</span></span>|
|<span data-ttu-id="2c01f-273">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="2c01f-273">ValidationSummary.None</span></span>|<span data-ttu-id="2c01f-274">Aucun.</span><span class="sxs-lookup"><span data-stu-id="2c01f-274">None</span></span>|

### <a name="sample"></a><span data-ttu-id="2c01f-275">Exemple</span><span class="sxs-lookup"><span data-stu-id="2c01f-275">Sample</span></span>

<span data-ttu-id="2c01f-276">Dans l’exemple suivant, le modèle de données est décoré avec les attributs `DataAnnotation`, ce qui génère des messages d’erreur de validation pour l’élément `<input>`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-276">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="2c01f-277">Quand une erreur de validation se produit, le Tag Helper Validation affiche le message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="2c01f-277">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="2c01f-278">Code HTML généré (quand le modèle est valide) :</span><span class="sxs-lookup"><span data-stu-id="2c01f-278">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="2c01f-279">Tag Helper Select</span><span class="sxs-lookup"><span data-stu-id="2c01f-279">The Select Tag Helper</span></span>

* <span data-ttu-id="2c01f-280">Génère l’élément [select](https://www.w3.org/wiki/HTML/Elements/select) et les éléments [option](https://www.w3.org/wiki/HTML/Elements/option) associés pour les propriétés de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="2c01f-280">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="2c01f-281">Comporte une alternative HTML Helper avec `Html.DropDownListFor` et `Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="2c01f-281">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="2c01f-282">Le `Select Tag Helper` `asp-for` spécifie le nom de propriété de modèle de l’élément [select](https://www.w3.org/wiki/HTML/Elements/select), et `asp-items` spécifie les éléments [option](https://www.w3.org/wiki/HTML/Elements/option).</span><span class="sxs-lookup"><span data-stu-id="2c01f-282">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="2c01f-283">Exemple :</span><span class="sxs-lookup"><span data-stu-id="2c01f-283">For example:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="2c01f-284">Aperçu :</span><span class="sxs-lookup"><span data-stu-id="2c01f-284">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="2c01f-285">La méthode `Index` initialise `CountryViewModel`, définit le pays sélectionné et le passe à la vue `Index`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-285">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

<span data-ttu-id="2c01f-286">La méthode HTTP POST `Index` affiche la sélection :</span><span class="sxs-lookup"><span data-stu-id="2c01f-286">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="2c01f-287">Vue `Index` :</span><span class="sxs-lookup"><span data-stu-id="2c01f-287">The `Index` view:</span></span>

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="2c01f-288">Qui génère le code HTML suivant (avec « CA » sélectionné) :</span><span class="sxs-lookup"><span data-stu-id="2c01f-288">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="2c01f-289">Nous déconseillons d’utiliser `ViewBag` ou `ViewData` avec le Tag Helper Select.</span><span class="sxs-lookup"><span data-stu-id="2c01f-289">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="2c01f-290">Un modèle de vue est plus robuste et, en général, moins problématique pour fournir des métadonnées MVC.</span><span class="sxs-lookup"><span data-stu-id="2c01f-290">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="2c01f-291">La valeur de l’attribut `asp-for` est un cas particulier et ne nécessite pas de préfixe `Model`, contrairement aux autres attributs du Tag Helper (par exemple `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="2c01f-291">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="2c01f-292">Liaison d’enum</span><span class="sxs-lookup"><span data-stu-id="2c01f-292">Enum binding</span></span>

<span data-ttu-id="2c01f-293">Il est souvent pratique d’utiliser `<select>` avec une propriété `enum` et de générer les éléments `SelectListItem` à partir des valeurs de `enum`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-293">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="2c01f-294">Aperçu :</span><span class="sxs-lookup"><span data-stu-id="2c01f-294">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="2c01f-295">La méthode `GetEnumSelectList` génère un objet `SelectList` pour un enum.</span><span class="sxs-lookup"><span data-stu-id="2c01f-295">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="2c01f-296">Vous pouvez décorer votre liste d’énumérateurs avec l’attribut `Display` pour obtenir une IU plus riche :</span><span class="sxs-lookup"><span data-stu-id="2c01f-296">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="2c01f-297">Le code HTML suivant est généré :</span><span class="sxs-lookup"><span data-stu-id="2c01f-297">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="2c01f-298">Groupe d’options</span><span class="sxs-lookup"><span data-stu-id="2c01f-298">Option Group</span></span>

<span data-ttu-id="2c01f-299">L’élément HTML [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) est généré quand le modèle de vue contient un ou plusieurs objets `SelectListGroup`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-299">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="2c01f-300">`CountryViewModelGroup` regroupe les éléments `SelectListItem` dans les groupes « North America » et « Europe » :</span><span class="sxs-lookup"><span data-stu-id="2c01f-300">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="2c01f-301">Les deux groupes sont affichés ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="2c01f-301">The two groups are shown below:</span></span>

![exemple de groupe d’options](working-with-forms/_static/grp.png)

<span data-ttu-id="2c01f-303">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="2c01f-303">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="2c01f-304">Sélection multiple</span><span class="sxs-lookup"><span data-stu-id="2c01f-304">Multiple select</span></span>

<span data-ttu-id="2c01f-305">Le Tag Helper Select génère automatiquement l’attribut [multiple = "multiple"](http://w3c.github.io/html-reference/select.html) si la propriété spécifiée dans l’attribut `asp-for` est `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-305">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="2c01f-306">Par exemple, le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="2c01f-306">For example, given the following model:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="2c01f-307">Avec la vue suivante :</span><span class="sxs-lookup"><span data-stu-id="2c01f-307">With the following view:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="2c01f-308">Génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="2c01f-308">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="2c01f-309">Aucune sélection</span><span class="sxs-lookup"><span data-stu-id="2c01f-309">No selection</span></span>

<span data-ttu-id="2c01f-310">Si vous constatez que l’option « not specified » est utilisée dans plusieurs pages, vous pouvez créer un modèle pour éviter de répéter le code HTML :</span><span class="sxs-lookup"><span data-stu-id="2c01f-310">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="2c01f-311">Modèle *Views/Shared/EditorTemplates/CountryViewModel.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2c01f-311">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="2c01f-312">L’ajout d’éléments HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) ne se limite pas au cas *No selection*.</span><span class="sxs-lookup"><span data-stu-id="2c01f-312">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="2c01f-313">Par exemple, la vue et la méthode d’action suivante génèrent du code HTML similaire au code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="2c01f-313">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="2c01f-314">L’élément `<option>` approprié est sélectionné (il contient l’attribut `selected="selected"`) en fonction de la valeur actuelle de `Country`.</span><span class="sxs-lookup"><span data-stu-id="2c01f-314">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="2c01f-315">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2c01f-315">Additional resources</span></span>

* [<span data-ttu-id="2c01f-316">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="2c01f-316">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="2c01f-317">Élément HTML Form</span><span class="sxs-lookup"><span data-stu-id="2c01f-317">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="2c01f-318">Jeton de vérification de requête</span><span class="sxs-lookup"><span data-stu-id="2c01f-318">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* [<span data-ttu-id="2c01f-319">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="2c01f-319">Model Binding</span></span>](xref:mvc/models/model-binding)
* [<span data-ttu-id="2c01f-320">Validation de modèle</span><span class="sxs-lookup"><span data-stu-id="2c01f-320">Model Validation</span></span>](xref:mvc/models/validation)
* [<span data-ttu-id="2c01f-321">IAttributeAdapter, interface</span><span class="sxs-lookup"><span data-stu-id="2c01f-321">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="2c01f-322">Extraits de code pour ce document</span><span class="sxs-lookup"><span data-stu-id="2c01f-322">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
