---
title: Tag Helper Ancre dans ASP.NET Core
author: pkellner
description: Découvrez les attributs ASP.NET Core Tag Helper Ancre et le rôle joué par chaque attribut dans l’extension du comportement de la balise d’ancrage HTML.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 31ff62b6bedb5e577a51f341c89d241d06a83ad3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="b790d-103">Tag Helper Ancre dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b790d-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="b790d-104">Par [Peter Kellner](http://peterkellner.net) et [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="b790d-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="b790d-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b790d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b790d-106">Le [Tag Helper Ancre](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) améliore la balise d’ancrage HTML standard (`<a ... ></a>`) en ajoutant de nouveaux attributs.</span><span class="sxs-lookup"><span data-stu-id="b790d-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="b790d-107">Par convention, les noms d’attribut commencent par `asp-`.</span><span class="sxs-lookup"><span data-stu-id="b790d-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="b790d-108">La valeur d’attribut de l’élément d’ancrage rendu `href` est déterminée par les valeurs des attributs `asp-`.</span><span class="sxs-lookup"><span data-stu-id="b790d-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="b790d-109">*SpeakerController* est utilisé dans les exemples dans ce document :</span><span class="sxs-lookup"><span data-stu-id="b790d-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="b790d-110">Un inventaire des attributs `asp-` vient ensuite.</span><span class="sxs-lookup"><span data-stu-id="b790d-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="b790d-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="b790d-111">asp-controller</span></span>

<span data-ttu-id="b790d-112">L’attribut [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) assigne le contrôleur utilisé pour générer l’URL.</span><span class="sxs-lookup"><span data-stu-id="b790d-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="b790d-113">Le balisage suivant répertorie tous les présentateurs :</span><span class="sxs-lookup"><span data-stu-id="b790d-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="b790d-114">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="b790d-115">Si l’attribut `asp-controller` est spécifié et que `asp-action` ne l’est pas, la valeur par défaut `asp-action` est l’action du contrôleur associée à la vue en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="b790d-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="b790d-116">Si `asp-action` est omis du balisage précédent, et le Tag Helper Ancre est utilisé dans la vue *Index* de *HomeController* (*/Home*), le code HTML généré est :</span><span class="sxs-lookup"><span data-stu-id="b790d-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="b790d-117">asp-action</span><span class="sxs-lookup"><span data-stu-id="b790d-117">asp-action</span></span>

<span data-ttu-id="b790d-118">La valeur de l’attribut [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) représente le nom d’action du contrôleur inclus dans l’attribut `href` généré.</span><span class="sxs-lookup"><span data-stu-id="b790d-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="b790d-119">Le balisage suivant définit la valeur de l’attribut `href` généré sur la page d’évaluations du présentateur :</span><span class="sxs-lookup"><span data-stu-id="b790d-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="b790d-120">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="b790d-121">Si aucun attribut `asp-controller` n’est spécifié, le contrôleur par défaut qui appelle la vue exécutant la vue actuelle est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b790d-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="b790d-122">Si la valeur de l’attribut `asp-action` est `Index`, aucune action n’est ajoutée à l’URL, ce qui aboutit à l’appel de l’action `Index` par défaut.</span><span class="sxs-lookup"><span data-stu-id="b790d-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="b790d-123">L’action spécifiée (ou par défaut) doit exister dans le contrôleur référencé dans `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="b790d-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="b790d-124">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="b790d-124">asp-route-{value}</span></span>

<span data-ttu-id="b790d-125">L’attribut [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) active un préfixe d’itinéraire générique.</span><span class="sxs-lookup"><span data-stu-id="b790d-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="b790d-126">Toute valeur occupant l’espace réservé `{value}` est interprétée comme un paramètre d’itinéraire potentiel.</span><span class="sxs-lookup"><span data-stu-id="b790d-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="b790d-127">Si aucune route par défaut n’est trouvée, ce préfixe de routage est ajouté à l’attribut `href` généré en tant que valeur et paramètre de requête.</span><span class="sxs-lookup"><span data-stu-id="b790d-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="b790d-128">Dans le cas contraire, il est remplacé dans le modèle de routage.</span><span class="sxs-lookup"><span data-stu-id="b790d-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="b790d-129">Examinons l’action du contrôleur ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b790d-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="b790d-130">Avec un modèle d’itinéraire par défaut défini dans *Startup.Configure* :</span><span class="sxs-lookup"><span data-stu-id="b790d-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="b790d-131">La vue MVC utilise le modèle, fourni par l’action, comme suit :</span><span class="sxs-lookup"><span data-stu-id="b790d-131">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="b790d-132">L’espace réservé `{id?}` de l’itinéraire par défaut a été mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="b790d-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="b790d-133">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="b790d-134">Supposons que le préfixe d’itinéraire ne fait pas partie du modèle de routage correspondant, comme dans la vue MVC suivante :</span><span class="sxs-lookup"><span data-stu-id="b790d-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="b790d-135">Le code HTML suivant est généré, car `speakerid` n’a pas été trouvé dans l’itinéraire correspondant :</span><span class="sxs-lookup"><span data-stu-id="b790d-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="b790d-136">Si `asp-controller` ou `asp-action` ne sont pas spécifiés, le même traitement par défaut est appliqué que dans l’attribut `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="b790d-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="b790d-137">asp-route</span><span class="sxs-lookup"><span data-stu-id="b790d-137">asp-route</span></span>

<span data-ttu-id="b790d-138">L’attribut [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) est utilisé pour créer une URL reliant directement à un itinéraire nommé.</span><span class="sxs-lookup"><span data-stu-id="b790d-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="b790d-139">À l’aide des [attributs de routage](xref:mvc/controllers/routing#attribute-routing), un itinéraire peut être nommé comme indiqué dans `SpeakerController` et utilisé dans son action `Evaluations` :</span><span class="sxs-lookup"><span data-stu-id="b790d-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="b790d-140">Dans le balisage suivant, l’attribut `asp-route` fait référence à l’itinéraire nommé :</span><span class="sxs-lookup"><span data-stu-id="b790d-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="b790d-141">Le Tag Helper Ancre génère un itinéraire directement vers cette action de contrôleur à l’aide de l’URL */Speaker/Evaluations*.</span><span class="sxs-lookup"><span data-stu-id="b790d-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="b790d-142">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="b790d-143">Si `asp-controller` ou `asp-action` est spécifié en plus de `asp-route`, la route générée ne correspond peut-être pas à ce que vous attendez.</span><span class="sxs-lookup"><span data-stu-id="b790d-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="b790d-144">`asp-route` ne doit pas être utilisé avec les attributs `asp-controller` ou `asp-action` afin d’éviter un conflit de routage.</span><span class="sxs-lookup"><span data-stu-id="b790d-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="b790d-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="b790d-145">asp-all-route-data</span></span>

<span data-ttu-id="b790d-146">L’attribut [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) prend en charge la création d’un dictionnaire de paires clé-valeur.</span><span class="sxs-lookup"><span data-stu-id="b790d-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="b790d-147">La clé est le nom du paramètre et la valeur est la valeur du paramètre.</span><span class="sxs-lookup"><span data-stu-id="b790d-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="b790d-148">Dans l’exemple suivant, un dictionnaire est initialisé et transmis à une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="b790d-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="b790d-149">Les données peuvent également être transmises avec votre modèle.</span><span class="sxs-lookup"><span data-stu-id="b790d-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="b790d-150">Le code précédent génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="b790d-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="b790d-151">Le dictionnaire `asp-all-route-data` est aplati pour produire une chaîne de requête conforme aux exigences de l’action `Evaluations` surchargée :</span><span class="sxs-lookup"><span data-stu-id="b790d-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="b790d-152">Si des clés dans le dictionnaire correspondent aux paramètres d’itinéraire, ces valeurs sont substituées dans l’itinéraire comme il convient.</span><span class="sxs-lookup"><span data-stu-id="b790d-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="b790d-153">Les autres valeurs non correspondantes sont générées en tant que paramètres de la requête.</span><span class="sxs-lookup"><span data-stu-id="b790d-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="b790d-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="b790d-154">asp-fragment</span></span>

<span data-ttu-id="b790d-155">L’attribut [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) définit un fragment d’URL à ajouter à l’URL.</span><span class="sxs-lookup"><span data-stu-id="b790d-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="b790d-156">Le Tag Helper Ancre ajoute le caractère de hachage (#).</span><span class="sxs-lookup"><span data-stu-id="b790d-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="b790d-157">Examinons le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="b790d-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="b790d-158">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="b790d-159">Les balises de hachage sont utiles lors de la création des applications côté client.</span><span class="sxs-lookup"><span data-stu-id="b790d-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="b790d-160">Elles peuvent servir à faciliter le marquage et la recherche en JavaScript, par exemple.</span><span class="sxs-lookup"><span data-stu-id="b790d-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="b790d-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="b790d-161">asp-area</span></span>

<span data-ttu-id="b790d-162">L’attribut [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) définit le nom de la zone utilisé pour définir l’itinéraire approprié.</span><span class="sxs-lookup"><span data-stu-id="b790d-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="b790d-163">L’exemple suivant décrit la façon dont l’attribut de zone entraîne un remappage des itinéraires.</span><span class="sxs-lookup"><span data-stu-id="b790d-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="b790d-164">L’affectation de la valeur « Blogs » à `asp-area` préfixe le répertoire *Areas/Blogs* dans les itinéraires des vues et contrôleurs associés pour cette balise d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="b790d-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="b790d-165">**<Nom du projet\>**</span><span class="sxs-lookup"><span data-stu-id="b790d-165">**<Project name\>**</span></span>
  * <span data-ttu-id="b790d-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="b790d-166">**wwwroot**</span></span>
  * <span data-ttu-id="b790d-167">**Les zones (areas)**</span><span class="sxs-lookup"><span data-stu-id="b790d-167">**Areas**</span></span>
    * <span data-ttu-id="b790d-168">**Blogs**</span><span class="sxs-lookup"><span data-stu-id="b790d-168">**Blogs**</span></span>
      * <span data-ttu-id="b790d-169">**Contrôleurs**</span><span class="sxs-lookup"><span data-stu-id="b790d-169">**Controllers**</span></span>
        * <span data-ttu-id="b790d-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="b790d-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="b790d-171">**Vues**</span><span class="sxs-lookup"><span data-stu-id="b790d-171">**Views**</span></span>
        * <span data-ttu-id="b790d-172">**Accueil**</span><span class="sxs-lookup"><span data-stu-id="b790d-172">**Home**</span></span>
          * <span data-ttu-id="b790d-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b790d-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="b790d-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b790d-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="b790d-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b790d-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="b790d-176">**Contrôleurs**</span><span class="sxs-lookup"><span data-stu-id="b790d-176">**Controllers**</span></span>

<span data-ttu-id="b790d-177">Étant donné la hiérarchie de répertoire précédente, le balisage pour faire référence au fichier *AboutBlog.cshtml* est :</span><span class="sxs-lookup"><span data-stu-id="b790d-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="b790d-178">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="b790d-179">Pour que les zones fonctionnent dans une application MVC, le modèle de routage doit inclure une référence à la zone si elle existe.</span><span class="sxs-lookup"><span data-stu-id="b790d-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="b790d-180">Ce modèle est représenté par le deuxième paramètre de l’appel de méthode `routes.MapRoute` dans *Startup.Configure* : [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="b790d-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="b790d-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="b790d-181">asp-protocol</span></span>

<span data-ttu-id="b790d-182">L’attribut [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) permet de spécifier un protocole (tel que `https`) dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="b790d-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="b790d-183">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b790d-183">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="b790d-184">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-184">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="b790d-185">Le nom d’hôte dans l’exemple est localhost, mais le Tag Helper Ancre utilise le domaine public du site web lors de la génération de l’URL.</span><span class="sxs-lookup"><span data-stu-id="b790d-185">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="b790d-186">asp-host</span><span class="sxs-lookup"><span data-stu-id="b790d-186">asp-host</span></span>

<span data-ttu-id="b790d-187">L’attribut [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) est destiné à spécifier un nom d’hôte dans votre URL.</span><span class="sxs-lookup"><span data-stu-id="b790d-187">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="b790d-188">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b790d-188">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="b790d-189">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-189">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="b790d-190">asp-page</span><span class="sxs-lookup"><span data-stu-id="b790d-190">asp-page</span></span>

<span data-ttu-id="b790d-191">L’attribut [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) est utilisé avec les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="b790d-191">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="b790d-192">Utilisez-le pour définir la valeur d’attribut `href` d’une balise d’ancrage sur une page spécifique.</span><span class="sxs-lookup"><span data-stu-id="b790d-192">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="b790d-193">En ajoutant une barre oblique (« / ») comme préfixe au nom de la page, vous créez l’URL.</span><span class="sxs-lookup"><span data-stu-id="b790d-193">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="b790d-194">L’exemple suivant pointe vers la Page Razor du participant :</span><span class="sxs-lookup"><span data-stu-id="b790d-194">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="b790d-195">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-195">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="b790d-196">L’attribut `asp-page` et les attributs `asp-route`, `asp-controller` et `asp-action` s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="b790d-196">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="b790d-197">Toutefois, `asp-page` peut être utilisé avec `asp-route-{value}` pour contrôler le routage, comme illustré dans le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="b790d-197">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="b790d-198">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-198">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="b790d-199">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="b790d-199">asp-page-handler</span></span>

<span data-ttu-id="b790d-200">L’attribut [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) est utilisé avec les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="b790d-200">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="b790d-201">Il est destiné à la liaison à des gestionnaires de page spécifiques.</span><span class="sxs-lookup"><span data-stu-id="b790d-201">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="b790d-202">Prenons le gestionnaire de page suivant :</span><span class="sxs-lookup"><span data-stu-id="b790d-202">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="b790d-203">Le balisage associé au modèle de page est lié au gestionnaire de page `OnGetProfile`.</span><span class="sxs-lookup"><span data-stu-id="b790d-203">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="b790d-204">Notez que le préfixe `On<Verb>` du nom de la méthode du gestionnaire de page est omis dans la valeur d’attribut `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="b790d-204">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="b790d-205">S’il s’agissait d’une méthode asynchrone, le suffixe `Async` serait également omis.</span><span class="sxs-lookup"><span data-stu-id="b790d-205">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="b790d-206">Code HTML généré :</span><span class="sxs-lookup"><span data-stu-id="b790d-206">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="b790d-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b790d-207">Additional resources</span></span>

* [<span data-ttu-id="b790d-208">Les zones (areas)</span><span class="sxs-lookup"><span data-stu-id="b790d-208">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="b790d-209">Présentation des Pages Razor</span><span class="sxs-lookup"><span data-stu-id="b790d-209">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
