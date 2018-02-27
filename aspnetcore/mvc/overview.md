---
title: "Vue d’ensemble du modèle MVC d’ASP.NET Core"
author: ardalis
description: "Découvrez ASP.NET Core MVC, un puissant framework qui vous permet de générer des applications web et des API à l’aide du modèle de conception Model-View-Controller."
manager: wpickett
ms.author: riande
ms.date: 01/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/overview
ms.openlocfilehash: 16fd1b5e71cde4364f02640f504d42218ed680df
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="overview-of-aspnet-core-mvc"></a>Vue d’ensemble du modèle MVC d’ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC est un puissant framework qui vous permet de générer des applications web et des API à l’aide du modèle de conception Model-View-Controller.

## <a name="what-is-the-mvc-pattern"></a>Qu’est-ce que le modèle MVC ?

Le modèle architectural MVC (Model-View-Controller) sépare une application en trois groupes principaux de composants : les modèles, les vues et les contrôleurs. Ce modèle permet d’obtenir une [séparation des responsabilités](http://deviq.com/separation-of-concerns/). Avec ce modèle, les requêtes des utilisateurs sont acheminées vers un contrôleur chargé d’interagir avec le modèle pour effectuer des actions utilisateur et/ou récupérer les résultats des requêtes. Le contrôleur choisit la vue à afficher à l’utilisateur et lui fournit les données de modèle dont il a besoin.

Le diagramme suivant montre les trois composants principaux et leurs relations entre eux :

![Modèle MVC](overview/_static/mvc.png)

Cette délimitation des responsabilités vous aide à mettre à l’échelle la complexité de l’application, car il est plus facile de coder, déboguer et tester une chose (modèle, vue ou contrôleur) qui a un seul travail (et suit le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/)). Il est plus difficile de mettre à jour, tester et déboguer du code dont les dépendances sont réparties sur deux de ces domaines ou plus. Par exemple, la logique de l’interface utilisateur a tendance à changer plus fréquemment que la logique métier. Si le code de présentation et la logique métier sont combinés en un seul objet, l’objet contenant la logique métier doit être modifié chaque fois que l’interface utilisateur change. Cela introduit souvent des erreurs et nécessite de retester la logique métier après chaque changement minimal de l’interface utilisateur.

> [!NOTE]
> La vue et le contrôleur dépendent tous deux du modèle. Toutefois, le modèle ne dépend ni de la vue ni du contrôleur. Il s’agit de l’un des principaux avantages de la séparation. Cette séparation permet de générer et de tester le modèle indépendamment de la présentation visuelle.

### <a name="model-responsibilities"></a>Responsabilités du modèle

Le modèle d’une application MVC représente l’état de l’application, ainsi que la logique métier ou les opérations à effectuer. La logique métier doit être encapsulée dans le modèle, ainsi que toute autre logique d’implémentation pour la persistance de l’état de l’application. En général, les vues fortement typées utilisent des types ViewModel conçus pour contenir les données à afficher sur cette vue. Le contrôleur crée et remplit ces instances de ViewModel à partir du modèle.

> [!NOTE]
> Il existe de nombreuses façons d’organiser le modèle dans une application qui utilise le modèle architectural MVC. Pour en savoir plus, consultez les informations sur les [différents genres de types de modèle](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Responsabilités de la vue

Les vues sont responsables de la présentation du contenu via l’interface utilisateur. Elles utilisent le [moteur de vue Razor](#razor-view-engine) pour incorporer du code .NET dans les balises HTML. Il doit exister une logique minimale dans les vues, et cette logique doit être liée à la présentation du contenu. Si vous avez besoin d’exécuter une grande partie de la logique dans les fichiers de vue pour afficher les données d’un modèle complexe, utilisez un [composant de vue](views/view-components.md), ViewModel ou un modèle de vue pour simplifier la vue.

### <a name="controller-responsibilities"></a>Responsabilités du contrôleur

Les contrôleurs sont des composants qui gèrent l’interaction avec l’utilisateur, fonctionnent avec le modèle et, au final, sélectionnent une vue à afficher. Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond. Dans le modèle MVC, le contrôleur est le point d’entrée initial. Il est responsable de la sélection des types de modèle à utiliser et de la vue à afficher (ce qui explique son nom, car il contrôle la manière dont l’application répond à une requête donnée).

> [!NOTE]
> Vous devez éviter les excès de responsabilités pour ne pas rendre les contrôleurs trop complexes. Pour éviter que la logique du contrôleur ne devienne trop complexe, utilisez le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/) afin d’envoyer (push) la logique métier hors du contrôleur vers le modèle de domaine.

>[!TIP]
> Si vous constatez que le contrôleur effectue souvent les mêmes genres d’action, vous pouvez suivre le [principe DRY (Ne vous répétez pas)](http://deviq.com/don-t-repeat-yourself/) en plaçant ces actions usuelles dans des [filtres](#filters).

## <a name="what-is-aspnet-core-mvc"></a>Qu’est-ce qu’ASP.NET Core MVC ?

Le framework ASP.NET Core MVC est un framework de présentation léger, open source et hautement testable, optimisé pour ASP.NET Core.

ASP.NET Core MVC permet de créer des sites web dynamiques à l’aide de modèles en délimitant clairement les responsabilités. Il vous donne un contrôle total des balises, prend en charge le développement TDD et utilise les dernières normes du web.

## <a name="features"></a>Fonctionnalités

ASP.NET Core MVC inclut les éléments suivants :

* [Routage](#routing)
* [Liaison de modèles](#model-binding)
* [Validation du modèle](#model-validation)
* [Injection de dépendances](../fundamentals/dependency-injection.md)
* [Filtres](#filters)
* [Zones](#areas)
* [API web](#web-apis)
* [Testabilité](#testability)
* [Moteur de vue Razor](#razor-view-engine)
* [Vues fortement typées](#strongly-typed-views)
* [Tag Helpers](#tag-helpers)
* [Composants de vues](#view-components)

### <a name="routing"></a>Routage

ASP.NET Core MVC est basé sur le [routage d’ASP.NET Core](../fundamentals/routing.md), un puissant composant de mappage d’URL qui vous permet de générer des applications ayant des URL compréhensibles et pouvant faire l’objet de recherches. Cela vous permet de définir des modèles de nommage d’URL pour votre application, qui soient bien adaptés au SEO (optimisation du référencement d’un site auprès d’un moteur de recherche) et à la génération de liens, indépendamment de la façon dont les fichiers sont organisés sur votre serveur web. Vous pouvez définir vos routages à l’aide d’une syntaxe de modèle de routage pratique qui prend en charge les contraintes des valeurs de routage, les valeurs par défaut et les valeurs facultatives.

*Le routage basé sur les conventions* vous permet de définir globalement les formats d’URL acceptés par votre application, ainsi que le mappage de chacun de ces formats à une méthode d’action spécifique sur un contrôleur donné. Quand une requête entrante est reçue, le moteur de routage analyse l’URL et la fait correspondre à l’un des formats d’URL définis, puis il appelle la méthode d’action du contrôleur associé.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

Le *routage d’attributs* vous permet de spécifier des informations de routage en décorant vos contrôleurs et vos actions avec des attributs qui définissent les routages de votre application. Cela signifie que vos définitions de routage sont placées à côté du contrôleur et de l’action auxquels elles sont associées.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Liaison de données

La liaison de données [ASP.NET Core MVC](models/model-binding.md) convertit les données de requêtes clientes (valeurs de formulaire, données de routage, paramètres de chaîne de requête, en-têtes HTTP) en objets que le contrôleur peut prendre en charge. Ainsi, la logique du contrôleur n’a pas besoin d’identifier les données de requête entrantes ; elle utilise simplement les données en tant que paramètres de ses méthodes d’action.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>Validation de modèle

ASP.NET Core MVC prend en charge la [validation](models/validation.md) en décorant votre objet de modèle avec des attributs de validation d’annotation de données. Les attributs de validation sont vérifiés côté client avant que les valeurs ne soient postées sur le serveur, ainsi que sur le serveur avant l’appel de l’action du contrôleur.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Action du contrôleur :

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

Le framework gère la validation des données de requête à la fois sur le client et sur le serveur. La logique de validation spécifiée pour les types de modèle est ajoutée aux vues affichées en tant qu’annotations discrètes, et est appliquée dans le navigateur à l’aide de la [validation jQuery](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Injection de dépendances

ASP.NET Core offre une prise en charge intégrée de l’[injection de dépendances](../fundamentals/dependency-injection.md). Dans ASP.NET Core MVC, les [contrôleurs](controllers/dependency-injection.md) peuvent demander les services nécessaires via leurs constructeurs, ce qui leur permet de suivre le [principe des dépendances explicites](http://deviq.com/explicit-dependencies-principle/).

Votre application peut également utiliser l’[injection de dépendances dans les fichiers de vue](views/dependency-injection.md), à l’aide de la directive `@inject` :

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>Filtres

Les [filtres](controllers/filters.md) permettent aux développeurs d’intégrer des problèmes transversaux, par exemple la prise en charge des exceptions ou les autorisations. Les filtres permettent d’exécuter une logique de prétraitement et de posttraitement personnalisée pour les méthodes d’action. Vous pouvez les configurer pour qu’ils se lancent à certaines étapes du pipeline d’exécution d’une requête donnée. Vous pouvez appliquer les filtres aux contrôleurs ou aux actions en tant qu’attributs (ou vous pouvez les exécuter de manière globale). Plusieurs filtres (tels que `Authorize`) sont inclus dans le framework.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Zones

Les [zones](controllers/areas.md) permettent de partitionner une grande application web ASP.NET Core MVC en regroupements opérationnels plus petits. Une zone est une structure MVC à l’intérieur d’une application. Dans un projet MVC, les composants logiques, tels que Modèle, Contrôleur et Vue, sont conservés dans des dossiers distincts. MVC utilise des conventions de nommage pour créer la relation entre ces composants. Pour une grande application, il peut être avantageux de partitionner celle-ci en différentes zones de fonctionnalités générales. Prenons le cas d’une application d’e-commerce avec plusieurs divisions, par exemple le paiement, la facturation, la recherche, etc. Chacune de ces divisions a ses propres vues, contrôleurs et modèles de composants logiques.

### <a name="web-apis"></a>API web

En plus d’être une excellente plateforme pour la création de sites web, ASP.NET Core MVC prend en charge la génération d’API web. Vous pouvez créer des services accessibles à un large éventail de clients, notamment les navigateurs et les appareils mobiles.

Le framework inclut la prise en charge de la négociation de contenu HTTP, ainsi que la prise en charge intégrée des [données de mise en forme](models/formatting.md) au format JSON ou XML. Écrivez des [formateurs personnalisés](advanced/custom-formatters.md) pour ajouter la prise en charge de vos propres formats.

Utilisez la génération de liens pour activer la prise en charge hypermédia. Activez facilement la prise en charge de [CORS (Cross-Origin Resource Sharing)](http://www.w3.org/TR/cors/) pour que vos API web puissent être partagées entre plusieurs applications web.

### <a name="testability"></a>Testabilité

Le framework utilise les interfaces et l’injection de dépendances, ce qui le rend particulièrement adapté aux tests unitaires. De plus, le framework inclut des fonctionnalités (par exemple un fournisseur TestHost et InMemory pour Entity Framework) qui facilitent aussi les [tests d’intégration](../testing/integration-testing.md). Pour en savoir plus, consultez les informations sur le [test de la logique du contrôleur](controllers/testing.md).

### <a name="razor-view-engine"></a>Moteur de vue Razor

Les [vues ASP.NET Core MVC](views/overview.md) utilisent le [moteur de vue Razor](views/razor.md) pour afficher les vues. Razor est un langage de balisage de modèles compact, expressif et fluide qui permet de définir des vues avec du code C# incorporé. Razor est utilisé pour générer dynamiquement du contenu web sur le serveur. Vous pouvez mélanger sans problème du code serveur avec du contenu et du code côté client.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

À l’aide du moteur de vue Razor, vous pouvez définir des [dispositions](views/layout.md), des [vues partielles](views/partial.md) et des sections remplaçables.

### <a name="strongly-typed-views"></a>Vues fortement typées

Les vues Razor dans MVC peuvent être fortement typées en fonction de votre modèle. Les contrôleurs peuvent passer un modèle fortement typé à des vues, ce qui leur permet de disposer du contrôle de type et de la prise en charge d’IntelliSense.

Par exemple, la vue suivante affiche un modèle de type `IEnumerable<Product>` :

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Tag Helpers

Les [Tag Helpers](views/tag-helpers/intro.md) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor. Vous pouvez utiliser des Tag Helpers pour définir des balises personnalisées (par exemple `<environment>`), ou pour modifier le comportement de balises existantes (par exemple `<label>`). Les Tag Helpers se lient à des éléments spécifiques en fonction du nom de l’élément et de ses attributs. Ils offrent les avantages du rendu côté serveur tout en conservant une expérience utilisateur d’édition HTML.

Il existe de nombreux Tag Helpers pour les tâches courantes (par exemple la création de formulaires ou de liens, le chargement de ressources, etc.) et bien d’autres encore, dans les dépôts publics GitHub et sous forme de packages NuGet. Les Tag Helpers sont créés en C# et ciblent les éléments HTML en fonction du nom de l’élément, du nom de l’attribut ou de la balise parente. Par exemple, le Tag Helper Link intégré permet de créer un lien vers l’action `Login` de `AccountsController` :

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

Le `EnvironmentTagHelper` permet d’inclure différents scripts dans vos vues (par exemple bruts ou minimisés) en fonction de l’environnement d’exécution, Développement, Préproduction ou Production :

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Les Tag Helpers fournissent une expérience utilisateur de développement HTML et un riche environnement IntelliSense pour la création de balises HTML et Razor. La plupart des Tag Helpers intégrés ciblent les éléments HTML existants et fournissent des attributs côté serveur pour l’élément.

### <a name="view-components"></a>Composants de vues

Les [composants de vues](views/view-components.md) vous permettent de compresser la logique de rendu et de la réutiliser dans l’application. Ils sont similaires aux [vues partielles](views/partial.md), mais avec une logique associée.
