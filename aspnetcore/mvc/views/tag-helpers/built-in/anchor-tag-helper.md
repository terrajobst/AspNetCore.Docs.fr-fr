---
title: Tag Helper Ancre dans ASP.NET Core
author: pkellner
description: Montre comment utiliser un Tag Helper Ancre
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>Tag Helper Ancre

Par [Peter Kellner](http://peterkellner.net) 

Le Tag Helper Ancre améliore la balise d’ancrage HTML (`<a ... ></a>`) en ajoutant de nouveaux attributs. Le lien généré (sur la balise `href`) est créé en utilisant les nouveaux attributs. Cette URL peut inclure un protocole facultatif, tel que https.

Le contrôleur de présentateur ci-dessous est utilisé dans les exemples de ce document.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Attributs de Tag Helper Ancre

### <a name="asp-controller"></a>asp-controller

`asp-controller` est utilisé pour associer le contrôleur qui doit servir à générer l’URL. Les contrôleurs spécifiés doivent exister dans le projet actuel. Le code suivant répertorie tous les présentateurs : 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Le balisage généré sera :

```html
<a href="/Speaker">All Speakers</a>
```

Si l’attribut `asp-controller` est spécifié et que l’attribut `asp-action` ne l’est pas, l’attribut par défaut `asp-action` correspondra à la méthode de contrôleur par défaut de la vue en cours d’exécution. Autrement dit, dans l’exemple ci-dessus, si `asp-action` est omis et que ce Tag Helper Ancre est généré à partir de la vue `Index` de *HomeController* (**/Home**), le balisage généré sera :

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` est le nom de la méthode d’action dans le contrôleur qui figure dans l’attribut `href` généré. Par exemple, le code suivant définit l’attribut `href` généré pour pointer vers la page détaillée du présentateur :

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Le balisage généré sera :

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Si aucun attribut `asp-controller` n’est spécifié, le contrôleur par défaut qui appelle la vue exécutant la vue actuelle est utilisé.  
 
Si l’attribut `asp-action` a la valeur `Index`, aucune action n’est ajoutée à l’URL, ce qui aboutit à l’appel de la méthode `Index` par défaut. L’action spécifiée (ou par défaut) doit exister dans le contrôleur référencé dans `asp-controller`.

### <a name="asp-page"></a>asp-page

Utilisez l’attribut `asp-page` dans une balise d’ancrage pour définir son URL afin qu’elle pointe vers une page spécifique. En ajoutant une barre oblique « / » comme préfixe au nom de la page, vous créez l’URL. L’URL dans l’exemple ci-dessous pointe vers la page « Speaker » dans le répertoire actif.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

L’attribut `asp-page` dans l’exemple de code précédent restitue la sortie HTML dans la vue qui est similaire à l’extrait de code suivant :

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

L’attribut `asp-page` et les attributs `asp-route`, `asp-controller` et `asp-action` s’excluent mutuellement. Toutefois, `asp-page` peut être utilisé avec `asp-route-id` pour contrôler le routage, comme illustré dans l’exemple de code suivant :

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` génère la sortie suivante :

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Pour utiliser l’attribut `asp-page` dans les pages Razor, les URL doivent être un chemin relatif, par exemple `"./Speaker"`. Les chemins relatifs dans l’attribut `asp-page` ne sont pas disponibles dans les vues MVC. Utilisez plutôt la syntaxe « / » pour les vues MVC.

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-` est un préfixe de routage de caractère générique. Toute valeur que vous placez après le tiret à la fin sera interprétée comme un paramètre de routage potentiel. Si aucune route par défaut n’est trouvée, ce préfixe de routage est ajouté à l’attribut href généré en tant que valeur et paramètre de requête. Dans le cas contraire, il est remplacé dans le modèle de routage.

En supposant que vous avez une méthode de contrôleur définie comme suit :

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

Et que le modèle de routage par défaut est défini dans *Startup.cs* comme suit :

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

Le fichier **cshtml** qui contient le Tag Helper Ancre nécessaire pour utiliser le paramètre de modèle **speaker** passé à partir du contrôleur à la vue se présente comme suit :

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Le code HTML généré se présentera ensuite comme suit, car **id** a été trouvé dans la route par défaut.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Si le préfixe de routage ne fait pas partie du modèle de routage trouvé, ce qui est le cas avec le fichier **cshtml** suivant :

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Le code HTML généré se présente ensuite comme suit, car **speakerid** n’a pas été trouvé dans la route associée :

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Si `asp-controller` ou `asp-action` ne sont pas spécifiés, le même traitement par défaut est appliqué que dans l’attribut `asp-route`.

### <a name="asp-route"></a>asp-route

`asp-route` fournit un moyen de créer une URL qui accède directement à une route nommée. À l’aide des attributs de routage, une route peut être nommée comme indiqué dans `SpeakerController` et utilisée dans sa méthode `Evaluations`.

`Name = "speakerevals"` indique au Tag Helper Ancre de générer une route directement vers cette méthode de contrôleur à l’aide de l’URL `/Speaker/Evaluations`. Si `asp-controller` ou `asp-action` est spécifié en plus de `asp-route`, la route générée ne correspond peut-être pas à ce que vous attendez. `asp-route` ne doit pas être utilisé avec les attributs `asp-controller` ou `asp-action` afin d’éviter un conflit de routage.

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data` permet de créer un dictionnaire de paires clé-valeur où la clé est le nom du paramètre et la valeur est la valeur associée à cette clé.

Comme illustré dans l’exemple ci-dessous, un dictionnaire inline est créé et les données sont transmises à la vue Razor. Les données peuvent également être passées avec votre modèle.

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

Le code ci-dessus génère l’URL suivante : http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Quand vous cliquez sur le lien, la méthode de contrôleur `EvaluationsCurrent` est appelée. Elle est appelée, car ce contrôleur possède deux paramètres de chaîne correspondant à ce qui a été créé à partir du dictionnaire `asp-all-route-data`.

Si des clés dans le dictionnaire correspondent à des paramètres de routage, ces valeurs sont remplacées dans la route selon le cas, tandis que les autres valeurs sans correspondance sont générées en tant que paramètres de requête.

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` définit un fragment d’URL à ajouter à l’URL. Le Tag Helper Ancre ajoutera le caractère de hachage (#). Si vous créez une balise :

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

L’URL générée sera : http://localhost/Speaker/Evaluations#SpeakerEvaluations

Les balises de hachage sont utiles lors de la création des applications côté client. Elles peuvent servir à faciliter le marquage et la recherche en JavaScript, par exemple.

### <a name="asp-area"></a>asp-area

`asp-area` indique le nom de zone qu’ASP.NET Core utilise pour définir la route appropriée. Voici des exemples de la façon dont l’attribut de zone entraîne un remappage de routes. L’affectation de la valeur Blogs à `asp-area` préfixe le répertoire `Areas/Blogs` dans les routes des vues et contrôleurs associés pour cette balise d’ancrage.

* Nom du projet
  * wwwroot
  * Zones
    * Blogs
      * Contrôleurs
        * HomeController.cs
      * Affichages
        * Accueil
          * Index.cshtml
          * AboutBlog.cshtml
  * Contrôleurs

La spécification d’une balise de zone valide, comme ```area="Blogs"``` quand vous référencez le fichier ```AboutBlog.cshtml```, ressemble à ce qui suit avec le Tag Helper Ancre.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Le code HTML généré inclut le segment de zones et se présente comme suit :

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Pour que les zones MVC fonctionnent dans une application web, le modèle de routage doit inclure une référence à la zone si elle existe. Ce modèle, qui est le deuxième paramètre de l’appel de méthode `routes.MapRoute`, apparaît sous la forme : `template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

L’attribut `asp-protocol` permet de spécifier un protocole (tel que `https`) dans l’URL. Un exemple de Tag Helper Ancre qui inclut le protocole se présentera ainsi :

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

et générera du code HTML comme suit :

```<a href="https://localhost/Home/About">About</a>```

Le domaine dans l’exemple est localhost, mais le Tag Helper Ancre utilise le domaine public du site web lors de la génération de l’URL.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Zones](xref:mvc/controllers/areas)
