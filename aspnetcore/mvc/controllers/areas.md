---
title: Zones dans ASP.NET Core
author: rick-anderson
description: Découvrez les zones, fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: b78bb5146f1ab9039fa9ff015471654510718ed6
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312216"
---
# <a name="areas-in-aspnet-core"></a>Zones dans ASP.NET Core

Par [Dhananjay Kumar](https://twitter.com/debug_mode) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Les zones sont une fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues). L’utilisation de zones crée une hiérarchie qui permet le routage par ajout d’un autre paramètre de route, `area`, à `controller` et à `action`.

Les zones permettent de partitionner une application web ASP.NET Core MVC de grande taille en regroupements fonctionnels plus petits. Une zone est en réalité une structure MVC à l’intérieur d’une application. Dans un projet MVC, les composants logiques, comme Modèle, Contrôleur et Vue, sont conservés dans des dossiers différents, et MVC utilise des conventions de nommage pour créer la relation entre ces composants. Pour une application de grande taille, il peut être avantageux de partitionner l’application en différentes zones de fonctionnalités de haut niveau. Par exemple, considérons une application de e-commerce avec plusieurs entités métier, comme la commande, la facturation, la recherche, etc. Chacune de ces unités a ses propres vues, contrôleurs et modèles de composant logique. Dans ce scénario, vous pouvez utiliser des zones pour partitionner physiquement les composants métier dans le même projet.

Une zone peut être définie comme une unité fonctionnelle plus petite dans un projet ASP.NET Core MVC avec son propre ensemble de contrôleurs, de vues et de modèles.

Envisagez l’utilisation de zones dans un projet MVC quand :

* Votre application est constituée de plusieurs composants fonctionnels de haut niveau qui doivent être logiquement séparés

* Vous voulez partitionner votre projet MVC de façon à pouvoir travailler indépendamment sur chaque zone fonctionnelle

Caractéristiques des zones :

* Une application ASP.NET Core MVC peut contenir n’importe quel nombre de zones.

* Chaque zone a ses propres contrôleurs, modèles et vues.

* Les zones vous permettent d’organiser des projets MVC de grande taille en plusieurs composants généraux sur lesquels vous pouvez travailler indépendamment.

* Les zones prennent en charge plusieurs contrôleurs de même nom, à condition que ces contrôleurs soient dans des *zones* différentes.

Examinons un exemple pour illustrer la façon dont les zones sont créées et utilisées. Supposons que vous avez une application de magasin avec deux regroupements distincts de contrôleurs et de vues : Produits et Services. Voici une structure de dossiers classique pour ce type d’application avec des zones MVC :

* Nom du projet

  * Zones (Areas)

    * Produits

      * Contrôleurs

        * HomeController.cs

        * ManageController.cs

      * Affichages

        * Accueil

          * Index.cshtml

        * Gérer

          * Index.cshtml

    * Services

      * Contrôleurs

        * HomeController.cs

      * Affichages

        * Accueil

          * Index.cshtml

Quand MVC tente d’afficher une vue dans une zone, par défaut, il tente de chercher aux emplacements suivants :

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Il s’agit des emplacements par défaut, qui peuvent être changés via `AreaViewLocationFormats` sur `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Par exemple, dans le code ci-dessous, le nom de dossier « Areas » a été changé en « Categories ».

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Une chose à noter est que la structure du dossier *Vues* est la seule qui est considérée comme importante ici, et que le contenu du reste des dossiers, comme *Contrôleurs* et *Modèles* **n’a pas** d’importance. Par exemple, il n’est pas du tout nécessaire d’avoir des dossiers *Contrôleurs* et *Modèles*. Ceci fonctionne, car le contenu de *Contrôleurs* et de *Modèles* est simplement du code qui est compilé dans un fichier .dll, alors que le contenu de *Vues* ne l’est pas tant qu’une demande n’est pas adressée à cette vue.

Une fois que vous avez défini la hiérarchie des dossiers, vous devez indiquer à MVC que chaque contrôleur est associé à une zone. Vous faites cela en décorant le nom du contrôleur avec l’attribut `[Area]`.

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

Configurez une définition de route qui fonctionne avec vos zones nouvellement créées. L’article [Router vers les actions du contrôleur](routing.md) explique en détail comment créer des définitions de route, notamment l’utilisation de routes conventionnelles par rapport aux routes d’attributs. Dans cet exemple, nous allons utiliser une route conventionnelle. Pour cela, ouvrez le fichier *Startup.cs* et modifiez-le en ajoutant la définition de route nommée `areaRoute` ci-dessous.

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

Avec un accès à `http://<yourApp>/products`, la méthode d’action `Index` de `HomeController` dans la zone `Products` sera appelée.

## <a name="link-generation"></a>Génération de liens

* Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action au sein du même contrôleur

  Supposons que le chemin de la requête actuelle soit `/Products/Home/Create`

  Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Product's Home Page", "Index")`

  Syntaxe de TagHelper : `<a asp-action="Index">Go to Product's Home Page</a>`

  Notez que nous ne devons pas fournir ici les valeurs « area » et « controller », car elles sont déjà disponibles dans le contexte de la requête actuelle. Ces types de valeurs sont appelées valeurs `ambient`.

* Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action sur un autre contrôleur

  Supposons que le chemin de la requête actuelle soit `/Products/Home/Create`

  Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  Syntaxe de TagHelper : `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Notez qu’ici la valeur ambiante d’une « area » (zone) est utilisée, mais que la valeur « controller » est spécifiée explicitement ci-dessus.

* Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action sur un autre contrôleur et une autre zone

  Supposons que le chemin de la requête actuelle soit `/Products/Home/Create`

  Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  Syntaxe de TagHelper : `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Notez qu’ici aucune valeur ambiante n’est utilisée.

* Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action sur un autre contrôleur mais **pas** dans une zone

  Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  Syntaxe de TagHelper : `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Comme nous voulons générer des liens vers une action de contrôleur qui n’est pas basé sur une zone, nous vidons ici la valeur ambiante pour « area » (zone).

## <a name="publishing-areas"></a>Zones de publication

Tous les fichiers `*.cshtml` et `wwwroot/**` sont publiés en sortie quand `<Project Sdk="Microsoft.NET.Sdk.Web">` est inclus dans le fichier *.csproj*.
