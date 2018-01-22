---
title: "L’ancrage d’assistance de balise | Documents Microsoft"
author: pkellner
description: "Montre comment travailler avec l’application d’assistance de balise d’ancrage"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 7923876c792544ac4d559eb8de29475d8a4b37e0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="5b05d-103">Application d’assistance de balise d’ancrage</span><span class="sxs-lookup"><span data-stu-id="5b05d-103">Anchor Tag Helper</span></span>

<span data-ttu-id="5b05d-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="5b05d-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="5b05d-105">L’application d’assistance de balise d’ancrage améliore le point d’ancrage HTML (`<a ... ></a>`) balise en ajoutant de nouveaux attributs.</span><span class="sxs-lookup"><span data-stu-id="5b05d-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="5b05d-106">Le lien généré (sur le `href` balise) est créé en utilisant les nouveaux attributs.</span><span class="sxs-lookup"><span data-stu-id="5b05d-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="5b05d-107">Cette URL peut inclure un protocole facultatifs, tel que https.</span><span class="sxs-lookup"><span data-stu-id="5b05d-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="5b05d-108">Le contrôleur de haut-parleur ci-dessous est utilisé dans les exemples dans ce document.</span><span class="sxs-lookup"><span data-stu-id="5b05d-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="5b05d-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="5b05d-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="5b05d-110">Attributs d’assistance des balises d’ancrage</span><span class="sxs-lookup"><span data-stu-id="5b05d-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="5b05d-111">contrôleur d’ASP</span><span class="sxs-lookup"><span data-stu-id="5b05d-111">asp-controller</span></span>

<span data-ttu-id="5b05d-112">`asp-controller`est utilisé pour associer le contrôleur qui doit servir à générer l’URL.</span><span class="sxs-lookup"><span data-stu-id="5b05d-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="5b05d-113">Les contrôleurs spécifiés doivent exister dans le projet actuel.</span><span class="sxs-lookup"><span data-stu-id="5b05d-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="5b05d-114">Le code suivant répertorie tous les haut-parleurs :</span><span class="sxs-lookup"><span data-stu-id="5b05d-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="5b05d-115">Le balisage généré sera :</span><span class="sxs-lookup"><span data-stu-id="5b05d-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="5b05d-116">Si le `asp-controller` est spécifié et `asp-action` n’est pas le cas, la valeur par défaut `asp-action` correspondra à la méthode de contrôleur par défaut de la vue en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="5b05d-116">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="5b05d-117">Qu’est, dans l’exemple ci-dessus, si `asp-action` est omis, et ce programme d’assistance de balise d’ancrage est généré à partir de *HomeController*de `Index` vue (**/Home**), le balisage généré sera :</span><span class="sxs-lookup"><span data-stu-id="5b05d-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="5b05d-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="5b05d-118">asp-action</span></span>

<span data-ttu-id="5b05d-119">`asp-action`nom de la méthode d’action dans le contrôleur qui est inclus dans le texte généré `href`.</span><span class="sxs-lookup"><span data-stu-id="5b05d-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="5b05d-120">Par exemple, le code suivant défini généré `href` pour pointer vers la page de détails du présentateur :</span><span class="sxs-lookup"><span data-stu-id="5b05d-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="5b05d-121">Le balisage généré sera :</span><span class="sxs-lookup"><span data-stu-id="5b05d-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="5b05d-122">Si aucun `asp-controller` attribut est spécifié, le contrôleur par défaut de l’appel de la vue de l’exécution de l’affichage actuel sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="5b05d-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="5b05d-123">Si l’attribut `asp-action` est `Index`, aucune action n’est ajoutée à l’URL menant à la valeur par défaut `Index` méthode appelée.</span><span class="sxs-lookup"><span data-stu-id="5b05d-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="5b05d-124">L’action spécifiée (ou par défaut), doivent exister dans le contrôleur référencé dans `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="5b05d-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="5b05d-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="5b05d-125">asp-page</span></span>

<span data-ttu-id="5b05d-126">Utilisez le `asp-page` attribut dans une balise d’ancrage pour définir son URL pointe vers une page spécifique.</span><span class="sxs-lookup"><span data-stu-id="5b05d-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="5b05d-127">En ajoutant le préfixe du nom de la page avec une barre oblique « / » crée l’URL.</span><span class="sxs-lookup"><span data-stu-id="5b05d-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="5b05d-128">L’URL dans l’exemple ci-dessous indique la page « Haut-parleur » dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="5b05d-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="5b05d-129">Le `asp-page` attribut dans l’exemple de code précédent restitue la sortie HTML dans la vue qui est similaire à l’extrait de code suivant :</span><span class="sxs-lookup"><span data-stu-id="5b05d-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="5b05d-130">Le `asp-page` attributs s’excluent mutuellement avec le `asp-route`, `asp-controller`, et `asp-action` attributs.</span><span class="sxs-lookup"><span data-stu-id="5b05d-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="5b05d-131">Toutefois, `asp-page` peut être utilisé avec `asp-route-id` pour contrôler le routage, comme l’exemple de code suivant montre comment :</span><span class="sxs-lookup"><span data-stu-id="5b05d-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="5b05d-132">Le `asp-route-id` génère la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="5b05d-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="5b05d-133">Pour utiliser le `asp-page` attribut dans les Pages Razor, l’URL doit être un chemin d’accès relatif, par exemple `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="5b05d-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="5b05d-134">Chemins d’accès relatifs dans le `asp-page` attribut ne sont pas disponibles dans les vues MVC.</span><span class="sxs-lookup"><span data-stu-id="5b05d-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="5b05d-135">Utilisez la syntaxe « / » pour les vues MVC à la place.</span><span class="sxs-lookup"><span data-stu-id="5b05d-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="5b05d-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="5b05d-136">asp-route-{value}</span></span>

<span data-ttu-id="5b05d-137">`asp-route-`est un préfixe d’itinéraire avec des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="5b05d-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="5b05d-138">Toute valeur que vous placez une fois que le tiret fin sera interprété comme un paramètre d’itinéraire potentiels.</span><span class="sxs-lookup"><span data-stu-id="5b05d-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="5b05d-139">Si un itinéraire par défaut n’est trouvé, ce préfixe d’itinéraire est adjointe à href généré comme une valeur et le paramètre de demande.</span><span class="sxs-lookup"><span data-stu-id="5b05d-139">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="5b05d-140">Dans le cas contraire, elle sera remplacée dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="5b05d-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="5b05d-141">En supposant que vous avez une méthode de contrôleur défini comme suit :</span><span class="sxs-lookup"><span data-stu-id="5b05d-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="5b05d-142">Et le modèle d’itinéraire par défaut défini dans votre *Startup.cs* comme suit :</span><span class="sxs-lookup"><span data-stu-id="5b05d-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="5b05d-143">Le **cshtml** fichier qui contient l’application auxiliaire de balise d’ancrage nécessaire d’utiliser le **haut-parleur** paramètre de modèle passé à partir du contrôleur à la vue est la suivante :</span><span class="sxs-lookup"><span data-stu-id="5b05d-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="5b05d-144">Le code HTML généré sera ensuite comme suit, car **id** a été trouvé dans l’itinéraire par défaut.</span><span class="sxs-lookup"><span data-stu-id="5b05d-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="5b05d-145">Si le préfixe d’itinéraire n’est pas partie du modèle de routage trouvé, qui est le cas avec les éléments suivants **cshtml** fichier :</span><span class="sxs-lookup"><span data-stu-id="5b05d-145">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="5b05d-146">Le code HTML généré sera ensuite comme suit, car **speakerid** est introuvable dans l’itinéraire mis en correspondance :</span><span class="sxs-lookup"><span data-stu-id="5b05d-146">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="5b05d-147">Si le paramètre `asp-controller` ou `asp-action` ne sont pas spécifiés, le même traitement par défaut est adopté est dans le `asp-route` attribut.</span><span class="sxs-lookup"><span data-stu-id="5b05d-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="5b05d-148">itinéraire d’ASP</span><span class="sxs-lookup"><span data-stu-id="5b05d-148">asp-route</span></span>

<span data-ttu-id="5b05d-149">`asp-route`fournit un moyen de créer une URL qui accède directement à un itinéraire nommé.</span><span class="sxs-lookup"><span data-stu-id="5b05d-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="5b05d-150">À l’aide des attributs de routage, un itinéraire peut être nommé comme indiqué dans le `SpeakerController` et utilisé dans son `Evaluations` (méthode).</span><span class="sxs-lookup"><span data-stu-id="5b05d-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="5b05d-151">`Name = "speakerevals"`Indique à l’application d’assistance de balise d’ancrage pour générer un itinéraire directement à cette méthode de contrôleur à l’aide de l’URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="5b05d-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="5b05d-152">Si `asp-controller` ou `asp-action` est spécifié en plus de `asp-route`, l’itinéraire généré est peut-être pas ce que vous attendez.</span><span class="sxs-lookup"><span data-stu-id="5b05d-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="5b05d-153">`asp-route`ne doit pas être utilisé avec un des attributs `asp-controller` ou `asp-action` afin d’éviter un conflit d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="5b05d-153">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="5b05d-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="5b05d-154">asp-all-route-data</span></span>

<span data-ttu-id="5b05d-155">`asp-all-route-data`permet de créer un dictionnaire de paires clé / valeur où la clé est le nom du paramètre et la valeur est la valeur associée à cette clé.</span><span class="sxs-lookup"><span data-stu-id="5b05d-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="5b05d-156">Comme l’exemple ci-dessous illustre, un dictionnaire inline est créé et les données sont transmises à la vue razor.</span><span class="sxs-lookup"><span data-stu-id="5b05d-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="5b05d-157">En guise d’alternative, les données peuvent également être passées avec votre modèle.</span><span class="sxs-lookup"><span data-stu-id="5b05d-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="5b05d-158">Le code ci-dessus génère l’URL suivante : http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="5b05d-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="5b05d-159">Lorsque vous cliquez sur le lien, la méthode de contrôleur `EvaluationsCurrent` est appelée.</span><span class="sxs-lookup"><span data-stu-id="5b05d-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="5b05d-160">Elle est appelée, car ce contrôleur possède deux paramètres de chaîne qui correspond à ce qui a été créé à partir de la `asp-all-route-data` dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="5b05d-160">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="5b05d-161">Si toutes les clés correspondent dans le dictionnaire de paramètres d’itinéraire, ces valeurs seront remplacées dans l’itinéraire, selon le cas et les autres valeurs de mise en correspondance ne seront générées en tant que paramètres de la demande.</span><span class="sxs-lookup"><span data-stu-id="5b05d-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="5b05d-162">fragment d’ASP</span><span class="sxs-lookup"><span data-stu-id="5b05d-162">asp-fragment</span></span>

<span data-ttu-id="5b05d-163">`asp-fragment`définit un fragment d’URL à ajouter à l’URL.</span><span class="sxs-lookup"><span data-stu-id="5b05d-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="5b05d-164">L’application d’assistance de balise d’ancrage ajoutera le caractère dièse (#).</span><span class="sxs-lookup"><span data-stu-id="5b05d-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="5b05d-165">Si vous créez une balise :</span><span class="sxs-lookup"><span data-stu-id="5b05d-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="5b05d-166">L’URL générée sera : http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="5b05d-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="5b05d-167">Balises de hachage sont utiles lors de la création des applications côté client.</span><span class="sxs-lookup"><span data-stu-id="5b05d-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="5b05d-168">Elles peuvent servir à faciliter le marquage et recherche dans JavaScript, par exemple.</span><span class="sxs-lookup"><span data-stu-id="5b05d-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="5b05d-169">zone d’ASP</span><span class="sxs-lookup"><span data-stu-id="5b05d-169">asp-area</span></span>

<span data-ttu-id="5b05d-170">`asp-area`définit le nom de zone ASP.NET Core utilise pour définir l’itinéraire approprié.</span><span class="sxs-lookup"><span data-stu-id="5b05d-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="5b05d-171">Voici des exemples de la façon dont l’attribut de zone provoque un remappage d’itinéraires.</span><span class="sxs-lookup"><span data-stu-id="5b05d-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="5b05d-172">Paramètre `asp-area` blogs préfixes le répertoire `Areas/Blogs` aux itinéraires contrôleurs associées et les vues de cette balise d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="5b05d-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="5b05d-173">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="5b05d-173">Project name</span></span>
  * <span data-ttu-id="5b05d-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="5b05d-174">wwwroot</span></span>
  * <span data-ttu-id="5b05d-175">Zones</span><span class="sxs-lookup"><span data-stu-id="5b05d-175">Areas</span></span>
    * <span data-ttu-id="5b05d-176">Blogs</span><span class="sxs-lookup"><span data-stu-id="5b05d-176">Blogs</span></span>
      * <span data-ttu-id="5b05d-177">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="5b05d-177">Controllers</span></span>
        * <span data-ttu-id="5b05d-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="5b05d-178">HomeController.cs</span></span>
      * <span data-ttu-id="5b05d-179">Affichages</span><span class="sxs-lookup"><span data-stu-id="5b05d-179">Views</span></span>
        * <span data-ttu-id="5b05d-180">Accueil</span><span class="sxs-lookup"><span data-stu-id="5b05d-180">Home</span></span>
          * <span data-ttu-id="5b05d-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="5b05d-181">Index.cshtml</span></span>
          * <span data-ttu-id="5b05d-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="5b05d-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="5b05d-183">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="5b05d-183">Controllers</span></span>

<span data-ttu-id="5b05d-184">Spécification d’une balise de zone qui est valide, comme ```area="Blogs"``` lorsque vous référencez le ```AboutBlog.cshtml``` fichier ressemble à ce qui suit à l’aide de l’application d’assistance de balise d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="5b05d-184">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="5b05d-185">Le code HTML généré inclut le segment de zones et est comme suit :</span><span class="sxs-lookup"><span data-stu-id="5b05d-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="5b05d-186">Pour les zones MVC travailler dans une application web, le modèle d’itinéraire doit inclure une référence à la zone si elle existe.</span><span class="sxs-lookup"><span data-stu-id="5b05d-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="5b05d-187">Ce modèle, qui est le deuxième paramètre de la `routes.MapRoute` l’appel de méthode, apparaît sous la forme :`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="5b05d-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="5b05d-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="5b05d-188">asp-protocol</span></span>

<span data-ttu-id="5b05d-189">Le `asp-protocol` de spécifier un protocole (tel que `https`) dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="5b05d-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="5b05d-190">Un exemple d’assistance à la balise d’ancrage qui inclut le protocole se présentera comme suit :</span><span class="sxs-lookup"><span data-stu-id="5b05d-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="5b05d-191">et générera du code HTML comme suit :</span><span class="sxs-lookup"><span data-stu-id="5b05d-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="5b05d-192">Le domaine dans l’exemple est localhost, mais l’application d’assistance de balise d’ancrage utilise de domaine du site Web public lors de la génération de l’URL.</span><span class="sxs-lookup"><span data-stu-id="5b05d-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b05d-193">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5b05d-193">Additional resources</span></span>

* [<span data-ttu-id="5b05d-194">Zones</span><span class="sxs-lookup"><span data-stu-id="5b05d-194">Areas</span></span>](xref:mvc/controllers/areas)
