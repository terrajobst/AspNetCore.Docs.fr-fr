---
title: "L’ancrage d’assistance de balise | Documents Microsoft"
author: pkellner
description: "Montre comment travailler avec l’application d’assistance de balise d’ancrage"
keywords: ASP.NET Core,tag helper
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 503ad7c4ce8c4f08b2a06dbe9f985566f54d3ca2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="0ebfe-104">Application d’assistance de balise d’ancrage</span><span class="sxs-lookup"><span data-stu-id="0ebfe-104">Anchor Tag Helper</span></span>

<span data-ttu-id="0ebfe-105">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="0ebfe-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="0ebfe-106">L’application d’assistance de balise d’ancrage améliore le point d’ancrage HTML (`<a ... ></a>`) balise en ajoutant de nouveaux attributs.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="0ebfe-107">Le lien généré (sur le `href` balise) est créé en utilisant les nouveaux attributs.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="0ebfe-108">Cette URL peut inclure un protocole facultatifs, tel que https.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="0ebfe-109">Le contrôleur de haut-parleur ci-dessous est utilisé dans les exemples dans ce document.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-109">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="0ebfe-110">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="0ebfe-110">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="0ebfe-111">Attributs d’assistance des balises d’ancrage</span><span class="sxs-lookup"><span data-stu-id="0ebfe-111">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="0ebfe-112">contrôleur d’ASP</span><span class="sxs-lookup"><span data-stu-id="0ebfe-112">asp-controller</span></span>

<span data-ttu-id="0ebfe-113">`asp-controller`est utilisé pour associer le contrôleur qui doit servir à générer l’URL.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="0ebfe-114">Les contrôleurs spécifiés doivent exister dans le projet actuel.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="0ebfe-115">Le code suivant répertorie tous les haut-parleurs :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="0ebfe-116">Le balisage généré sera :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="0ebfe-117">Si le `asp-controller` est spécifié et `asp-action` n’est pas le cas, la valeur par défaut `asp-action` correspondra à la méthode de contrôleur par défaut de la vue en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="0ebfe-118">Qu’est, dans l’exemple ci-dessus, si `asp-action` est omis, et ce programme d’assistance de balise d’ancrage est généré à partir de *HomeController*de `Index` vue (**/Home**), le balisage généré sera :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="0ebfe-119">action d’ASP</span><span class="sxs-lookup"><span data-stu-id="0ebfe-119">asp-action</span></span>

<span data-ttu-id="0ebfe-120">`asp-action`nom de la méthode d’action dans le contrôleur qui est inclus dans le texte généré `href`.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="0ebfe-121">Par exemple, le code suivant défini généré `href` pour pointer vers la page de détails du présentateur :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="0ebfe-122">Le balisage généré sera :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="0ebfe-123">Si aucun `asp-controller` attribut est spécifié, le contrôleur par défaut de l’appel de la vue de l’exécution de l’affichage actuel sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="0ebfe-124">Si l’attribut `asp-action` est `Index`, aucune action n’est ajoutée à l’URL menant à la valeur par défaut `Index` méthode appelée.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="0ebfe-125">L’action spécifiée (ou par défaut), doivent exister dans le contrôleur référencé dans `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="0ebfe-126">page ASP</span><span class="sxs-lookup"><span data-stu-id="0ebfe-126">asp-page</span></span>

<span data-ttu-id="0ebfe-127">Utilisez le `asp-page` attribut dans une balise d’ancrage pour définir son URL pointe vers une page spécifique.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-127">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="0ebfe-128">En ajoutant le préfixe du nom de la page avec une barre oblique « / » crée l’URL.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-128">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="0ebfe-129">L’URL dans l’exemple ci-dessous indique la page « Haut-parleur » dans le répertoire actif.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-129">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="0ebfe-130">Le `asp-page` attribut dans l’exemple de code précédent restitue la sortie HTML dans la vue qui est similaire à l’extrait de code suivant :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-130">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```
<span data-ttu-id="0ebfe-131">cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">haut-parleur de la vue</a></span><span class="sxs-lookup"><span data-stu-id="0ebfe-131">cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a></span></span>
```

The `asp-route-id` produces the following output:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```


### <a name="asp-route-value"></a><span data-ttu-id="0ebfe-132">ASP - route-{value}</span><span class="sxs-lookup"><span data-stu-id="0ebfe-132">asp-route-{value}</span></span>

<span data-ttu-id="0ebfe-133">`asp-route-`est un préfixe d’itinéraire avec des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-133">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="0ebfe-134">Toute valeur que vous placez une fois que le tiret fin sera interprété comme un paramètre d’itinéraire potentiels.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-134">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="0ebfe-135">Si un itinéraire par défaut n’est trouvé, ce préfixe d’itinéraire est adjointe à href généré comme une valeur et le paramètre de demande.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-135">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="0ebfe-136">Dans le cas contraire, elle sera remplacée dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-136">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="0ebfe-137">En supposant que vous avez une méthode de contrôleur défini comme suit :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-137">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="0ebfe-138">Et le modèle d’itinéraire par défaut défini dans votre *Startup.cs* comme suit :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-138">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="0ebfe-139">Le **cshtml** fichier qui contient l’application auxiliaire de balise d’ancrage nécessaire d’utiliser le **haut-parleur** paramètre de modèle passé à partir du contrôleur à la vue est la suivante :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-139">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="0ebfe-140">Le code HTML généré sera ensuite comme suit, car **id** a été trouvé dans l’itinéraire par défaut.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-140">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="0ebfe-141">Si le préfixe d’itinéraire n’est pas partie du modèle de routage trouvé, qui est le cas avec les éléments suivants **cshtml** fichier :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-141">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="0ebfe-142">Le code HTML généré sera ensuite comme suit, car **speakerid** est introuvable dans l’itinéraire mis en correspondance :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-142">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="0ebfe-143">Si le paramètre `asp-controller` ou `asp-action` ne sont pas spécifiés, le même traitement par défaut est adopté est dans le `asp-route` attribut.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-143">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="0ebfe-144">itinéraire d’ASP</span><span class="sxs-lookup"><span data-stu-id="0ebfe-144">asp-route</span></span>

<span data-ttu-id="0ebfe-145">`asp-route`fournit un moyen de créer une URL qui accède directement à un itinéraire nommé.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-145">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="0ebfe-146">À l’aide des attributs de routage, un itinéraire peut être nommé comme indiqué dans le `SpeakerController` et utilisé dans son `Evaluations` (méthode).</span><span class="sxs-lookup"><span data-stu-id="0ebfe-146">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="0ebfe-147">`Name = "speakerevals"`Indique à l’application d’assistance de balise d’ancrage pour générer un itinéraire directement à cette méthode de contrôleur à l’aide de l’URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-147">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="0ebfe-148">Si `asp-controller` ou `asp-action` est spécifié en plus de `asp-route`, l’itinéraire généré est peut-être pas ce que vous attendez.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-148">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="0ebfe-149">`asp-route`ne doit pas être utilisé avec un des attributs `asp-controller` ou `asp-action` afin d’éviter un conflit d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-149">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="0ebfe-150">ASP-all-données d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="0ebfe-150">asp-all-route-data</span></span>

<span data-ttu-id="0ebfe-151">`asp-all-route-data`permet de créer un dictionnaire de paires clé / valeur où la clé est le nom du paramètre et la valeur est la valeur associée à cette clé.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-151">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="0ebfe-152">Comme l’exemple ci-dessous illustre, un dictionnaire inline est créé et les données sont transmises à la vue razor.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-152">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="0ebfe-153">En guise d’alternative, les données peuvent également être passées avec votre modèle.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-153">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="0ebfe-154">Le code ci-dessus génère l’URL suivante : http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="0ebfe-154">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="0ebfe-155">Lorsque vous cliquez sur le lien, la méthode de contrôleur `EvaluationsCurrent` est appelée.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-155">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="0ebfe-156">Elle est appelée, car ce contrôleur possède deux paramètres de chaîne qui correspond à ce qui a été créé à partir de la `asp-all-route-data` dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-156">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="0ebfe-157">Si toutes les clés correspondent dans le dictionnaire de paramètres d’itinéraire, ces valeurs seront remplacées dans l’itinéraire, selon le cas et les autres valeurs de mise en correspondance ne seront générées en tant que paramètres de la demande.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-157">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="0ebfe-158">fragment d’ASP</span><span class="sxs-lookup"><span data-stu-id="0ebfe-158">asp-fragment</span></span>

<span data-ttu-id="0ebfe-159">`asp-fragment`définit un fragment d’URL à ajouter à l’URL.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-159">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="0ebfe-160">L’application d’assistance de balise d’ancrage ajoutera le caractère dièse (#).</span><span class="sxs-lookup"><span data-stu-id="0ebfe-160">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="0ebfe-161">Si vous créez une balise :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-161">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="0ebfe-162">L’URL générée sera : http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="0ebfe-162">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="0ebfe-163">Balises de hachage sont utiles lors de la création des applications côté client.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-163">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="0ebfe-164">Elles peuvent servir à faciliter le marquage et recherche dans JavaScript, par exemple.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-164">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="0ebfe-165">zone d’ASP</span><span class="sxs-lookup"><span data-stu-id="0ebfe-165">asp-area</span></span>

<span data-ttu-id="0ebfe-166">`asp-area`définit le nom de zone ASP.NET Core utilise pour définir l’itinéraire approprié.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-166">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="0ebfe-167">Voici des exemples de la façon dont l’attribut de zone provoque un remappage d’itinéraires.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-167">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="0ebfe-168">Paramètre `asp-area` blogs préfixes le répertoire `Areas/Blogs` aux itinéraires contrôleurs associées et les vues de cette balise d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-168">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="0ebfe-169">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="0ebfe-169">Project name</span></span>
  * <span data-ttu-id="0ebfe-170">wwwroot</span><span class="sxs-lookup"><span data-stu-id="0ebfe-170">wwwroot</span></span>
  * <span data-ttu-id="0ebfe-171">Zones</span><span class="sxs-lookup"><span data-stu-id="0ebfe-171">Areas</span></span>
    * <span data-ttu-id="0ebfe-172">Blogs</span><span class="sxs-lookup"><span data-stu-id="0ebfe-172">Blogs</span></span>
      * <span data-ttu-id="0ebfe-173">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="0ebfe-173">Controllers</span></span>
        * <span data-ttu-id="0ebfe-174">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="0ebfe-174">HomeController.cs</span></span>
      * <span data-ttu-id="0ebfe-175">Affichages</span><span class="sxs-lookup"><span data-stu-id="0ebfe-175">Views</span></span>
        * <span data-ttu-id="0ebfe-176">Accueil</span><span class="sxs-lookup"><span data-stu-id="0ebfe-176">Home</span></span>
          * <span data-ttu-id="0ebfe-177">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="0ebfe-177">Index.cshtml</span></span>
          * <span data-ttu-id="0ebfe-178">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="0ebfe-178">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="0ebfe-179">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="0ebfe-179">Controllers</span></span>

<span data-ttu-id="0ebfe-180">Spécification d’une balise de zone qui est valide, comme ```area="Blogs"``` lorsque vous référencez le ```AboutBlog.cshtml``` fichier ressemble à ce qui suit à l’aide de l’application d’assistance de balise d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-180">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="0ebfe-181">Le code HTML généré inclut le segment de zones et est comme suit :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-181">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="0ebfe-182">Pour les zones MVC travailler dans une application web, le modèle d’itinéraire doit inclure une référence à la zone si elle existe.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-182">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="0ebfe-183">Ce modèle, qui est le deuxième paramètre de la `routes.MapRoute` l’appel de méthode, apparaît sous la forme :`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="0ebfe-183">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="0ebfe-184">protocole d’ASP</span><span class="sxs-lookup"><span data-stu-id="0ebfe-184">asp-protocol</span></span>

<span data-ttu-id="0ebfe-185">Le `asp-protocol` de spécifier un protocole (tel que `https`) dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-185">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="0ebfe-186">Un exemple d’assistance à la balise d’ancrage qui inclut le protocole se présentera comme suit :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-186">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="0ebfe-187">et générera du code HTML comme suit :</span><span class="sxs-lookup"><span data-stu-id="0ebfe-187">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="0ebfe-188">Le domaine dans l’exemple est localhost, mais l’application d’assistance de balise d’ancrage utilise de domaine du site Web public lors de la génération de l’URL.</span><span class="sxs-lookup"><span data-stu-id="0ebfe-188">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ebfe-189">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0ebfe-189">Additional resources</span></span>

* [<span data-ttu-id="0ebfe-190">Zones</span><span class="sxs-lookup"><span data-stu-id="0ebfe-190">Areas</span></span>](xref:mvc/controllers/areas)
